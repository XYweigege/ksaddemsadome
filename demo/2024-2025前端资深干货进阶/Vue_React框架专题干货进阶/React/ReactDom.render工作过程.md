<font style="color:rgb(44, 62, 80);">从本讲开始，我们将以首次渲染为切入点，拆解 Fiber 架构下 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> 所触发的渲染链路，结合源码理解整个链路中所涉及的初始化、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render 和 commit</font><font style="color:rgb(44, 62, 80);"> 等过程</font>

## [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#%E4%B8%80%E3%80%81reactdom-render-%E8%B0%83%E7%94%A8%E6%A0%88%E7%9A%84%E9%80%BB%E8%BE%91%E5%88%86%E5%B1%82)一、ReactDOM.render 调用栈的逻辑分层
<font style="color:rgb(44, 62, 80);">开篇先给到你一个简单的 React AppDemo：</font>

```javascript
import React from "react";
import ReactDOM from "react-dom";

function App() {
  return (
    <div className="App">
    <div className="container">
    <h1>我是标题</h1>
    <p>我是第一段话</p>
    <p>我是第二段话</p>
    </div>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

<font style="color:rgb(44, 62, 80);">Demo 启动后，渲染出的界面如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873496825-cb06f0c4-4d5c-46de-95e3-4f325cd8fe97.png)

<font style="color:rgb(44, 62, 80);">现在请你打开 Chrome 的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Performance</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">面板，点击下图红色圈圈所圈住的这个“记录”按钮：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873497438-1826ba7e-8a87-4b95-9676-bff30be73573.png)

<font style="color:rgb(44, 62, 80);">然后重新访问 Demo 页面对应的本地服务地址，待页面刷新后，终止记录，便能够得到如下图右下角所示的这样一个调用栈大图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873496990-97d74d7e-8e95-4e4a-963f-38f104340aeb.png)

<font style="color:rgb(44, 62, 80);">放大该图，定位“</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">src/index.js</font><font style="color:rgb(44, 62, 80);">”这个文件路径，我们就可以找到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法对应的调用栈，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873496954-0b3fab1e-dbf1-44c7-97bd-b1afed1edaff.png)

<font style="color:rgb(44, 62, 80);">从图中你可以看到，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法对应的调用栈非常深，中间涉及的函数量也比较大。如果这张图使你心里发虚，请先不要急于撤退——分析调用栈只是我们理解渲染链路的一个手段，我们的目的是借此提取关键逻辑，而非理解调用栈中的每一个方法。就这张图来说，你首先需要把握的，就是整个调用链路中所包含的三个阶段</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873496846-bea043f2-54c8-4d42-b579-22e3ae5ebdbb.png)

<font style="color:rgb(44, 62, 80);">图中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scheduleUpdateOnFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的作用是调度更新，在由</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">发起的首屏渲染这个场景下，它触发的就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performSyncWorkOnRoot</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performSyncWorkOnRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">开启的正是我们反复强调的</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">render 阶段</font>**<font style="color:rgb(44, 62, 80);">；而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commitRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法开启的则是真实 DOM 的渲染过程（</font>**<font style="color:rgb(44, 62, 80);">commit 阶段</font>**<font style="color:rgb(44, 62, 80);">）。因此以</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scheduleUpdateOnFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commitRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">两个方法为界，我们可以大致把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的调用栈划分为三个阶段：</font>

+ <font style="color:rgb(44, 62, 80);">初始化阶段</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">阶段</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commit</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">阶段</font>

## [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#%E4%BA%8C%E3%80%81%E5%88%9D%E5%A7%8B%E5%8C%96%E9%98%B6%E6%AE%B5)二、初始化阶段
### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#%E6%8B%86%E8%A7%A3-reactdom-render-%E8%B0%83%E7%94%A8%E6%A0%88-%E5%88%9D%E5%A7%8B%E5%8C%96%E9%98%B6%E6%AE%B5)<font style="color:rgb(44, 62, 80);">拆解 ReactDOM.render 调用栈——初始化阶段</font>
<font style="color:rgb(44, 62, 80);">首先我们提取出初始化过程中涉及的调用栈大图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873498706-19cf261a-c266-4a89-aa8e-af1158ee8e59.png)

<font style="color:rgb(44, 62, 80);">图中的方法虽然看上去又多又杂，但做的事情清清爽爽，那就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">完成 Fiber 树中基本实体的创建</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">什么是基本实体？基本实体有哪些？问题的答案藏在源码里，这里我为你提取了源码中的关键逻辑，首先是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">legacyRenderSubtreeIntoContainer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法。在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数体中，以下面代码所示的姿势调用了它：</font>

```javascript
return legacyRenderSubtreeIntoContainer(null, element, container, false, callback);
```

<font style="color:rgb(44, 62, 80);">而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">legacyRenderSubtreeIntoContainer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的关键逻辑如下（解析在注释里）：</font>

```javascript
function legacyRenderSubtreeIntoContainer(parentComponent, children, container, forceHydrate, callback) {
  // container 对应的是我们传入的真实 DOM 对象
  var root = container._reactRootContainer;
  // 初始化 fiberRoot 对象
  var fiberRoot;
  // DOM 对象本身不存在 _reactRootContainer 属性，因此 root 为空
  if (!root) {
    // 若 root 为空，则初始化 _reactRootContainer，并将其值赋值给 root
    root = container._reactRootContainer = legacyCreateRootFromDOMContainer(container, forceHydrate);
    // legacyCreateRootFromDOMContainer 创建出的对象会有一个 _internalRoot 属性，将其赋值给 fiberRoot
    fiberRoot = root._internalRoot;

    // 这里处理的是 ReactDOM.render 入参中的回调函数，你了解即可
    if (typeof callback === 'function') {
      var originalCallback = callback;
      callback = function () {
        var instance = getPublicRootInstance(fiberRoot);
        originalCallback.call(instance);
      };
    } // Initial mount should not be batched.
    // 进入 unbatchedUpdates 方法
    unbatchedUpdates(function () {
      updateContainer(children, fiberRoot, parentComponent, callback);
    });
  } else {
    // else 逻辑处理的是非首次渲染的情况（即更新），其逻辑除了跳过了初始化工作，与楼上基本一致
    fiberRoot = root._internalRoot;
    if (typeof callback === 'function') {
      var _originalCallback = callback;
      callback = function () {
        var instance = getPublicRootInstance(fiberRoot);
        _originalCallback.call(instance);
      };
    } // Update

    updateContainer(children, fiberRoot, parentComponent, callback);
  }
  return getPublicRootInstance(fiberRoot);
}
```

<font style="color:rgb(44, 62, 80);">这里我为你总结一下首次渲染过程中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">legacyRenderSubtreeIntoContainer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的主要逻辑链路：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873507409-c3edb3f4-6825-4e4e-95a6-8c7d868b7169.png)

<font style="color:rgb(44, 62, 80);">在这个流程中，你需要关注到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiberRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个对象。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiberRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">到底是什么呢？这里我将运行时的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">root 和 fiberRoot</font><font style="color:rgb(44, 62, 80);">为你截取出来，其中 root 对象的结构如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873506830-df5addff-5186-4665-a555-c4c55874877a.png)

<font style="color:rgb(44, 62, 80);">可以看出，root 对象（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">container._reactRootContainer</font><font style="color:rgb(44, 62, 80);">）上有一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_internalRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，这个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_internalRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">也就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiberRoot</font><font style="color:rgb(44, 62, 80);">。fiberRoot 的本质是一个 FiberRootNode 对象，其中包含一个 current 属性，该属性同样需要划重点。这里我为你高亮出 current 属性的部分内容：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873507047-7a54d1d0-3f31-4b8f-8e2a-e94219cbc5b1.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">或许你会对</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象包含的海量属性感到陌生和头大，但这并不妨碍你 Get 到“</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current 对象是一个 FiberNode 实例</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">”这一点，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FiberNode</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，正是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">节点对应的对象类型。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象是一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">节点，不仅如此，它还是当前</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">树的头部节点</font>

<font style="color:rgb(44, 62, 80);">考虑到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FiberNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点，在调用栈中实际是由</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createHostRootFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法创建的，React 源码中也有多处以</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">代指</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象，因此下文中我们将以</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber 指代 current 对象</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">读到这里，你脑海中应该不难形成一个这样的指向关系：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873506963-e34656af-82cc-4eeb-9980-28ad99757048.png)

<font style="color:rgb(44, 62, 80);">其中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiberRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的关联对象是真实</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的容器节点；而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">则作为虚拟 DOM 的根节点存在。这两个节点，将是后续整棵</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树构建的起点。</font>

<font style="color:rgb(44, 62, 80);">接下来，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiberRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的其他入参一起，被传入</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateContainer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法，从而形成一个回调。这个回调，正是接下来要调用的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unbatchedUpdates</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的入参。我们一起看看</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unbatchedUpdates</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">做了什么，下面代码是对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unbatchedUpdates</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">主体逻辑的提取：</font>

```javascript
function unbatchedUpdates(fn, a) {
  // 这里是对上下文的处理，不必纠结
  var prevExecutionContext = executionContext;
  executionContext &= ~BatchedContext;
  executionContext |= LegacyUnbatchedContext;
  try {
    // 重点在这里，直接调用了传入的回调函数 fn，对应当前链路中的 updateContainer 方法
    return fn(a);
  } finally {
    // finally 逻辑里是对回调队列的处理，此处不用太关注
    executionContext = prevExecutionContext;
    if (executionContext === NoContext) {
      // Flush the immediate callbacks that were scheduled during this batch
      resetRenderTimer();
      flushSyncCallbackQueue();
    }
  }
}
```

<font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unbatchedUpdates</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数体里，当下你只需要 Get 到一个信息：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">它直接调用了传入的回调 fn</font><font style="color:rgb(44, 62, 80);">。而在当前链路中，fn 是什么呢？fn 是一个针对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateContainer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的调用：</font>

```plain
unbatchedUpdates(function () {
  updateContainer(children, fiberRoot, parentComponent, callback);
});
```

<font style="color:rgb(44, 62, 80);">接下来我们很有必要去看看</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateContainer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里面的逻辑。这里我将主体代码提取如下（解析在注释里，如果没有耐心读完可以直接看文字解读）：</font>

```javascript
function updateContainer(element, container, parentComponent, callback) {
  ......

  // 这是一个 event 相关的入参，此处不必关注
  var eventTime = requestEventTime();

  ......

  // 这是一个比较关键的入参，lane 表示优先级
  var lane = requestUpdateLane(current$1);
  // 结合 lane（优先级）信息，创建 update 对象，一个 update 对象意味着一个更新
  var update = createUpdate(eventTime, lane); 

  // update 的 payload 对应的是一个 React 元素
  update.payload = {
    element: element
  };

  // 处理 callback，这个 callback 其实就是我们调用 ReactDOM.render 时传入的 callback
  callback = callback === undefined ? null : callback;
  if (callback !== null) {
    {
      if (typeof callback !== 'function') {
        error('render(...): Expected the last optional `callback` argument to be a ' + 'function. Instead received: %s.', callback);
      }
    }
    update.callback = callback;
  }

  // 将 update 入队
  enqueueUpdate(current$1, update);
  // 调度 fiberRoot 
  scheduleUpdateOnFiber(current$1, lane, eventTime);
  // 返回当前节点（fiberRoot）的优先级
  return lane;
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateContainer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的逻辑相对来说丰富了点，但大部分逻辑也是在干杂活，它做的最关键的事情可以总结为三件：</font>

+ <font style="color:rgb(44, 62, 80);">请求当前</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点的 lane（优先级）；</font>
+ <font style="color:rgb(44, 62, 80);">结合</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">lane</font><font style="color:rgb(44, 62, 80);">（优先级），创建当前</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">update</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象，并将其入队；</font>
+ <font style="color:rgb(44, 62, 80);">调度当前节点（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);">）。</font>

