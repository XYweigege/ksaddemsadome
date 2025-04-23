## React-Hooks 设计动机初探
### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#%E4%BD%95%E8%B0%93%E7%B1%BB%E7%BB%84%E4%BB%B6-class-component)<font style="color:rgb(44, 62, 80);">何谓类组件（Class Component）</font>
<font style="color:rgb(44, 62, 80);">所谓类组件，就是基于 ES6 Class 这种写法，通过继承</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.Component</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">得来的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件。以下是一个典型的类组件：</font>

```javascript
class DemoClass extends React.Component {

  // 初始化类组件的 state
  state = {
    text: ""
  };
  // 编写生命周期方法 didMount
  componentDidMount() {
    // 省略业务逻辑
  }
  // 编写自定义的实例方法
  changeText = (newText) => {
    // 更新 state
    this.setState({
      text: newText
    });
  };
  // 编写生命周期方法 render
  render() {
    return (
      <div className="demoClass">
      <p>{this.state.text}</p>
  <button onClick={this.changeText}>点我修改</button>
  </div>
);
}
}
```

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#%E4%BD%95%E8%B0%93%E5%87%BD%E6%95%B0%E7%BB%84%E4%BB%B6-%E6%97%A0%E7%8A%B6%E6%80%81%E7%BB%84%E4%BB%B6-function-component-stateless-component)<font style="color:rgb(44, 62, 80);">何谓函数组件/无状态组件（Function Component/Stateless Component）</font>
<font style="color:rgb(44, 62, 80);">函数组件顾名思义，就是以函数的形态存在的 React 组件。早期并没有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React-Hooks</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的加持，函数组件内部无法定义和维护</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">，因此它还有一个别名叫“无状态组件”。以下是一个典型的函数组件：</font>

```javascript
function DemoFunction(props) {
  const { text } = props
  return (
    <div className="demoFunction">
    <p>{`function 组件所接收到的来自外界的文本内容是：[${text}]`}</p>
  </div>
);
}
```

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#%E5%87%BD%E6%95%B0%E7%BB%84%E4%BB%B6%E4%B8%8E%E7%B1%BB%E7%BB%84%E4%BB%B6%E7%9A%84%E5%AF%B9%E6%AF%94-%E6%97%A0%E5%85%B3-%E4%BC%98%E5%8A%A3-%E5%8F%AA%E8%B0%88-%E4%B8%8D%E5%90%8C)<font style="color:rgb(44, 62, 80);">函数组件与类组件的对比：无关“优劣”，只谈“不同”</font>
<font style="color:rgb(44, 62, 80);">我们先基于上面的两个 Demo，从形态上对两种组件做区分。它们之间肉眼可见的区别就包括但不限于：</font>

+ <font style="color:rgb(44, 62, 80);">类组件需要继承</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);">，函数组件不需要；</font>
+ <font style="color:rgb(44, 62, 80);">类组件可以访问生命周期方法，函数组件不能；</font>
+ <font style="color:rgb(44, 62, 80);">类组件中可以获取到实例化后的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">，并基于这个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">做各种各样的事情，而函数组件不可以；</font>
+ <font style="color:rgb(44, 62, 80);">类组件中可以定义并维护 state（状态），而函数组件不可以；</font>

<font style="color:rgb(44, 62, 80);">单就我们列出的这几点里面，频繁出现了“类组件可以 xxx，函数组件不可以 xxx”，这是否就意味着类组件比函数组件更好呢？</font>

<font style="color:rgb(44, 62, 80);">答案当然是否定的。你可以说，在 React-Hooks 出现之前的世界里，类组件的能力边界明显强于函数组件，但要进一步推导“类组件强于函数组件”，未免显得有些牵强。同理，一些文章中一味鼓吹函数组件轻量优雅上手迅速，不久的将来一定会把类组件干没（类组件：我做错了什么？）之类的，更是不可偏听偏信。</font>

<font style="color:rgb(44, 62, 80);">当我们讨论这两种组件形式时，不应怀揣“孰优孰劣”这样的成见，而应该更多地去关注两者的不同，进而把不同的特性与不同的场景做连接，这样才能求得一个全面的、辩证的认知。</font>

## [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#%E9%87%8D%E6%96%B0%E7%90%86%E8%A7%A3%E7%B1%BB%E7%BB%84%E4%BB%B6-%E5%8C%85%E8%A3%B9%E5%9C%A8%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E6%80%9D%E6%83%B3%E4%B8%8B%E7%9A%84-%E9%87%8D%E8%A3%85%E6%88%98%E8%88%B0)重新理解类组件
<font style="color:rgb(44, 62, 80);">类组件是面向对象编程思想的一种表征。面向对象是一个老生常谈的概念了，当我们应用面向对象的时候，总是会有意或无意地做这样两件事情。</font>

+ <font style="color:rgb(44, 62, 80);">封装：将一类属性和方法，“聚拢”到一个 Class 里去。</font>
+ <font style="color:rgb(44, 62, 80);">继承：新的 Class 可以通过继承现有 Class，实现对某一类属性和方法的复用。</font>

