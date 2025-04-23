<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><script></font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">标签不受跨域限制的特点，缺点是只能支持 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> 请求</font>

+ <font style="color:rgb(44, 62, 80);">创建</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);">标签</font>
+ <font style="color:rgb(44, 62, 80);">设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);">标签的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">src</font><font style="color:rgb(44, 62, 80);">属性，以问号传递参数，设置好回调函数</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callback</font><font style="color:rgb(44, 62, 80);">名称</font>
+ <font style="color:rgb(44, 62, 80);">插入到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);">文本中</font>
+ <font style="color:rgb(44, 62, 80);">调用回调函数，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">res</font><font style="color:rgb(44, 62, 80);">参数就是获取的数据</font>

```javascript
function jsonp({url,params,callback}) {
  return new Promise((resolve,reject)=>{
    let script = document.createElement('script')

    window[callback] = function (data) {
      resolve(data)
      document.body.removeChild(script)
    }
    var arr = []
    for(var key in params) {
      arr.push(`${key}=${params[key]}`)
    }
    script.type = 'text/javascript'
    script.src = `${url}?callback=${callback}&${arr.join('&')}`
    document.body.appendChild(script)
  })
}
```

```javascript
// 测试用例
jsonp({
  url: 'http://suggest.taobao.com/sug',
  callback: 'getData',
  params: {
    q: 'iphone手机',
    code: 'utf-8'
  },
}).then(data=>{console.log(data)})
```

+ <font style="color:rgb(44, 62, 80);">设置</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CORS: Access-Control-Allow-Origin：*</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">postMessage</font>



