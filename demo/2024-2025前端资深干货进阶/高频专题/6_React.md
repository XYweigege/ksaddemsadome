### <font style="color:rgb(44, 62, 80);">JSX本质</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">即</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);">函数，返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font>
+ <font style="color:rgb(44, 62, 80);">第一个参数，可能是组件，也可能是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html tag</font>
+ <font style="color:rgb(44, 62, 80);">组件名，首字母必须是大写（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">规定）</font>

```javascript
// React.createElement写法
React.createElement('tag', null, [child1,child2])
React.createElement('tag', props, child1,child2,child3)
React.createElement(Comp, props, child1,child2,'文本节点')
```

```jsx
// jsx基本用法
<div className="container">
  <p>tet</p>
  <img src={imgSrc} />
</div>

// 编译后 https://babeljs.io/repl
React.createElement(
  "div",
  {
    className: "container"
  },
  React.createElement("p", null, "tet"),
  React.createElement("img", {
    src: imgSrc
  })
);
```

```jsx
// jsx style
const styleData = {fontSize:'20px',color:'f00'}
const styleElem = <p style={styleData}>设置style</p>

// 编译后
const styleData = {
  fontSize: "20px",
  color: "f00"
};
const styleElem = React.createElement(
  "p",
  {
    style: styleData
  },
  "\u8BBE\u7F6Estyle"
);
```

```jsx
// jsx加载组件
const app = <div>
    <Input submitTitle={onSubmitTitle} />
    <List list={list} />
</div>

// 编译后
const app = React.createElement(
  "div",
  null,
  React.createElement(Input, {
    submitTitle: onSubmitTitle
  }),
  React.createElement(List, {
    list: list
  })
);
```

```jsx
// jsx事件
const eventList = <p onClick={this.clickHandler}>text</p>

// 编译后
const eventList = React.createElement(
  "p",
  {
    onClick: (void 0).clickHandler
  },
  "text"
);
```

```javascript
// jsx列表
const listElem = <ul>
{
  this.state.list.map((item,index)=>{
    return <li key={index}>index:{index},title:{item.title}</li>
})
}
</ul>

// 编译后

const listElem = React.createElement(
  "ul",
  null,
  (void 0).state.list.map((item, index) => {
    return React.createElement(
      "li",
      {
        key: index
      },
      "index:",
      index,
      ",title:",
      item.title
    );
  })
);
```

### [](https://www.123fe.net/docs/base/high-frequency.html#react%E5%90%88%E6%88%90%E4%BA%8B%E4%BB%B6%E6%9C%BA%E5%88%B6)<font style="color:rgb(44, 62, 80);">React合成事件机制</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React16</font><font style="color:rgb(44, 62, 80);">事件绑定到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document</font><font style="color:rgb(44, 62, 80);">上</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React17</font><font style="color:rgb(44, 62, 80);">事件绑定到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">root</font><font style="color:rgb(44, 62, 80);">组件上，有利于多个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">版本共存，例如微前端</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event</font><font style="color:rgb(44, 62, 80);">不是原生的，是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SyntheticEvent</font><font style="color:rgb(44, 62, 80);">合成事件对象</font>
+ <font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">不同，和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">事件也不同</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784107141-6c7b1cd1-c689-46b9-a232-c988ee123869.png)

**<font style="color:rgb(44, 62, 80);">合成事件图示</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784110008-8976d2da-6461-4ebd-b6db-411c1a168854.png)

**<font style="color:rgb(44, 62, 80);">为何需要合成事件</font>**

+ <font style="color:rgb(44, 62, 80);">更好的兼容性和跨平台，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react native</font>
+ <font style="color:rgb(44, 62, 80);">挂载到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document</font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">root</font><font style="color:rgb(44, 62, 80);">上，减少内存消耗，避免频繁解绑</font>
+ <font style="color:rgb(44, 62, 80);">方便事件的统一管理（如事务机制）</font>

```javascript
// 获取 event
clickHandler3 = (event) => {
  event.preventDefault() // 阻止默认行为
  event.stopPropagation() // 阻止冒泡
  console.log('target', event.target) // 指向当前元素，即当前元素触发
  console.log('current target', event.currentTarget) // 指向当前元素，假象！！！

  // 注意，event 其实是 React 封装的。可以看 __proto__.constructor 是 SyntheticEvent 组合事件
  console.log('event', event) // 不是原生的 Event ，原生的 MouseEvent
  console.log('event.__proto__.constructor', event.__proto__.constructor)

  // 原生 event 如下。其 __proto__.constructor 是 MouseEvent
  console.log('nativeEvent', event.nativeEvent)
  console.log('nativeEvent target', event.nativeEvent.target)  // 指向当前元素，即当前元素触发
  console.log('nativeEvent current target', event.nativeEvent.currentTarget) // 指向 document ！！！

  // 1. event 是 SyntheticEvent ，模拟出来 DOM 事件所有能力
  // 2. event.nativeEvent 是原生事件对象
  // 3. 所有的事件，都被挂载到 document 上
  // 4. 和 DOM 事件不一样，和 Vue 事件也不一样
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#setstate%E5%92%8Cbatchupdate%E6%9C%BA%E5%88%B6)<font style="color:rgb(44, 62, 80);">setState和batchUpdate机制</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">在react事件、生命周期中是异步的（在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">上下文中是异步）；在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">、自定义</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">事件中是同步的</font>
+ <font style="color:rgb(44, 62, 80);">有时合并（对象形式</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState({})</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">=> 通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.assign</font><font style="color:rgb(44, 62, 80);">形式合并对象），有时不合并（函数形式</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState((prevState,nextState)=>{})</font><font style="color:rgb(44, 62, 80);">）</font>

**<font style="color:rgb(44, 62, 80);">核心要点</font>**

<font style="color:rgb(44, 62, 80);">1.</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">主流程</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">是否是异步还是同步，看是否能命中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchUpdate</font><font style="color:rgb(44, 62, 80);">机制，判断</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isBatchingUpdates</font>
+ <font style="color:rgb(44, 62, 80);">哪些能命中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchUpdate</font><font style="color:rgb(44, 62, 80);">机制</font>
    - <font style="color:rgb(44, 62, 80);">生命周期</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">中注册的事件和它调用的函数</font>
    - <font style="color:rgb(44, 62, 80);">总之在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">的上下文中</font>
+ <font style="color:rgb(44, 62, 80);">哪些不能命中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchUpdate</font><font style="color:rgb(44, 62, 80);">机制</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);">等</font>
    - <font style="color:rgb(44, 62, 80);">自定义</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">事件</font>
    - <font style="color:rgb(44, 62, 80);">总之不在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">的上下文中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">管不到的</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784107315-50f56a2e-7262-416c-8727-8672a0cd5953.png)

1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchUpdate</font><font style="color:rgb(44, 62, 80);">机制</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784109703-eb498d46-6ee7-4eee-9f4e-3d35d7c13e59.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784107076-57bc267c-7372-44ad-b413-bca24a9abcbe.png)

```javascript
// setState batchUpdate原理模拟
let isBatchingUpdate = true;