<font style="color:rgb(44, 62, 80);">函数体中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scheduleWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">其实就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scheduleUpdateOnFiber</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scheduleUpdateOnFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数的任务是调度当前节点的更新。在这个函数中，会处理一系列与优先级、打断操作相关的逻辑。但是在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">发起的首次渲染链路中，这些意义都不大，因为这个渲染过程其实是同步的。我们可以尝试在 Source 面板中为该函数打上断点，逐行执行代码，会发现逻辑最终会走到下图的高亮处：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873514262-8db6b427-8056-4365-9788-3976013e1e5a.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performSyncWorkOnRoot</font><font style="color:rgb(44, 62, 80);">直译过来就是“执行根节点的同步任务”，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">这里的“同步”二字需要注意，它明示了接下来即将开启的是一个同步的过程</font><font style="color:rgb(44, 62, 80);">。这也正是为什么在整个渲染链路中，调度（Schedule）动作没有存在感的原因。</font>

<font style="color:rgb(44, 62, 80);">前面我们曾经提到过，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performSyncWorkOnRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">阶段的起点，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render 阶段</font><font style="color:rgb(44, 62, 80);">的任务就是完成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树的构建，它是整个渲染链路中最核心的一环。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">在异步渲染的模式下，render 阶段应该是一个可打断的异步过程</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">而现在，我相信你心里更多的疑惑在于：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">都说 Fiber 架构带来的异步渲染是 React 16 的亮点，为什么分析到现在，竟然发现 ReactDOM.render 触发的首次渲染是个同步过程呢</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#%E5%90%8C%E6%AD%A5%E7%9A%84-reactdom-render-%E5%BC%82%E6%AD%A5%E7%9A%84-reactdom-createroot)<font style="color:rgb(44, 62, 80);">同步的 ReactDOM.render，异步的 ReactDOM.createRoot</font>
<font style="color:rgb(44, 62, 80);">其实在 React 16，包括近期发布的 React 17 小版本中，React 都有以下 3 种启动方式：</font>

**<font style="color:rgb(44, 62, 80);">legacy 模式：</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render(<App />, rootNode)</font><font style="color:rgb(44, 62, 80);">。这是当前</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React App</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">使用的方式，当前没有计划删除本模式，但是这个模式可能不支持这些新功能。</font>

**<font style="color:rgb(44, 62, 80);">blocking 模式：</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.createBlockingRoot(rootNode).render(<App />)</font><font style="color:rgb(44, 62, 80);">。目前正在实验中，作为迁移到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">concurrent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">模式的第一个步骤</font>

**<font style="color:rgb(44, 62, 80);">concurrent 模式：</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.createRoot(rootNode).render(<App />)</font><font style="color:rgb(44, 62, 80);">。目前在实验中，未来稳定之后，打算作为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">的默认开发模式，这个模式开启了所有的新功能</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在这 3 种模式中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">我们常用的 ReactDOM.render 对应的是 legacy 模式，它实际触发的仍然是同步的渲染链路</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">blocking</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">模式可以理解为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">legacy</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">concurrent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之间的一个过渡形态，之所以会有这个模式，是因为 React 官方希望能够提供渐进的迁移策略，帮助我们更加顺滑地过渡到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Concurrent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">模式。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">blocking</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在实际应用中是比较低频的一个模式，了解即可。</font>

<font style="color:rgb(44, 62, 80);">按照官方的说法，“</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">长远来看，模式的数量会收敛，不用考虑不同的模式</font><font style="color:rgb(44, 62, 80);">，但就目前而言，模式是一项重要的迁移策略，让每个人都能决定自己什么时候迁移，并按照自己的速度进行迁移”。由此可以看出，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Concurrent 模式确实是 React 的终极目标</font><font style="color:rgb(44, 62, 80);">，也是其创作团队使用 Fiber 架构重写核心算法的动机所在。</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#%E6%8B%93%E5%B1%95-%E5%85%B3%E4%BA%8E%E5%BC%82%E6%AD%A5%E6%A8%A1%E5%BC%8F%E4%B8%8B%E7%9A%84%E9%A6%96%E6%AC%A1%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF)<font style="color:rgb(44, 62, 80);">拓展：关于异步模式下的首次渲染链路</font>
<font style="color:rgb(44, 62, 80);">当下，如果想要开启异步渲染，我们需要调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.createRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法来启动应用，那</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.createRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">开启的渲染链路与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">有何不同呢？</font>

<font style="color:rgb(44, 62, 80);">这里我修改一下调用方式，给你展示一下调用栈。由于本讲的源码取材于 React 17.0.0 版本，在这个版本中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">仍然是一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unstable</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的方法。因此实际调用的 API 应该是“</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unstable_createRoot</font><font style="color:rgb(44, 62, 80);">”：</font>

