源码地址： [https://gitee.com/sohucw/webwork-file-upload](https://gitee.com/sohucw/webwork-file-upload)

当文件体积过大时，传统的文件上传方式往往会导致页面卡顿，用户体验不佳。为了解决这一问题，我们可以利用`Web Worker`技术来进行大文件的切片上传。本文将详细介绍如何使用`Web Worker`进行大文件切片上传，并通过具体的例子来演示其实现过程。

## Web Worker简介
Web Worker是Web浏览器提供的一种在后台线程中运行JavaScript的功能。它独立于主线程运行，可以执行计算密集型或长时间运行的任务，而不会阻塞页面的渲染和交互。通过将大文件切片上传的逻辑放在Web Worker中执行，我们可以充分利用浏览器的多线程能力，提高上传速度，并保持页面的流畅运行。

### Web Worker基于Vue的基础用法
在Vue项目中配置webpack来使用web-worker涉及几个关键步骤。这主要涉及到处理worker文件的加载，确保它们被正确地打包和引用。以下是一个基本的配置过程：

#### 1.安装worker-loader
首先，你需要安装`worker-loader`，这是一个webpack的loader，用于处理worker文件。

```bash
npm install --save-dev worker-loader
```

#### 2.配置webpack
```javascript
module.exports = {
  publicPath: './',

  chainWebpack: config => {  
    config.module  
      .rule('worker')  
      .test(/\.worker\.js$/)  // 如果需要.worker.js后缀
      .use('worker-loader')  
      .loader('worker-loader')
      .options({ // 可以查阅worker-loader文档，根据自己的需求进行配置
      })
  }  
}
```

#### 3.创建和使用worker
创建一个worker文件，并给它一个`.worker.js`的扩展名。例如，你可以创建一个`my-worker.worker.js`文件。

```javascript
// my-worker.worker.js  
self.onmessage = function(e) {  
  console.log('Worker: Hello World');  
  const result = doSomeWork(e.data);  
  self.postMessage(result);  
};  

function doSomeWork(data) {  
  // 模拟一些工作  
  return data * 2;  
}
```

在你的Vue组件或其他JavaScript文件中，你可以像下面这样创建一个worker实例：

```javascript
// MyComponent.vue 或其他.js文件  
import MyWorker from './my-worker.worker.js';  

export default {  
  methods: {  
    startWorker() {  
      const myWorker = new MyWorker();  

      myWorker.onmessage = (e) => {  
        console.log('Main script: Received result', e.data);  
      };  

      myWorker.postMessage(100); // 发送数据给worker  
    }  
  },  
  mounted() {  
    this.startWorker();  
  }  
};
```

现在，当组件被挂载时，它将启动worker，发送一个消息，并在收到worker的响应时打印结果。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723449021378-e74423ef-454f-4459-be46-641ef3c14664.png)

接下来我们进行实战，利用web-worker的机制进行大文件切片上传

## 实战：实现大文件切片上传
### 1.逻辑梳理
1. **文件切片**：使用 JavaScript 的 `Blob.prototype.slice()` 方法将大文件切分成多个切片。
2. **上传切片**：使用 `axios` 或其他 HTTP 客户端库逐个上传切片。可以为每个切片生成一个唯一的标识符（例如，使用文件的哈希值和切片索引），以便后端能够正确地将它们合并。
3. **客户端线程数**：获取用户CPU线程数量，以便最大优化上传文件速度。
4. **控制上传接口的并发数量**：防止大量的请求并发导致页面卡死，设计一个线程队列，控制请求数量一直保持在6。

### 2.实现
我会在文章后面放demo的GitHub源码。

#### 1.获取客户端线程数量
`navigator.hardwareConcurrency` 是一个只读属性，它返回用户设备的逻辑处理器内核数。

```javascript
export const getConcurrency = () => navigator.hardwareConcurrency || 4 // 浏览器不支持就默认4核
```

#### 2.主线程
定义和处理一些必要的常量，并且根据用户的线程数进行开启多线程Web-worker任务处理文件切片。

```javascript
import { defer, createEventHandler } from 'js-hodgepodge'
import FileWorker from './files.worker'

export const getConcurrency = () => navigator.hardwareConcurrency || 4

export const handleEvent = () => createEventHandler('handleSchedule')

export const sliceFile = file => {

  const dfd = defer()

  const chunkSize = 1024 // 1Kb
  const thread = getConcurrency() // 线程数

  const chunks = []
  const chunkNum = Math.ceil(file.size / chunkSize) // 切片总数量

  const workerChunkCount = Math.ceil(chunkNum / thread) // 每个线程需要处理的切片数量
  let finishCount = 0;

  for (let i = 0; i < thread; i++) {

    const worker = new FileWorker()

    // 计算每个线程的开始索引和结束索引
    const startIndex = i * workerChunkCount;

    let endIndex = startIndex + workerChunkCount;

    // 防止最后一个线程结束索引大于文件的切片数量的总数量
    if (endIndex > chunkNum) {
      endIndex = chunkNum;
    }

    worker.postMessage({
      file,
      chunkSize,
      startIndex,
      endIndex,
    });

    worker.onmessage = (e) => {

      // 接收到 worker 线程返回的消息
      for (let i = startIndex; i < endIndex; i++) {

        chunks[i] = {
          ...e.data[i - startIndex],
          chunkNum,
          filename: file.name
        };

      }

      worker.terminate(); // 关闭线程

      finishCount++;

      if (finishCount === thread) {

        dfd.resolve({
          chunks,
          chunkNum
        });
      }
    };

  }

  return dfd
}
```