let queue = [];
let state = {number:0};
function setState(newSate){
  //state={...state,...newSate}
  // setState异步更新
  if(isBatchingUpdate){
    queue.push(newSate);
  }else{
    // setState同步更新
    state={...state,...newSate}
  }   
}

// react事件是合成事件，在合成事件中isBatchingUpdate需要设置为true
// 模拟react中事件点击
function handleClick(){
  isBatchingUpdate=true; // 批量更新标志

  /**我们自己逻辑开始 */
  setState({number:state.number+1});
  setState({number:state.number+1});
  console.log(state); // 0
  setState({number:state.number+1});
  console.log(state); // 0
  /**我们自己逻辑结束 */

  state= queue.reduce((newState,action)=>{
    return {...newState,...action}
  },state); 

  isBatchingUpdate=false; // 执行结束设置false
}
handleClick();
console.log(state); // 1
```

1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transaction</font><font style="color:rgb(44, 62, 80);">事务机制</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784121179-cfb617bd-40d8-4ce9-93ea-944a56a859b3.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784123036-e900b82f-1a99-4990-bdc3-33c315208b9d.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784124410-7fcf8da9-0b6d-4496-9421-0c7e6b31c0af.png)

```javascript
// setState现象演示

import React from 'react'

// 函数组件（后面会讲），默认没有 state
class StateDemo extends React.Component {
  constructor(props) {
    super(props)

    // 第一，state 要在构造函数中定义
    this.state = {
      count: 0
    }
  }
  render() {
    return <div>
      <p>{this.state.count}</p>
  <button onClick={this.increase}>累加</button>
  </div>
}
increase = () => {
  // // 第二，不要直接修改 state ，使用不可变值 ----------------------------
  // // this.state.count++ // 错误
  // this.setState({
  //     count: this.state.count + 1 // SCU
  // })
  // 操作数组、对象的的常用形式

  // 第三，setState 可能是异步更新（有可能是同步更新） ----------------------------

  // this.setState({
  //     count: this.state.count + 1
  // }, () => {
  //     // 联想 Vue $nextTick - DOM
  //     console.log('count by callback', this.state.count) // 回调函数中可以拿到最新的 state
  // })
  // console.log('count', this.state.count) // 异步的，拿不到最新值

  // // setTimeout 中 setState 是同步的
  // setTimeout(() => {
  //     this.setState({
  //         count: this.state.count + 1
  //     })
  //     console.log('count in setTimeout', this.state.count)
  // }, 0)

  // 自己定义的 DOM 事件，setState 是同步的。再 componentDidMount 中

  // 第四，state 异步更新的话，更新前会被合并 ----------------------------

  // 传入对象，会被合并（类似 Object.assign ）。执行结果只一次 +1
  // this.setState({
  //     count: this.state.count + 1
  // })
  // this.setState({
  //     count: this.state.count + 1
  // })
  // this.setState({
  //     count: this.state.count + 1
  // })

  // 传入函数，不会被合并。执行结果是 +3
  this.setState((prevState, props) => {
    return {
      count: prevState.count + 1
    }
  })
  this.setState((prevState, props) => {
    return {
      count: prevState.count + 1
    }
  })
  this.setState((prevState, props) => {
    return {
      count: prevState.count + 1
    }
  })
}
// bodyClickHandler = () => {
//     this.setState({
//         count: this.state.count + 1
//     })
//     console.log('count in body event', this.state.count)
// }
// componentDidMount() {
//     // 自己定义的 DOM 事件，setState 是同步的
//     document.body.addEventListener('click', this.bodyClickHandler)
// }
// componentWillUnmount() {
//     // 及时销毁自定义 DOM 事件
//     document.body.removeEventListener('click', this.bodyClickHandler)
//     // clearTimeout
// }
}

export default StateDemo

  // -------------------------- 我是分割线 -----------------------------

  // 不可变值（函数式编程，纯函数） - 数组
  // const list5Copy = this.state.list5.slice()
  // list5Copy.splice(2, 0, 'a') // 中间插入/删除
  // this.setState({
  //     list1: this.state.list1.concat(100), // 追加
  //     list2: [...this.state.list2, 100], // 追加
//     list3: this.state.list3.slice(0, 3), // 截取
//     list4: this.state.list4.filter(item => item > 100), // 筛选
//     list5: list5Copy // 其他操作
// })
// // 注意，不能直接对 this.state.list 进行 push pop splice 等，这样违反不可变值