```plain
ReactDOM.unstable_createRoot(rootElement).render(<App />);
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Concurrent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">模式开启后，首次渲染的调用栈变成了如下图所示的样子：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873509319-8a3d08bf-ec66-4410-878a-c82baa88e5b8.png)

<font style="color:rgb(44, 62, 80);">乍一看，好像和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">差别很大，其实不然。图中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所触发的逻辑仍然是一些准备性质的初始化工作，此处不必太纠结。关键在于下面我给你框出来的这部分，如下图所示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873513441-8767d1e0-69be-4880-85f4-65223591c710.png)

<font style="color:rgb(44, 62, 80);">我们拉近一点来看，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873509485-84384b72-96f6-4102-9386-a0125001648e.png)

<font style="color:rgb(44, 62, 80);">你会发现这地方也调用了一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">。再顺着这个调用往下看，发现有大量的熟悉面孔：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateContainer</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestUpdateLane</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createUpdate</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scheduleUpdateOnFiber</font><font style="color:rgb(44, 62, 80);">......这些函数在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的调用栈中也出现过。</font>

<font style="color:rgb(44, 62, 80);">其实，当前你看到的这个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">调用链路，和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的调用链路是非常相似的，主要的区别在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scheduleUpdateOnFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的这个判断里：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873513414-a35c408a-eb5c-4a12-9c9a-c73902455fdf.png)

<font style="color:rgb(44, 62, 80);">在异步渲染模式下，由于请求到的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">lane</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不再是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SyncLane</font><font style="color:rgb(44, 62, 80);">（同步优先级），故不会再走到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performSyncWorkOnRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个调用，而是会转而执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">else</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中调度相关的逻辑。</font>

<font style="color:rgb(44, 62, 80);">这里有个点要给你点出来——React 是如何知道当前处于哪个模式的呢？我们可以以</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestUpdateLane</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数为例，下面是它局部的代码：</font>

```javascript
function requestUpdateLane(fiber) {
  // 获取 mode 属性
  var mode = fiber.mode;
  // 结合 mode 属性判断当前的
  if ((mode & BlockingMode) === NoMode) {
    return SyncLane;
  } else if ((mode & ConcurrentMode) === NoMode) {
    return getCurrentPriorityLevel() === ImmediatePriority$1 ? SyncLane : SyncBatchedLane;
  }
    ......
  return lane;
}
```

<font style="color:rgb(44, 62, 80);">上面代码中需要注意</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiber</font><font style="color:rgb(44, 62, 80);">节点上的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 将会通过修改 mode 属性为不同的值，来标识当前处于哪个渲染模式；在执行过程中，也是通过判断这个属性，来区分不同的渲染模式</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">因此不同的渲染模式在挂载阶段的差异，本质上来说并不是工作流的差异（其工作流涉及 初始化 → render → commit 这 3 个步骤），而是 mode 属性的差异。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mode 属性决定着这个工作流是一气呵成（同步）的，还是分片执行（异步）的</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#fiber-%E6%9E%B6%E6%9E%84%E4%B8%80%E5%AE%9A%E6%98%AF%E5%BC%82%E6%AD%A5%E6%B8%B2%E6%9F%93%E5%90%97)<font style="color:rgb(44, 62, 80);">Fiber 架构一定是异步渲染吗？</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 16</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">如果没有开启</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Concurrent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">模式，那它还能叫</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">架构吗？</font>

<font style="color:rgb(44, 62, 80);">从动机上来看，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber 架构的设计确实主要是为了 Concurrent 而存在</font><font style="color:rgb(44, 62, 80);">。但经过了本讲紧贴源码的讲解，相信你也能够看出，在 React 16，包括已发布的 React 17 版本中，不管是否是 Concurrent，整个数据结构层面的设计、包括贯穿整个渲染链路的处理逻辑，已经完全用 Fiber 重构了一遍。站在这个角度来看，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber 架构在 React 中并不能够和异步渲染画严格的等号，它是一种同时兼容了同步渲染与异步渲染的设计</font><font style="color:rgb(44, 62, 80);">。</font>

## [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#%E4%B8%89%E3%80%81render-%E9%98%B6%E6%AE%B5)三、render 阶段
### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#%E6%8B%86%E8%A7%A3-reactdom-render-%E8%B0%83%E7%94%A8%E6%A0%88-render-%E9%98%B6%E6%AE%B5)<font style="color:rgb(44, 62, 80);">拆解 ReactDOM.render 调用栈——render 阶段</font>
<font style="color:rgb(44, 62, 80);">首先，我们复习一下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render 阶段</font><font style="color:rgb(44, 62, 80);">在整个渲染链路中的定位，如下图所示。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873511789-51cdfd67-14a1-4b04-b74f-44a0167bbb3f.png)

<font style="color:rgb(44, 62, 80);">图中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performSyncWorkOnRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标志着</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render 阶段</font><font style="color:rgb(44, 62, 80);">的开始，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">finishSyncRender</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标志着</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">阶段的结束。这中间包含了大量的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">调用栈，正是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的工作内容。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这两个方法需要注意，它们串联起的是一个“模拟递归”的过程。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 15 下的调和过程是一个递归的过程</font><font style="color:rgb(44, 62, 80);">。而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">架构下的调和过程，虽然</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">并不是依赖递归来实现的</font><font style="color:rgb(44, 62, 80);">，但在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">触发的同步模式下，它仍然是一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">深度优先搜索</font><font style="color:rgb(44, 62, 80);">的过程。在这个过程中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将创建新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">则负责将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点映射为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点。</font>

<font style="color:rgb(44, 62, 80);">我们的 Fiber 树都还长这个样子：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873511825-6317c9f1-20d6-4e41-a3b8-4a9acc95782a.png)

<font style="color:rgb(44, 62, 80);">就这么个样子，你遍历它，能遍历出来什么？到底怎么个遍历法？接下来我们就深入到源码里去一探究竟！</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#workinprogress-%E8%8A%82%E7%82%B9%E7%9A%84%E5%88%9B%E5%BB%BA)<font style="color:rgb(44, 62, 80);">workInProgress 节点的创建</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performSyncWorkOnRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);"> 是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">阶段的起点，而这个函数最关键的地方在于它调用了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">renderRootSync</font><font style="color:rgb(44, 62, 80);">。下面我们放大</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Performance</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">调用栈，来看看</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">renderRootSync</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">被调用后，紧接着发生了什么：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873513393-adc6ad31-b7ad-4fff-88bf-9bc86048778c.png)

<font style="color:rgb(44, 62, 80);">紧随其后的是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prepareFreshStack</font><font style="color:rgb(44, 62, 80);">，这里不卖关子，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prepareFreshStack</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的作用是重置一个新的堆栈环境，其中最需要我们关注的步骤，就是对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createWorkInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的调用。以下我对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createWorkInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的主要逻辑进行了提取（解析在注释里）：</font>

```javascript
// 这里入参中的 current 传入的是现有树结构中的 rootFiber 对象
function createWorkInProgress(current, pendingProps) {
  var workInProgress = current.alternate;
  // ReactDOM.render 触发的首屏渲染将进入这个逻辑
  if (workInProgress === null) {
    // 这是需要你关注的第一个点，workInProgress 是 createFiber 方法的返回值
    workInProgress = createFiber(current.tag, pendingProps, current.key, current.mode);
    workInProgress.elementType = current.elementType;
    workInProgress.type = current.type;
    workInProgress.stateNode = current.stateNode;
    // 这是需要你关注的第二个点，workInProgress 的 alternate 将指向 current
    workInProgress.alternate = current;
    // 这是需要你关注的第三个点，current 的 alternate 将反过来指向 workInProgress
    current.alternate = workInProgress;
  } else {
    // else 的逻辑此处先不用关注
  }

  // 以下省略大量 workInProgress 对象的属性处理逻辑
  // 返回 workInProgress 节点
  return workInProgress;
}
```

<font style="color:rgb(44, 62, 80);">首先要声明的是，该函数中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">入参指的是现有树结构中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873513414-682efa65-4b2f-4e3c-86eb-3c1b65953bab.png)

<font style="color:rgb(44, 62, 80);">源码太长（其实经过处理已经不长了）不看版的重点如下：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createWorkInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createFiber</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的返回值；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">alternate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将指向</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">alternate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将反过来指向</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">理解了这三点，你就会自然而然地想知道</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的本体到底是什么样的，也就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">到底会返回什么。下面我们就看看</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的逻辑：</font>

```plain
var createFiber = function (tag, pendingProps, key, mode) {

  return new FiberNode(tag, pendingProps, key, mode);
};
```

<font style="color:rgb(44, 62, 80);">代码出奇的简单，但信息却给得很到位 ——</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createFiber 将创建一个 FiberNode 实例，而 FiberNode，它正是 Fiber 节点的类型</font><font style="color:rgb(44, 62, 80);">。因此</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress 就是一个 Fiber 节点</font><font style="color:rgb(44, 62, 80);">。不仅如此，细心的你可能还会发现</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的创建入参其实来源于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(44, 62, 80);">，如下面代码所示：</font>

```plain
workInProgress = createFiber(current.tag, pendingProps, current.key, current.mode);
```

**<font style="color:rgb(44, 62, 80);">workInProgress 节点其实就是 current 节点（即 rootFiber）的副本</font>**

<font style="color:rgb(44, 62, 80);">再结合  current 指向 rootFiber 对象（同样是 FiberNode 实例），以及 current 和 workInProgress 通过 alternate 互相连接这些信息，我们可以分析出这波操作执行完之后，整棵树的结构应该如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873514733-493fd6d8-0674-46f4-9d2d-b16c1d850150.png)

<font style="color:rgb(44, 62, 80);">完成了这个任务之后，就会进入 workLoopSync 的逻辑。这个 workLoopSync 函数也是个“人狠话不多”的主，它的逻辑同样是简洁明了的，如下所示（解析在注释里）：</font>

```plain
function workLoopSync() {
  // 若 workInProgress 不为空
  while (workInProgress !== null) {
    // 针对它执行 performUnitOfWork 方法
    performUnitOfWork(workInProgress);
  }
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workLoopSync</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">做的事情就是通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">while</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">循环反复判断</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是否为空，并在不为空的情况下针对它执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUnitOfWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">而</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUnitOfWork</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数将触发对</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的调用，进而实现对新</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">节点的创建。若</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">所创建的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">节点不为空，则</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUniOfWork</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会用这个新的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">节点来更新</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的值，为下一次循环做准备。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">通过循环调用 performUnitOfWork 来触发 beginWork，新的 Fiber 节点就会被不断地创建</font><font style="color:rgb(44, 62, 80);">。当</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">终于为空时，说明没有新的节点可以创建了，也就意味着已经完成对整棵</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树的构建。</font>

<font style="color:rgb(44, 62, 80);">在这个过程中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">每一个被创建出来的新 Fiber 节点，都会一个一个挂载为最初那个 workInProgress 节点（如下图高亮处）的后代节点</font><font style="color:rgb(44, 62, 80);">。而上述过程中构建出的这棵 Fiber 树，也正是大名鼎鼎的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress 树</font><font style="color:rgb(44, 62, 80);">。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873515133-9626aee5-7dc9-4420-acff-e3e8a928bad8.png)

<font style="color:rgb(44, 62, 80);">相应地，图中 current 指针所指向的根节点所在的那棵树，我们叫它“</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current 树</font><font style="color:rgb(44, 62, 80);">”。</font>

<font style="color:rgb(44, 62, 80);">相应地，图中 current 指针所指向的根节点所在的那棵树，我们叫它“current 树”。</font>

<font style="color:rgb(44, 62, 80);">这时候，相信一些同学心里已经开始犯嘀咕了：一棵 current 树，一棵 workInProgress 树，这名堂也太多了吧！况且这两棵 Fiber 树至少在现在看来，是完全没区别的（毕竟都还只有一个根节点，哈哈）。React 这样设计的目的何在？或者换个问法——到底是什么样的事情一棵树做不到，非得搞两棵“一样”的树出来？</font>

<font style="color:rgb(44, 62, 80);">在一步一步理解 Fiber 树的构建和更新过程之后，我将带你去认识“两棵 Fiber 树”这一现象背后的动机。</font>

<font style="color:rgb(44, 62, 80);">接下来我们就深入到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的逻辑里去，一起看看</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树的构建过程及最终形态。</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#beginwork-%E5%BC%80%E5%90%AF-fiber-%E8%8A%82%E7%82%B9%E5%88%9B%E5%BB%BA%E8%BF%87%E7%A8%8B)<font style="color:rgb(44, 62, 80);">beginWork 开启 Fiber 节点创建过程</font>
<font style="color:rgb(44, 62, 80);">有一说一，beginWork 的源码实在是长到不科学。这里我们本着抓主要矛盾的原则，针对与树构建过程强相关的动作进行逻辑提取，代码如下（解析在注释里）：</font>

```javascript
function beginWork(current, workInProgress, renderLanes) {
  ......

  //  current 节点不为空的情况下，会加一道辨识，看看是否有更新逻辑要处理
  if (current !== null) {
    // 获取新旧 props
    var oldProps = current.memoizedProps;
    var newProps = workInProgress.pendingProps;

    // 若 props 更新或者上下文改变，则认为需要"接受更新"
    if (oldProps !== newProps || hasContextChanged() || (
      workInProgress.type !== current.type )) {
      // 打个更新标
      didReceiveUpdate = true;
    } else if (xxx) {
      // 不需要更新的情况 A
      return A
    } else {
      if (需要更新的情况 B) {
        didReceiveUpdate = true;
      } else {
        // 不需要更新的其他情况，这里我们的首次渲染就将执行到这一行的逻辑
        didReceiveUpdate = false;
      }
    }
  } else {
    didReceiveUpdate = false;
  } 
  ......
  // 这坨 switch 是 beginWork 中的核心逻辑，原有的代码量相当大
  switch (workInProgress.tag) {
      ......
      // 这里省略掉大量形如"case: xxx"的逻辑
      // 根节点将进入这个逻辑
    case HostRoot:
      return updateHostRoot(current, workInProgress, renderLanes)
      // dom 标签对应的节点将进入这个逻辑
    case HostComponent:
      return updateHostComponent(current, workInProgress, renderLanes)

      // 文本节点将进入这个逻辑
    case HostText:
      return updateHostText(current, workInProgress)
        ...... 
        // 这里省略掉大量形如"case: xxx"的逻辑
        }
  // 这里是错误兜底，处理 switch 匹配不上的情况
  {
    {
      throw Error(
        "Unknown unit of work tag (" +
        workInProgress.tag +
        "). This error is likely caused by a bug in React. Please file an issue."
      )
    }
  }
}
```

**<font style="color:rgb(44, 62, 80);">beginWork 源码太长不看版的重点总结</font>**<font style="color:rgb(44, 62, 80);">：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的入参是一对用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">alternate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">连接起来的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的核心逻辑是根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);">）的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的不同，调用不同的节点创建函数。</font>

