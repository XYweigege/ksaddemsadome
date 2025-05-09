### <font style="color:rgb(44, 62, 80);">0 对虚拟DOM的理解</font>
<br/>color1
<font style="color:rgb(199, 37, 78);">虚拟dom从来不是用来和直接操作dom对比的</font><font style="color:rgb(85, 85, 85);">，它们俩最终殊途同归。</font><font style="color:rgb(199, 37, 78);">虚拟dom只不过是局部更新的一个环节而已</font><font style="color:rgb(85, 85, 85);">，整个环节的对比对象是全量更新。虚拟dom对于state＝UI的意义是，虚拟dom使diff成为可能（理论上也可以直接用dom对象diff，但是太臃肿），促进了新的开发思想，又不至于性能太差。但是性能再好也不可能好过直接操作dom，人脑连diff都省了。还有一个很重要的意义是，</font><font style="color:rgb(199, 37, 78);">对视图抽象，为跨平台助力</font>

<br/>

<font style="color:rgb(44, 62, 80);">其实我最终希望你明白的事情只有一件：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">虚拟 DOM 的价值不在性能，而在别处</font><font style="color:rgb(44, 62, 80);">。因此想要从性能角度来把握虚拟 DOM 的优势，无异于南辕北辙。偏偏在面试场景下，10 个人里面有 9 个都走这条歧路，最后9个人里面自然没有一个能自圆其说，实在让人惋惜。</font>

真正理解虚拟DOM(opens new window)

### [](https://www.123fe.net/docs/base/improve.html#_1-%E8%B0%88%E8%B0%88%E4%BD%A0%E5%AF%B9react%E7%9A%84%E7%90%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">1 谈谈你对React的理解</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">React 是一个网页 UI 框架，通过组件化的方式解决视图层开发复用的问题，本质是一个组件化框架。</font>

+ <font style="color:rgb(44, 62, 80);">它的核心设计思路有三点，分别是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">声明式、组件化与 通用性</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">声明式的优势在于直观与组合。</font>
+ <font style="color:rgb(44, 62, 80);">组件化的优势在于视图的拆分与模块复用，可以更容易做到高内聚低耦合。</font>
+ <font style="color:rgb(44, 62, 80);">通用性在于一次学习，随处编写。比如 React Native，React 360 等， 这里主要靠虚拟 DOM 来保证实现。</font>
+ <font style="color:rgb(44, 62, 80);">这使得 React 的适用范围变得足够广，无论是 Web、Native、VR，甚至 Shell 应用都可以进行开发。这也是 React 的优势。</font>
+ <font style="color:rgb(44, 62, 80);">但作为一个视图层的框架，React 的劣势也十分明显。它并没有提供完整的一揽子解决方 案，在开发大型前端应用时，需要向社区寻找并整合解决方案。虽然一定程度上促进了社区的繁荣，但也为开发者在技术选型和学习适用上造成了一定的成本。</font>
+ <font style="color:rgb(44, 62, 80);">承接在优势后，可以再谈一下自己对于 React 优化的看法、对虚拟 DOM 的看法</font>

### [](https://www.123fe.net/docs/base/improve.html#_2-%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8Dreact%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E4%B8%AD%E7%9A%84%E5%9D%91)<font style="color:rgb(44, 62, 80);">2 如何避免React生命周期中的坑</font>
**<font style="color:rgb(44, 62, 80);">16.3版本</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781934832-0f4f0908-d6a8-4444-99a6-9eaa79bc32e9.png)

**<font style="color:rgb(44, 62, 80);">>=16.4版本</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781935252-4e84d47b-efb2-425a-a2c3-e7ed2918f91e.png)

**<font style="color:rgb(44, 62, 80);">在线查看</font>**<font style="color:rgb(44, 62, 80);">：</font>[https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram(opens new window)](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781935294-dcb30df4-0e47-499b-84e5-abfcb2b7a7ce.png)

+ <font style="color:rgb(44, 62, 80);">避免生命周期中的坑需要做好两件事：不在恰当的时候调用了不该调用的代码；在需要调用时，不要忘了调用。</font>
+ <font style="color:rgb(44, 62, 80);">那么主要有这么 7 种情况容易造成生命周期的坑</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getDerivedStateFromProps</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">容易编写反模式代码，使受控组件与非受控组件区分模糊</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillMount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在 React 中已被标记弃用，不推荐使用，主要原因是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">新的异步渲染架构会导致它被多次调用</font><font style="color:rgb(44, 62, 80);">。所以网络请求及事件绑定代码应移至</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillReceiveProps</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">同样被标记弃用，被</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getDerivedStateFromProps</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所取代，主要原因是性能问题</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">通过返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来确定是否需要触发新的渲染。主要用于性能优化</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">同样是由于新的异步渲染机制，而被标记废弃，不推荐使用，原先的逻辑可结合</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getSnapshotBeforeUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">改造使用。</font>
    - <font style="color:rgb(44, 62, 80);">如果在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillUnmount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数中忘记解除事件绑定，取消定时器等清理操作，容易引发 bug</font>
    - <font style="color:rgb(44, 62, 80);">如果没有添加错误边界处理，当渲染发生异常时，用户将会看到一个无法操作的白屏，所以一定要添加</font>

**<font style="color:rgb(44, 62, 80);">“React 的请求应该放在哪里，为什么?” 这也是经常会被追问的问题。你可以这样回答。</font>**

<font style="color:rgb(44, 62, 80);">对于异步请求，应该放在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中去操作。从时间顺序来看，除了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">还可以有以下选择：</font>

+ <font style="color:rgb(44, 62, 80);">constructor：可以放，但从设计上而言不推荐。constructor 主要用于初始化 state 与函数绑定，并不承载业务逻辑。而且随着类属性的流行，constructor 已经很少使用了</font>
+ <font style="color:rgb(44, 62, 80);">componentWillMount：已被标记废弃，在新的异步渲染架构下会触发多次渲染，容易引发 Bug，不利于未来 React 升级后的代码维护。</font>
+ <font style="color:rgb(44, 62, 80);">所以React 的请求放在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount 里是最好的选择</font><font style="color:rgb(44, 62, 80);">。</font>

**<font style="color:rgb(44, 62, 80);">透过现象看本质：React 16 缘何两次求变？</font>**

<font style="color:rgb(44, 62, 80);">Fiber 架构简析</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">Fiber 是 React 16 对 React 核心算法的一次重写。你只需要 get 到这一个点：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber 会使原本同步的渲染过程变成异步的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

<font style="color:rgb(44, 62, 80);">在 React 16 之前，每当我们触发一次组件的更新，React 都会构建一棵新的虚拟 DOM 树，通过与上一次的虚拟 DOM 树进行 diff，实现对 DOM 的定向更新。这个过程，是一个递归的过程。下面这张图形象地展示了这个过程的特征：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781935256-35856601-e3da-4c3f-9fa9-0912d822f113.png)

<font style="color:rgb(44, 62, 80);">如图所示，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">同步渲染的递归调用栈是非常深的，只有最底层的调用返回了，整个渲染过程才会开始逐层返回</font><font style="color:rgb(44, 62, 80);">。这个漫长且不可打断的更新过程，将会带来用户体验层面的巨大风险：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">同步渲染一旦开始，便会牢牢抓住主线程不放，直到递归彻底完成</font><font style="color:rgb(44, 62, 80);">。在这个过程中，浏览器没有办法处理任何渲染之外的事情，会进入一种无法处理用户交互的状态。因此若渲染时间稍微长一点，页面就会面临卡顿甚至卡死的风险。</font>

<font style="color:rgb(44, 62, 80);">而 React 16 引入的 Fiber 架构，恰好能够解决掉这个风险：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber 会将一个大的更新任务拆解为许多个小任务</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">每当执行完一个小任务时，渲染线程都会把主线程交回去</font><font style="color:rgb(44, 62, 80);">，看看有没有优先级更高的工作要处理，确保不会出现其他任务被“饿死”的情况，进而避免同步渲染带来的卡顿。在这个过程中，渲染线程不再“一去不回头”，而是可以被打断的，这就是所谓的“异步渲染”，它的执行过程如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781935246-ab04be4f-b3bc-4da3-8bd6-7eed39db3a19.png)

**<font style="color:rgb(44, 62, 80);">换个角度看生命周期工作流</font>**

<font style="color:rgb(44, 62, 80);">Fiber 架构的重要特征就是可以被打断的异步渲染模式。但这个“打断”是有原则的，根据“能否被打断”这一标准，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 16 的生命周期被划分为了 render 和 commit 两个阶段</font><font style="color:rgb(44, 62, 80);">，而 commit 阶段又被细分为了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pre-commit 和 commit</font><font style="color:rgb(44, 62, 80);">。每个阶段所涵盖的生命周期如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781937344-f7be7b24-3603-4207-b757-65746bb7f8fe.png)

<font style="color:rgb(44, 62, 80);">我们先来看下三个阶段各自有哪些特征</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render 阶段</font><font style="color:rgb(44, 62, 80);">：纯净且没有副作用，可能会被 React 暂停、终止或重新启动。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pre-commit 阶段</font><font style="color:rgb(44, 62, 80);">：可以读取 DOM。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commit 阶段</font><font style="color:rgb(44, 62, 80);">：可以使用 DOM，运行副作用，安排更新。</font>

<font style="color:rgb(44, 62, 80);">总的来说，render 阶段在执行过程中允许被打断，而 commit 阶段则总是同步执行的。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为什么这样设计呢？简单来说，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">由于 render 阶段的操作对用户来说其实是“不可见”的，所以就算打断再重启，对用户来说也是零感知</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。而</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commit 阶段的操作则涉及真实 DOM 的渲染</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，所以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">这个过程必须用同步渲染来求稳</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

**<font style="color:rgb(44, 62, 80);">为什么 React 16 要更改组件的生命周期详解</font>**

