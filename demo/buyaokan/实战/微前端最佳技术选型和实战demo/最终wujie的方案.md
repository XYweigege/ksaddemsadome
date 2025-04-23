<font style="color:rgb(51, 51, 51);">直接看 wujie 的github仓库 可以发现它的实现实际上非常简单，加起来不过3000行代码</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731638622903-97f4e1b6-6627-4140-9862-a3d6ca8c56c4.png)

## <font style="color:rgb(39, 56, 73);">wujie是怎么解决纯 iframe 微前端方案所遇到问题的？</font>
<font style="color:rgb(51, 51, 51);">wujie 是一个iframe + webComponent 的解决方案 既然提到了iframe，那么我们肯定要关注前面提到的iframe存在着通讯、路由、dom割裂严重、白屏时间长、难做预加载、应用无法保活、浏览器对相同域的连接有限制，会影响⻚⾯的并⾏加载等问题在wujie中是否被解决</font>



<font style="color:rgb(51, 51, 51);">在wujie中提供了三种通讯方式： </font>

<font style="color:rgb(51, 51, 51);">（1 </font>**props 通信**<font style="color:rgb(51, 51, 51);">，主应用可以通过props注入数据和方法 </font>

<font style="color:rgb(51, 51, 51);">（2 </font>**window 通信**<font style="color:rgb(51, 51, 51);">，由于在设计上子应用运行的iframe的src和主应用是同域的，所以相互可以直接通信，这个我们在下一步中细述 </font>

<font style="color:rgb(51, 51, 51);">（3 </font>**eventBus 通信**<font style="color:rgb(51, 51, 51);"> 其中最有趣的就是这一点，wujie 提供了一套去中心化的通讯方式</font>

<font style="color:rgb(51, 51, 51);">其中的 eventBus 的功能实现非常简单简洁，整块功能的实现不过 105 行，除了全部事件的存储的map会根据是否存在wujie进行判断和兼容之外，本质上就是一个非常常规的发布订阅的实现 </font>  


### <font style="color:rgb(39, 56, 73);">iframe 问题解决：dom 割裂严重、路由状态丢失、通信非常困难的问题</font>
#### <font style="color:rgb(39, 56, 73);">路由状态的问题</font>
<font style="color:rgb(51, 51, 51);">假如我们在应用a中想要加载应用b，那么我们可以怎么做才能避免上述问题？</font>

<font style="color:rgb(51, 51, 51);">在应用 A 中构造一个shadowRoot 和iframe，然后将应用 B 的html写入shadowRoot中，js运行在iframe中，因为iframe的js隔离真的很完美。</font>

<font style="color:rgb(51, 51, 51);">这时候我们注意 iframe 的 url，iframe保持和主应用同域但是保留子应用的路径信息，这样子应用的js可以运行在iframe的location和history中保持路由正确。</font>

**这样就可以解决了路由状态的问题，并且解决可以使用window来通讯**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731638851752-0614aca0-2998-44bd-9d2a-ab89a9f18164.png)

<font style="color:rgb(51, 51, 51);">Shadow DOM API 的 ShadowRoot 接口是一个 DOM 子树的根节点，它与文档的主 DOM 树分开渲染。</font>

**源码**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731638932527-03ab4a75-9d38-4420-a013-819060307304.png)

#### <font style="color:rgb(39, 56, 73);">dom割裂的问题</font>
<font style="color:rgb(51, 51, 51);">那在iframe中dom割裂的问题要怎么处理？</font>

<font style="color:rgb(51, 51, 51);">在iframe中拦截document对象，统一将dom指向shadowRoot，此时比如新建元素、弹窗或者冒泡组件就可以正常约束在shadowRoot内部。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731638968169-9a62f540-e088-4def-9999-5d73a1252e22.png)

<font style="color:rgb(51, 51, 51);">我们可以从源码中看出，在非降级的情况下， wujie对iframe的 document进行了代理，从而解决了解决了 dom 割裂严重的问题</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1731639023133-58a0ba85-dd19-4776-a9dc-e3a708096d5f.jpeg)