<font style="color:rgb(44, 62, 80);">当前的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current 节点是 rootFiber</font><font style="color:rgb(44, 62, 80);">，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">则是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current 的副本</font><font style="color:rgb(44, 62, 80);">，它们的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag 都是 3</font><font style="color:rgb(44, 62, 80);">，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873516150-66c3b117-5b86-4671-9382-82c2f2b32381.png)

<font style="color:rgb(44, 62, 80);">而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">正是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HostRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所对应的值，因此第一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将进入</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateHostRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的逻辑。</font>

<font style="color:rgb(44, 62, 80);">这里你先不必急于关注</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateHostRoot</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的逻辑细节。事实上，在整段</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">switch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">逻辑里，包含的形如“</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">update+类型名</font><font style="color:rgb(44, 62, 80);">”这样的函数是非常多的</font>

<font style="color:rgb(44, 62, 80);">幸运的是，这些函数之间不仅命名形式一致，工作内容也相似。就 render 链路来说，它们共同的特性，就是都会通过调用 reconcileChildren 方法，生成当前节点的子节点。</font>

<font style="color:rgb(44, 62, 80);">reconcileChildren 的源码如下：</font>

```javascript
function reconcileChildren(current, workInProgress, nextChildren, renderLanes) {
  // 判断 current 是否为 null
  if (current === null) {
    // 若 current 为 null，则进入 mountChildFibers 的逻辑
    workInProgress.child = mountChildFibers(workInProgress, null, nextChildren, renderLanes);
  } else {
    // 若 current 不为 null，则进入 reconcileChildFibers 的逻辑
    workInProgress.child = reconcileChildFibers(workInProgress, current.child, nextChildren, renderLanes);
  }
}
```

<font style="color:rgb(44, 62, 80);">从源码来看，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildren</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">也只是做逻辑的分发，具体的工作还要到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里去看。</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#childreconciler-%E5%A4%84%E7%90%86-fiber-%E8%8A%82%E7%82%B9%E7%9A%84%E5%B9%95%E5%90%8E-%E6%93%8D%E7%9B%98%E6%89%8B)<font style="color:rgb(44, 62, 80);">ChildReconciler，处理 Fiber 节点的幕后“操盘手”</font>
<font style="color:rgb(44, 62, 80);">那么这两个函数又是何方神圣呢？在源码中，我们可以觅得这样两个赋值语句：</font>

```plain
var reconcileChildFibers = ChildReconciler(true);
var mountChildFibers = ChildReconciler(false);
```

<font style="color:rgb(44, 62, 80);">原来</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不仅名字相似，出处也一致。它们都是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildReconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个函数的返回值，仅仅存在入参上的区别。而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildReconciler</font><font style="color:rgb(44, 62, 80);">，则是一个实打实的“庞然大物”，其内部的逻辑量堪比</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">N</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(44, 62, 80);">。这里我将关键要素提取如下（解析在注释里）：</font>

```javascript
function ChildReconciler(shouldTrackSideEffects) {
  // 删除节点的逻辑
  function deleteChild(returnFiber, childToDelete) {
    if (!shouldTrackSideEffects) {
      // Noop.
      return;
    } 
    // 以下执行删除逻辑
  }

  ......


  // 单个节点的插入逻辑
  function placeSingleChild(newFiber) {
    if (shouldTrackSideEffects && newFiber.alternate === null) {
      newFiber.flags = Placement;
    }
    return newFiber;
  }

  // 插入节点的逻辑
  function placeChild(newFiber, lastPlacedIndex, newIndex) {
    newFiber.index = newIndex;
    if (!shouldTrackSideEffects) {
      // Noop.
      return lastPlacedIndex;
    }
    // 以下执行插入逻辑
  }
  ......
  // 此处省略一系列 updateXXX 的函数，它们用于处理 Fiber 节点的更新

  // 处理不止一个子节点的情况
  function reconcileChildrenArray(returnFiber, currentFirstChild, newChildren, lanes) {
    ......
  }
  // 此处省略一堆 reconcileXXXXX 形式的函数，它们负责处理具体的 reconcile 逻辑
  function reconcileChildFibers(returnFiber, currentFirstChild, newChild, lanes) {
    // 这是一个逻辑分发器，它读取入参后，会经过一系列的条件判断，调用上方所定义的负责具体节点操作的函数
  }

  // 将总的 reconcileChildFibers 函数返回
  return reconcileChildFibers;
}
```

<font style="color:rgb(44, 62, 80);">由于原本的代码量着实巨大，感兴趣的同学可以</font>[点开这个文件查看细节(opens new window)](https://github.com/facebook/react/blob/56e9feead0f91075ba0a4f725c9e4e343bca1c67/packages/react-reconciler/src/ReactChildFiber.old.js#L253)<font style="color:rgb(44, 62, 80);">，此处我仅针对与主流程强相关的逻辑为你总结以下要点：</font>

+ <font style="color:rgb(44, 62, 80);">关键的入参</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldTrackSideEffects</font><font style="color:rgb(44, 62, 80);">，意为“是否需要追踪副作用”，因此</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的不同，在于对副作用的处理不同；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildReconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中定义了大量如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">placeXXX、deleteXXX、updateXXX、reconcileXXX</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等这样的函数，这些函数覆盖了对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点的创建、增加、删除、修改等动作，将直接或间接地被</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所调用；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildReconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的返回值是一个名为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的函数，这个函数是一个逻辑分发器，它将根据入参的不同，执行不同的 Fiber 节点操作，最终返回不同的目标 Fiber 节点。</font>

<font style="color:rgb(44, 62, 80);">对于第 1 点，这里展开说说。对副作用的处理不同，到底是哪里不同？以 placeSingleChild 为例，以下是 placeSingleChild 的源码：</font>

```javascript
function placeSingleChild(newFiber) {
  if (shouldTrackSideEffects && newFiber.alternate === null) {
    newFiber.flags = Placement;
  }
  return newFiber;
}
```

<font style="color:rgb(44, 62, 80);">可以看出，一旦判断</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldTrackSideEffects</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">，那么下面所有的逻辑都不执行了，直接返回。那如果执行下去会发生什么呢？简而言之就是给 Fiber 节点打上一个叫“flags”的标记，像这样：</font>

```plain
newFiber.flags = Placement;
```

<font style="color:rgb(44, 62, 80);">这个名为 flags 的标记有何作用呢？</font>

**<font style="color:rgb(44, 62, 80);">小科普：flags 是什么</font>**

<font style="color:rgb(44, 62, 80);">由于这里我引用的是 v17.0.0 版本的源码，属性名已经变更为 flags，但在更早一些的版本中，这个属性名叫“effectTag”。在时下的社区讨论中，effectTag 这个命名更常见，也更语义化，因此下文我将以 “effectTag”代指“flags”。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Placement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">effectTag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的意义，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">是在渲染器执行时，也就是真实 DOM 渲染时，告诉渲染器：我这里需要新增 DOM 节点</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">effectTag 记录的是副作用的类型</font><font style="color:rgb(44, 62, 80);">，而所谓“副作用”，React 给出的定义是“数据获取、订阅或者修改 DOM”等动作。在这里，Placement 对应的显然是 DOM 相关的副作用操作。</font>

<font style="color:rgb(44, 62, 80);">像</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Placement 这样的副作用标识，还有很多，它们均以二进制常量的形式存在</font><font style="color:rgb(44, 62, 80);">，下图我为你截取了局部（你可以在这个文件里查看</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">effectTag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型）：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873517655-8462507a-f92b-4a3f-b6c8-b6b874553f83.png)

<font style="color:rgb(44, 62, 80);">回到我们的调用链路里来，由于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);">，它不为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">，因此它将走入的是下图所高亮的这行逻辑。也就是说在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之间，它选择的是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);">：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873518701-cad1bcdb-8108-4fe1-b3d1-a955c3ec875c.png)

