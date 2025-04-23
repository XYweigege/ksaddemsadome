## 一、Vue.js 运行机制全局概览
<font style="color:rgb(44, 62, 80);">这一节笔者将为大家介绍一下 Vue.js 内部的整个流程，希望能让大家对全局有一个整体的印象，然后我们再来逐个模块进行讲解。从来没有了解过 Vue.js 实现的同学可能会对一些内容感到疑惑，这是很正常的，这一节的目的主要是为了让大家对整个流程有一个大概的认识，算是一个概览预备的过程，当把整本小册认真读完以后，再来阅读这一节，相信会有收获的。</font>

<font style="color:rgb(44, 62, 80);">首先我们来看一下笔者画的内部流程图</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718862901525-022bf4cc-4a8b-4087-9a55-66b3ea8b9f84.png)

<font style="color:rgb(44, 62, 80);">大家第一次看到这个图一定是一头雾水的，没有关系，我们来逐个讲一下这些模块的作用以及调用关系。相信讲完之后大家对Vue.js内部运行机制会有一个大概的认识。</font>

## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/a-overview.html#%E5%88%9D%E5%A7%8B%E5%8C%96%E5%8F%8A%E6%8C%82%E8%BD%BD)<font style="color:rgb(44, 62, 80);">初始化及挂载</font>
<font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">new Vue()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之后。 Vue 会调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">_init</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数进行初始化，也就是这里的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">init</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">过程，它会初始化生命周期、事件、 props、 methods、 data、 computed 与 watch 等。其中最重要的是通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">Object.defineProperty</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">设置</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">setter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">getter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，用来实现「响应式」以及「依赖收集」，后面会详细讲到，这里只要有一个印象即可。</font>

<font style="color:rgb(44, 62, 80);">初始化之后调用 $mount 会挂载组件，如果是运行时编译，即不存在 render function 但是存在 template 的情况，需要进行「编译」步骤。</font>

## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/a-overview.html#%E7%BC%96%E8%AF%91)<font style="color:rgb(44, 62, 80);">编译</font>
<font style="color:rgb(44, 62, 80);">compile编译可以分成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">parse</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(71, 101, 130);">optimize</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">generate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">三个阶段，最终需要得到 render function。</font>

### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/a-overview.html#parse)<font style="color:rgb(44, 62, 80);">parse</font>
<font style="color:rgb(71, 101, 130);">parse</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会用正则等方式解析 template 模板中的指令、class、style等数据，形成AST。</font>

### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/a-overview.html#optimize)<font style="color:rgb(44, 62, 80);">optimize</font>
<font style="color:rgb(71, 101, 130);">optimize</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的主要作用是标记 static 静态节点，这是 Vue 在编译过程中的一处优化，后面当</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">update</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">更新界面时，会有一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">patch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的过程， diff 算法会直接跳过静态节点，从而减少了比较的过程，优化了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">patch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的性能。</font>

### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/a-overview.html#generate)<font style="color:rgb(44, 62, 80);">generate</font>
<font style="color:rgb(71, 101, 130);">generate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是将 AST 转化成 render function 字符串的过程，得到结果是 render 的字符串以及 staticRenderFns 字符串。</font>

<font style="color:rgb(44, 62, 80);">在经历过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">parse</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(71, 101, 130);">optimize</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">generate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这三个阶段以后，组件中就会存在渲染 VNode 所需的 render function 了。</font>

## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/a-overview.html#%E5%93%8D%E5%BA%94%E5%BC%8F)<font style="color:rgb(44, 62, 80);">响应式</font>
<font style="color:rgb(44, 62, 80);">接下来也就是 Vue.js 响应式核心部分。</font><font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718862901799-5aa7c488-9f71-48b0-9812-b1980f6ca4ff.png)

<font style="color:rgb(44, 62, 80);">这里的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">getter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">跟</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">setter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">已经在之前介绍过了，在 init 的时候通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">Object.defineProperty</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">进行了绑定，它使得当被设置的对象被读取的时候会执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">getter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，而在当被赋值的时候会执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">setter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数。</font>

<font style="color:rgb(44, 62, 80);">当 render function 被渲染的时候，因为会读取所需对象的值，所以会触发 getter 函数进行「依赖收集」，「依赖收集」的目的是将观察者 Watcher 对象存放到当前闭包中的订阅者 Dep 的 subs 中。形成如下所示的这样一个关系。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1718862900538-d4cf6cd7-fba7-4239-8130-280b1050cffc.jpeg)

<font style="color:rgb(44, 62, 80);">在修改对象的值的时候，会触发对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">setter</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">setter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">通知之前</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">「依赖收集」</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">得到的 Dep 中的每一个 Watcher，告诉它们自己的值改变了，需要重新渲染视图。这时候这些 Watcher 就会开始调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">update</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来更新视图，当然这中间还有一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">patch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的过程以及使用队列来异步更新的策略，这个我们后面再讲。</font>

## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/a-overview.html#virtual-dom)<font style="color:rgb(44, 62, 80);">Virtual DOM</font>
<font style="color:rgb(44, 62, 80);">我们知道，render function 会被转化成 VNode 节点。Virtual DOM 其实就是一棵以 JavaScript 对象（ VNode 节点）作为基础的树，用对象属性来描述节点，实际上它只是一层对真实 DOM 的抽象。最终可以通过一系列操作使这棵树映射到真实环境上。由于 Virtual DOM 是以 JavaScript 对象为基础而不依赖真实平台环境，所以使它具有了跨平台的能力，比如说浏览器平台、Weex、Node 等。</font>

<font style="color:rgb(44, 62, 80);">比如说下面这样一个例子：</font>

```javascript
{
  tag: 'div',                 /*说明这是一个div标签*/
    children: [                 /*存放该标签的子节点*/
    {
      tag: 'a',           /*说明这是一个a标签*/
      text: 'click me'    /*标签的内容*/
    }
  ]
}
```



<font style="color:rgb(44, 62, 80);">渲染后可以得到</font>

```javascript
<div>
  <a>click me</a>
  </div>
```



<font style="color:rgb(44, 62, 80);">这只是一个简单的例子，实际上的节点有更多的属性来标志节点，比如 isStatic （代表是否为静态节点）、 isComment （代表是否为注释节点）等。</font>

## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/a-overview.html#%E6%9B%B4%E6%96%B0%E8%A7%86%E5%9B%BE)<font style="color:rgb(44, 62, 80);">更新视图</font>
![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1718862900535-1a223335-cb7c-4d86-a8cc-7aa93bde6fa6.jpeg)

<font style="color:rgb(44, 62, 80);">前面我们说到，在修改一个对象值的时候，会通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">setter -> Watcher -> update</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的流程来修改对应的视图，那么最终是如何更新视图的呢？</font>

<font style="color:rgb(44, 62, 80);">当数据变化后，执行 render function 就可以得到一个新的 VNode 节点，我们如果想要得到新的视图，最简单粗暴的方法就是直接解析这个新的 VNode 节点，然后用 innerHTML 直接全部渲染到真实 DOM 中。但是其实我们只对其中的一小块内容进行了修改，这样做似乎有些「浪费」。</font>

