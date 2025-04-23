### <font style="color:rgb(44, 62, 80);">HTTP基础总结</font>
**<font style="color:rgb(44, 62, 80);">HTTP状态码</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1XX</font><font style="color:rgb(44, 62, 80);">：信息状态码</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100 Continue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">继续，一般在发送</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">post</font><font style="color:rgb(44, 62, 80);">请求时，已发送了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">http header</font><font style="color:rgb(44, 62, 80);">之后服务端将返回此信息，表示确认，之后发送具体参数信息</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">2XX</font><font style="color:rgb(44, 62, 80);">：成功状态码</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">200 OK</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">正常返回信息</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">201 Created</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">请求成功并且服务器创建了新的资源</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">202 Accepted</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">服务器已接受请求，但尚未处理</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">3XX</font><font style="color:rgb(44, 62, 80);">：重定向</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">301 Moved Permanently</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">请求的网页已永久移动到新位置。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">302 Found</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">临时性重定向。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">303 See Other</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">临时性重定向，且总是使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GET</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">请求新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">URI</font><font style="color:rgb(44, 62, 80);">。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">304 Not Modified</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">自从上次请求后，请求的网页未修改过。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">4XX</font><font style="color:rgb(44, 62, 80);">：客户端错误</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">400 Bad Request</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">401 Unauthorized</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">请求未授权。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">403 Forbidden</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">禁止访问。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">404 Not Found</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">找不到如何与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">URI</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">相匹配的资源。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">5XX:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">服务器错误</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">500 Internal Server Error</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">最常见的服务器端错误。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">503 Service Unavailable</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">服务器端暂时无法处理请求（可能是过载或维护）。</font>

**<font style="color:rgb(44, 62, 80);">常见状态码</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">200</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">成功</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">301</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">永久重定向（配合</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">location</font><font style="color:rgb(44, 62, 80);">，浏览器自动处理）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">302</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">临时重定向（配合</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">location</font><font style="color:rgb(44, 62, 80);">，浏览器自动处理）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">304</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">资源未被修改</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">403</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">没有权限访问，一般做权限角色</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">404</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">资源未找到</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">500</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Internal Server Error</font><font style="color:rgb(44, 62, 80);">服务器内部错误</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">502</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Bad Gateway</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">503</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Service Unavailable</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">504</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Gateway Timeout</font><font style="color:rgb(44, 62, 80);">网关超时</font>

**<font style="color:rgb(44, 62, 80);">502 与 504 的区别</font>**

<font style="color:rgb(44, 62, 80);">这两种异常状态码都与网关</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Gateway</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">有关，首先明确两个概念</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy (Gateway)</font><font style="color:rgb(44, 62, 80);">，反向代理层或者网关层。在公司级应用中一般使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Nginx</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">扮演这个角色</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Application (Upstream server)</font><font style="color:rgb(44, 62, 80);">，应用层服务，作为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">层的上游服务。在公司中一般为各种语言编写的服务器应用，如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Go/Java/Python/PHP/Node</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等</font>
+ **<font style="color:rgb(44, 62, 80);">此时关于 502 与 504 的区别就很显而易见</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">502 Bad Gateway</font><font style="color:rgb(44, 62, 80);">：一般表现为你自己写的「应用层服务(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Java/Go/PHP</font><font style="color:rgb(44, 62, 80);">)挂了」，或者网关指定的上游服务直接指错了地址，网关层无法接收到响应</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">504 Gateway Timeout</font><font style="color:rgb(44, 62, 80);">：一般表现为「应用层服务 (</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Upstream</font><font style="color:rgb(44, 62, 80);">) 超时，超过了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Gatway</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">配置的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Timeout</font><font style="color:rgb(44, 62, 80);">」，如查库操作耗时三分钟，超过了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Nginx</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">配置的超时时间</font>

**<font style="color:rgb(44, 62, 80);">http headers</font>**

+ **<font style="color:rgb(44, 62, 80);">常见的Request Headers</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Accept</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">浏览器可接收的数据格式</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Accept-Enconding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">浏览器可接收的压缩算法，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">gzip</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Accept-Language</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">浏览器可接收的语言，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">zh-CN</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Connection:keep-alive</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">TCP</font><font style="color:rgb(44, 62, 80);">连接重复复用</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cookie</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Host</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">请求的域名是什么</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">User-Agent</font><font style="color:rgb(44, 62, 80);">（简称</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UA</font><font style="color:rgb(44, 62, 80);">） 浏览器信息</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Content-type</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">发送数据的格式，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">application/json</font>
+ **<font style="color:rgb(44, 62, 80);">常见的Response Headers</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Content-type</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">返回数据的格式，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">application/json</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Content-length</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">返回数据的大小，多少字节</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Content-Encoding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">返回数据的压缩算法，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">gzip</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">set-cookie</font>
+ **<font style="color:rgb(44, 62, 80);">缓存相关的Headers</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache Control</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Expired</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-Modified</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-Modified-Since</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Etag</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-None-Match</font>

**<font style="color:rgb(44, 62, 80);">从输入URL到显示出页面的整个过程</font>**