<font style="color:rgb(44, 62, 80);">结合前面的分析可知，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildReconciler(true)</font><font style="color:rgb(44, 62, 80);">的返回值。入参为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">，意味着其内部逻辑是允许追踪副作用的，因此“打</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">effectTag</font><font style="color:rgb(44, 62, 80);">”这个动作将会生效。</font>

<font style="color:rgb(44, 62, 80);">接下来进入</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的逻辑，在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个逻辑分发器中，会把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">子节点的创建工作分发给</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileXXX</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数家族的一员——</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileSingleElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来处理，具体的调用形式如下图高亮处所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873521185-117f1a9b-c6f4-4d31-9318-9e3fe9149e42.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileSingleElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将基于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">子节点的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象信息，创建其对应的 FiberNode。这个过程中涉及的函数调用如下图高亮处所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873525028-84e5fee1-6376-4b85-863b-2385fbd438d2.png)

<font style="color:rgb(44, 62, 80);">这里需要说明的一点是：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber 作为 Fiber 树的根节点</font><font style="color:rgb(44, 62, 80);">，它并没有一个确切的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与之映射。结合</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">结构来看，我们可以将其理解为是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中根组件的父节点。</font>

```plain
import React from "react";
import ReactDOM from "react-dom";
function App() {
    return (
      <div className="App">
        <div className="container">
          <h1>我是标题</h1>
          <p>我是第一段话</p>
          <p>我是第二段话</p>
        </div>
      </div>
    );
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

<font style="color:rgb(44, 62, 80);">可以看出，根组件是一个类型为 App 的函数组件，因此</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber 就是 App 的父节点</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">结合这个分析来看，图中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_created4</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的第一个子节点对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement 来创建的 Fiber 节点</font><font style="color:rgb(44, 62, 80);">，那么它就是 App 所对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点。现在我为你打印出运行时的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_created4</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值，会发现确实如此：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873525344-627e5ea0-edc4-4218-b3db-1d132451d98c.png)

<font style="color:rgb(44, 62, 80);">App 所对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点，将被</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">placeSingleChild</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">打上“</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Placement</font><font style="color:rgb(44, 62, 80);">”（新增）的副作用标记，而后作为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildFibers</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数的返回值，返回给下图中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress.child</font><font style="color:rgb(44, 62, 80);">：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873527945-bce5ed88-b82c-40d5-a45c-8531522103a0.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconcileChildren</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数上下文里的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点。那么此时，我们就将新创建的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">App Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">关联了起来，整个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873530086-b0044e06-8227-4433-927a-8230513ffe36.png)

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#fiber-%E8%8A%82%E7%82%B9%E7%9A%84%E5%88%9B%E5%BB%BA%E8%BF%87%E7%A8%8B%E6%A2%B3%E7%90%86)<font style="color:rgb(44, 62, 80);">Fiber 节点的创建过程梳理</font>
<font style="color:rgb(44, 62, 80);">分析完</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">App FiberNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的创建过程，我们先不必急于继续往下走这个渲染链路。因为其实最关键的东西已经讲完了，剩余节点的创建只不过是对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUnitOfWork</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildReconciler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等相关逻辑的重复。</font>

<font style="color:rgb(44, 62, 80);">刚刚这一通分析所涉及的调用栈很长，相信不少人如果是初读的话，过程中肯定不可避免地要反复回看，确认自己现在到底在调用栈的哪一环。这里为了方便你把握逻辑脉络，我将本讲讲解的 beginWork 所触发的调用流程总结进一张大图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873529974-10c5aff5-4b32-42e9-b161-3eca45313b1a.png)

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#fiber-%E6%A0%91%E7%9A%84%E6%9E%84%E5%BB%BA%E8%BF%87%E7%A8%8B)<font style="color:rgb(44, 62, 80);">Fiber 树的构建过程</font>
<font style="color:rgb(44, 62, 80);">理解了 Fiber 节点的创建过程，就不难理解 Fiber 树的构建过程了。</font>

<font style="color:rgb(44, 62, 80);">前面我们已经锲而不舍地研究了各路关键函数的源码逻辑，此时相信你已经能够将函数名与函数的工作内容做到对号入座。这里我们不必再纠结与源码的实现细节，可以直接从工作流程的角度来看后续节点的创建。</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#%E5%BE%AA%E7%8E%AF%E5%88%9B%E5%BB%BA%E6%96%B0%E7%9A%84-fiber-%E8%8A%82%E7%82%B9)<font style="color:rgb(44, 62, 80);">循环创建新的 Fiber 节点</font>
<font style="color:rgb(44, 62, 80);">研究节点创建的工作流，我们的切入点是workLoopSync这个函数。</font>

<font style="color:rgb(44, 62, 80);">为什么选它？这里来复习一遍workLoopSync会做什么：</font>

```plain
function workLoopSync() {
  // 若 workInProgress 不为空
  while (workInProgress !== null) {
    // 针对它执行 performUnitOfWork 方法
    performUnitOfWork(workInProgress);
  }
}
```

<font style="color:rgb(44, 62, 80);">它会循环地调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUnitOfWork</font><font style="color:rgb(44, 62, 80);">，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUnitOfWork</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">其主要工作是“通过调用 beginWork，来实现新 Fiber 节点的创建”</font><font style="color:rgb(44, 62, 80);">；它还有一个次要工作，就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">把新创建的这个 Fiber 节点的值更新到 workInProgress 变量里去</font><font style="color:rgb(44, 62, 80);">。源码中的相关逻辑提取如下：</font>

```plain
// 新建 Fiber 节点
next = beginWork$1(current, unitOfWork, subtreeRenderLanes);
// 将新的 Fiber 节点赋值给 workInProgress
if (next === null) {
  // If this doesn't spawn new work, complete the current work.
  completeUnitOfWork(unitOfWork);
} else {
  workInProgress = next;
}
```

<font style="color:rgb(44, 62, 80);">如此便能够确保每次</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUnitOfWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">执行完毕后，当前的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);">都存储着下一个需要被处理的节点，从而为下一次的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workLoopSync</font><font style="color:rgb(44, 62, 80);">循环做好准备。</font>

<font style="color:rgb(44, 62, 80);">现在我在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workLoopSync</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内部打个断点，尝试输出每一次获取到的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值的变化过程如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873531790-730f6863-44b5-43cb-a573-203a5fd88a20.png)

<font style="color:rgb(44, 62, 80);">共有 7 个节点，若你点击展开查看每个节点的内容，就会发现这 7 个节点其实分别是：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rootFiber</font><font style="color:rgb(44, 62, 80);">（当前 Fiber 树的根节点）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">App FiberNode</font><font style="color:rgb(44, 62, 80);">（App 函数组件对应的节点）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为 App 的 DOM 元素对应的节点，其内容如下图所示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873532688-f4805363-8ddf-44c2-8320-b5a6eba64202.png)

<font style="color:rgb(44, 62, 80);">class 为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">container</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的 DOM 元素对应的节点，其内容如下图所示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873534075-ccb0faf2-d383-4a47-a74d-cba36382a213.png)

<font style="color:rgb(44, 62, 80);">h1 标签对应的节点</font>

<font style="color:rgb(44, 62, 80);">第 1 个 p 标签对应的 FiberNode，内容为“我是第一段话”，如下图所示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873533842-2cae3301-e74e-4d5a-a0ba-a696877f9785.png)

<font style="color:rgb(44, 62, 80);">第 2 个 p 标签对应的 FiberNode，内容为“我是第二段话”，如下图所示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873534082-76eb4d8f-a8a2-4b16-9eb1-e270c3e65cb7.png)

<font style="color:rgb(44, 62, 80);">结合这 7 个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FiberNode</font><font style="color:rgb(44, 62, 80);">，再对照对照我们的 Demo：</font>

```plain
function App() {
    return (
      <div className="App">
        <div className="container">
          <h1>我是标题</h1>
          <p>我是第一段话</p>
          <p>我是第二段话</p>
        </div>
      </div>
    );
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">你会发现组件自上而下，每一个非文本类型的 ReactElement 都有了它对应的 Fiber 节点。</font>

**<font style="color:rgb(44, 62, 80);">注：</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">React 并不会为所有的文本类型</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">创建对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FiberNode</font><font style="color:rgb(44, 62, 80);">，这是一种优化策略。是否需要创建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FiberNode</font><font style="color:rgb(44, 62, 80);">，在源码中是通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isDirectTextChild</font><font style="color:rgb(44, 62, 80);">这个变量来区分的</font>

<font style="color:rgb(44, 62, 80);">这样一来，我们构建的这棵树里，就多出了不少 FiberNode，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873534999-c5b7228f-03d6-4816-becf-a25dcf29ee48.png)

