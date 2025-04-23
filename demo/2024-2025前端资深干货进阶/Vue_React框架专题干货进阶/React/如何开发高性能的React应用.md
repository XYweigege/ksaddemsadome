+ <font style="color:rgb(44, 62, 80);">使用 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> 规避冗余的更新逻辑</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent + Immutable.js</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">注：这 3 个思路同时也是 React 面试中“性能优化”这一环的核心所在</font>

## [](https://www.123fe.net/principle-docs/react/24-%E5%A6%82%E4%BD%95%E6%89%93%E9%80%A0%E9%AB%98%E6%80%A7%E8%83%BD%E7%9A%84%20React%20%E5%BA%94%E7%94%A8.html#%E5%96%84%E7%94%A8-shouldcomponentupdate)善用 shouldComponentUpdate
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的调用形式如下：</font>

```javascript
shouldComponentUpdate(nextProps, nextState)
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法由于伴随着对虚拟</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的构建和对比，过程可以说相当耗时。而在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当中，很多时候我们会不经意间就频繁地调用了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。为了避免不必要的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">操作带来的性能开销，React 提供了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个口子。React 组件会根据</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的返回值，来决定是否执行该方法之后的生命周期，进而决定是否对组件进行</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">re-render</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（重渲染）。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的默认值为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，也就是说 “无条件 re-render”。在实际的开发中，我们往往通过手动往</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中填充判定逻辑，来实现“有条件的 re-render”。</font>

<font style="color:rgb(44, 62, 80);">首先我们来看两个子组件的代码，这里为了尽量简化与数据变更无关的逻辑，ChildA 和 ChildB 都只负责从父组件处读取数据并渲染，它们的编码分别如下所示。</font>

```javascript
// ChildA.js：

import React from "react";
export default class ChildA extends React.Component {
  render() {
    console.log("ChildA 的render方法执行了");
    return (
      <div className="childA">
      子组件A的内容：
    {this.props.text}
    </div>
    );
  }
}
```

```javascript
// ChildB.js：

import React from "react";
export default class ChildB extends React.Component {
  render() {
    console.log("ChildB 的render方法执行了");
    return (
      <div className="childB">
      子组件B的内容：
    {this.props.text}
    </div>
    );
  }
}
```

```javascript
// 在共同的父组件 App.js 中，会将 ChildA 和 ChildB 组合起来，并分别向其中注入数据：