+ **<font style="color:rgb(44, 62, 80);">下载资源</font>**<font style="color:rgb(44, 62, 80);">：各个资源类型，下载过程</font>
+ **<font style="color:rgb(44, 62, 80);">加载过程</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DNS</font><font style="color:rgb(44, 62, 80);">解析：域名 =></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IP</font><font style="color:rgb(44, 62, 80);">地址</font>
    - <font style="color:rgb(44, 62, 80);">浏览器根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IP</font><font style="color:rgb(44, 62, 80);">地址向服务器发起</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">请求</font>
    - <font style="color:rgb(44, 62, 80);">服务器处理</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">请求，并返回浏览器</font>
+ **<font style="color:rgb(44, 62, 80);">渲染过程</font>**
    - <font style="color:rgb(44, 62, 80);">根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM Tree</font>
    - <font style="color:rgb(44, 62, 80);">根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSSOM</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM Tree</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSSOM</font><font style="color:rgb(44, 62, 80);">整合形成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Render Tree</font><font style="color:rgb(44, 62, 80);">，根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Render Tree</font><font style="color:rgb(44, 62, 80);">渲染页面</font>
    - <font style="color:rgb(44, 62, 80);">遇到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><script></font><font style="color:rgb(44, 62, 80);">暂停渲染，优先加载并执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">代码，执行完在解析渲染（JS线程和渲染线程共用一个线程，JS执行要暂停DOM渲染）</font>
    - <font style="color:rgb(44, 62, 80);">直至把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Render Tree</font><font style="color:rgb(44, 62, 80);">渲染完成</font>

**<font style="color:rgb(44, 62, 80);">window.onload和DOMContentLoaded</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onload</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">页面的全部资源加载完才会执行，包括图片、视频等</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOMContentLoaded</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">渲染完即可，图片可能尚未下载</font>

```javascript
window.addEventListener('load',function() {
  // 页面的全部资源加载完才会执行，包括图片、视频等
})
window.addEventListener('DOMContentLoaded',function() {
  // DOM渲染完才执行，此时图片、视频等可能还没有加载完
})
```

<font style="color:rgb(44, 62, 80);">演示</font>

```html
<p>一段文字 1</p>
<p>一段文字 2</p>
<p>一段文字 3</p>
<img
    id="img1"
    src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570191150419&di=37b1892665fc74806306ce7f9c3f1971&imgtype=0&src=http%3A%2F%2Fimg.pconline.com.cn%2Fimages%2Fupload%2Fupc%2Ftx%2Fitbbs%2F1411%2F13%2Fc14%2F26229_1415883419758.jpg"
/>

<script>
  const img1 = document.getElementById('img1')
  img1.onload = function () {
    console.log('img loaded')
  }

  window.addEventListener('load', function () {
    console.log('window loaded')
  })

  document.addEventListener('DOMContentLoaded', function () {
    console.log('dom content loaded')
  })

  // 结果
  // dom content loaded
  // img loaded
  // window loaded
</script>
```

**<font style="color:rgb(44, 62, 80);">拓展：关于Restful API</font>**

+ <font style="color:rgb(44, 62, 80);">一种新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">API</font><font style="color:rgb(44, 62, 80);">设计方法</font>
+ <font style="color:rgb(44, 62, 80);">传统</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">API</font><font style="color:rgb(44, 62, 80);">设计：把每个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">url</font><font style="color:rgb(44, 62, 80);">当做一个功能</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Restful API</font><font style="color:rgb(44, 62, 80);">设计：把每个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">url</font><font style="color:rgb(44, 62, 80);">当前一个唯一的资源</font>
    - **<font style="color:rgb(44, 62, 80);">如何设计成一个资源</font>**
        * <font style="color:rgb(44, 62, 80);">尽量不用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">url</font><font style="color:rgb(44, 62, 80);">参数</font>
            + <font style="color:rgb(44, 62, 80);">传统</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">API</font><font style="color:rgb(44, 62, 80);">设计：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/api/list?pageIndex=2</font>
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Restful API</font><font style="color:rgb(44, 62, 80);">设计：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/api/list/2</font>
        * <font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">method</font><font style="color:rgb(44, 62, 80);">表示操作类型</font>
            + <font style="color:rgb(44, 62, 80);">传统</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">API</font><font style="color:rgb(44, 62, 80);">设计：</font>
                - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">post</font><font style="color:rgb(44, 62, 80);">新增请求：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/api/create-blog</font>
                - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">post</font><font style="color:rgb(44, 62, 80);">更新请求：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/api/update-blog?id=100</font>
                - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">post</font><font style="color:rgb(44, 62, 80);">删除请求：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/api/delete-blog?id=100</font>
                - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);">请求：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/api/get-blog?id=100</font>
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Restful API</font><font style="color:rgb(44, 62, 80);">设计：</font>
                - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">post</font><font style="color:rgb(44, 62, 80);">新增请求：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/api/blog</font>
                - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);">更新请求：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/api/blog/100</font>
                - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">delete</font><font style="color:rgb(44, 62, 80);">删除请求：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/api/blog/100</font>
                - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);">请求：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/api/blog/100</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#http%E7%BC%93%E5%AD%98)<font style="color:rgb(44, 62, 80);">HTTP缓存</font>
+ **<font style="color:rgb(44, 62, 80);">关于缓存介绍</font>**
    - <font style="color:rgb(44, 62, 80);">为什么需要缓存？减少网络请求（网络请求不稳定性），让页面渲染更快</font>
    - <font style="color:rgb(44, 62, 80);">哪些资源可以被缓存？静态资源（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">img</font><font style="color:rgb(44, 62, 80);">）</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">webpack</font><font style="color:rgb(44, 62, 80);">打包加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">contenthash</font><font style="color:rgb(44, 62, 80);">根据内容生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hash</font>