<font style="color:rgb(44, 62, 80);">Fiber 节点有是有了，但这些 Fiber 节点之间又是如何相互连接的呢？</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#fiber-%E8%8A%82%E7%82%B9%E9%97%B4%E6%98%AF%E5%A6%82%E4%BD%95%E8%BF%9E%E6%8E%A5%E7%9A%84%E5%91%A2)<font style="color:rgb(44, 62, 80);">Fiber 节点间是如何连接的呢</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不同的 Fiber 节点之间，将通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">child、return、sibling</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这 3 个属性建立关系，其中</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">child、return</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">记录的是父子节点关系，而</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sibling</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">记录的则是兄弟节点关系。</font>

<font style="color:rgb(44, 62, 80);">这里我以 h1 这个元素对应的 Fiber 节点为例，给你展示下它是如何与其他节点相连接的。展开这个 Fiber 节点，对它的 child、 return、sibling 3 个属性作截取，如下图所示：</font>

<font style="color:rgb(44, 62, 80);">child 属性为 null，说明 h1 节点没有子 Fiber 节点：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873536604-d75d517f-d7bf-4605-ac6a-1108ea86c9d2.png)

<font style="color:rgb(44, 62, 80);">return 属性局部截图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873536724-9cfea484-da1f-4d78-b86c-b179f51ad8f7.png)

<font style="color:rgb(44, 62, 80);">sibling 属性局部截图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873536720-d834a541-6958-4ab0-9686-276e1aa3d4a6.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以看到，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return 属性指向的是 class 为 container 的 div 节点</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，而</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sibling 属性指向的是第 1 个 p 节点</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。结合 JSX 中的嵌套关系我们不难得知 ——</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FiberNode 实例中，return 指向的是当前 Fiber 节点的父节点</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，而</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sibling 指向的是当前节点的第 1 个兄弟节点</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

<font style="color:rgb(44, 62, 80);">结合这 3 个属性所记录的节点间关系信息，我们可以轻松地将上面梳理出来的新 FiberNode 连接起来：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873538147-91d4b317-9687-46ce-827d-8f64598863ee.png)

<font style="color:rgb(44, 62, 80);">以上便是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树的最终形态了。从图中可以看出，虽然人们习惯上仍然将眼前的这个产物称为“Fiber 树”，但</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">它的数据结构本质其实已经从树变成了链表</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">注意，在分析</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树的构建过程时，我们选取了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">作为切入点，但整个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树的构建过程中，并不是只有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beginWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在工作。这其中，还穿插着</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的工作。只有将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork 和 beginWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">放在一起来看，你才能够真正理解，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">架构下的“深度优先遍历”到底是怎么一回事。</font>

## [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#%E5%9B%9B%E3%80%81commit-%E9%98%B6%E6%AE%B5)四、commit 阶段
<font style="color:rgb(44, 62, 80);">实验 Demo 与前面保持一致，代码如下：</font>