<font style="color:rgb(44, 62, 80);">那么我们为什么不能只修改那些 </font>**<font style="color:rgb(44, 62, 80);">「改变了的地方」</font>**<font style="color:rgb(44, 62, 80);"> 呢？这个时候就要介绍我们的「</font><font style="color:rgb(71, 101, 130);">patch</font><font style="color:rgb(44, 62, 80);">」了。我们会将新的 VNode 与旧的 VNode 一起传入 patch 进行比较，经过 diff 算法得出它们的 </font>**<font style="color:rgb(44, 62, 80);">「差异」</font>**<font style="color:rgb(44, 62, 80);"> 。最后我们只需要将这些 </font>**<font style="color:rgb(44, 62, 80);">「差异」</font>**<font style="color:rgb(44, 62, 80);"> 的对应 DOM 进行修改即可。</font>

## 二、响应式系统的基本原理
### [](https://www.123fe.net/principle-docs/vue/12-%E5%89%96%E6%9E%90%20Vue%20%E5%86%85%E9%83%A8%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6.html#%E5%93%8D%E5%BA%94%E5%BC%8F%E7%B3%BB%E7%BB%9F)<font style="color:rgb(44, 62, 80);">响应式系统</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.js</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是一款 MVVM 框架，数据模型仅仅是普通的 JavaScript 对象，但是对这些对象进行操作时，却能影响对应视图，它的核心实现就是「</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">响应式系统</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">」。尽管我们在使用 Vue.js 进行开发时不会直接修改「</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">响应式系统</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">」，但是理解它的实现有助于避开一些常见的「</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">坑</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">」，也有助于在遇见一些琢磨不透的问题时可以深入其原理来解决它。</font>

### [](https://www.123fe.net/principle-docs/vue/12-%E5%89%96%E6%9E%90%20Vue%20%E5%86%85%E9%83%A8%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6.html#object-defineproperty)<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font>
<font style="color:rgb(44, 62, 80);">首先我们来介绍一下</font><font style="color:rgb(44, 62, 80);"> </font>[Object.defineProperty(opens new window)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)<font style="color:rgb(44, 62, 80);">，Vue.js就是基于它实现「</font>**<font style="color:rgb(44, 62, 80);">响应式系统</font>**<font style="color:rgb(44, 62, 80);">」的。</font>

<font style="color:rgb(44, 62, 80);">首先是使用方法：</font>