<font style="color:rgb(44, 62, 80);">React 类组件也不例外。我们再次审视一下这个典型的类组件 Case：</font>

```javascript
class DemoClass extends React.Component {

  // 初始化类组件的 state
  state = {
    text: ""
  };
  // 编写生命周期方法 didMount
  componentDidMount() {
    // 省略业务逻辑
  }
  // 编写自定义的实例方法
  changeText = (newText) => {
    // 更新 state
    this.setState({
      text: newText
    });
  };
  // 编写生命周期方法 render
  render() {
    return (
      <div className="demoClass">
      <p>{this.state.text}</p>
  <button onClick={this.changeText}>点我修改</button>
  </div>
);
}
}
```

<font style="color:rgb(44, 62, 80);">不难看出，React 类组件内部预置了相当多的“现成的东西”等着你去调度/定制，state 和生命周期就是这些“现成东西”中的典型。要想得到这些东西，难度也不大，你只需要轻轻地继承一个 React.Component 即可。</font>

<font style="color:rgb(44, 62, 80);">这种感觉就好像是你不费吹灰之力，就拥有了一辆“重装战舰”，该有的枪炮导弹早已配备整齐，就等你操纵控制台上的一堆开关了。</font>

<font style="color:rgb(44, 62, 80);">毋庸置疑，类组件给到开发者的东西是足够多的，但“多”就是“好”吗？其实未必。</font>

<font style="color:rgb(44, 62, 80);">把一个人塞进重装战舰里，他就一定能操纵这台战舰吗？如果他没有经过严格的训练，不清楚每一个操作点的内涵，那他极有可能会把炮弹打到友军的营地里去。</font>

<font style="color:rgb(44, 62, 80);">React 类组件，也有同样的问题——它提供了多少东西，你就需要学多少东西。假如背不住生命周期，你的组件逻辑顺序大概率会变成一团糟。“大而全”的背后，是不可忽视的学习成本。</font>

<font style="color:rgb(44, 62, 80);">再想这样一个场景：假如我现在只是需要打死一只蚊子，而不是打掉一个军队。这时候继续开动重装战舰，是不是正应了那句老话——“可以，但没有必要”。这也是类组件的一个不便，它太重了，对于解决许多问题来说，编写一个类组件实在是一个过于复杂的姿势。复杂的姿势必然带来高昂的理解成本，这也是我们所不想看到的。</font>

<font style="color:rgb(44, 62, 80);">更要命的是，由于开发者编写的逻辑在封装后是和组件粘在一起的，这就使得类**组件内部的逻辑难以实现拆分和复用。**如果你想要打破这个僵局，则需要进一步学习更加复杂的设计模式（比如高阶组件、Render Props 等），用更高的学习成本来交换一点点编码的灵活度。</font>

<font style="color:rgb(44, 62, 80);">这一切的一切，光是想想就让人头秃。所以说，类组件固然强大， 但它绝非万能。</font>

## [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E5%87%BD%E6%95%B0%E7%BB%84%E4%BB%B6-%E5%91%BC%E5%BA%94-react-%E8%AE%BE%E8%AE%A1%E6%80%9D%E6%83%B3%E7%9A%84-%E8%BD%BB%E5%B7%A7%E5%BF%AB%E8%89%87)深入理解函数组件
<font style="color:rgb(44, 62, 80);">我们再来看这个函数组件的 case：</font>

```javascript
function DemoFunction(props) {
  const { text } = props
  return (
    <div className="demoFunction">
    <p>{`function 组件所接收到的来自外界的文本内容是：[${text}]`}</p>
  </div>
);
}
```

<font style="color:rgb(44, 62, 80);">当然啦，要是你以为函数组件的简单是因为它只能承担渲染这一种任务，那可就太小瞧它了。它同样能够承接相对复杂的交互逻辑，像这样：</font>

```javascript
function DemoFunction(props) {
  const { text } = props 

  const showAlert = ()=> {
    alert(`我接收到的文本是${text}`)
  } 

  return (
    <div className="demoFunction">
    <p>{`function 组件所接收到的来自外界的文本内容是：[${text}]`}</p>
  <button onClick={showAlert}>点击弹窗</button>
  </div>
);
}
```

<font style="color:rgb(44, 62, 80);">相比于类组件，函数组件肉眼可见的特质自然包括轻量、灵活、易于组织和维护、较低的学习成本等。</font>

<font style="color:rgb(44, 62, 80);">类组件和函数组件之间，纵有千差万别，但最不能够被我们忽视掉的，是心智模式层面的差异，是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">面向对象和函数式编程</font><font style="color:rgb(44, 62, 80);">这两套不同的设计思想之间的差异。</font>

<font style="color:rgb(44, 62, 80);">说得更具体一点，函数组件更加契合 React 框架的设计理念。何出此言？不要忘了这个赫赫有名的 React 公式：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872208155-d7b5e8ae-310c-44f0-a1ce-1e38c225e202.png)

