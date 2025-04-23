<font style="color:rgb(44, 62, 80);">React 团队面向开发者给出了两条 React-Hooks 的使用原则，原则的内容如下：</font>

+ <font style="color:rgb(44, 62, 80);">只在 React 函数中调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Hook</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">不要在循环、条件或嵌套函数中调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Hook</font><font style="color:rgb(44, 62, 80);">。</font>

**<font style="color:rgb(44, 62, 80);">从现象看问题：若不保证 Hooks 执行顺序，会带来什么麻烦</font>**

<font style="color:rgb(44, 62, 80);">先来看一个小 Demo：</font>

```javascript
import React, { useState } from "react";

function PersonalInfoComponent() {
  // 集中定义变量
  let name, age, career, setName, setCareer;

  // 获取姓名状态
  [name, setName] = useState("修言");

  // 获取年龄状态
  [age] = useState("99");

  // 获取职业状态
  [career, setCareer] = useState("我是一个前端，爱吃小熊饼干");

  // 输出职业信息
  console.log("career", career);

  // 编写 UI 逻辑
  return (
    <div className="personalInfo">
    <p>姓名：{name}</p>
    <p>年龄：{age}</p>
    <p>职业：{career}</p>
    <button
  onClick={() => {
    setName("秀妍");
  }}
    >
    修改姓名
    </button>
    </div>
  );
}

export default PersonalInfoComponent;
```

<font style="color:rgb(44, 62, 80);">这个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PersonalInfoComponent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件渲染出来的界面长这样：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872411425-36119a22-0184-4a64-9e50-e580db1d45cb.png)

<font style="color:rgb(44, 62, 80);">PersonalInfoComponent 用于对个人信息进行展示，这里展示的内容包括姓名、年龄、职业。出于测试效果需要，PersonalInfoComponent 还允许你点击“修改姓名”按钮修改姓名信息。点击一次后，“修言”会被修改为“秀妍”，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872412610-8f7a16ee-631e-4231-a4f0-4e328851a903.png)

<font style="color:rgb(44, 62, 80);">到目前为止，组件的行为都是符合我们的预期的，一切看上去都是那么的和谐。但倘若我对代码做一丝小小的改变，把一部分的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">操作放进</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">if 语句</font><font style="color:rgb(44, 62, 80);">里，事情就会变得大不一样。改动后的代码如下：</font>

```javascript
import React, { useState } from "react";
// isMounted 用于记录是否已挂载（是否是首次渲染）
let isMounted = false;
function PersonalInfoComponent() {
  // 定义变量的逻辑不变
  let name, age, career, setName, setCareer;

  // 这里追加对 isMounted 的输出，这是一个 debug 性质的操作
  console.log("isMounted is", isMounted);
  // 这里追加 if 逻辑：只有在首次渲染（组件还未挂载）时，才获取 name、age 两个状态
  if (!isMounted) {
    // eslint-disable-next-line
    [name, setName] = useState("修言");
    // eslint-disable-next-line
    [age] = useState("99");

    // if 内部的逻辑执行一次后，就将 isMounted 置为 true（说明已挂载，后续都不再是首次渲染了）
    isMounted = true;
  }

  // 对职业信息的获取逻辑不变
  [career, setCareer] = useState("我是一个前端，爱吃小熊饼干");
  // 这里追加对 career 的输出，这也是一个 debug 性质的操作
  console.log("career", career);
  // UI 逻辑的改动在于，name和age成了可选的展示项，若值为空，则不展示
  return (
    <div className="personalInfo">
    {name ? <p>姓名：{name}</p> : null}
  {age ? <p>年龄：{age}</p> : null}
    <p>职业：{career}</p>
    <button
   onClick={() => {
     setName("秀妍");
   }}
     >
     修改姓名
     </button>
     </div>
   );
  }
  export default PersonalInfoComponent;
```

<font style="color:rgb(44, 62, 80);">修改后的组件在初始渲染的时候，界面与上个版本无异：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872413169-0a414c8c-cf59-4476-8075-974d36a2c67a.png)

**<font style="color:rgb(44, 62, 80);">注意</font>**<font style="color:rgb(44, 62, 80);">，你在自己电脑上模仿这段代码的时候，千万不要漏掉 if 语句里面</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">// eslint-disable-next-line</font><font style="color:rgb(44, 62, 80);">这个注释——因为目前大部分的 React 项目都在内部预置了对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React-Hooks-Rule</font><font style="color:rgb(44, 62, 80);">（React-Hooks 使用规则）的强校验，而示例代码中把 Hooks 放进 if 语句的操作作为一种不合规操作，会被直接识别为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Error</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">级别的错误，进而导致程序报错。这里我们只有将相关代码的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eslint</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">校验给禁用掉，才能够避免校验性质的报错，从而更直观地看到错误的效果到底是什么样的，进而理解错误的原因。</font>