```javascript
/*
    obj: 目标对象
    prop: 需要操作的目标对象的属性名
    descriptor: 描述符
    
    return value 传入对象
*/
Object.defineProperty(obj, prop, descriptor)
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">descriptor的一些属性，简单介绍几个属性，具体可以参考</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font>[MDN 文档(opens new window)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">enumerable</font><font style="color:rgb(44, 62, 80);">，属性是否可枚举，默认 false。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">configurable</font><font style="color:rgb(44, 62, 80);">，属性是否可以被修改或者删除，默认 false。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);">，获取属性的方法。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">set</font><font style="color:rgb(44, 62, 80);">，设置属性的方法。</font>

### [](https://www.123fe.net/principle-docs/vue/12-%E5%89%96%E6%9E%90%20Vue%20%E5%86%85%E9%83%A8%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6.html#%E5%AE%9E%E7%8E%B0-observer-%E5%8F%AF%E8%A7%82%E5%AF%9F%E7%9A%84)<font style="color:rgb(44, 62, 80);">实现</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">observer</font><font style="color:rgb(44, 62, 80);">（可观察的）</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">知道了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以后，我们来用它使对象变成可观察的。</font>

<font style="color:rgb(44, 62, 80);">这一部分的内容我们在第二小节中已经初步介绍过，在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">init</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的阶段会进行初始化，对数据进行「</font>**<font style="color:rgb(44, 62, 80);">响应式化</font>**<font style="color:rgb(44, 62, 80);">」。</font>

+ <font style="color:rgb(44, 62, 80);">为了便于理解，我们不考虑数组等复杂的情况，只对对象进行处理。</font>
+ <font style="color:rgb(44, 62, 80);">首先我们定义一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cb</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，这个函数用来模拟视图更新，调用它即代表更新视图，内部可以是一些更新视图的方法。</font>

```javascript
function cb (val) {
  /* 渲染视图 */
  console.log("视图更新啦～");
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">然后我们定义一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defineReactive</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，这个方法通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来实现对对象的「</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">响应式</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">」化，入参是一个 obj（需要绑定的对象）、key（obj的某一个属性），val（具体的值）。经过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defineReactive</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">处理以后，我们的 obj 的 key 属性在「读」的时候会触发</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactiveGetter</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法，而在该属性被「写」的时候则会触发</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactiveSetter</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法。</font>

```javascript
function defineReactive (obj, key, val) {
  Object.defineProperty(obj, key, {
    enumerable: true,       /* 属性可枚举 */
    configurable: true,     /* 属性可被修改或删除 */
    get: function reactiveGetter () {
      return val;         /* 实际上会依赖收集，下一小节会讲 */
    },
    set: function reactiveSetter (newVal) {
      if (newVal === val) return;
      cb(newVal);
    }
  });
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当然这是不够的，我们需要在上面再封装一层</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">observer</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。这个函数传入一个 value（需要「</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">响应式</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">」化的对象），通过遍历所有属性的方式对该对象的每一个属性都通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defineReactive</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">处理。（注：实际上 observer 会进行递归调用，为了便于理解去掉了递归的过程）</font>

```javascript
function observer (value) {
  if (!value || (typeof value !== 'object')) {
    return;
  }

  Object.keys(value).forEach((key) => {
    defineReactive(value, key, value[key]);
  });
}
```

+ <font style="color:rgb(44, 62, 80);">最后，让我们用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">observer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来封装一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">吧！</font>
+ <font style="color:rgb(44, 62, 80);">在 Vue 的构造函数中，对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">options</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">进行处理，这里的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">想必大家很熟悉，就是平时我们在写 Vue 项目时组件中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性（实际上是一个函数，这里当作一个对象来简单处理）。</font>

```javascript
class Vue {
  /* Vue构造类 */
  constructor(options) {
    this._data = options.data;
    observer(this._data);
  }
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这样我们只要 new 一个 Vue 对象，就会将</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中的数据进行「</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">响应式</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">」化。如果我们对</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的属性进行下面的操作，就会触发</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cb</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法更新视图。</font>

```javascript
let o = new Vue({
  data: {
    test: "I am test."
  }
});
o._data.test = "hello,world.";  /* 视图更新啦～ */
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">至此，响应式原理已经介绍完了，接下来让我们学习「</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">响应式系统</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">」的另一部分 ——「</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">依赖收集</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">」</font>

## 三、响应式系统的依赖收集追踪原理
<font style="color:rgb(44, 62, 80);">为什么要依赖收集？</font>

**<font style="color:rgb(44, 62, 80);">先举个栗子</font>****<font style="color:rgb(44, 62, 80);">🌰</font>**

<font style="color:rgb(44, 62, 80);">我们现在有这么一个 Vue 对象。</font>

```vue
new Vue({
  template: 
    `<div>
            <span>{{text1}}</span> 
            <span>{{text2}}</span> 
        <div>`,
  data: {
    text1: 'text1',
    text2: 'text2',
    text3: 'text3'
  }
});
```

<font style="color:rgb(44, 62, 80);">然后我们做了这么一个操作。</font>

```javascript
this.text3 = 'modify text3';
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们修改了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text3</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的数据，但是因为视图中并不需要用到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text3</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，所以我们并不需要触发上一章所讲的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cb</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数来更新视图，调用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cb</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">显然是不正确的。</font>

**<font style="color:rgb(44, 62, 80);">再来一个栗子</font>****<font style="color:rgb(44, 62, 80);">🌰</font>**

<font style="color:rgb(44, 62, 80);">假设我们现在有一个全局的对象，我们可能会在多个 Vue 对象中用到它进行展示。</font>

```javascript
let globalObj = {
  text1: 'text1'
};

let o1 = new Vue({
  template:
    `<div>
            <span>{{text1}}</span> 
        <div>`,
  data: globalObj
});

let o2 = new Vue({
  template:
    `<div>
            <span>{{text1}}</span> 
        <div>`,
  data: globalObj
});
```

<font style="color:rgb(44, 62, 80);">这个时候，我们执行了如下操作。</font>

```javascript
globalObj.text1 = 'hello,text1';
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们应该需要通知</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">o1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以及</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">o2</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">两个vm实例进行视图的更新，「依赖收集」会让</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个数据知道“哦～有两个地方依赖我的数据，我变化的时候需要通知它们～”。</font>

<font style="color:rgb(44, 62, 80);">最终会形成数据与视图的一种对应关系，如下图。</font>

<font style="color:rgb(44, 62, 80);">接下来我们来介绍一下「依赖收集」是如何实现的。</font>

### [](https://www.123fe.net/principle-docs/vue/12-%E5%89%96%E6%9E%90%20Vue%20%E5%86%85%E9%83%A8%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6.html#%E8%AE%A2%E9%98%85%E8%80%85-dep)<font style="color:rgb(44, 62, 80);">订阅者 Dep</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">首先我们来实现一个订阅者</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dep</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，它的主要作用是用来存放</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">观察者对象。</font>

```javascript
class Dep {
  constructor () {
    /* 用来存放Watcher对象的数组 */
    this.subs = [];
  }

  /* 在subs中添加一个Watcher对象 */
  addSub (sub) {
    this.subs.push(sub);
  }

  /* 通知所有Watcher对象更新视图 */
  notify () {
    this.subs.forEach((sub) => {
      sub.update();
    })
  }
}
```

**<font style="color:rgb(44, 62, 80);">为了便于理解我们只实现了添加的部分代码，主要是两件事情：</font>**

1. <font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">addSub</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法可以在目前的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dep</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象中增加一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的订阅操作；</font>
2. <font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">notify</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法通知目前</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dep</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">subs</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的所有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象触发更新操作。</font>

### [](https://www.123fe.net/principle-docs/vue/12-%E5%89%96%E6%9E%90%20Vue%20%E5%86%85%E9%83%A8%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6.html#%E8%A7%82%E5%AF%9F%E8%80%85-watcher)<font style="color:rgb(44, 62, 80);">观察者 Watcher</font>
```javascript
class Watcher {
  constructor () {
    /* 在new一个Watcher对象时将该对象赋值给Dep.target，在get中会用到 */
    Dep.target = this;
  }

  /* 更新视图的方法 */
  update () {
    console.log("视图更新啦～");
  }
}

Dep.target = null;
```

### [](https://www.123fe.net/principle-docs/vue/12-%E5%89%96%E6%9E%90%20Vue%20%E5%86%85%E9%83%A8%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6.html#%E4%BE%9D%E8%B5%96%E6%94%B6%E9%9B%86)<font style="color:rgb(44, 62, 80);">依赖收集</font>
<font style="color:rgb(44, 62, 80);">接下来我们修改一下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defineReactive</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以及 Vue 的构造函数，来完成依赖收集。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们在闭包中增加了一个 Dep 类的对象，用来收集</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象。在对象被「读」的时候，会触发</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactiveGetter</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数把当前的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象（存放在 Dep.target 中）收集到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dep</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">类中去。之后如果当该对象被「</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">写</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">」的时候，则会触发</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactiveSetter</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法，通知</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dep</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">类调用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">notify</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来触发所有</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">update</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法更新对应视图。</font>

```javascript
function defineReactive (obj, key, val) {
  /* 一个Dep类对象 */
  const dep = new Dep();

  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function reactiveGetter () {
      /* 将Dep.target（即当前的Watcher对象存入dep的subs中） */
      dep.addSub(Dep.target);
      return val;         
    },
    set: function reactiveSetter (newVal) {
      if (newVal === val) return;
      /* 在set的时候触发dep的notify来通知所有的Watcher对象更新视图 */
      dep.notify();
    }
  });
}