// 不可变值 - 对象
// this.setState({
//     obj1: Object.assign({}, this.state.obj1, {a: 100}),
//     obj2: {...this.state.obj2, a: 100}
// })
// // 注意，不能直接对 this.state.obj 进行属性设置，这样违反不可变值
```

```javascript
// setState笔试题考察 下面这道题输出什么
class Example extends React.Component {
  constructor() {
    super()
    this.state = {
      val: 0
    }
  }
  // componentDidMount中isBatchingUpdate=true setState批量更新
  componentDidMount() {
    // setState传入对象会合并，后面覆盖前面的Object.assign({})
    this.setState({ val: this.state.val + 1 }) // 添加到queue队列中，等待处理 
    console.log(this.state.val)
    // 第 1 次 log
    this.setState({ val: this.state.val + 1 }) // 添加到queue队列中，等待处理
    console.log(this.state.val)
    // 第 2 次 log
    setTimeout(() => {
      // 到这里this.state.val结果等于1了
      // 在原生事件和setTimeout中（isBatchingUpdate=false），setState同步更新，可以马上获取更新后的值
      this.setState({ val: this.state.val + 1 }) // 同步更新
      console.log(this.state.val)
      // 第 3 次 log
      this.setState({ val: this.state.val + 1 }) // 同步更新
      console.log(this.state.val)
      // 第 4 次 log
    }, 0)
  }
  render() {
    return null
  }
}

// 答案：0, 0, 2, 3
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E6%A0%B9%E6%8D%AEjsx%E5%86%99%E5%87%BAvnode%E5%92%8Crender%E5%87%BD%E6%95%B0)<font style="color:rgb(44, 62, 80);">根据jsx写出vnode和render函数</font>
```html
<!-- jsx -->
<div className="container">
  <p onClick={onClick} data-name="p1">
    hello <b>{name}</b>
  </p>
  <img src={imgSrc} />
  <MyComponent title={title}></MyComponent>
</div>
```

**<font style="color:rgb(44, 62, 80);">注意</font>**

+ <font style="color:rgb(44, 62, 80);">注意</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);">中的常量和变量</font>
+ <font style="color:rgb(44, 62, 80);">注意</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML tag</font><font style="color:rgb(44, 62, 80);">和自定义组件</font>

```javascript
const vnode = {
  tag: 'div',
  props: {
    className: 'container'
  },
  children: [
    // <p>
    {
      tag: 'p',
      props: {
        dataset: {
          name: 'p1'
        },
        on: {
          click: onClick // 变量
        }
      },
      children: [
        'hello',
        {
          tag: 'b',
          props: {},
          children: [name] // name变量
        }
      ]
    },
    // <img />
    {
      tag: 'img',
      props: {
        src: imgSrc // 变量
      },
      children: [/**无子节点**/]
    },
    // <MyComponent>
    {
      tag: MyComponent, // 变量
      props: {
        title: title, // 变量
      },
      children: [/**无子节点**/]
    }
  ]
}
```

```javascript
// render函数
function render() {
  // h(tag, props, children)
  return h('div', {
    props: {
      className: 'container'
    }
  }, [

    // p
    h('p', {
      dataset: {
        name: 'p1'
      },
      on: {
        click: onClick
      }
    }, [
      'hello',
      h('b', {}, [name])
    ])

    // img
    h('img', {
      props: {
        src: imgSrc
      }
    }, [/**无子节点**/])

    // MyComponent
    h(MyComponent, {
      title: title
    }, [/**无子节点**/])
  ]
          )
}
```

**<font style="color:rgb(44, 62, 80);">在react中jsx编译后</font>**