+ **<font style="color:rgb(44, 62, 80);">http缓存策略</font>**<font style="color:rgb(44, 62, 80);">（强制缓存 + 协商缓存）</font>
    - **<font style="color:rgb(44, 62, 80);">强制缓存</font>**
        * <font style="color:rgb(44, 62, 80);">服务端在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Response Headers</font><font style="color:rgb(44, 62, 80);">中返回给客户端</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache-Control</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">max-age=31536000</font><font style="color:rgb(44, 62, 80);">（单位：秒）一年</font>
        * **<font style="color:rgb(44, 62, 80);">Cache-Control的值</font>**
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">max-age</font><font style="color:rgb(44, 62, 80);">（常用）缓存的内容将在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">max-age</font><font style="color:rgb(44, 62, 80);">秒后失效</font>
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">no-cache</font><font style="color:rgb(44, 62, 80);">（常用）不要本地强制缓存，正常向服务端请求（只要服务端最新的内容）。需要使用协商缓存来验证缓存数据（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Etag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-Modified</font><font style="color:rgb(44, 62, 80);">）</font>
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">no-store</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不要本地强制缓存，也不要服务端做缓存，所有内容都不会缓存，强制缓存和协商缓存都不会触发</font>
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">public</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所有内容都将被缓存（客户端和代理服务器都可缓存）</font>
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">private</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所有内容只有客户端可以缓存</font>
        * **<font style="color:rgb(44, 62, 80);">Expires</font>**
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Expires</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Thu, 31 Dec 2037 23:55:55 GMT</font><font style="color:rgb(44, 62, 80);">（过期时间）</font>
            + <font style="color:rgb(44, 62, 80);">已被</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache-Control</font><font style="color:rgb(44, 62, 80);">代替</font>
        * **<font style="color:rgb(44, 62, 80);">Expires和Cache-Control的区别</font>**
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Expires</font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP1.0</font><font style="color:rgb(44, 62, 80);">的产物，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache-Control</font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP1.1</font><font style="color:rgb(44, 62, 80);">的产物</font>
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Expires</font><font style="color:rgb(44, 62, 80);">是服务器返回的具体过期时间，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache-Control</font><font style="color:rgb(44, 62, 80);">是相对时间</font>
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Expires</font><font style="color:rgb(44, 62, 80);">存在兼容性问题，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache-Control</font><font style="color:rgb(44, 62, 80);">优先级更高</font>
        * **<font style="color:rgb(44, 62, 80);">强制缓存的优先级高于协商缓存</font>**
        * **<font style="color:rgb(44, 62, 80);">强制缓存的流程</font>**
            + <font style="color:rgb(44, 62, 80);">浏览器第一次请求资源，服务器返回资源和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache-Control</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Expires</font>
            + <font style="color:rgb(44, 62, 80);">浏览器第二次请求资源，会带上</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache-Control</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Expires</font><font style="color:rgb(44, 62, 80);">，服务器根据这两个值判断是否命中强制缓存</font>
            + <font style="color:rgb(44, 62, 80);">命中强制缓存，直接从缓存中读取资源，返回给浏览器</font>
            + <font style="color:rgb(44, 62, 80);">未命中强制缓存，会带上</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-Modified-Since</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-None-Match</font><font style="color:rgb(44, 62, 80);">，服务器根据这两个值判断是否命中协商缓存</font>
            + <font style="color:rgb(44, 62, 80);">命中协商缓存，返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">304</font><font style="color:rgb(44, 62, 80);">，浏览器直接从缓存中读取资源</font>
            + <font style="color:rgb(44, 62, 80);">未命中协商缓存，返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">200</font><font style="color:rgb(44, 62, 80);">，浏览器重新请求资源</font>
        * **<font style="color:rgb(44, 62, 80);">强制缓存的流程图</font>**<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786223797-e0afd655-56b6-420f-8c83-98b2224cba4b.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786223901-caf9789f-60cf-4a47-9dbd-53b57ba7068f.png)
    - **<font style="color:rgb(44, 62, 80);">协商缓存</font>**
        * <font style="color:rgb(44, 62, 80);">服务端缓存策略</font>
        * <font style="color:rgb(44, 62, 80);">服务端判断客户端资源，是否和服务端资源一样</font>
        * <font style="color:rgb(44, 62, 80);">如果判断一致则返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">304</font><font style="color:rgb(44, 62, 80);">（不在返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);">、图片内容等资源），否则返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">200</font><font style="color:rgb(44, 62, 80);">和最新资源</font>
        * **<font style="color:rgb(44, 62, 80);">服务端怎么判断客户端资源一样？</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">根据资源标识</font>
            + <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Response Headers</font><font style="color:rgb(44, 62, 80);">中，有两种</font>
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-Modified</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Etag</font><font style="color:rgb(44, 62, 80);">会优先使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Etag</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-Modified</font><font style="color:rgb(44, 62, 80);">只能精确到秒级，如果资源被重复生成而内容不变，则</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Etag</font><font style="color:rgb(44, 62, 80);">更准确</font>
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-Modified</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">服务端返回的资源的最后修改时间</font>
                - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-Modified-Since</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">客户端请求时，携带的资源的最后修改时间（即</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-Modified</font><font style="color:rgb(44, 62, 80);">的值）</font><font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786223899-7239549a-2376-4b63-a7e0-17b3b2da0b8b.png)
            + <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Etag</font><font style="color:rgb(44, 62, 80);">服务端返回的资源的唯一标识（一个字符串，类似指纹）</font>
                - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-None-Matche</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">客户端请求时，携带的资源的唯一标识（即</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Etag</font><font style="color:rgb(44, 62, 80);">的值）</font><font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786223762-8bd8c179-ead6-48a8-92b2-edba31a01f5a.png)
            + **<font style="color:rgb(44, 62, 80);">Headers示例</font>**<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786224450-680d8228-7d5c-42f1-bb2e-57cdafded79b.png)
            + **<font style="color:rgb(44, 62, 80);">请求示例</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Etag</font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-Modified</font><font style="color:rgb(44, 62, 80);">命中缓存，没有返回资源，返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">304</font><font style="color:rgb(44, 62, 80);">，体积非常小</font><font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786226223-5efabc2a-f4aa-43ab-acb1-eb6f4ade2001.png)
    - **<font style="color:rgb(44, 62, 80);">HTTP缓存总结</font>**<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786226262-d54ffdf1-d0fe-47a4-bb7c-384915001c49.png)