+ [React16为什么要更改生命周期(opens new window)](https://www.123fe.net/principle-docs/react/11-React16%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9B%B4%E6%94%B9%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E4%B8%8A.html)
+ [React16为什么要更改生命周期(opens new window)](https://www.123fe.net/principle-docs/react/12-React16%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9B%B4%E6%94%B9%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E4%B8%8B.html)

### [](https://www.123fe.net/docs/base/improve.html#_3-react-fiber%E6%9E%B6%E6%9E%84)<font style="color:rgb(44, 62, 80);">3 React Fiber架构</font>
**<font style="color:rgb(44, 62, 80);">最主要的思想就是将任务拆分</font>**<font style="color:rgb(44, 62, 80);">。</font>

+ <font style="color:rgb(44, 62, 80);">DOM需要渲染时暂停，空闲时恢复。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.requestIdleCallback</font>
+ <font style="color:rgb(44, 62, 80);">React内部实现的机制</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">React 追求的是 “快速响应”，那么，“快速响应“的制约因素都有什么呢</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CPU</font><font style="color:rgb(44, 62, 80);">的瓶颈：当项目变得庞大、组件数量繁多、遇到大计算量的操作或者设备性能不足使得页面掉帧，导致卡顿。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IO</font><font style="color:rgb(44, 62, 80);">的瓶颈：发送网络请求后，由于需要等待数据返回才能进一步操作导致不能快速响应。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">架构主要就是用来解决</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CPU</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和网络的问题，这两个问题一直也是最影响前端开发体验的地方，一个会造成卡顿，一个会造成白屏。为此 react 为前端引入了两个新概念：Time Slicing</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">时间分片</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Suspense</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

**<font style="color:rgb(44, 62, 80);">1. React 都做过哪些优化</font>**

+ **<font style="color:rgb(44, 62, 80);">React渲染页面的两个阶段</font>**
    - <font style="color:rgb(44, 62, 80);">调度阶段（reconciliation）：在这个阶段 React 会更新数据生成新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(44, 62, 80);">，然后通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Diff</font><font style="color:rgb(44, 62, 80);">算法，快速找出需要更新的元素，放到更新队列中去，得到新的更新队列。</font>
    - <font style="color:rgb(44, 62, 80);">渲染阶段（commit）：这个阶段 React 会遍历更新队列，将其所有的变更一次性更新到DOM上</font>
+ **<font style="color:rgb(44, 62, 80);">React 15 架构</font>**
    - <font style="color:rgb(44, 62, 80);">React15架构可以分为两层</font>
        * <font style="color:rgb(44, 62, 80);">Reconciler（协调器）—— 负责找出变化的组件；</font>
        * <font style="color:rgb(44, 62, 80);">Renderer（渲染器）—— 负责将变化的组件渲染到页面上；</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在React15及以前，Reconciler采用递归的方式创建虚拟DOM，递归过程是不能中断的。如果组件树的层级很深，递归会占用线程很多时间，递归更新时间超过了16ms，用户交互就会卡顿。</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为了解决这个问题，React16将递归的无法中断的更新重构为异步的可中断更新，由于曾经用于递归的虚拟DOM数据结构已经无法满足需要。于是，全新的Fiber架构应运而生。</font>
+ **<font style="color:rgb(44, 62, 80);">React 16 架构</font>**
    - <font style="color:rgb(44, 62, 80);">为了解决同步更新长时间占用线程导致页面卡顿的问题，也为了探索运行时优化的更多可能，React开始重构并一直持续至今。重构的目标是实现Concurrent Mode（并发模式）。</font>
    - <font style="color:rgb(44, 62, 80);">从v15到v16，React团队花了两年时间将源码架构中的Stack Reconciler重构为Fiber Reconciler</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React16架构可以分为三层</font><font style="color:rgb(44, 62, 80);">：</font>
        * <font style="color:rgb(44, 62, 80);">Scheduler（调度器）—— 调度任务的优先级，高优任务优先进入Reconciler；</font>
        * <font style="color:rgb(44, 62, 80);">Reconciler（协调器）—— 负责找出变化的组件：更新工作从递归变成了可以中断的循环过程。Reconciler内部采用了Fiber的架构；</font>
        * <font style="color:rgb(44, 62, 80);">Renderer（渲染器）—— 负责将变化的组件渲染到页面上。</font>
+ **<font style="color:rgb(44, 62, 80);">React 17 优化</font>**
    - <font style="color:rgb(44, 62, 80);">使用Lane来管理任务的优先级。Lane用二进制位表示任务的优先级，方便优先级的计算（位运算），不同优先级占用不同位置的“赛道”，而且存在批的概念，优先级越低，“赛道”越多。高优先级打断低优先级，新建的任务需要赋予什么优先级等问题都是Lane所要解决的问题。</font>
    - <font style="color:rgb(44, 62, 80);">Concurrent Mode的目的是实现一套可中断/恢复的更新机制。其由两部分组成：</font>
        * <font style="color:rgb(44, 62, 80);">一套协程架构：Fiber Reconciler</font>
        * <font style="color:rgb(44, 62, 80);">基于协程架构的启发式更新算法：控制协程架构工作方式的算法</font>

**<font style="color:rgb(44, 62, 80);">2. 浏览器一帧都会干些什么以及requestIdleCallback的启示</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们都知道，页面的内容都是一帧一帧绘制出来的，浏览器刷新率代表浏览器一秒绘制多少帧。原则上说 1s 内绘制的帧数也多，画面表现就也细腻。目前浏览器大多是 60Hz（60帧/s），每一帧耗时也就是在 16.6ms 左右。那么在这一帧的（16.6ms） 过程中浏览器又干了些什么呢</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781938481-0338fc67-c2ca-4d54-944a-ebc615d2045b.png)

<font style="color:rgb(44, 62, 80);">通过上面这张图可以清楚的知道，浏览器一帧会经过下面这几个过程：</font>

1. <font style="color:rgb(44, 62, 80);">接受输入事件</font>
2. <font style="color:rgb(44, 62, 80);">执行事件回调</font>
3. <font style="color:rgb(44, 62, 80);">开始一帧</font>
4. <font style="color:rgb(44, 62, 80);">执行 RAF (RequestAnimationFrame)</font>
5. <font style="color:rgb(44, 62, 80);">页面布局，样式计算</font>
6. <font style="color:rgb(44, 62, 80);">绘制渲染</font>
7. <font style="color:rgb(44, 62, 80);">执行 RIC (RequestIdelCallback)</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">第七步的 RIC 事件不是每一帧结束都会执行，只有在一帧的 16.6ms 中做完了前面 6 件事儿且还有剩余时间，才会执行。如果一帧执行结束后还有时间执行 RIC 事件，那么下一帧需要在事件执行结束才能继续渲染，所以 RIC 执行不要超过 30ms，如果长时间不将控制权交还给浏览器，会影响下一帧的渲染，导致页面出现卡顿和事件响应不及时。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback 的启示</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">：我们以浏览器是否有剩余时间作微任务中断的标准，那么我们需要一种机制，当浏览器有剩余时间时通知我们。</font>

```javascript
requestIdleCallback((deadline) => {
  // deadline 有两个参数
  // timeRemaining(): 当前帧还剩下多少时间
  // didTimeout: 是否超时
  // 另外 requestIdleCallback 后如果跟上第二个参数 {timeout: ...} 则会强制浏览器在当前帧执行完后执行。
  if (deadline.timeRemaining() > 0) {
    // TODO
  } else {
    requestIdleCallback(otherTasks);
  }
});
```

```javascript
// 用法示例
var tasksNum = 10000

requestIdleCallback(unImportWork)

function unImportWork(deadline) {
  while (deadline.timeRemaining() && tasksNum > 0) {
    console.log(`执行了${10000 - tasksNum + 1}个任务`)
    tasksNum--
  }
  if (tasksNum > 0) { // 在未来的帧中继续执行
    requestIdleCallback(unImportWork)
  }
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">其实部分浏览器已经实现了这个API，这就是requestIdleCallback。但是由于以下因素，Facebook 抛弃了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的原生 API：</font>

+ <font style="color:rgb(44, 62, 80);">浏览器兼容性；</font>
+ <font style="color:rgb(44, 62, 80);">触发频率不稳定，受很多因素影响。比如当我们的浏览器切换tab后，之前tab注册的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(44, 62, 80);">触发的频率会变得很低。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">基于以上原因，在React中实现了功能更完备的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallbackpolyfill</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，这就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Scheduler</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。除了在空闲时触发回调的功能外，Scheduler还提供了多种调度优先级供任务设置</font>

**<font style="color:rgb(44, 62, 80);">3. React Fiber是什么</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是对核心算法的一次重新实现。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">把更新过程碎片化，把一个耗时长的任务分成很多小片，每一个小片的运行时间很短，虽然总时间依然很长，但是在每个小片执行完之后，都给其他任务一个执行的机会，这样唯一的线程就不会被独占，其他任务依然有运行的机会</font>

1. <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Fiber</font><font style="color:rgb(44, 62, 80);">中，一次更新过程会分成多个分片完成，所以完全有可能一个更新任务还没有完成，就被另一个更高优先级的更新过程打断，这时候，优先级高的更新任务会优先处理完，而低优先级更新任务所做的工作则会完全作废，然后等待机会重头再来</font>
2. <font style="color:rgb(44, 62, 80);">因为一个更新过程可能被打断，所以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Fiber</font><font style="color:rgb(44, 62, 80);">一个更新过程被分为两个阶段(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Phase</font><font style="color:rgb(44, 62, 80);">)：第一个阶段</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Reconciliation Phase</font><font style="color:rgb(44, 62, 80);">和第二阶段</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Commit Phase</font>
3. <font style="color:rgb(44, 62, 80);">在第一阶段</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Reconciliation Phase</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Fiber</font><font style="color:rgb(44, 62, 80);">会找出需要更新哪些</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">，这个阶段是可以被打断的；但是到了第二阶段</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Commit Phase</font><font style="color:rgb(44, 62, 80);">，那就一鼓作气把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">更新完，绝不会被打断</font>
4. <font style="color:rgb(44, 62, 80);">这两个阶段大部分工作都是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Fiber</font><font style="color:rgb(44, 62, 80);">做，和我们相关的也就是生命周期函数</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">改变了之前</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的组件渲染机制，新的架构使原来同步渲染的组件现在可以异步化，可中途中断渲染，执行更高优先级的任务。释放浏览器主线程</font>

**<font style="color:rgb(44, 62, 80);">关键特性</font>**

+ <font style="color:rgb(44, 62, 80);">增量渲染（把渲染任务拆分成块，匀到多帧）</font>
+ <font style="color:rgb(44, 62, 80);">更新时能够暂停，终止，复用渲染任务</font>
+ <font style="color:rgb(44, 62, 80);">给不同类型的更新赋予优先级</font>
+ <font style="color:rgb(44, 62, 80);">并发方面新的基础能力</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">增量渲染用来解决掉帧的问题，渲染任务拆分之后，每次只做一小段，做完一段就把时间控制权交还给主线程，而不像之前长时间占用</font>

**<font style="color:rgb(44, 62, 80);">4. 组件的渲染顺序</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">假如有A,B,C,D组件，层级结构为：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781939583-4dc2aef7-2fce-4374-84f1-6c8908a28264.png)

<font style="color:rgb(44, 62, 80);">我们知道组件的生命周期为：</font>

**<font style="color:rgb(44, 62, 80);">挂载阶段</font>**<font style="color:rgb(44, 62, 80);">：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">constructor()</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillMount()</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render()</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount()</font>

**<font style="color:rgb(44, 62, 80);">更新阶段为</font>**<font style="color:rgb(44, 62, 80);">：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillReceiveProps()</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate()</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillUpdate()</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render()</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidUpdate</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">那么在挂载阶段，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">A,B,C,D</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的生命周期渲染顺序是如何的呢？</font>

<font style="color:rgb(44, 62, 80);">那么在挂载阶段，A,B,C,D的生命周期渲染顺序是如何的呢？</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781940937-7f7755c9-0ad8-4acf-84b5-26407a0baf17.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数为分界线。从顶层组件开始，一直往下，直至最底层子组件。然后再往上</font>

<font style="color:rgb(44, 62, 80);">组件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">update</font><font style="color:rgb(44, 62, 80);">阶段同理</font>

<font style="color:rgb(44, 62, 80);">前面是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react16</font><font style="color:rgb(44, 62, 80);">以前的组建渲染方式。这就存在一个问题</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如果这是一个很大，层级很深的组件，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">渲染它需要几十甚至几百毫秒，在这期间，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会一直占用浏览器主线程，任何其他的操作（包括用户的点击，鼠标移动等操作）都无法执行</font>

**<font style="color:rgb(44, 62, 80);">Fiber架构就是为了解决这个问题</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">看一下fiber架构 组建的渲染顺序</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781941687-167297b9-1223-45f3-9d00-f099e1ec28c9.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">加入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">将组件更新分为两个时期</font>

**<font style="color:rgb(44, 62, 80);">这两个时期以render为分界</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">前的生命周期为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">phase1</font><font style="color:rgb(44, 62, 80);">,</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">后的生命周期为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">phase2</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">phase1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的生命周期是可以被打断的，每隔一段时间它会跳出当前渲染进程，去确定是否有其他更重要的任务。此过程，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workingProgressTree</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（并不是真实的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">virtualDomTree</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）上复用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">数据结构来一步地（通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）来构建新的 tree，标记处需要更新的节点，放入队列中</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">phase2</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的生命周期是不可被打断的，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">将其所有的变更一次性更新到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上</font>

**<font style="color:rgb(44, 62, 80);">这里最重要的是phase1这是时期所做的事。因此我们需要具体了解phase1的机制</font>**

+ <font style="color:rgb(44, 62, 80);">如果不被打断，那么</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">phase1</font><font style="color:rgb(44, 62, 80);">执行完会直接进入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">函数，构建真实的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">virtualDomTree</font>
+ <font style="color:rgb(44, 62, 80);">如果组件再</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">phase1</font><font style="color:rgb(44, 62, 80);">过程中被打断，即当前组件只渲染到一半（也许是在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">willMount</font><font style="color:rgb(44, 62, 80);">,也许是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">willUpdate</font><font style="color:rgb(44, 62, 80);">~反正是在render之前的生命周期），那么</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">会怎么干呢？</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">会放弃当前组件所有干到一半的事情，去做更高优先级更重要的任务（当然，也可能是用户鼠标移动，或者其他react监听之外的任务），当所有高优先级任务执行完之后，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callback</font><font style="color:rgb(44, 62, 80);">回到之前渲染到一半的组件，从头开始渲染。（看起来放弃已经渲染完的生命周期，会有点不合理，反而会增加渲染时长，但是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">确实是这么干的）</font>

**<font style="color:rgb(44, 62, 80);">所有phase1的生命周期函数都可能被执行多次，因为可能会被打断重来</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这样的话，就和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react16</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">版本之前有很大区别了，因为可能会被执行多次，那么我们最好就得保证</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">phase1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的生命周期每一次执行的结果都是一样的，否则就会有问题，因此，最好都是纯函数</font>

+ <font style="color:rgb(44, 62, 80);">如果高优先级的任务一直存在，那么低优先级的任务则永远无法进行，组件永远无法继续渲染。这个问题facebook目前好像还没解决</font>
+ <font style="color:rgb(44, 62, 80);">所以，facebook在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react16</font><font style="color:rgb(44, 62, 80);">增加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiber</font><font style="color:rgb(44, 62, 80);">结构，其实并不是为了减少组件的渲染时间，事实上也并不会减少，最重要的是现在可以使得一些更高优先级的任务，如用户的操作能够优先执行，提高用户的体验，至少用户不会感觉到卡顿</font>

**<font style="color:rgb(44, 62, 80);">5 React Fiber架构总结</font>**

**<font style="color:rgb(44, 62, 80);">React Fiber如何性能优化</font>**

+ **<font style="color:rgb(44, 62, 80);">更新的两个阶段</font>**
    - <font style="color:rgb(44, 62, 80);">调度算法阶段-执行diff算法，纯js计算</font>
    - <font style="color:rgb(44, 62, 80);">Commit阶段-将diff结果渲染dom</font>
+ <font style="color:rgb(44, 62, 80);">可能会有性能问题</font>
    - <font style="color:rgb(44, 62, 80);">JS是单线程的，且和DOM渲染公用一个线程</font>
    - <font style="color:rgb(44, 62, 80);">当组件足够复杂，组件更新时计算和渲染压力都大</font>
    - <font style="color:rgb(44, 62, 80);">同时再有DOM操作需求（动画、鼠标拖拽等），将卡顿</font>
+ **<font style="color:rgb(44, 62, 80);">解决方案fiber</font>**
    - <font style="color:rgb(44, 62, 80);">将调度算法阶段阶段任务拆分（Commit无法拆分）</font>
    - <font style="color:rgb(44, 62, 80);">DOM需要渲染时暂停，空闲时恢复</font>
    - **<font style="color:rgb(44, 62, 80);">分散执行:</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">任务分割后，就可以把小任务单元分散到浏览器的空闲期间去排队执行，而实现的关键是两个新API:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font>
        * <font style="color:rgb(44, 62, 80);">低优先级的任务交给</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(44, 62, 80);">处理，这是个浏览器提供的事件循环空闲期的回调函数，需要</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pollyfill</font><font style="color:rgb(44, 62, 80);">，而且拥有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">deadline</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">参数，限制执行事件，以继续切分任务；</font>
        * <font style="color:rgb(44, 62, 80);">高优先级的任务交给</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font><font style="color:rgb(44, 62, 80);">处理；</font>

**<font style="color:rgb(44, 62, 80);">React 的核心流程可以分为两个部分:</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconciliation</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">(调度算法，也可称为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">)</font>
    - <font style="color:rgb(44, 62, 80);">更新</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">；</font>
    - <font style="color:rgb(44, 62, 80);">调用生命周期钩子；</font>
    - <font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">virtual dom</font>
        * <font style="color:rgb(44, 62, 80);">这里应该称为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber Tree</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">更为符合；</font>
    - <font style="color:rgb(44, 62, 80);">通过新旧 vdom 进行 diff 算法，获取 vdom change</font>
    - <font style="color:rgb(44, 62, 80);">确定是否需要重新渲染</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commit</font>
    - <font style="color:rgb(44, 62, 80);">如需要，则操作</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点更新</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">要了解 Fiber，我们首先来看为什么需要它</font>

+ **<font style="color:rgb(44, 62, 80);">问题</font>**<font style="color:rgb(44, 62, 80);">: 随着应用变得越来越庞大，整个更新渲染的过程开始变得吃力，大量的组件渲染会导致主进程长时间被占用，导致一些动画或高频操作出现卡顿和掉帧的情况。而关键点，便是 同步阻塞。在之前的调度算法中，React 需要实例化每个类组件，生成一颗组件树，使用 同步递归 的方式进行遍历渲染，而这个过程最大的问题就是无法 暂停和恢复。</font>
+ **<font style="color:rgb(44, 62, 80);">解决方</font>**<font style="color:rgb(44, 62, 80);">案: 解决同步阻塞的方法，通常有两种: 异步 与 任务分割。而 React Fiber 便是为了实现任务分割而诞生的</font>
+ **<font style="color:rgb(44, 62, 80);">简述</font>**
    - <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React V16</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将调度算法进行了重构， 将之前的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stack reconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">重构成新版的 fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconciler</font><font style="color:rgb(44, 62, 80);">，变成了具有链表和指针的 单链表树遍历算法。通过指针映射，每个单元都记录着遍历当下的上一步与下一步，从而使遍历变得可以被暂停和重启</font>
    - <font style="color:rgb(44, 62, 80);">这里我理解为是一种 任务分割调度算法，主要是 将原先同步更新渲染的任务分割成一个个独立的 小任务单位，根据不同的优先级，将小任务分散到浏览器的空闲时间执行，充分利用主进程的事件循环机制</font>
+ **<font style="color:rgb(44, 62, 80);">核心</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这里可以具象为一个 数据结构</font>

```plain
class Fiber {
	constructor(instance) {
		this.instance = instance
		// 指向第一个 child 节点
		this.child = child
		// 指向父节点
		this.return = parent
		// 指向第一个兄弟节点
		this.sibling = previous
	}	
}
```

+ **<font style="color:rgb(44, 62, 80);">链表树遍历算法</font>**<font style="color:rgb(44, 62, 80);">: 通过 节点保存与映射，便能够随时地进行 停止和重启，这样便能达到实现任务分割的基本前提</font>
    - <font style="color:rgb(44, 62, 80);">首先通过不断遍历子节点，到树末尾；</font>
    - <font style="color:rgb(44, 62, 80);">开始通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sibling</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">遍历兄弟节点；</font>
    - <font style="color:rgb(44, 62, 80);">return 返回父节点，继续执行2；</font>
    - <font style="color:rgb(44, 62, 80);">直到 root 节点后，跳出遍历；</font>
+ **<font style="color:rgb(44, 62, 80);">任务分割</font>**<font style="color:rgb(44, 62, 80);">，React 中的渲染更新可以分成两个阶段</font>
    - **<font style="color:rgb(44, 62, 80);">reconciliation 阶段</font>**<font style="color:rgb(44, 62, 80);">: vdom 的数据对比，是个适合拆分的阶段，比如对比一部分树后，先暂停执行个动画调用，待完成后再回来继续比对</font>
    - **<font style="color:rgb(44, 62, 80);">Commit 阶段</font>**<font style="color:rgb(44, 62, 80);">: 将 change list 更新到 dom 上，并不适合拆分，才能保持数据与 UI 的同步。否则可能由于阻塞 UI 更新，而导致数据更新和 UI 不一致的情况</font>
+ **<font style="color:rgb(44, 62, 80);">分散执行:</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">任务分割后，就可以把小任务单元分散到浏览器的空闲期间去排队执行，而实现的关键是两个新API:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font>
    - <font style="color:rgb(44, 62, 80);">低优先级的任务交给</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(44, 62, 80);">处理，这是个浏览器提供的事件循环空闲期的回调函数，需要</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pollyfill</font><font style="color:rgb(44, 62, 80);">，而且拥有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">deadline</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">参数，限制执行事件，以继续切分任务；</font>
    - <font style="color:rgb(44, 62, 80);">高优先级的任务交给</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font><font style="color:rgb(44, 62, 80);">处理；</font>

```javascript
// 类似于这样的方式
requestIdleCallback((deadline) => {
  // 当有空闲时间时，我们执行一个组件渲染；
  // 把任务塞到一个个碎片时间中去；
  while ((deadline.timeRemaining() > 0 || deadline.didTimeout) && nextComponent) {
    nextComponent = performWork(nextComponent);
  }
});
```

+ **<font style="color:rgb(44, 62, 80);">优先级策略:</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">文本框输入 > 本次调度结束需完成的任务 > 动画过渡 > 交互反馈 > 数据更新 > 不会显示但以防将来会显示的任务</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">Fiber 其实可以算是一种编程思想，在其它语言中也有许多应用(Ruby Fiber)。</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">核心思想是 任务拆分和协同，主动把执行权交给主线程，使主线程有时间空挡处理其他高优先级任务。</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当遇到进程阻塞的问题时，任务分割、异步调用 和 缓存策略 是三个显著的解决思路。</font>

### [](https://www.123fe.net/docs/base/improve.html#_4-createelement%E8%BF%87%E7%A8%8B)<font style="color:rgb(44, 62, 80);">4 createElement过程</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">React.createElement()： 根据指定的第一个参数创建一个React元素</font>

```javascript
React.createElement(
  type,
  [props],
  [...children]
)
```

+ <font style="color:rgb(44, 62, 80);">第一个参数是必填，传入的是似HTML标签名称，eg: ul, li</font>
+ <font style="color:rgb(44, 62, 80);">第二个参数是选填，表示的是属性，eg: className</font>
+ <font style="color:rgb(44, 62, 80);">第三个参数是选填, 子节点，eg: 要显示的文本内容</font>

```javascript
//写法一：

var child1 = React.createElement('li', null, 'one');
var child2 = React.createElement('li', null, 'two');
var content = React.createElement('ul', { className: 'teststyle' }, child1, child2); // 第三个参数可以分开也可以写成一个数组
ReactDOM.render(
  content,
  document.getElementById('example')
);

//写法二：

var child1 = React.createElement('li', null, 'one');
var child2 = React.createElement('li', null, 'two');
var content = React.createElement('ul', { className: 'teststyle' }, [child1, child2]);
ReactDOM.render(
  content,
  document.getElementById('example')
);
```

### [](https://www.123fe.net/docs/base/improve.html#_5-%E8%B0%83%E5%92%8C%E9%98%B6%E6%AE%B5-setstate%E5%86%85%E9%83%A8%E5%B9%B2%E4%BA%86%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">5 调和阶段 setState内部干了什么</font>
+ <font style="color:rgb(44, 62, 80);">当调用 setState 时，React会做的第一件事情是将传递给 setState 的对象合并到组件的当前状态</font>
+ <font style="color:rgb(44, 62, 80);">这将启动一个称为和解（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconciliation</font><font style="color:rgb(44, 62, 80);">）的过程。和解（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconciliation</font><font style="color:rgb(44, 62, 80);">）的最终目标是以最有效的方式，根据这个新的状态来更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI</font><font style="color:rgb(44, 62, 80);">。 为此，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">将构建一个新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素树（您可以将其视为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的对象表示）</font>
+ <font style="color:rgb(44, 62, 80);">一旦有了这个树，为了弄清 UI 如何响应新的状态而改变，React 会将这个新树与上一个元素树相比较（ diff ）</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">通过这样做， React 将会知道发生的确切变化，并且通过了解发生什么变化，只需在绝对必要的情况下进行更新即可最小化 UI 的占用空间</font>

### [](https://www.123fe.net/docs/base/improve.html#_6-setstate)<font style="color:rgb(44, 62, 80);">6 setState</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在了解setState之前，我们先来简单了解下 React 一个包装结构: Transaction:</font>

**<font style="color:rgb(44, 62, 80);">事务 (Transaction)</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是 React 中的一个调用结构，用于包装一个方法，结构为: initialize - perform(method) - close。通过事务，可以统一管理一个方法的开始与结束；处于事务流中，表示进程正在执行一些操作</font>

+ <font style="color:rgb(44, 62, 80);">setState: React 中用于修改状态，更新视图。它具有以下特点:</font>

**<font style="color:rgb(44, 62, 80);">异步与同步:</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">setState并不是单纯的异步或同步，这其实与调用时的环境相关:</font>

+ <font style="color:rgb(44, 62, 80);">在</font>**<font style="color:rgb(44, 62, 80);">合成事件</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">生命周期钩子</font>**<font style="color:rgb(44, 62, 80);">(除 componentDidUpdate) 中，setState是"异步"的；</font>
    - <font style="color:rgb(44, 62, 80);">原因: 因为在setState的实现中，有一个判断: 当更新策略正在事务流的执行中时，该组件更新会被推入dirtyComponents队列中等待执行；否则，开始执行batchedUpdates队列更新；</font>
        * <font style="color:rgb(44, 62, 80);">在生命周期钩子调用中，更新策略都处于更新之前，组件仍处于事务流中，而componentDidUpdate是在更新之后，此时组件已经不在事务流中了，因此则会同步执行；</font>
        * <font style="color:rgb(44, 62, 80);">在合成事件中，React 是基于 事务流完成的事件委托机制 实现，也是处于事务流中；</font>
    - <font style="color:rgb(44, 62, 80);">问题: 无法在setState后马上从this.state上获取更新后的值。</font>
    - <font style="color:rgb(44, 62, 80);">解决: 如果需要马上同步去获取新值，setState其实是可以传入第二个参数的。setState(updater, callback)，在回调中即可获取最新值；</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">原生事件</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和 setTimeout 中，setState是同步的，可以马上获取更新后的值；</font>
    - <font style="color:rgb(44, 62, 80);">原因: 原生事件是浏览器本身的实现，与事务流无关，自然是同步；而setTimeout是放置于定时器线程中延后执行，此时事务流已结束，因此也是同步；</font>
+ **<font style="color:rgb(44, 62, 80);">批量更新</font>**<font style="color:rgb(44, 62, 80);">: 在 合成事件 和 生命周期钩子 中，setState更新队列时，存储的是 合并状态(Object.assign)。因此前面设置的 key 值会被后面所覆盖，最终只会执行一次更新；</font>
+ **<font style="color:rgb(44, 62, 80);">函数式</font>**<font style="color:rgb(44, 62, 80);">: 由于 Fiber 及 合并 的问题，官方推荐可以传入 函数 的形式。setState(fn)，在fn中返回新的state对象即可，例如this.setState((state, props) => newState)；</font>
    - <font style="color:rgb(44, 62, 80);">使用函数式，可以用于避免setState的批量更新的逻辑，传入的函数将会被 顺序调用；</font>

**<font style="color:rgb(44, 62, 80);">注意事项:</font>**

+ <font style="color:rgb(44, 62, 80);">setState 合并，在 合成事件 和 生命周期钩子 中多次连续调用会被优化为一次；</font>
+ <font style="color:rgb(44, 62, 80);">当组件已被销毁，如果再次调用setState，React 会报错警告，通常有两种解决办法</font>
    - <font style="color:rgb(44, 62, 80);">将数据挂载到外部，通过 props 传入，如放到 Redux 或 父级中；</font>
    - <font style="color:rgb(44, 62, 80);">在组件内部维护一个状态量 (isUnmounted)，componentWillUnmount中标记为 true，在setState前进行判断；</font>

**<font style="color:rgb(44, 62, 80);">总结</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">setState 并非真异步，只是看上去像异步。在源码中，通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来判断</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是先存进</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">队列还是直接更新，如果值为 true 则执行异步操作，为 false 则直接更新。</font>
+ <font style="color:rgb(44, 62, 80);">那么什么情况下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">呢？在 React 可以控制的地方，就为 true，比如在 React 生命周期事件和合成事件中，都会走合并操作，延迟更新的策略。</font>
+ <font style="color:rgb(44, 62, 80);">但在 React 无法控制的地方，比如原生事件，具体就是在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">addEventListener</font><font style="color:rgb(44, 62, 80);"> 、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等事件中，就只能同步更新。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">一般认为，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">做异步设计是为了性能优化、减少渲染次数</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，React 团队还补充了两点。</font>

+ <font style="color:rgb(44, 62, 80);">保持内部一致性。如果将 state 改为同步更新，那尽管 state 的更新是同步的，但是 props不是。</font>
+ <font style="color:rgb(44, 62, 80);">启用并发更新，完成异步渲染。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781942548-a3b2fbb8-d62e-4eda-8151-c849b3c6a3dc.png)

1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">只有在 React 自身的合成事件和钩子函数中是异步的，在原生事件和 setTimeout 中都是同步的</font>
2. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的异步并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数中没法立马拿到更新后的值，形成了所谓的异步。当然可以通过 setState 的第二个参数中的 callback 拿到更新后的结果</font>
3. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的批量更新优化也是建立在异步（合成事件、钩子函数）之上的，在原生事件和 setTimeout 中不会批量更新，在异步中如果对同一个值进行多次 setState，setState 的批量更新策略会对其进行覆盖，去最后一次的执行，如果是同时 setState 多个不同的值，在更新时会对其进行合并批量更新</font>
+ <font style="color:rgb(44, 62, 80);">合成事件中是异步</font>
+ <font style="color:rgb(44, 62, 80);">钩子函数中的是异步</font>
+ <font style="color:rgb(44, 62, 80);">原生事件中是同步</font>
+ <font style="color:rgb(44, 62, 80);">setTimeout中是同步</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781944241-de055b89-29a6-42be-b99c-4593314925d3.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781944571-852dd2fa-52cc-4827-a275-d404104068a1.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781946247-1b22ef63-85ed-4317-941d-1437d04fc071.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781948147-ec2c4dd7-7f83-45cc-9b60-6a327f839a5e.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781948274-75938ca9-f07d-4887-8809-33b35459593e.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781951203-097ce4d4-bdd5-4255-b705-c3186a07d8d7.png)

**<font style="color:rgb(44, 62, 80);">这是一道经常会出现的 React setState 笔试题：下面的代码输出什么呢？</font>**

```javascript
class Test extends React.Component {
  state  = {
    count: 0
  };

  componentDidMount() {
    this.setState({count: this.state.count + 1});
    console.log(this.state.count);

    this.setState({count: this.state.count + 1});
    console.log(this.state.count);

    setTimeout(() => {
      this.setState({count: this.state.count + 1});
      console.log(this.state.count);

      this.setState({count: this.state.count + 1});
      console.log(this.state.count);
    }, 0);
  }

  render() {
    return null;
  }
};
```

<font style="color:rgb(44, 62, 80);">我们可以进行如下的分析：</font>

+ <font style="color:rgb(44, 62, 80);">首先第一次和第二次的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">console.log</font><font style="color:rgb(44, 62, 80);">，都在 React 的生命周期事件中，所以是异步的处理方式，则输出都为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">而在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">console.log</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">处于原生事件中，所以会同步的处理再输出结果，但需要注意，虽然</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">count</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在前面经过了两次的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state.count + 1</font><font style="color:rgb(44, 62, 80);">，但是每次获取的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state.count</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">都是初始化时的值，也就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">所以此时</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">count</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">，那么后续在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">中的输出则是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">2</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">3</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">所以完整答案是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0,0,2,3</font>

**<font style="color:rgb(44, 62, 80);">同步场景</font>**

<font style="color:rgb(44, 62, 80);">异步场景中的案例使我们建立了这样一个认知：setState 是异步的，但下面这个案例又会颠覆你的认知。如果我们将 setState 放在 setTimeout 事件中，那情况就完全不同了。</font>

```javascript
class Test extends Component {
  state = {
    count: 0
  }

  componentDidMount(){
    this.setState({ count: this.state.count + 1 });
    console.log(this.state.count);
    setTimeout(() => {
      this.setState({ count: this.state.count + 1 });
      console.log("setTimeout: " + this.state.count);
    }, 0);
  }

  render(){
    ...
  }
}
```

<font style="color:rgb(44, 62, 80);">那这时输出的应该是什么呢？如果你认为是 0,0，那么又错了。</font>

<font style="color:rgb(44, 62, 80);">正确的结果是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0,2</font><font style="color:rgb(44, 62, 80);">。因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">并不是真正的异步函数，它实际上是通过队列延迟执行操作实现的，通过 isBatchingUpdates 来判断 setState 是先存进 state 队列还是直接更新。值为 true 则执行异步操作，false 则直接同步更新</font>

**<font style="color:rgb(44, 62, 80);">接下来这个案例的答案是什么呢</font>**

```javascript
class Test extends Component {
  state = {
    count: 0
  }

  componentDidMount(){
    this.setState({
      count: this.state.count + 1
    }, () => {
      console.log(this.state.count)
    })
    this.setState({
      count: this.state.count + 1
    }, () => {
      console.log(this.state.count)
    })
  }

  render(){
    ...
  }
}
```

<font style="color:rgb(44, 62, 80);">如果你觉得答案是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1,2</font><font style="color:rgb(44, 62, 80);">，那肯定就错了。这种迷惑性极强的考题在面试中非常常见，因为它反直觉。</font>

<font style="color:rgb(44, 62, 80);">如果重新仔细思考，你会发现当前拿到的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state.count</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值并没有变化，都是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">，所以输出结果应该是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1,1</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">当然，也可以在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数中获取修改后的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值进行修改。</font>

```javascript
class Test extends Component {
  state = {
    count: 0
  }

  componentDidMount(){
    this.setState(
      preState=> ({
        count:preState.count + 1
      }),()=>{
        console.log(this.state.count)
      })
    this.setState(
      preState=>({
        count:preState.count + 1
      }),()=>{
        console.log(this.state.count)
      })
  }

  render(){
    ...
  }
}
```

<font style="color:rgb(44, 62, 80);">这些通通是异步的回调，如果你以为输出结果是 1,2，那就又错了，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">实际上是 2,2</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">为什么会这样呢？当调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数时，就会</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">把当前的操作放入队列中</font><font style="color:rgb(44, 62, 80);">。React 根据队列内容，合并 state 数据，完成后再逐一执行回调，根据结果更新虚拟 DOM，触发渲染。所以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">回调时，state 已经合并计算完成了</font><font style="color:rgb(44, 62, 80);">，输出的结果就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">2,2</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">了。</font>

### [](https://www.123fe.net/docs/base/improve.html#_7-setstate%E5%8E%9F%E7%90%86%E5%88%86%E6%9E%90)<font style="color:rgb(44, 62, 80);">7 setState原理分析</font>
**<font style="color:rgb(44, 62, 80);">1. setState异步更新</font>**

+ <font style="color:rgb(44, 62, 80);">我们都知道，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state</font><font style="color:rgb(44, 62, 80);">来访问</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">，通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.setState()</font><font style="color:rgb(44, 62, 80);">方法来更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">。当</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.setState()</font><font style="color:rgb(44, 62, 80);">方法被调用的时候，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">会重新调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">方法来重新渲染</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI</font>
+ <font style="color:rgb(44, 62, 80);">首先如果直接在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">后面获取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">的值是获取不到的。在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">内部机制能检测到的地方，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">就是异步的；</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">在React</font><font style="color:rgb(44, 62, 80);">检测不到的地方，例如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">就是同步更新的</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781953308-ce386dc0-1d06-49ab-b1a1-c208bfcfa33d.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">因为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是可以接受两个参数的，一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，一个回调函数。因此我们可以在回调函数里面获取值</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781954816-82ad15b8-6d6c-4a7b-ac5b-972eddec62ba.png)

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">方法通过一个队列机制实现</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">更新，当执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">的时候，会将需要更新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">合并之后放入状态队列，而不会立即更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state</font>
+ <font style="color:rgb(44, 62, 80);">如果我们不使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">而是使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state.key</font><font style="color:rgb(44, 62, 80);">来修改，将不会触发组件的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">re-render</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">如果将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state</font><font style="color:rgb(44, 62, 80);">赋值给一个新的对象引用，那么其他不在对象上的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">将不会被放入状态队列中，当下次调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">并对状态队列进行合并时，直接造成了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">丢失</font>

**<font style="color:rgb(44, 62, 80);">1.1 setState批量更新的过程</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">生命周期和合成事件执行前后都有相应的钩子，分别是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pre</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">钩子和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">post</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">钩子，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pre</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">钩子会调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchedUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">变量置为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，开启批量更新，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">post</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">钩子会将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">置为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font><font style="color:rgb(44, 62, 80);">变量置为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">，则会走批量更新分支，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">的更新会被存入队列中，待同步代码执行完后，再执行队列中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">更新。</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">，则把当前组件（即调用了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">的组件）放入</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dirtyComponents</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数组中；否则</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所有队列中的更新</font>
+ <font style="color:rgb(44, 62, 80);">而在原生事件和异步操作中，不会执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pre</font><font style="color:rgb(44, 62, 80);">钩子，或者生命周期的中的异步操作之前执行了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pre</font><font style="color:rgb(44, 62, 80);">钩子，但是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pos</font><font style="color:rgb(44, 62, 80);">钩子也在异步操作之前执行完了，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font><font style="color:rgb(44, 62, 80);">必定为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">，也就不会进行批量更新</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781955377-90a5f2f3-b405-4176-adac-0fb812e46996.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">enqueueUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">包含了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">避免重复</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的逻辑。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountComponent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateComponent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法在执行的最开始，会调用到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchedUpdates</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">进行批处理更新，此时会将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">设置为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，也就是将状态标记为现在正处于更新阶段了。</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，则把当前组件（即调用了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的组件）放入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dirtyComponents</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">数组中；否则</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">所有队列中的更新</font>

**<font style="color:rgb(44, 62, 80);">1.2 为什么直接修改this.state无效</font>**

+ <font style="color:rgb(44, 62, 80);">要知道</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">本质是通过一个队列机制实现</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">更新的。 执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">时，会将需要更新的state合并后放入状态队列，而不会立刻更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">，队列机制可以批量更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">如果不通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">而直接修改</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state</font><font style="color:rgb(44, 62, 80);">，那么这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">不会放入状态队列中，下次调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">时对状态队列进行合并时，会忽略之前直接被修改的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">，这样我们就无法合并了，而且实际也没有把你想要的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">更新上去</font>

**<font style="color:rgb(44, 62, 80);">1.3 什么是批量更新 Batch Update</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在一些</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mv*</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">框架中，，就是将一段时间内对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">model</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的修改批量更新到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">view</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的机制。比如那前端比较火的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nextTick</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">机制,视图的更新以及实现）</font>

**<font style="color:rgb(44, 62, 80);">1.4 setState之后发生的事情</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">操作并不保证是同步的，也可以认为是异步的</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">之后，会经对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">进行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">，判断是否有改变，然后去</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff dom</font><font style="color:rgb(44, 62, 80);">决定是否要更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI</font><font style="color:rgb(44, 62, 80);">。如果这一系列过程立刻发生在每一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">之后，就可能会有性能问题</font>
+ <font style="color:rgb(44, 62, 80);">在短时间内频繁</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">会将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">的改变压入栈中，在合适的时机，批量更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">和视图，达到提高性能的效果</font>

**<font style="color:rgb(44, 62, 80);">1.5 如何知道state已经被更新</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">传入回调函数</font>

```javascript
setState({
  index: 1
}}, function(){
  console.log(this.state.index);
})
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在钩子函数中体现</font>