```javascript
// 使用https://babeljs.io/repl编译后效果

React.createElement(
  "div",
  {
    className: "container"
  },
  React.createElement(
    "p",
    {
      onClick: onClick,
      "data-name": "p1"
    },
    "hello ",
    React.createElement("b", null, name)
  ),
  React.createElement("img", {
    src: imgSrc
  }),
  React.createElement(MyComponent, {
    title: title
  })
);
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E8%99%9A%E6%8B%9Fdom-vdom-%E7%9C%9F%E7%9A%84%E5%BE%88%E5%BF%AB%E5%90%97)<font style="color:rgb(44, 62, 80);">虚拟DOM（vdom）真的很快吗</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">virutal DOM</font><font style="color:rgb(44, 62, 80);">，虚拟</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font>
+ <font style="color:rgb(44, 62, 80);">用JS对象模拟</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">节点数据</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);">并不快，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">直接操作</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">才是最快的</font>
    - <font style="color:rgb(44, 62, 80);">以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(44, 62, 80);">为例，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">变化 =></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">=> 更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">肯定是比不过直接操作</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">节点快的</font>
+ <font style="color:rgb(44, 62, 80);">但是"数据驱动视图"要有合适的技术方案，不能全部</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">重建</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">就是目前最合适的技术方案（并不是因为它快，而是合适）</font>
+ <font style="color:rgb(44, 62, 80);">在大型系统中，全部更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">的成本太高，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);">把更新范围减少到最小</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">并不是所有的框架都在用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">svelte</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就不用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784124716-691e63be-b73a-4f96-b4bc-0d85a486d590.png)

### [](https://www.123fe.net/docs/base/high-frequency.html#react%E7%BB%84%E4%BB%B6%E6%B8%B2%E6%9F%93%E8%BF%87%E7%A8%8B)<font style="color:rgb(44, 62, 80);">react组件渲染过程</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);">如何渲染为页面</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">之后如何更新页面</font>
+ <font style="color:rgb(44, 62, 80);">面试考察全流程</font>

**<font style="color:rgb(44, 62, 80);">1.组件渲染过程</font>**

+ <font style="color:rgb(44, 62, 80);">分析</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">变化</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render()</font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch(elem, vnode)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">渲染到页面上（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">并一定用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">渲染过程</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState(newState)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">=></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newState</font><font style="color:rgb(44, 62, 80);">存入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pending</font><font style="color:rgb(44, 62, 80);">队列，判断是否处于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">batchUpdate</font><font style="color:rgb(44, 62, 80);">状态，保存组件于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dirtyComponents</font><font style="color:rgb(44, 62, 80);">中（可能有子组件）</font><font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784128912-fb25e660-6996-4b88-8bbd-383e1d4c383c.png)
    - <font style="color:rgb(44, 62, 80);">遍历所有的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dirtyComponents</font><font style="color:rgb(44, 62, 80);">调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateComponent</font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newVnode</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch(vnode,newVnode)</font>

**<font style="color:rgb(44, 62, 80);">2.组件更新过程</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);">更新被分为两个阶段</font>
    - **<font style="color:rgb(44, 62, 80);">reconciliation阶段</font>**<font style="color:rgb(44, 62, 80);">：执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">算法，纯</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">计算</font>
    - **<font style="color:rgb(44, 62, 80);">commit阶段</font>**<font style="color:rgb(44, 62, 80);">：将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">结果渲染到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">中</font>
+ <font style="color:rgb(44, 62, 80);">如果不拆分，可能有性能问题</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">是单线程的，且和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">渲染共用一个线程</font>
    - <font style="color:rgb(44, 62, 80);">当组件足够复杂，组件更新时计算和渲染都压力大</font>
    - <font style="color:rgb(44, 62, 80);">同时再有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">操作需求（动画、鼠标拖拽等）将卡顿</font>
+ **<font style="color:rgb(44, 62, 80);">解决方案Fiber</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reconciliation</font><font style="color:rgb(44, 62, 80);">阶段拆分为多个子任务</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">需要渲染时更新，空闲时恢复在执行计算</font>
    - <font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.requestIdleCallback</font><font style="color:rgb(44, 62, 80);">来判断浏览器是否空闲</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#react-setstate%E7%BB%8F%E5%85%B8%E9%9D%A2%E8%AF%95%E9%A2%98)<font style="color:rgb(44, 62, 80);">React setState经典面试题</font>
```javascript
// setState笔试题考察 下面这道题输出什么
class Example extends React.Component {
  constructor() {
    super()
    this.state = {
      val: 0
    }
  }
  componentDidMount() {
    this.setState({ val: this.state.val + 1 })
    console.log(this.state.val)
    // 第 1 次 log
    this.setState({ val: this.state.val + 1 })
    console.log(this.state.val)
    // 第 2 次 log
    setTimeout(() => {
      this.setState({ val: this.state.val + 1 }) 
      console.log(this.state.val)
      // 第 3 次 log
      this.setState({ val: this.state.val + 1 })
      console.log(this.state.val)
      // 第 4 次 log
    }, 0)
  }
  render() {
    return null
  }
}
```

```bash
// 答案
0
0
2
3
```

+ **<font style="color:rgb(44, 62, 80);">关于setState的两个考点</font>**
    - <font style="color:rgb(44, 62, 80);">同步或异步</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">合并或不合并</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">传入函数不会合并覆盖</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">传入对象会合并覆盖</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.assign({})</font>
+ <font style="color:rgb(44, 62, 80);">分析</font>
    - **<font style="color:rgb(44, 62, 80);">默认情况</font>**
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">默认异步更新</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">默认合并后更新（后面的覆盖前面的，多次重复执行不会累加）</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">在合成事件和生命周期钩子中，是异步更新的</font>
    - **<font style="color:rgb(44, 62, 80);">react同步更新，不在react上下文中触发</font>**
        * <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">原生事件</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise.then</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ajax</font><font style="color:rgb(44, 62, 80);">回调中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">是同步的，可以马上获取更新后的值</font>
            + <font style="color:rgb(44, 62, 80);">原生事件如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document.getElementById('test').addEventListener('click',()=>{this.setState({count:this.state.count + 1}})</font>
        * <font style="color:rgb(44, 62, 80);">原因: 原生事件是浏览器本身的实现，与事务流无关，自然是同步；而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">是放置于定时器线程中延后执行，此时事务流已结束，因此也是同步</font>
    - **<font style="color:rgb(44, 62, 80);">注意：在react18中不一样</font>**
        * <font style="color:rgb(44, 62, 80);">上述场景，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react18</font><font style="color:rgb(44, 62, 80);">中可以异步更新（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Auto Batch</font><font style="color:rgb(44, 62, 80);">）</font>
        * <font style="color:rgb(44, 62, 80);">需将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);">替换为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.createRoot</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如需要实时获取结果，在回调函数中获取</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState({count:this.state.count + 1},()=>console.log(this.state.count)})</font>

```javascript
// setState原理模拟
let isBatchingUpdate = true;

let queue = [];
let state = {number:0};
function setState(newSate){
  //state={...state,...newSate}
  // setState异步更新
  if(isBatchingUpdate){
    queue.push(newSate);
  }else{
    // setState同步更新
    state={...state,...newSate}
  }   
}

// react事件是合成事件，在合成事件中isBatchingUpdate需要设置为true
// 模拟react中事件点击
function handleClick(){
  isBatchingUpdate=true; // 批量更新标志

  /**我们自己逻辑开始 */
  setState({number:state.number+1});
  setState({number:state.number+1});
  console.log(state); // 0
  setState({number:state.number+1});
  console.log(state); // 0
  /**我们自己逻辑结束 */

  state= queue.reduce((newState,action)=>{
    return {...newState,...action}
  },state); 
}
handleClick();
console.log(state); // 1
```

```javascript
// setState笔试题考察 下面这道题输出什么
class Example extends React.Component {
  constructor() {
    super()
    this.state = {
      val: 0
    }
  }
  // componentDidMount中isBatchingUpdate=true setState批量更新
  componentDidMount() {
    // setState传入对象会合并，后面覆盖前面的Object.assign({})
    this.setState({ val: this.state.val + 1 }) // 添加到queue队列中，等待处理 
    console.log(this.state.val)
    // 第 1 次 log
    this.setState({ val: this.state.val + 1 }) // 添加到queue队列中，等待处理
    console.log(this.state.val)
    // 第 2 次 log
    setTimeout(() => {
      // 到这里this.state.val结果等于1了
      // 在原生事件和setTimeout中（isBatchingUpdate=false），setState同步更新，可以马上获取更新后的值
      this.setState({ val: this.state.val + 1 }) // 同步更新
      console.log(this.state.val)
      // 第 3 次 log
      this.setState({ val: this.state.val + 1 }) // 同步更新
      console.log(this.state.val)
      // 第 4 次 log
    }, 0)
  }
  render() {
    return null
  }
}