```plain
import React from "react";
import ReactDOM from "react-dom";
function App() {
  return (
    <div className="App">
      <div className="container">
        <h1>我是标题</h1>
        <p>我是第一段话</p>
        <p>我是第二段话</p>
      </div>
    </div>
  );
}
const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#completework-%E5%B0%86-fiber-%E8%8A%82%E7%82%B9%E6%98%A0%E5%B0%84%E4%B8%BA-dom-%E8%8A%82%E7%82%B9)<font style="color:rgb(44, 62, 80);">completeWork——将 Fiber 节点映射为 DOM 节点</font>
**<font style="color:rgb(44, 62, 80);">completeWork 的调用时机</font>**

<font style="color:rgb(44, 62, 80);">首先，我们先在调用栈中定位一下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);">。Demo 所对应的调用栈中，第一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">出现在下图红框选中的位置：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873538246-a8afb90c-a215-4213-be80-662ddbfdcb10.png)

<font style="color:rgb(44, 62, 80);">从图上我们需要把握住的一个信息是，从</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUnitOfWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);">，中间会经过一个这样的调用链路：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873538079-cf053777-a31b-4c70-a071-cd7da24c69cc.png)

<font style="color:rgb(44, 62, 80);">其中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeUnitOfWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的工作也非常关键，但眼下我们先拿</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">开刀，你可以暂时将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeUnitOfWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">简单理解为一个用于发起</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">调用的“工具人”。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeUnitOfWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUnitOfWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中被调用的，那么</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUnitOfWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是如何把握其调用时机的呢？我们直接来看相关源码（解析在注释里）：</font>

```javascript
function performUnitOfWork(unitOfWork) {
  ......
  // 获取入参节点对应的 current 节点
  var current = unitOfWork.alternate;

  var next;
  if (xxx) {
    ...
    // 创建当前节点的子节点
    next = beginWork$1(current, unitOfWork, subtreeRenderLanes);
    ...
  } else {
    // 创建当前节点的子节点
    next = beginWork$1(current, unitOfWork, subtreeRenderLanes);
  }
  ......
  if (next === null) {
    // 调用 completeUnitOfWork
    completeUnitOfWork(unitOfWork);
  } else {
    // 将当前节点更新为新创建出的 Fiber 节点
    workInProgress = next;
  }
  ......
}
```

<font style="color:rgb(44, 62, 80);">这段源码中你需要提取出的信息是：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">performUnitOfWork 每次会尝试调用 beginWork 来创建当前节点的子节点，若创建出的子节点为空（也就意味着当前节点不存在子 Fiber 节点），则说明当前节点是一个叶子节点</font><font style="color:rgb(44, 62, 80);">。按照深度优先遍历的原则，当遍历到叶子节点时，“递”阶段就结束了，随之而来的是“归”的过程。因此这种情况下，就会调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeUnitOfWork</font><font style="color:rgb(44, 62, 80);">，执行当前节点对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">逻辑。</font>

<font style="color:rgb(44, 62, 80);">接下来我们在 Demo 代码的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">处打上断点，看看第一个走到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的节点是哪个，结果如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873538158-d8a82dc7-1eda-455f-9727-baa81f7fe8a9.png)

<font style="color:rgb(44, 62, 80);">显然，第一个进入</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的节点是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h1</font><font style="color:rgb(44, 62, 80);">，这也符合我们上一讲所构建出来的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树中的节点关系，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873538123-aa10e59c-def6-4eb3-bf19-6c751895d432.png)

<font style="color:rgb(44, 62, 80);">由图可知，按照深度优先遍历的原则，h1 确实将是第一个被遍历到的叶子节点。接下来我们就以 h1 为例，一起看看 completeWork 都围绕它做了哪些事情。</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#completework-%E7%9A%84%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)<font style="color:rgb(44, 62, 80);">completeWork 的工作原理</font>
<font style="color:rgb(44, 62, 80);">这里仍然为你提取一下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的源码结构和主体逻辑，代码如下（解析在注释里）：</font>

```javascript
function completeWork(current, workInProgress, renderLanes) {
  // 取出 Fiber 节点的属性值，存储在 newProps 里
  var newProps = workInProgress.pendingProps;

  // 根据 workInProgress 节点的 tag 属性的不同，决定要进入哪段逻辑
  switch (workInProgress.tag) {
    case ......:
      return null;
    case ClassComponent:
      {
        .....
      }
    case HostRoot:
      {
        ......
      }
      // h1 节点的类型属于 HostComponent，因此这里为你讲解的是这段逻辑
    case HostComponent:
      {
        popHostContext(workInProgress);
        var rootContainerInstance = getRootHostContainer();
        var type = workInProgress.type;
        // 判断 current 节点是否存在，因为目前是挂载阶段，因此 current 节点是不存在的
        if (current !== null && workInProgress.stateNode != null) {
          updateHostComponent$1(current, workInProgress, type, newProps, rootContainerInstance);
          if (current.ref !== workInProgress.ref) {
            markRef$1(workInProgress);
          }
        } else {
          // 这里首先是针对异常情况进行 return 处理
          if (!newProps) {
            if (!(workInProgress.stateNode !== null)) {
              {
                throw Error("We must have new props for new mounts. This error is likely caused by a bug in React. Please file an issue.");
              }
            } 

            return null;
          }

          // 接下来就为 DOM 节点的创建做准备了
          var currentHostContext = getHostContext();
          // _wasHydrated 是一个与服务端渲染有关的值，这里不用关注
          var _wasHydrated = popHydrationState(workInProgress);

          // 判断是否是服务端渲染
          if (_wasHydrated) {
            // 这里不用关注，请你关注 else 里面的逻辑
            if (prepareToHydrateHostInstance(workInProgress, rootContainerInstance, currentHostContext)) {
              markUpdate(workInProgress);
            }
          } else {
            // 这一步很关键， createInstance 的作用是创建 DOM 节点
            var instance = createInstance(type, newProps, rootContainerInstance, currentHostContext, workInProgress);
            // appendAllChildren 会尝试把上一步创建好的 DOM 节点挂载到 DOM 树上去
            appendAllChildren(instance, workInProgress, false, false);
            // stateNode 用于存储当前 Fiber 节点对应的 DOM 节点
            workInProgress.stateNode = instance; 

            // finalizeInitialChildren 用来为 DOM 节点设置属性
            if (finalizeInitialChildren(instance, type, newProps, rootContainerInstance)) {
              markUpdate(workInProgress);
            }
          }
          ......
        }
        return null;
      }
    case HostText:
      {
        ......
      }
    case SuspenseComponent:
      {
        ......
      }
    case HostPortal:
      ......
      return null;
    case ContextProvider:
      ......
      return null;
      ......
  }
  {
    {
      throw Error("Unknown unit of work tag (" + workInProgress.tag + "). This error is likely caused by a bug in React. Please file an issue.");
    }
  }
}
```

<font style="color:rgb(44, 62, 80);">试图捋顺这段</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">逻辑，你需要掌握以下几个要点。</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的核心逻辑是一段体量巨大的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">switch</font><font style="color:rgb(44, 62, 80);">语句，在这段</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">switch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">语句中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的不同，进入不同的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点的创建、处理逻辑。</font>
+ <font style="color:rgb(44, 62, 80);">在 Demo 示例中，h1 节点的 tag 属性对应的类型应该是 HostComponent，也就是“原生 DOM 元素类型”。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current、 workInProgress</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">分别对应的是下图中左右两棵</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树上的节点：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873539717-984c8b1c-e573-4bdf-b092-06477921af8e.png)

<font style="color:rgb(44, 62, 80);">其中 workInProgress 树代表的是“当前正在 render 中的树”，而 current 树则代表“已经存在的树”。</font>

<font style="color:rgb(44, 62, 80);">workInProgress 节点和 current 节点之间用 alternate 属性相互连接。在组件的挂载阶段，current 树只有一个 rootFiber 节点，并没有其他内容。因此 h1 这个 workInProgress 节点对应的 current 节点是 null。</font>

<font style="color:rgb(44, 62, 80);">带着上面这些前提，再去结合注释读一遍上面提炼出来的源码，思路是不是就清晰多了？</font>

<font style="color:rgb(44, 62, 80);">捋顺思路后，我们直接来提取知识点。关于 completeWork，你需要明白以下几件事。</font>

+ <font style="color:rgb(44, 62, 80);">用一句话来总结</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的工作内容：负责处理 Fiber 节点到 DOM 节点的映射逻辑。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内部有 3 个关键动作：</font>
    - <font style="color:rgb(44, 62, 80);">创建</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点（CreateInstance）</font>
    - <font style="color:rgb(44, 62, 80);">将 DOM 节点插入到 DOM 树中（AppendAllChildren）</font>
    - <font style="color:rgb(44, 62, 80);">为 DOM 节点设置属性（FinalizeInitialChildren）</font>
+ <font style="color:rgb(44, 62, 80);">创建好的 DOM 节点会被赋值给 workInProgress 节点的 stateNode 属性。也就是说当我们想要定位一个 Fiber 对应的 DOM 节点时，访问它的 stateNode 属性就可以了。这里我们可以尝试访问运行时的 h1 节点的 stateNode 属性，结果如下图所示：</font><font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873541000-99bae8ed-1bb6-4463-9f76-a8f0dd6a7a78.png)
+ <font style="color:rgb(44, 62, 80);">将 DOM 节点插入到 DOM 树的操作是通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">appendAllChildren</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数来完成的</font>

<font style="color:rgb(44, 62, 80);">说是将 DOM 节点插入到 DOM 树里去，实际上是将子 Fiber 节点所对应的 DOM 节点挂载到其父 Fiber 节点所对应的 DOM 节点里去。比如说在本讲 Demo 所构建出的 Fiber 树中，h1 节点的父结点是 div，那么 h1 对应的 DOM 节点就理应被挂载到 div 对应的 DOM 节点里去。</font>

<font style="color:rgb(44, 62, 80);">那么如果执行 appendAllChildren 时，父级的 DOM 节点还不存在怎么办？</font>

<font style="color:rgb(44, 62, 80);">比如 h1 节点作为第一个进入 completeWork 的节点，它的父节点 div 对应的 DOM 就尚不存在。其实不存在也没关系，反正 h1 DOM 节点被创建后，会作为 h1 Fiber 节点的 stateNode 属性存在，丢不掉的。当父节点 div 进入 appendAllChildren 逻辑后，会逐个向下查找并添加自己的后代节点，这时候，h1 就会被它的父级 DOM 节点“收入囊中”啦~</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#completeunitofwork-%E5%BC%80%E5%90%AF%E6%94%B6%E9%9B%86-effectlist-%E7%9A%84-%E5%A4%A7%E5%BE%AA%E7%8E%AF)<font style="color:rgb(44, 62, 80);">completeUnitOfWork —— 开启收集 EffectList 的“大循环”</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeUnitOfWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的作用是开启一个大循环，在这个大循环中，将会重复地做下面三件事：</font>

1. <font style="color:rgb(44, 62, 80);">针对传入的当前节点，调用 completeWork，completeWork 的工作内容前面已经讲过，这一步应该是没有异议的；</font>
2. <font style="color:rgb(44, 62, 80);">将当前节点的副作用链（EffectList）插入到其父节点对应的副作用链（EffectList）中；</font>
3. <font style="color:rgb(44, 62, 80);">以当前节点为起点，循环遍历其兄弟节点及其父节点。当遍历到兄弟节点时，将 return 掉当前调用，触发兄弟节点对应的 performUnitOfWork 逻辑；而遍历到父节点时，则会直接进入下一轮循环，也就是重复 1、2 的逻辑。</font>

<font style="color:rgb(44, 62, 80);">步骤 1 无须多言，接下来我将为你解读步骤 2 和步骤 3 的含义。</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#completeunitofwork-%E5%BC%80%E5%90%AF%E4%B8%8B%E4%B8%80%E8%BD%AE%E5%BE%AA%E7%8E%AF%E7%9A%84%E5%8E%9F%E5%88%99)<font style="color:rgb(44, 62, 80);">completeUnitOfWork 开启下一轮循环的原则</font>
<font style="color:rgb(44, 62, 80);">在理解副作用链之前，首先要理解</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">completeUnitOfWork</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">开启下一轮循环的原则，也就是步骤 3。步骤 3 相关的源码如下所示（解析在注释里）：</font>

```plain
do {
  ......
  // 这里省略步骤 1 和步骤 2 的逻辑 

  // 获取当前节点的兄弟节点
  var siblingFiber = completedWork.sibling;

  // 若兄弟节点存在
  if (siblingFiber !== null) {
    // 将 workInProgress 赋值为当前节点的兄弟节点
    workInProgress = siblingFiber;
    // 将正在进行的 completeUnitOfWork 逻辑 return 掉
    return;
  } 

  // 若兄弟节点不存在，completeWork 会被赋值为 returnFiber，也就是当前节点的父节点
  completedWork = returnFiber; 
    // 这一步与上一步是相辅相成的，上下文中要求 workInProgress 与 completedWork 保持一致
  workInProgress = completedWork;
} while (completedWork !== null);
```

<font style="color:rgb(44, 62, 80);">步骤 3 是整个循环体的收尾工作，它会在当前节点相关的各种工作都做完之后执行。</font>

<font style="color:rgb(44, 62, 80);">当前节点处理完了，自然是去寻找下一个可以处理的节点。我们知道，当前的 Fiber 节点之所以会进入 completeWork，是因为“递无可递”了，才会进入“归”的逻辑，这就意味着当前 Fiber 要么没有 child 节点、要么 child 节点的 completeWork 早就执行过了。因此 child 节点不会是下次循环需要考虑的对象，下次循环只需要考虑兄弟节点（siblingFiber）和父节点（returnFiber）。</font>

<font style="color:rgb(44, 62, 80);">那么为什么在源码中，遇到兄弟节点会 return，遇到父节点才会进入下次循环呢？这里我以 h1 节点的节点关系为例进行说明。请看下图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873541672-ef30d441-6d98-4cbe-8838-16cc54b04c1d.png)

<font style="color:rgb(44, 62, 80);">结合前面的分析和图示可知，h1 节点是递归过程中所触及的第一个叶子节点，也是其兄弟节点中被遍历到的第一个节点；而剩下的两个 p 节点，此时都还没有被遍历到，也就是说连 beginWork 都没有执行过。</font>

<font style="color:rgb(44, 62, 80);">因此对于 h1 节点的兄弟节点来说，当下的第一要务是回去从 beginWork 开始走起，直到 beginWork “递无可递”时，才能够执行 completeWork 的逻辑。beginWork 的调用是在 performUnitOfWork 里发生的，因此 completeUnitOfWork 一旦识别到当前节点的兄弟节点不为空，就会终止后续的逻辑，退回到上一层的 performUnitOfWork 里去。</font>

<font style="color:rgb(44, 62, 80);">接下来我们再来看 h1 的父节点 div：在向下递归到 h1 的过程中，div 必定已经被遍历过了，也就是说 div 的“递”阶段（ beginWork） 已经执行完毕，只剩下“归”阶段的工作要处理了。因此，对于父节点，completeUnitOfWork 会毫不犹豫地把它推到下一次循环里去，让它进入 completeWork 的逻辑。</font>

<font style="color:rgb(44, 62, 80);">值得注意的是，completeUnitOfWork 中处理兄弟节点和父节点的顺序是：先检查兄弟节点是否存在，若存在则优先处理兄弟节点；确认没有待处理的兄弟节点后，才转而处理父节点。这也就意味着，completeWork 的执行是严格自底向上的，子节点的 completeWork 总会先于父节点执行。</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#%E5%89%AF%E4%BD%9C%E7%94%A8%E9%93%BE-effectlist-%E7%9A%84%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E7%8E%B0)<font style="color:rgb(44, 62, 80);">副作用链（effectList）的设计与实现</font>
<font style="color:rgb(44, 62, 80);">无论是 beginWork 还是 completeWork，它们的应用对象都是 workInProgress 树上的节点。我们说 render 阶段是一个递归的过程，“递归”的对象，正是这棵 workInProgress 树（见下图右侧高亮部分）：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873541147-7ccf2c78-078c-4446-8050-a49140b50936.png)

<font style="color:rgb(44, 62, 80);">那么我们递归的目的是什么呢？或者说，render 阶段的工作目标是什么呢？</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#render-%E9%98%B6%E6%AE%B5%E7%9A%84%E5%B7%A5%E4%BD%9C%E7%9B%AE%E6%A0%87%E6%98%AF%E6%89%BE%E5%87%BA%E7%95%8C%E9%9D%A2%E4%B8%AD%E9%9C%80%E8%A6%81%E5%A4%84%E7%90%86%E7%9A%84%E6%9B%B4%E6%96%B0)<font style="color:rgb(44, 62, 80);">render 阶段的工作目标是找出界面中需要处理的更新</font>
<font style="color:rgb(44, 62, 80);">在实际的操作中，并不是所有的节点上都会产生需要处理的更新。比如在挂载阶段，对图中的整棵 workInProgress 递归完毕后，React 会发现实际只需要对 App 节点执行一个挂载操作就可以了；而在更新阶段，这种现象更为明显。</font>

<font style="color:rgb(44, 62, 80);">更新阶段与挂载阶段的主要区别在于更新阶段的 current 树不为空，比如说情况可以是下图这样子的：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873541816-3d90446b-d77f-4dee-82e5-82fca6280116.png)

<font style="color:rgb(44, 62, 80);">假如说我的某一次操作，仅仅对 p 节点产生了影响，那么对于渲染器来说，它理应只关注 p 节点这一处的更新。这时候问题就来了：怎样做才能让渲染器又快又好地定位到那些真正需要更新的节点呢？</font>

<font style="color:rgb(44, 62, 80);">在 render 阶段，我们通过艰难的递归过程来明确“p 节点这里有一处更新”这件事情。按照 React 的设计思路，render 阶段结束后，“找不同”这件事情其实也就告一段落了。commit 只负责实现更新，而不负责寻找更新，这就意味着我们必须找到一个办法能让 commit 阶段“坐享其成”，能直接拿到 render 阶段的工作成果。而这，正是副作用链（effectList）的价值所在。</font>

<font style="color:rgb(44, 62, 80);">副作用链（effectList） 可以理解为 render 阶段“工作成果”的一个集合：每个 Fiber 节点都维护着一个属于它自己的 effectList，effectList 在数据结构上以链表的形式存在，链表内的每一个元素都是一个 Fiber 节点。这些 Fiber 节点需要满足两个共性：</font>

+ <font style="color:rgb(44, 62, 80);">都是当前 Fiber 节点的后代节点</font>
+ <font style="color:rgb(44, 62, 80);">都有待处理的副作用</font>

<font style="color:rgb(44, 62, 80);">没错，Fiber 节点的 effectList 里记录的并非它自身的更新，而是其需要更新的后代节点。带着这个结论，我们再来品品小节开头 completeUnitOfWork 中的“步骤 2”：</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">将当前节点的副作用链（effectList）插入到其父节点对应的副作用链（effectList）中。</font>

<font style="color:rgb(44, 62, 80);">咱们前面已经分析过，“completeWork 是自底向上执行的”，也就是说，子节点的 completeWork 总是比父节点先执行。试想，若每次处理到一个节点，都将当前节点的 effectList 插入到其父节点的 effectList 中。那么当所有节点的 completeWork 都执行完毕时，我是不是就可以从“终极父节点”，也就是 rootFiber 上，拿到一个存储了当前 Fiber 树所有 effect Fiber的“终极版”的 effectList 了？</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">把所有需要更新的 Fiber 节点单独串成一串链表，方便后续有针对性地对它们进行更新，这就是所谓的“收集副作用”的过程。</font>

<font style="color:rgb(44, 62, 80);">这里我以挂载过程为例，带你分析一下这个过程是如何实现的。</font>

<font style="color:rgb(44, 62, 80);">首先我们要知道的是，这个 effectList 链表在 Fiber 节点中是通过 firstEffect 和 lastEffect 来维护的，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873542437-bc954e1a-be2c-4865-a080-b71c2aee32b6.png)

<font style="color:rgb(44, 62, 80);">其中 firstEffect 表示 effectList 的第一个节点，而 lastEffect 则记录最后一个节点。</font>

<font style="color:rgb(44, 62, 80);">对于挂载过程来说，我们唯一要做的就是把 App 组件挂载到界面上去，因此 App 后代节点们的 effectList 其实都是不存在的。effectList 只有在 App 的父节点（rootFiber）这才不为空。</font>

<font style="color:rgb(44, 62, 80);">那么 effectList 的创建逻辑又是怎样的呢？其实非常简单，只需要为 firstEffect 和 lastEffect 各赋值一个引用即可。以下是从 completeUnitOfWork 源码中提取出的相关逻辑（解析在注释里）：</font>

```plain
// 若副作用类型的值大于“PerformedWork”，则说明这里存在一个需要记录的副作用
if (flags > PerformedWork) {
  // returnFiber 是当前节点的父节点
  if (returnFiber.lastEffect !== null) {
    // 若父节点的 effectList 不为空，则将当前节点追加到 effectList 的末尾去
    returnFiber.lastEffect.nextEffect = completedWork;
  } else {
    // 若父节点的 effectList 为空，则当前节点就是 effectList 的 firstEffect
    returnFiber.firstEffect = completedWork;
  }

  // 将 effectList 的 lastEffect 指针后移一位
  returnFiber.lastEffect = completedWork;
}
```

<font style="color:rgb(44, 62, 80);">代码中的 flags 咱们已经反复强调过了，它旧时的名字叫“effectTag”，是用来标识副作用类型的；而“completedWork”这个变量，在当前上下文中存储的就是“正在被执行 completeWork 相关逻辑”的节点；至于“PerformedWork”，它是一个值为 1 的常量，React 规定若 flags（又名 effectTag）的值小于等于 1，则不必提交到 commit 阶段。因此 completeUnitOfWork 只会对 flags 大于 PerformedWork 的 effect fiber 进行收集。</font>

<font style="color:rgb(44, 62, 80);">结合这些信息，再去读一遍源码片段，相信你的理解过程就会很流畅了。这里我以 App 节点为例，带你走一遍 effectList 的创建过程：</font>

+ <font style="color:rgb(44, 62, 80);">App FiberNode 的 flags 属性为 3，大于 PerformedWork，因此会进入 effectList 的创建逻辑；</font>
+ <font style="color:rgb(44, 62, 80);">创建 effectList 时，并不是为当前 Fiber 节点创建，而是为它的父节点创建，App 节点的父节点是 rootFiber，rootFiber 的 effectList 此时为空；</font>
+ <font style="color:rgb(44, 62, 80);">rootFiber 的 firstEffect 和 lastEffect 指针都会指向 App 节点，App 节点由此成为 effectList 中的唯一一个 FiberNode，如下图所示。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873542491-2b30759e-1f37-4ce9-8620-83e7b21a4a77.png)

<font style="color:rgb(44, 62, 80);">OK，读到这里，相信你已经对 effectList 的创建过程知根知底了。</font>

<font style="color:rgb(44, 62, 80);">现在，即便你对部分源码细节的消化可能没有那么快，也请你不要因为这些细节去中断自己串联整个渲染链路的思路。你只需要把握住“根节点（rootFiber）上的 effectList 信息，是 commit 阶段的更新线索”这个结论，就足以将 render 阶段和 commit 阶段串联起来。</font>

### [](https://www.123fe.net/principle-docs/react/19-ReactDOM.render%20%E6%98%AF%E5%A6%82%E4%BD%95%E4%B8%B2%E8%81%94%E6%B8%B2%E6%9F%93%E9%93%BE%E8%B7%AF%E7%9A%84.html#commit-%E9%98%B6%E6%AE%B5%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%AE%80%E6%9E%90)<font style="color:rgb(44, 62, 80);">commit 阶段工作流简析</font>
<font style="color:rgb(44, 62, 80);">在整个 ReactDOM.render 的渲染链路中，render 阶段是 Fiber 架构的核心体现，也是我们讲解的重点。对于 render 阶段，我对你的期望是“熟悉”，为了达成这个目标，我们对 render 阶段的学习还会再持续一个课时；而对于 commit 阶段，我只要求你做到“了解”。因此这里我会快速地带你过一遍 commit 阶段的重点知识，不占用你太多时间。</font>

<font style="color:rgb(44, 62, 80);">commit 会在 performSyncWorkOnRoot 中被调用，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718873542770-78c3c908-b7d4-478f-bf27-463dfadf0a9f.png)

<font style="color:rgb(44, 62, 80);">这里的入参 root 并不是 rootFiber，而是 fiberRoot（FiberRootNode）实例。fiberRoot 的 current 节点指向 rootFiber，因此拿到 effectList 对后续的 commit 流程来说不是什么难事。</font>

<font style="color:rgb(44, 62, 80);">从流程上来说，commit 共分为 3 个阶段：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">before mutation、mutation、layout</font><font style="color:rgb(44, 62, 80);">。</font>

+ <font style="color:rgb(44, 62, 80);">before mutation 阶段，这个阶段 DOM 节点还没有被渲染到界面上去，过程中会触发 getSnapshotBeforeUpdate，也会处理 useEffect 钩子相关的调度逻辑。</font>
+ <font style="color:rgb(44, 62, 80);">mutation，这个阶段负责 DOM 节点的渲染。在渲染过程中，会遍历 effectList，根据 flags（effectTag）的不同，执行不同的 DOM 操作。</font>
+ <font style="color:rgb(44, 62, 80);">layout，这个阶段处理 DOM 渲染完毕之后的收尾逻辑。比如调用 componentDidMount/componentDidUpdate，调用 useLayoutEffect 钩子函数的回调等。除了这些之外，它还会把 fiberRoot 的 current 指针指向 workInProgress Fiber 树。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">关于 commit 阶段的实现细节，感兴趣的同学课下可以参阅 </font>[commit 相关源码(opens new window)](https://github.com/facebook/react/blob/a81c02ac150233bdb5f31380d4135397fb8f4660/packages/react-reconciler/src/ReactFiberWorkLoop.new.js)<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，这里不再展开讨论。对于 commit，如果你只能记住一个知识点，我希望你记住</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">它是一个绝对同步的过程</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render 阶段可以同步也可以异步，但 commit 一定是同步的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

