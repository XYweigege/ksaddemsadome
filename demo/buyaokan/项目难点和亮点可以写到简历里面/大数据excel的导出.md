### 源码地址&视频讲解
[代码地址：](https://gitee.com/sohucw/big-data-excel)

[视频讲解：](https://www.douyin.com/user/self?modal_id=7119418147039448355)[https://www.douyin.com/user/self?modal_id=7119418147039448355](https://www.douyin.com/user/self?modal_id=7119418147039448355)

### 场景
<br/>color1
在前端导出几万条或者百万条Excel文件的业务场景中，通常涉及到需要将大量数据以Excel格式提供给用户或其他系统 场景如下；

<br/>

**数据分析和报告： **当用户需要对大量数据进行分析或生成报告时，将数据导出为Excel文件是一种常见的做法。这可以帮助用户在本地使用他们熟悉的工具进行进一步的数据处理和分析。

**批量数据导出： **在某些业务中，用户可能需要批量导出数据以备份、迁移或与其他系统集成。将数据导出为Excel文件提供了一种通用的、易于使用的格式。

**数据交换： **在不同的系统之间共享数据时，Excel文件可能是一个普遍接受的格式。通过导出Excel文件，可以方便地在不同系统之间传递数据。

**报表生成：** 在企业环境中，导出大量数据到Excel文件可以用于生成各种报表，用于监测业务绩效、分析趋势等。



![](https://cdn.nlark.com/yuque/0/2024/gif/207857/1729733439373-e26df69a-ac9e-491d-a1c8-ae3dd20a3bec.gif)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1729733513967-c110b7c9-c5f7-44db-afa0-a0d174d969d5.png)

### 分析过程
**传统玩法**

<font style="color:rgb(51, 51, 51);"></font>

<font style="color:rgb(51, 51, 51);">导出excel，常规玩法：让后端处理好，前端直接调后端接口，</font>

```javascript
xxxApi(params).then(res=>{
	if(res){
          const blob = new Blob([res], { type: 'application/vnd.ms-excel' })
          const a = document.createElement('a')
          a.download = '表格.xlsx'
          a.href = window.URL.createObjectURL(blob)
          a.click()
          console.log('导出成功')
	}else{
		console.log('导出失败')
	}
})

xxxApi(params).then(res=>{
	if(res && res.url){
           window.open(res.url, '_blank');
           console.log('导出成功')
	}
  elMesage.alert(res.message);
})



```

> <font style="color:rgb(51, 51, 51);">excel表格文件都是后端生成的</font>
>

**前端生成**

**pnpm install xlsx**

```javascript
import * as XLSX from 'xlsx' 
export function exportExcel(filename,data) {
    let exc = XLSX.utils.book_new();
    let exc_data = XLSX.utils.aoa_to_sheet(data); 
    XLSX.utils.book_append_sheet(exc, exc_data, filename);
    XLSX.writeFile(exc, filename + 'xlsx');
}

```

```vue
<template>
  <button @click="download">下载表格</button>
</template>
<script  setup>
  import { exportExcel } from "./excelUtils"
  const exc_data = [
    ['第一列', '第二列' ,'第三列'],
    ['aa', 'bb' ,'cc'],
    ['dd', 'ee' ,'ff']
  ];
  function download() {  exportExcel('vue3导出的表格',this.exc_data)  }
</script>

```

```jsx
import React from "react";
import {exportExcel } from './excelConfig'
const exc_data = [
  ['第一列', '第二列' ,'第三列'],
  ['aa', 'bb' ,'cc'],
  ['dd', 'ee' ,'ff']
];
function Index() {
  return (
    <div className="box">
      <button onClick={()=>{ exportExcel('react导出表格',exc_data) }}>下载</button>
    </div>
  );
}
export default Index;

```

**效果**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1729735378366-c0663c01-8d75-4660-b70b-df2a0f071090.png)

> **<font style="color:rgb(51, 51, 51);">这是后端加工成表格，我们其实只是下载</font>**
>

<br/>color1
**<font style="color:#DF2A3F;">这两种方法，数据量小可以，但我们是10万条数据，这种方法太卡了</font>**

<br/>



**业务常识**

<br/>color1
<font style="color:rgb(51, 51, 51);">10万数据8秒多，20万32秒！这还没对数据加工，实际数据是需要处理的。比如修改日期格式、给收益率这种加上百分号</font>

<br/>



**webWorker里面处理**

<br/>color1
+ <font style="color:rgb(0, 127, 255);">WebWorker 允许在主线程之外再创建一个 worker 线程，</font>**在主线程执行任务的同时，worker 线程也可以在后台执行它自己的任务，互不干扰。**
+ <font style="color:rgb(0, 127, 255);">主线程：调用new Worker()构造函数，新建一个 worker 线程，构造函数的参数是一个 url，生成这个 url 的方法有两种：1脚本文件（会有两个限制）；2字符串形式（需要</font>**new Blob([data])**<font style="color:rgb(0, 127, 255);"> 将数据转成</font>**二进制**<font style="color:rgb(0, 127, 255);">，）。</font>
+ <font style="color:rgb(0, 127, 255);">子线程：</font>**self.onmessage监听**<font style="color:rgb(0, 127, 255);">主线程传过来的信息，</font>**self.postMessage发送**<font style="color:rgb(0, 127, 255);">信息给主线程，self.close()worker 线程关闭自身。</font>**Worker 线程**<font style="color:rgb(0, 127, 255);">能够访问一个</font>**全局函数 imprtScripts()**<font style="color:rgb(0, 127, 255);"> 来引入脚本，该函数接受 0 个或者多个 URL作为参数。</font>
+ <font style="color:rgb(0, 127, 255);">因为 worker 创造了另外一个线程，不在主线程上，浏览器给设定了一些</font>**限制**<font style="color:rgb(0, 127, 255);">（无法使用：window 对象、document 对象、</font>**DOM 对象**<font style="color:rgb(0, 127, 255);">、parent 对象；可以使用：浏览器：navigator 对象、URL：location 对象 只读、发送请求：XMLHttpRequest 对象、定时器：setTimeout/setInterva、应用缓存：Application Cache）</font>
+ <font style="color:rgb(0, 127, 255);">因为主线程与 worker 线程之间的通信是</font>**拷贝关系**<font style="color:rgb(0, 127, 255);">，当需要传递一个</font>**巨大**<font style="color:rgb(0, 127, 255);">的二进制文件给 worker 线程处理时(worker 线程就是用来干这个的)，这时候使用拷贝的方式来传递数据，无疑会造成</font>**性能**<font style="color:rgb(0, 127, 255);">问题。</font>

 

<br/>

****

**主线程**

** **主线程加工成表格，等于没做优化

****

**子线程**

子线程中加工成表格，<font style="color:rgb(51, 51, 51);">会出现新的问题，他们的源码里面用到了dom</font>

<font style="color:rgb(51, 51, 51);"></font>

**<font style="color:rgb(51, 51, 51);">面试当中聊的方案</font>**

<font style="color:rgb(51, 51, 51);">子线程处理数据，处理后传递给主线程，</font>

<font style="color:#DF2A3F;">子线程传递主线程的开销（序列化json开销大）， 大于你直接放在主线程处理的开销，负优化了</font>

<br/>color1
主线程与 worker 线程之间的通信是拷贝关系，当需要传递一个巨大的二进制文件给 worker 线程处理时(worker 线程就是用来干这个的)，这时候使用拷贝的方式来传递数据，无疑会造成性能问题。

<br/>

****

**怎么办？**

<br/>color1
**Web Worker 提供了一种转移数据的方式，允许主线程把二进制数据直接转移给子线程**<font style="color:rgb(0, 127, 255);">。这种方式比原先拷贝的方式，有巨大的性能提升。只是要</font>**注意**<font style="color:rgb(0, 127, 255);">，</font>_一旦数据转移到其他线程，原先线程就无法再使用这些二进制数据了_<font style="color:rgb(0, 127, 255);">，这是为了防止出现多个线程同时修改数据的麻烦局面</font>

<br/>

```jsx
// 创建二进制数据
var uInt8Array = new Uint8Array(1024*1024*32); // 32MB
for (var i = 0; i < uInt8Array .length; ++i) {
    uInt8Array[i] = i;
}
console.log(uInt8Array.length); // 传递前长度:33554432
// 字符串形式创建worker线程
var myTask = `
    onmessage = function (e) {
        var data = e.data;
        console.log('worker:', data);
    };
`;

var blob = new Blob([myTask]);
var myWorker = new Worker(window.URL.createObjectURL(blob));

// 使用这个格式(a,[a]) 来转移二进制数据
myWorker.postMessage(uInt8Array.buffer, [uInt8Array.buffer]); // 发送数据、转移数据

console.log(uInt8Array.length); // 传递后长度:0，原先线程内没有这个数据了

```

<br/>color1
当要导出的数据量极大的时候，主线程与 Worker 线程之间还使用对象形式进行数据通信就会造成性能问题，所幸 WebWorker 在线程之间的通信支持以二进制形式数据进行通信。

项目当中使用的为new Blob([string])来对导出的数据进行二进制形式转换，

恰巧 **file-saver** 也可以使用 **Blob** 二进制数据形式，进行对文件的导出

<br/>

```jsx
FileSaver.saveAs(new Blob([outputString], {
  type: 'application/octet-stream'
}), `${fileName}.xlsx`)
 
```

