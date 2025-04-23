在写上传文件这个功能的时候，需要得到文件的MD5值。

小文件（100M以内）的MD5值可以1秒内算出，但是大文件会比较耗时间，比如计算一个500M的文件需要10S左右。



我先选择文件1（500M）上传，正在计算MD5值，我又选择一个文件2上传，会打断文件1的MD5计算，这样显然是不行的。

所以，需要开一个子线程来计算MD5值，且不影响主线程。

MD5的计算：基于spark切片。

 

<font style="color:rgb(77, 77, 77);">HTML代码：</font>

```html
<!DOCTYPE html>
<html lang="en">

  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script src="../static/js/spark-md5.min.js"></script>
    <script src="../static/js/jquery.min.js"></script>
  </head>

  <body>
    <input type="file" id="file" />
    <div id="box"></div>
    <div id="md5"></div>
  </body>

  <script>
    let num = 0;  //主线程要用到的变量

    //子线程向主线程传递消息
    let worker = new Worker('1.js');

    worker.onmessage = ev => {  //接收子线程发回来的消息,事件对象的data属性可以获取 Worker 发来的数据
      $('#md5').text(JSON.parse(ev.data).md5);
    }

    document.querySelector('#file').addEventListener('change', e => {
      var file = e.target.files[0];
      worker.postMessage(file);//向子线程发送message事件
    });

    setInterval(function() {
      num++;
      $("#box").text(num);
    }, 1000);

  </script>

</html>

```

<font style="color:rgb(77, 77, 77);">worker.js</font>

```javascript
// 子线程要用的变量
var obj = {
  md5: 999
}

addEventListener("message", function (event) {
  importScripts('../static/js/spark-md5.min.js'); //导入

  let file_obj = event.data, //传过来的数据是放在参数的data属性里
    fileReader = new FileReader(),
    md5 = new SparkMD5(),
    md5_sum = 0,
    currentChunk = 0,
    chunkSize = Math.ceil(file_obj.size / 5), //分成5片，每片的大小
    start = 0;  //起始字节
  let loadFile = () => {
    let slice = file_obj.slice(start, start + chunkSize);  //根据字节范围切割每一片
    fileReader.readAsBinaryString(slice);
  }
  loadFile();
  fileReader.onload = e => {
    console.log("read chunk nr", currentChunk + 1, "of", 5);
    md5.appendBinary(e.target.result);
    currentChunk++;
    if (start < file_obj.size) {
      start += chunkSize;
      loadFile();
    } else {
      md5_sum = md5.end();
      obj.md5 = md5_sum;
      postMessage(JSON.stringify(obj));
    }
  };
});

```

<font style="color:rgb(77, 77, 77);">开始计算：</font>  
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1724930631152-44f37175-441f-4088-b12d-f0e488407bf9.png)  
<font style="color:rgb(77, 77, 77);">计算完成：</font>  
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1724930660841-65b45a63-589c-4b1f-93f8-722f2ad6e1c9.png)



webwork大文件上传

[代码地址：](https://github.com/LIAOJIANS/file-web-worker.git)  
  
当文件非常大时，直接计算整个文件的hash值可能会导致界面非常卡，这个你可以根据我的案例进行扩展。  
秒传的思路：  
1、用户上传文件时切片，并计算文件hash。  
2、根据用户名请求后端，返回对应切片hash数组，如果切片hash存在，则chunk过滤存在切片，并显示进度：存在切片数/chunk总数，并上传剩余切片（如果chunk过滤后为[]则上传成功）->秒传），如果切片hash不存在则继续上传.

无需计算整个文件hash。  
1.如1GB的文件，按10MB一个切片，随机取5-10个切片并计算hash。  
2. 将文件后缀+内存大小+切片hash值给后端对比。  
3. 若都一样，说明后端已有文件，无需重复上传。若没有说明要重新上传1GB整个文件  
<font style="color:rgb(77, 77, 77);"> </font>