// 答案：0, 0, 2, 3
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 18</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之前，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的合成事件中是合并更新的，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的原生事件中是同步按序更新的。例如</font>

```javascript
handleClick = () => {
  this.setState({ age: this.state.age + 1 });
  console.log(this.state.age); // 0
  this.setState({ age: this.state.age + 1 });
  console.log(this.state.age); // 0
  this.setState({ age: this.state.age + 1 });
  console.log(this.state.age); // 0
  setTimeout(() => {
    this.setState({ age: this.state.age + 1 });
    console.log(this.state.age); // 2
    this.setState({ age: this.state.age + 1 });
    console.log(this.state.age); // 3
  });
};
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">而在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React 18</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中，不论是在合成事件中，还是在宏任务中，都是会合并更新</font>

```javascript
function handleClick() {
  setState({ age: state.age + 1 }, onePriority);
  console.log(state.age);// 0
  setState({ age: state.age + 1 }, onePriority);
  console.log(state.age); // 0
  setTimeout(() => {
    setState({ age: state.age + 1 }, towPriority);
    console.log(state.age); // 1
    setState({ age: state.age + 1 }, towPriority);
    console.log(state.age); // 1
  });
}
```

```javascript
// 拓展：setState传入函数不会合并
class Example extends React.Component {
  constructor() {
    super()
    this.state = {
      val: 0
    }
  }
  componentDidMount() {
    this.setState((prevState,props)=>{
      return {val: prevState.val + 1}
    })
    console.log(this.state.val) // 0
    // 第 1 次 log
    this.setState((prevState,props)=>{ // 传入函数，不会合并覆盖前面的
      return {val: prevState.val + 1}
    })
    console.log(this.state.val) // 0
    // 第 2 次 log
    setTimeout(() => {
      // setTimeout中setState同步执行
      // 到这里this.state.val结果等于2了
      this.setState({ val: this.state.val + 1 }) 
      console.log(this.state.val) // 3
      // 第 3 次 log
      this.setState({ val: this.state.val + 1 })
      console.log(this.state.val) // 4
      // 第 4 次 log
    }, 0)
  }
  render() {
    return null
  }
}
// 答案：0 0 3 4
```

```javascript
// react hooks中打印

function useStateDemo() {
  const [value, setValue] = useState(100)

  function clickHandler() {
    // 1.传入常量，state会合并
    setValue(value + 1)
    setValue(value + 1)
    console.log(1, value) // 100
    // 2.传入函数，state不会合并
    setValue(value=>value + 1)
    setValue(value=>value + 1)
    console.log(2, value) // 100

    // 3.setTimeout中，React18也开始合并state（之前版本会同步更新、不合并）
    setTimeout(()=>{
      setValue(value + 1)
      setValue(value + 1)
      console.log(3, value) // 100
      setValue(value + 1)
    })

    // 4.同理 setTimeout中，传入函数不合并
    setTimeout(()=>{
      setValue(value => value + 1)
      setValue(value => value + 1)
      console.log(4, value) // 100
    })
  }
  return (
    <button onClick={clickHandler}>点击 {value}</button>
  )
}
```

**<font style="color:rgb(44, 62, 80);">连环问：setState是宏任务还是微任务</font>**

+ **<font style="color:rgb(44, 62, 80);">setState本质是同步的</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">是同步的，不过让</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">做成异步更新的样子而已</font>
        * <font style="color:rgb(44, 62, 80);">如果</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">是微任务，就不应该在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise.then</font><font style="color:rgb(44, 62, 80);">微任务之前打印出来（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise then</font><font style="color:rgb(44, 62, 80);">微任务先注册）</font>
    - <font style="color:rgb(44, 62, 80);">因为要考虑性能，多次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">修改，只进行一次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">渲染</font>
    - <font style="color:rgb(44, 62, 80);">日常所说的“异步”是不严谨的，但沟通成本低</font>
+ **<font style="color:rgb(44, 62, 80);">总结</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">是同步执行，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">都是同步更新（只是我们日常把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">当异步来处理）</font>
    - <font style="color:rgb(44, 62, 80);">在微任务</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise.then</font><font style="color:rgb(44, 62, 80);">之前，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">已经计算完了</font>
    - <font style="color:rgb(44, 62, 80);">同步，不是微任务或宏任务</font>

```javascript
import React from 'react'

class Example extends React.Component {
  constructor() {
    super()
    this.state = {
      val: 0
    }
  }

  clickHandler = () => {
    // react事件中 setState异步执行
    console.log('--- start ---')

    Promise.resolve().then(() => console.log('promise then') /* callback */)

    // “异步”
    this.setState(
      { val: this.state.val + 1 },
      () => { console.log('state callback...', this.state) } // callback
    )

    console.log('--- end ---')

    // 结果: 
    // start 
    // end
    // state callback {val:1} 
    // promise then 

    // 疑问？
    // promise then微任务先注册的，按理应该先打印promise then再到state callback
    // 因为：setState本质是同步的，不过让react做成异步更新的样子而已
    // 因为要考虑性能，多次state修改，只进行一次DOM渲染
  }

  componentDidMount() {
    setTimeout(() => {
      // setTimeout中setState是同步更新
      console.log('--- start ---')

      Promise.resolve().then(() => console.log('promise then'))

      this.setState(
        { val: this.state.val + 1 }
      )
      console.log('state...', this.state)

      console.log('--- end ---')
    })

    // 结果: 
    // start 
    // state {val:1} 
    // end
    // promise then 
  }