<font style="color:rgb(44, 62, 80);">修改后的组件在初始挂载的时候，实际执行的逻辑内容和上个版本是没有区别的，都涉及对 name、age、career 三个状态的获取和渲染。理论上来说，变化应该发生在我单击“修改姓名”之后触发的二次渲染里：二次渲染时，isMounted 已经被置为 true，if 内部的逻辑会被直接跳过。此时按照代码注释中给出的设计意图，这里我希望在二次渲染时，只获取并展示 career 这一个状态。那么事情是否会如我所愿呢？我们一起来看看单击“修改姓名”按钮后会发生什么：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872411184-065b78a0-fbf6-4d4b-a37f-a5ac06b198e3.png)

<font style="color:rgb(44, 62, 80);">组件不仅没有像预期中一样发生界面变化，甚至直接报错了。报错信息提醒我们，这是因为“组件渲染的 Hooks 比期望中更少”</font>

<font style="color:rgb(44, 62, 80);">确实，按照现有的逻辑，初始渲染调用了三次</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useState</font><font style="color:rgb(44, 62, 80);">，而二次渲染时只会调用一次。但仅仅因为这个，就要报错吗？</font>

<font style="color:rgb(44, 62, 80);">按道理来说，二次渲染的时候，只要我获取到的 career 值没有问题，那么渲染就应该是没有问题的（因为二次渲染实际只会渲染 career 这一个状态），React 就没有理由阻止我的渲染动作。啊这……难道是 career 出问题了吗？还好我们预先留了一手 Debug 逻辑，每次渲染的时候都会尝试去输出一次 isMounted 和 career 这两个变量的值。现在我们就赶紧来看看，这两个变量到底是什么情况。</font>

<font style="color:rgb(44, 62, 80);">首先我将界面重置回初次挂载的状态，观察控制台的输出，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872413818-3268ee1d-323d-4d7c-af2e-d923cbfb4fe1.png)

<font style="color:rgb(44, 62, 80);">这里我把关键的 isMounted 和 career 两个变量用红色框框圈了出来：isMounted 值为 false，说明是初次渲染；career 值为“我是一个前端，爱吃小熊饼干”，这也是没有问题的。</font>

<font style="color:rgb(44, 62, 80);">接下来单击“修改姓名”按钮后，我们再来看一眼两个变量的内容，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872415253-f486682e-1dec-4344-9a1e-7316060ed143.png)

<font style="color:rgb(44, 62, 80);">二次渲染时，isMounted 为 true，这个没毛病。但是 career 竟然被修改为了“秀妍”，这也太诡异了？代码里面可不是这么写的。赶紧回头确认一下按钮单击事件的回调内容，代码如下所示：</font>

```plain
<button
   onClick={() => {
    setName("秀妍");
  }}
   >
  修改姓名
</button>
```

<font style="color:rgb(44, 62, 80);">确实，代码是没错的，我们调用的是 setName，那么它修改的状态也应该是 name，而不是 career。</font>

<font style="color:rgb(44, 62, 80);">那为什么最后发生变化的竟然是 career 呢？年轻人，不如我们一起来看一看 Hooks 的实现机制吧！</font>

## [](https://www.123fe.net/principle-docs/react/14-%E6%B7%B1%E5%85%A5%20React%20Hooks%20%E5%B7%A5%E4%BD%9C%E6%9C%BA%E5%88%B6.html#%E4%BB%8E%E6%BA%90%E7%A0%81%E8%B0%83%E7%94%A8%E6%B5%81%E7%A8%8B%E7%9C%8B%E5%8E%9F%E7%90%86-hooks-%E7%9A%84%E6%AD%A3%E5%B8%B8%E8%BF%90%E4%BD%9C-%E5%9C%A8%E5%BA%95%E5%B1%82%E4%BE%9D%E8%B5%96%E4%BA%8E%E9%A1%BA%E5%BA%8F%E9%93%BE%E8%A1%A8)从源码调用流程看原理：Hooks 的正常运作，在底层依赖于顺序链表
<font style="color:rgb(44, 62, 80);">这里强调“源码流程”而非“源码”，主要有两方面的考虑：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React-Hooks</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在源码层面和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fiber</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">关联十分密切</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">原理 !== 源码</font><font style="color:rgb(44, 62, 80);">，阅读源码只是掌握原理的一种手段，在某些场景下，阅读源码确实能够迅速帮我们定位到问题的本质（比如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的源码就可以快速帮我们理解 JSX 转换出来的到底是什么东西）；而 React-Hooks 的源码链路相对来说比较长，涉及的关键函数 renderWithHooks 中“脏逻辑”也比较多，整体来说，学习成本比较高，学习效果也难以保证。</font>

