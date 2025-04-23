## 拆解 React 生命周期：从 React 15 说起
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在 React 15 中，大家需要关注以下几个生命周期方法：</font>

```plain
constructor()
componentWillReceiveProps()
shouldComponentUpdate()
componentWillMount()
componentWillUpdate()
componentDidUpdate()
componentDidMount()
render()
componentWillUnmount()
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如果你接触 React 足够早，或许会记得还有</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getDefaultProps</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getInitState</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这两个方法，它们都是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createClass()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">模式下初始化数据的方法。由于这种写法在 ES6 普及后已经不常见，这里不再详细展开</font>

<font style="color:rgb(44, 62, 80);">这些生命周期方法是如何彼此串联、相互依存的呢？这里我为你总结了一张大图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871793998-73be57f4-da57-4c24-b8bb-e2d4beb92a06.png)

<font style="color:rgb(44, 62, 80);">接下来，我就围绕这张大图，分阶段讨论组件生命周期的运作规律。在学习的过程中，下面这个 Demo 可以帮助你具体地验证每个阶段的工作流程：</font>

```javascript
import React from "react";
import ReactDOM from "react-dom";
// 定义子组件
class LifeCycle extends React.Component {
  constructor(props) {
    console.log("进入constructor");
    super(props);
    // state 可以在 constructor 里初始化
    this.state = { text: "子组件的文本" };
  }
  // 初始化渲染时调用
  componentWillMount() {
    console.log("componentWillMount方法执行");
  }
  // 初始化渲染时调用
  componentDidMount() {
    console.log("componentDidMount方法执行");
  }
  // 父组件修改组件的props时会调用
  componentWillReceiveProps(nextProps) {
    console.log("componentWillReceiveProps方法执行");
  }
  // 组件更新时调用
  shouldComponentUpdate(nextProps, nextState) {
    console.log("shouldComponentUpdate方法执行");
    return true;
  }

  // 组件更新时调用
  componentWillUpdate(nextProps, nextState) {
    console.log("componentWillUpdate方法执行");
  }
  // 组件更新后调用
  componentDidUpdate(preProps, preState) {
    console.log("componentDidUpdate方法执行");
  }
  // 组件卸载时调用
  componentWillUnmount() {
    console.log("子组件的componentWillUnmount方法执行");
  }
  // 点击按钮，修改子组件文本内容的方法
  changeText = () => {
    this.setState({
      text: "修改后的子组件文本"
    });
  };
  render() {
    console.log("render方法执行");
    return (
      <div className="container">
      <button onClick={this.changeText} className="changeText">
  修改子组件文本内容
  </button>
  <p className="textContent">{this.state.text}</p>
  <p className="fatherContent">{this.props.text}</p>
  </div>
);
}
}
// 定义 LifeCycle 组件的父组件
class LifeCycleContainer extends React.Component {

  // state 也可以像这样用属性声明的形式初始化
  state = {
    text: "父组件的文本",
    hideChild: false
  };
  // 点击按钮，修改父组件文本的方法
  changeText = () => {
    this.setState({
      text: "修改后的父组件文本"
    });
  };
  // 点击按钮，隐藏（卸载）LifeCycle 组件的方法
  hideChild = () => {
    this.setState({
      hideChild: true
    });
  };
  render() {
    return (
      <div className="fatherContainer">
      <button onClick={this.changeText} className="changeText">
  修改父组件文本内容
  </button>
  <button onClick={this.hideChild} className="hideChild">
  隐藏子组件
  </button>
  {this.state.hideChild ? null : <LifeCycle text={this.state.text} />}
  </div>
);
}
}
ReactDOM.render(<LifeCycleContainer />, document.getElementById("root"));
```

<font style="color:rgb(44, 62, 80);">该入口文件对应的 index.html 中预置了 id 为 root 的真实 DOM 节点作为根节点，body 标签内容如下：</font>

```html
<body>
  <div id="root"></div>