```javascript
componentDidUpdate(){
  console.log(this.state.index);
}
```

**<font style="color:rgb(44, 62, 80);">2. setState循环调用风险</font>**

+ <font style="color:rgb(44, 62, 80);">当调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">时，实际上会执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">enqueueSetState</font><font style="color:rgb(44, 62, 80);">方法，并对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">partialState</font><font style="color:rgb(44, 62, 80);">以及</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_pending-StateQueue</font><font style="color:rgb(44, 62, 80);">更新队列进行合并操作，最终通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">enqueueUpdate</font><font style="color:rgb(44, 62, 80);">执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">更新</font>
+ <font style="color:rgb(44, 62, 80);">而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUpdateIfNecessary</font><font style="color:rgb(44, 62, 80);">方法会获</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">取_pendingElement</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_pendingStateQueue</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_pending-ForceUpdate</font><font style="color:rgb(44, 62, 80);">，并调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">receiveComponent</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateComponent</font><font style="color:rgb(44, 62, 80);">方法进行组件更新</font>
+ <font style="color:rgb(44, 62, 80);">如果在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillUpdate</font><font style="color:rgb(44, 62, 80);">方法中调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">，此时</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this._pending-StateQueue != null</font><font style="color:rgb(44, 62, 80);">，就会造成循环调用，使得浏览器内存占满后崩溃</font>

**<font style="color:rgb(44, 62, 80);">3 事务</font>**

+ <font style="color:rgb(44, 62, 80);">事务就是将需要执行的方法使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">wrapper</font><font style="color:rgb(44, 62, 80);">封装起来，再通过事务提供的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">perform</font><font style="color:rgb(44, 62, 80);">方法执行，先执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">wrapper</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">initialize</font><font style="color:rgb(44, 62, 80);">方法，执行完</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">perform</font><font style="color:rgb(44, 62, 80);">之后，在执行所有的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">close</font><font style="color:rgb(44, 62, 80);">方法，一组</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">initialize</font><font style="color:rgb(44, 62, 80);">及</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">close</font><font style="color:rgb(44, 62, 80);">方法称为一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">wrapper</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">那么事务和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">方法的不同表现有什么关系，首先我们把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">4</font><font style="color:rgb(44, 62, 80);">次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setStat</font><font style="color:rgb(44, 62, 80);">e简单归类，前两次属于一类，因为它们在同一调用栈中执行，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">中的两次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">属于另一类</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">调用之前，已经处在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchedUpdates</font><font style="color:rgb(44, 62, 80);">执行的事务中了。那么这次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchedUpdates</font><font style="color:rgb(44, 62, 80);">方法是谁调用的呢，原来是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactMount.js</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_renderNewRootComponent</font><font style="color:rgb(44, 62, 80);">方法。也就是说，整个将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">组件渲染到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">中的过程就是处于一个大的事务中。而在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount</font><font style="color:rgb(44, 62, 80);">中调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">时，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchingStrategy</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font><font style="color:rgb(44, 62, 80);">已经被设为了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">，所以两次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">的结果没有立即生效</font>
+ <font style="color:rgb(44, 62, 80);">再反观</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">中的两次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">，因为没有前置的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchedUpdates</font><font style="color:rgb(44, 62, 80);">调用，所以导致了新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">马上生效</font>

**<font style="color:rgb(44, 62, 80);">4. 总结</font>**

+ <font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">去更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state</font><font style="color:rgb(44, 62, 80);">，不要直接操作</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state</font><font style="color:rgb(44, 62, 80);">，请把它当成不可变的</font>
+ <font style="color:rgb(44, 62, 80);">调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state</font><font style="color:rgb(44, 62, 80);">不是马上生效的，它是异步的，所以不要天真以为执行完</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">后</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state</font><font style="color:rgb(44, 62, 80);">就是最新的值了</font>
+ <font style="color:rgb(44, 62, 80);">多个顺序执行的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">不是同步地一个一个执行滴，会一个一个加入队列，然后最后一起执行，即批处理</font>

### [](https://www.123fe.net/docs/base/improve.html#_8-react%E4%BA%8B%E5%8A%A1%E6%9C%BA%E5%88%B6)<font style="color:rgb(44, 62, 80);">8 React事务机制</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781956010-ec5cdeb2-3f68-47cf-ae49-bda18fd72c2b.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781957823-76ae654f-299e-42f9-9fd9-c590f1c9ad57.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781958427-16a84e88-47b7-421f-9a55-a889edaa8b24.png)

### [](https://www.123fe.net/docs/base/improve.html#_9-react%E7%BB%84%E4%BB%B6%E5%92%8C%E6%B8%B2%E6%9F%93%E6%9B%B4%E6%96%B0%E8%BF%87%E7%A8%8B)<font style="color:rgb(44, 62, 80);">9 React组件和渲染更新过程</font>
**<font style="color:rgb(44, 62, 80);">渲染和更新过程</font>**

+ <font style="color:rgb(44, 62, 80);">jsx如何渲染为页面</font>
+ <font style="color:rgb(44, 62, 80);">setState之后如何更新页面</font>
+ <font style="color:rgb(44, 62, 80);">面试考察全流程</font>

**<font style="color:rgb(44, 62, 80);">JSX本质和vdom</font>**

+ <font style="color:rgb(44, 62, 80);">JSX即</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">函数</font>
+ <font style="color:rgb(44, 62, 80);">执行生成vnode</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch(elem,vnode)</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch(vnode,newNode)</font>

**<font style="color:rgb(44, 62, 80);">组件渲染过程</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props state</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render()</font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch(elem, vnode)</font>

**<font style="color:rgb(44, 62, 80);">组件更新过程</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState-->dirtyComponents</font><font style="color:rgb(44, 62, 80);">(可能有子组件)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newVnode</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch(vnode, newVnode)</font>

### [](https://www.123fe.net/docs/base/improve.html#_10-%E5%A6%82%E4%BD%95%E8%A7%A3%E9%87%8A-react-%E7%9A%84%E6%B8%B2%E6%9F%93%E6%B5%81%E7%A8%8B)<font style="color:rgb(44, 62, 80);">10 如何解释 React 的渲染流程</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781959493-60790ebd-1668-49f6-810c-26f437330792.png)

+ <font style="color:rgb(44, 62, 80);">React 的渲染过程大致一致，但协调并不相同，以</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 16</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为分界线，分为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Stack Reconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber Reconciler</font><font style="color:rgb(44, 62, 80);">。这里的协调从狭义上来讲，特指 React 的 diff 算法，广义上来讲，有时候也指 React 的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">模块，它通常包含了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">算法和一些公共逻辑。</font>
+ <font style="color:rgb(44, 62, 80);">回到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Stack Reconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Stack Reconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">核心调度方式是递归</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">调度的基本处理单位是事务</font><font style="color:rgb(44, 62, 80);">，它的事务基类是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Transaction</font><font style="color:rgb(44, 62, 80);">，这里的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">事务是 React 团队从后端开发中加入的概念</font><font style="color:rgb(44, 62, 80);">。在 React 16 以前，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">挂载主要通过 ReactMount 模块完成</font><font style="color:rgb(44, 62, 80);">，更新通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">模块完成，模块之间相互分离，落脚执行点也是事务。</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 16</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">及以后，协调改为了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber Reconciler</font><font style="color:rgb(44, 62, 80);">。它的调度方式主要有两个特点，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">第一个是协作式多任务模式</font><font style="color:rgb(44, 62, 80);">，在这个模式下，线程会定时放弃自己的运行权利，交还给主线程，通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实现。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">第二个特点是策略优先级</font><font style="color:rgb(44, 62, 80);">，调度任务通过标记</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的方式分优先级执行，比如动画，或者标记为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">high</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的任务可以优先执行。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber Reconciler</font><font style="color:rgb(44, 62, 80);">的基本单位是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">基于过去的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Element</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">提供了二次封装，提供了指向父、子、兄弟节点的引用，为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">工作的双链表实现提供了基础。</font>
+ <font style="color:rgb(44, 62, 80);">在新的架构下，整个生命周期被划分为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Render 和 Commit 两个阶段</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Render 阶段的执行特点是可中断、可停止、无副作用</font><font style="color:rgb(44, 62, 80);">，主要是通过构造</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树计算出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">。以</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树为基础，将每个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);">作为一个基本单位，自下而上逐个节点检查并构造 workInProgress 树。这个过程不再是递归，而是基于循环来完成</font>
+ <font style="color:rgb(44, 62, 80);">在执行上通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来调度执行每组任务，每组中的每个计算任务被称为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">work</font><font style="color:rgb(44, 62, 80);">，每个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">work</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">完成后确认是否有优先级更高的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">work</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">需要插入，如果有就让位，没有就继续。优先级通常是标记为动画或者</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">high</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的会先处理。每完成一组后，将调度权交回主线程，直到下一次</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">调用，再继续构建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commit</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">阶段需要处理</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">effect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">列表，这里的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">effect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">列表包含了根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff 更新 DOM 树</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">回调生命周期</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">响应 ref</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等。</font>
+ <font style="color:rgb(44, 62, 80);">但一定要注意，这个阶段是同步执行的，不可中断暂停，所以不要在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidUpdate</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWiilUnmount</font><font style="color:rgb(44, 62, 80);">中去执行重度消耗算力的任务</font>
+ <font style="color:rgb(44, 62, 80);">如果只是一般的应用场景，比如管理后台、H5 展示页等，两者性能差距并不大，但在动画、画布及手势等场景下，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Stack Reconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的设计会占用占主线程，造成卡顿，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiber reconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的设计则能带来高性能的表现</font>

### [](https://www.123fe.net/docs/base/improve.html#_11-diff%E7%AE%97%E6%B3%95%E6%98%AF%E6%80%8E%E4%B9%88%E8%BF%90%E4%BD%9C)<font style="color:rgb(44, 62, 80);">11 diff算法是怎么运作</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">每一种节点类型有自己的属性，也就是prop，每次进行diff的时候，react会先比较该节点类型，假如节点类型不一样，那么react会直接删除该节点，然后直接创建新的节点插入到其中，假如节点类型一样，那么会比较prop是否有更新，假如有prop不一样，那么react会判定该节点有更新，那么重渲染该节点，然后在对其子节点进行比较，一层一层往下，直到没有子节点</font>

+ <font style="color:rgb(44, 62, 80);">把树形结构按照层级分解，只比较同级元素。</font>
+ <font style="color:rgb(44, 62, 80);">给列表结构的每个单元添加唯一的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">属性，方便比较。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">只会匹配相同</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">component</font><font style="color:rgb(44, 62, 80);">（这里面的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);">指的是组件的名字）</font>
+ <font style="color:rgb(44, 62, 80);">合并操作，调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">component</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的时候,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将其标记为 -</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dirty</font><font style="color:rgb(44, 62, 80);">.到每一个事件循环结束,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">检查所有标记</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dirty</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">component</font><font style="color:rgb(44, 62, 80);">重新绘制.</font>
+ <font style="color:rgb(44, 62, 80);">选择性子树渲染。开发人员可以重写</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);">提高</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">的性能</font>

**<font style="color:rgb(44, 62, 80);">优化</font>****<font style="color:rgb(44, 62, 80);">⬇️</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为了降低算法复杂度，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会预设三个限制：</font>

1. <font style="color:rgb(44, 62, 80);">只对同级元素进行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Diff</font><font style="color:rgb(44, 62, 80);">。如果一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM节点</font><font style="color:rgb(44, 62, 80);">在前后两次更新中跨越了层级，那么</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">不会尝试复用他。</font>
2. <font style="color:rgb(44, 62, 80);">两个不同类型的元素会产生出不同的树。如果元素由</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">变为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p</font><font style="color:rgb(44, 62, 80);">，React会销毁</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">及其子孙节点，并新建</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p</font><font style="color:rgb(44, 62, 80);">及其子孙节点。</font>
3. <font style="color:rgb(44, 62, 80);">开发者可以通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key prop</font><font style="color:rgb(44, 62, 80);">来暗示哪些子元素在不同的渲染下能保持稳定。考虑如下例子：</font>

**<font style="color:rgb(44, 62, 80);">Diff的思路</font>**

<font style="color:rgb(44, 62, 80);">该如何设计算法呢？如果让我设计一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Diff算法</font><font style="color:rgb(44, 62, 80);">，我首先想到的方案是：</font>

1. <font style="color:rgb(44, 62, 80);">判断当前节点的更新属于哪种情况</font>
2. <font style="color:rgb(44, 62, 80);">如果是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">新增</font><font style="color:rgb(44, 62, 80);">，执行新增逻辑</font>
3. <font style="color:rgb(44, 62, 80);">如果是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">删除</font><font style="color:rgb(44, 62, 80);">，执行删除逻辑</font>
4. <font style="color:rgb(44, 62, 80);">如果是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">更新</font><font style="color:rgb(44, 62, 80);">，执行更新逻辑</font>
+ <font style="color:rgb(44, 62, 80);">按这个方案，其实有个隐含的前提——</font>**<font style="color:rgb(44, 62, 80);">不同操作的优先级是相同的</font>**
+ <font style="color:rgb(44, 62, 80);">但是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React团队</font><font style="color:rgb(44, 62, 80);">发现，在日常开发中，相较于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">新增</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">删除</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">更新</font><font style="color:rgb(44, 62, 80);">组件发生的频率更高。所以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Diff</font><font style="color:rgb(44, 62, 80);">会优先判断当前节点是否属于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">更新</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">基于以上原因，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Diff算法</font><font style="color:rgb(44, 62, 80);">的整体逻辑会经历两轮遍历：</font>