<font style="color:rgb(44, 62, 80);">不夸张地说，React 组件本身的定位就是函数，一个吃进数据、吐出 UI 的函数。作为开发者，我们编写的是声明式的代码，而 React 框架的主要工作，就是及时地把声明式的代码转换为命令式的 DOM 操作，把数据层面的描述映射到用户可见的 UI 变化中去。这就意味着从原则上来讲，React 的数据应该总是紧紧地和渲染绑定在一起的，而类组件做不到这一点。</font>

<font style="color:rgb(44, 62, 80);">首先我们来看这样一个类组件：</font>

```javascript
class ProfilePage extends React.Component {
  showMessage = () => {
    alert('Followed ' + this.props.user);
  };
  handleClick = () => {
    setTimeout(this.showMessage, 3000);
  };
  render() {
    return <button onClick={this.handleClick}>Follow</button>;
  }
}
```

<font style="color:rgb(44, 62, 80);">这个组件返回的是一个按钮，交互内容也很简单：点击按钮后，过 3s，界面上会弹出“Followed xxx”的文案。类似于我们在微博上点击“关注某人”之后弹出的“已关注”这样的提醒。</font>

<font style="color:rgb(44, 62, 80);">看起来好像没啥毛病，但是如果你在这个</font>[在线 Demo(opens new window)](https://codesandbox.io/s/pjqnl16lm7)<font style="color:rgb(44, 62, 80);">中尝试点击基于类组件形式编写的 ProfilePage 按钮后 3s 内把用户切换为 Sophie，你就会看到如下图所示的效果：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872208158-59c825d6-c483-4706-ae9f-1581974b9e88.png)

<font style="color:rgb(44, 62, 80);">明明我们是在 Dan 的主页点击的关注，结果却提示了“Followed Sophie”！</font>

<font style="color:rgb(44, 62, 80);">这个现象必然让许多人感到困惑：user 的内容是通过 props 下发的，props 作为不可变值，为什么会从 Dan 变成 Sophie 呢？</font>

<font style="color:rgb(44, 62, 80);">因为虽然 props 本身是不可变的，但 this 却是可变的，this 上的数据是可以被修改的，this.props 的调用每次都会获取最新的 props，而这正是 React 确保数据实时性的一个重要手段。</font>

<font style="color:rgb(44, 62, 80);">多数情况下，在 React 生命周期对执行顺序的调控下，this.props 和 this.state 的变化都能够和预期中的渲染动作保持一致。但在这个案例中，我们通过 setTimeout 将预期中的渲染推迟了 3s，打破了 this.props 和渲染动作之间的这种时机上的关联，进而导致渲染时捕获到的是一个错误的、修改后的 this.props。这就是问题的所在。</font>

<font style="color:rgb(44, 62, 80);">但如果我们把 ProfilePage 改造为一个像这样的函数组件：</font>

```javascript
function ProfilePage(props) {
  const showMessage = () => {
    alert('Followed ' + props.user);
  };
  const handleClick = () => {
    setTimeout(showMessage, 3000);
  };
  return (
    <button onClick={handleClick}>Follow</button>
  );
}
```

<font style="color:rgb(44, 62, 80);">事情就会大不一样。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ProfilePage</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数执行的一瞬间就被捕获，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">本身又是一个不可变值，因此我们可以充分确保从现在开始，在任何时机下读取到的 props，都是最初捕获到的那个 props。当父组件传入新的 props 来尝试重新渲染 ProfilePage 时，本质上是基于新的 props 入参发起了一次全新的函数调用，并不会影响上一次调用对上一个 props 的捕获。这样一来，我们便确保了渲染结果确实能够符合预期。</font>

<font style="color:rgb(44, 62, 80);">如果你认真阅读了我前面说过的那些话，相信你现在一定也**不仅仅能够充分理解 Dan 所想要表达的“函数组件会捕获 render 内部的状态”**这个结论，而是能够更进一步地意识到这样一件事情：函数组件真正地把数据和渲染绑定到了一起。</font>

<font style="color:rgb(44, 62, 80);">经过岁月的洗礼，React 团队显然也认识到了，函数组件是一个更加匹配其设计理念、也更有利于逻辑拆分与重用的组件表达形式，接下来便开始“用脚投票”，用实际行动支持开发者编写函数式组件。于是，React-Hooks 便应运而生。</font>

## [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#hooks-%E7%9A%84%E6%9C%AC%E8%B4%A8-%E4%B8%80%E5%A5%97%E8%83%BD%E5%A4%9F%E4%BD%BF%E5%87%BD%E6%95%B0%E7%BB%84%E4%BB%B6%E6%9B%B4%E5%BC%BA%E5%A4%A7%E3%80%81%E6%9B%B4%E7%81%B5%E6%B4%BB%E7%9A%84-%E9%92%A9%E5%AD%90)Hooks 的本质：一套能够使函数组件更强大、更灵活的“钩子”
<font style="color:rgb(44, 62, 80);">React-Hooks 是什么？它是一套能够使函数组件更强大、更灵活的“钩子”。</font>

<font style="color:rgb(44, 62, 80);">前面我们已经说过，函数组件比起类组件“少”了很多东西，比如生命周期、对 state 的管理等。这就给函数组件的使用带来了非常多的局限性，导致我们并不能使用函数这种形式，写出一个真正的全功能的组件。</font>

<font style="color:rgb(44, 62, 80);">React-Hooks 的出现，就是为了帮助函数组件补齐这些（相对于类组件来说）缺失的能力。</font>

<font style="color:rgb(44, 62, 80);">如果说函数组件是一台轻巧的快艇，那么 React-Hooks 就是一个内容丰富的零部件箱。“重装战舰”所预置的那些设备，这个箱子里基本全都有，同时它还不强制你全都要，而是允许你自由地选择和使用你需要的那些能力，然后将这些能力以 Hook（钩子）的形式“钩”进你的组件里，从而定制出一个最适合你的“专属战舰”。</font>

## [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#%E4%BB%8E%E6%A0%B8%E5%BF%83-api-%E7%9C%8B-hooks-%E7%9A%84%E5%9F%BA%E6%9C%AC%E5%BD%A2%E6%80%81)从核心 API 看 Hooks 的基本形态
### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#usestate-%E4%B8%BA%E5%87%BD%E6%95%B0%E7%BB%84%E4%BB%B6%E5%BC%95%E5%85%A5%E7%8A%B6%E6%80%81)<font style="color:rgb(44, 62, 80);">useState()：为函数组件引入状态</font>
<font style="color:rgb(44, 62, 80);">早期的函数组件相比于类组件，其一大劣势是缺乏定义和维护 state 的能力，而 state（状态）作为 React 组件的灵魂，必然是不可省略的。因此 React-Hooks 在诞生之初，就优先考虑了对 state 的支持。useState 正是这样一个能够为函数组件引入状态的 API。</font>

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#%E5%87%BD%E6%95%B0%E7%BB%84%E4%BB%B6-%E7%9C%9F%E7%9A%84%E5%BE%88%E8%BD%BB)<font style="color:rgb(44, 62, 80);">函数组件，真的很轻</font>
<font style="color:rgb(44, 62, 80);">在过去，你可能会为了使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">，不得不去编写一个类组件（这里我给出一个 Demo，编码如下所示）：</font>

```javascript
import React, { Component } from "react";
export default class TextButton extends Component {

  constructor() {
    super();
    this.state = {
      text: "初始文本"
    };
  }

  changeText = () => {
    this.setState(() => {
      return {
        text: "修改后的文本"
      };
    });
  };
  render() {
    const { text } = this.state;
    return (
      <div className="textButton">
      <p>{text}</p>
      <button onClick={this.changeText}>点击修改文本</button>
      </div>
    );
  }
}
```

<font style="color:rgb(44, 62, 80);">有了 useState 后，我们就可以直接在函数组件里引入 state。以下是使用 useState 改造过后的 TextButton 组件：</font>

```javascript
import React, { useState } from "react";
export default function Button() {
  const [text, setText] = useState("初始文本");
  function changeText() {
    return setText("修改后的文本");
  }
  return (
    <div className="textButton">
    <p>{text}</p>
    <button onClick={changeText}>点击修改文本</button>
    </div>
  );
}
```

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#usestate-%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B)<font style="color:rgb(44, 62, 80);">useState 快速上手</font>
<font style="color:rgb(44, 62, 80);">从用法上看，useState 返回的是一个数组，数组的第一个元素对应的是我们想要的那个 state 变量，第二个元素对应的是能够修改这个变量的 API。我们可以通过数组解构的语法，将这两个元素取出来，并且按照我们自己的想法命名。一个典型的调用示例如下：</font>

```plain
const [state, setState] = useState(initialState);
```

<font style="color:rgb(44, 62, 80);">在这个示例中，我们给自己期望的那个状态变量命名为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">，给修改</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的 API 命名为 setState。useState 中传入的 initialState 正是 state 的初始值。后续我们可以通过调用 setState，来修改 state 的值，像这样：</font>

```plain
setState(newState)
```

<font style="color:rgb(44, 62, 80);">状态更新后会触发渲染层面的更新，这点和类组件是一致的。</font>

<font style="color:rgb(44, 62, 80);">这里需要向初学者强调的一点是：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">状态和修改状态的 API 名都是可以自定义的</font><font style="color:rgb(44, 62, 80);">。比如在上文的 Demo 中，就分别将其自定义为 text 和 setText：</font>

```plain
const [text, setText] = useState("初始文本");
```

<font style="color:rgb(44, 62, 80);">“set + 具体变量名”这种命名形式，可以帮助我们快速地将 API 和它对应的状态建立逻辑联系。</font>

<font style="color:rgb(44, 62, 80);">当我们在函数组件中调用 React.useState 的时候，实际上是给这个组件关联了一个状态——注意，是“一个状态”而不是“一批状态”。这一点是相对于类组件中的 state 来说的。在类组件中，我们定义的 state 通常是一个这样的对象，如下所示：</font>

```plain
this.state {
  text: "初始文本",
  length: 10000,
  author: ["xiuyan", "cuicui", "yisi"]
}
```

<font style="color:rgb(44, 62, 80);">这个对象是“包容万物”的：整个组件的状态都在 state 对象内部做收敛，当我们需要某个具体状态的时候，会通过 this.state.xxx 这样的访问对象属性的形式来读取它。</font>

<font style="color:rgb(44, 62, 80);">而在 useState 这个钩子的使用背景下，state 就是单独的一个状态，它可以是任何你需要的 JS 类型。像这样：</font>

```plain
// 定义为数组
const [author, setAuthor] = useState(["xiuyan", "cuicui", "yisi"]);
 
// 定义为数值
const [length, setLength] = useState(100);
// 定义为字符串
const [text, setText] = useState("初始文本")
```

<font style="color:rgb(44, 62, 80);">你还可以定义为布尔值、对象等，都是没问题的。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">它就像类组件中 state 对象的某一个属性一样，对应着一个单独的状态，允许你存储任意类型的值</font><font style="color:rgb(44, 62, 80);">。</font>

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#useeffect-%E5%85%81%E8%AE%B8%E5%87%BD%E6%95%B0%E7%BB%84%E4%BB%B6%E6%89%A7%E8%A1%8C%E5%89%AF%E4%BD%9C%E7%94%A8%E6%93%8D%E4%BD%9C)<font style="color:rgb(44, 62, 80);">useEffect()：允许函数组件执行副作用操作</font>
<font style="color:rgb(44, 62, 80);">函数组件相比于类组件来说，最显著的差异就是 state 和生命周期的缺失。useState 为函数组件引入了 state，而 useEffect 则在一定程度上弥补了生命周期的缺席。</font>

<font style="color:rgb(44, 62, 80);">useEffect 能够为函数组件引入副作用。过去我们习惯放在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentDidMount、componentDidUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">componentWillUnmount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">三个生命周期里来做的事，现在可以放在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里来做，比如操作 DOM、订阅事件、调用外部 API 获取数据等。</font>

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#useeffect-%E5%92%8C%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%87%BD%E6%95%B0%E4%B9%8B%E9%97%B4%E7%9A%84-%E6%9B%BF%E6%8D%A2-%E5%85%B3%E7%B3%BB)<font style="color:rgb(44, 62, 80);">useEffect 和生命周期函数之间的“替换”关系</font>
<font style="color:rgb(44, 62, 80);">我们可以通过下面这个例子来理解</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和生命周期函数之间的替换关系。这里我先给到你一个用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">编写的函数组件示例：</font>

```javascript
// 注意 hook 在使用之前需要引入
import React, { useState, useEffect } from 'react';
// 定义函数组件
function IncreasingTodoList() {
  // 创建 count 状态及其对应的状态修改函数
  const [count, setCount] = useState(0);
  // 此处的定位与 componentDidMount 和 componentDidUpdate 相似
  useEffect(() => {
    // 每次 count 增加时，都增加对应的待办项
    const todoList = document.getElementById("todoList");
    const newItem = document.createElement("li");
    newItem.innerHTML = `我是第${count}个待办项`;
    todoList.append(newItem);
  });
  // 编写 UI 逻辑
  return (
    <div>
    <p>当前共计 {count} 个todo Item</p>
    <ul id="todoList"></ul>
    <button onClick={() => setCount(count + 1)}>点我增加一个待办项</button>
  </div>
);
}
```

<font style="color:rgb(44, 62, 80);">通过上面这段代码构造出来的界面在刚刚挂载完毕时，就是如下图所示的样子：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872207985-46f00722-d8ee-495b-9daf-de88543dc20b.png)