#### 3.实现文件切片
首先，我们需要创建一个 Web Worker 脚本，用于处理文件切片和切片hash

```javascript
import md5 from 'js-md5'

self.onmessage = async function ({
  data: {
    file,
    chunkSize,
    startIndex,
    endIndex,
  }
}) {

  const arr = [];

  for (let i = startIndex; i < endIndex; i++) {
    arr.push(
      createChunks(file, i, chunkSize)
    );
  }
  const chunks = await Promise.all(arr)

  // 提交线程信息
  postMessage(chunks);
}

const createChunks = (
  file,
  index,
  chunkSize
) => {
  return new Promise((resolve) => {

    // 开始第几个*分片的大小
    const start = index * chunkSize;

    // 结束时start + 分片的大小
    const end = start + chunkSize;
    const fileReader = new FileReader();

    // 每个切片都通过FileReader读取为ArrayBuffer
    fileReader.onload = (e) => {

      const content = new Uint8Array(e.target.result);
      const files = file.slice(start, end);

      const md5s = md5.arrayBuffer(content)

      function arrayBufferToHex(buffer) {
        let bytes = new Uint8Array(buffer);
        let hexString = '';
        for (let i = 0; i < bytes.byteLength; i++) {
          let hex = bytes[i].toString(16);

          hexString += hex.length === 1 ? '0' + hex : hex;
        }
        return hexString;
      }

      resolve({
        start,
        end,
        index,
        hash: arrayBufferToHex(md5s),  // 生成唯一的hash
        files,
      });
    };

    // 读取文件的分片
    fileReader.readAsArrayBuffer(file.slice(start, end));
  });
}
```

Web Worker通过`onmessage`事件接收消息。当主线程发送消息时，这个消息会作为参数传递给`onmessage`函数。

切片`hash`处理流程：使用`FileReader`来读取文件内容。当文件分片读取完毕后，会触发`onload`这个事件,使用`new Uint8Array(e.target.result)`将读取的ArrayBuffer转换为Uint8Array，再利用`js-md5`的使用`md5.arrayBuffer(content)`计算分片的MD5哈希值，使用`arrayBufferToHex`函数将切片buffer转换为十六进制String，当所有分片处理完毕后，将结果（即分片及其相关信息）发送`postMessage`回主线程。

#### 4.请求池的设计与处理
我这里创建一个请求队列，并使用 Promise 来控制并发请求的数量。创建一个数组来存储待处理的请求，并使用 Promise 来控制每次只有一定数量的请求被发送。当某个请求完成时，再从队列中取出下一个请求来发送。

```javascript
export const uploadFile = (
  chunks // 总切片
) => {
  chunks = chunks || []

  let schedule = 0 // 进度

  const { dispatch } = handleEvent()

  const requestQueue = (concurrency) => {
    concurrency = concurrency || 6
    const queue = [] // 线程池
    let current = 0

    const dequeue = () => {
      while (current < concurrency && queue.length) {
        current++;
        const requestPromiseFactory = queue.shift();
        requestPromiseFactory()
          .then(result => { // 上传成功处理
            console.log(result)

            schedule++; // 收集上传切片成功的数量

            dispatch(window, schedule);  // 事件派发，通知进度
          })
          .catch(error => { // 失败
            console.log(error)
          })
          .finally(() => {
            current--;
            dequeue();
          });
      }

    }

    return (requestPromiseFactory) => {
      queue.push(requestPromiseFactory)
      dequeue()
    }

  }

  const handleFormData = obj => {
    const formData = new FormData()

    Object
      .entries(obj)
      .forEach(([key, val]) => {
        formData.append(key, val)
      })

    return formData
  }

  const enqueue = requestQueue(6)

  for (let i = 0; i < chunks.length; i++) {

    enqueue(() => axios.post(
      '/api/upload',
      handleFormData(chunks[i]),
      {
        headers: {
          'Content-Type': 'multipart/form-data' 
        }
      }
    ))
  }

  return schedule

}
```

利用了第三方库`js-hodgepodge`的发布订阅，将上传切片成功的数量发布给主界面，得到相应的上传进度。

可以参考这个内容：

