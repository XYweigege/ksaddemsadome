<font style="color:rgb(52, 73, 94);">它借鉴了WebComponent的思想，通过</font>`<font style="color:rgb(233, 105, 0);background-color:rgb(248, 248, 248);">js沙箱</font>`<font style="color:rgb(52, 73, 94);">、</font>`<font style="color:rgb(233, 105, 0);background-color:rgb(248, 248, 248);">样式隔离</font>`<font style="color:rgb(52, 73, 94);">、</font>`<font style="color:rgb(233, 105, 0);background-color:rgb(248, 248, 248);">元素隔离</font>`<font style="color:rgb(52, 73, 94);">、</font>`<font style="color:rgb(233, 105, 0);background-color:rgb(248, 248, 248);">路由隔离</font>`<font style="color:rgb(52, 73, 94);">模拟实现了ShadowDom的隔离特性，并结合CustomElement将微前端封装成一个类WebComponent组件，从而实现微前端的组件化渲染，旨在降低上手难度、提升工作效率。</font>

`<font style="color:rgb(233, 105, 0);background-color:rgb(248, 248, 248);">micro-app</font>`<font style="color:rgb(52, 73, 94);">和技术栈无关，也不和业务绑定，可以用于任何前端框架。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731637915777-a260c913-dcaf-41a7-a8a4-02d129b1d440.png)

| **能力** | **现状** |
| :--- | :--- |
| <font style="color:rgb(51, 51, 51);">通讯与状态管理</font> | <font style="color:rgb(51, 51, 51);">基于发布订阅+CustomEvent</font> |
| <font style="color:rgb(51, 51, 51);">js和css隔离</font> | <font style="color:rgb(51, 51, 51);">js: 使用Proxy拦截了用户全局操作的行为。  </font><br/><font style="color:rgb(51, 51, 51);">css: 利用标签的name属性为每个样式添加前缀，将子应用的样式影响禁锢在当前标签区域，但是依旧没办法必定隔绝</font> |
| <font style="color:rgb(51, 51, 51);">预加载</font> | <font style="color:rgb(51, 51, 51);">在浏览器空闲时间，依照开发者传入的顺序，依次加载每个应用的静态资源，以确保不会影响基座应用的性能</font> |
| <font style="color:rgb(51, 51, 51);">公共依赖的处理</font> | <font style="color:rgb(51, 51, 51);">没有解决基座应用和子应用共用依赖的特性，issues中回答了在计划中，但是还没想好怎么做</font> |
| <font style="color:rgb(51, 51, 51);">路由管理</font> | <font style="color:rgb(51, 51, 51);">每个应用的路由实例都是不同的，应用的路由实例只能控制自身，无法影响其它应用，包括基座应用无法通过控制自身路由影响到子应，路由跳转只有三种方式：window.history，通过history.pushState或history.replaceState进行跳转、通过数据通信控制跳转(基座控制子应用跳转，子应用监听基座数据变化而跳转)、传递路由实例方法，把实例传到子应用里去跳转(子应用控制基座跳转)  url属性和子应用路由没有关系，只是用来加载html资源</font> |
| <font style="color:rgb(51, 51, 51);">html entry</font> | <font style="color:rgb(51, 51, 51);">支持, 实际上是以类WebComponent + HTML Entry实现微前端的组件化，但是webcomponents没有做降级处理</font> |
| <font style="color:rgb(51, 51, 51);">应用保活</font> | <font style="color:rgb(51, 51, 51);">支持，< micro-app name='xx' url='xx' keep-alive></font> |
| <font style="color:rgb(51, 51, 51);">应用嵌套</font> | <font style="color:rgb(51, 51, 51);">支持</font> |
| <font style="color:rgb(51, 51, 51);">生命周期</font> | <font style="color:rgb(51, 51, 51);">支持</font> |