<font style="color:rgb(44, 62, 80);">IncreasingTodoList 是一个只允许增加 item 的 ToDoList（待办事项列表）。按照 useEffect 的设定，每当我们点击“点我增加一个待办项”这个按钮，驱动 count+1 的同时，DOM 结构里也会被追加一个 li 元素。以下是连击按钮三次之后的效果图：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718872208158-fcb4b62b-4fe7-4c35-bfaf-b6b3dff89eb6.png)

<font style="color:rgb(44, 62, 80);">同样的效果，按照注释里的提示，我们也可以通过编写 class 组件来实现：</font>

```javascript
import React from 'react';
// 定义类组件
class IncreasingTodoList extends React.Component {

  // 初始化 state
  state = { count: 0 }
  // 此处调用上个 demo 中 useEffect 中传入的函数
  componentDidMount() {
    this.addTodoItem()
  }
  // 此处调用上个 demo 中 useEffect 中传入的函数
  componentDidUpdate() {
    this.addTodoItem()
  }
  // 每次 count 增加时，都增加对应的待办项
  addTodoItem = () => {
    const { count } = this.state
    const todoList = document.getElementById("todoList")
    const newItem = document.createElement("li")
    newItem.innerHTML = `我是第${count}个待办项`
    todoList.append(newItem)
  }

  // 定义渲染内容
  render() {
    const { count } = this.state
    return (
      <div>
      <p>当前共计 {count} 个todo Item</p>
      <ul id="todoList"></ul>
      <button
    onClick={() =>
      this.setState({
        count: this.state.count + 1,
      })
      }
      >
      点我增加一个待办项
      </button>
      </div>
    )
  }
}
```