  render() {
    return <p id="p1" onClick={this.clickHandler}>
      setState demo: {this.state.val}
    </p>
  }
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#react-useeffect%E9%97%AD%E5%8C%85%E9%99%B7%E9%98%B1%E9%97%AE%E9%A2%98)<font style="color:rgb(44, 62, 80);">React useEffect闭包陷阱问题</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">问：按钮点击三次后，定时器输出什么？</font>

```javascript
function useEffectDemo() {
  const [value,setValue] = useState(0)

  useEffect(()=>{
    setInterval(()=>{
      console.log(value)
    },1000)
  }, [])

  const clickHandler = () => {
    setValue(value + 1)
  }

  return (
    <div>
    value: {value} <button onClick={clickHandler}>点击</button>
    </div>
  )
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">答案一直是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">闭包陷阱问题，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">依赖是空的，只会执行一次。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就只会获取它之前的变量。而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">有个特点，每次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">变化都会重新执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffectDemo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个函数。点击了三次函数会执行三次，三次过程中每个函数中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">都不一样，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">获取的永远是第一个函数里面的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font>

```javascript
// 追问：怎么才能打印出3？

function useEffectDemo() {
  const [value,setValue] = useState(0)

  useEffect(()=>{
    const timer = setInterval(()=>{
      console.log(value) // 3
    },1000)
    return ()=>{
      clearInterval(timer) // value变化会导致useEffectDemo函数多次执行，多次执行需要清除上一次的定时器，否则多次注册定时器
    }
  }, [value]) // 这里增加依赖项，每次依赖变化都会重新执行

  const clickHandler = () => {
    setValue(value + 1)
  }

  return (
    <div>
    value: {value} <button onClick={clickHandler}>点击</button>
    </div>
  )
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#vue-react-diff-%E7%AE%97%E6%B3%95%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">Vue React diff 算法有什么区别</font>
**<font style="color:rgb(44, 62, 80);">diff 算法</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue React diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不是对比文字，而是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树，即</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tree diff</font>
+ <font style="color:rgb(44, 62, 80);">传统的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tree diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">算法复杂度是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">O(n^3)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，算法不可用。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784129834-85ab7f75-8cb5-4c6b-8df6-449144110695.png)

**<font style="color:rgb(44, 62, 80);">优化</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">都是用于网页开发，基于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">结构，对</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">算法都进行了优化（或者简化）</font>

+ <font style="color:rgb(44, 62, 80);">只在同一层级比较，不跨层级（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">结构的变化，很少有跨层级移动）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不同则直接删掉重建，不去对比内部细节（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">结构变化，很少有只改外层，不改内层）</font>
+ <font style="color:rgb(44, 62, 80);">同一个节点下的子节点，通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">区分</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">最终把时间复杂度降低到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">O(n)</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，生产环境下可用。这一点</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue React</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">都是相同的。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784130772-b4041878-28ad-4eef-9cf0-0121aef968b6.png)

**<font style="color:rgb(44, 62, 80);">React diff 特点 - 仅向右移动</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">比较子节点时，仅向右移动，不向左移动。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784133064-6e371cb3-8495-47d2-9232-da02b2aa6b70.png)

**<font style="color:rgb(44, 62, 80);">Vue2 diff 特点 - 双端比较</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784133247-9fa7847b-3b44-43aa-8207-2ccccf4b72d6.png)

<font style="color:rgb(44, 62, 80);">定义四个指针，分别比较</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">oldStartNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newStartNode</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">oldStartNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newEndNode</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">oldEndNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newStartNode</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">oldEndNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newEndNode</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">然后指针继续向中间移动，直到指针汇合</font>

**<font style="color:rgb(44, 62, 80);">Vue3 diff 特点 - 最长递增子序列</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">例如数组</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[3，5，7，1，2，8]</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的最长递增子序列就是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[3，5，7，8 ]</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。这是一个专门的算法。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784133853-71d81d88-07e5-481a-9e3c-ba06e44c24ba.png)

**<font style="color:rgb(44, 62, 80);">算法步骤</font>**

+ <font style="color:rgb(44, 62, 80);">通过“前-前”比较找到开始的不变节点</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[A, B]</font>
+ <font style="color:rgb(44, 62, 80);">通过“后-后”比较找到末尾的不变节点</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[G]</font>
+ <font style="color:rgb(44, 62, 80);">剩余的有变化的节点</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[F, C, D, E, H]</font>
    - <font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newIndexToOldIndexMap</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">拿到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">oldChildren</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">index</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[5, 2, 3, 4, -1]</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">-1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">表示之前没有，要新增）</font>
    - <font style="color:rgb(44, 62, 80);">计算</font>**<font style="color:rgb(44, 62, 80);">最长递增子序列</font>**<font style="color:rgb(44, 62, 80);">得到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[2, 3, 4]</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，对应的就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[C, D, E]</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，即这些节点可以不变</font>
    - <font style="color:rgb(44, 62, 80);">剩余的节点，根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">index</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">进行新增、删除</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">该方法旨在尽量减少</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的移动，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">达到最少的DOM操作</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

**<font style="color:rgb(44, 62, 80);">总结</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">特点 - 仅向右移动</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue2 diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">特点 -</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateChildren</font><font style="color:rgb(44, 62, 80);">双端比较</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue3 diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">特点 -</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateChildren</font><font style="color:rgb(44, 62, 80);">增加了最长递增子序列，更快</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue3</font><font style="color:rgb(44, 62, 80);">增加了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patchFlag</font><font style="color:rgb(44, 62, 80);">、静态提升、函数缓存等</font>

**<font style="color:rgb(44, 62, 80);">连环问：diff 算法中 key 为何如此重要</font>**

<font style="color:rgb(44, 62, 80);">无论在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">还是 React 中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的作用都非常大。以</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为例，是否使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对内部</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">变化影响非常大。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784133272-87c3744f-d9ef-469e-90bc-d7047a1f4699.png)

```html
<ul>
  <li v-for="(index, num) in nums" :key="index">
    {{num}}
  </li>
</ul>
```