class Vue {
  constructor(options) {
    this._data = options.data;
    observer(this._data);
    /* 新建一个Watcher观察者对象，这时候Dep.target会指向这个Watcher对象 */
    new Watcher();
    /* 在这里模拟render的过程，为了触发test属性的get函数 */
    console.log('render~', this._data.test);
  }
}
```

### [](https://www.123fe.net/principle-docs/vue/12-%E5%89%96%E6%9E%90%20Vue%20%E5%86%85%E9%83%A8%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6.html#%E5%B0%8F%E7%BB%93)<font style="color:rgb(44, 62, 80);">小结</font>

<font style="color:rgb(85, 85, 85);">首先在 </font><font style="color:rgb(199, 37, 78);">observer</font><font style="color:rgb(85, 85, 85);"> 的过程中会注册 </font><font style="color:rgb(199, 37, 78);">get</font><font style="color:rgb(85, 85, 85);"> 方法，该方法用来进行「</font>**<font style="color:rgb(85, 85, 85);">依赖收集</font>**<font style="color:rgb(85, 85, 85);">」。在它的闭包中会有一个 </font><font style="color:rgb(199, 37, 78);">Dep</font><font style="color:rgb(85, 85, 85);"> 对象，这个对象用来存放 Watcher 对象的实例。其实「</font>**<font style="color:rgb(85, 85, 85);">依赖收集</font>**<font style="color:rgb(85, 85, 85);">」的过程就是把 </font><font style="color:rgb(199, 37, 78);">Watcher</font><font style="color:rgb(85, 85, 85);"> 实例存放到对应的 </font><font style="color:rgb(199, 37, 78);">Dep</font><font style="color:rgb(85, 85, 85);"> 对象中去。</font><font style="color:rgb(199, 37, 78);">get</font><font style="color:rgb(85, 85, 85);"> 方法可以让当前的 </font><font style="color:rgb(199, 37, 78);">Watcher</font><font style="color:rgb(85, 85, 85);"> 对象（Dep.target）存放到它的 subs 中（</font><font style="color:rgb(199, 37, 78);">addSub</font><font style="color:rgb(85, 85, 85);">）方法，在数据变化时，</font><font style="color:rgb(199, 37, 78);">set</font><font style="color:rgb(85, 85, 85);"> 会调用 </font><font style="color:rgb(199, 37, 78);">Dep</font><font style="color:rgb(85, 85, 85);"> 对象的 </font><font style="color:rgb(199, 37, 78);">notify</font><font style="color:rgb(85, 85, 85);"> 方法通知它内部所有的 </font><font style="color:rgb(199, 37, 78);">Watcher</font><font style="color:rgb(85, 85, 85);"> 对象进行视图更新。</font>

<br/>

<font style="color:rgb(44, 62, 80);">这是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">set/get</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法处理的事情，那么「</font>**<font style="color:rgb(44, 62, 80);">依赖收集</font>**<font style="color:rgb(44, 62, 80);">」的前提条件还有两个：</font>

1. <font style="color:rgb(44, 62, 80);">触发</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法；</font>
2. <font style="color:rgb(44, 62, 80);">新建一个 Watcher 对象。</font>


<font style="color:rgb(85, 85, 85);">这个我们在 Vue 的构造类中处理。新建一个 </font><font style="color:rgb(199, 37, 78);">Watcher</font><font style="color:rgb(85, 85, 85);"> 对象只需要 new 出来，这时候 </font><font style="color:rgb(199, 37, 78);">Dep.target</font><font style="color:rgb(85, 85, 85);"> 已经指向了这个 new 出来的 </font><font style="color:rgb(199, 37, 78);">Watcher</font><font style="color:rgb(85, 85, 85);"> 对象来。而触发 </font><font style="color:rgb(199, 37, 78);">get</font><font style="color:rgb(85, 85, 85);"> 方法也很简单，实际上只要把 render function 进行渲染，那么其中的依赖的对象都会被「读取」，这里我们通过打印来模拟这个过程，读取 test 来触发 </font><font style="color:rgb(199, 37, 78);">get</font><font style="color:rgb(85, 85, 85);"> 进行「依赖收集」。</font>

<br/>

<font style="color:rgb(44, 62, 80);">本章我们介绍了「依赖收集」的过程，配合之前的响应式原理，已经把整个「响应式系统」介绍完毕了。其主要就是 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);"> 进行「依赖收集」。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">set</font><font style="color:rgb(44, 62, 80);"> 通过观察者来更新视图，配合下图仔细捋一捋，相信一定能搞懂它！</font>

## 四、实现 Virtual DOM 下的一个 VNode 节点
### <font style="color:rgb(44, 62, 80);">什么是VNode</font>

<font style="color:rgb(85, 85, 85);">我们知道，render function 会被转化成 VNode 节点。Virtual DOM 其实就是一棵以 JavaScript 对象（VNode 节点）作为基础的树，用对象属性来描述节点，实际上它只是一层对真实 DOM 的抽象。最终可以通过一系列操作使这棵树映射到真实环境上。由于 Virtual DOM 是以 JavaScript 对象为基础而不依赖真实平台环境，所以使它具有了跨平台的能力，比如说浏览器平台、Weex、Node 等。</font>

<br/>

### [](https://www.123fe.net/principle-docs/vue/12-%E5%89%96%E6%9E%90%20Vue%20%E5%86%85%E9%83%A8%E8%BF%90%E8%A1%8C%E6%9C%BA%E5%88%B6.html#%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AAvnode)<font style="color:rgb(44, 62, 80);">实现一个VNode</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">归根结底就是一个 JavaScript 对象，只要这个类的一些属性可以正确直观地描述清楚当前节点的信息即可。我们来实现一个简单的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">类，加入一些基本属性，为了便于理解，我们先不考虑复杂的情况。</font>

```javascript
class VNode {
  constructor (tag, data, children, text, elm) {
    /*当前节点的标签名*/
    this.tag = tag;
    /*当前节点的一些数据信息，比如props、attrs等数据*/
    this.data = data;
    /*当前节点的子节点，是一个数组*/
    this.children = children;
    /*当前节点的文本*/
    this.text = text;
    /*当前虚拟节点对应的真实dom节点*/
    this.elm = elm;
  }
}
```

<font style="color:rgb(44, 62, 80);">比如我目前有这么一个 Vue 组件。</font>

```html
<template>
  <span class="demo" v-show="isShow">
    This is a span.
  </span>
</template>
```

<font style="color:rgb(44, 62, 80);">用 JavaScript 代码形式就是这样的。</font>

```javascript
function render () {
  return new VNode(
    'span',
    {
      /* 指令集合数组 */
      directives: [
        {
          /* v-show指令 */
          rawName: 'v-show',
          expression: 'isShow',
          name: 'show',
          value: true
        }
      ],
      /* 静态class */
      staticClass: 'demo'
    },
    [ new VNode(undefined, undefined, undefined, 'This is a span.') ]
  );
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">看看转换成</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以后的情况。</font>

```javascript
{
  tag: 'span',
    data: {
    /* 指令集合数组 */
    directives: [
      {
        /* v-show指令 */
        rawName: 'v-show',
        expression: 'isShow',
        name: 'show',
        value: true
      }
    ],
      /* 静态class */
      staticClass: 'demo'
  },
  text: undefined,
    children: [
    /* 子节点是一个文本VNode节点 */
    {
      tag: undefined,
      data: undefined,
      text: 'This is a span.',
      children: undefined
    }
  ]
}
```

<font style="color:rgb(44, 62, 80);">然后我们可以将 VNode 进一步封装一下，可以实现一些产生常用 VNode 的方法。</font>

+ <font style="color:rgb(44, 62, 80);">创建一个空节点</font>

```javascript
function createEmptyVNode () {
  const node = new VNode();
  node.text = '';
  return node;
}
```

+ <font style="color:rgb(44, 62, 80);">创建一个文本节点</font>

```javascript
function createTextVNode (val) {
  return new VNode(undefined, undefined, undefined, String(val));
}
```

+ <font style="color:rgb(44, 62, 80);">克隆一个 VNode 节点</font>

```javascript
function cloneVNode (node) {
  const cloneVnode = new VNode(
    node.tag,
    node.data,
    node.children,
    node.text,
    node.elm
  );
  return cloneVnode;
}
```


<font style="color:rgb(85, 85, 85);">总的来说，</font><font style="color:rgb(199, 37, 78);">VNode</font><font style="color:rgb(85, 85, 85);"> 就是一个 JavaScript 对象，用 JavaScript 对象的属性来描述当前节点的一些状态，用 VNode 节点的形式来模拟一棵 </font><font style="color:rgb(199, 37, 78);">Virtual DOM</font><font style="color:rgb(85, 85, 85);"> 树。</font>

<br/>

## 五、template 模板是怎样通过 Compile 编译的
## <font style="color:rgb(44, 62, 80);">compile</font>
<font style="color:rgb(44, 62, 80);">compile 编译可以分成 parse、optimize 与 generate 三个阶段，最终需要得到 render function。这部分内容不算 Vue.js 的响应式核心，只是用来编译的，笔者认为在精力有限的情况下不需要追究其全部的实现细节，能够把握如何解析的大致流程即可。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718868774116-66a48f32-b84d-4047-bd71-27d8e5d8a284.png)

