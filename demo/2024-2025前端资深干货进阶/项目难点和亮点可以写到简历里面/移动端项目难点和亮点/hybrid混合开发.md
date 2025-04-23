<br/>color1
<font style="color:rgb(65, 44, 12);">混合开发（即Hybrid App）其实是一种开发模式，它混合使用了Native和Web技术开发来实现同一个应用</font>

<br/>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1727141482824-3c7855e0-a84f-469b-9c5e-b70966909fa1.png)

主流跨端目前还是比较多的，分了以下这几大类：

1. **web整体渲染为主：** Cordova（前身为PhoneGap）
2. **原生渲染组件为主：** React Native，weex
3. **开放底层渲染能力：** Flutter
4. **较封闭的混合渲染：** 微信/支付宝/抖音/百度等小程序 （通常会用到taro或者uniapp做技术栈）

 

**<font style="color:rgb(97, 69, 0);">JSBridge</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1727141885848-67704abe-058a-4b5d-877b-eecb91adc37c.png)

<br/>color1
前端页面渲染到app上的webview中，原生与h5如何进行双向通信的？ 答案就是 **JSBridge**

JSBridge 简单来讲，主要是 给 JavaScript 提供调用 Native 功能的接口，让混合开发中的『前端部分』可以方便地使用地址位置、摄像头甚至支付等 Native 功能。

实际上，JSBridge 就像其名称中的『Bridge』的意义一样，是 Native 和非 Native 之间的桥梁，它的核心是 构建 Native 和非 Native 间消息通信的通道，而且是 双向通信的通道。

<br/>



在开发过程中，往往原生开发会与前端开发做出一个类似于接口文档形式的API使用文档的东西，根据文档来进行协助，即原生开发只注重如何封装成JS接口，然后给webview容器注入，而前端开发只要清楚调用即可

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1727141636165-9140b834-4ce6-4106-a1a7-e147c880c4e7.png)



**<font style="color:rgb(97, 69, 0);">JSBridge实现的两种方案</font>**

#### **拦截URL Schema**
**拦截WebView请求的URL Schema是H5对原生发起通信最早的一个方案，URL Schema是类URL的一种请求格式，**

**即 ：**`**<protocol>://<domain>/<path>?<query>**`** ，**

**如：**`**"jsbridge://getUserInfo?a=1&b=2"**`** ，当然 **`**<protocol> 协议头**`** 是自定义的，不一定非要是 **`**jsbridge**`

**其原理就是，如下图：**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1727141669466-1980fd07-e282-4032-8067-e343af17ae0f.png)

**原生移动端是可以拿到我们发出的任何一个请求的，他们会对这些请求进行处理，如果带上**`**<protocol> 协议头**`** 后就进行拦截处理，然后解析出对应的功能来返回给我们使用。**

**当与移动端协议好某个功能后，就可以调取了，有如下几种方式：**

1. **<font style="color:rgb(97, 69, 0);">a标签：</font>**

```html
<a href="jsbridge://getUserInfo?a=1&b=2">

  // 抖音的协议  
<a href='snssdk1128://user/profile/用户ID'>点击跳转</a>  
  // 微信的协议  
<a href='weixin://'>点击跳转</a> 
```

2. **<font style="color:rgb(97, 69, 0);">location.href：</font>**

```javascript
location.href = "jsbridge://getUserInfo?a=1&b=2"
```

3. **<font style="color:rgb(97, 69, 0);">window.open：</font>**

```javascript
window.open("jsbridge://getUserInfo?a=1&b=2")
```

4. **<font style="color:rgb(97, 69, 0);">i</font>****<font style="color:rgb(97, 69, 0);">frame.src：</font>**

```javascript
var iframe = document.createElement('iframe')
iframe.src = "jsbridge://getUserInfo?a=1&b=2"
```

5. **<font style="color:rgb(97, 69, 0);">发送</font>**`**<font style="color:rgb(97, 69, 0);">ajax</font>**`**<font style="color:rgb(97, 69, 0);">请求</font>**

```json
$ajax.get("jsbridge://getUserInfo?a=1&b=2")
```

#### **<font style="color:rgb(97, 69, 0);">WebView容器注入JSAPI</font>**
**可能刚才你会发现上面的拦截URL Schema这个方案，其实写起来是非常的丑陋的不直观，即便后面加深它的封装，也是不太优雅，而且最致命的问题就是URL是有长度限制的，当然可以通过扩展 ajax post 方式来解决URL长度问题但这样相对于使用方式又受到了限制，总之不太好，此时又有了另一种方案，就是向WebView容器注入JSAPI。**

**它的原理就是说App直接往Webview里注入js对象，这样Webview可以使用这些注入的js对象，就可以实现调取原生的功能了。**

**通常我们主要会选择使用DSBridge库，它是一个三端易用的现代跨平台 JSBridge，支持以类的方式集中统一管理API，同时还支持同步调用和异步调用。**

```javascript
dsBridge.call("callApp",{"action":"getUserInfo","params":{"a":1,"b":2}},res=> { 			      
  alert(JSON.stringify(res)); 
})
```

**上面可以看到，我们调用原生API是多么的直白清晰，但凡事没有什么绝对的完美，它也有个致命的缺陷，就是安卓低端机是不支持注入JSAPI的，如果开发的app需要兼容性非常高的话，这样就无法使用这个方案了。**