</body>
```

<font style="color:rgb(44, 62, 80);">这个 Demo 渲染到浏览器上大概是这样的：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871793973-bc087600-cd15-4dd4-bf1d-ac4d80284383.png)

<font style="color:rgb(44, 62, 80);">此处由于我们强调的是对生命周期执行规律的验证，所以样式上从简，你也可以根据自己的喜好添加 CSS 相关的内容。</font>

<font style="color:rgb(44, 62, 80);">接下来我们就结合这个 Demo 和开头的生命周期大图，一起来看看</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">挂载、更新、卸载这 3 个阶段</font><font style="color:rgb(44, 62, 80);">，React 组件都经历了哪些事情。</font>

## [](https://www.123fe.net/principle-docs/react/11-React16%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9B%B4%E6%94%B9%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E4%B8%8A.html#mounting-%E9%98%B6%E6%AE%B5-%E7%BB%84%E4%BB%B6%E7%9A%84%E5%88%9D%E5%A7%8B%E5%8C%96%E6%B8%B2%E6%9F%93-%E6%8C%82%E8%BD%BD)Mounting 阶段：组件的初始化渲染（挂载）
<font style="color:rgb(44, 62, 80);">挂载过程在组件的一生中仅会发生一次，在这个过程中，组件被初始化，然后会被渲染到真实 DOM 里，完成所谓的“首次渲染”。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在挂载阶段，一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">组件会按照顺序经历如下图所示的生命周期：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871794205-60a9d38a-aaa2-4b14-9873-be03a3ee0a0e.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">首先我们来看</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">constructor</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法，该方法仅仅在挂载的时候被调用一次，我们可以在该方法中对</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">进行初始化</font>

```plain
constructor(props) {
  console.log("进入constructor");
  super(props);
  // state 可以在 constructor 里初始化
  this.state = { text: "子组件的文本" };
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillMount</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法同样只会在挂载阶段被调用一次。其中</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillMount</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会在执行</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法前被触发</font>

<font style="color:rgb(44, 62, 80);">接下来</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render 方法被触发</font><font style="color:rgb(44, 62, 80);">。注意</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render 在执行过程中并不会去操作真实 DOM（也就是说不会渲染）</font><font style="color:rgb(44, 62, 80);">，它的职能是把需要渲染的内容返回出来。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">真实 DOM 的渲染工作，在挂载阶段是由 ReactDOM.render 来承接的</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount 方法在渲染结束后被触发</font><font style="color:rgb(44, 62, 80);">，此时因为真实 DOM 已经挂载到了页面上，我们可以在这个生命周期里执行真实 DOM 相关的操作。此外，类似于异步请求、数据初始化这样的操作也大可以放在这个生命周期来做</font>

<font style="color:rgb(44, 62, 80);">这一整个流程对应的其实就是我们 Demo 页面刚刚打开时，组件完成初始化渲染的过程。下图是 Demo 中的 LifeCycle 组件在挂载过程中控制台的输出，你可以用它来验证挂载过程中生命周期顺序的正确性：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871793562-e64b1c2a-e384-4f0e-ac25-a9f0b832ee90.png)

## [](https://www.123fe.net/principle-docs/react/11-React16%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9B%B4%E6%94%B9%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E4%B8%8A.html#updating-%E9%98%B6%E6%AE%B5-%E7%BB%84%E4%BB%B6%E7%9A%84%E6%9B%B4%E6%96%B0)Updating 阶段：组件的更新
<font style="color:rgb(44, 62, 80);">组件的更新分为两种：</font>

+ <font style="color:rgb(44, 62, 80);">一种是由父组件更新触发的更新；</font>
+ <font style="color:rgb(44, 62, 80);">另一种是组件自身调用自己的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">触发的更新。</font>

<font style="color:rgb(44, 62, 80);">这两种更新对应的生命周期流程如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871793929-62ee12fe-b801-45a3-9226-af02621f3b08.png)