import React from "react";
import ChildA from './ChildA'
import ChildB from './ChildB'
class App extends React.Component {
  state = {
    textA: '我是A的文本',
    textB: '我是B的文本'
  }
  changeA = () => {
    this.setState({
      textA: 'A的文本被修改了'
    })
  }
  changeB = () => {
    this.setState({
      textB: 'B的文本被修改了'
    })
  }
  render() {
    return (
      <div className="App">
      <div className="container">
      <button onClick={this.changeA}>点击修改A处的文本</button>
      <button onClick={this.changeB}>点击修改B处的文本</button>
      <ul>
      <li>
      <ChildA text={this.state.textA}/>
  </li>
  <li>
  <ChildB text={this.state.textB}/>
  </li>
  </ul>
  </div>
  </div>
);
}
}
export default App;
```

<font style="color:rgb(44, 62, 80);">App 组件最终渲染到界面上的效果如下图所示，两个子组件在图中分别被不同颜色的标注圈出：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874111903-6a3f6f00-fca0-4741-97f2-692e481893c4.png)

<font style="color:rgb(44, 62, 80);">通过点击左右两个按钮，我们可以分别对 ChildA 和 ChildB 中的文案进行修改。</font>

<font style="color:rgb(44, 62, 80);">由于初次渲染时，两个组件的 render 函数都必然会被触发，因此控制台在挂载完成后的输出内容如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874111899-38f87ea0-dc3b-428f-97bb-b0f137037ad6.png)

<font style="color:rgb(44, 62, 80);">接下来我点击左侧的按钮，尝试对 A 处的文本进行修改。我们可以看到界面上只有 A 处的渲染效果发生了改变，如下图箭头处所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874111894-2908c7a0-4fa8-4824-ae09-34ceeae284d3.png)

<font style="color:rgb(44, 62, 80);">但是如果我们打开控制台，会发现输出的内容如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874111872-36fbcdcb-d9ad-4a6d-b74a-9880d7e08dcb.png)

<font style="color:rgb(44, 62, 80);">这样的输出结果告诉我们，在刚刚的点击动作后，不仅 ChildA 的 re-render 被触发了，ChildB 的 re-render 也被触发了。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在 React 中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">只要父组件发生了更新，那么所有的子组件都会被无条件更新</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。这就导致了 ChildB 的 props 尽管没有发生任何变化，它本身也没有任何需要被更新的点，却还是会走一遍更新流程。</font>

**<font style="color:rgb(44, 62, 80);">注：</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">同样的情况也适用于组件自身的更新：当组件自身调用了 setState 后，那么不管 setState 前后的状态内容是否真正发生了变化，它都会去走一遍更新流程</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">而在刚刚这个更新流程中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数没有被手动定义，因此它将返回“true”这个默认值。“true”则意味着对更新流程不作任何制止，也即所谓的“无条件 re-render”。在这种情况下，我们就可以考虑使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来对更新过程进行管控，避免没有意义的 re-render 发生。</font>

<font style="color:rgb(44, 62, 80);">现在我们就可以为 ChildB 加装这样一段</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">逻辑：</font>

```javascript
shouldComponentUpdate(nextProps, nextState) {
  // 判断 text 属性在父组件更新前后有没有发生变化，若没有发生变化，则返回 false
  if(nextProps.text === this.props.text) {
    return false
  }
  // 只有在 text 属性值确实发生变化时，才允许更新进行下去
  return true
}
```

<font style="color:rgb(44, 62, 80);">在这段逻辑中，我们对 ChildB 中的可变数据，也就是 this.props.text 这个属性进行了判断。</font>

<font style="color:rgb(44, 62, 80);">这样，当父组件 App 组件发生更新、进而试图触发 ChildB 的更新流程时，shouldComponentUpdate 就会充当一个“守门员”的角色：它会检查新下发的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props.text</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是否和之前的值一致，如果一致，那么就没有更新的必要，直接返回“false”将整个 ChildB 的更新生命周期中断掉即可。只有当 props.text 确实发生变化时，它才会“准许” re-render 的发生。</font>

<font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的加持下，当我们再次点击左侧按钮，试图修改 ChildA 的渲染内容时，控制台的输出就会变成下图这样：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874111882-058a3ecc-5046-41a2-9f07-dc66f0397481.png)

<font style="color:rgb(44, 62, 80);">我们看到，控制台中现在只有 ChildA 的 re-render 提示。ChildB “稳如泰山”，成功躲开了一次多余的渲</font>染。

使用 <font style="background-color:rgb(249, 242, 244);">shouldComponentUpdate</font> 来调停不必要的更新，避免无意义的 re-render 发生，这是 React 组件中最基本的性能优化手段，也是最重要的手段。许多看似高级的玩法，都是基于 <font style="background-color:rgb(249, 242, 244);">shouldComponentUpdate</font> 衍生出来的。我们接下来要讲的 <font style="background-color:rgb(249, 242, 244);">PureComponent</font>，就是这类玩法中的典型。

## [](https://www.123fe.net/principle-docs/react/24-%E5%A6%82%E4%BD%95%E6%89%93%E9%80%A0%E9%AB%98%E6%80%A7%E8%83%BD%E7%9A%84%20React%20%E5%BA%94%E7%94%A8.html#%E8%BF%9B%E9%98%B6%E7%8E%A9%E6%B3%95-purecomponent-immutable-js)进阶玩法：PureComponent  + Immutable.js
**<font style="color:rgb(44, 62, 80);">PureComponent：提前帮你安排好更新判定逻辑</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">虽然一定程度上帮我们解决了性能方面的问题，但每次避免 re-render，都要手动实现一次</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，未免太累了。作为一个不喜欢重复劳动的前端开发者来说，在写了不计其数个</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">逻辑之后，难免会怀疑人生</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 15.3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">很明显听到了开发者的声音，它新增了一个叫</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类，恰到好处地解决了“程序员写</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">写出腱鞘炎”这个问题。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent 与 Component 的区别点</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，就在于它内置了对</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的实现：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">将会在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中对组件更新前后的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props 和 state 进行浅比较</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，并根据浅比较的结果，决定是否需要继续更新流程。</font>

```plain
import React from "react";
export default class ChildB extends React.PureComponent {
  render() {
    console.log("ChildB 的render方法执行了");
    return (
      <div className="childB">
        子组件B的内容：
        {this.props.text}
      </div>
    );
  }
}
```

<font style="color:rgb(44, 62, 80);">此时再去修改 ChildA 中的文本，我们会发现 ChildB 同样不受影响</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在值类型数据这种场景下，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以说是战无不胜。但是如果数据类型为引用类型，那么这种基于浅比较的判断逻辑就会带来这样两个风险：</font>

+ <font style="color:rgb(44, 62, 80);">若数据内容没变，但是引用变了，那么浅比较仍然会认为“数据发生了变化”，进而触发一次不必要的更新，导致过度渲染；</font>
+ <font style="color:rgb(44, 62, 80);">若数据内容变了，但是引用没变，那么浅比较则会认为“数据没有发生变化”，进而阻断一次更新，导致不渲染。</font>

<font style="color:rgb(44, 62, 80);">怎么办呢？</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Immutable.js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来帮忙！</font>

**<font style="color:rgb(44, 62, 80);">Immutable：“不可变值”让“变化”无处遁形</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent 浅比较带来的问题，本质上是对“变化”的判断不够精准导致的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。那有没有一种办法，能够让引用的变化和内容的变化之间，建立一种必然的联系呢？</font>

<font style="color:rgb(44, 62, 80);">这就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Immutable.js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所做的事情。</font>

<font style="color:rgb(44, 62, 80);">Immutable 直译过来是“不可变的”，顾名思义，Immutable.js 是对“不可变值”这一思想的贯彻实践。它在 2014 年被 Facebook 团队推出，Facebook 给它的定位是“实现持久性数据结构的库”。所谓“持久性数据”，指的是这个数据只要被创建出来了，就不能被更改。我们对当前数据的任何修改动作，都会导致一个新的对象的返回。这就将数据内容的变化和数据的引用严格地关联了起来，使得“变化”无处遁形。</font>

<font style="color:rgb(44, 62, 80);">这里我用一个简单的例子，来演示一下 Immutable.js 的效果。请看下面代码：</font>

```javascript
// 引入 immutable 库里的 Map 对象，它用于创建对象
import { Map } from 'immutable'
// 初始化一个对象 baseMap
const baseMap = Map({
  name: 'po',
  career: 'fe',
  age: 99
})
// 使用 immutable 暴露的 Api 来修改 baseMap 的内容
const changedMap = baseMap.set({
  age: 100
})
// 我们会发现修改 baseMap 后将会返回一个新的对象，这个对象的引用和 baseMap 是不同的
console.log('baseMap === changedMap', baseMap === changedMap)
```

<font style="color:rgb(44, 62, 80);">由此可见，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComonent 和 Immutable.js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">真是一对好基友！在实际的开发中，我们也确实经常左手 PureComonent，右手 Immutable.js，研发质量大大地提升呀！</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">值得注意的是，由于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Immutable.js</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">存在一定的学习成本，并不是所有场景下都可以作为最优解被团队采纳。因此，一些团队也会基于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComonent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Immutable.js</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">去打造将两者结合的公共类，通过改写 setState 来提升研发体验，这也是不错的思路。</font>

## [](https://www.123fe.net/principle-docs/react/24-%E5%A6%82%E4%BD%95%E6%89%93%E9%80%A0%E9%AB%98%E6%80%A7%E8%83%BD%E7%9A%84%20React%20%E5%BA%94%E7%94%A8.html#%E5%87%BD%E6%95%B0%E7%BB%84%E4%BB%B6%E7%9A%84%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96-react-memo-%E5%92%8C-usememo)函数组件的性能优化：React.memo 和 useMemo
<font style="background-color:rgb(255, 249, 249);">以上咱们讨论的都是类组件的优化思路。那么在函数组件中，有没有什么通用的手段可以阻</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">止“过度 re-render”的发生呢？接下来我们就一起认识一下“函数版”的 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate/Purecomponent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> —— </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

**<font style="color:rgb(44, 62, 80);">React.memo：“函数版”shouldComponentUpdate/PureComponent</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是 React 导出的一个顶层函数，它本质上是一个高阶组件，负责对函数组件进行包装。基本的调用姿势如下面代码所示：</font>

```javascript
import React from "react";
// 定义一个函数组件
function FunctionDemo(props) {
  return xxx
}
// areEqual 函数是 memo 的第二个入参，我们之前放在 shouldComponentUpdate 里面的逻辑就可以转移至此处
function areEqual(prevProps, nextProps) {
  /*
  return true if passing nextProps to render would return
  the same result as passing prevProps to render,
  otherwise return false
  */
}
// 使用 React.memo 来包装函数组件
export default React.memo(FunctionDemo, areEqual);
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会帮我们“记住”函数组件的渲染结果，在组件前后两次</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对比结果一致的情况下，它会直接复用最近一次渲染的结果。如果我们的组件在相同的 props 下会渲染相同的结果，那么使用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来包装它将是个不错的选择。</font>