+ **<font style="color:rgb(44, 62, 80);">刷新操作方式，对缓存的影响</font>**
    - <font style="color:rgb(44, 62, 80);">正常操作：地址栏输入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">url</font><font style="color:rgb(44, 62, 80);">，跳转链接，前进后退</font>
    - <font style="color:rgb(44, 62, 80);">手动操作：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">F5</font><font style="color:rgb(44, 62, 80);">，点击刷新，右键菜单刷新</font>
    - <font style="color:rgb(44, 62, 80);">强制刷新：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ctrl + F5</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">command + r</font>
+ **<font style="color:rgb(44, 62, 80);">不同刷新操作，不同缓存策略</font>**
    - <font style="color:rgb(44, 62, 80);">正常操作：强缓存有效，协商缓存有效</font>
    - <font style="color:rgb(44, 62, 80);">手动操作：强缓存失效，协商缓存有效</font>
    - <font style="color:rgb(44, 62, 80);">强制刷新：强缓存失效，协商缓存失效</font>
+ **<font style="color:rgb(44, 62, 80);">小结</font>**
    - <font style="color:rgb(44, 62, 80);">强缓存</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache-Contorl</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Expired</font><font style="color:rgb(44, 62, 80);">（弃用）</font>
    - <font style="color:rgb(44, 62, 80);">协商缓存</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-Modified</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-Modified-Since</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Etag</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-None-Matche</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">304</font><font style="color:rgb(44, 62, 80);">状态码</font>
    - <font style="color:rgb(44, 62, 80);">完整流程图</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#http%E5%8D%8F%E8%AE%AE1-0%E5%92%8C1-1%E5%92%8C2-0%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">HTTP协议1.0和1.1和2.0有什么区别</font>
+ **<font style="color:rgb(44, 62, 80);">HTTP1.0</font>**
    - <font style="color:rgb(44, 62, 80);">最基础的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">协议</font>
    - <font style="color:rgb(44, 62, 80);">支持基本的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GET</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">POST</font><font style="color:rgb(44, 62, 80);">方法</font>
+ **<font style="color:rgb(44, 62, 80);">HTTP1.1</font>**
    - <font style="color:rgb(44, 62, 80);">缓存策略</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cache-control</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">E-tag</font>
    - <font style="color:rgb(44, 62, 80);">支持长链接</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Connection:keep-alive</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">TCP</font><font style="color:rgb(44, 62, 80);">连接多次请求</font>
    - <font style="color:rgb(44, 62, 80);">断点续传，状态码</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">206</font>
    - <font style="color:rgb(44, 62, 80);">支持新的方法</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PUT DELETE</font><font style="color:rgb(44, 62, 80);">等，可用于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Restful API</font><font style="color:rgb(44, 62, 80);">写法</font>
+ **<font style="color:rgb(44, 62, 80);">HTTP2.0</font>**
    - <font style="color:rgb(44, 62, 80);">可压缩</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">header</font><font style="color:rgb(44, 62, 80);">，减少体积</font>
    - <font style="color:rgb(44, 62, 80);">多路复用，一次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">TCP</font><font style="color:rgb(44, 62, 80);">连接中可以多个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">并行请求</font>
    - <font style="color:rgb(44, 62, 80);">服务端推送（实际中使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">websocket</font><font style="color:rgb(44, 62, 80);">）</font>

**<font style="color:rgb(44, 62, 80);">连环问：HTTP协议和UDP协议有什么区别</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">是应用层，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">TCP</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UDP</font><font style="color:rgb(44, 62, 80);">是传输层</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">TCP</font><font style="color:rgb(44, 62, 80);">有连接（三次握手），有断开（四次挥手），传输稳定</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UDP</font><font style="color:rgb(44, 62, 80);">无连接，无断开不稳定传输，但效率高。如视频会议、语音通话</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#websocket%E5%92%8Chttp%E5%8D%8F%E8%AE%AE%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">WebSocket和HTTP协议有什么区别</font>
+ <font style="color:rgb(44, 62, 80);">支持端对端通信</font>
+ <font style="color:rgb(44, 62, 80);">可由</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">client</font><font style="color:rgb(44, 62, 80);">发起，也可由</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sever</font><font style="color:rgb(44, 62, 80);">发起</font>
+ <font style="color:rgb(44, 62, 80);">用于消息通知、直播间讨论区、聊天室、协同编辑</font>

