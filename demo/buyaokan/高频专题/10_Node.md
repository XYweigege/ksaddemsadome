### <font style="color:rgb(44, 62, 80);">浏览器和nodejs事件循环（Event Loop）有什么区别</font>
**<font style="color:rgb(44, 62, 80);">单线程和异步</font>**

+ <font style="color:rgb(44, 62, 80);">JS是单线程的，无论在浏览器还是在nodejs</font>
+ <font style="color:rgb(44, 62, 80);">浏览器中JS执行和DOM渲染共用一个线程，是互斥的</font>
+ <font style="color:rgb(44, 62, 80);">异步是单线程的解决方案</font>

**<font style="color:rgb(44, 62, 80);">1. 浏览器中的事件循环</font>**

**<font style="color:rgb(44, 62, 80);">异步里面分宏任务和微任务</font>**

+ <font style="color:rgb(44, 62, 80);">宏任务：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">I/O</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI</font><font style="color:rgb(44, 62, 80);">渲染，网络请求</font>
+ <font style="color:rgb(44, 62, 80);">微任务：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MutationObserver</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">async/await</font>
+ <font style="color:rgb(44, 62, 80);">宏任务和微任务的区别：微任务的优先级高于宏任务，微任务会在当前宏任务执行完毕后立即执行，而宏任务会在下一个事件循环中执行</font>
    - <font style="color:rgb(44, 62, 80);">宏任务在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">页面渲染之后</font><font style="color:rgb(44, 62, 80);">执行</font>
    - <font style="color:rgb(44, 62, 80);">微任务在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">页面渲染之前</font><font style="color:rgb(44, 62, 80);">执行</font>
    - <font style="color:rgb(44, 62, 80);">也就是微任务在下一轮</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">渲染之前执行，宏任务在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">渲染之后执行</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786307991-b7b3dc32-fb21-4f43-9236-f07a8aae4a30.png)

```javascript
console.log('start')
setTimeout(() => { 
  console.log('timeout')
})
Promise.resolve().then(() => {
  console.log('promise then')
})
console.log('end')

// 输出
// start 
// end 
// promise then
// timeout
```

```javascript
// 分析

// 等同步代码执行完后，先从微任务队列中获取（微任务队列优先级高），队列先进先出

// 宏任务 MarcoTask 队列
// 如setTimeout 1000ms到1000ms后才会放到队列中
const MarcoTaskQueue = [
  () => {
    console.log('timeout')
  },
  fn // ajax回调放到宏任务队列中等待
]  

ajax(url, fn) // ajax 宏任务 如执行需要300ms


// ********** 宏任务和微任务中间隔着 【DOM 渲染】 ****************

// 微任务 MicroTask 队列
const MicroTaskQueue = [
  () => {
    console.log('promise then')
  }
]

// 等宏任务和微任务执行完后 Event Loop 继续监听（一旦有任务到了宏任务微任务队列就会立马拿过来执行）...
```

```html
<p>Event Loop</p>

<script>
  const p = document.createElement('p')
  p.innerHTML = 'new paragraph'
  document.body.appendChild(p)
  const list = document.getElementsByTagName('p')
  console.log('length----', list.length) // 2

  console.log('start')
  // 宏任务在页面渲染之后执行
  setTimeout(() => {
    const list = document.getElementsByTagName('p')
    console.log('length on timeout----', list.length) // 2
    alert('阻塞 timeout') // 阻塞JS执行和渲染
  })
  // 微任务在页面渲染之前执行
  Promise.resolve().then(() => {
    const list = document.getElementsByTagName('p')
    console.log('length on promise.then----', list.length) // 2
    alert('阻塞 promise') // 阻塞JS执行和渲染
  })
  console.log('end')
</script>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786310880-b347a531-f2a5-469c-84c5-9c030ae81ff5.png)

**<font style="color:rgb(44, 62, 80);">2. nodejs中的事件循环</font>**

+ <font style="color:rgb(44, 62, 80);">nodejs也是单线程，也需要异步</font>
+ <font style="color:rgb(44, 62, 80);">异步任务也分为：宏任务 + 微任务</font>
+ <font style="color:rgb(44, 62, 80);">但是，它的宏任务和微任务分为不同的类型，有不同的优先级</font>
+ <font style="color:rgb(44, 62, 80);">和浏览器的主要区别就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">类型</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">优先级</font><font style="color:rgb(44, 62, 80);">，理解了这里就理解了nodejs的事件循环</font>

**<font style="color:rgb(44, 62, 80);">宏任务类型和优先级</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">类型分为6个，优先级从高到底执行</font>

+ **<font style="color:rgb(44, 62, 80);">Timer</font>**<font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font>
+ **<font style="color:rgb(44, 62, 80);">I/O callbacks</font>**<font style="color:rgb(44, 62, 80);">：处理网络、流、TCP的错误回调</font>
+ **<font style="color:rgb(44, 62, 80);">Idle,prepare</font>**<font style="color:rgb(44, 62, 80);">：闲置状态（nodejs内部使用）</font>
+ **<font style="color:rgb(44, 62, 80);">Poll轮询</font>**<font style="color:rgb(44, 62, 80);">：执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">poll</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">I/O</font><font style="color:rgb(44, 62, 80);">队列</font>
+ **<font style="color:rgb(44, 62, 80);">Check检查</font>**<font style="color:rgb(44, 62, 80);">：存储</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font><font style="color:rgb(44, 62, 80);">回调</font>
+ **<font style="color:rgb(44, 62, 80);">Close callbacks</font>**<font style="color:rgb(44, 62, 80);">：关闭回调，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">socket.on('close')</font>

**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">注意</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">优先级最高，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">比</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">优先级高</font>

**<font style="color:rgb(44, 62, 80);">执行过程</font>**

+ <font style="color:rgb(44, 62, 80);">执行同步代码</font>
+ <font style="color:rgb(44, 62, 80);">执行微任务（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(44, 62, 80);">优先级最高）</font>
+ <font style="color:rgb(44, 62, 80);">按顺序执行6个类型的宏任务（每个开始之前都执行当前的微任务）</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786308835-632a5691-04b1-492c-bd6d-d9c731ae4278.png)

**<font style="color:rgb(44, 62, 80);">总结</font>**

+ <font style="color:rgb(44, 62, 80);">浏览器和nodejs的事件循环流程基本相同</font>
+ <font style="color:rgb(44, 62, 80);">nodejs宏任务和微任务分类型，有优先级。浏览器里面的宏任务和微任务是没有类型和优先级的</font>
+ <font style="color:rgb(44, 62, 80);">node17之后推荐使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font><font style="color:rgb(44, 62, 80);">代替</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(44, 62, 80);">（如果使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(44, 62, 80);">执行复杂任务导致后面的卡顿就得不偿失了，尽量使用低优先级的api去执行异步）</font>

```javascript
console.info('start')
setImmediate(() => {
  console.info('setImmediate')
})
setTimeout(() => {
  console.info('timeout')
})
Promise.resolve().then(() => {
  console.info('promise then')
})
process.nextTick(() => {
  console.info('nextTick')
})
console.info('end')