+ <font style="color:rgb(44, 62, 80);">第一轮遍历：处理</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">更新</font><font style="color:rgb(44, 62, 80);">的节点。</font>
+ <font style="color:rgb(44, 62, 80);">第二轮遍历：处理剩下的不属于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">更新</font><font style="color:rgb(44, 62, 80);">的节点。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781960420-10a9a4f4-d0d9-41bd-9882-7b3fc9b1ee60.png)

**<font style="color:rgb(44, 62, 80);">diff算法的作用</font>**

<font style="color:rgb(44, 62, 80);">计算出Virtual DOM中真正变化的部分，并只针对该部分进行原生DOM操作，而非重新渲染整个页面。</font>

**<font style="color:rgb(44, 62, 80);">传统diff算法</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">通过循环递归对节点进行依次对比，算法复杂度达到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">O(n^3)</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，n是树的节点数，这个有多可怕呢？——如果要展示1000个节点，得执行上亿次比较。。即便是CPU快能执行30亿条命令，也很难在一秒内计算出差异。</font>

**<font style="color:rgb(44, 62, 80);">React的diff算法</font>**

1. <font style="color:rgb(44, 62, 80);">什么是调和？</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">将Virtual DOM树转换成actual DOM树的最少操作的过程 称为 调和 。</font>

1. <font style="color:rgb(44, 62, 80);">什么是React diff算法？</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">算法是调和的具体实现。</font>

**<font style="color:rgb(44, 62, 80);">diff策略</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">React用 三大策略 将O(n^3)复杂度 转化为 O(n)复杂度</font>

**<font style="color:rgb(44, 62, 80);">策略一（tree diff）：</font>**

+ <font style="color:rgb(44, 62, 80);">Web UI中DOM节点跨层级的移动操作特别少，可以忽略不计。</font>

**<font style="color:rgb(44, 62, 80);">策略二（component diff）：</font>**

+ <font style="color:rgb(44, 62, 80);">拥有相同类的两个组件 生成相似的树形结构，</font>
+ <font style="color:rgb(44, 62, 80);">拥有不同类的两个组件 生成不同的树形结构。</font>

**<font style="color:rgb(44, 62, 80);">策略三（element diff）：</font>**

<font style="color:rgb(44, 62, 80);">对于同一层级的一组子节点，通过唯一id区分。</font>

**<font style="color:rgb(44, 62, 80);">tree diff</font>**

+ <font style="color:rgb(44, 62, 80);">React通过updateDepth对Virtual DOM树进行层级控制。</font>
+ <font style="color:rgb(44, 62, 80);">对树分层比较，两棵树 只对同一层次节点 进行比较。如果该节点不存在时，则该节点及其子节点会被完全删除，不会再进一步比较。</font>
+ <font style="color:rgb(44, 62, 80);">只需遍历一次，就能完成整棵DOM树的比较。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781959209-85793751-f3e1-4d26-819f-de029c4ab137.png)

<font style="color:rgb(44, 62, 80);">那么问题来了，如果DOM节点出现了跨层级操作,diff会咋办呢？</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">答：diff只简单考虑同层级的节点位置变换，如果是跨层级的话，只有创建节点和删除节点的操作。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781961726-9f6653bf-87e6-4eab-83a5-8629f137c216.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如上图所示，以A为根节点的整棵树会被重新创建，而不是移动，因此 官方建议不要进行DOM节点跨层级操作，可以通过CSS隐藏、显示节点，而不是真正地移除、添加DOM节点</font>

**<font style="color:rgb(44, 62, 80);">component diff</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">React对不同的组件间的比较，有三种策略</font>

1. <font style="color:rgb(44, 62, 80);">同一类型的两个组件，按原策略（层级比较）继续比较Virtual DOM树即可。</font>
2. <font style="color:rgb(44, 62, 80);">同一类型的两个组件，组件A变化为组件B时，可能Virtual DOM没有任何变化，如果知道这点（变换的过程中，Virtual DOM没有改变），可节省大量计算时间，所以 用户 可以通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来判断是否需要 判断计算。</font>
3. <font style="color:rgb(44, 62, 80);">不同类型的组件，将一个（将被改变的）组件判断为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dirty component</font><font style="color:rgb(44, 62, 80);">（脏组件），从而替换 整个组件的所有节点。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">注意：如果组件D和组件G的结构相似，但是 React判断是 不同类型的组件，则不会比较其结构，而是删除 组件D及其子节点，创建组件G及其子节点。</font>

**<font style="color:rgb(44, 62, 80);">element diff</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当节点处于同一层级时，diff提供三种节点操作：删除、插入、移动。</font>

+ <font style="color:rgb(44, 62, 80);">插入：组件 C 不在集合（A,B）中，需要插入</font>
+ <font style="color:rgb(44, 62, 80);">删除：</font>
    - <font style="color:rgb(44, 62, 80);">组件 D 在集合（A,B,D）中，但 D的节点已经更改，不能复用和更新，所以需要删除 旧的 D ，再创建新的。</font>
    - <font style="color:rgb(44, 62, 80);">组件 D 之前在 集合（A,B,D）中，但集合变成新的集合（A,B）了，D 就需要被删除。</font>
+ <font style="color:rgb(44, 62, 80);">移动：组件D已经在集合（A,B,C,D）里了，且集合更新时，D没有发生更新，只是位置改变，如新集合（A,D,B,C），D在第二个，无须像传统diff，让旧集合的第二个B和新集合的第二个D 比较，并且删除第二个位置的B，再在第二个位置插入D，而是 （对同一层级的同组子节点） 添加唯一key进行区分，移动即��。</font>

**<font style="color:rgb(44, 62, 80);">总结</font>**

1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tree diff</font><font style="color:rgb(44, 62, 80);">：只对比同一层的 dom 节点，忽略 dom 节点的跨层级移动</font>

<font style="color:rgb(44, 62, 80);">如下图，react 只会对相同颜色方框内的 DOM 节点进行比较，即同一个父节点下的所有子节点。当发现节点不存在时，则该节点及其子节点会被完全删除掉，不会用于进一步的比较。</font>

<font style="color:rgb(44, 62, 80);">这样只需要对树进行一次遍历，便能完成整个 DOM 树的比较。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781964237-d239cb18-e024-4407-a8ff-13cfa8efb1c5.png)

<font style="color:rgb(44, 62, 80);">这就意味着，如果 dom 节点发生了跨层级移动，react 会删除旧的节点，生成新的节点，而不会复用。</font>

1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">component diff</font><font style="color:rgb(44, 62, 80);">：如果不是同一类型的组件，会删除旧的组件，创建新的组件</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781963545-8036dada-60b4-4a7d-b74d-3ed35ec1b414.png)

1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">element diff</font><font style="color:rgb(44, 62, 80);">：对于同一层级的一组子节点，需要通过唯一 id 进行来区分</font>
+ <font style="color:rgb(44, 62, 80);">如果没有 id 来进行区分，一旦有插入动作，会导致插入位置之后的列表全部重新渲染</font>
+ <font style="color:rgb(44, 62, 80);">这也是为什么渲染列表时为什么要使用唯一的 key。</font>

**<font style="color:rgb(44, 62, 80);">diff的不足与待优化的地方</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">尽量减少类似将最后一个节点移动到列表首部的操作，当节点数量过大或更新操作过于频繁时，会影响React的渲染性能</font>

**<font style="color:rgb(44, 62, 80);">与其他框架相比，React 的 diff 算法有何不同？</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781964283-b80ce4b9-0aae-4d9a-9a3d-52f7334f8dae.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">diff 算法探讨的就是虚拟 DOM 树发生变化后，生成 DOM 树更新补丁的方式。它通过对比新旧两株虚拟 DOM 树的变更差异，将更新补丁作用于真实 DOM，以最小成本完成视图更新</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781964952-0f26c5ba-7c13-41f3-87fe-445d90d0db9a.png)

<font style="color:rgb(44, 62, 80);">具体的流程是这样的：</font>

+ <font style="color:rgb(44, 62, 80);">真实 DOM 与虚拟 DOM 之间存在一个映射关系。这个映射关系依靠初始化时的 JSX 建立完成；</font>
+ <font style="color:rgb(44, 62, 80);">当虚拟 DOM 发生变化后，就会根据差距计算生成 patch，这个 patch 是一个结构化的数据，内容包含了增加、更新、移除等；</font>
+ <font style="color:rgb(44, 62, 80);">最后再根据 patch 去更新真实的 DOM，反馈到用户的界面上。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781965441-4b318b40-f242-42e0-8b58-cc4ecad2cad8.png)

<font style="color:rgb(44, 62, 80);">在回答有何不同之前，首先需要说明下什么是 diff 算法。</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff 算法是指生成更新补丁的方式</font><font style="color:rgb(44, 62, 80);">，主要应用于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">虚拟 DOM 树变化后，更新真实 DOM</font><font style="color:rgb(44, 62, 80);">。所以 diff 算法一定存在这样一个过程：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">触发更新 → 生成补丁 → 应用补丁</font>
+ <font style="color:rgb(44, 62, 80);">React 的 diff 算法，触发更新的时机主要在 state 变化与 hooks 调用之后。此时触发虚拟 DOM 树变更遍历，采用了深度优先遍历算法。但传统的遍历方式，效率较低。为了优化效率，使用了分治的方式。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">将单一节点比对转化为了 3 种类型节点的比对</font><font style="color:rgb(44, 62, 80);">，分别是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">树、组件及元素</font><font style="color:rgb(44, 62, 80);">，以此提升效率。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">树比对</font><font style="color:rgb(44, 62, 80);">：由于网页视图中较少有跨层级节点移动，两株虚拟 DOM 树只对同一层次的节点进行比较。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">组件比对</font><font style="color:rgb(44, 62, 80);">：如果组件是同一类型，则进行树比对，如果不是，则直接放入到补丁中。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">元素比对</font><font style="color:rgb(44, 62, 80);">：主要发生在同层级中，通过标记节点操作生成补丁，节点操作对应真实的 DOM 剪裁操作。同一层级的子节点，可以通过标记 key 的方式进行列表对比。</font>
+ <font style="color:rgb(44, 62, 80);">以上是经典的 React diff 算法内容。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">自 React 16 起，引入了 Fiber 架构</font><font style="color:rgb(44, 62, 80);">。为了使整个更新过程</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">可随时暂停恢复</font><font style="color:rgb(44, 62, 80);">，节点与树分别采用了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FiberNode 与 FiberTree 进行重构</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiberNode 使用了双链表的结构</font><font style="color:rgb(44, 62, 80);">，可以直接找到兄弟节点与子节点</font>
+ <font style="color:rgb(44, 62, 80);">然后拿 Vue 和 Preact 与 React 的 diff 算法进行对比</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Preact</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">算法相较于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">，整体设计思路相似，但最底层的元素采用了真实</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对比操作，也没有采用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">设计。Vue 的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">算法整体也与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">相似，同样未实现</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">设计</font>
+ <font style="color:rgb(44, 62, 80);">然后进行横向比较，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 拥有完整的 Diff 算法策略，且拥有随时中断更新的时间切片能力</font><font style="color:rgb(44, 62, 80);">，在大批量节点更新的极端情况下，拥有更友好的交互体验。</font>
+ <font style="color:rgb(44, 62, 80);">Preact 可以在一些对性能要求不高，仅需要渲染框架的简单场景下应用。</font>
+ <font style="color:rgb(44, 62, 80);">Vue 的整体</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff 策略与 React 对齐</font><font style="color:rgb(44, 62, 80);">，虽然缺乏时间切片能力，但这并不意味着 Vue 的性能更差，因为在 Vue 3 初期引入过，后期因为收益不高移除掉了。除了高帧率动画，在 Vue 中其他的场景几乎都可以使用防抖和节流去提高响应性能。</font>

<font style="color:rgb(44, 62, 80);">**学习原理的目的就是应用。那如何根据 React diff 算法原理优化代码呢？**这个问题其实按优化方式逆向回答即可。</font>

+ <font style="color:rgb(44, 62, 80);">根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">算法的设计原则，应尽量避免跨层级节点移动。</font>
+ <font style="color:rgb(44, 62, 80);">通过设置唯一</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">进行优化，尽量减少组件层级深度。因为过深的层级会加深遍历深度，带来性能问题。</font>
+ <font style="color:rgb(44, 62, 80);">设置</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.pureComponet</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">减少</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">次数。</font>

### [](https://www.123fe.net/docs/base/improve.html#_12-%E5%90%88%E6%88%90%E4%BA%8B%E4%BB%B6%E5%8E%9F%E7%90%86)<font style="color:rgb(44, 62, 80);">12 合成事件原理</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为了解决跨浏览器兼容性问题，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会将浏览器原生事件（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Browser Native Event</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）封装为合成事件（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SyntheticEvent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）传入设置的事件处理器中。这里的合成事件提供了与原生事件相同的接口，不过它们屏蔽了底层浏览器的细节差异，保证了行为的一致性。另外有意思的是，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">并没有直接将事件附着到子元素上，而是以单一事件监听器的方式将所有的事件发送到顶层进行处理。这样</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在更新</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的时候就不需要考虑如何去处理附着在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上的事件监听器，最终达到优化性能的目的</font>

+ <font style="color:rgb(44, 62, 80);">所有的事件挂在document上，DOM 事件触发后冒泡到 document；React 找到对应的组件，造出一个合成事件出来；并按组件树模拟一遍事件冒泡。</font>
+ <font style="color:rgb(44, 62, 80);">event不是原生的，是SyntheticEvent合成事件对象</font>
+ <font style="color:rgb(44, 62, 80);">和Vue事件不同,和DOM事件也不同</font>

**<font style="color:rgb(44, 62, 80);">React 17 之前的事件冒泡流程图</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781966672-c8ac22f4-8895-45f3-8f69-e37e909860e6.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">所以这就造成了，在一个页面中，只能有一个版本的 React。如果有多个版本，事件就乱套了。值得一提的是，这个问题在 React 17 中得到了解决，事件委托不再挂在 document 上，而是挂在 DOM 容器上，也就是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDom.Render</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">所调用的节点上。</font>

**<font style="color:rgb(44, 62, 80);">React 17 后的事件冒泡流程图</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781965751-db4ecc4c-25dc-483f-a44c-5c0577218725.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">那到底哪些事件会被捕获生成合成事件呢？可以从 React 的源码测试文件中一探究竟。下面的测试快照中罗列了大量的事件名，也只有在这份快照中的事件，才会被捕获生成合成事件。</font>

```plain
// react/packages/react-dom/src/__tests__/__snapshots__/ReactTestUtils-test.js.snap
Array [
	  "abort",
	  "animationEnd",
	  "animationIteration",
	  "animationStart",
	  "auxClick",
	  "beforeInput",
	  "blur",
	  "canPlay",
	  "canPlayThrough",
	  "cancel",
	  "change",
	  "click",
	  "close",
	  "compositionEnd",
	  "compositionStart",
	  "compositionUpdate",
	  "contextMenu",
	  "copy",
	  "cut",
	  "doubleClick",
	  "drag",
	  "dragEnd",
	  "dragEnter",
	  "dragExit",
	  "dragLeave",
	  "dragOver",
	  "dragStart",
	  "drop",
	  "durationChange",
	  "emptied",
	  "encrypted",
	  "ended",
	  "error",
	  "focus",
	  "gotPointerCapture",
	  "input",
	  "invalid",
	  "keyDown",
	  "keyPress",
	  "keyUp",
	  "load",
	  "loadStart",
	  "loadedData",
	  "loadedMetadata",
	  "lostPointerCapture",
	  "mouseDown",
	  "mouseEnter",
	  "mouseLeave",
	  "mouseMove",
	  "mouseOut",
	  "mouseOver",
	  "mouseUp",
	  "paste",
	  "pause",
	  "play",
	  "playing",
	  "pointerCancel",
	  "pointerDown",
	  "pointerEnter",
	  "pointerLeave",
	  "pointerMove",
	  "pointerOut",
	  "pointerOver",
	  "pointerUp",
	  "progress",
	  "rateChange",
	  "reset",
	  "scroll",
	  "seeked",
	  "seeking",
	  "select",
	  "stalled",
	  "submit",
	  "suspend",
	  "timeUpdate",
	  "toggle",
	  "touchCancel",
	  "touchEnd",
	  "touchMove",
	  "touchStart",
	  "transitionEnd",
	  "volumeChange",
	  "waiting",
	  "wheel",
	]
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如果DOM上绑定了过多的事件处理函数,整个页面响应以及内存占用可能都会受到影响。React为了避免这类DOM事件滥用,同时屏蔽底层不同浏览器之间的事件系统的差异,实现了一个中间层 - SyntheticEvent</font>

1. <font style="color:rgb(44, 62, 80);">当用户在为onClick添加函数时,React并没有将Click绑定到DOM上面</font>
2. <font style="color:rgb(44, 62, 80);">而是在document处监听所有支持的事件,当事件发生并冒泡至document处时,React将事件内容封装交给中间层 SyntheticEvent (负责所有事件合成)</font>
3. <font style="color:rgb(44, 62, 80);">所以当事件触发的时候, 对使用统一的分发函数 dispatchEvent 将指定函数执行</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781967732-4110baa5-78d4-4c00-acf2-042c6545d9d5.png)

**<font style="color:rgb(44, 62, 80);">为何要合成事件</font>**

+ <font style="color:rgb(44, 62, 80);">兼容性和跨平台</font>
+ <font style="color:rgb(44, 62, 80);">挂在统一的document上，减少内存消耗，避免频繁解绑</font>
+ <font style="color:rgb(44, 62, 80);">方便事件的统一管理（事务机制）</font>
+ <font style="color:rgb(44, 62, 80);">dispatchEvent事件机制</font>

### [](https://www.123fe.net/docs/base/improve.html#_13-jsx%E8%AF%AD%E6%B3%95%E7%B3%96%E6%9C%AC%E8%B4%A8)<font style="color:rgb(44, 62, 80);">13 JSX语法糖本质</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">JSX是语法糖，通过babel转成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数，在babel官网上可以在线把JSX转成React的JS语法</font>

+ <font style="color:rgb(44, 62, 80);">首先解析出来的话，就是一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">函数</font>
+ <font style="color:rgb(44, 62, 80);">然后这个函数执行完后，会返回一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font>
+ <font style="color:rgb(44, 62, 80);">通过vdom的patch或者是其他的一个方法，最后渲染一个页面</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781967283-2ebc7034-5a13-4944-a6e6-b3281c508a5d.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781967956-43022ab1-c849-497e-8e5d-1ae5f70d4c72.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">script标签中不添加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text/babel</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">解析jsx语法的情况下</font>

```html
<script>
  const ele = React.createElement("h2", null, "Hello React!");
  ReactDOM.render(ele, document.getElementById("app"));
</script>
```

**<font style="color:rgb(44, 62, 80);">JSX的本质是React.createElement()函数</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781969081-588cd8dd-bf1e-48e3-a0d2-6f3eb3a0c983.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数返回的对象是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactEelement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">的写法如下</font>

```plain
class App extends React.Component {
  constructor() {
    super()
    this.state = {}
  }

  render() {
    return React.createElement("div", null,
        /*第一个子元素，header*/
        React.createElement("div", { className: "header" },
                            React.createElement("h1", { title: "\u6807\u9898" }, "\u6211\u662F\u6807\u9898")
                          ),
        /*第二个子元素，content*/
        React.createElement("div", { className: "content" },
                            React.createElement("h2", null, "\u6211\u662F\u9875\u9762\u7684\u5185\u5BB9"),
                            React.createElement("button", null, "\u6309\u94AE"),
                            React.createElement("button", null, "+1"),
                            React.createElement("a", { href: "http://www.baidu.com" },
                                                "\u767E\u5EA6\u4E00\u4E0B")
                          ),
        /*第三个子元素，footer*/
        React.createElement("div", { className: "footer" },
                            React.createElement("p", null, "\u6211\u662F\u5C3E\u90E8\u7684\u5185\u5BB9")
                          )
      );
  }
}

ReactDOM.render(<App />, document.getElementById("app"));
```

<font style="color:rgb(44, 62, 80);">实际开发中不会使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">来创建</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(44, 62, 80);">的，一般都是使用JSX的形式开发。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(44, 62, 80);">在程序中打印一下</font>

```plain
render() {
  let ele = (
    <div>
      <div className="header">
        <h1 title="标题">我是标题</h1>
      </div>
      <div className="content">
        <h2>我是页面的内容</h2>
        <button>按钮</button>
        <button>+1</button>
        <a href="http://www.baidu.com">百度一下</a>
      </div>
      <div className="footer">
        <p>我是尾部的内容</p>
      </div>
    </div>
  )
  console.log(ele);
  return ele;
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781969558-2a79e82d-7833-4d3e-96ee-fe2fa28de745.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">react通过babel把JSX转成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数，生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象，然后通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render函</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">数把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">渲染成真实的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">元素</font>

**<font style="color:rgb(44, 62, 80);">为什么 React 使用 JSX</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781971494-352d8005-5577-4a40-a3e9-2c2045086f0c.png)