```jsx
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
)
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E5%A6%82%E4%BD%95%E7%BB%9F%E4%B8%80%E7%9B%91%E5%90%ACreact%E7%BB%84%E4%BB%B6%E6%8A%A5%E9%94%99)<font style="color:rgb(44, 62, 80);">如何统一监听React组件报错</font>
+ **<font style="color:rgb(44, 62, 80);">ErrorBoundary组件</font>**
    - <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react16</font><font style="color:rgb(44, 62, 80);">版本之后，增加了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ErrorBoundary</font><font style="color:rgb(44, 62, 80);">组件</font>
    - <font style="color:rgb(44, 62, 80);">监听所有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">下级组件</font><font style="color:rgb(44, 62, 80);">报错，可降级展示</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI</font>
    - <font style="color:rgb(44, 62, 80);">只监听组件渲染时报错，不监听</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">事件错误、异步错误</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ErrorBoundary</font><font style="color:rgb(44, 62, 80);">没有办法监听到点击按钮时候的在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">click</font><font style="color:rgb(44, 62, 80);">的时候报错</font>
        * <font style="color:rgb(44, 62, 80);">只能监听组件从一开始渲染到渲染成功这段时间报错，渲染成功后在怎么操作产生的错误就不管了</font>
        * <font style="color:rgb(44, 62, 80);">可用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">try catch</font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onerror</font><font style="color:rgb(44, 62, 80);">（二选一）</font>
    - <font style="color:rgb(44, 62, 80);">只在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">production</font><font style="color:rgb(44, 62, 80);">环境生效(需要打包之后查看效果)，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dev</font><font style="color:rgb(44, 62, 80);">会直接抛出错误</font>
+ **<font style="color:rgb(44, 62, 80);">总结</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ErrorBoundary</font><font style="color:rgb(44, 62, 80);">监听组件渲染报错</font>
    - <font style="color:rgb(44, 62, 80);">事件报错使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">try catch</font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onerror</font>
    - <font style="color:rgb(44, 62, 80);">异步报错使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onerror</font>

```javascript
// ErrorBoundary.js

import React from 'react'

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      error: null // 存储当前的报错信息
    }
  }
  static getDerivedStateFromError(error) {
    // 更新 state 使下一次渲染能够显示降级后的 UI
    console.info('getDerivedStateFromError...', error)
    return { error } // return的信息会等于this.state的信息
  }
  componentDidCatch(error, errorInfo) {
    // 统计上报错误信息
    console.info('componentDidCatch...', error, errorInfo)
  }
  render() {
    if (this.state.error) {
      // 提示错误
      return <h1>报错了</h1>
    }

    // 没有错误，就渲染子组件
    return this.props.children
  }
}
```

```javascript
// index.js 中使用
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import ErrorBoundary from './ErrorBoundary'

ReactDOM.render(
  <React.StrictMode>
  <ErrorBoundary>
  <App />
  </ErrorBoundary>
  </React.StrictMode>,
document.getElementById('root')
);
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E5%9C%A8%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E4%B8%AD-%E4%BD%A0%E5%AF%B9react%E5%81%9A%E8%BF%87%E5%93%AA%E4%BA%9B%E4%BC%98%E5%8C%96)<font style="color:rgb(44, 62, 80);">在实际工作中，你对React做过哪些优化</font>
+ **<font style="color:rgb(44, 62, 80);">修改CSS模拟v-show</font>**

```javascript
// 原始写法
{!flag && <MyComonent style={{display:'none'}} />}
{flag && <MyComonent />}

// 模拟v-show
{<MyComonent style={{display:flag ? 'block' : 'none'}} />}
```

+ **<font style="color:rgb(44, 62, 80);">循环使用key</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">不要用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">index</font>
+ **<font style="color:rgb(44, 62, 80);">使用Flagment或<></>空标签包裹减少多个层级组件的嵌套</font>**
+ **<font style="color:rgb(44, 62, 80);">jsx中不要定义函数</font>**<font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);">会被频繁执行的</font>

```javascript
// bad 
// react中的jsx被频繁执行（state更改）应该避免函数被多次新建
<button onClick={()=>{}}>点击</button>
// goods
function useButton() {
  const handleClick = ()=>{}
  return <button onClick={handleClick}>点击</button>
}
```

+ **<font style="color:rgb(44, 62, 80);">使用shouldComponentUpdate</font>**
    - <font style="color:rgb(44, 62, 80);">判断组件是否需要更新</font>
    - <font style="color:rgb(44, 62, 80);">或者使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.PureComponent</font><font style="color:rgb(44, 62, 80);">比较</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">第一层属性</font>
    - <font style="color:rgb(44, 62, 80);">函数组件使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo(comp, fn)</font><font style="color:rgb(44, 62, 80);">包裹</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function fn(prevProps,nextProps) {// 自己实现对比，像shouldComponentUpdate}</font>
+ **<font style="color:rgb(44, 62, 80);">Hooks缓存数据和函数</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useCallback</font><font style="color:rgb(44, 62, 80);">: 缓存回调函数，避免传入的回调每次都是新的函数实例而导致依赖组件重新渲染，具有性能优化的效果</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);">: 用于缓存传入的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">，避免依赖的组件每次都重新渲染</font>
+ **<font style="color:rgb(44, 62, 80);">使用异步组件</font>**

```plain
import React,{lazy,Suspense} from 'react'
const OtherComp = lazy(/**webpackChunkName:'OtherComp'**/ ()=>import('./otherComp'))

function MyComp(){
  return (
    <Suspense fallback={<div>loading...</div>}>
      <OtherComp />
    </Suspense>
  )
}
```

+ **<font style="color:rgb(44, 62, 80);">路由懒加载</font>**

```plain
import React,{lazy,Suspense} from 'react'
import {BrowserRouter as Router,Route, Switch} from 'react-router-dom'

const Home = lazy(/**webpackChunkName:'h=Home'**/()=>import('./Home'))
const List = lazy(/**webpackChunkName:'List'**/()=>import('./List'))

const App = ()=>(
  <Router>
    <Suspense fallback={<div>loading...</div>}>
      <Switch>
        <Route exact path='/' component={Home} />
        <Route exact path='/list' component={List} />
      </Switch>
    </Suspense>
  </Router>
)
```