<font style="color:rgb(44, 62, 80);">通过这样一个对比，类组件生命周期和函数组件 useEffect 之间的转换关系可以说是跃然纸上了。</font>

<font style="color:rgb(44, 62, 80);">在这里，我提个醒：初学 useEffect 时，我们难免习惯于借助对生命周期的理解来推导对 useEffect 的理解。但长期来看，若是执着于这个学习路径，无疑将阻碍你真正从心智模式的层面拥抱 React-Hooks。</font>

<font style="color:rgb(44, 62, 80);">有时候，我们必须学会忘记旧的知识，才能够更好地拥抱新的知识。对于每一个学习 useEffect 的人来说，生命周期到 useEffect 之间的转换关系都不是最重要的，最重要的是在脑海中构建一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">“组件有副作用 → 引入 useEffect”</font><font style="color:rgb(44, 62, 80);">这样的条件反射——当你真正抛却类组件带给你的刻板印象、拥抱函数式编程之后，想必你会更加认同“useEffect 是用于为函数组件引入副作用的钩子”这个定义。</font>

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#useeffect-%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B)<font style="color:rgb(44, 62, 80);">useEffect 快速上手</font>
<font style="color:rgb(44, 62, 80);">useEffect 可以接收两个参数，分别是回调函数与依赖数组，如下面代码所示：</font>