<br/>color1
在创建CustomEvent对象时，通常需要指定事件的类型（type）以及一个可选的事件初始化字典（eventInitDict），后者用于设置事件的详细属性。一旦创建了CustomEvent对象，就可以使用dispatchEvent方法将其触发，进而在事件监听器中进行处理。

<br/>

### 基本用法
下面是一个简单的示例，展示了如何使用window.CustomEvent来创建一个自定义事件并触发它：

```javascript
// 创建一个自定义事件  
const myEvent = new CustomEvent('myCustomEvent', {  
  detail: {  
    message: 'Hello, World！'  
  },  
  bubbles: true,  
  cancelable: true  
});  

// 触发自定义事件  
window.dispatchEvent(myEvent);  

// 监听自定义事件  
window.addEventListener('myCustomEvent', function(event) {  
  console.log(event.detail.message); // 输出: Hello, World！
});
```

**CustomEvent接收两个参数：**

1. `type`：一个字符串，表示事件的名称。
2. `eventInitDict`：一个配置事件的选项对象，是可选的。这个对象可以包含以下属性：
    - `bubbles`：一个布尔值，表示事件是否冒泡。默认为`false`。
    - `cancelable`：一个布尔值，表示事件是否可以被取消。默认为`false`。
    - `detail`：包含传递给事件监听器的任何自定义数据的对象。

### 接下来我们实现属于自己的发布订阅工具函数吧
我这里是基于TS封装的一个发布订阅函数，接下来我们来解析它：

```javascript
function createEventHandler<DataType>(name: string) {

  const addEventListener = (
    Win: Window,
    fn: (e: { detail: DataType }) => void
  ) => {
    // @ts-ignore
    Win.addEventListener(name, fn)

    // @ts-ignore
    const eject = () => Win.removeEventListener(name, fn)

    return eject
  }

  const dispatch = (
    Win: Window,
    data: DataType
  ) => {
    Win.dispatchEvent(
      new CustomEvent(name, { detail: data })
    )
  }

  return {
    addEventListener,
    dispatch
  }

}
```

我们定义了一个名叫createEventHandler的函数并暴露**发布（dispatch）** 和**订阅（addEventListener）** 这两个方法:

1. **addEventListener**： 接收两个参数，一个是Window对象，一个是事件订阅的回调。
2. **dispatch**： 接收两个参数，一个是Window对象，一个是发布事件所需的内容。

用法：

```javascript
const { addEventListener, dispatch } = createEventHandler('handleMessage')

const eject = addEventListener(window, ({ detail }) => {
  console.log(detail); // { message: 1111 }

})

setTimeout(() => {
  dispatch(window, { message: 1111 })

  // 当不再需要事件监听器时，可以调用 eject 函数来移除它  
  eject();

})
```

在这个示例中，我们创建了一个名为 `'handleMessage'` 的自定义事件，其 `detail` 属性包含对象 `{ message: 1111 }`。我们添加了一个事件监听器来打印这个对象，并在之后触发了这个事件。最后，我们使用返回的 `eject` 函数移除了事件监听器。

CustomEvent在Web开发中非常有用，尤其是在需要实现组件间通信或处理特定业务逻辑时。通过自定义事件，开发者可以更精确地控制事件的触发和响应，从而实现更复杂的交互和功能。

需要注意的是，由于CustomEvent是DOM API的一部分，因此它主要在浏览器环境 **（大部分浏览器都支持）** 中使用。在非浏览器环境 **（如Node.js）** 中，可能无法使用CustomEvent或需要使用类似功能的库或工具。

 

#### 7.主界面代码
```vue
<template>
  <div>
    <input type="file" ref="file">

      <button @click="handleUpload">提交</button>

      <p>进度：{{ progress * 100 }}%</p>
    </div>
</template>

<script>
  import { sliceFile, uploadFile, handleEvent } from './file.utils'
  export default {

    data() {
      return {
        progress: 0
      }
    },

    methods: {
      async handleUpload() {
        const file = this.$refs.file.files[0]

        if(!file) {
          return
        }

        console.time()

        const dfd = sliceFile(file)

        dfd
          .promise
          .then(({ chunks, chunkNum }) => {
            uploadFile(chunks)

            const { addEventListener } = handleEvent()

            const eject = addEventListener(window, ({ detail: schedule }) => {

              this.progress = schedule / chunkNum

              if(schedule === chunkNum) { // 上传完成，关闭事件监听
                eject()
              }
            })
          })

        console.timeEnd() 
      }
    }
  }
</script>

<style>

</style>
```

#### 6.执行响应结果打印
当执行一个大文件上传时，时间可被大大的压缩了。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723449129307-d154d9de-ee41-4d39-b8f9-14ef8691057b.png)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723449136388-c28a1359-e398-403e-b41c-07439c36aa9e.png)

node后端切片与组合结果

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723449144434-7a4eccda-46e3-4e94-ba03-af408d46134a.png)

其实整个流程比较重要的就是文件切片，和请求池的设计，具体项目细节请查看源码 