+ <font style="color:rgb(44, 62, 80);">在回答问题之前，我首先解释下什么是 JSX 吧。JSX 是一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的语法扩展，结构类似 XML。</font>
+ <font style="color:rgb(44, 62, 80);">JSX 主要用于声明</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素，但 React 中并不强制使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);">。即使使用了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);">，也会在构建过程中，通过 Babel 插件编译为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);">。所以 JSX 更像是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的一种语法糖</font>
+ <font style="color:rgb(44, 62, 80);">接下来与 JSX 以外的三种技术方案进行对比</font>
    - <font style="color:rgb(44, 62, 80);">首先是模板，React 团队认为模板不应该是开发过程中的关注点，因为引入了模板语法、模板指令等概念，是一种不佳的实现方案</font>
    - <font style="color:rgb(44, 62, 80);">其次是模板字符串，模板字符串编写的结构会造成多次内部嵌套，使整个结构变得复杂，并且优化代码提示也会变得困难重重</font>
    - <font style="color:rgb(44, 62, 80);">所以 React 最后选用了 JSX，因为 JSX 与其设计思想贴合，不需要引入过多新的概念，对编辑器的代码提示也极为友好。</font>

**<font style="color:rgb(44, 62, 80);">Babel 插件如何实现 JSX 到 JS 的编译？ 在 React 面试中，这个问题很容易被追问，也经常被要求手写。</font>**

<font style="color:rgb(44, 62, 80);">它的实现原理是这样的。Babel 读取代码并解析，生成 AST，再将 AST 传入插件层进行转换，在转换时就可以将 JSX 的结构转换为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的函数。如下代码所示：</font>

```plain
module.exports = function (babel) {
  var t = babel.types;
  return {
    name: "custom-jsx-plugin",
    visitor: {
      JSXElement(path) {
        var openingElement = path.node.openingElement;
        var tagName = openingElement.name.name;
        var args = []; 
        args.push(t.stringLiteral(tagName)); 
        var attribs = t.nullLiteral(); 
        args.push(attribs); 
        var reactIdentifier = t.identifier("React"); //object
        var createElementIdentifier = t.identifier("createElement"); 
        var callee = t.memberExpression(reactIdentifier, createElementIdentifier)
        var callExpression = t.callExpression(callee, args);
        callExpression.arguments = callExpression.arguments.concat(path.node.children);
        path.replaceWith(callExpression, path.node); 
      },
    },
  };
};
```

**<font style="color:rgb(44, 62, 80);">React.createElement源码分析</font>**

```plain
/**
 101. React的创建元素方法
 */
export function createElement(type, config, children) {
  // propName 变量用于储存后面需要用到的元素属性
  let propName; 
  // props 变量用于储存元素属性的键值对集合
  const props = {}; 
  // key、ref、self、source 均为 React 元素的属性，此处不必深究
  let key = null;
  let ref = null; 
  let self = null; 
  let source = null; 

  // config 对象中存储的是元素的属性
  if (config != null) { 
    // 进来之后做的第一件事，是依次对 ref、key、self 和 source 属性赋值
    if (hasValidRef(config)) {
      ref = config.ref;
    }
    // 此处将 key 值字符串化
    if (hasValidKey(config)) {
      key = '' + config.key; 
    }
    self = config.__self === undefined ? null : config.__self;
    source = config.__source === undefined ? null : config.__source;
    // 接着就是要把 config 里面的属性都一个一个挪到 props 这个之前声明好的对象里面
    for (propName in config) {
      if (
        // 筛选出可以提进 props 对象里的属性
        hasOwnProperty.call(config, propName) &&
        !RESERVED_PROPS.hasOwnProperty(propName) 
      ) {
        props[propName] = config[propName]; 
      }
    }
  }
  // childrenLength 指的是当前元素的子元素的个数，减去的 2 是 type 和 config 两个参数占用的长度
  const childrenLength = arguments.length - 2; 
  // 如果抛去type和config，就只剩下一个参数，一般意味着文本节点出现了
  if (childrenLength === 1) { 
    // 直接把这个参数的值赋给props.children
    props.children = children; 
    // 处理嵌套多个子元素的情况
  } else if (childrenLength > 1) { 
    // 声明一个子元素数组
    const childArray = Array(childrenLength); 
    // 把子元素推进数组里
    for (let i = 0; i < childrenLength; i++) { 
      childArray[i] = arguments[i + 2];
    }
    // 最后把这个数组赋值给props.children
    props.children = childArray; 
  } 

  // 处理 defaultProps
  if (type && type.defaultProps) {
    const defaultProps = type.defaultProps;
    for (propName in defaultProps) { 
      if (props[propName] === undefined) {
        props[propName] = defaultProps[propName];
      }
    }
  }

  // 最后返回一个调用ReactElement执行方法，并传入刚才处理过的参数
  return ReactElement(
    type,
    key,
    ref,
    self,
    source,
    ReactCurrentOwner.current,
    props,
  );
}
```

**<font style="color:rgb(44, 62, 80);">入参解读：创造一个元素需要知道哪些信息</font>**

```plain
export function createElement(type, config, children)
```

<font style="color:rgb(44, 62, 80);">createElement 有 3 个入参，这 3 个入参囊括了 React 创建一个元素所需要知道的全部信息。</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">type</font><font style="color:rgb(44, 62, 80);">：用于标识节点的类型。它可以是类似“h1”“div”这样的标准 HTML 标签字符串，也可以是 React 组件类型或</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React fragment</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">config</font><font style="color:rgb(44, 62, 80);">：以对象形式传入，组件所有的属性都会以键值对的形式存储在 config 对象中。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">：以对象形式传入，它记录的是组件标签之间嵌套的内容，也就是所谓的“子节点”“子元素”</font>

```plain
React.createElement("ul", {
  // 传入属性键值对
  className: "list"
   // 从第三个入参开始往后，传入的参数都是 children
}, React.createElement("li", {
  key: "1"
}, "1"), React.createElement("li", {
  key: "2"
}, "2"));
```

<font style="color:rgb(44, 62, 80);">这个调用对应的 DOM 结构如下：</font>

```html
<ul className="list">
  <li key="1">1</li>
  <li key="2">2</li>
</ul>
```

**<font style="color:rgb(44, 62, 80);">createElement 函数体拆解</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781971252-e4aab08d-8341-4f34-8e4a-6aec9df3c141.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">createElement 中并没有十分复杂的涉及算法或真实 DOM 的逻辑，它的每一个步骤几乎都是在格式化数据。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781973477-ed0d2eb5-5e15-41b6-ba11-daaed43f059e.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">现在看来，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">原来只是个“参数中介”。此时我们的注意力自然而然地就聚焦在了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上</font>

**<font style="color:rgb(44, 62, 80);">出参解读：初识虚拟 DOM</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">执行到最后会 return 一个针对 ReactElement 的调用。这里关于 ReactElement，我依然先给出源码 + 注释形式的解析</font>

```plain
const ReactElement = function(type, key, ref, self, source, owner, props) {
  const element = {
    // REACT_ELEMENT_TYPE是一个常量，用来标识该对象是一个ReactElement
    $$typeof: REACT_ELEMENT_TYPE,

    // 内置属性赋值
    type: type,
    key: key,
    ref: ref,
    props: props,

    // 记录创造该元素的组件
    _owner: owner,
  };

  // 
  if (__DEV__) {
    // 这里是一些针对 __DEV__ 环境下的处理，对于大家理解主要逻辑意义不大，此处我直接省略掉，以免混淆视听
  }

  return element;
};
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">其实只做了一件事情，那就是“创建”，说得更精确一点，是“组装”：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">把传入的参数按照一定的规范，“组装”进了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">element</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象里，并把它返回给了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eact.createElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，最终</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">又把它交回到了开发者手中</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781973816-62fb3655-7cfe-4363-b60e-9bc223cfcc2f.png)

```plain
const AppJSX = (<div className="App">
  <h1 className="title">I am the title</h1>
  <p className="content">I am the content</p>
</div>)

console.log(AppJSX)
```

<font style="color:rgb(44, 62, 80);">你会发现它确实是一个标准的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象实例</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781974492-b3a29a0a-a790-417c-90c9-37fd9c1471c8.png)

<font style="color:rgb(44, 62, 80);">这个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象实例，本质上是以 JavaScript 对象形式存在的对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的描述，也就是老生常谈的“虚拟 DOM”（准确地说，是虚拟</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的一个节点)</font>

### [](https://www.123fe.net/docs/base/improve.html#_14-%E4%B8%BA%E4%BB%80%E4%B9%88-react-%E5%85%83%E7%B4%A0%E6%9C%89%E4%B8%80%E4%B8%AA-typeof-%E5%B1%9E%E6%80%A7)<font style="color:rgb(44, 62, 80);">14 为什么 React 元素有一个 $$typeof 属性</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781974210-6a607c4a-a368-4537-8273-230e17625851.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">目的是为了防止 XSS 攻击。因为 Synbol 无法被序列化，所以 React 可以通过有没有 $$typeof 属性来断出当前的 element 对象是从数据库来的还是自己生成的。</font>

+ <font style="color:rgb(44, 62, 80);">如果没有 $$typeof 这个属性，react 会拒绝处理该元素。</font>
+ <font style="color:rgb(44, 62, 80);">在 React 的古老版本中，下面的写法会出现 XSS 攻击：</font>

```plain
// 服务端允许用户存储 JSON
let expectedTextButGotJSON = {
  type: 'div',
  props: {
    dangerouslySetInnerHTML: {
      __html: '/* 把你想的搁着 */'
    },
  },
  // ...
};
let message = { text: expectedTextButGotJSON };

// React 0.13 中有风险
<p>
  {message.text}
</p>
```

### [](https://www.123fe.net/docs/base/improve.html#_15-virtual-dom-%E7%9A%84%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86%E6%98%AF%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">15 Virtual DOM 的工作原理是什么</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781974515-805a64be-0ba1-4221-aeec-7f655a5ff9b5.png)

+ <font style="color:rgb(44, 62, 80);">虚拟 DOM 的工作原理是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">通过 JS 对象模拟 DOM 的节点</font><font style="color:rgb(44, 62, 80);">。在 Facebook 构建 React 初期时，考虑到要提升代码抽象能力、避免人为的 DOM 操作、降低代码整体风险等因素，所以引入了虚拟 DOM</font>
+ <font style="color:rgb(44, 62, 80);">虚拟 DOM 在实现上通常是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Plain Object</font><font style="color:rgb(44, 62, 80);">，以 React 为例，在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数中写的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Babel</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">插件的作用下，编译为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的属性参数</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">执行后会返回一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Plain Object</font><font style="color:rgb(44, 62, 80);">，它会描述自己的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性以及</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">情况等。这些</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Plain Object</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">通过树形结构组成一棵虚拟</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树。当状态发生变更时，将变更前后的虚拟</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树进行差异比较，这个过程称为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">，生成的结果称为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);">。计算之后，会渲染</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Patch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">完成对真实</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的操作。</font>
+ <font style="color:rgb(44, 62, 80);">虚拟 DOM 的优点主要有三点：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">改善大规模</font><font style="color:rgb(44, 62, 80);">DOM</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">操作的性能</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">规避 XSS 风险</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">能以较低的成本实现跨平台开发</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">虚拟 DOM 的缺点在社区中主要有两点</font>
    - <font style="color:rgb(44, 62, 80);">内存占用较高，因为需要模拟整个网页的真实</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font>
    - <font style="color:rgb(44, 62, 80);">高性能应用场景存在难以优化的情况，类似像 Google Earth 一类的高性能前端应用在技术选型上往往不会选择 React</font>

**<font style="color:rgb(44, 62, 80);">除了渲染页面，虚拟 DOM 还有哪些应用场景？</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个问题考验面试者的想象力。通常而言，我们只是将虚拟 DOM 与渲染绑定在一起，但实际上虚拟 DOM 的应用更为广阔。比如，只要你记录了真实 DOM 变更，它甚至可以应用于埋点统计与数据记录等。</font>

**<font style="color:rgb(44, 62, 80);">SSR原理</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">借助虚拟dom,服务器中没有dom概念的，react巧妙的借助虚拟dom，然后可以在服务器中nodejs可以运行起来react代码。</font>

### [](https://www.123fe.net/docs/base/improve.html#_16-react%E6%9C%89%E5%93%AA%E4%BA%9B%E4%BC%98%E5%8C%96%E6%80%A7%E8%83%BD%E7%9A%84%E6%89%8B%E6%AE%B5)<font style="color:rgb(44, 62, 80);">16 React有哪些优化性能的手段</font>
**<font style="color:rgb(44, 62, 80);">类组件中的优化手段</font>**

+ <font style="color:rgb(44, 62, 80);">使用纯组件</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">作为基类。</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">生命周期函数来自定义渲染逻辑。</font>

**<font style="color:rgb(44, 62, 80);">方法组件中的优化手段</font>**

+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">高阶函数包装组件，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以实现类似于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的效果</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font>
    - <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.useMemo</font><font style="color:rgb(44, 62, 80);">精细化的管控，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo 控制的则是是否需要重复执行某一段逻辑</font><font style="color:rgb(44, 62, 80);">，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo 控制是否需要重渲染一个组件</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useCallBack</font><font style="color:rgb(44, 62, 80);">。</font>

**<font style="color:rgb(44, 62, 80);">其他方式</font>**

+ <font style="color:rgb(44, 62, 80);">在列表需要频繁变动时，使用唯一 id 作为 key，而不是数组下标。</font>
+ <font style="color:rgb(44, 62, 80);">必要时通过改变 CSS 样式隐藏显示组件，而不是通过条件判断显示隐藏组件。</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Suspense</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和 lazy 进行懒加载，例如：</font>

```plain
import React, { lazy, Suspense } from "react";

export default class CallingLazyComponents extends React.Component {
  render() {
    var ComponentToLazyLoad = null;

    if (this.props.name == "Mayank") {
      ComponentToLazyLoad = lazy(() => import("./mayankComponent"));
    } else if (this.props.name == "Anshul") {
      ComponentToLazyLoad = lazy(() => import("./anshulComponent"));
    }

    return (
      <div>
        <h1>This is the Base User: {this.state.name}</h1>
        <Suspense fallback={<div>Loading...</div>}>
          <ComponentToLazyLoad />
        </Suspense>
      </div>
    )
  }
}
```

### [](https://www.123fe.net/docs/base/improve.html#_17-redux%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86%E8%A7%A3%E6%9E%90)<font style="color:rgb(44, 62, 80);">17 Redux实现原理解析</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">在 Redux 的整个工作过程中，数据流是严格单向的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。这一点一定一定要背下来，面试的时候也一定一定要记得说</font>

**<font style="color:rgb(44, 62, 80);">为什么要用redux</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中，数据在组件中是单向流动的，数据从一个方向父组件流向子组件（通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）,所以，两个非父子组件之间通信就相对麻烦，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的出现就是为了解决</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">里面的数据问题</font>

**<font style="color:rgb(44, 62, 80);">Redux设计理念</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Redux</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是将整个应用状态存储到一个地方上称为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">,里面保存着一个状态树</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store tree</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">,组件可以派发(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)行为(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)给</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">,而不是直接通知其他组件，组件内部通过订阅</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中的状态</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来刷新自己的视图</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781975936-92e58eac-22b7-4fd2-b1e2-03dfd496bc0f.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如果你想对数据进行修改，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">只有一种途径：派发 action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。action 会被 reducer 读取，进而根据 action 内容的不同对数据进行修改、生成新的 state（状态），这个新的 state 会更新到 store 对象里，进而驱动视图层面做出对应的改变。</font>

**<font style="color:rgb(44, 62, 80);">Redux三大原则</font>**

+ <font style="color:rgb(44, 62, 80);">唯一数据源</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">整个应用的state都被存储到一个状态树里面，并且这个状态树，只存在于唯一的store中</font>

+ <font style="color:rgb(44, 62, 80);">保持只读状态</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是只读的，唯一改变</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的方法就是触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是一个用于描述以发生时间的普通对象</font>

+ <font style="color:rgb(44, 62, 80);">数据改变只能通过纯函数来执行</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">使用纯函数来执行修改，为了描述</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如何改变</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的，你需要编写</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducers</font>

**<font style="color:rgb(44, 62, 80);">从编码的角度理解 Redux 工作流</font>**

1. <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createStore 来完成 store 对象的创建</font>

```plain
// 引入 redux
import { createStore } from 'redux'
// 创建 store
const store = createStore(
    reducer,
    initial_state,
    applyMiddleware(middleware1, middleware2, ...)
);
```

<font style="color:rgb(44, 62, 80);">createStore 方法是一切的开始，它接收三个入参：</font>

+ <font style="color:rgb(44, 62, 80);">reducer；</font>
+ <font style="color:rgb(44, 62, 80);">初始状态内容；</font>
+ <font style="color:rgb(44, 62, 80);">指定中间件</font>
1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer 的作用是将新的 state 返回给 store</font>

<font style="color:rgb(44, 62, 80);">一个 reducer 一定是一个纯函数，它可以有各种各样的内在逻辑，但它最终一定要返回一个 state：</font>

```plain
const reducer = (state, action) => {
    // 此处是各种样的 state处理逻辑
    return new_state
}
```

<font style="color:rgb(44, 62, 80);">当我们基于某个 reducer 去创建 store 的时候，其实就是给这个 store 指定了一套更新规则：</font>

```plain
// 更新规则全都写在 reducer 里 
const store = createStore(reducer)
```

1. <font style="color:rgb(44, 62, 80);">action 的作用是通知 reducer “让改变发生”</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">要想让 state 发生改变，就必须用正确的 action 来驱动这个改变。</font>

```plain
const action = {
  type: "ADD_ITEM",
  payload: '<li>text</li>'
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">action 对象中允许传入的属性有多个，但只有 type 是必传的。type 是 action 的唯一标识，reducer 正是通过不同的 type 来识别出需要更新的不同的 state，由此才能够实现精准的“定向更新”。</font>

1. <font style="color:rgb(44, 62, 80);">派发 action，靠的是 dispatch</font>

<font style="color:rgb(44, 62, 80);">action 本身只是一个对象，要想让 reducer 感知到 action，还需要“派发 action”这个动作，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">这个动作是由 store.dispatch 完成的</font><font style="color:rgb(44, 62, 80);">。这里我简单地示范一下：</font>

```plain
import { createStore } from 'redux'
// 创建 reducer
const reducer = (state, action) => {
    // 此处是各种样的 state处理逻辑
    return new_state
}
// 基于 reducer 创建 state
const store = createStore(reducer)
// 创建一个 action，这个 action 用 “ADD_ITEM” 来标识 
const action = {
  type: "ADD_ITEM",
  payload: '<li>text</li>'
}
// 使用 dispatch 派发 action，action 会进入到 reducer 里触发对应的更新
store.dispatch(action)
```

<font style="color:rgb(44, 62, 80);">以上这段代码，是从编码角度对 Redux 主要工作流的概括，这里我同样为你总结了一张对应的流程图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781975991-7d4ad5c4-dec5-43aa-8639-eb7a6e6d1ba8.png)

**<font style="color:rgb(44, 62, 80);">Redux源码</font>**

```plain
let createStore = (reducer) => {
    let state;
    //获取状态对象
    //存放所有的监听函数
    let listeners = [];
    let getState = () => state;
    //提供一个方法供外部调用派发action
    let dispath = (action) => {
        //调用管理员reducer得到新的state
        state = reducer(state, action);
        //执行所有的监听函数
        listeners.forEach((l) => l())
    }
    //订阅状态变化事件，当状态改变发生之后执行监听函数
    let subscribe = (listener) => {
        listeners.push(listener);
    }
    dispath();
    return {
        getState,
        dispath,
        subscribe
    }
}
let combineReducers=(renducers)=>{
    //传入一个renducers管理组，返回的是一个renducer
    return function(state={},action={}){
        let newState={};
        for(var attr in renducers){
            newState[attr]=renducers[attr](state[attr],action)

        }
        return newState;
    }
}
export {createStore,combineReducers};
```

**<font style="color:rgb(44, 62, 80);">聊聊 Redux 和 Vuex 的设计思想</font>**

+ **<font style="color:rgb(44, 62, 80);">共同点</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">首先两者都是处理全局状态的工具库，大致实现思想都是：全局</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">保存状态----></font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch(action)</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">------></font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vuex</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">里的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mutation</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)----> 生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newState</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">; 整个状态为同步操作；</font>

+ **<font style="color:rgb(44, 62, 80);">区别</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">最大的区别在于处理异步的不同，vuex里面多了一步</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commit</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">操作，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之后</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commit(mutation)</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之前处理异步，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">里面则是通过中间件处理</font>

**<font style="color:rgb(44, 62, 80);">redux 中间件</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中间件提供第三方插件的模式，自定义拦截 action -> reducer 的过程。变为 action -> middlewares -> reducer 。这种机制可以让我们改变数据流，实现如异步 action ，action 过 滤，日志输出，异常报告等功能</font>

<font style="color:rgb(44, 62, 80);">常见的中间件:</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-logger</font><font style="color:rgb(44, 62, 80);">:提供日志输出;</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-thunk</font><font style="color:rgb(44, 62, 80);">:处理异步操作;</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-promise</font><font style="color:rgb(44, 62, 80);">: 处理异步操作;</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">actionCreator</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的返回值是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font>

**<font style="color:rgb(44, 62, 80);">redux中间件的原理是什么</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">applyMiddleware</font>

**<font style="color:rgb(44, 62, 80);">为什么会出现中间件？</font>**

+ <font style="color:rgb(44, 62, 80);">它只是一个用来加工dispatch的工厂，而要加工什么样的dispatch出来，则需要我们传入对应的中间件函数</font>
+ <font style="color:rgb(44, 62, 80);">让每一个中间件函数，接收一个dispatch，然后返回一个改造后的dispatch，来作为下一个中间件函数的next，以此类推。</font>