**<font style="color:rgb(44, 62, 80);">WebSocket连接过程</font>**

+ <font style="color:rgb(44, 62, 80);">先发起一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">请求</font>
+ <font style="color:rgb(44, 62, 80);">成功之后在升级到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WebSocket</font><font style="color:rgb(44, 62, 80);">协议，再通讯</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786226340-1d964b99-73f5-4387-a14d-a5dcb1b58371.png)

**<font style="color:rgb(44, 62, 80);">WebSocket和HTTP区别</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WebSocket</font><font style="color:rgb(44, 62, 80);">协议名是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ws://</font><font style="color:rgb(44, 62, 80);">，可双端发起请求（双端都可以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">send</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onmessage</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WebSocket</font><font style="color:rgb(44, 62, 80);">没有跨域限制</font>
+ <font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">send</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onmessage</font><font style="color:rgb(44, 62, 80);">通讯（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">req</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">res</font><font style="color:rgb(44, 62, 80);">）</font>

**<font style="color:rgb(44, 62, 80);">WebSocket和HTTP长轮询的区别</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">长轮询：一般是由客户端向服务端发出一个设置较长网络超时时间的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">请求，并在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Http</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">连接超时前，不主动断开连接；待客户端超时或有数据返回后，再次建立一个同样的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">请求，重复以上过程</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">长轮询：客户端发起请求，服务端阻塞，不会立即返回</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">长轮询需要处理</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">timeout</font><font style="color:rgb(44, 62, 80);">，即</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">timeout</font><font style="color:rgb(44, 62, 80);">之后重新发起请求</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WebSocket</font><font style="color:rgb(44, 62, 80);">：客户端可发起请求，服务端也可发起请求</font>

**<font style="color:rgb(44, 62, 80);">ws可升级为wss（像https）</font>**

```javascript
import {createServer} from 'https'
import {readFileSync} from 'fs'
import {WebSocketServer} from 'ws'

const server = createServer({
  cert: readFileSync('/path/to/cert.pem'),
  key: readFileSync('/path/to/key.pem'),
})
const wss = new WebSocketServer({ server })
```

**<font style="color:rgb(44, 62, 80);">实际项目中推荐使用socket.io API更简洁</font>**

```javascript
io.on('connection',sockert=>{
  // 发送信息
  socket.emit('request', /**/)
  // 广播事件到客户端
  io.emit('broadcast', /**/)
  // 监听事件
  socket.on('reply', ()=>{/**/})
})
```

**<font style="color:rgb(44, 62, 80);">WebSocket基本使用例子</font>**

```javascript
// server.js
const { WebSocketServer } = require('ws') // npm i ws 
const wsServer = new WebSocketServer({ port: 3000 })

wsServer.on('connection', ws => {
  console.info('connected')

  ws.on('message', msg => {
    console.info('收到了信息', msg.toString())

    // 服务端向客户端发送信息
    setTimeout(() => {
      ws.send('服务端已经收到了信息: ' + msg.toString())
    }, 2000)
  })
})
```

```html
<!-- websocket main page -->
<button id="btn-send">发送消息</button>

<script>
    const ws = new WebSocket('ws://127.0.0.1:3000')
    ws.onopen = () => {
      console.info('opened')
      ws.send('client opened')
    }
    ws.onmessage = event => {
      console.info('收到了信息', event.data)
    }

    document.getElementById('btn-send').addEventListener('click', () => {
      console.info('clicked')
      ws.send('当前时间' + Date.now())
    })
</script>
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E8%AF%B7%E6%8F%8F%E8%BF%B0tcp%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B%E5%92%8C%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B)<font style="color:rgb(44, 62, 80);">请描述TCP三次握手和四次挥手</font>
**<font style="color:rgb(44, 62, 80);">建立TCP连接</font>**

+ <font style="color:rgb(44, 62, 80);">先建立连接，确保双方都有收发消息的能力</font>
+ <font style="color:rgb(44, 62, 80);">再传输内容（如发送一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);">请求）</font>
+ <font style="color:rgb(44, 62, 80);">网络连接是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">TCP</font><font style="color:rgb(44, 62, 80);">协议，传输内容是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">协议</font>

**<font style="color:rgb(44, 62, 80);">三次握手-建立连接</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">发包，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">接收。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">就知道有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">要找我了</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">发包，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">接收。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">就知道</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">已经收到消息</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">发包，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">接收。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">就知道</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">要准备发送了</font>
+ <font style="color:rgb(44, 62, 80);">前两步确定双发都能收发消息，第三步确定双方都准备好了</font>

**<font style="color:rgb(44, 62, 80);">四次挥手-关闭连接</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">发包，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">接收。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">就知道</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">已请求结束</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">发包，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">接收。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">就知道</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">已收到消息，我等待</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">server</font><font style="color:rgb(44, 62, 80);">传输完成了在关闭</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">发包，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">接收。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">就知道</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">已经传输完成了，可以关闭连接了</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">发包，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">接收。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">就知道</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Client</font><font style="color:rgb(44, 62, 80);">已经关闭了，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Server</font><font style="color:rgb(44, 62, 80);">可以关闭连接了</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786228717-b6d99055-55f8-4e53-96e2-f3050be02866.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786228740-4c0dad5c-5480-475e-9e2d-476e46660a61.png)

### [](https://www.123fe.net/docs/base/high-frequency.html#http%E8%B7%A8%E5%9F%9F%E8%AF%B7%E6%B1%82%E6%97%B6%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%8F%91%E9%80%81options%E8%AF%B7%E6%B1%82)<font style="color:rgb(44, 62, 80);">HTTP跨域请求时为什么要发送options请求</font>
**<font style="color:rgb(44, 62, 80);">跨域请求</font>**

+ <font style="color:rgb(44, 62, 80);">浏览器同源策略</font>
+ <font style="color:rgb(44, 62, 80);">同源策略一般限制</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ajax</font><font style="color:rgb(44, 62, 80);">网络请求，不能跨域请求</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">server</font>
+ <font style="color:rgb(44, 62, 80);">不会限制</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><link></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><img></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><script></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><iframe></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">加载第三方资源</font>

**<font style="color:rgb(44, 62, 80);">JSONP实现跨域</font>**

```html
<!-- aa.com网页 -->
<script>
  window.onSuccess = function(data) {
    console.log(data)
  }