<font style="color:rgb(44, 62, 80);">由于解析过程比较复杂，直接上代码可能会导致不了解这部分内容的同学一头雾水。所以准备提供一个 template 的示例，通过这个示例的变化来看解析的过程。但是解析的过程及结果都是将最重要的部分抽离出来展示，希望能让读者更好地了解其核心部分的实现。</font>

```html
<div :class="c" class="demo" v-if="isShow">
    <span v-for="item in sz">{{item}}</span>
</div>
```



```javascript
var html = '<div :class="c" class="demo" v-if="isShow"><span v-for="item in sz">{{item}}</span></div>';
```

<font style="color:rgba(255, 255, 255, 0.3);background-color:rgb(40, 44, 52);">1</font>

<font style="color:rgb(44, 62, 80);">接下来的过程都会依赖这个示例来进行。</font>

## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#parse)<font style="color:rgb(44, 62, 80);">parse</font>
<font style="color:rgb(44, 62, 80);">首先是 parse，parse 会用正则等方式将 template 模板中进行字符串解析，得到指令、class、style等数据，形成 AST（在计算机科学中，抽象语法树（abstract syntax tree或者缩写为AST），或者语法树（syntax tree），是源代码的抽象语法结构的树状表现形式，这里特指编程语言的源代码。）。</font>

<font style="color:rgb(44, 62, 80);">这个过程比较复杂，会涉及到比较多的正则进行字符串解析，我们来看一下得到的 AST 的样子。</font>

```javascript
{
  /* 标签属性的map，记录了标签上属性 */
  'attrsMap': {
    ':class': 'c',
      'class': 'demo',
      'v-if': 'isShow'
  },
  /* 解析得到的:class */
  'classBinding': 'c',
    /* 标签属性v-if */
    'if': 'isShow',
    /* v-if的条件 */
    'ifConditions': [
    {
      'exp': 'isShow'
    }
  ],
    /* 标签属性class */
    'staticClass': 'demo',
    /* 标签的tag */
    'tag': 'div',
    /* 子标签数组 */
    'children': [
    {
      'attrsMap': {
        'v-for': "item in sz"
      },
      /* for循环的参数 */
      'alias': "item",
      /* for循环的对象 */
      'for': 'sz',
      /* for循环是否已经被处理的标记位 */
      'forProcessed': true,
      'tag': 'span',
      'children': [
        {
          /* 表达式，_s是一个转字符串的函数 */
          'expression': '_s(item)',
          'text': '{{item}}'
        }
      ]
    }
  ]
}
```



<font style="color:rgb(44, 62, 80);">最终得到的 AST 通过一些特定的属性，能够比较清晰地描述出标签的属性以及依赖关系。</font>

<font style="color:rgb(44, 62, 80);">接下来我们用代码来讲解一下如何使用正则来把 template 编译成我们需要的 AST 的。</font>

[**接下来所涉及的源码->**](https://github.com/vuejs/vue/blob/dev/src/compiler/parser/html-parser.js)

## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#%E6%AD%A3%E5%88%99)<font style="color:rgb(44, 62, 80);">正则</font>
<font style="color:rgb(44, 62, 80);">首先我们定义一下接下来我们会用到的正则。</font>

```javascript
const ncname = '[a-zA-Z_][\\w\\-\\.]*';
const singleAttrIdentifier = /([^\s"'<>/=]+)/
const singleAttrAssign = /(?:=)/
const singleAttrValues = [
  /"([^"]*)"+/.source,
  /'([^']*)'+/.source,
  /([^\s"'=<>`]+)/.source
]
const attribute = new RegExp(
  '^\\s*' + singleAttrIdentifier.source +
  '(?:\\s*(' + singleAttrAssign.source + ')' +
  '\\s*(?:' + singleAttrValues.join('|') + '))?'
)

const qnameCapture = '((?:' + ncname + '\\:)?' + ncname + ')'
const startTagOpen = new RegExp('^<' + qnameCapture)
const startTagClose = /^\s*(\/?)>/

const endTag = new RegExp('^<\\/' + qnameCapture + '[^>]*>')

const defaultTagRE = /\{\{((?:.|\n)+?)\}\}/g