```plain
useEffect(callBack, [])
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">用什么姿势来调用，本质上取决于你想用它来达成什么样的效果。下面我们就以效果为线索，简单介绍 useEffect 的调用规则。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">每一次渲染后都执行的副作用：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">传入回调函数，不传依赖数组</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。调用形式如下所示</font>

```plain
useEffect(callBack)
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">仅在挂载阶段执行一次的副作用：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">传入回调函数，且这个函数的返回值不是一个函数，同时传入一个空数组</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。调用形式如下所示：</font>

```plain
useEffect(()=>{
  // 这里是业务逻辑 
}, [])
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">仅在挂载阶段和卸载阶段执行的副作用：传入回调函数，且这个函数的返回值是一个函数，同时传入一个空数组。假如回调函数本身记为 A， 返回的函数记为 B，那么将在挂载阶段执行 A，卸载阶段执行 B。调用形式如下所示：</font>

```plain
useEffect(()=>{
  // 这里是 A 的业务逻辑

  // 返回一个函数记为 B
  return ()=>{
  }
}, [])
```

<font style="color:rgb(44, 62, 80);">这里需要注意，这种调用方式之所以会在卸载阶段去触发 B 函数的逻辑，是由 useEffect 的执行规则决定的：useEffect 回调中返回的函数被称为“清除函数”，当 React 识别到清除函数时，会在卸载时执行清除函数内部的逻辑。这个规律不会受第二个参数或者其他因素的影响，只要你在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect 回调中返回了一个函数，它就会被作为清除函数来处理</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">每一次渲染都触发，且卸载阶段也会被触发的副作用：传入回调函数，且这个函数的返回值是一个函数，同时不传第二个参数。如下所示：</font>

```plain
useEffect(()=>{
  // 这里是 A 的业务逻辑

  // 返回一个函数记为 B
  return ()=>{
  }
})
```

<font style="color:rgb(44, 62, 80);">上面这段代码就会使得 React 在每一次渲染都去触发 A 逻辑，并且在卸载阶段去触发 B 逻辑。</font>

<font style="color:rgb(44, 62, 80);">其实你只要记住，如果你有一段副作用逻辑需要在卸载阶段执行，那么把它写进 useEffect 回调的返回函数（上面示例中的 B 函数）里就行了。也可以认为，这个 B 函数的角色定位就类似于生命周期里 componentWillUnmount 方法里的逻辑</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">根据一定的依赖条件来触发的副作用：传入回调函数（若返回值是一个函数，仍然仅影响卸载阶段对副作用的处理，此处不再赘述），同时传入一个非空的数组，如下所示：</font>

```plain
useEffect(()=>{
  // 这是回调函数的业务逻辑 

  // 若 xxx 是一个函数，则 xxx 会在组件卸载时被触发
  return xxx
}, [num1, num2, num3])
```

<font style="color:rgb(44, 62, 80);">这里我给出的一个示意数组是 [num1, num2, num3]。首先需要说明，数组中的变量一般都是来源于组件本身的数据（props 或者 state）。若数组不为空，那么 React 就会在新的一次渲染后去对比前后两次的渲染，查看数组内是否有变量发生了更新（只要有一个数组元素变了，就会被认为更新发生了），并在有更新的前提下去触发 useEffect 中定义的副作用逻辑。</font>

## [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#hooks-%E6%98%AF%E5%A6%82%E4%BD%95%E5%B8%AE%E5%8A%A9%E6%88%91%E4%BB%AC%E5%8D%87%E7%BA%A7%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F%E7%9A%84)Hooks 是如何帮助我们升级工作模式的
<font style="color:rgb(44, 62, 80);">函数组件相比类组件来说，有着不少能够利好 React 组件开发的特性，而 React-Hooks 的出现正是为了强化函数组件的能力。现在，基于对 React-Hooks 编码层面的具体认知，想必你对“动机”的理解也已经上了一个台阶。这里我们就趁热打铁，针对“Why React-Hooks”这个问题，做一个加强版的总结。</font>

<font style="color:rgb(44, 62, 80);">相信有不少嗅觉敏锐的同学已经感觉到了——没错，这个环节就是手把手教你做“为什么需要 React-Hooks”这道面试题。以“Why xxx”开头的这种面试题，往往都没有标准答案，但会有一些关键的“点”，只要能答出关键的点，就足以证明你思考的方向是正确的，也就意味着这道题能给你加分。这里，我梳理了以下 4 条答题思路：</font>

+ <font style="color:rgb(44, 62, 80);">告别难以理解的 Class；</font>
+ <font style="color:rgb(44, 62, 80);">解决业务逻辑难以拆分的问题；</font>
+ <font style="color:rgb(44, 62, 80);">使状态逻辑复用变得简单可行；</font>
+ <font style="color:rgb(44, 62, 80);">函数组件从设计思想上来看，更加契合 React 的理念。</font>

<font style="color:rgb(44, 62, 80);">关于思路 4，我在上个课时已经讲得透透的了，这里我主要是借着代码的东风，把 1、2、3 摊开来给你看一下。</font>

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#_1-%E5%91%8A%E5%88%AB%E9%9A%BE%E4%BB%A5%E7%90%86%E8%A7%A3%E7%9A%84-class-%E6%8A%8A%E6%8F%A1-class-%E7%9A%84%E4%B8%A4%E5%A4%A7-%E7%97%9B%E7%82%B9)<font style="color:rgb(44, 62, 80);">1. 告别难以理解的 Class：把握 Class 的两大“痛点”</font>
<font style="color:rgb(44, 62, 80);">坊间总有传言说 Class 是“难以理解”的，这个说法的背后是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this 和生命周期</font><font style="color:rgb(44, 62, 80);">这两个痛点。</font>

<font style="color:rgb(44, 62, 80);">先来说说 this，在上个课时，你已经初步感受了一把 this 有多么难以捉摸。但那毕竟是个相对特殊的场景，更为我们所熟悉的，可能还是 React 自定义组件方法中的 this。看看下面这段代码：</font>

```javascript
class Example extends Component {
  state = {
    name: 'test',
    age: '99';
};
changeAge() {
  // 这里会报错
  this.setState({
    age: '100'
  });
}
render() {
  return <button onClick={this.changeAge}>{this.state.name}的年龄是{this.state.age}</button>
}
}
```

<font style="color:rgb(44, 62, 80);">你先不用关心组件具体的逻辑，就看 changeAge 这个方法：它是 button 按钮的事件监听函数。当我点击 button 按钮时，希望它能够帮我修改状态，但事实是，点击发生后，程序会报错。原因很简单，changeAge 里并不能拿到组件实例的 this</font>

<font style="color:rgb(44, 62, 80);">为了解决 this 不符合预期的问题，各路前端也是各显神通，之前用 bind、现在推崇箭头函数。但不管什么招数，本质上都是在用实践层面的约束来解决设计层面的问题。好在现在有了 Hooks，一切都不一样了，我们可以在函数组件里放飞自我（毕竟函数组件是不用关心 this 的）哈哈，解放啦</font>

<font style="color:rgb(44, 62, 80);">至于生命周期，它带来的麻烦主要有以下两个方面：</font>

+ <font style="color:rgb(44, 62, 80);">学习成本</font>
+ <font style="color:rgb(44, 62, 80);">不合理的逻辑规划方式</font>

<font style="color:rgb(44, 62, 80);">对于第一点，大家都学过生命周期，都懂。下面着重说说这“不合理的逻辑规划方式”是如何被 Hooks 解决掉的。</font>

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#_2-hooks-%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%9B%B4%E5%A5%BD%E7%9A%84%E9%80%BB%E8%BE%91%E6%8B%86%E5%88%86)<font style="color:rgb(44, 62, 80);">2. Hooks 如何实现更好的逻辑拆分</font>
<font style="color:rgb(44, 62, 80);">在过去，你是怎么组织自己的业务逻辑的呢？我想多数情况下应该都是先想清楚业务的需要是什么样的，然后将对应的业务逻辑拆到不同的生命周期函数里去——没错，逻辑曾经一度与生命周期耦合在一起。</font>

<font style="color:rgb(44, 62, 80);">在这样的前提下，生命周期函数常常做一些奇奇怪怪的事情：比如在 componentDidMount 里获取数据，在 componentDidUpdate 里根据数据的变化去更新 DOM 等。如果说你只用一个生命周期做一件事，那好像也还可以接受，但是往往在一个稍微成规模的 React 项目中，一个生命周期不止做一件事情。下面这段伪代码就很好地诠释了这一点：</font>

```javascript
componentDidMount() {
  // 1. 这里发起异步调用
  // 2. 这里从 props 里获取某个数据，根据这个数据更新 DOM

  // 3. 这里设置一个订阅

  // 4. 这里随便干点别的什么 

  // ...
}
componentWillUnMount() {
  // 在这里卸载订阅
}
componentDidUpdate() {
  // 1. 在这里根据 DidMount 获取到的异步数据更新 DOM

  // 2. 这里从 props 里获取某个数据，根据这个数据更新 DOM（和 DidMount 的第2步一样）
}
```

<font style="color:rgb(44, 62, 80);">像这样的生命周期函数，它的体积过于庞大，做的事情过于复杂，会给阅读和维护者带来很多麻烦。最重要的是，这些事情之间看上去毫无关联，逻辑就像是被“打散”进生命周期里了一样。比如，设置订阅和卸载订阅的逻辑，虽然它们在逻辑上是有强关联的，但是却只能被分散到不同的生命周期函数里去处理，这无论如何也不能算作是一个非常合理的设计。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">而在 Hooks 的帮助下，我们完全可以把这些繁杂的操作按照逻辑上的关联拆分进不同的函数组件里：我们可以有专门管理订阅的函数组件、专门处理 DOM 的函数组件、专门获取数据的函数组件等。Hooks 能够帮助我们实现业务逻辑的聚合，避免复杂的组件和冗余的代码。</font>

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#_3-%E7%8A%B6%E6%80%81%E5%A4%8D%E7%94%A8-hooks-%E5%B0%86%E5%A4%8D%E6%9D%82%E7%9A%84%E9%97%AE%E9%A2%98%E5%8F%98%E7%AE%80%E5%8D%95)<font style="color:rgb(44, 62, 80);">3. 状态复用：Hooks 将复杂的问题变简单</font>
<font style="color:rgb(44, 62, 80);">过去我们复用状态逻辑，靠的是 HOC（高阶组件）和 Render Props 这些组件设计模式，这是因为 React 在原生层面并没有为我们提供相关的途径。但这些设计模式并非万能，它们在实现逻辑复用的同时，也破坏着组件的结构，其中一个最常见的问题就是“嵌套地狱”现象。</font>

<font style="color:rgb(44, 62, 80);">Hooks 可以视作是 React 为解决状态逻辑复用这个问题所提供的一个原生途径。现在我们可以通过自定义 Hook，达到既不破坏组件结构、又能够实现逻辑复用的效果。</font>

### [](https://www.123fe.net/principle-docs/react/13-React%20Hooks%20%E8%AE%BE%E8%AE%A1%E5%8A%A8%E6%9C%BA%E4%B8%8E%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.html#%E4%BF%9D%E6%8C%81%E6%B8%85%E9%86%92-hooks-%E5%B9%B6%E9%9D%9E%E4%B8%87%E8%83%BD)<font style="color:rgb(44, 62, 80);">保持清醒：Hooks 并非万能</font>
<font style="color:rgb(44, 62, 80);">尽管我们已经说了这么多 Hooks 的“好话”，尽管 React 团队已经用脚投票表明了对函数组件的积极态度，但我们还是要谨记这样一个基本的认知常识：事事无绝对，凡事皆有两面性。更何况 React 仅仅是推崇函数组件，并没有“拉踩”类组件，甚至还官宣了“类组件和函数组件将继续共存”这件事情。这些都在提醒我们，在认识到 Hooks 带来的利好的同时，还需要认识到它的局限性。</font>

<font style="color:rgb(44, 62, 80);">关于 Hooks 的局限性，目前社区鲜少有人讨论。这里我想结合团队开发过程当中遇到的一些瓶颈，和你分享实践中的几点感受：</font>

+ **<font style="color:rgb(44, 62, 80);">Hooks 暂时还不能完全地为函数组件补齐类组件的能力</font>**<font style="color:rgb(44, 62, 80);">：比如 getSnapshotBeforeUpdate、componentDidCatch 这些生命周期，目前都还是强依赖类组件的</font>
+ **<font style="color:rgb(44, 62, 80);">“轻量”几乎是函数组件的基因，这可能会使它不能够很好地消化“复杂”</font>**<font style="color:rgb(44, 62, 80);">：我们有时会在类组件中见到一些方法非常繁多的实例，如果用函数组件来解决相同的问题，业务逻辑的拆分和组织会是一个很大的挑战。我个人的感觉是，从头到尾都在“过于复杂”和“过度拆分”之间摇摆不定，哈哈。耦合和内聚的边界，有时候真的很难把握，函数组件给了我们一定程度的自由，却也对开发者的水平提出了更高的要求。</font>
+ **<font style="color:rgb(44, 62, 80);">Hooks 在使用层面有着严格的规则约束</font>**<font style="color:rgb(44, 62, 80);">：对于如今的 React 开发者来说，如果不能牢记并践行 Hooks 的使用原则，如果对 Hooks 的关键原理没有扎实的把握，很容易把自己的 React 项目搞成大型车祸现场。</font>

