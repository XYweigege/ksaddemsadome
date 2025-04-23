**<font style="color:rgb(44, 62, 80);">步骤</font>**

+ <font style="color:rgb(44, 62, 80);">创建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XMLHttpRequest</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例</font>
+ <font style="color:rgb(44, 62, 80);">发出 HTTP 请求</font>
+ <font style="color:rgb(44, 62, 80);">服务器返回 XML 格式的字符串</font>[](https://www.123fe.net/docs/base.html#_2-%E5%BE%AE%E4%BF%A1%E5%B0%8F%E7%A8%8B%E5%BA%8F%E6%80%8E%E6%A0%B7%E8%B7%9F%E4%BA%8B%E4%BB%B6%E4%BC%A0%E5%80%BC)
+ <font style="color:rgb(44, 62, 80);">JS 解析 XML，并更新局部页面</font>
+ <font style="color:rgb(44, 62, 80);">不过随着历史进程的推进，XML 已经被淘汰，取而代之的是 JSON。</font>

<font style="color:rgb(44, 62, 80);">了解了属性和方法之后，根据 AJAX 的步骤，手写最简单的 GET 请求。</font>

### [](https://www.123fe.net/docs/base/handwritten.html#_1-%E5%8E%9F%E7%94%9F%E5%AE%9E%E7%8E%B0)<font style="color:rgb(44, 62, 80);">1 原生实现</font>
```javascript
function ajax() {
  let xhr = new XMLHttpRequest() //实例化，以调用方法
  xhr.open('get', 'https://www.google.com')  //参数2，url。参数三：异步
  xhr.onreadystatechange = () => {  //每当 readyState 属性改变时，就会调用该函数。
    if (xhr.readyState === 4) {  //XMLHttpRequest 代理当前所处状态。
      if (xhr.status >= 200 && xhr.status < 300) {  //200-300请求成功
        let string = request.responseText
        //JSON.parse() 方法用来解析JSON字符串，构造由字符串描述的JavaScript值或对象
        let object = JSON.parse(string)
      }
    }
  }
  request.send() //用于实际发出 HTTP 请求。不带参数为GET请求
}
```

### [](https://www.123fe.net/docs/base/handwritten.html#_2-promise%E5%AE%9E%E7%8E%B0)<font style="color:rgb(44, 62, 80);">2 Promise实现</font>
<font style="color:rgb(44, 62, 80);">基于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">封装</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ajax</font>

+ <font style="color:rgb(44, 62, 80);">返回一个新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">实例</font>
+ <font style="color:rgb(44, 62, 80);">创建</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HMLHttpRequest</font><font style="color:rgb(44, 62, 80);">异步对象</font>
+ <font style="color:rgb(44, 62, 80);">调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">open</font><font style="color:rgb(44, 62, 80);">方法，打开</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">url</font><font style="color:rgb(44, 62, 80);">，与服务器建立链接（发送前的一些处理）</font>
+ <font style="color:rgb(44, 62, 80);">监听</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ajax</font><font style="color:rgb(44, 62, 80);">状态信息</font>
+ <font style="color:rgb(44, 62, 80);">如果</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">xhr.readyState == 4</font><font style="color:rgb(44, 62, 80);">（表示服务器响应完成，可以获取使用服务器的响应了）</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">xhr.status == 200</font><font style="color:rgb(44, 62, 80);">，返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resolve</font><font style="color:rgb(44, 62, 80);">状态</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">xhr.status == 404</font><font style="color:rgb(44, 62, 80);">，返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reject</font><font style="color:rgb(44, 62, 80);">状态</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">xhr.readyState !== 4</font><font style="color:rgb(44, 62, 80);">，把请求主体的信息基于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">send</font><font style="color:rgb(44, 62, 80);">发送给服务器</font>

```javascript
function ajax(url) {
  return new Promise((resolve, reject) => {
    let xhr = new XMLHttpRequest()
    xhr.open('get', url)
    xhr.onreadystatechange = () => {
      if (xhr.readyState == 4) {
        if (xhr.status >= 200 && xhr.status <= 300) {
          resolve(JSON.parse(xhr.responseText))
        } else {
          reject('请求出错')
        }
      }
    }
    xhr.send()  //发送hppt请求
  })
}

let url = '/data.json'
ajax(url).then(res => console.log(res))
  .catch(reason => console.log(reason))
```