const forAliasRE = /(.*?)\s+(?:in|of)\s+(.*)/
```



## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#advance)<font style="color:rgb(44, 62, 80);">Advance</font>
<font style="color:rgb(44, 62, 80);">因为我们解析 template 采用循环进行字符串匹配的方式，所以每匹配解析完一段我们需要将已经匹配掉的去掉，头部的指针指向接下来需要匹配的部分。</font>

```plain
function advance (n) {
    index += n
    html = html.substring(n)
}
```



## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#parsehtml)<font style="color:rgb(44, 62, 80);">parseHTML</font>
<font style="color:rgb(44, 62, 80);">首先我们需要定义个 parseHTML 函数，在里面我们循环解析 template 字符串。</font>

```javascript
function parseHTML () {
  while(html) {
    let textEnd = html.indexOf('<');
    if (textEnd === 0) {
      if (html.match(endTag)) {
        //...process end tag
        continue;
      }
      if (html.match(startTagOpen)) {
        //...process start tag
        continue;
      }
    } else {
      //...process text
      continue;
    }
  }
}
```



<font style="color:rgb(71, 101, 130);">parseHTML</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">while</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来循环解析</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">template</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，用正则在匹配到标签头、标签尾以及文本的时候分别进行不同的处理。直到整个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">template</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">被解析完毕。</font>

## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#parsestarttag)<font style="color:rgb(44, 62, 80);">parseStartTag</font>
<font style="color:rgb(44, 62, 80);">我们来写一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">parseStartTag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，用来解析起始标签(</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">"<div :class="c" class="demo" v-if="isShow">"</font><font style="color:rgb(44, 62, 80);">部分的内容)。</font>

```javascript
function parseStartTag () {
  const start = html.match(startTagOpen);
  if (start) {
    const match = {
      tagName: start[1],
      attrs: [],
      start: index
    }
    advance(start[0].length);

    let end, attr
    while (!(end = html.match(startTagClose)) && (attr = html.match(attribute))) {
      advance(attr[0].length)
      match.attrs.push({
        name: attr[1],
        value: attr[3]
      });
    }
    if (end) {
      match.unarySlash = end[1];
      advance(end[0].length);
      match.end = index;
      return match
    }
  }
}
```



<font style="color:rgb(44, 62, 80);">首先用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">startTagOpen</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">正则得到标签的头部，可以得到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">tagName</font><font style="color:rgb(44, 62, 80);">（标签名称），同时我们需要一个数组</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">attrs</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">用来存放标签内的属性。 接下来使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">startTagClose</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">attribute</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">两个正则分别用来解析标签结束以及标签内的属性。这段代码用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">while</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">循环一直到匹配到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">startTagClose</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为止，解析内部所有的属性。</font>

## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#stack)<font style="color:rgb(44, 62, 80);">stack</font>
<font style="color:rgb(44, 62, 80);">此外，我们需要维护一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">stack</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">栈来保存已经解析好的标签头，这样我们可以根据在解析尾部标签的时候得到所属的层级关系以及父标签。同时我们定义一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">currentParent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">变量用来存放当前标签的父标签节点的引用，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">root</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">变量用来指向根标签节点。</font>

```javascript
const stack = [];
let currentParent, root;
```



<font style="color:rgb(44, 62, 80);">知道这个以后，我们优化一下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">parseHTML</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">startTagOpen</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">if</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">逻辑中加上新的处理。</font>

```javascript
if (html.match(startTagOpen)) {
  const startTagMatch = parseStartTag();
  const element = {
    type: 1,
    tag: startTagMatch.tagName,
    lowerCasedTag: startTagMatch.tagName.toLowerCase(),
    attrsList: startTagMatch.attrs,
    attrsMap: makeAttrsMap(startTagMatch.attrs),
    parent: currentParent,
    children: []
  }

  if(!root){
    root = element
  }

  if(currentParent){
    currentParent.children.push(element);
  }

  stack.push(element);
  currentParent = element;
  continue;
}
```



<font style="color:rgb(44, 62, 80);">我们将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">startTagMatch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">得到的结果首先封装成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">element</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，这个就是最终形成的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">AST</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的节点，标签节点的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">type</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为 1。 然后让</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">root</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指向根节点的引用。</font>

```javascript
if(!root){
  root = element
}
```



<font style="color:rgb(44, 62, 80);">接着我们将当前节点的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">element</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">放入父节点</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">currentParent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数组中。 最后将当前节点</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">element</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">压入</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">stack</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">栈中，并将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">currentParent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指向当前节点，因为接下去下一个解析如果还是头标签或者是文本的话，会成为当前节点的子节点，如果是尾标签的话，那么将会从栈中取出当前节点，这种情况我们接下来要讲。</font>

```javascript
stack.push(element);
currentParent = element;
continue;
```



<font style="color:rgb(44, 62, 80);">其中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">makeAttrsMap</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是将 attrs 转换成 map 格式的一个方法。</font>

```javascript
function makeAttrsMap (attrs) {
  const map = {}
  for (let i = 0, l = attrs.length; i < l; i++) {
    map[attrs[i].name] = attrs[i].value;
  }
  return map
}
```



## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#parseendtag)<font style="color:rgb(44, 62, 80);">parseEndTag</font>
<font style="color:rgb(44, 62, 80);">同样，我们在 parseHTML 中加入对尾标签的解析函数，为了匹配如</font><font style="color:rgb(71, 101, 130);">“</div>”</font><font style="color:rgb(44, 62, 80);">。</font>

```javascript
const endTagMatch = html.match(endTag)
if (endTagMatch) {
  advance(endTagMatch[0].length);
  parseEndTag(endTagMatch[1]);
  continue;
}
```



<font style="color:rgb(44, 62, 80);">用 parseEndTag 来解析尾标签，它会从 stack 栈中取出最近的跟自己标签名一致的那个元素，将 currentParent 指向那个元素，并将该元素之前的元素都从 stack 中出栈。</font>

<font style="color:rgb(44, 62, 80);">这里可能有同学会问，难道解析的尾元素不应该对应 stack 栈的最上面的一个元素才对吗？</font>

<font style="color:rgb(44, 62, 80);">其实不然，比如说可能会存在自闭合的标签，如</font><font style="color:rgb(71, 101, 130);">“<br />”</font><font style="color:rgb(44, 62, 80);">，或者是写了</font><font style="color:rgb(71, 101, 130);">“<span>”</font><font style="color:rgb(44, 62, 80);">但是没有加上</font><font style="color:rgb(71, 101, 130);">“< /span>”</font><font style="color:rgb(44, 62, 80);">的情况，这时候就要找到 stack 中的第二个位置才能找到同名标签。</font>

```javascript
function parseEndTag (tagName) {
  let pos;
  for (pos = stack.length - 1; pos >= 0; pos--) {
    if (stack[pos].lowerCasedTag === tagName.toLowerCase()) {
      break;
    }
  }

  if (pos >= 0) {
    stack.length = pos;
    currentParent = stack[pos];
  }
}
```



## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#parsetext)<font style="color:rgb(44, 62, 80);">parseText</font>
<font style="color:rgb(44, 62, 80);">最后是解析文本，这个比较简单，只需要将文本取出，然后有两种情况，一种是普通的文本，直接构建一个节点 push 进当前</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">currentParent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的 children 中即可。还有一种情况是文本是如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这样的 Vue.js 的表达式，这时候我们需要用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">parseText</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来将表达式转化成代码。</font>

```javascript
text = html.substring(0, textEnd)
advance(textEnd)
let expression;
if (expression = parseText(text)) {
  currentParent.children.push({
    type: 2,
    text,
    expression
  });
} else {
  currentParent.children.push({
    type: 3,
    text,
  });
}
continue;
```



<font style="color:rgb(44, 62, 80);">我们会用到一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">parseText</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数。</font>

```javascript
function parseText (text) {
  if (!defaultTagRE.test(text)) return;

  const tokens = [];
  let lastIndex = defaultTagRE.lastIndex = 0
  let match, index
  while ((match = defaultTagRE.exec(text))) {
    index = match.index

    if (index > lastIndex) {
      tokens.push(JSON.stringify(text.slice(lastIndex, index)))
    }

    const exp = match[1].trim()
    tokens.push(`_s(${exp})`)
    lastIndex = index + match[0].length
  }

  if (lastIndex < text.length) {
    tokens.push(JSON.stringify(text.slice(lastIndex)))
  }
  return tokens.join('+');
}
```

<font style="color:rgba(255, 255, 255, 0.3);background-color:rgb(40, 44, 52);"></font>

<font style="color:rgb(44, 62, 80);">我们使用一个 tokens 数组来存放解析结果，通过 defaultTagRE 来循环匹配该文本，如果是普通文本直接 push 到 tokens 数组中去，如果是表达式</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">， 则转化成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">_s(${exp})</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的形式。</font>

<font style="color:rgb(44, 62, 80);">举个例子，如果我们有这样一个文本。</font>

```javascript
<div>hello,{{name}}.</div>
```

<font style="color:rgba(255, 255, 255, 0.3);background-color:rgb(40, 44, 52);">1</font>

<font style="color:rgb(44, 62, 80);">最终得到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">tokens</font><font style="color:rgb(44, 62, 80);">。</font>

```javascript
tokens = ['hello,', _s(name), '.'];
```

<font style="color:rgba(255, 255, 255, 0.3);background-color:rgb(40, 44, 52);">1</font>

<font style="color:rgb(44, 62, 80);">最终通过 join 返回表达式。</font>

```javascript
'hello' + _s(name) + '.';...
```



## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#processif-%E4%B8%8E-processfor)<font style="color:rgb(44, 62, 80);">processIf 与 processFor</font>
<font style="color:rgb(44, 62, 80);">最后介绍一下如何处理</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">v-if</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以及</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">v-for</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这样的 Vue.js 的表达式的，这里我们只简单介绍两个示例中用到的表达式解析。</font>

<font style="color:rgb(44, 62, 80);">我们只需要在解析头标签的内容中加入这两个表达式的解析函数即可，在这时 </font><font style="color:rgb(71, 101, 130);">v-for</font><font style="color:rgb(44, 62, 80);"> 之类指令已经在属性解析时存入了 attrsMap 中了。</font><font style="color:rgb(44, 62, 80);background-color:rgb(40, 44, 52);">  
</font>

```javascript
if (html.match(startTagOpen)) {
  const startTagMatch = parseStartTag();
  const element = {
    type: 1,
    tag: startTagMatch.tagName,
    attrsList: startTagMatch.attrs,
    attrsMap: makeAttrsMap(startTagMatch.attrs),
    parent: currentParent,
    children: []
  }

  processIf(element);
  processFor(element);

  if(!root){
    root = element
  }

  if(currentParent){
    currentParent.children.push(element);
  }

  stack.push(element);
  currentParent = element;
  continue;
}
```



<font style="color:rgb(44, 62, 80);">首先我们需要定义一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">getAndRemoveAttr</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，用来从 el 的 attrsMap 属性或是 attrsList 属性中取出 name 对应值。</font>

```javascript
function getAndRemoveAttr (el, name) {
  let val
  if ((val = el.attrsMap[name]) != null) {
    const list = el.attrsList
    for (let i = 0, l = list.length; i < l; i++) {
      if (list[i].name === name) {
        list.splice(i, 1)
        break
      }
    }
  }
  return val
}
```

 

<font style="color:rgb(44, 62, 80);">比如说解析示例的 div 标签属性。</font>

```javascript
getAndRemoveAttr(el, 'v-for');
```

<font style="color:rgba(255, 255, 255, 0.3);background-color:rgb(40, 44, 52);">1</font>

<font style="color:rgb(44, 62, 80);">可有得到“item in sz”。</font>

<font style="color:rgb(44, 62, 80);">有了这个函数这样我们就可以开始实现 processFor 与 processIf 了。</font>

<font style="color:rgb(71, 101, 130);">v-for</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会将指令解析成 for 属性以及 alias 属性，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">v-if</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会将条件都存入</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">ifConditions</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数组中。</font>

```javascript
function processFor (el) {
  let exp;
  if ((exp = getAndRemoveAttr(el, 'v-for'))) {
    const inMatch = exp.match(forAliasRE);
    el.for = inMatch[2].trim();
    el.alias = inMatch[1].trim();
  }
}