<font style="color:rgb(44, 62, 80);">从示例中我们可以看出，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">接收两个参数，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">第一个参数是我们需要渲染的目标组件</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">第二个参数 areEqual 则用来承接 props 的对比逻辑</font><font style="color:rgb(44, 62, 80);">。之前我们在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里面做的事情，现在就可以放在 areEqual 里来做。</font>

<font style="color:rgb(44, 62, 80);">比如开篇 Demo 中的 ChildB 组件，就完全可以用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Function Component + React.memo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来改造。改造后的 ChildB 代码如下：</font>

```javascript
import React from "react";

// 将 ChildB 改写为 function 组件
function ChildB(props) {
  console.log("ChildB 的render 逻辑执行了");
  return (
    <div className="childB">
    子组件B的内容：
  {props.text}
  </div>
  );
}

// areEqual 用于对比 props 的变化
function areEqual(prevProps, nextProps) {
  if(prevProps.text === nextProps.text) {
    return true
  }
  return false
}

// 使用 React.memo 来包装 ChildB
export default React.memo(ChildB, areEqual);
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">改造后的组件在效果上就等价于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">加持后的类组件</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildB</font>

<font style="color:rgb(44, 62, 80);">这里的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">areEqual 函数是一个可选参数</font><font style="color:rgb(44, 62, 80);">，当我们不传入</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">areEqual</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">时，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">也可以工作，此时它的作用就类似于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent——React.memo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会自动为你的组件执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props 的浅比较逻辑</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不同的是，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">只负责对比</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，而不会去感知组件内部状态（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）的变化</font>

**<font style="color:rgb(44, 62, 80);">useMemo：更加“精细”的 memo</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">通过上面的分析我们知道，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以实现类似于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">或者</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的效果，对组件级别的 re-render 进行管控。但是有时候，我们希望复用的并不是整个组件，而是组件中的某一个或几个部分。这种更加“精细化”的管控，就需要</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来帮忙了。</font>

**<font style="color:rgb(44, 62, 80);">简而言之</font>**<font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo 控制是否需要重渲染一个组件</font><font style="color:rgb(44, 62, 80);">，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo 控制的则是是否需要重复执行某一段逻辑</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的使用方式如下面代码所示：</font>

```plain
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们可以把目标逻辑作为第一个参数传入，把逻辑的依赖项数组作为第二个参数传入。这样只有当依赖项数组中的某个依赖发生变化时，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">才会重新执行第一个入参中的目标逻辑</font>