// 输出
// start
// end
// nextTick
// promise then
// timeout
// setImmediate
```

### [](https://www.123fe.net/docs/base/high-frequency.html#nodejs%E5%A6%82%E4%BD%95%E5%BC%80%E5%90%AF%E5%A4%9A%E8%BF%9B%E7%A8%8B-%E8%BF%9B%E7%A8%8B%E5%A6%82%E4%BD%95%E9%80%9A%E8%AE%AF)<font style="color:rgb(44, 62, 80);">nodejs如何开启多进程，进程如何通讯</font>
**<font style="color:rgb(44, 62, 80);">进程process和线程thread的区别</font>**

+ <font style="color:rgb(44, 62, 80);">进程，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">OS</font><font style="color:rgb(44, 62, 80);">进行资源分配和调度的最小单位，有独立的内存空间</font>
+ <font style="color:rgb(44, 62, 80);">线程，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">OS</font><font style="color:rgb(44, 62, 80);">进程运算调度的最小单位，共享进程内存空间</font>
+ <font style="color:rgb(44, 62, 80);">JS是单线程的，但可以开启多进程执行，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WebWorker</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786310099-60394b9b-5ee4-4724-911b-ecb4585e176f.png)

**<font style="color:rgb(44, 62, 80);">为何需要多进程</font>**

+ <font style="color:rgb(44, 62, 80);">多核CPU，更适合处理多进程</font>
+ <font style="color:rgb(44, 62, 80);">内存较大，多个进程才能更好利用（单进程有内存上限）</font>
+ <font style="color:rgb(44, 62, 80);">总之，压榨机器资源，更快、更节省</font>

**<font style="color:rgb(44, 62, 80);">如何开启多进程</font>**

+ <font style="color:rgb(44, 62, 80);">开启子进程</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">child_process.fork</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cluster.fork</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">child_process.fork</font><font style="color:rgb(44, 62, 80);">用于单个计算量较大的计算</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cluster</font><font style="color:rgb(44, 62, 80);">用于开启多个进程，多个服务</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">send</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">on</font><font style="color:rgb(44, 62, 80);">传递消息</font>

**<font style="color:rgb(44, 62, 80);">使用child_process.fork方式</font>**

```javascript
const http = require('http')
const fork = require('child_process').fork

const server = http.createServer((req, res) => {
  if (req.url === '/get-sum') {
    console.info('主进程 id', process.pid)

    // 开启子进程 计算结果返回
    const computeProcess = fork('./compute.js')
    computeProcess.send('开始计算') // 发送消息给子进程开始计算，在子进程中接收消息调用计算逻辑，计算完成后发送消息给主进程

    computeProcess.on('message', data => {
      console.info('主进程接收到的信息：', data)
      res.end('sum is ' + data)
    })

    computeProcess.on('close', () => {
      console.info('子进程因报错而退出')
      computeProcess.kill() // 关闭子进程
      res.end('error')
    })
  }
})
server.listen(3000, () => {
  console.info('localhost: 3000')
})
```

```javascript
// compute.js

/**
 * @description 子进程，计算
 */

function getSum() {
  let sum = 0
  for (let i = 0; i < 10000; i++) {
    sum += i
  }
  return sum
}

process.on('message', data => {
  console.log('子进程 id', process.pid)
  console.log('子进程接收到的信息: ', data)

  const sum = getSum()

  // 发送消息给主进程
  process.send(sum)
})
```

**<font style="color:rgb(44, 62, 80);">使用cluster方式</font>**

```javascript
const http = require('http')
const cpuCoreLength = require('os').cpus().length
const cluster = require('cluster')

// 主进程
if (cluster.isMaster) {
  for (let i = 0; i < cpuCoreLength; i++) {
    cluster.fork() // 根据核数 开启子进程
  }

  cluster.on('exit', worker => {
    console.log('子进程退出')
    cluster.fork() // 进程守护
  })
} else {
  // 多个子进程会共享一个 TCP 连接，提供一份网络服务
  const server = http.createServer((req, res) => {
    res.writeHead(200)
    res.end('done')
  })
  server.listen(3000)
}


// 工作中 使用PM2开启进程守护更方便
```