<font style="color:rgb(51, 51, 51);">  
</font>

#### <font style="color:rgb(39, 56, 73);">首次白屏的问题</font>
<font style="color:rgb(51, 51, 51);">那这时候实际就只剩下了白屏、预加载和应用保活的问题</font>

<font style="color:rgb(51, 51, 51);">白屏实际上可以分为两个场景，一是</font>**首次加载白屏**<font style="color:rgb(51, 51, 51);">，二是</font>**切换应用时白屏**

**首次白屏的问题**<font style="color:rgb(51, 51, 51);">，wujie实例可以提前实例化，包括shadowRoot、iframe的创建、js的执行，这样极大的加快子应用第一次打开的时间</font>

**切换白屏的问题**<font style="color:rgb(51, 51, 51);">，一旦wujie实例可以缓存下来，子应用的切换成本变的极低，如果采用保活模式，那么相当于shadowRoot的插拔</font>

<font style="color:rgb(51, 51, 51);">所以以上的整套机制在wujie中的实现对应的流程图如下图一样</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731639041702-8a76a5d7-fd43-425c-956c-793c28f3a1e3.png)

<font style="color:rgb(51, 51, 51);">  
</font>

<font style="color:rgb(51, 51, 51);">  
</font>

#### <font style="color:rgb(39, 56, 73);">预加载的处理</font>
<font style="color:rgb(51, 51, 51);">对于预加载的处理也非常简单，在子应用使用fiber模式执行的情况下，使用 requestIdleCallback ，在浏览器空闲时间去加载资源</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731639053533-3ba2017a-4b59-4534-9581-8277a6fcf5b6.png)

#### <font style="color:rgb(39, 56, 73);">js和css隔离</font>
<font style="color:rgb(51, 51, 51);">这个不用多言，懂的都懂 </font>

#### <font style="color:rgb(39, 56, 73);">公共依赖的处理</font>
<font style="color:rgb(51, 51, 51);">文档内写明不推荐，但是硬要的话可以在微应用中将公共依赖配置成 external，然后在主应用中导入这些公共依赖的 </font>

#### <font style="color:rgb(39, 56, 73);">路由管理</font>
<font style="color:rgb(51, 51, 51);">跟其他方案不同的是，wujie的路由管理有两个比较有意思的点</font>

**1、路由同步**

<font style="color:rgb(51, 51, 51);">会将子应用路径的path+query+hash通过window.encodeURIComponent编码后挂载在主应用url的查询参数上，其中key值为子应用的 name。</font>

<font style="color:rgb(51, 51, 51);">开启路由同步后，</font>**刷新浏览器或者将url分享出去子应用的路由状态都不会丢失**<font style="color:rgb(51, 51, 51);">，当一个页面存在多个子应用时无界支持所有子应用路由同步，浏览器刷新、前进、后退子应用路由状态也都不会丢失，并且</font>**提供短路径的能力**<font style="color:rgb(51, 51, 51);">，当子应用的url过长时，可以通过配置 prefix 来缩短子应用同步到主应用的路径，无界在选取短路径的时候，按照匹配最长路径原则选取短路径。</font>

**2、路由跳转**

<font style="color:rgb(51, 51, 51);">无界支持子应用间的路由的跳转，例如子应用a可以跳转子应用b，也可以从子应用a跳转到子应用b的指定路由中，这点在其他方案中暂时还没看到有。</font>

<font style="color:rgb(51, 51, 51);">并且如果子应用a要跳转到子应用b是保活应用，并且已经进行过初始化了，那也有对应的方案使子应用b的状态不会因为跳转而丢失</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1731639584740-11823062-531d-4fe8-b5b5-51296c25da21.jpeg)

#### <font style="color:rgb(39, 56, 73);">降级处理</font>
<font style="color:rgb(51, 51, 51);">wujie 和 micro-app 的发布时间其实相距不远，并且两者都用到了webComponent的能力，但是wujie 对于webComponent 特性不支持的情况做了无感知的降级方案，但是micro-app没有</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1731639113853-21e9d96a-55ed-4874-b044-4aff9a700c58.jpeg)