<font style="color:rgb(44, 62, 80);">这里我仍然以开篇的示例为例，现在我尝试向 ChildB 中传入两个属性：text 和 count，它们分别是一段文本和一个数字。当我点击右边的按钮时，只有 count 数字会发生变化。改造后的 App 组件代码如下：</font>

```javascript
class App extends React.Component {
  state = {
    textA: '我是A的文本',
    stateB: {
      text: '我是B的文本',
      count: 10
    }
  }
  changeA = () => {
    this.setState({
      textA: 'A的文本被修改了'
    })
  }
  changeB = () => {
    this.setState({
      stateB: {
        ...this.state.stateB,
        count: 100
      }
    })
  }
  render() {
    return (
      <div className="App">
      <div className="container">
      <button onClick={this.changeA}>点击修改A处的文本</button>
      <button onClick={this.changeB}>点击修改B处的文本</button>
      <ul>
      <li>
      <ChildA text={this.state.textA}/>
  </li>
  <li>
  <ChildB {...this.state.stateB}/>
  </li>
  </ul>
  </div>
  </div>
);
}
}
export default App;
```

<font style="color:rgb(44, 62, 80);">在 ChildB 中，使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来加持</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text 和 count</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">各自的渲染逻辑。改造后的 ChildB 代码如下所示：</font>

```javascript
import React,{ useMemo } from "react";
export default function ChildB({text, count}) {
  console.log("ChildB 的render 逻辑执行了");
  // text 文本的渲染逻辑
  const renderText = (text)=> {
    console.log('renderText 执行了')
    return <p>
      子组件B的文本内容：
    {text}
    </p>
}
// count 数字的渲染逻辑
const renderCount = (count) => {
  console.log('renderCount 执行了')
  return <p>
    子组件B的数字内容：
  {count}
  </p>
}

// 使用 useMemo 加持两段渲染逻辑
const textContent = useMemo(()=>renderText(text),[text])
const countContent = useMemo(()=>renderCount(count),[count])
return (
  <div className="childB">
  {textContent}
{countContent}
</div>
);
}
```

<font style="color:rgb(44, 62, 80);">渲染 App 组件，我们可以看到初次渲染时，renderText 和 renderCount 都执行了，控制台输出如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874112414-f74c75c4-b377-4321-aadd-ef4cadb101ed.png)

<font style="color:rgb(44, 62, 80);">点击右边按钮，对 count 进行修改，修改后的界面会发生如下的变化：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874113533-1ea0859d-1fa3-4096-a94e-71f6b9b5ee4d.png)

<font style="color:rgb(44, 62, 80);">可以看出，由于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">count</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">发生了变化，因此</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">针对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">renderCount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的逻辑进行了重计算。而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">没有发生变化，因此</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">renderText</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的逻辑压根没有执行。</font>

<font style="color:rgb(44, 62, 80);">使用 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);">，我们可以对函数组件的执行逻辑进行更加细粒度的管控（尤其是定向规避掉一些高开销的计算），同时也弥补了 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);"> 无法感知函数内部状态的遗憾，这对我们整体的性能提升是大有裨益的。</font>