function processIf (el) {
  const exp = getAndRemoveAttr(el, 'v-if');
  if (exp) {
    el.if = exp;
    if (!el.ifConditions) {
      el.ifConditions = [];
    }
    el.ifConditions.push({
      exp: exp,
      block: el
    });
  }
}
```

 

<font style="color:rgb(44, 62, 80);">到这里，我们已经把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">parse</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的过程介绍完了，接下来看一下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">optimize</font><font style="color:rgb(44, 62, 80);">。</font>

## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#optimize)<font style="color:rgb(44, 62, 80);">optimize</font>
<font style="color:rgb(44, 62, 80);">optimize 主要作用就跟它的名字一样，用作「优化」。</font>

<font style="color:rgb(44, 62, 80);">这个涉及到后面要讲 patch 的过程，因为 patch 的过程实际上是将 VNode 节点进行一层一层的比对，然后将「差异」更新到视图上。那么一些静态节点是不会根据数据变化而产生变化的，这些节点我们没有比对的需求，是不是可以跳过这些静态节点的比对，从而节省一些性能呢？</font>

<font style="color:rgb(44, 62, 80);">那么我们就需要为静态的节点做上一些「标记」，在 patch 的时候我们就可以直接跳过这些被标记的节点的比对，从而达到「优化」的目的。</font>

<font style="color:rgb(44, 62, 80);">经过 optimize 这层的处理，每个节点会加上 static 属性，用来标记是否是静态的。</font>

<font style="color:rgb(44, 62, 80);">得到如下结果。</font><font style="color:rgb(44, 62, 80);background-color:rgb(40, 44, 52);">  
</font>

```javascript
{
  'attrsMap': {
    ':class': 'c',
      'class': 'demo',
      'v-if': 'isShow'
  },
  'classBinding': 'c',
    'if': 'isShow',
    'ifConditions': [
    'exp': 'isShow'
  ],
  'staticClass': 'demo',
    'tag': 'div',
    /* 静态标志 */
    'static': false,
    'children': [
    {
      'attrsMap': {
        'v-for': "item in sz"
      },
      'static': false,
      'alias': "item",
      'for': 'sz',
      'forProcessed': true,
      'tag': 'span',
      'children': [
        {
          'expression': '_s(item)',
          'text': '{{item}}',
          'static': false
        }
      ]
    }
  ]
}
```

<font style="color:rgba(255, 255, 255, 0.3);background-color:rgb(40, 44, 52);"></font>

<font style="color:rgb(44, 62, 80);">我们用代码实现一下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">optimize</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数。</font>

### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#isstatic)<font style="color:rgb(44, 62, 80);">isStatic</font>
<font style="color:rgb(44, 62, 80);">首先实现一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">isStatic</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，传入一个 node 判断该 node 是否是静态节点。判断的标准是当 type 为 2（表达式节点）则是非静态节点，当 type 为 3（文本节点）的时候则是静态节点，当然，如果存在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">if</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">for</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这样的条件的时候（表达式节点），也是非静态节点。</font>

```javascript
function isStatic (node) {
  if (node.type === 2) {
    return false
  }
  if (node.type === 3) {
    return true
  }
  return (!node.if && !node.for);
}
```



### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#markstatic)<font style="color:rgb(44, 62, 80);">markStatic</font>
<font style="color:rgb(71, 101, 130);">markStatic</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为所有的节点标记上</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">static</font><font style="color:rgb(44, 62, 80);">，遍历所有节点通过 isStatic 来判断当前节点是否是静态节点，此外，会遍历当前节点的所有子节点，如果子节点是非静态节点，那么当前节点也是非静态节点。</font>

```javascript
function markStatic (node) {
  node.static = isStatic(node);
  if (node.type === 1) {
    for (let i = 0, l = node.children.length; i < l; i++) {
      const child = node.children[i];
      markStatic(child);
      if (!child.static) {
        node.static = false;
      }
    }
  }
}
```



### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#markstaticroots)<font style="color:rgb(44, 62, 80);">markStaticRoots</font>
<font style="color:rgb(44, 62, 80);">接下来是 markStaticRoots 函数，用来标记 staticRoot（静态根）。这个函数实现比较简单，简单来将就是如果当前节点是静态节点，同时满足该节点并不是只有一个文本节点左右子节点时，标记 staticRoot 为 true，否则为 false。</font>

```javascript
function markStaticRoots (node) {
  if (node.type === 1) {
    if (node.static && node.children.length && !(
      node.children.length === 1 &&
      node.children[0].type === 3
    )) {
      node.staticRoot = true;
      return;
    } else {
      node.staticRoot = false;
    }
  }
}
```



<font style="color:rgb(44, 62, 80);">有了以上的函数，就可以实现 optimize 了。</font>

```javascript
function optimize (rootAst) {
  markStatic(rootAst);
  markStaticRoots(rootAst);
}
```



## [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#generate)<font style="color:rgb(44, 62, 80);">generate</font>
<font style="color:rgb(44, 62, 80);">generate 会将 AST 转化成 render funtion 字符串，最终得到 render 的字符串以及 staticRenderFns 字符串。</font>

<font style="color:rgb(44, 62, 80);">首先带大家感受一下真实的 Vue.js 编译得到的结果。</font>

```javascript
with(this){
  return (isShow) ?
    _c(
      'div',
      {
        staticClass: "demo",
        class: c
      },
      _l(
        (sz),
        function(item){
          return _c('span',[_v(_s(item))])
        }
      )
    )
    : _e()
}
```



<font style="color:rgb(44, 62, 80);">看到这里可能会纳闷了，这些</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(71, 101, 130);">_c，_l</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">到底是什么？其实他们是 Vue.js 对一些函数的简写，比如说 _c 对应的是 createElement 这个函数。没关系，我们把它用 VNode 的形式写出来就会明白了，这个对接上一章写的 VNode 函数。</font>

<font style="color:rgb(44, 62, 80);">首先是第一层 div 节点。</font>

```javascript
render () {
  return isShow ? (new VNode('div', {
    'staticClass': 'demo',
    'class': c
  }, [ /*这里还有子节点*/ ])) : createEmptyVNode();
}
```



<font style="color:rgb(44, 62, 80);">然后我们在 children 中加上第二层 span 及其子文本节点节点。</font>

```javascript
/* 渲染v-for列表 */
function renderList (val, render) {
  let ret = new Array(val.length);
  for (i = 0, l = val.length; i < l; i++) {
    ret[i] = render(val[i], i);
  }
}