**<font style="color:rgb(44, 62, 80);">componentWillReceiveProps 到底是由什么触发的？</font>**

<font style="color:rgb(44, 62, 80);">从图中你可以明显看出，父组件触发的更新和组件自身的更新相比，多出了这样一个生命周期方法：</font>

```plain
componentWillReceiveProps(nextProps)
```

<font style="color:rgb(44, 62, 80);">在这个生命周期方法里，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nextProps</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">表示的是接收到新</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内容，而现有的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">（相对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nextProps</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的“旧 props”）我们可以通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">拿到，由此便能够感知到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的变化</font>

<font style="color:rgb(44, 62, 80);">写到这里，就不得不在“变化”这个动作上深挖一下了。包括一些候选人面试时的回答里，都不约而同地见过/听过这样一种说法：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillReceiveProps</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是在组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内容发生了变化时被触发的。</font>

<font style="color:rgb(44, 62, 80);">这种说法不够严谨。远的不说，就拿咱们上文给出的 Demo 开刀，该界面的控制台输出在初始化完成后是这样的：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871796453-460f756f-e1d3-45ce-a343-eae3e8cd8fae.png)

<font style="color:rgb(44, 62, 80);">注意，我们代码里面，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LifeCycleContainer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个父组件传递给子组件 LifeCycle 的 props 只有一个 text：</font>

```html
<LifeCycle text={this.state.text} />
```

<font style="color:rgb(44, 62, 80);">假如我点击“修改父组件文本内容”这个按钮，父组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.state.text</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会发生改变，进而带动子组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.props.text</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">发生改变。此时一定会触发</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillReceiveProps</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个生命周期，这是毋庸置疑的：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871796056-6fc1614f-0856-4f1c-a288-d28131a1d840.png)

<font style="color:rgb(44, 62, 80);">但如果我现在对父组件的结构进行一个小小的修改，给它一个和子组件完全无关的 state（this.state.ownText），同时相应地给到一个修改这个 state 的方法（this.changeOwnText），并用一个新的 button 按钮来承接这个触发的动作。</font>

<font style="color:rgb(44, 62, 80);">改变后的 LifeCycleContainer 如下所示：</font>

```javascript
// 定义 LifeCycle 组件的父组件
class LifeCycleContainer extends React.Component {
  // state 也可以像这样用属性声明的形式初始化
  state = {
    text: "父组件的文本",
    // 新增的只与父组件有关的 state
    ownText: "仅仅和父组件有关的文本",
    hideChild: false
  };
  changeText = () => {
    this.setState({
      text: "修改后的父组件文本"
    });
  };
  // 修改 ownText 的方法
  changeOwnText = () => {
    this.setState({
      ownText: "修改后的父组件自有文本"
    });
  };
  hideChild = () => {
    this.setState({
      hideChild: true
    });
  };
  render() {
    return (
      <div className="fatherContainer">
      {/* 新的button按钮 */}
      <button onClick={this.changeOwnText} className="changeText">
      修改父组件自有文本内容
      </button>
      <button onClick={this.changeText} className="changeText">
      修改父组件文本内容
      </button>
      <button onClick={this.hideChild} className="hideChild">
      隐藏子组件
      </button>
      <p> {this.state.ownText} </p>
{this.state.hideChild ? null : <LifeCycle text={this.state.text} />}
  </div>
);
}
}
```

<font style="color:rgb(44, 62, 80);">新的界面如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871797494-9e7962cd-a133-4297-8b3c-446035f121a3.png)

<font style="color:rgb(44, 62, 80);">可以看到，this.state.ownText 这个状态和子组件完全无关。但是当我点击“修改父组件自有文本内容”这个按钮的时候，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentReceiveProps</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">仍然被触发了，效果如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871797433-cdca8fad-7433-488a-bb17-b9a86cad9590.png)