</script>
<script src="https://bb.com/api/getData"></script>
```

```plain
// server端https://bb.com/api/getData
onSuccess({ "name":"test", "age":12, "city":"shenzhen" });
```

**<font style="color:rgb(44, 62, 80);">cors</font>**

```plain
response.setHeader('Access-Control-Allow-Origin', 'https://aa.com') // 或者*
response.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, OPTIONS') // 允许的请求方法
response.setHeader('Access-Control-Allow-Headers', 'X-Requested-With') // 允许的请求头
response.setHeader（'Access-Control-Allow-Credentials', 'true'）// 允许跨域携带cookie
```

**<font style="color:rgb(44, 62, 80);">多余的options请求</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786231316-f583b106-06a3-45e3-b932-176ab221c714.png)

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">options</font><font style="color:rgb(44, 62, 80);">是跨域请求之前的预检查</font>
+ <font style="color:rgb(44, 62, 80);">浏览器自行发起的，无需我们干预</font>
+ <font style="color:rgb(44, 62, 80);">不会影响实际的功能</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#http%E8%AF%B7%E6%B1%82%E4%B8%ADtoken%E3%80%81cookie%E3%80%81session%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">HTTP请求中token、cookie、session有什么区别</font>
**<font style="color:rgb(44, 62, 80);">cookie</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">无状态的，每次请求都要携带</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">,以帮助识别身份</font>
+ <font style="color:rgb(44, 62, 80);">服务端也可以向客户端</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">set-cookie</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">大小</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">4kb</font>
+ <font style="color:rgb(44, 62, 80);">默认有跨域限制：不可跨域共享，不可跨域传递</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">（可通过设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">withCredential</font><font style="color:rgb(44, 62, 80);">跨域传递</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">）</font>

**<font style="color:rgb(44, 62, 80);">cookie本地存储</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML5</font><font style="color:rgb(44, 62, 80);">之前</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">常被用于本地存储</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML5</font><font style="color:rgb(44, 62, 80);">之后推荐使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">localStorage</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sessionStorage</font>

**<font style="color:rgb(44, 62, 80);">现代浏览器开始禁止第三方cookie</font>**

+ <font style="color:rgb(44, 62, 80);">和跨域限制不同，这里是：禁止网页引入第三方js设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font>
+ <font style="color:rgb(44, 62, 80);">打击第三方广告设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font>
+ <font style="color:rgb(44, 62, 80);">可以通过属性设置</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SameSite:Strict/Lax/None</font>

**<font style="color:rgb(44, 62, 80);">cookie和session</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">用于登录验证，存储用户表示（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">userId</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">session</font><font style="color:rgb(44, 62, 80);">在服务端，存储用户详细信息，和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">信息一一对应</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie+session</font><font style="color:rgb(44, 62, 80);">是常见的登录验证解决方案</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786245180-7cbd09a8-b965-4a2e-a906-b65c20a03a0e.png)

```plain
// 登录：用户名 密码
// 服务端set-cookie: userId=x1 把用户id传给浏览器存储在cookie中
// 下次请求直接带上cookie:userId=x1 服务端根据userId找到哪个用户的信息