**<font style="color:rgb(44, 62, 80);">以 useState 为例，分析 React-Hooks 的调用链路</font>**

<font style="color:rgb(44, 62, 80);">首先要说明的是，React-Hooks 的调用链路在首次渲染和更新阶段是不同的，这里我将两个阶段的链路各总结进了两张大图里，我们依次来看。首先是首次渲染的过程，请看下图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872415261-161770d5-a651-4dca-832a-feb2c5598908.png)

<font style="color:rgb(44, 62, 80);">在这个流程中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">触发的一系列操作最后会落到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里面去，所以我们重点需要关注的就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">做了什么事情。以下我为你提取了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的源码：</font>

```javascript
// 进入 mounState 逻辑
function mountState(initialState) {

  // 将新的 hook 对象追加进链表尾部
  var hook = mountWorkInProgressHook();

  // initialState 可以是一个回调，若是回调，则取回调执行后的值
  if (typeof initialState === 'function') {
    // $FlowFixMe: Flow doesn't like mixed types
    initialState = initialState();
  }

  // 创建当前 hook 对象的更新队列，这一步主要是为了能够依序保留 dispatch
  const queue = hook.queue = {
    last: null,
    dispatch: null,
    lastRenderedReducer: basicStateReducer,
    lastRenderedState: (initialState: any),
  };

  // 将 initialState 作为一个“记忆值”存下来
  hook.memoizedState = hook.baseState = initialState;

  // dispatch 是由上下文中一个叫 dispatchAction 的方法创建的，这里不必纠结这个方法具体做了什么
  var dispatch = queue.dispatch = dispatchAction.bind(null, currentlyRenderingFiber$1, queue);
  // 返回目标数组，dispatch 其实就是示例中常常见到的 setXXX 这个函数，想不到吧？哈哈
  return [hook.memoizedState, dispatch];
}
```

<font style="color:rgb(44, 62, 80);">从这段源码中我们可以看出，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的主要工作是初始化</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Hooks</font><font style="color:rgb(44, 62, 80);">。在整段源码中，最需要关注的是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountWorkInProgressHook</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法，它为我们道出了 Hooks 背后的数据结构组织形式。以下是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountWorkInProgressHook</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的源码：</font>

```plain
function mountWorkInProgressHook() {
  // 注意，单个 hook 是以对象的形式存在的
  var hook = {
    memoizedState: null,
    baseState: null,
    baseQueue: null,
    queue: null,
    next: null
  };
  if (workInProgressHook === null) {
    // 这行代码每个 React 版本不太一样，但做的都是同一件事：将 hook 作为链表的头节点处理
    firstWorkInProgressHook = workInProgressHook = hook;
  } else {
    // 若链表不为空，则将 hook 追加到链表尾部
    workInProgressHook = workInProgressHook.next = hook;
  }
  // 返回当前的 hook
  return workInProgressHook;
}
```

<font style="color:rgb(44, 62, 80);">到这里可以看出，hook 相关的所有信息收敛在一个 hook 对象里，而 hook 对象之间以单向链表的形式相互串联。</font>

<font style="color:rgb(44, 62, 80);">接下来我们再看更新过程的大图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872415245-b7ab922d-20eb-48aa-81fe-e553f514e85a.png)

<font style="color:rgb(44, 62, 80);">根据图中高亮部分的提示不难看出，首次渲染和更新渲染的区别，在于调用的是 mountState，还是 updateState。mountState 做了什么，你已经非常清楚了；而 updateState 之后的操作链路，虽然涉及的代码有很多，但其实做的事情很容易理解：按顺序去遍历之前构建好的链表，取出对应的数据信息进行渲染。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们把 mountState 和 updateState 做的事情放在一起来看：mountState（首次渲染）构建链表并渲染；updateState 依次遍历链表并渲染。</font>

<font style="color:rgb(44, 62, 80);">看到这里，你是不是已经大概知道怎么回事儿了？没错，hooks 的渲染是通过“依次遍历”来定位每个 hooks 内容的。如果前后两次读到的链表在顺序上出现差异，那么渲染的结果自然是不可控的。</font>