```plain
function applyMiddleware(middlewares) {
  middlewares = middlewares.slice()
  middlewares.reverse()

  let dispatch = store.dispatch
  middlewares.forEach(middleware =>
    dispatch = middleware(store)(dispatch)
  )
  return Object.assign({}, store, { dispatch })
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上面的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">middleware(store)(dispatch)</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就相当于是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">const logger = store => next => {}</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，这就是构造后的dispatch，继续向下传递。这里</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">middlewares.reverse()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，进行数组反转的原因，是最后构造的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，实际上是最先执行的。因为在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">applyMiddleware</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">串联的时候，每个中间件只是返回一个新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数给下一个中间件，实际上这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">并不会执行。只有当我们在程序中通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store.dispatch(action)</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，真正派发的时候，才会执行。而此时的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是最后一个中间件返回的包装函数。然后依次向前递推执行。</font>

[浅析中间件(opens new window)](https://www.123fe.net/principle-docs/react/08-%E6%B5%85%E6%9E%90%E4%B8%AD%E9%97%B4%E4%BB%B6.html)

**<font style="color:rgb(44, 62, 80);">action、store、reducer分析</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">redux的核心概念就是store、action、reducer，从调用关系来看如下所示</font>

```javascript
store.dispatch(action) --> reducer(state, action) --> final state
```

```javascript
// reducer方法, 传入的参数有两个
// state: 当前的state
// action: 当前触发的行为, {type: 'xx'}
// 返回值: 新的state
var reducer = function(state, action){
    switch (action.type) {
        case 'add_todo':
            return state.concat(action.text);
        default:
            return state;
    }
};

// 创建store, 传入两个参数
// 参数1: reducer 用来修改state
// 参数2(可选): [], 默认的state值,如果不传, 则为undefined
var store = redux.createStore(reducer, []);

// 通过 store.getState() 可以获取当前store的状态(state)
// 默认的值是 createStore 传入的第二个参数
console.log('state is: ' + store.getState());  // state is:

// 通过 store.dispatch(action) 来达到修改 state 的目的
// 注意: 在redux里,唯一能够修改state的方法,就是通过 store.dispatch(action)
store.dispatch({type: 'add_todo', text: '读书'});
// 打印出修改后的state
console.log('state is: ' + store.getState());  // state is: 读书

store.dispatch({type: 'add_todo', text: '写作'});
console.log('state is: ' + store.getState());  // state is: 读书,写作
```

1. <font style="color:rgb(44, 62, 80);">store、reducer、action关联</font>

**<font style="color:rgb(44, 62, 80);">store</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">在这里代表的是数据模型，内部维护了一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">变量</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">有两个核心方法，分别是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getState</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(44, 62, 80);">。前者用来获取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">的状态（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">），后者用来修改</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">的状态</font>

```javascript
// 创建store, 传入两个参数
// 参数1: reducer 用来修改state
// 参数2(可选): [], 默认的state值,如果不传, 则为undefined
var store = redux.createStore(reducer, []);

// 通过 store.getState() 可以获取当前store的状态(state)
// 默认的值是 createStore 传入的第二个参数
console.log('state is: ' + store.getState());  // state is:

// 通过 store.dispatch(action) 来达到修改 state 的目的
// 注意: 在redux里,唯一能够修改state的方法,就是通过 store.dispatch(action)
store.dispatch({type: 'add_todo', text: '读书'});
```

**<font style="color:rgb(44, 62, 80);">action</font>**

+ <font style="color:rgb(44, 62, 80);">对行为（如用户行为）的抽象，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux</font><font style="color:rgb(44, 62, 80);">里是一个普通的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);">对象</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">必须有一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">type</font><font style="color:rgb(44, 62, 80);">字段来标识这个行为的类型</font>

```javascript
{type:'add_todo', text:'读书'}
{type:'add_todo', text:'写作'}
{type:'add_todo', text:'睡觉', time:'晚上'}
```

**<font style="color:rgb(44, 62, 80);">reducer</font>**

+ <font style="color:rgb(44, 62, 80);">一个普通的函数，用来修改</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">的状态。传入两个参数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font>
+ <font style="color:rgb(44, 62, 80);">其中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">为当前的状态（可通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store.getState()</font><font style="color:rgb(44, 62, 80);">获得），而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">为当前触发的行为（通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store.dispatch(action)</font><font style="color:rgb(44, 62, 80);">调用触发）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer(state, action)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">返回的值，就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">最新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">值</font>

```javascript
// reducer方法, 传入的参数有两个
// state: 当前的state
// action: 当前触发的行为, {type: 'xx'}
// 返回值: 新的state
var reducer = function(state, action){
    switch (action.type) {
        case 'add_todo':
            return state.concat(action.text);
        default:
            return state;
    }
};
```

1. <font style="color:rgb(44, 62, 80);">关于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">actionCreator</font>

```plain
actionCreator(args) => action
```

```plain
var addTodo = function(text){
    return {
        type: 'add_todo',
        text: text
    };
};

addTodo('睡觉');  // 返回：{type: 'add_todo', text: '睡觉'}
```

**<font style="color:rgb(44, 62, 80);">异步Action及操作</font>**

1. <font style="color:rgb(44, 62, 80);">创建同步Action</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是数据从应用传递到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的载体，也是开启一次完成数据流的开始</font>

**<font style="color:rgb(44, 62, 80);">普通的action对象</font>**

```javascript
const action = {
	type:'ADD_TODO',
	name:'poetries'
}

dispatch(action)
```

**<font style="color:rgb(44, 62, 80);">封装action creator</font>**

```javascript
function actionCreator(data){
    return {
    	type:'ADD_TODO',
    	data:data
    }
}

dispatch(actionCreator('poetries'))
```

**<font style="color:rgb(44, 62, 80);">bindActionCreators合并</font>**

```javascript
function a(name,id){
	reurn {
		type:'a',
		name,
		id
	}
}
function b(name,id){
	reurn {
		type:'b',
		name,
		id
	}
}

let actions = Redux.bindActionCreators({a,b},store.dispatch)

//调用
actions.a('poetries','id001')
actions.b('jing','id002')
```

**<font style="color:rgb(44, 62, 80);">action创建的标准</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在Flux的架构中，一个Action要符合 FSA(Flux Standard Action) 规范，需要满足如下条件</font>

+ <font style="color:rgb(44, 62, 80);">是一个纯文本对象</font>
+ <font style="color:rgb(44, 62, 80);">只具备</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">type</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">payload</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">error</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">meta</font><font style="color:rgb(44, 62, 80);">中的一个或者多个属性。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">type</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">字段不可缺省，其它字段可缺省</font>
+ <font style="color:rgb(44, 62, 80);">若</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Action</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">报错，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">error</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">字段不可缺省，切必须为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">payload</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是一个对象，用作Action携带数据的载体</font>

**<font style="color:rgb(44, 62, 80);">标准action示例</font>**

+ <font style="color:rgb(44, 62, 80);">A basic Flux Standard Action:</font>

```javascript
{
  type: 'ADD_TODO',
  payload: {
    text: 'Do something.'  
  }
}
```

+ <font style="color:rgb(44, 62, 80);">An FSA that represents an error, analogous to a rejected Promise</font>

```javascript
{
  type: 'ADD_TODO',
  payload: new Error(),
  error: true
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">https://github.com/acdlite/flux-standard-action</font>

+ <font style="color:rgb(44, 62, 80);">可以采用如下一个简单的方式检验一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Action</font><font style="color:rgb(44, 62, 80);">是否符合FSA标准</font>

```javascript
// every有一个匹配不到返回false
let isFSA = Object.keys(action).every((item)=>{
   return  ['payload','type','error','meta'].indexOf(item) >  -1
})
```

1. <font style="color:rgb(44, 62, 80);">创建异步action的多种方式</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">最简单的方式就是使用同步的方式来异步，将原来同步时一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">拆分成多个异步的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的，在异步开始前、异步请求中、异步正常返回（异常）操作分别使用同步的操作，从而模拟出一个异步操作了。这样的方式是比较麻烦的，现在已经有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-saga</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">等插件来解决这些问题了</font>

**<font style="color:rgb(44, 62, 80);">异步action的实现方式一：setTimeout</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-thunk</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中间处理解析</font>

```javascript
function thunkAction(data) {
    reutrn (dispatch)=>{
        setTimeout(function(){
            dispatch({
                type:'ADD_TODO',
                data
            })
        },3000)
    }
}
```

**<font style="color:rgb(44, 62, 80);">异步action的实现方式二：promise实现异步action</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中间处理这种</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font>

```javascript
function promiseAction(name){
    return new Promise((resolve,reject) => {
        setTimeout((param)=>{
            resolve({
                type:'ADD_TODO',
                name
            })
        },3000)
    }).then((param)=>{
        dispatch(action("action2"))
        return;
    }).then((param)=>{
        dispatch(action("action3"))
    })
}
```

1. <font style="color:rgb(44, 62, 80);">redux异步流程</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781977831-96615d3b-5c49-4120-9bb0-10a7644f5928.png)

+ <font style="color:rgb(44, 62, 80);">首先发起一个action，然后通过中间件，这里为什么要用中间件呢，因为这样</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(44, 62, 80);">的返回值才能是一个函数。</font>
+ <font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store.dispatch</font><font style="color:rgb(44, 62, 80);">，将状态的的改变传给</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">的小弟</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer</font><font style="color:rgb(44, 62, 80);">根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">的改变，传递新的状态</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">最后将所有的改变告诉给它的大哥，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">保存着所有的数据，并将数据注入到组件的顶部，这样组件就可以获得它需要的数据了</font>
1. <font style="color:rgb(44, 62, 80);">Redux异步方案选型</font>

**<font style="color:rgb(44, 62, 80);">redux-thunk</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Redux</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">本身只能处理同步的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，但可以通过中间件来拦截处理其它类型的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，比如函数(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Thunk</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)，再用回调触发普通</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，从而实现异步处理</font>

+ <font style="color:rgb(44, 62, 80);">发送异步的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">其实是被中间件捕获的，函数类型的action就被</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">middleware</font><font style="color:rgb(44, 62, 80);">捕获。至于怎么定义异步的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">要看你用哪个中间件，根据他们的实例来定义，这样才会正确解析</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Redux</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">本身不处理异步行为，需要依赖中间件。结合</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-actions</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">使用，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Redux</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">有两个推荐的异步中间件</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-thunk</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-promise</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-thunk</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的源码如下</font>

```javascript
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">源码可知，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action creator</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">需要返回一个函数给</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-thunk</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">进行调用，示例如下</font>

```javascript
export let addTodoWithThunk = (val) => async (dispatch, getState)=>{
    //请求之前的一些处理

    let value = await Promise.resolve(val + ' thunk');
    dispatch({
        type:CONSTANT.ADD_TO_DO_THUNK,
        payload:{
            value
        }
    });
};
```

+ <font style="color:rgb(44, 62, 80);">而它使用起来最大的问题，就是重复的模板代码太多</font>

```javascript
//action types
const GET_DATA = 'GET_DATA',
    GET_DATA_SUCCESS = 'GET_DATA_SUCCESS',
    GET_DATA_FAILED = 'GET_DATA_FAILED';
    
//action creator
const getDataAction = (id) => (dispatch, getState) => {
        dispatch({
            type: GET_DATA, 
            payload: id
        })
        api.getData(id) //注：本文所有示例的api.getData都返回promise对象
            .then(response => {
                dispatch({
                    type: GET_DATA_SUCCESS,
                    payload: response
                })
            })
            .catch(error => {
                dispatch({
                    type: GET_DATA_FAILED,
                    payload: error
                })
            }) 
    }
}

//reducer
const reducer = (oldState, action) => {
    switch(action.type) {
    case GET_DATA : 
        return oldState;
    case GET_DATA_SUCCESS : 
        return successState;
    case GET_DATA_FAILED : 
        return errorState;
    }
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这已经是最简单的场景了，请注意：我们甚至还没写一行业务逻辑，如果每个异步处理都像这样，重复且无意义的工作会变成明显的阻碍</font>

+ <font style="color:rgb(44, 62, 80);">另一方面，像</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GET_DATA_SUCCESS</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GET_DATA_FAILED</font><font style="color:rgb(44, 62, 80);">这样的字符串声明也非常无趣且易错 上例中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GET_DATA</font><font style="color:rgb(44, 62, 80);">这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">并不是多数场景需要的</font>

**<font style="color:rgb(44, 62, 80);">redux-promise</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">由于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-thunk</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">写起来实在是太麻烦了，社区当然会有其它轮子出现。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">则是其中比较知名的</font>

+ <font style="color:rgb(44, 62, 80);">它自定义了一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">middleware</font><font style="color:rgb(44, 62, 80);">，当检测到有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">payload</font><font style="color:rgb(44, 62, 80);">属性是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">对象时，就会</font>
    - <font style="color:rgb(44, 62, 80);">若</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resolve</font><font style="color:rgb(44, 62, 80);">，触发一个此</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">的拷贝，但</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">payload</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">，并设</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">status</font><font style="color:rgb(44, 62, 80);">属性为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">"success"</font>
    - <font style="color:rgb(44, 62, 80);">若</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reject</font><font style="color:rgb(44, 62, 80);">，触发一个此</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">的拷贝，但</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">payload</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reason</font><font style="color:rgb(44, 62, 80);">，并设</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">status</font><font style="color:rgb(44, 62, 80);">属性为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">"error"</font>

```javascript
//action types
const GET_DATA = 'GET_DATA';

//action creator
const getData = function(id) {
    return {
        type: GET_DATA,
        payload: api.getData(id) //payload为promise对象
    }
}

//reducer
function reducer(oldState, action) {
    switch(action.type) {
        case GET_DATA: 
            if (action.status === 'success') {
                return successState
            } else {
                   return errorState
            }
        }
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为了精简而做出的妥协非常明显：无法处理乐观更新</font>

**<font style="color:rgb(44, 62, 80);">场景解析之：乐观更新</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">多数异步场景都是悲观更新的，即等到请求成功才渲染数据。而与之相对的乐观更新，则是不等待请求成功，在发送请求的同时立即渲染数据</font>

+ <font style="color:rgb(44, 62, 80);">由于乐观更新发生在用户操作时，要处理它，意味着必须有action表示用户的初始动作</font>
+ <font style="color:rgb(44, 62, 80);">在上面</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-thunk</font><font style="color:rgb(44, 62, 80);">的例子中，我们看到了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GET_DATA</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GET_DATA_SUCCESS</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GET_DATA_FAILED</font><font style="color:rgb(44, 62, 80);">三个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">，分别表示初始动作、异步成功和异步失败，其中第一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">使得</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-thunk</font><font style="color:rgb(44, 62, 80);">具备乐观更新的能力</font>
+ <font style="color:rgb(44, 62, 80);">而在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-promise</font><font style="color:rgb(44, 62, 80);">中，最初触发的action被中间件拦截然后过滤掉了。原因很简单，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux</font><font style="color:rgb(44, 62, 80);">认可的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">对象是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">plain JavaScript objects</font><font style="color:rgb(44, 62, 80);">，即简单对象，而在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-promise</font><font style="color:rgb(44, 62, 80);">中，初始</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">payload</font><font style="color:rgb(44, 62, 80);">是个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font>

**<font style="color:rgb(44, 62, 80);">redux-promise-middleware</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-promise-middleware</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">相比</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，采取了更为温和和渐进式的思路，保留了和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-thunk</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">类似的三个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font>

```javascript
//action types
const GET_DATA = 'GET_DATA',
    GET_DATA_PENDING = 'GET_DATA_PENDING',
    GET_DATA_FULFILLED = 'GET_DATA_FULFILLED',
    GET_DATA_REJECTED = 'GET_DATA_REJECTED';
    
//action creator
const getData = function(id) {
    return {
        type: GET_DATA,
        payload: {
            promise: api.getData(id),
            data: id
        }
    }
}

//reducer
const reducer = function(oldState, action) {
    switch(action.type) {
    case GET_DATA_PENDING :
        return oldState; // 可通过action.payload.data获取id
    case GET_DATA_FULFILLED : 
        return successState;
    case GET_DATA_REJECTED : 
        return errorState;
    }
}
```

1. <font style="color:rgb(44, 62, 80);">redux异步操作代码演示</font>
+ <font style="color:rgb(44, 62, 80);">根据官网的async例子分析 https://github.com/lewis617/react-redux-tutorial/tree/master/redux-examples/async</font>

**<font style="color:rgb(44, 62, 80);">action/index.js</font>**

```javascript
import fetch from 'isomorphic-fetch'
export const RECEIVE_POSTS = 'RECEIVE_POSTS'

//获取新闻成功的action
function receivePosts(reddit, json) {
  return {
    type: RECEIVE_POSTS,
    reddit: reddit,
    posts: json.data.children.map(child =>child.data)
  }
}

function fetchPosts(subreddit) {

  return function (dispatch) {
    
    return fetch(`http://www.subreddit.com/r/${subreddit}.json`)
      .then(response => response.json())
      .then(json =>
        dispatch(receivePosts(subreddit, json))
      )
  }
}

//如果需要则开始获取文章
export function fetchPostsIfNeeded(subreddit) {

  return (dispatch, getState) => {

      return dispatch(fetchPosts(subreddit))

    }
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fetchPostsIfNeeded</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这里就是一个中间件。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux-thunk</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会拦截</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fetchPostsIfNeeded</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，会先发起数据请求，如果成功，就将数据传给</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从而到达</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">那里</font>

**<font style="color:rgb(44, 62, 80);">reducers/index.js</font>**

```javascript
import { combineReducers } from 'redux'
import {
  RECEIVE_POSTS
} from '../actions'


function posts(state = {
  items: []
}, action) {
  switch (action.type) {

    case RECEIVE_POSTS:
      // Object.assign是ES6的一个语法。合并对象，将对象合并为一个，前后相同的话，后者覆盖强者。详情可以看这里
      //  https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign
      return Object.assign({}, state, {
        items: action.posts //数据都存在了这里
      })
    default:
      return state
  }
}


// 将所有的reducer结合为一个,传给store
const rootReducer = combineReducers({
  postsByReddit
})

export default rootReducer
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个跟正常的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">差不多。判断</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的类型，从而根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的不同类型，返回不同的数据。这里将数据存储在了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">items</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这里。这里的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">只有一个。最后结合成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootReducer</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">,传给</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font>

**<font style="color:rgb(44, 62, 80);">store/configureStore.js</font>**

```javascript
import { createStore, applyMiddleware } from 'redux'
import thunkMiddleware from 'redux-thunk'
import createLogger from 'redux-logger'
import rootReducer from '../reducers'

const createStoreWithMiddleware = applyMiddleware(
  thunkMiddleware,  
  createLogger()  
)(createStore)

export default function configureStore(initialState) {
  const store = createStoreWithMiddleware(rootReducer, initialState)

  if (module.hot) {
    // Enable Webpack hot module replacement for reducers
    module.hot.accept('../reducers', () => {
      const nextRootReducer = require('../reducers')
      store.replaceReducer(nextRootReducer)
    })
  }

  return store
}
```

+ <font style="color:rgb(44, 62, 80);">我们是如何在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">机制中引入</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Redux Thunk middleware</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的呢？ 我们使用了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">applyMiddleware()</font>
+ <font style="color:rgb(44, 62, 80);">通过使用指定的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">middleware</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action creator</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">除了返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象外还可以返回函数</font>
+ <font style="color:rgb(44, 62, 80);">这时，这个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action creator</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就成为了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">thunk</font>

**<font style="color:rgb(44, 62, 80);">界面上的调用：在containers/App.js</font>**

```javascript
//初始化渲染后触发
  componentDidMount() {
    const { dispatch} = this.props
    // 这里可以传两个值，一个是 reactjs 一个是 frontend
    dispatch(fetchPostsIfNeeded('frontend'))
  }
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">改变状态的时候也是需要通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来传递的</font>

+ <font style="color:rgb(44, 62, 80);">数据的获取是通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">provider</font><font style="color:rgb(44, 62, 80);">,将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">里面的数据注入给组件。让顶级组件提供给他们的子孙组件调用。代码如下：</font>

```javascript
import 'babel-core/polyfill'
import React from 'react'
import { render } from 'react-dom'
import { Provider } from 'react-redux'
import App from './containers/App'
import configureStore from './store/configureStore'
const store = configureStore()
render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这样就完成了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的异步操作。其实最主要的区别还是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">里面还有中间件的调用，其他的地方基本跟同步的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">差不多的。搞懂了中间件，就基本搞懂了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">redux</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的异步操作</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781978204-ef73fc0e-b389-4a79-9d7f-6aa98332c9e1.png)

### [](https://www.123fe.net/docs/base/improve.html#_18-%E8%B0%88%E8%B0%88%E4%BD%A0%E5%AF%B9%E7%8A%B6%E6%80%81%E7%AE%A1%E7%90%86%E7%9A%84%E7%90%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">18 谈谈你对状态管理的理解</font>
+ <font style="color:rgb(44, 62, 80);">首先介绍 Flux，Flux 是一种使用单向数据流的形式来组合 React 组件的应用架构。</font>
+ <font style="color:rgb(44, 62, 80);">Flux 包含了 4 个部分，分别是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dispatcher</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Store</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">View</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Action</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Store</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">存储了视图层所有的数据，当</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Store</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">变化后会引起 View 层的更新。如果在视图层触发一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Action</font><font style="color:rgb(44, 62, 80);">，就会使当前的页面数据值发生变化。Action 会被 Dispatcher 进行统一的收发处理，传递给 Store 层，Store 层已经注册过相关 Action 的处理逻辑，处理对应的内部状态变化后，触发 View 层更新。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Flux 的优点是单向数据流，解决了 MVC 中数据流向不清的问题</font><font style="color:rgb(44, 62, 80);">，使开发者可以快速了解应用行为。从项目结构上简化了视图层设计，明确了分工，数据与业务逻辑也统一存放管理，使在大型架构的项目中更容易管理、维护代码。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">其次是 Redux</font><font style="color:rgb(44, 62, 80);">，Redux 本身是一个 JavaScript 状态容器，提供可预测化状态的管理。社区通常认为 Redux 是 Flux 的一个简化设计版本，它提供的状态管理，简化了一些高级特性的实现成本，比如撤销、重做、实时编辑、时间旅行、服务端同构等。</font>
+ <font style="color:rgb(44, 62, 80);">Redux 的核心设计包含了三大原则：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">单一数据源、纯函数 Reducer、State 是只读的</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">Redux 中整个数据流的方案与 Flux 大同小异</font>
+ <font style="color:rgb(44, 62, 80);">Redux 中的另一大核心点是处理“副作用”，AJAX 请求等异步工作，或不是纯函数产生的第三方的交互都被认为是 “副作用”。这就造成在纯函数设计的 Redux 中，处理副作用变成了一件至关重要的事情。社区通常有两种解决方案：</font>
    - <font style="color:rgb(44, 62, 80);">第一类是在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dispatch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的时候会有一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">middleware 中间件层</font><font style="color:rgb(44, 62, 80);">，拦截分发的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Action 并添加额外的复杂行为</font><font style="color:rgb(44, 62, 80);">，还可以添加副作用。第一类方案的流行框架有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Redux-thunk、Redux-Promise、Redux-Observable、Redux-Saga</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等。</font>
    - <font style="color:rgb(44, 62, 80);">第二类是允许</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Reducer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">层中直接处理副作用，采取该方案的有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Loop</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Loop</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在实现中采用了 Elm 中分形的思想，使代码具备更强的组合能力。</font>
    - <font style="color:rgb(44, 62, 80);">除此以外，社区还提供了更为工程化的方案，比如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rematch 或 dva</font><font style="color:rgb(44, 62, 80);">，提供了更详细的模块架构能力，提供了拓展插件以支持更多功能。</font>
+ <font style="color:rgb(44, 62, 80);">Redux 的优点很多：</font>
    - <font style="color:rgb(44, 62, 80);">结果可预测；</font>
    - <font style="color:rgb(44, 62, 80);">代码结构严格易维护；</font>
    - <font style="color:rgb(44, 62, 80);">模块分离清晰且小函数结构容易编写单元测试；</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Action</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">触发的方式，可以在调试器中使用时间回溯，定位问题更简单快捷；</font>
    - <font style="color:rgb(44, 62, 80);">单一数据源使服务端同构变得更为容易；社区方案多，生态也更为繁荣。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">最后是 Mobx</font><font style="color:rgb(44, 62, 80);">，Mobx 通过监听数据的属性变化，可以直接在数据上更改触发UI 的渲染。在使用上更接近 Vue，比起</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Flux 与 Redux</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的手动挡的体验，更像开自动挡的汽车。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Mobx 的响应式实现原理与 Vue 相同</font><font style="color:rgb(44, 62, 80);">，以</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Mobx 5</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为分界点，5 以前采用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的方案，5 及以后使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的方案。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">它的优点是样板代码少、简单粗暴、用户学习快、响应式自动更新数据</font><font style="color:rgb(44, 62, 80);">让开发者的心智负担更低。</font>
+ <font style="color:rgb(44, 62, 80);">Mobx 在开发项目时简单快速，但应用 Mobx 的场景 ，其实完全可以用 Vue 取代。如果纯用 Vue，体积还会更小巧</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781978610-cfc678ad-76d8-41f7-8cfc-53d9c7def0f0.png)

### [](https://www.123fe.net/docs/base/improve.html#_19-connect%E7%BB%84%E4%BB%B6%E5%8E%9F%E7%90%86%E5%88%86%E6%9E%90)<font style="color:rgb(44, 62, 80);">19 connect组件原理分析</font>
**<font style="color:rgb(44, 62, 80);">1. connect用法</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">作用：连接</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">组件与</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Redux store</font>

```javascript
connect([mapStateToProps], [mapDispatchToProps], [mergeProps],[options])
// 这个函数允许我们将 store 中的数据作为 props 绑定到组件上
const mapStateToProps = (state) => {
  return {
    count: state.count
  }
}
```

+ <font style="color:rgb(44, 62, 80);">这个函数的第一个参数就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Redux</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">，我们从中摘取了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">count</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性。你不必将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的数据原封不动地传入组件，可以根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的数据，动态地输出组件需要的（最小）属性</font>
+ <font style="color:rgb(44, 62, 80);">函数的第二个参数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ownProps</font><font style="color:rgb(44, 62, 80);">，是组件自己的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">变化，或者</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ownProps</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">变化的时候，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mapStateToProps</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">都会被调用，计算出一个新的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stateProps</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，（在与</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ownProps merge</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">后）更新给组件</font>

```plain
mapDispatchToProps(dispatch, ownProps): dispatchProps
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">connect</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的第二个参数是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mapDispatchToProps</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，它的功能是，将</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">作为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">绑定到组件上，也会成为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MyComp</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的 `props</font>

**<font style="color:rgb(44, 62, 80);">2. 原理解析</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">首先</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">connect</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之所以会成功，是因为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Provider</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">组件</font>

+ <font style="color:rgb(44, 62, 80);">在原应用组件上包裹一层，使原来整个应用成为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Provider</font><font style="color:rgb(44, 62, 80);">的子组件</font>
+ <font style="color:rgb(44, 62, 80);">接收</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Redux</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">作为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">，通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">context</font><font style="color:rgb(44, 62, 80);">对象传递给子孙组件上的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">connect</font>

**<font style="color:rgb(44, 62, 80);">connect做了些什么</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">它真正连接</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Redux</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，它包在我们的容器组件的外一层，它接收上面</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Provider</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">提供的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">里面的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，传给一个构造函数，返回一个对象，以属性形式传给我们的容器组件</font>

**<font style="color:rgb(44, 62, 80);">3. 源码</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">connect</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是一个高阶函数，首先传入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mapStateToProps</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mapDispatchToProps</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，然后返回一个生产</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的函数(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">wrapWithConnect</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)，然后再将真正的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">作为参数传入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">wrapWithConnect</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，这样就生产出一个经过包裹的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Connect</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">组件，该组件具有如下特点</font>

+ <font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props.store</font><font style="color:rgb(44, 62, 80);">获取祖先</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store props</font><font style="color:rgb(44, 62, 80);">包括</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stateProps</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatchProps</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">parentProps</font><font style="color:rgb(44, 62, 80);">,合并在一起得到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nextState</font><font style="color:rgb(44, 62, 80);">，作为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">传给真正的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount</font><font style="color:rgb(44, 62, 80);">时，添加事件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.store.subscribe(this.handleChange)</font><font style="color:rgb(44, 62, 80);">，实现页面交互</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);">时判断是否有避免进行渲染，提升页面性能，并得到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nextState</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillUnmount</font><font style="color:rgb(44, 62, 80);">时移除注册的事件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.handleChange</font>