// 服务端session集中存储所有的用户信息在缓存中
const session = {
  x1: {
    username:'xx1',
    email:'xx1'
  },
  x2: { // 当下次来了一个用户x2也记录x2的登录信息,同时x1也不会丢失
    username:'xx2',
    email:'xx2'
  },
}
```

**<font style="color:rgb(44, 62, 80);">token和cookie</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">规范（每次请求都会携带），而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">是自定义传递</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">会默认被浏览器存储，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">需自己存储</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">默认没有跨域限制</font>

**<font style="color:rgb(44, 62, 80);">JWT(json web token)</font>**

+ <font style="color:rgb(44, 62, 80);">前端发起登录，后端验证成功后，返回一个加密的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font>
+ <font style="color:rgb(44, 62, 80);">前端自行存储这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">（其他包含了用户信息，加密的）</font>
+ <font style="color:rgb(44, 62, 80);">以后访问服务端接口，都携带着这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">，作为用户信息</font>

**<font style="color:rgb(44, 62, 80);">session和jwt哪个更好？</font>**

+ **<font style="color:rgb(44, 62, 80);">session的优点</font>**
    - <font style="color:rgb(44, 62, 80);">用户信息存储在服务端，可快速封禁某个用户</font>
    - <font style="color:rgb(44, 62, 80);">占用服务端内存，成本高</font>
    - <font style="color:rgb(44, 62, 80);">多进程多服务器时不好同步，需要使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redis</font><font style="color:rgb(44, 62, 80);">缓存</font>
    - <font style="color:rgb(44, 62, 80);">默认有跨域限制</font>
+ **<font style="color:rgb(44, 62, 80);">JWT的优点</font>**
    - <font style="color:rgb(44, 62, 80);">不占用服务端内存，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">存储在客户端浏览器</font>
    - <font style="color:rgb(44, 62, 80);">多进程、多服务器不受影响</font>
    - <font style="color:rgb(44, 62, 80);">没有跨域限制</font>
    - <font style="color:rgb(44, 62, 80);">用户信息存储在客户端，无法快速封禁某用户（可以在服务端建立黑名单，也需要成本）</font>
    - <font style="color:rgb(44, 62, 80);">万一服务端密钥被泄露，则用户信息全部丢失</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">体积一般比</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">大，会增加请求的数据量</font>
+ <font style="color:rgb(44, 62, 80);">如严格管理用户信息（保密、快速封禁）推荐使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">session</font>
+ <font style="color:rgb(44, 62, 80);">没有特殊要求，推荐使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JWT</font>

**<font style="color:rgb(44, 62, 80);">如何实现SSO(Single Sign On)单点登录</font>**

+ <font style="color:rgb(44, 62, 80);">单点登录的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">本质就是在多个应用系统中共享登录状态</font><font style="color:rgb(44, 62, 80);">，如果用户的登录状态是记录在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Session</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的，要实现共享登录状态，就要先共享</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Session</font>
+ <font style="color:rgb(44, 62, 80);">所以实现单点登录的关键在于，如何让</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Session ID</font><font style="color:rgb(44, 62, 80);">（或</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Token</font><font style="color:rgb(44, 62, 80);">）在多个域中共享</font>
+ **<font style="color:rgb(44, 62, 80);">主域名相同，基于cookie实现单点登录</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">默认不可跨域共享，但有些情况下可设置跨域共享</font>
    - <font style="color:rgb(44, 62, 80);">主域名相同，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">www.baidu.com</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">image.baidu.com</font>
    - <font style="color:rgb(44, 62, 80);">设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie domain</font><font style="color:rgb(44, 62, 80);">为主域</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">baidu.com</font><font style="color:rgb(44, 62, 80);">，即可共享</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font>
    - <font style="color:rgb(44, 62, 80);">主域名不同，则</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">无法共享。可使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sso</font><font style="color:rgb(44, 62, 80);">技术方案来做</font>
+ **<font style="color:rgb(44, 62, 80);">主域名不同，基于SSO技术方案实现</font>**
    - <font style="color:rgb(44, 62, 80);">系统</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">A</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">B</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">域名都是独立的</font>
    - <font style="color:rgb(44, 62, 80);">用户访问系统</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">A</font><font style="color:rgb(44, 62, 80);">，系统</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">A</font><font style="color:rgb(44, 62, 80);">重定向到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">登录（登录页面在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">）输入用户名密码提交到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">，验证用户名密码，将登录状态写入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">session</font><font style="color:rgb(44, 62, 80);">，同时将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">作为参数返回给客户端</font>
    - <font style="color:rgb(44, 62, 80);">客户端携带</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">去访问系统</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">A</font><font style="color:rgb(44, 62, 80);">，系统</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">A</font><font style="color:rgb(44, 62, 80);">携带</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">去</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">验证，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">验证通过返回用户信息给系统</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">A</font>
    - <font style="color:rgb(44, 62, 80);">用户访问</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">B</font><font style="color:rgb(44, 62, 80);">系统，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">B</font><font style="color:rgb(44, 62, 80);">系统没有登录，重定向到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">获取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">（由于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">已经登录了，不需要重新登录认证，之前在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">A</font><font style="color:rgb(44, 62, 80);">系统登录过）,拿着</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">去</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">B</font><font style="color:rgb(44, 62, 80);">系统，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">B</font><font style="color:rgb(44, 62, 80);">系统拿着</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">去</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">里面换取用户信息</font>
    - <font style="color:rgb(44, 62, 80);">整个所有用户的登录、用户信息的保存、用户的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">token</font><font style="color:rgb(44, 62, 80);">验证，全部都在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSO</font><font style="color:rgb(44, 62, 80);">第三方独立的服务中处理</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786246364-3a14fe81-75b8-457c-afb2-3b6a8a91a56f.png)

### [](https://www.123fe.net/docs/base/high-frequency.html#%E4%BB%80%E4%B9%88%E6%98%AFhttps%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E5%87%BB-%E5%A6%82%E4%BD%95%E9%A2%84%E9%98%B2-https%E5%8A%A0%E5%AF%86%E8%BF%87%E7%A8%8B%E3%80%81%E5%8E%9F%E7%90%86)<font style="color:rgb(44, 62, 80);">什么是HTTPS中间人攻击，如何预防（HTTPS加密过程、原理）</font>
**<font style="color:rgb(44, 62, 80);">HTTPS加密传输</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">是明文传输</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTPS</font><font style="color:rgb(44, 62, 80);">加密传输</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP + TLS/SSL</font>

**<font style="color:rgb(44, 62, 80);">TLS 中的加密</font>**

+ **<font style="color:rgb(44, 62, 80);">对称加密</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">两边拥有相同的秘钥，两边都知道如何将密文加密解密。</font>
+ **<font style="color:rgb(44, 62, 80);">非对称加密</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">有公钥私钥之分，公钥所有人都可以知道，可以将数据用公钥加密，但是将数据解密必须使用私钥解密，私钥只有分发公钥的一方才知道</font>

**<font style="color:rgb(44, 62, 80);">对称密钥加密和非对称密钥加密它们有什么区别</font>**

+ <font style="color:rgb(44, 62, 80);">对称密钥加密是最简单的一种加密方式，它的加解密用的都是相同的密钥，这样带来的好处就是加解密效率很快，但是并不安全，如果有人拿到了这把密钥那谁都可以进行解密了。</font>
+ <font style="color:rgb(44, 62, 80);">而非对称密钥会有两把密钥，一把是私钥，只有自己才有；一把是公钥，可以发布给任何人。并且加密的内容只有相匹配的密钥才能解。这样带来的一个好处就是能保证传输的内容是安全的，因为例如如果是公钥加密的数据，就算是第三方截取了这个数据但是没有对应的私钥也破解不了。不过它也有缺点，一是公钥因为是公开的，谁都可以过去，如果内容是通过私钥加密的话，那拥有对应公钥的黑客就可以用这个公钥来进行解密得到里面的信息；二来公钥里并没有包含服务器的信息，也就是并不能确保服务器身份的合法性；并且非对称加密的时候要消耗一定的时间，减低了数据的传输效率。</font>

**<font style="color:rgb(44, 62, 80);">HTTPS加密的过程</font>**

1. <font style="color:rgb(44, 62, 80);">客户端请求</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">www.baidu.com</font>
2. <font style="color:rgb(44, 62, 80);">服务端存储着公钥和私钥</font>
3. <font style="color:rgb(44, 62, 80);">服务器把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CA</font><font style="color:rgb(44, 62, 80);">数字证书（包含公钥）响应式给客户端</font>
4. <font style="color:rgb(44, 62, 80);">客户端解析证书拿到公钥，并生成随机码</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">KEY</font><font style="color:rgb(44, 62, 80);">（加密的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">没有任何意义，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ABC</font><font style="color:rgb(44, 62, 80);">只有服务端的私钥才能解密出来，黑客劫持了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">KEY</font><font style="color:rgb(44, 62, 80);">也是没用的）</font>
5. <font style="color:rgb(44, 62, 80);">客户端把解密后的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">KEY</font><font style="color:rgb(44, 62, 80);">传递给服务端，作为接下来对称加密的密钥</font>
6. <font style="color:rgb(44, 62, 80);">服务端拿私钥解密随机码</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">KEY</font><font style="color:rgb(44, 62, 80);">，使用随机码</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">KEY</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对传输数据进行对称加密</font>
7. <font style="color:rgb(44, 62, 80);">把对称加密后的内容传输给客户端，客户端使用之前生成的随机码</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">KEY</font><font style="color:rgb(44, 62, 80);">进行解密数据</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786246503-7d521ddf-0769-4063-abd6-87f5ed3587c4.png)

**<font style="color:rgb(44, 62, 80);">介绍下https中间人攻击的过程</font>**

<font style="color:rgb(44, 62, 80);">这个问题也可以问成为什么需要CA认证机构颁发证书？</font>

<font style="color:rgb(44, 62, 80);">我们假设如果不存在认证机构，则人人都可以制造证书，这就带来了"中间人攻击"问题。</font>

**<font style="color:rgb(44, 62, 80);">中间人攻击的过程如下</font>**

+ <font style="color:rgb(44, 62, 80);">客户端请求被劫持，将所有的请求发送到中间人的服务器</font>
+ <font style="color:rgb(44, 62, 80);">中间人服务器返回自己的证书</font>
+ <font style="color:rgb(44, 62, 80);">客户端创建随机数，使用中间人证书中的公钥进行加密发送给中间人服务器，中间人使用私钥对随机数解密并构造对称加密，对之后传输的内容进行加密传输</font>
+ <font style="color:rgb(44, 62, 80);">中间人通过客户端的随机数对客户端的数据进行解密</font>
+ <font style="color:rgb(44, 62, 80);">中间人与服务端建立合法的https连接（https握手过程），与服务端之间使用对称加密进行数据传输，拿到服务端的响应数据，并通过与服务端建立的对称加密的秘钥进行解密</font>
+ <font style="color:rgb(44, 62, 80);">中间人再通过与客户端建立的对称加密对响应数据进行加密后传输给客户端</font>
+ <font style="color:rgb(44, 62, 80);">客户端通过与中间人建立的对称加密的秘钥对数据进行解密</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">简单来说，中间人攻击中，中间人首先伪装成服务端和客户端通信，然后又伪装成客户端和服务端进行通信（如图）。 整个过程中，由于缺少了证书的验证过程，虽然使用了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">https</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，但是传输的数据已经被监听，客户端却无法得知</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786246535-d03f1242-1c5e-46a9-9563-b2f52378dea4.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786246336-06a9ecd4-9f17-4b6b-84d5-e73a02091f62.png)

**<font style="color:rgb(44, 62, 80);">预防中间人攻击</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">使用正规厂商的证书，慎用免费的</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718786248896-3bd2f4ad-9ffa-4c2c-9537-3892baa64ac2.png)

## 
  


