[https://juejin.cn/post/7293935529199042611](https://juejin.cn/post/7293935529199042611)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731638331884-16101571-3e1f-4afc-a1b1-e8b341e3ff5f.png)





| **能力** | **现状** |
| :--- | :--- |
| <font style="color:rgb(51, 51, 51);">通讯与状态管理</font> | <font style="color:rgb(51, 51, 51);">别想了，互相之间都是独立模块，并且去中心了，你还想咋共享？</font> |
| <font style="color:rgb(51, 51, 51);">js和css隔离</font> | <font style="color:rgb(51, 51, 51);">js: 没有沙箱，全凭开发者自觉不瞎搞   css: 别想了，只能通过 postcss-selector-namespace 添加前缀或者别名之类的方式来处理 简单来说，没有有用的 css 沙箱和 js 沙箱，一切需求靠用户自觉</font> |
| <font style="color:rgb(51, 51, 51);">预加载</font> | <font style="color:rgb(51, 51, 51);">没有利用浏览器空闲时间去做子应用加载的处理</font> |
| <font style="color:rgb(51, 51, 51);">公共依赖的处理</font> | <font style="color:rgb(51, 51, 51);">天然支持</font> |
| <font style="color:rgb(51, 51, 51);">路由管理</font> | <font style="color:rgb(51, 51, 51);">微应用的路由是有发生冲突的可能性的，为了实现独立部署能切换页面，各微应用都有自己的路由，要解决就只能微应用单独部署时，使用 web 路由，集成到 container 时，使用内存路由，web 路由由 container 接管，浏览器地址栏变化时，告 诉集成进来的微应用，然后微应用再跳转到相应的页面。</font> |
| <font style="color:rgb(51, 51, 51);">html entry</font> | <font style="color:rgb(51, 51, 51);">没有</font> |
| <font style="color:rgb(51, 51, 51);">应用保活</font> | <font style="color:rgb(51, 51, 51);">没有，别想</font> |
| <font style="color:rgb(51, 51, 51);">应用嵌套</font> | <font style="color:rgb(51, 51, 51);">想怎么套就怎么套</font> |
| <font style="color:rgb(51, 51, 51);">生命周期</font> | <font style="color:rgb(51, 51, 51);">别想</font> |


