<font style="color:rgb(51, 51, 51);">qiankun是一个很经典的基座模式下的微服务方案，但是在使用成本上来说是有点大的，它对代码的侵入型很强，如果要改造的话，从 webpack、代码、路由等等都要做一系列的适配</font>

<font style="color:rgb(51, 51, 51);"></font>

| **能力** | **现状** |
| :--- | :--- |
| <font style="color:rgb(51, 51, 51);">通讯与状态管理</font> | <font style="color:rgb(51, 51, 51);">1、初始化状态通过props传入子组件   </font><br/><font style="color:rgb(51, 51, 51);">2、qiankun通过initGlobalState, onGlobalStateChange, setGlobalState实现主应⽤的全局状态管理，然后默认会通过props将通信⽅法传递给⼦应⽤，但是本质上还是通过发布 - 订阅的方式来进行通讯  </font><br/><font style="color:rgb(51, 51, 51);">3、主子应用localStrage、cookie可共享 所以在通讯上并不是很完美</font> |
| <font style="color:rgb(51, 51, 51);">js和css隔离</font> | <font style="color:rgb(51, 51, 51);">提供js和css隔离，但是在js的沙箱方面依然有不少坑有问题，近一年的很多changelog都是在修js沙箱的问题</font> |
| <font style="color:rgb(51, 51, 51);">预加载</font> | <font style="color:rgb(51, 51, 51);">做了静态资源的预加载能力</font> |
| <font style="color:rgb(51, 51, 51);">公共依赖的处理</font> | <font style="color:rgb(51, 51, 51);">文档内写明不推荐，但是硬要的话可以在微应用中将公共依赖配置成 external，然后在主应用中导入这些公共依赖的</font> |
| <font style="color:rgb(51, 51, 51);">路由管理</font> | <font style="color:rgb(51, 51, 51);">1、每个子应用中注册，然后由主应用进行管理，主应用的路由实例通过 props 传给微应用，微应用这个路由实例跳转  </font><br/><font style="color:rgb(51, 51, 51);">2、注册微应用的基础配置信息。当浏览器 url 发生变化时，会自动检查每一个微应用注册的 activeRule 规则，符合规则的应用将会被自动激活  </font><br/><font style="color:rgb(51, 51, 51);">3、页面上不能同时显示多个依赖于路由的微应用，因为浏览器只有一个 url，如果有多个依赖路由的微应用同时被激活，那么必定会导致其中一个 404。 </font>**(因为基于路由匹配，所以无法同时激活多个子应用)** |
| <font style="color:rgb(51, 51, 51);">html entry</font> | <font style="color:rgb(51, 51, 51);">支持</font> |
| <font style="color:rgb(51, 51, 51);">应用保活</font> | <font style="color:rgb(51, 51, 51);">无法支持子应用保活</font> |
| <font style="color:rgb(51, 51, 51);">应用嵌套</font> | <font style="color:rgb(51, 51, 51);">支持</font> |
| <font style="color:rgb(51, 51, 51);">生命周期</font> | |