+ **<font style="color:rgb(44, 62, 80);">使用SSR</font>**<font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Next.js</font>

**<font style="color:rgb(44, 62, 80);">连环问：你在使用React时遇到过哪些坑</font>**

+ **<font style="color:rgb(44, 62, 80);">自定义组件的名称首字母要大写</font>**

```plain
// 原生html组件
<input />

// 自定义组件
<Input />
```

+ **<font style="color:rgb(44, 62, 80);">JS关键字的冲突</font>**

```plain
// for改成htmlFor，class改成className
<label htmlFor="input-name" className="label">
  用户名 <input id="username" />
</label>
```

+ **<font style="color:rgb(44, 62, 80);">JSX数据类型</font>**

```plain
// correct
<Demo flag={true} />
// error
<Demo flag="true" />
```

+ **<font style="color:rgb(44, 62, 80);">setState不会马上获取最新的结果</font>**
    - <font style="color:rgb(44, 62, 80);">如需要实时获取结果，在回调函数中获取</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState({count:this.state.count + 1},()=>console.log(this.state.count)})</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">在合成事件和生命周期钩子中，是异步更新的</font>
    - <font style="color:rgb(44, 62, 80);">在</font>**<font style="color:rgb(44, 62, 80);">原生事件</font>**<font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setState</font><font style="color:rgb(44, 62, 80);">是同步的，可以马上获取更新后的值；</font>
    - <font style="color:rgb(44, 62, 80);">原因: 原生事件是浏览器本身的实现，与事务流无关，自然是同步；而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">是放置于定时器线程中延后执行，此时事务流已结束，因此也是同步；</font>

```javascript
// setState原理模拟
let isBatchingUpdate = true;

let queue = [];
let state = {number:0};
function setState(newSate){
  //state={...state,...newSate}
  // setState异步更新
  if(isBatchingUpdate){
    queue.push(newSate);
  }else{
    // setState同步更新
    state={...state,...newSate}
  }   
}

// react事件是合成事件，在合成事件中isBatchingUpdate需要设置为true
// 模拟react中事件点击
function handleClick(){
  isBatchingUpdate=true; // 批量更新标志

  /**我们自己逻辑开始 */
  setState({number:state.number+1});
  setState({number:state.number+1});
  console.log(state); // 0
  setState({number:state.number+1});
  console.log(state); // 0
  /**我们自己逻辑结束 */

  state= queue.reduce((newState,action)=>{
    return {...newState,...action}
  },state); 
}
handleClick();
console.log(state); // 1
```

```javascript
// setState笔试题考察 下面这道题输出什么
class Example extends React.Component {
  constructor() {
    super()
    this.state = {
      val: 0
    }
  }
  // componentDidMount中isBatchingUpdate=true setState批量更新
  componentDidMount() {
    this.setState({ val: this.state.val + 1 }) // 添加到queue队列中，等待处理
    console.log(this.state.val)
    // 第 1 次 log
    this.setState({ val: this.state.val + 1 }) // 添加到queue队列中，等待处理
    console.log(this.state.val)
    // 第 2 次 log
    setTimeout(() => {
      // 在原生事件和setTimeout中（isBatchingUpdate=false），setState同步更新，可以马上获取更新后的值
      this.setState({ val: this.state.val + 1 }) // 同步更新
      console.log(this.state.val)
      // 第 3 次 log
      this.setState({ val: this.state.val + 1 }) // 同步更新
      console.log(this.state.val)
      // 第 4 次 log
    }, 0)
  }
  render() {
    return null
  }
}

// 答案：0, 0, 2, 3
```

### [](https://www.123fe.net/docs/base/high-frequency.html#react%E7%9C%9F%E9%A2%98)<font style="color:rgb(44, 62, 80);">React真题</font>
**<font style="color:rgb(44, 62, 80);">1. 函数组件和class组件区别</font>**

+ <font style="color:rgb(44, 62, 80);">纯函数，输入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">，输出</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font>
+ <font style="color:rgb(44, 62, 80);">没有实例、没有生命周期、没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font>
+ <font style="color:rgb(44, 62, 80);">不能拓展其他方法</font>

**<font style="color:rgb(44, 62, 80);">2. 什么是受控组件</font>**

+ <font style="color:rgb(44, 62, 80);">表单的值，受到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">控制</font>
+ <font style="color:rgb(44, 62, 80);">需要自行监听</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onChange</font><font style="color:rgb(44, 62, 80);">，更新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font>
+ <font style="color:rgb(44, 62, 80);">对比非受控组件</font>

**<font style="color:rgb(44, 62, 80);">3. 何时使用异步组件</font>**

+ <font style="color:rgb(44, 62, 80);">加载大组件</font>
+ <font style="color:rgb(44, 62, 80);">路由懒加载</font>

**<font style="color:rgb(44, 62, 80);">4. 多个组件有公共逻辑如何抽离</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HOC</font><font style="color:rgb(44, 62, 80);">高阶组件</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Render Props</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Hooks</font>

**<font style="color:rgb(44, 62, 80);">5. react router如何配置懒加载</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784142739-63e4af00-6a44-4e7c-9b47-57f31c74f043.png)

**<font style="color:rgb(44, 62, 80);">6. react和vue的区别</font>**

**<font style="color:rgb(44, 62, 80);">共同</font>**

+ <font style="color:rgb(44, 62, 80);">都支持组件化</font>
+ <font style="color:rgb(44, 62, 80);">都是数据驱动视图</font>
+ <font style="color:rgb(44, 62, 80);">都用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);">操作</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font>

**<font style="color:rgb(44, 62, 80);">区别</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);">拥抱</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">使用模板拥抱</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">函数式编程，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">是声明式编程</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">更多的是自力更生，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">把你想要的都给你</font>

