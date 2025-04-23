### 简历里面可以写这些点
+ <font style="color:rgb(7, 17, 27);">通用组件库的封装：将项目中通用的组件抽离出来形成单独的组件库，这个各个页面在使用的时候直接使用封装的组件库，方便后期的维护和更新。</font>
+ <font style="color:rgb(7, 17, 27);">统一编码规范和风格，制定前端规范全家桶, ：</font>eslint+prettier+husky+lint-staged<font style="color:rgb(7, 17, 27);"> + </font>TypeScript <font style="color:rgb(7, 17, 27);">定义一套编码规范，保证团队代码规范统一；</font>
+ <font style="color:rgb(7, 17, 27);">统一开发了团队内的脚手架工具，沉淀出了基础的项目模板</font>
+ <font style="color:rgb(7, 17, 27);">极致的axios的封装（针对请求竞态，请求缓存， 请求参数加密）做了优化</font>
    - <font style="color:rgb(7, 17, 27);">说清楚什么是 请求竞态 请求缓存 请求参数加密的使用场景和技术实现</font>
+ <font style="color:rgb(7, 17, 27);">针对系统首页加载做了极致的优化，原来的白屏时间从xx秒降低到了xx秒；</font>
+ <font style="color:rgb(7, 17, 27);">对项目整体做了打包优化：打包体积从xxx M减少到xxxx kb；把整个思路说一下，怎么引入依赖分析库的（</font><font style="color:rgb(51, 51, 51);">rollup-plugin-visualizer， </font><font style="color:rgb(7, 17, 27);">），找到需要优化的点</font>

减少文件的体积，体积小了加载自然就快了。如：首屏优化、CDN加速、UI组件库按需加载，gzip br压缩方案对比

代码层面的优化。如：组件、样式的抽取，路由懒加载、Vue3性能优化、代码层面优化 等等

### <font style="color:rgba(0, 0, 0, 0.847);">可能涉及到的技术点：</font>
组件的封装，组件封装的原则？怎么继承第三方组件（element-ui antdesign ）的事件属性方法, 最好举例说明！

代码风格的指定，怎么开发自己的eslint插件（集成团队的代码规范），项目里面的eslint prettier相关的配置文件说明 （在这里 eslint官网至少要看一下，项目当中的.prettierrc  .eslintrc.js 配置文件每个配置项的作用你要清楚）

如何开发一个脚手架需要的第三方的插件你要能说上来，至少自己得动手实践过

axios的封装随处可见，要了解 xhr 和fetch的区别，用法，http请求状态码

最核心的性能优化部分：不管是首屏加载优化，还是项目打包优化，体积优化，

怎么做的？（一定要动手在项目里面实战，千万不要翻翻网上的文章看一下，以为看懂了其实啥也不会）

怎么做的 cdn加速？

手动拆包分包，自动的拆包分包？

 怎么区分不同的打包环境，对输出的dist目录又是如何更改的；

代码压缩的方案和具体的配置。

按需引入具体是怎么做的？举例说明一下

其他的几个维度：

**<font style="color:#601BDE;">减少请求次数</font>**

**<font style="color:#D22D8D;">减少资源大小</font>**

**<font style="color:#601BDE;">优化加载时机</font>**

**<font style="color:#601BDE;">优化加载方式</font>**

**<font style="color:#601BDE;">提高响应速度</font>**

**<font style="color:#D22D8D;">提高加载速度</font>**

骨架屏又是怎么做的？







