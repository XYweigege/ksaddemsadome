**通讯和状态管理**

<font style="color:rgb(51, 51, 51);">就好像组件一样，子应用总会有各样的组合方式，那么父子应用间的通讯、兄弟应用之间的通讯、甚至爷孙应用之间的通讯就成了一个需要关注的问题。</font>

<font style="color:rgb(51, 51, 51);">态管理的问题，无论是全局下的状态管理、几个子应用间的局部状态管理，还是单个子应用间的状态管理，以及状态的上传和下发，都将是我们需要去考虑的问题</font>

<font style="color:rgb(51, 51, 51);"></font>

<font style="color:rgb(51, 51, 51);">微前端架构，在应用这一层级之上，目标是</font>**分开开发，分开构建，分开部署**

****

**js 沙箱**

<font style="color:rgb(51, 51, 51);">每一个子应用都是一个单独的项目和应用，在同一个页面中出现多个子应用是很常见的场景</font>

<font style="color:rgb(51, 51, 51);">全局变量污染、多版本库，需要对每个子应用间的 js 的工作空间进行隔离，使每个子应用内的执行自洽，只关注输出和输入，那么js沙箱的实现也就无疑成为了微前端方案中急需关注的一点。</font>

**css 沙箱**

<font style="color:rgb(51, 51, 51);">本质上同一个页面中每个子应用的挂载依旧在同一个dom树上</font>

<font style="color:rgb(51, 51, 51);">不希望子应用间的样式会出现互相干扰的情况，并且在子应用切换时可以自行装载和卸载</font>

<font style="color:rgb(51, 51, 51);"></font>

**<font style="color:#DF2A3F;">iframe实现微前端的方案虽然有很多问题，但是在js和css的隔离的上无疑是成本最低且较好的实现 </font>**

**<font style="color:#DF2A3F;"></font>**

### <font style="color:rgb(39, 56, 73);">预加载</font>
<font style="color:rgb(51, 51, 51);">在微前端的方案中，每个子应用都是独立的，所以如果不做任何处理的话在一个页面存在多个子应用的情况下，每个子应用的加载都是独立的，在这种情况下我们就很容易让用户体验到不同功能之间陆续从白屏到加载完成的割裂过程</font>

<font style="color:rgb(51, 51, 51);"></font>

<font style="color:rgb(51, 51, 51);">并且在实际用户使用中，浏览器大部分时间是处在空闲的，我们要怎么利用这样的空闲时间，去加载其他子应用的JS，用来优化用户体验？</font>

<font style="color:rgb(51, 51, 51);"></font>

### <font style="color:rgb(39, 56, 73);">公共依赖的处理问题</font>
<font style="color:rgb(51, 51, 51);">这个问题属于我们软件开发中项目管理的部分，子项目多了之后，公共依赖如果处理不好，不但造成我们开发工时的浪费，有各种重复工作，同时BUG的风险也会随着复制的代码过多，成指数增长，更不要说日后的长期维护</font>

1. <font style="color:rgb(51, 51, 51);">可以企业内部搭建npm服务器发布到，但问题也很明显，例如npm包更新，需要手动更新、任何改动都需要子应用重新部署上线，用的子应用多了，这更新气来就麻烦死了</font>
2. <font style="color:rgb(51, 51, 51);">webpack external 外部扩展，可以将通用的一些包排除在bundle之外，然后使用直接访问公共包JS的方式（一般采用CDN），直接在index.html中引入，但是这样的话，公共依赖必须采用UMD格式，那用的时候也要遵循，就有点麻烦</font>
3. <font style="color:rgb(51, 51, 51);">webpack federation 模块联邦，可以使一个JS应用，动态加载其他JS应用的代码，并且我们可以把一些公共依赖，都抽离到主包。在子包中，只输出业务代码即可，模块联邦提供对应的配置功能，并且由于是从网络获取，可以做做热更新。但是老项目要升级webpack，子项目和主项目都需要进行webpack federation的配置工作，才能用，这个也是要开发工作量，太麻烦</font>
4. <font style="color:rgb(51, 51, 51);">monorepo 多包管理，目前主流使用lerna框架进行多包管理，把单独的包抽离到独立的子项目中维护，后期如果项目稳定，可以把依赖抽离到webpack federation 做热更新，但是也好麻烦</font>

### <font style="color:rgb(39, 56, 73);">路由的管理</font>
#### <font style="color:rgb(39, 56, 73);">路由劫持</font>
<font style="color:rgb(51, 51, 51);">方案原理大致是监听了popstats或者hashchange事件，并劫持了浏览器history下的pushState和replaceState后</font>

<font style="color:rgb(51, 51, 51);"></font>

### <font style="color:rgb(39, 56, 73);">子应用的资源加载方式</font>
<font style="color:rgb(51, 51, 51);">是html entry还是js entry。别问，问就是快，一个加载的是按原来方式打包出来的一个html文件，一个是加载子应用打出来的整个js文件。</font>

<font style="color:rgb(51, 51, 51);">而且将整个微应用打包成一个 JS 文件常见的打包优化基本上都没了，按需加载、首屏资源加载优化、css 独立打包等想都别想</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731636636419-55786416-af83-4894-b9f9-3b6d98cdcdde.png)



<font style="color:rgb(51, 51, 51);">在我们需要一个微前端方案时，我们需要考虑的问题有如下几点：</font>

+ <font style="color:rgb(51, 51, 51);">应用间的通讯</font>
+ <font style="color:rgb(51, 51, 51);">应用间的状态管理</font>
+ <font style="color:rgb(51, 51, 51);">js 沙箱</font>
+ <font style="color:rgb(51, 51, 51);">css 隔离</font>
+ <font style="color:rgb(51, 51, 51);">预加载</font>
+ <font style="color:rgb(51, 51, 51);">公共依赖的处理</font>
+ <font style="color:rgb(51, 51, 51);">路由状态管理</font>
+ <font style="color:rgb(51, 51, 51);">是否支持html entry</font>
+ <font style="color:rgb(51, 51, 51);">应用支不支持保活</font>
+ <font style="color:rgb(51, 51, 51);">子应用之间的互相嵌套</font>
+ <font style="color:rgb(51, 51, 51);">是否有生命周期的设计</font>

