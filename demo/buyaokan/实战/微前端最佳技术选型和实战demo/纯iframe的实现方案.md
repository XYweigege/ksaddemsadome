### <font style="color:rgb(39, 56, 73);">纯iframe的实现方案优点和缺点</font>
<font style="color:rgb(51, 51, 51);">根据我们第二部分的思考，其实iframe的方案在一些地方有着天然的优势，例如 iframe 的隔离完美，无论是 js、css、dom 都完全隔离开来，子应用间的嵌套毫无压力，随便套，并且使用简单，没有任何心智负担，天然支持html entry </font>

<font style="color:rgb(51, 51, 51);">但是</font>**缺点**<font style="color:rgb(51, 51, 51);">也非常明显：</font>

1. <font style="color:rgb(51, 51, 51);">⽆论是使⽤postMessage还是通过 iframeEl.contentWindow 去获取 iFrame 元素的 Window 对象，⼜或者是直接⽗⻚⾯调⽤⼦⻚⾯⽅法：FrameName.window.childMethod();⼦⻚⾯调⽤⽗⻚⾯⽅法：parent.window.parentMethod();来通讯都并不是太过友好的事情，需要设计⼀套规范的通讯标准，实在过于麻烦，并且状态管理、公共依赖的处理等都能通过其他的方式封装实现</font>
2. <font style="color:rgb(51, 51, 51);">路由状态丢失，刷新一下，iframe 的 url 状态就丢失了</font>
3. <font style="color:rgb(51, 51, 51);">dom 割裂严重，弹窗只能在 iframe 内部展示，无法覆盖全局，并且事件传递上存在者很大的问题，例如拖拽</font>
4. <font style="color:rgb(51, 51, 51);">白屏时间太长，对于SPA 应用应用来说无法接受</font>
5. <font style="color:rgb(51, 51, 51);">难以做预加载</font>
6. <font style="color:rgb(51, 51, 51);">应用完全没办法保活，每次都是新的加载</font>
7. <font style="color:rgb(51, 51, 51);">iframe和主⻚⾯共享连接池，⽽浏览器对相同域的连接有限制，所以会影响⻚⾯的并⾏加载，出现iframe中的资源占⽤了可⽤连接⽽阻塞了主⻚⾯的资源加载</font>

<font style="color:rgb(51, 51, 51);">所以这很难说得上是一个比较好的实现方案，但是后续我们会讲到wujie是怎么解决iframe的缺点，打造一个接近完美的iframe方案的。</font>