<font style="color:rgb(44, 62, 80);">这个现象有点像我们构建了一个长度确定的数组，数组中的每个坑位都对应着一块确切的信息，后续每次从数组里取值的时候，只能够通过索引（也就是位置）来定位数据。也正因为如此，在许多文章里，都会直截了当地下这样的定义：Hooks 的本质就是数组。但读完这一课时的内容你就会知道，Hooks 的本质其实是链表。</font>

<font style="color:rgb(44, 62, 80);">接下来我们把这个已知的结论还原到 PersonalInfoComponent 里去，看看实际项目中，变量到底是怎么发生变化的。</font>

## [](https://www.123fe.net/principle-docs/react/14-%E6%B7%B1%E5%85%A5%20React%20Hooks%20%E5%B7%A5%E4%BD%9C%E6%9C%BA%E5%88%B6.html#%E7%AB%99%E5%9C%A8%E5%BA%95%E5%B1%82%E8%A7%86%E8%A7%92-%E9%87%8D%E7%8E%B0-personalinfocomponent-%E7%BB%84%E4%BB%B6%E7%9A%84%E6%89%A7%E8%A1%8C%E8%BF%87%E7%A8%8B)站在底层视角，重现 PersonalInfoComponent 组件的执行过程
<font style="color:rgb(44, 62, 80);">我们先来复习一下修改过后的 PersonalInfoComponent 组件代码：</font>

```javascript
import React, { useState } from "react";
// isMounted 用于记录是否已挂载（是否是首次渲染）
let isMounted = false;
function PersonalInfoComponent() {
  // 定义变量的逻辑不变
  let name, age, career, setName, setCareer;

  // 这里追加对 isMounted 的输出，这是一个 debug 性质的操作
  console.log("isMounted is", isMounted);
  // 这里追加 if 逻辑：只有在首次渲染（组件还未挂载）时，才获取 name、age 两个状态
  if (!isMounted) {
    // eslint-disable-next-line
    [name, setName] = useState("修言");
    // eslint-disable-next-line
    [age] = useState("99");

    // if 内部的逻辑执行一次后，就将 isMounted 置为 true（说明已挂载，后续都不再是首次渲染了）
    isMounted = true;
  }

  // 对职业信息的获取逻辑不变
  [career, setCareer] = useState("我是一个前端，爱吃小熊饼干");
  // 这里追加对 career 的输出，这也是一个 debug 性质的操作
  console.log("career", career);
  // UI 逻辑的改动在于，name 和 age 成了可选的展示项，若值为空，则不展示
  return (
    <div className="personalInfo">
    {name ? <p>姓名：{name}</p> : null}
  {age ? <p>年龄：{age}</p> : null}
    <p>职业：{career}</p>
    <button
   onClick={() => {
     setName("秀妍");
   }}
     >
     修改姓名
     </button>
     </div>
   );
  }
  export default PersonalInfoComponent;
```

<font style="color:rgb(44, 62, 80);">从代码里面，我们可以提取出来的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">调用有三个：</font>

```plain
[name, setName] = useState("修言");
[age] = useState("99");
[career, setCareer] = useState("我是一个前端，爱吃小熊饼干");
```

<font style="color:rgb(44, 62, 80);">这三个调用在首次渲染的时候都会发生，伴随而来的链表结构如图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872416204-b051efa9-e254-401f-b1cd-3dc1a9781265.png)

<font style="color:rgb(44, 62, 80);">当首次渲染结束，进行二次渲染的时候，实际发生的 useState 调用只有一个：</font>

```plain
useState("我是一个前端，爱吃小熊饼干")
```

<font style="color:rgb(44, 62, 80);">而此时的链表情况如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872416768-e2dd8bb6-754d-4a69-82a2-baad02a516c2.png)

<font style="color:rgb(44, 62, 80);">我们再复习一遍更新（二次渲染）的时候会发生什么事情：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会依次遍历链表、读取数据并渲染。注意这个过程就像从数组中依次取值一样，是完全按照顺序（或者说索引）来的。因此 React 不会看你命名的变量名是 career 还是别的什么，它只认你这一次 useState 调用，于是它难免会认为：喔，原来你想要的是第一个位置的 hook 啊。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872416378-65c5388c-604b-4e9c-b2fa-5fcbd46b7a93.png)

<font style="color:rgb(44, 62, 80);">如此一来，career 就自然而然地取到了链表头节点 hook 对象中的“秀妍”这个值。</font>