```javascript
// 主要逻辑

export default function connect(mapStateToProps, mapDispatchToProps, mergeProps, options = {}) {
  return function wrapWithConnect(WrappedComponent) {
    class Connect extends Component {
      constructor(props, context) {
        // 从祖先Component处获得store
        this.store = props.store || context.store
        this.stateProps = computeStateProps(this.store, props)
        this.dispatchProps = computeDispatchProps(this.store, props)
        this.state = { storeState: null }
        // 对stateProps、dispatchProps、parentProps进行合并
        this.updateState()
      }
      shouldComponentUpdate(nextProps, nextState) {
        // 进行判断，当数据发生改变时，Component重新渲染
        if (propsChanged || mapStateProducedChange || dispatchPropsChanged) {
          this.updateState(nextProps)
          return true
        }
      }
      componentDidMount() {
        // 改变Component的state
        this.store.subscribe(() = {
          this.setState({
            storeState: this.store.getState()
          })
        })
      }
      render() {
        // 生成包裹组件Connect
        return (
          <WrappedComponent {...this.nextState} />
        )
      }
    }
    Connect.contextTypes = {
      store: storeShape
    }
    return Connect;
  }
}
```

### [](https://www.123fe.net/docs/base/improve.html#_20-react-hooks)<font style="color:rgb(44, 62, 80);">20 React Hooks</font>
+ <font style="color:rgb(44, 62, 80);">代码逻辑聚合，逻辑复用</font>
+ <font style="color:rgb(44, 62, 80);">HOC嵌套地狱</font>
+ <font style="color:rgb(44, 62, 80);">代替class</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">React 中通常使用 类定义 或者 函数定义 创建组件:</font>

<font style="color:rgb(44, 62, 80);">在类定义中，我们可以使用到许多 React 特性，例如 state、 各种组件生命周期钩子等，但是在函数定义中，我们却无能为力，因此 React 16.8 版本推出了一个新功能 (React Hooks)，通过它，可以更好的在函数定义组件中使用 React 特性。</font>

**<font style="color:rgb(44, 62, 80);">函数组件与类组件的对比：无关“优劣”，只谈“不同”</font>**

+ <font style="color:rgb(44, 62, 80);">类组件需要继承 class，函数组件不需要；</font>
+ <font style="color:rgb(44, 62, 80);">类组件可以访问生命周期方法，函数组件不能；</font>
+ <font style="color:rgb(44, 62, 80);">类组件中可以获取到实例化后的 this，并基于这个 this 做各种各样的事情，而函数组件不可以；</font>
+ <font style="color:rgb(44, 62, 80);">类组件中可以定义并维护 state（状态），而函数组件不可以；</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">但是类组件它太重了，对于解决许多问题来说，编写一个类组件实在是一个过于复杂的姿势。复杂的姿势必然带来高昂的理解成本，这也是我们所不想看到的</font>

**<font style="color:rgb(44, 62, 80);">react hooks的好处:</font>**

1. <font style="color:rgb(44, 62, 80);">跨组件复用: 其实 render props / HOC 也是为了复用，相比于它们，Hooks 作为官方的底层 API，最为轻量，而且改造成本小，不会影响原来的组件层次结构和传说中的嵌套地狱；</font>
2. <font style="color:rgb(44, 62, 80);">类定义更为复杂</font>
+ <font style="color:rgb(44, 62, 80);">不同的生命周期会使逻辑变得分散且混乱，不易维护和管理；</font>
+ <font style="color:rgb(44, 62, 80);">时刻需要关注this的指向问题；</font>
+ <font style="color:rgb(44, 62, 80);">代码复用代价高，高阶组件的使用经常会使整个组件树变得臃肿；</font>
1. <font style="color:rgb(44, 62, 80);">状态与UI隔离: 正是由于 Hooks 的特性，状态逻辑会变成更小的粒度，并且极容易被抽象成一个自定义 Hooks，组件中的状态和 UI 变得更为清晰和隔离。</font>

**<font style="color:rgb(44, 62, 80);">注意:</font>**

+ <font style="color:rgb(44, 62, 80);">避免在 循环/条件判断/嵌套函数 中调用 hooks，保证调用顺序的稳定；</font>
+ <font style="color:rgb(44, 62, 80);">只有 函数定义组件 和 hooks 可以调用 hooks，避免在 类组件 或者 普通函数 中调用；</font>
+ <font style="color:rgb(44, 62, 80);">不能在useEffect中使用useState，React 会报错提示；</font>
+ <font style="color:rgb(44, 62, 80);">类组件不会被替换或废弃，不需要强制改造类组件，两种方式能并存；</font>

**<font style="color:rgb(44, 62, 80);">重要钩子</font>**

1. <font style="color:rgb(44, 62, 80);">状态钩子 (useState): 用于定义组件的 State，其到类定义中this.state的功能；</font>

```javascript
// useState 只接受一个参数: 初始状态
// 返回的是组件名和更改该组件对应的函数
const [flag, setFlag] = useState(true);
// 修改状态
setFlag(false)

// 上面的代码映射到类定义中:
this.state = {
  flag: true	
}
const flag = this.state.flag
const setFlag = (bool) => {
  this.setState({
    flag: bool,
  })
}
```

1. <font style="color:rgb(44, 62, 80);">生命周期钩子 (useEffect):</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">类定义中有许多生命周期函数，而在 React Hooks 中也提供了一个相应的函数 (useEffect)，这里可以看做componentDidMount、componentDidUpdate和componentWillUnmount的结合。</font>

**<font style="color:rgb(44, 62, 80);">useEffect(callback, [source])接受两个参数</font>**

+ <font style="color:rgb(44, 62, 80);">callback: 钩子回调函数；</font>
+ <font style="color:rgb(44, 62, 80);">source: 设置触发条件，仅当 source 发生改变时才会触发；</font>
+ <font style="color:rgb(44, 62, 80);">useEffect钩子在没有传入[source]参数时，默认在每次 render 时都会优先调用上次保存的回调中返回的函数，后再重新调用回调；</font>

```javascript
useEffect(() => {
  // 组件挂载后执行事件绑定
  console.log('on')
  addEventListener()

  // 组件 update 时会执行事件解绑
  return () => {
    console.log('off')
    removeEventListener()
  }
}, [source]);


// 每次 source 发生改变时，执行结果(以类定义的生命周期，便于大家理解):
// --- DidMount ---
// 'on'
// --- DidUpdate ---
// 'off'
// 'on'
// --- DidUpdate ---
// 'off'
// 'on'
// --- WillUnmount --- 
// 'off'
```

**<font style="color:rgb(44, 62, 80);">通过第二个参数，我们便可模拟出几个常用的生命周期:</font>**

+ <font style="color:rgb(44, 62, 80);">componentDidMount: 传入[]时，就只会在初始化时调用一次</font>

```javascript
const useMount = (fn) => useEffect(fn, [])
```

+ <font style="color:rgb(44, 62, 80);">componentWillUnmount: 传入[]，回调中的返回的函数也只会被最终执行一次</font>

```javascript
const useUnmount = (fn) => useEffect(() => fn, [])
```

+ <font style="color:rgb(44, 62, 80);">mounted: 可以使用 useState 封装成一个高度可复用的 mounted 状态；</font>

```javascript
const useMounted = () => {
  const [mounted, setMounted] = useState(false);
  useEffect(() => {
    !mounted && setMounted(true);
    return () => setMounted(false);
  }, []);
  return mounted;
}
```

+ <font style="color:rgb(44, 62, 80);">componentDidUpdate: useEffect每次均会执行，其实就是排除了 DidMount 后即可；</font>

```javascript
const mounted = useMounted() 
useEffect(() => {
  mounted && fn()
})
```

1. <font style="color:rgb(44, 62, 80);">其它内置钩子:</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useContext</font><font style="color:rgb(44, 62, 80);">: 获取 context 对象</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useReducer</font><font style="color:rgb(44, 62, 80);">: 类似于 Redux 思想的实现，但其并不足以替代 Redux，可以理解成一个组件内部的 redux:</font>
    - <font style="color:rgb(44, 62, 80);">并不是持久化存储，会随着组件被销毁而销毁；</font>
    - <font style="color:rgb(44, 62, 80);">属于组件内部，各个组件是相互隔离的，单纯用它并无法共享数据；</font>
    - <font style="color:rgb(44, 62, 80);">配合useContext`的全局性，可以完成一个轻量级的 Redux；(easy-peasy)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useCallback</font><font style="color:rgb(44, 62, 80);">: 缓存回调函数，避免传入的回调每次都是新的函数实例而导致依赖组件重新渲染，具有性能优化的效果；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);">: 用于缓存传入的 props，避免依赖的组件每次都重新渲染；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useRef</font><font style="color:rgb(44, 62, 80);">: 获取组件的真实节点；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useLayoutEffect</font>
    - <font style="color:rgb(44, 62, 80);">DOM更新同步钩子。用法与useEffect类似，只是区别于执行时间点的不同</font>
    - <font style="color:rgb(44, 62, 80);">useEffect属于异步执行，并不会等待 DOM 真正渲染后执行，而useLayoutEffect则会真正渲染后才触发；</font>
    - <font style="color:rgb(44, 62, 80);">可以获取更新后的 state；</font>
1. <font style="color:rgb(44, 62, 80);">自定义钩子(useXxxxx): 基于 Hooks 可以引用其它 Hooks 这个特性，我们可以编写自定义钩子，如上面的useMounted。又例如，我们需要每个页面自定义标题:</font>

```javascript
function useTitle(title) {
  useEffect(
    () => {
      document.title = title;
    });
}

// 使用:
function Home() {
  const title = '我是首页'
  useTitle(title)

  return (
    <div>{title}</div>
  )
}
```

**<font style="color:rgb(44, 62, 80);">React Hooks 的限制</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781979264-5ffed9d3-565e-439a-917a-c76a9a6f236e.png)

+ <font style="color:rgb(44, 62, 80);">不要在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">循环、条件</font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">嵌套函数中调用 Hook</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">在 React 的函数组件中调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Hook</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">那为什么会有这样的限制呢？就得从 Hooks 的设计说起。Hooks 的设计初衷是为了改进 React 组件的开发模式。在旧有的开发模式下遇到了三个问题。</font>

+ <font style="color:rgb(44, 62, 80);">组件之间难以复用状态逻辑。过去常见的解决方案是高阶组件、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">及状态管理框架。</font>
+ <font style="color:rgb(44, 62, 80);">复杂的组件变得难以理解。生命周期函数与业务逻辑耦合太深，导致关联部分难以拆分。</font>
+ <font style="color:rgb(44, 62, 80);">常见的有 this 的问题，但在 React 团队中还有类难以优化的问题，他们希望在编译优化层面做出一些改进。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这三个问题在一定程度上阻碍了 React 的后续发展，所以为了解决这三个问题，Hooks 基于函数组件开始设计。然而第三个问题决定了 Hooks 只支持函数组件。</font>

<font style="color:rgb(44, 62, 80);">那为什么不要在循环、条件或嵌套函数中调用 Hook 呢？</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">因为 Hooks 的设计是基于数组实现</font><font style="color:rgb(44, 62, 80);">。在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">调用时按顺序加入数组中</font><font style="color:rgb(44, 62, 80);">，如果使用循环、条件或嵌套函数很有可能导致数组取值错位，执行错误的 Hook。当然，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">实质上 React 的源码里不是数组，是链表</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">这些限制会在编码上造成一定程度的心智负担，新手可能会写错，为了避免这样的情况，可以引入 ESLint 的 Hooks 检查插件进行预防。</font>

**<font style="color:rgb(44, 62, 80);">useEffect 与 useLayoutEffect 区别在哪里</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781980694-fe4c0752-29f8-4227-b64d-546d9870daf5.png)

+ <font style="color:rgb(44, 62, 80);">它们的共同点很简单，底层的函数签名是完全一致的，都是调用的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountEffectImpl</font><font style="color:rgb(44, 62, 80);">，在使用上也没什么差异，基本可以直接替换，也都是用于处理副作用。</font>
+ <font style="color:rgb(44, 62, 80);">那不同点就很大了，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在 React 的渲染过程中是被异步调用的，用于绝大多数场景，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LayoutEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会在所有的 DOM 变更之后同步调用，主要用于处理 DOM 操作、调整样式、避免页面闪烁等问题。也正因为是同步处理，所以需要避免在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LayoutEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">做计算量较大的耗时任务从而造成阻塞。</font>
+ <font style="color:rgb(44, 62, 80);">在未来的趋势上，两个 API 是会长期共存的，暂时没有删减合并的计划，需要开发者根据场景去自行选择。React 团队的建议非常实用，如果实在分不清，先用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);">，一般问题不大；如果页面有异常，再直接替换为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useLayoutEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">即可。</font>

### [](https://www.123fe.net/docs/base/improve.html#_21-%E5%8F%97%E6%8E%A7%E7%BB%84%E4%BB%B6%E5%92%8C%E9%9D%9E%E5%8F%97%E6%8E%A7%E7%BB%84%E4%BB%B6)<font style="color:rgb(44, 62, 80);">21 受控组件和非受控组件</font>
```javascript
<FInput value = {x} onChange = {fn} /> 
  // 上面的是受控组件 下面的是非受控组件
  <FInput defaultValue = {x} />