render () {
  return isShow ? (new VNode('div', {
    'staticClass': 'demo',
    'class': c
  },
                             /* begin */
                             renderList(sz, (item) => {
                               return new VNode('span', {}, [
                                 createTextVNode(item);
                               ]);
                             })
                             /* end */
                            )) : createEmptyVNode();
}
```



<font style="color:rgb(44, 62, 80);">那我们如何来实现一个 generate 呢？</font>

### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#genif)<font style="color:rgb(44, 62, 80);">genIf</font>
<font style="color:rgb(44, 62, 80);">首先实现一个处理 if 条件的 genIf 函数。</font>

```javascript
function genIf (el) {
  el.ifProcessed = true;
  if (!el.ifConditions.length) {
    return '_e()';
  }
  return `(${el.ifConditions[0].exp})?${genElement(el.ifConditions[0].block)}: _e()`
}
```



### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#genfor)<font style="color:rgb(44, 62, 80);">genFor</font>
<font style="color:rgb(44, 62, 80);">然后是处理 for 循环的函数。</font>

```javascript
function genFor (el) {
  el.forProcessed = true;

  const exp = el.for;
  const alias = el.alias;
  const iterator1 = el.iterator1 ? `,${el.iterator1}` : '';
  const iterator2 = el.iterator2 ? `,${el.iterator2}` : '';

  return `_l((${exp}),` +
    `function(${alias}${iterator1}${iterator2}){` +
    `return ${genElement(el)}` +
    '})';
}
```



### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#genfor-2)<font style="color:rgb(44, 62, 80);">genFor</font>
<font style="color:rgb(44, 62, 80);">然后是处理 for 循环的函数。</font>

```javascript
function genFor (el) {
  el.forProcessed = true;

  const exp = el.for;
  const alias = el.alias;
  const iterator1 = el.iterator1 ? `,${el.iterator1}` : '';
  const iterator2 = el.iterator2 ? `,${el.iterator2}` : '';

  return `_l((${exp}),` +
    `function(${alias}${iterator1}${iterator2}){` +
    `return ${genElement(el)}` +
    '})';
}
```



### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#gentext)<font style="color:rgb(44, 62, 80);">genText</font>
<font style="color:rgb(44, 62, 80);">处理文本节点的函数。</font>

```javascript
function genText (el) {
  return `_v(${el.expression})`;
}
```



### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#genelement)<font style="color:rgb(44, 62, 80);">genElement</font>
<font style="color:rgb(44, 62, 80);">接下来实现一下 genElement，这是一个处理节点的函数，因为它依赖 genChildren 以及g enNode ，所以这三个函数放在一起讲。</font>

<font style="color:rgb(44, 62, 80);">genElement会根据当前节点是否有 if 或者 for 标记然后判断是否要用 genIf 或者 genFor 处理，否则通过 genChildren 处理子节点，同时得到 staticClass、class 等属性。</font>

<font style="color:rgb(44, 62, 80);">genChildren 比较简单，遍历所有子节点，通过 genNode 处理后用“，”隔开拼接成字符串。</font>

<font style="color:rgb(44, 62, 80);">genNode 则是根据 type 来判断该节点是用文本节点 genText 还是标签节点 genElement 来处理。</font>

```javascript
function genNode (el) {
  if (el.type === 1) {
    return genElement(el);
  } else {
    return genText(el);
  }
}

function genChildren (el) {
  const children = el.children;

  if (children && children.length > 0) {
    return `${children.map(genNode).join(',')}`;
  }
}

function genElement (el) {
  if (el.if && !el.ifProcessed) {
    return genIf(el);
  } else if (el.for && !el.forProcessed) {
    return genFor(el);
  } else {
    const children = genChildren(el);
    let code;
    code = `_c('${el.tag},'{
            staticClass: ${el.attrsMap && el.attrsMap[':class']},
            class: ${el.attrsMap && el.attrsMap['class']},
        }${
          children ? `,${children}` : ''
        })`
    return code;
  }
}
```



### [****](https://xinxingyu.github.io/blog/blog/Vue/mechanism/e-templateCompile.html#generate-2)<font style="color:rgb(44, 62, 80);">generate</font>
<font style="color:rgb(44, 62, 80);">最后我们使用上面的函数来实现 generate，其实很简单，我们只需要将整个 AST 传入后判断是否为空，为空则返回一个 div 标签，否则通过 generate 来处理。</font>

```javascript
function generate (rootAst) {
  const code = rootAst ? genElement(rootAst) : '_c("div")'
  return {
    render: `with(this){return ${code}}`,
  }
}
```



<font style="color:rgb(44, 62, 80);">经历过这些过程以后，我们已经把 template 顺利转成了 render function 了，接下来我们将介绍 patch 的过程，来看一下具体 VNode 节点如何进行差异的比对</font>