<font style="color:rgb(44, 62, 80);">面对这样的运行结果，我不由得要带你复习一下 React 官方文档中的这句话：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871797563-792a48bf-49a4-4a08-89d5-c2109d46d8c4.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentReceiveProps 并不是由 props 的变化触发的，而是由父组件的更新触发的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，这个结论，请你谨记。</font>

**<font style="color:rgb(44, 62, 80);">组件自身 setState 触发的更新</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.setState()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">调用后导致的更新流程，前面大图中已经有体现，这里我直接沿用上一个 Demo 来做演示。若我们点击上一个 Demo 中的“修改子组件文本内容”这个按钮：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871797642-9a3de9b5-179f-430b-9385-54c77cef5be4.png)

<font style="color:rgb(44, 62, 80);">这个动作将会触发子组件 LifeCycle 自身的更新流程，随之被触发的生命周期函数如下图增加的 console 内容所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871798916-917f6799-29f5-47b2-9972-694270de7376.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">先来说说</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillUpdate 和 componentDidUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这一对好基友。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">前被触发，它和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillMount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类似，允许你在里面做一些不涉及真实 DOM 操作的准备工作；而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">则在组件更新完毕后被触发，和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类似，这个生命周期也经常被用来处理 DOM 操作。此外，我们也常常将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的执行作为子组件更新完毕的标志通知到父组件</font>

**<font style="color:rgb(44, 62, 80);">render 与性能：初识 shouldComponentUpdate</font>**

<font style="color:rgb(44, 62, 80);">这里需要重点提一下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个生命周期方法，它的调用形式如下所示：</font>

```plain
shouldComponentUpdate(nextProps, nextState)
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render 方法由于伴随着对虚拟 DOM 的构建和对比，过程可以说相当耗时</font><font style="color:rgb(44, 62, 80);">。而在 React 当中，很多时候我们会不经意间就频繁地调用了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">。为了避免不必要的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">操作带来的性能开销，React 为我们提供了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个钩子。</font>

<font style="color:rgb(44, 62, 80);">React 组件会根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的返回值，来决定是否执行该方法之后的生命周期，进而决定是否对组件进行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">re-render（重渲染</font><font style="color:rgb(44, 62, 80);">）。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的默认值为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">，也就是说“无条件 re-render”。在实际的开发中，我们往往通过手动往</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中填充判定逻辑，或者直接在项目中引入</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等最佳实践，来实现“有条件的 re-render”。</font>

## [](https://www.123fe.net/principle-docs/react/11-React16%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E6%9B%B4%E6%94%B9%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E4%B8%8A.html#unmounting-%E9%98%B6%E6%AE%B5-%E7%BB%84%E4%BB%B6%E7%9A%84%E5%8D%B8%E8%BD%BD)Unmounting 阶段：组件的卸载
<font style="color:rgb(44, 62, 80);">组件的销毁阶段本身是比较简单的，只涉及一个生命周期，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871798830-147b4c77-8cbb-491d-90d8-73e1122317ed.png)

<font style="color:rgb(44, 62, 80);">对应上文的 Demo 来看，我们点击“隐藏子组件”后就可以把 LifeCycle 从父组件中移除掉，进而实现卸载的效果。整个过程如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871798281-a1a7d0f0-7480-4846-937d-424950078ebf.png)

<font style="color:rgb(44, 62, 80);">这个生命周期本身不难理解，我们重点说说怎么触发它。组件销毁的常见原因有以下两个。</font>

+ **<font style="color:rgb(44, 62, 80);">组件在父组件中被移除了</font>**<font style="color:rgb(44, 62, 80);">：这种情况相对比较直观，对应的就是我们上图描述的这个过程。</font>
+ **<font style="color:rgb(44, 62, 80);">组件中设置了 key 属性</font>**<font style="color:rgb(44, 62, 80);">，父组件在 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> 的过程中，发现 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> 值和上一次不一致，那么这个组件就会被干掉。</font>