```

+ <font style="color:rgb(44, 62, 80);">当你一个组件同时传递一个value以及onChange事件时，它就是一个受控组件，收入输出都是我来控制的。</font>
+ <font style="color:rgb(44, 62, 80);">第二个只是传递了默认的初时值，并没有传onchange事件，</font>
+ <font style="color:rgb(44, 62, 80);">非受控组件是一种反模式，它的值不受组件自身的state或props控制</font>

### [](https://www.123fe.net/docs/base/improve.html#_22-%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8Dajax%E6%95%B0%E6%8D%AE%E8%AF%B7%E6%B1%82%E9%87%8D%E6%96%B0%E8%8E%B7%E5%8F%96)<font style="color:rgb(44, 62, 80);">22 如何避免ajax数据请求重新获取</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">一般而言，ajax请求的数据都放在redux中存取。</font>

### [](https://www.123fe.net/docs/base/improve.html#_23-%E7%BB%84%E4%BB%B6%E4%B9%8B%E9%97%B4%E9%80%9A%E4%BF%A1)<font style="color:rgb(44, 62, 80);">23 组件之间通信</font>
+ <font style="color:rgb(44, 62, 80);">父子组件通信</font>
+ <font style="color:rgb(44, 62, 80);">自定义事件</font>
+ <font style="color:rgb(44, 62, 80);">redux和context</font>

**<font style="color:rgb(44, 62, 80);">context如何运用</font>**

+ <font style="color:rgb(44, 62, 80);">父组件向其下所有子孙组件传递信息</font>
+ <font style="color:rgb(44, 62, 80);">如一些简单的信息：主题、语言</font>
+ <font style="color:rgb(44, 62, 80);">复杂的公共信息用redux</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在跨层级通信中，主要分为一层或多层的情况</font>

+ <font style="color:rgb(44, 62, 80);">如果只有一层，那么按照 React 的树形结构进行分类的话，主要有以下三种情况：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">父组件向子组件通信</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">子组件向父组件通信</font><font style="color:rgb(44, 62, 80);">以及</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">平级的兄弟组件间互相通信</font><font style="color:rgb(44, 62, 80);">。</font>
+ **<font style="color:rgb(44, 62, 80);">在父与子的情况下</font>**<font style="color:rgb(44, 62, 80);">，因为 React 的设计实际上就是传递</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">即可。那么场景体现在容器组件与展示组件之间，通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">传递</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">，让展示组件受控。</font>
+ **<font style="color:rgb(44, 62, 80);">在子与父的情况下</font>**<font style="color:rgb(44, 62, 80);">，有两种方式，分别是回调函数与实例函数。回调函数，比如输入框向父级组件返回输入内容，按钮向父级组件传递点击事件等。实例函数的情况有些特别，主要是在父组件中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">通过 React 的 ref API 获取子组件的实例</font><font style="color:rgb(44, 62, 80);">，然后是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">通过实例调用子组件的实例函数</font><font style="color:rgb(44, 62, 80);">。这种方式在过去常见于 Modal 框的显示与隐藏</font>
+ **<font style="color:rgb(44, 62, 80);">多层级间的数据通信，有两种情况</font>**<font style="color:rgb(44, 62, 80);">。第一种是一个容器中包含了多层子组件，需要最底部的子组件与顶部组件进行通信。在这种情况下，如果不断透传 Props 或回调函数，不仅代码层级太深，后续也很不好维护。第二种是两个组件不相关，在整个 React 的组件树的两侧，完全不相交。那么基于多层级间的通信一般有三个方案。</font>
    - <font style="color:rgb(44, 62, 80);">第一个是使用 React 的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Context API</font><font style="color:rgb(44, 62, 80);">，最常见的用途是做语言包国际化</font>
    - <font style="color:rgb(44, 62, 80);">第二个是使用全局变量与事件。</font>
    - <font style="color:rgb(44, 62, 80);">第三个是使用状态管理框架，比如 Flux、Redux 及 Mobx。优点是由于引入了状态管理，使得项目的开发模式与代码结构得以约束，缺点是学习成本相对较高</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781980163-83460779-92b9-464e-9c39-2480d56b9606.png)

### [](https://www.123fe.net/docs/base/improve.html#_24-%E7%B1%BB%E7%BB%84%E4%BB%B6%E4%B8%8E%E5%87%BD%E6%95%B0%E7%BB%84%E4%BB%B6%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%E5%91%A2)<font style="color:rgb(44, 62, 80);">24 类组件与函数组件有什么区别呢？</font>
+ <font style="color:rgb(44, 62, 80);">作为组件而言，类组件与函数组件在使用与呈现上没有任何不同，性能上在现代浏览器中也不会有明显差异</font>
+ <font style="color:rgb(44, 62, 80);">它们在开发时的心智模型上却存在巨大的差异。类组件是基于面向对象编程的，它主打的是继承、生命周期等核心概念；而函数组件内核是函数式编程，主打的是 immutable、没有副作用、引用透明等特点。</font>
+ <font style="color:rgb(44, 62, 80);">之前，在使用场景上，如果存在需要使用生命周期的组件，那么主推类组件；设计模式上，如果需要使用继承，那么主推类组件。</font>
+ <font style="color:rgb(44, 62, 80);">但现在由于 React Hooks 的推出，生命周期概念的淡出，函数组件可以完全取代类组件。</font>
+ <font style="color:rgb(44, 62, 80);">其次继承并不是组件最佳的设计模式，官方更推崇“组合优于继承”的设计概念，所以类组件在这方面的优势也在淡出。</font>
+ <font style="color:rgb(44, 62, 80);">性能优化上，类组件主要依靠</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> 阻断渲染来提升性能，而函数组件依靠</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">缓存渲染结果来提升性能。</font>
+ <font style="color:rgb(44, 62, 80);">从上手程度而言，类组件更容易上手，从未来趋势上看，由于React Hooks 的推出，函数组件成了社区未来主推的方案。</font>
+ <font style="color:rgb(44, 62, 80);">类组件在未来时间切片与并发模式中，由于生命周期带来的复杂度，并不易于优化。而函数组件本身轻量简单，且在 Hooks 的基础上提供了比原先更细粒度的逻辑组织与复用，更能适应 React 的未来发展。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781982359-3049f290-7c73-4238-a286-5a2791cc8506.png)

### [](https://www.123fe.net/docs/base/improve.html#_25-%E5%A6%82%E4%BD%95%E8%AE%BE%E8%AE%A1react%E7%BB%84%E4%BB%B6)<font style="color:rgb(44, 62, 80);">25 如何设计React组件</font>
<font style="color:rgb(44, 62, 80);">React 组件应从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">设计与工程实践</font><font style="color:rgb(44, 62, 80);">两个方向进行探讨</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从设计上而言，社区主流分类的方案是展示组件与灵巧组件</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">展示组件内部没有状态管理，仅仅用于最简单的展示表达</font><font style="color:rgb(44, 62, 80);">。展示组件中最基础的一类组件称作代理组件。代理组件常用于封装常用属性、减少重复代码。很经典的场景就是引入 Antd 的 Button 时，你再自己封一层。如果未来需要替换掉 Antd 或者需要在所有的 Button 上添加一个属性，都会非常方便。基于代理组件的思想还可以继续分类，分为样式组件与布局组件两种，分别是将样式与布局内聚在自己组件内部。</font>
+ <font style="color:rgb(44, 62, 80);">从工程实践而言，通过文件夹划分的方式切分代码。我初步常用的分割方式是将页面单独建立一个目录，将复用性略高的 components 建立一个目录，在下面分别建立 basic、container 和 hoc 三类。这样可以保证无法复用的业务逻辑代码尽量留在 Page 中，而可以抽象复用的部分放入 components 中。其中 basic 文件夹放展示组件，由于展示组件本身与业务关联性较低，所以可以使用 Storybook 进行组件的开发管理，提升项目的工程化管理能力</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781982466-ef57021c-281a-4eae-8851-7b126150502f.png)

### [](https://www.123fe.net/docs/base/improve.html#_26-%E7%BB%84%E4%BB%B6%E7%9A%84%E5%8D%8F%E5%90%8C%E5%8F%8A-%E4%B8%8D-%E5%8F%AF%E6%8E%A7%E7%BB%84%E4%BB%B6)<font style="color:rgb(44, 62, 80);">26 组件的协同及（不）可控组件</font>
**<font style="color:rgb(44, 62, 80);">为什么要进行组件的协同</font>**

+ <font style="color:rgb(44, 62, 80);">我们在实际的开发项目的时候，不会只用几个组件，有时候遇到大型的项目，可能会有成千上百的组件，难免会遇到有功能重复的组件。要进行修改，就会修改大部分的文件。所以我们需要进行组件的协同开发。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781982471-a6f8ee9d-77b2-409c-8e3a-daef769aa0b4.png)

**<font style="color:rgb(44, 62, 80);">什么是组件的协同使用？</font>**

+ <font style="color:rgb(44, 62, 80);">组件的协同本质上是对组件的一种组织、管理的方式。</font>
+ <font style="color:rgb(44, 62, 80);">目的：</font>
    - <font style="color:rgb(44, 62, 80);">逻辑清晰：这是组件与组件之间的逻辑</font>
    - <font style="color:rgb(44, 62, 80);">代码模块化</font>
    - <font style="color:rgb(44, 62, 80);">封装细节：像面向对象一样将常用的方法以及数据封装起来</font>
    - <font style="color:rgb(44, 62, 80);">提高代码的复用性：因为是组件，相当于一个封装好的东西，用的时候直接调用</font>

**<font style="color:rgb(44, 62, 80);">如何实现组件的协同使用</font>**

+ <font style="color:rgb(44, 62, 80);">第一种：增加一个父组件，将其他的组件进行嵌套，更多的是实现代码的封装</font>
+ <font style="color:rgb(44, 62, 80);">第二种：通过一些操作从后台获取数据，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Mixin</font><font style="color:rgb(44, 62, 80);">，更多的是实现代码的复用</font>

**<font style="color:rgb(44, 62, 80);">组件嵌套的含义</font>**

+ <font style="color:rgb(44, 62, 80);">组件嵌套的本质是父子关系</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781983712-1b13e0c5-e132-4961-81bd-83d0fd299c14.png)

**<font style="color:rgb(44, 62, 80);">组件嵌套的优缺点</font>**

+ <font style="color:rgb(44, 62, 80);">优点：</font>
    - <font style="color:rgb(44, 62, 80);">逻辑清晰：父子关系类似于人类中的父子关系</font>
    - <font style="color:rgb(44, 62, 80);">模块化开发：每个模块对应一个功能，不同的模块可以同步开发</font>
    - <font style="color:rgb(44, 62, 80);">封装细节：开发者必须要关注组件的功能，不需要了解细节</font>
+ <font style="color:rgb(44, 62, 80);">缺点：</font>
    - <font style="color:rgb(44, 62, 80);">编写难度高：父子组件的关系需要经过深思熟虑，贸然编写可能导致关系混乱，代码难以维护</font>
    - <font style="color:rgb(44, 62, 80);">无法掌握所有细节：使用者只知道组件的用法，不知道实现细节，遇到问题难以修复</font>

**<font style="color:rgb(44, 62, 80);">Mixin</font>**

**<font style="color:rgb(44, 62, 80);">Mixin的含义</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Mixin=一组方法</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">他的目的是横向抽离出组件的相似代码，把组件的共同作用以及效果的代码提出来</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781983712-bb624e92-3fdd-4b4f-9a23-d8a3a2525bdf.png)

**<font style="color:rgb(44, 62, 80);">Mixin的优缺点</font>**

+ <font style="color:rgb(44, 62, 80);">优点</font>
    - <font style="color:rgb(44, 62, 80);">代码复用：抽离出通用的代码，减少开发成本，提高开发效率</font>
    - <font style="color:rgb(44, 62, 80);">即插即用：可以使用许多现有的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Mixin</font><font style="color:rgb(44, 62, 80);">来开发自己的代码</font>
    - <font style="color:rgb(44, 62, 80);">适应性强：改动一次代码，影响多个组件</font>
+ <font style="color:rgb(44, 62, 80);">缺点</font>
    - <font style="color:rgb(44, 62, 80);">编写难度高：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Mixin</font><font style="color:rgb(44, 62, 80);">可能被用在各种环境中，想要兼容多种环境就需要更多的 - 码与逻辑，通用的代价是提高复杂度</font>
    - <font style="color:rgb(44, 62, 80);">降低代码的可读性：组件的优势在于将逻辑与是界面直接结合在一起，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Mixin</font><font style="color:rgb(44, 62, 80);">本质上会分散逻辑，理解起来难度大</font>

**<font style="color:rgb(44, 62, 80);">不可控组件</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781983691-b4ec9a62-ffc9-4418-9610-3839b77f3a54.png)

+ <font style="color:rgb(44, 62, 80);">上图：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defaultValue</font><font style="color:rgb(44, 62, 80);">的值是固定的，这就是一个不可控组件</font>
+ <font style="color:rgb(44, 62, 80);">如果要获取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">input</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">值，只有使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">获取节点来获取值</font>

**<font style="color:rgb(44, 62, 80);">可控组件</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781983881-7ffde15f-67af-4403-a88a-a69d9cb49c99.png)

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defaultValue</font><font style="color:rgb(44, 62, 80);">的值是根据状态确定了，只需要拿到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state.value</font><font style="color:rgb(44, 62, 80);">的值就可以了</font>
+ <font style="color:rgb(44, 62, 80);">这里需要注意一下：使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">的值是不可修改的，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defaultValue</font><font style="color:rgb(44, 62, 80);">的值是可以修改的</font>

**<font style="color:rgb(44, 62, 80);">可控组件的优点</font>**

+ <font style="color:rgb(44, 62, 80);">符合</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">的数据流</font>
+ <font style="color:rgb(44, 62, 80);">数据存储在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">中，便于获取</font>
+ <font style="color:rgb(44, 62, 80);">便于处理数据</font>

### [](https://www.123fe.net/docs/base/improve.html#_27-react-router-%E7%9A%84%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86%E5%8F%8A%E5%B7%A5%E4%BD%9C%E6%96%B9%E5%BC%8F%E5%88%86%E5%88%AB%E6%98%AF%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">27 React-Router 的实现原理及工作方式分别是什么</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Router</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">路由的基础实现原理分为两种，如果是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">切换 Hash</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的方式，那么依靠浏览器</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Hash</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">变化即可；如果是切换网址中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Path</font><font style="color:rgb(44, 62, 80);">，就要用到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML5 History API</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pushState</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">replaceState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等。在使用这个方式时，还需要在服务端完成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">historyApiFallback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">配置</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Router</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内部主要依靠</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">history</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">库完成，这是由</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Router</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">自己封装的库，为了实现跨平台运行的特性，内部提供两套基础</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">history</font><font style="color:rgb(44, 62, 80);">，一套是直接使用浏览器的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">History API</font><font style="color:rgb(44, 62, 80);">，用于支持</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-router-dom</font><font style="color:rgb(44, 62, 80);">；另一套是基于内存实现的版本，这是自己做的一个数组，用于支持</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-router-native</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Router</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的工作方式可以分为设计模式与关键模块两个部分。从设计模式的角度出发，在架构上通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Monorepo</font><font style="color:rgb(44, 62, 80);">进行库的管理。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Monorepo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">具有团队间透明、迭代便利的优点。其次在整体的数据通信上使用了 Context API 完成上下文传递。</font>
+ <font style="color:rgb(44, 62, 80);">在关键模块上，主要分为三类组件：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">第一类是 Context 容器</font><font style="color:rgb(44, 62, 80);">，比如 Router 与 MemoryRouter；</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">第二类是消费者组件，用以匹配路由</font><font style="color:rgb(44, 62, 80);">，主要有 Route、Redirect、Switch 等；</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">第三类是与平台关联的功能组件</font><font style="color:rgb(44, 62, 80);">，比如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Link、NavLink、DeepLinking</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781984013-289c25ae-70d0-4a93-a535-4c47e22d4d38.png)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781984614-84c2fc9a-3ea9-4c1c-887c-91608cbb6e7a.png)

[React router原理分析(opens new window)](https://www.123fe.net/principle-docs/react/01-React-router%E5%8E%9F%E7%90%86.html)

### [](https://www.123fe.net/docs/base/improve.html#_28-react-17-%E5%B8%A6%E6%9D%A5%E4%BA%86%E5%93%AA%E4%BA%9B%E6%94%B9%E5%8F%98)<font style="color:rgb(44, 62, 80);">28 React 17 带来了哪些改变</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">最重要的是以下三点：</font>

+ <font style="color:rgb(44, 62, 80);">新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">转换逻辑</font>
+ <font style="color:rgb(44, 62, 80);">事件系统重构</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Lane 模型</font><font style="color:rgb(44, 62, 80);">的引入</font>

**<font style="color:rgb(44, 62, 80);">1. 重构 JSX 转换逻辑</font>**

<font style="color:rgb(44, 62, 80);">在过去，如果我们在 React 项目中写入下面这样的代码：</font>

```javascript
function MyComponent() {
  return <p>这是我的组件</p>
}
```

<font style="color:rgb(44, 62, 80);">React 是会报错的，原因是 React 中对 JSX 代码的转换依赖的是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个函数。因此但凡我们在代码中包含了 JSX，那么就必须在文件中引入 React，像下面这样：</font>

```javascript
import React from 'react';
function MyComponent() {
  return <p>这是我的组件</p>
}
```

<font style="color:rgb(44, 62, 80);">而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 17 则允许我们在不引入 React 的情况下直接使用 JSX</font><font style="color:rgb(44, 62, 80);">。这是因为在 React 17 中，编译器会自动帮我们引入 JSX 的解析器，也就是说像下面这样一段逻辑：</font>

```javascript
function MyComponent() {
  return <p>这是我的组件</p>
}
```

<font style="color:rgb(44, 62, 80);">会被编译器转换成这个样子：</font>

```javascript
import {jsx as _jsx} from 'react/jsx-runtime';
function MyComponent() {
  return _jsx('p', { children: '这是我的组件' });
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react/jsx-runtime</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的 JSX 解析器将取代</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">完成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的编译工作，这个过程对开发者而言是自动化、无感知的。因此，新的 JSX 转换逻辑带来的最显著的改变就是降低了开发者的学习成本。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react/jsx-runtime</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的 JSX 解析器看上去似乎在调用姿势上和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">区别不大，那么它是否只是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">换了个马甲呢？当然不是，它在内部实现了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">无法做到的性能优化和简化。在一定情况下，它可能会略微改善编译输出内容的大小</font>

**<font style="color:rgb(44, 62, 80);">2. 事件系统重构</font>**

<font style="color:rgb(44, 62, 80);">事件系统在 React 17 中的重构要从以下两个方面来看：</font>

+ <font style="color:rgb(44, 62, 80);">卸掉历史包袱</font>
+ <font style="color:rgb(44, 62, 80);">拥抱新的潮流</font>

**<font style="color:rgb(44, 62, 80);">2.1 卸掉历史包袱：放弃利用 document 来做事件的中心化管控</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">React 16.13.x 版本中的事件系统会通过将所有事件冒泡到 document 来实现对事件的中心化管控</font>

<font style="color:rgb(44, 62, 80);">这样的做法虽然看上去已经足够巧妙，但仍然有它不聪明的地方——document 是整个文档树的根节点，操作 document 带来的影响范围实在是太大了，这将会使事情变得更加不可控</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在 React 17 中，React 团队终于正面解决了这个问题：事件的中心化管控不会再全部依赖</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，管控相关的逻辑被转移到了每个 React 组件自己的容器 DOM 节点中。比如说我们在 ID 为 root 的 DOM 节点下挂载了一个 React 组件，像下面代码这样：</font>

```javascript
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

<font style="color:rgb(44, 62, 80);">那么事件管控相关的逻辑就会被安装到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">root 节点</font><font style="color:rgb(44, 62, 80);">上去。这样一来， React 组件就能够自己玩自己的，再也无法对全局的事件流构成威胁了</font>

**<font style="color:rgb(44, 62, 80);">2.2 拥抱新的潮流：放弃事件池</font>**

<font style="color:rgb(44, 62, 80);">在 React 17 之前，合成事件对象会被放进一个叫作“事件池”的地方统一管理。这样做的目的是能够实现事件对象的复用，进而提高性能：每当事件处理函数执行完毕后，其对应的合成事件对象内部的所有属性都会被置空，意在为下一次被复用做准备。这也就意味着事件逻辑一旦执行完毕，我们就拿不到事件对象了，React 官方给出的这个例子就很能说明问题，请看下面这个代码</font>

```javascript
function handleChange(e) {
  // This won't work because the event object gets reused.
  setTimeout(() => {
    console.log(e.target.value); // Too late!
  }, 100);
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">异步执行的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">回调会在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">handleChange</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个事件处理函数执行完毕后执行，因此它拿不到想要的那个事件对象</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">e</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

<font style="color:rgb(44, 62, 80);">要想拿到目标事件对象，必须显式地告诉 React——我永远需要它，也就是调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">e.persist()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，像下面这样：</font>

```javascript
function handleChange(e) {
  // Prevents React from resetting its properties:
  e.persist();
  setTimeout(() => {
    console.log(e.target.value); // Works
  }, 100);
}
```

<font style="color:rgb(44, 62, 80);">在 React 17 中，我们不需要</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">e.persist()</font><font style="color:rgb(44, 62, 80);">，也可以随时随地访问我们想要的事件对象。</font>

**<font style="color:rgb(44, 62, 80);">3. Lane 模型的引入</font>**

<font style="color:rgb(44, 62, 80);">初学 React 源码的同学由此可能会很自然地认为：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">优先级就应该是用 Lane 来处理的</font><font style="color:rgb(44, 62, 80);">。但事实上，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 16 中处理优先级采用的是 expirationTime 模型</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">expirationTime</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">模型使用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">expirationTime</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（一个时间长度） 来描述任务的优先级；而</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Lane 模型</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">则使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">二进制数来表示任务的优先级</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">：</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">lane 模型</font><font style="color:rgb(44, 62, 80);">通过将不同优先级赋值给一个位，通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">31 位的位运算</font><font style="color:rgb(44, 62, 80);">来操作优先级。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Lane 模型</font><font style="color:rgb(44, 62, 80);">提供了一个新的优先级排序的思路，相对于 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">expirationTime</font><font style="color:rgb(44, 62, 80);"> 来说，它对优先级的处理会更细腻，能够覆盖更多的边界条件。</font>

