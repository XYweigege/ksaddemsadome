## 组件类
<font style="color:rgb(44, 62, 80);">组件类，详细分的话有三种类，第一类说白了就是我平时用于继承的基类组件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);">,还有就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">提供的内置的组件，比如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">StrictMode</font><font style="color:rgb(44, 62, 80);">,另一部分就是高阶组件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwardRef</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">memo</font><font style="color:rgb(44, 62, 80);">等。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874451067-43ec37a3-d54f-42dd-b71b-a230d1770e0c.png)

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#component)<font style="color:rgb(44, 62, 80);">Component</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);">组件的根基。类组件一切始于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(44, 62, 80);">。对于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.Component</font><font style="color:rgb(44, 62, 80);">使用，我们没有什么好讲的。我们这里重点研究一下</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(44, 62, 80);">做了些什么。</font>

```javascript
react/src/ReactBaseClasses.js
function Component(props, context, updater) {
  this.props = props;
  this.context = context;
  this.refs = emptyObject;
  this.updater = updater || ReactNoopUpdateQueue;
}
```

<font style="color:rgb(44, 62, 80);">这就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(44, 62, 80);">函数，其中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updater</font><font style="color:rgb(44, 62, 80);">对象上保存着更新组件的方法。</font>

**<font style="color:rgb(44, 62, 80);">我们声明的类组件是什么时候以何种形式被实例化的呢？</font>**

```plain
react-reconciler/src/ReactFiberClassComponent.js
```

**<font style="color:rgb(44, 62, 80);">constructClassInstance</font>**

```javascript
function constructClassInstance(
  workInProgress,
  ctor,
  props
){
  const instance = new ctor(props, context);
  instance.updater = {
    isMounted,
    enqueueSetState(){
      /* setState 触发这里面的逻辑 */
    },
    enqueueReplaceState(){},
    enqueueForceUpdate(){
      /* forceUpdate 触发这里的逻辑 */
    }
  }
}
```

<font style="color:rgb(44, 62, 80);">对于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">处理逻辑还是很简单的，实例化我们类组件，然后赋值</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updater</font><font style="color:rgb(44, 62, 80);">对象，负责组件的更新。然后在组件各个阶段，执行类组件的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">函数，和对应的生命周期函数就可以了。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#purecomponent)<font style="color:rgb(44, 62, 80);">PureComponent</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(44, 62, 80);">用法，差不多一样，唯一不同的是，纯组件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);">会浅比较，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">是否相同，来决定是否重新渲染组件。所以一般用于</font>**<font style="color:rgb(44, 62, 80);">性能调优</font>**<font style="color:rgb(44, 62, 80);">，减少</font>**<font style="color:rgb(44, 62, 80);">render</font>**<font style="color:rgb(44, 62, 80);">次数。</font>

<font style="color:rgb(44, 62, 80);">什么叫做</font>**<font style="color:rgb(44, 62, 80);">浅比较</font>**<font style="color:rgb(44, 62, 80);">，我这里举个列子：</font>

```javascript
class Index extends React.PureComponent{
  constructor(props){
    super(props)
    this.state={
      data:{
        name:'alien',
        age:28
      }
    }
  }
  handerClick= () =>{
    const { data } = this.state
    data.age++
    this.setState({ data })
  }
  render(){
    const { data } = this.state
    return <div className="box" >
      <div className="show" >
      <div> 你的姓名是: { data.name } </div>
      <div> 年龄： { data.age  }</div>
      <button onClick={ this.handerClick } >age++</button>
      </div>
      </div>
  }
}
```



**<font style="color:rgb(44, 62, 80);">点击按钮，没有任何反应</font>**<font style="color:rgb(44, 62, 80);">，因为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);">会比较两次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">对象，都指向同一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">,没有发生改变，所以不更新视图。</font>

<font style="color:rgb(44, 62, 80);">解决这个问题很简单，只需要在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">handerClick</font><font style="color:rgb(44, 62, 80);">事件中这么写：</font>

```plain
this.setState({ data:{...data} })
```

**<font style="color:rgb(44, 62, 80);">浅拷贝</font>**<font style="color:rgb(44, 62, 80);">就能根本解决问题。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#memo)<font style="color:rgb(44, 62, 80);">memo</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);">作用类似，可以用作性能优化，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是高阶组件，函数组件和类组件都可以使用， 和区别</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);">只能对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">的情况确定是否渲染，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PureComponent</font><font style="color:rgb(44, 62, 80);">是针对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">接受两个参数，第一个参数原始组件本身，第二个参数，可以根据一次更新中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">是否相同决定原始组件是否重新渲染。是一个返回布尔值，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">证明组件无须重新渲染，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">证明组件需要重新渲染，这个和类组件中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate()</font><font style="color:rgb(44, 62, 80);">正好相反 。</font>

**<font style="color:rgb(44, 62, 80);">React.memo: 第二个参数 返回</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">组件不渲染 ， 返回</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">组件重新渲染。</font>**<font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">shouldComponentUpdate: 返回</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">组件渲染 ， 返回</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">组件不渲染。</font>**

<font style="color:rgb(44, 62, 80);">接下来我们做一个场景，控制组件在仅此一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">数字变量，一定范围渲染。</font>

<font style="color:rgb(44, 62, 80);">例子</font><font style="color:rgb(44, 62, 80);">🌰</font><font style="color:rgb(44, 62, 80);">：</font>

<font style="color:rgb(44, 62, 80);">控制</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">number</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">：</font>

+ <font style="color:rgb(44, 62, 80);">1 只有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">number</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">更改，组件渲染。</font>
+ <font style="color:rgb(44, 62, 80);">2 只有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">number</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">小于 5 ，组件渲染。</font>

```javascript
function TextMemo(props){
  console.log('子组件渲染')
  if(props)
    return <div>hello,world</div> 
}

const controlIsRender = (pre,next)=>{
  if(pre.number === next.number  ){ // number 不改变 ，不渲染组件
    return true 
  }else if(pre.number !== next.number && next.number > 5 ) { // number 改变 ，但值大于5 ， 不渲染组件
    return true
  }else { // 否则渲染组件
    return false
  }
}

const NewTexMemo = memo(TextMemo,controlIsRender)
class Index extends React.Component{
  constructor(props){
    super(props)
    this.state={
      number:1,
      num:1
    }
  }
  render(){
    const { num , number }  = this.state
    return <div>
      <div>
      改变num：当前值 { num }  
    <button onClick={ ()=>this.setState({ num:num + 1 }) } >num++</button>
  <button onClick={ ()=>this.setState({ num:num - 1 }) } >num--</button>  
  </div>
  <div>
  改变number： 当前值 { number } 
<button onClick={ ()=>this.setState({ number:number + 1 }) } > number ++</button>
  <button onClick={ ()=>this.setState({ number:number - 1 }) } > number -- </button>  
  </div>
  <NewTexMemo num={ num } number={number}  />
  </div>
}
}
```

**<font style="color:rgb(44, 62, 80);">效果：</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874452278-39af137d-068f-43db-8478-2d501fbaac54.png)

<font style="color:rgb(44, 62, 80);">完美达到了效果，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.memo</font><font style="color:rgb(44, 62, 80);">一定程度上，可以等价于组件外部使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">shouldComponentUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，用于拦截新老</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">，确定组件是否更新。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#forwardref)<font style="color:rgb(44, 62, 80);">forwardRef</font>
<font style="color:rgb(44, 62, 80);">官网对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwardRef</font><font style="color:rgb(44, 62, 80);">的概念和用法很笼统，也没有给定一个具体的案例。很多同学不知道</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwardRef</font><font style="color:rgb(44, 62, 80);">具体怎么用，下面我结合具体例子给大家讲解</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwardRef</font><font style="color:rgb(44, 62, 80);">应用场景。</font>

**<font style="color:rgb(44, 62, 80);">1 转发引入Ref</font>**

<font style="color:rgb(44, 62, 80);">这个场景实际很简单，比如父组件想获取孙组件，某一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">元素。这种隔代</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">获取引用，就需要</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwardRef</font><font style="color:rgb(44, 62, 80);">来助力。</font>

```javascript
function Son (props){
  const { grandRef } = props
  return <div>
    <div> i am alien </div>
    <span ref={grandRef} >这个是想要获取元素</span>
    </div>
}

class Father extends React.Component{
  constructor(props){
    super(props)
  }
  render(){
    return <div>
      <Son grandRef={this.props.grandRef}  />
  </div>
}
}

const NewFather = React.forwardRef((props,ref)=><Father grandRef={ref}  {...props} />  )

class GrandFather extends React.Component{
  constructor(props){
    super(props)
  }
  node = null 
  componentDidMount(){
    console.log(this.node)
  }
  render(){
    return <div>
      <NewFather ref={(node)=> this.node = node } />
  </div>
}
}
```

**<font style="color:rgb(44, 62, 80);">效果</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874451541-0a90ec45-6f28-4fd5-b6cc-78d2450dab4c.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">不允许</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">传递，因为组件上已经有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个属性,在组件调和过程中，已经被特殊处理，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwardRef</font><font style="color:rgb(44, 62, 80);">出现就是解决这个问题，把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">转发到自定义的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwardRef</font><font style="color:rgb(44, 62, 80);">定义的属性上，让</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">，可以通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">传递。</font>

**<font style="color:rgb(44, 62, 80);">2 高阶组件转发Ref</font>**

<font style="color:rgb(44, 62, 80);">一文吃透</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hoc</font><font style="color:rgb(44, 62, 80);">文章中讲到，由于属性代理的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hoc</font><font style="color:rgb(44, 62, 80);">，被包裹一层，所以如果是类组件，是通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">拿不到原始组件的实例的，不过我们可以通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forWardRef</font><font style="color:rgb(44, 62, 80);">转发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">。</font>

```javascript
function HOC(Component){
  class Wrap extends React.Component{
    render(){
      const { forwardedRef ,...otherprops  } = this.props
      return <Component ref={forwardedRef}  {...otherprops}  />
        }
  }
  return  React.forwardRef((props,ref)=> <Wrap forwardedRef={ref} {...props} /> ) 
}
class Index extends React.Component{
  componentDidMount(){
    console.log(666)
  }
  render(){
    return <div>hello,world</div>
  }
}
const HocIndex =  HOC(Index,true)
export default ()=>{
  const node = useRef(null)
  useEffect(()=>{
    /* 就可以跨层级，捕获到 Index 组件的实例了 */ 
    console.log(node.current.componentDidMount)
  },[])
  return <div><HocIndex ref={node}  /></div>
}
```

<font style="color:rgb(44, 62, 80);">如上，解决了高阶组件引入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ref</font><font style="color:rgb(44, 62, 80);">的问题。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#lazy)<font style="color:rgb(44, 62, 80);">lazy</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">React.lazy 和 Suspense 技术还不支持服务端渲染。如果你想要在使用服务端渲染的应用中使用，我们推荐 Loadable Components 这个库</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.lazy</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Suspense</font><font style="color:rgb(44, 62, 80);">配合一起用，能够有动态加载组件的效果。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.lazy</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">接受一个函数，这个函数需要动态调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">import()</font><font style="color:rgb(44, 62, 80);">。它必须返回一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">需要</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resolve</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">default export</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件。</font>

<font style="color:rgb(44, 62, 80);">我们模拟一个动态加载的场景。</font>

**<font style="color:rgb(44, 62, 80);">父组件</font>**

```javascript
import Test from './comTest'
const LazyComponent =  React.lazy(()=> new Promise((resolve)=>{
  setTimeout(()=>{
    resolve({
      default: ()=> <Test />
        })
  },2000)
}))
class index extends React.Component{   
  render(){
    return <div className="context_box"  style={ { marginTop :'50px' } }   >
  <React.Suspense fallback={ <div className="icon" ><SyncOutlined  spin  /></div> } >
  <LazyComponent />
  </React.Suspense>
  </div>
}
}
```

<font style="color:rgb(44, 62, 80);">我们用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">来模拟</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">import</font><font style="color:rgb(44, 62, 80);">异步引入效果。</font>

**<font style="color:rgb(44, 62, 80);">Test</font>**

```javascript
class Test extends React.Component{
  constructor(props){
    super(props)
  }
  componentDidMount(){
    console.log('--componentDidMount--')
  }
  render(){
    return <div>
      <img src={alien}  className="alien" />
      </div>
  }
}
```

**<font style="color:rgb(44, 62, 80);">效果</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874451842-d7426bca-ec69-4304-8027-d669e98f08e3.png)

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#suspense)<font style="color:rgb(44, 62, 80);">Suspense</font>
<font style="color:rgb(44, 62, 80);">何为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Suspense</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Suspense</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">让组件“等待”某个异步操作，直到该异步操作结束即可渲染。</font>

<font style="color:rgb(44, 62, 80);">用于数据获取的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Suspense</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是一个新特性，你可以使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><Suspense></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以声明的方式来“等待”任何内容，包括数据。本文重点介绍它在数据获取的用例，它也可以用于等待图像、脚本或其他异步的操作。</font>

<font style="color:rgb(44, 62, 80);">上面讲到高阶组件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">lazy</font><font style="color:rgb(44, 62, 80);">时候，已经用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">lazy</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Suspense</font><font style="color:rgb(44, 62, 80);">模式，构建了异步渲染组件。我们看一下官网文档中的案例：</font>

```plain
const ProfilePage = React.lazy(() => import('./ProfilePage')); // 懒加载
<Suspense fallback={<Spinner />}>
  <ProfilePage />
</Suspense>
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#fragment)<font style="color:rgb(44, 62, 80);">Fragment</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">不允许一个组件返回多个节点元素，比如说如下情况</font>

```plain
render(){
    return <li> 🍎🍎🍎 </li>
           <li> 🍌🍌🍌 </li>
           <li> 🍇🍇🍇 </li>
}
```

<font style="color:rgb(44, 62, 80);">如果我们想解决这个情况，很简单，只需要在外层套一个容器元素。</font>

```plain
render(){
    return <div>
           <li> 🍎🍎🍎 </li>
           <li> 🍌🍌🍌 </li>
           <li> 🍇🍇🍇 </li>
    </div>
}
```

<font style="color:rgb(44, 62, 80);">但是我们不期望，增加额外的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">节点，所以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">提供</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);">碎片概念，能够让一个组件返回多个元素。 所以我们可以这么写</font>

```plain
<React.Fragment>
    <li> 🍎🍎🍎 </li>
    <li> 🍌🍌🍌 </li>
    <li> 🍇🍇🍇 </li>
</React.Fragment>
```

<font style="color:rgb(44, 62, 80);">还可以简写成：</font>

```plain
<>
    <li> 🍎🍎🍎 </li>
    <li> 🍌🍌🍌 </li>
    <li> 🍇🍇🍇 </li>
</>
```

<font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);">区别是，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);">可以支持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">属性。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><></></font><font style="color:rgb(44, 62, 80);">不支持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">属性。</font>

**<font style="color:rgb(44, 62, 80);">温馨提示</font>**<font style="color:rgb(44, 62, 80);">。我们通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">map</font><font style="color:rgb(44, 62, 80);">遍历后的元素，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">底层会处理，默认在外部嵌套一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><Fragment></font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">比如：</font>

```plain
{
   [1,2,3].map(item=><span key={item.id} >{ item.name }</span>)
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">底层处理之后，等价于：</font>

```html
<Fragment>
   <span></span>
   <span></span>
   <span></span>
</Fragment>
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#profiler)<font style="color:rgb(44, 62, 80);">Profiler</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Profiler</font><font style="color:rgb(44, 62, 80);">这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">api</font><font style="color:rgb(44, 62, 80);">一般用于开发阶段，性能检测，检测一次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">组件渲染用时，性能开销。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Profiler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">需要两个参数：</font>

<font style="color:rgb(44, 62, 80);">第一个参数：是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">id</font><font style="color:rgb(44, 62, 80);">，用于表识唯一性的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Profiler</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">第二个参数：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onRender</font><font style="color:rgb(44, 62, 80);">回调函数，用于渲染完成，接受渲染参数。</font>

**<font style="color:rgb(44, 62, 80);">实践：</font>**

```javascript
const index = () => {
  const callback = (...arg) => console.log(arg)
  return <div >
    <div >
    <Profiler id="root" onRender={ callback }  >
    <Router  >
    <Meuns/>
    <KeepaliveRouterSwitch withoutRoute >
    { renderRoutes(menusList) }
    </KeepaliveRouterSwitch>
    </Router>
    </Profiler> 
    </div>
    </div>
}
```

**<font style="color:rgb(44, 62, 80);">结果</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874722961-2c89a0b9-35d9-458e-a8c2-0eaabd1455a2.png)

<font style="color:rgb(44, 62, 80);">onRender</font>

+ <font style="color:rgb(44, 62, 80);">0 -id:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">root</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">-></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Profiler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">树的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">id</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">1 -phase:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">-></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">挂载 ，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">update</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">渲染了。</font>
+ <font style="color:rgb(44, 62, 80);">2 -actualDuration:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">6.685000262223184</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">-> 更新</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">committed</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">花费的渲染时间。</font>
+ <font style="color:rgb(44, 62, 80);">3 -baseDuration:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">4.430000321008265</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">-> 渲染整颗子树需要的时间</font>
+ <font style="color:rgb(44, 62, 80);">4 -startTime :</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">689.7299999836832</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">-> 本次更新开始渲染的时间</font>
+ <font style="color:rgb(44, 62, 80);">5 -commitTime :</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">698.5799999674782</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">-> 本次更新committed 的时间</font>
+ <font style="color:rgb(44, 62, 80);">6 -interactions:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">set{}</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">-> 本次更新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">interactions</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的集合</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">尽管 Profiler 是一个轻量级组件，我们依然应该在需要时才去使用它。对一个应用来说，每添加一些都会给 CPU 和内存带来一些负担。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#strictmode)<font style="color:rgb(44, 62, 80);">StrictMode</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">StrictMode</font><font style="color:rgb(44, 62, 80);">见名知意，严格模式，用于检测</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">项目中的潜在的问题，。与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一样，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">StrictMode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不会渲染任何可见的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">。它为其后代元素触发额外的检查和警告。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">严格模式检查仅在开发模式下运行；它们不会影响生产构建。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">StrictMode</font><font style="color:rgb(44, 62, 80);">目前有助于：</font>

+ <font style="color:rgb(44, 62, 80);">①识别不安全的生命周期。</font>
+ <font style="color:rgb(44, 62, 80);">②关于使用过时字符串</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref API</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的警告</font>
+ <font style="color:rgb(44, 62, 80);">③关于使用废弃的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">findDOMNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的警告</font>
+ <font style="color:rgb(44, 62, 80);">④检测意外的副作用</font>
+ <font style="color:rgb(44, 62, 80);">⑤检测过时的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">context API</font>

**<font style="color:rgb(44, 62, 80);">实践:识别不安全的生命周期</font>**

<font style="color:rgb(44, 62, 80);">对于不安全的生命周期，指的是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UNSAFE_componentWillMount</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UNSAFE_componentWillReceiveProps</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UNSAFE_componentWillUpdate</font>

```javascript
外层开启严格模式：
  <React.StrictMode> 
  <Router  >
  <Meuns/>
  <KeepaliveRouterSwitch withoutRoute >
    { renderRoutes(menusList) }
    </KeepaliveRouterSwitch>
    </Router>
    </React.StrictMode>

我们在内层组件中，使用不安全的生命周期:
class Index extends React.Component{    
  UNSAFE_componentWillReceiveProps(){
  }
  render(){      
    return <div className="box" />   
      }
}

效果：
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874713528-7759ad94-564c-4971-9c71-7f2425907d53.png)

## [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#%E5%B7%A5%E5%85%B7%E7%B1%BB)工具类
<font style="color:rgb(44, 62, 80);">接下来我们一起来探究一下</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">工具类函数的用法。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874697624-347fbc52-c599-4f8b-903c-4e37ac599663.png)

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#createelement)<font style="color:rgb(44, 62, 80);">createElement</font>
<font style="color:rgb(44, 62, 80);">一提到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">，就不由得和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSX</font><font style="color:rgb(44, 62, 80);">联系一起。我们写的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jsx</font><font style="color:rgb(44, 62, 80);">，最终会被</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">babel</font><font style="color:rgb(44, 62, 80);">，用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">编译成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">元素形式。我写一个组件，我们看一下会被编译成什么样子，</font>

<font style="color:rgb(44, 62, 80);">如果我们在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">里面这么写：</font>

```plain
render(){
    return <div className="box" >
        <div className="item"  >生命周期</div>
        <Text  mes="hello,world"  />
        <React.Fragment> Flagment </React.Fragment>
        { /*  */ }
        text文本
    </div>
}
```

<font style="color:rgb(44, 62, 80);">会被编译成这样：</font>

```plain
render() {
    return React.createElement("div", { className: "box" },
            React.createElement("div", { className: "item" }, "\u751F\u547D\u5468\u671F"),
            React.createElement(Text, { mes: "hello,world" }),
            React.createElement(React.Fragment, null, " Flagment "),
            "text\u6587\u672C");
    }
```

<font style="color:rgb(44, 62, 80);">当然我们可以不用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jsx</font><font style="color:rgb(44, 62, 80);">模式，而是直接通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">进行开发。</font>

**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font>****<font style="color:rgb(44, 62, 80);">模型:</font>**

```plain
React.createElement(
  type,
  [props],
  [...children]
)
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">参数：</font>

<font style="color:rgb(44, 62, 80);">**第一个参数:**如果是组件类型，会传入组件，如果是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">元素类型，传入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">span</font><font style="color:rgb(44, 62, 80);">之类的字符串。</font>

**<font style="color:rgb(44, 62, 80);">第二个参数:</font>**<font style="color:rgb(44, 62, 80);">:第二个参数为一个对象，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">类型中为</font>**<font style="color:rgb(44, 62, 80);">属性</font>**<font style="color:rgb(44, 62, 80);">，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">组件</font><font style="color:rgb(44, 62, 80);">类型中为</font>**<font style="color:rgb(44, 62, 80);">props</font>**<font style="color:rgb(44, 62, 80);">。</font>

**<font style="color:rgb(44, 62, 80);">其他参数:</font>**<font style="color:rgb(44, 62, 80);">，依次为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">，根据顺序排列。</font>

**<font style="color:rgb(44, 62, 80);">createElement做了些什么？</font>**

<font style="color:rgb(44, 62, 80);">经过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">处理，最终会形成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$$typeof = Symbol(react.element)</font><font style="color:rgb(44, 62, 80);">对象。对象上保存了该</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react.element</font><font style="color:rgb(44, 62, 80);">的信息。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#cloneelement)<font style="color:rgb(44, 62, 80);">cloneElement</font>
<font style="color:rgb(44, 62, 80);">可能有的同学还傻傻的分不清楚</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cloneElement</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">区别和作用。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createElement</font><font style="color:rgb(44, 62, 80);">把我们写的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jsx</font><font style="color:rgb(44, 62, 80);">，变成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">element</font><font style="color:rgb(44, 62, 80);">对象; 而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cloneElement</font><font style="color:rgb(44, 62, 80);">的作用是以</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">element</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素为样板克隆并返回新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素。返回元素的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是将新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与原始元素的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">浅层合并后的结果。</font>

<font style="color:rgb(44, 62, 80);">那么</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cloneElement</font><font style="color:rgb(44, 62, 80);">感觉在我们实际业务组件中，可能没什么用，但是在</font>**<font style="color:rgb(44, 62, 80);">一些开源项目，或者是公共插槽组件中</font>**<font style="color:rgb(44, 62, 80);">用处还是蛮大的，比如说，我们可以在组件中，劫持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children element</font><font style="color:rgb(44, 62, 80);">，然后通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cloneElement</font><font style="color:rgb(44, 62, 80);">克隆</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">element</font><font style="color:rgb(44, 62, 80);">，混入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">。经典的案例就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-router</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Swtich</font><font style="color:rgb(44, 62, 80);">组件，通过这种方式，来匹配唯一的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Route</font><font style="color:rgb(44, 62, 80);">并加以渲染。</font>

<font style="color:rgb(44, 62, 80);">我们设置一个场景，在组件中，去劫持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">，然后给</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">赋能一些额外的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">:</font>

```javascript
function FatherComponent({ children }){
  const newChildren = React.cloneElement(children, { age: 18})
  return <div> { newChildren } </div>
}

function SonComponent(props){
  console.log(props)
  return <div>hello,world</div>
}

class Index extends React.Component{    
  render(){      
    return <div className="box" >
      <FatherComponent>
      <SonComponent name="alien"  />
      </FatherComponent>
      </div>   
  }
}
```

**<font style="color:rgb(44, 62, 80);">打印：</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874683865-05f7dbdc-8dc0-48af-9427-a62c265b8f63.png)

<font style="color:rgb(44, 62, 80);">完美达到了效果！</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#createcontext)<font style="color:rgb(44, 62, 80);">createContext</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createContext</font><font style="color:rgb(44, 62, 80);">用于创建一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Context</font><font style="color:rgb(44, 62, 80);">对象，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createContext</font><font style="color:rgb(44, 62, 80);">对象中，包括用于传递</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Context</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象值</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Provider</font><font style="color:rgb(44, 62, 80);">，和接受</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">变化订阅的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Consumer</font><font style="color:rgb(44, 62, 80);">。</font>

```plain
const MyContext = React.createContext(defaultValue)
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createContext</font><font style="color:rgb(44, 62, 80);">接受一个参数</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defaultValue</font><font style="color:rgb(44, 62, 80);">，如果</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Consumer</font><font style="color:rgb(44, 62, 80);">上一级一直没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Provider</font><font style="color:rgb(44, 62, 80);">,则会应用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defaultValue</font><font style="color:rgb(44, 62, 80);">作为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">。</font>**<font style="color:rgb(44, 62, 80);">只有</font>**<font style="color:rgb(44, 62, 80);">当组件所处的树中没有匹配到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Provider</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">时，其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defaultValue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">参数才会生效。</font>

<font style="color:rgb(44, 62, 80);">我们来模拟一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Context.Provider</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Context.Consumer</font><font style="color:rgb(44, 62, 80);">的例子：</font>

```javascript
function ComponentB(){
  /* 用 Consumer 订阅， 来自 Provider 中 value 的改变  */
  return <MyContext.Consumer>
    { (value) => <ComponentA  {...value} /> }
    </MyContext.Consumer>
}

function ComponentA(props){
  const { name , mes } = props
  return <div> 
    <div> 姓名： { name }  </div>
    <div> 想对大家说： { mes }  </div>
    </div>
}

function index(){
  const [ value , ] = React.useState({
    name:'alien',
    mes:'let us learn React '
  })
  return <div style={{ marginTop:'50px' }} >
  <MyContext.Provider value={value}  >
  <ComponentB />
    </MyContext.Provider>
    </div>
}
```

**<font style="color:rgb(44, 62, 80);">打印结果：</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874673496-2e03ae38-856b-493b-a3ef-ea7c23f3e8cf.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Provider</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Consumer</font><font style="color:rgb(44, 62, 80);">的良好的特性，可以做数据的</font>**<font style="color:rgb(44, 62, 80);">存</font>**<font style="color:rgb(44, 62, 80);">和</font>**<font style="color:rgb(44, 62, 80);">取</font>**<font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Consumer</font><font style="color:rgb(44, 62, 80);">一方面传递</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">,另一方面可以订阅</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">的改变。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Provider</font><font style="color:rgb(44, 62, 80);">还有一个特性可以层层传递</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">，这种特性在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-redux</font><font style="color:rgb(44, 62, 80);">中表现的淋漓尽致。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#createfactory)<font style="color:rgb(44, 62, 80);">createFactory</font>
```plain
React.createFactory(type)
```

<font style="color:rgb(44, 62, 80);">返回用于生成指定类型 React 元素的函数。类型参数既可以是标签名字符串（像是 '</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">' 或 '</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">span</font><font style="color:rgb(44, 62, 80);">'），也可以是 React 组件 类型 （</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件或函数组件），或是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React fragment</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型。</font>

<font style="color:rgb(44, 62, 80);">使用：</font>

```javascript
const Text = React.createFactory(()=><div>hello,world</div>) 
                                 function Index(){  
                                   return <div style={{ marginTop:'50px'  }} >
                                   <Text/>
                                   </div>
}
```

**<font style="color:rgb(44, 62, 80);">效果</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874660208-d5543433-44e3-420f-9e8d-57a333b13196.png)

<font style="color:rgb(44, 62, 80);">报出警告，这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">api</font><font style="color:rgb(44, 62, 80);">将要被废弃，我们这里就不多讲了，如果想要达到同样的效果，请用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.createElement</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#createref)<font style="color:rgb(44, 62, 80);">createRef</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createRef</font><font style="color:rgb(44, 62, 80);">可以创建一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素，附加在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">元素上。</font>

**<font style="color:rgb(44, 62, 80);">用法：</font>**

```javascript
class Index extends React.Component{
  constructor(props){
    super(props)
    this.node = React.createRef()
  }
  componentDidMount(){
    console.log(this.node)
  }
  render(){
    return <div ref={this.node} > my name is alien </div>
  }
}
```

<font style="color:rgb(44, 62, 80);">个人觉得</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createRef</font><font style="color:rgb(44, 62, 80);">这个方法，很鸡肋，我们完全可以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);">类组件中这么写，来捕获</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">。</font>

```javascript
class Index extends React.Component{
  node = null
  componentDidMount(){
    console.log(this.node)
  }
  render(){
    return <div ref={(node)=> this.node } > my name is alien </div>
  }
}
```

<font style="color:rgb(44, 62, 80);">或者在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);">组件中这么写：</font>

```javascript
function Index(){
  const node = React.useRef(null)
  useEffect(()=>{
    console.log(node.current)
  },[])
  return <div ref={node} >  my name is alien </div>
}
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#isvalidelement)<font style="color:rgb(44, 62, 80);">isValidElement</font>
<font style="color:rgb(44, 62, 80);">这个方法可以用来检测是否为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react element</font><font style="color:rgb(44, 62, 80);">元素,接受待验证对象，返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">。这个api可能对于业务组件的开发，作用不大，因为对于组件内部状态，都是已知的，我们根本就不需要去验证，是否是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react element</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素。 但是，对于一起公共组件或是开源库，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isValidElement</font><font style="color:rgb(44, 62, 80);">就很有作用了。</font>

**<font style="color:rgb(44, 62, 80);">实践</font>**

<font style="color:rgb(44, 62, 80);">我们做一个场景，验证容器组件的所有子组件，过滤到非</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react element</font><font style="color:rgb(44, 62, 80);">类型。</font>

<font style="color:rgb(44, 62, 80);">没有用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isValidElement</font><font style="color:rgb(44, 62, 80);">验证之前：</font>

```javascript
const Text = () => <div>hello,world</div> 
class WarpComponent extends React.Component{
  constructor(props){
    super(props)
  }
  render(){
    return this.props.children
  }
}
function Index(){
  return <div style={{ marginTop:'50px' }} >
  <WarpComponent>
  <Text/>
  <div> my name is alien </div>
Let's learn react together!
  </WarpComponent>
  </div>
}
```

**<font style="color:rgb(44, 62, 80);">过滤之前的效果</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874625331-0da18134-94d1-4674-84dd-f85586c7daed.png)

**<font style="color:rgb(44, 62, 80);">我们用</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isValidElement</font>****<font style="color:rgb(44, 62, 80);">进行</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react element</font>****<font style="color:rgb(44, 62, 80);">验证:</font>**

```javascript
class WarpComponent extends React.Component{
  constructor(props){
    super(props)
    this.newChidren = this.props.children.filter(item => React.isValidElement(item) )
  }
  render(){
    return this.newChidren
  }
}
```

**<font style="color:rgb(44, 62, 80);">过滤之后效果</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874615345-169ff654-42aa-4e66-aa18-3d7961987563.png)

<font style="color:rgb(44, 62, 80);">过滤掉了非</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react element</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Let's learn react together!</font><font style="color:rgb(44, 62, 80);">。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#children-map)<font style="color:rgb(44, 62, 80);">Children.map</font>
<font style="color:rgb(44, 62, 80);">接下来的五个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">api</font><font style="color:rgb(44, 62, 80);">都是和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react.Chidren</font><font style="color:rgb(44, 62, 80);">相关的，我们来分别介绍一下，我们先来看看官网的描述，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.Children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">提供了用于处理</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.props.children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不透明数据结构的实用方法。</font>

<font style="color:rgb(44, 62, 80);">有的同学会问遍历</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">用数组方法,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">map</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forEach</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不就可以了吗？ 请我们注意一下</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">不透明数据结构</font><font style="color:rgb(44, 62, 80);">,什么叫做不透明结构?</font>

**<font style="color:rgb(44, 62, 80);">我们先看一下透明的结构：</font>**

```javascript
class Text extends React.Component{
  render(){
    return <div>hello,world</div>
  }
}
function WarpComponent(props){
  console.log(props.children)
  return props.children
}
function Index(){
  return <div style={{ marginTop:'50px' }} >
  <WarpComponent>
  <Text/>
  <Text/>
  <Text/>
  <span>hello,world</span>
  </WarpComponent>
  </div>
}
```

**<font style="color:rgb(44, 62, 80);">打印</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874461776-c00397ab-783c-4fe9-a3b7-57102910a481.png)

<font style="color:rgb(44, 62, 80);">但是我们把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Index</font><font style="color:rgb(44, 62, 80);">结构改变一下：</font>

```javascript
function Index(){
  return <div style={{ marginTop:'50px' }} >
  <WarpComponent>
  { new Array(3).fill(0).map(()=><Text/>) }
<span>hello,world</span>
  </WarpComponent>
  </div>
}
```

**<font style="color:rgb(44, 62, 80);">打印</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874461932-83a2c190-64c2-478e-8f65-499fd83d43b5.png)

<font style="color:rgb(44, 62, 80);">这个数据结构，我们不能正常的遍历了，即使遍历也不能遍历，每一个子元素。此时就需要</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react.Chidren</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来帮忙了。</font>

<font style="color:rgb(44, 62, 80);">但是我们把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WarpComponent</font><font style="color:rgb(44, 62, 80);">组件用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react.Chidren</font><font style="color:rgb(44, 62, 80);">处理</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">:</font>

```plain
function WarpComponent(props){
    const newChildren = React.Children.map(props.children,(item)=>item)
    console.log(newChildren)
    return newChildren
}
```

<font style="color:rgb(44, 62, 80);">此时就能正常遍历了，达到了预期效果。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874463342-e3d08a95-4920-4e99-b0f3-0d75ffcec0cb.png)

**<font style="color:rgb(44, 62, 80);">注意</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">如果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象，它将被视为单一子节点的情况处理，而不会被遍历。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#children-foreach)<font style="color:rgb(44, 62, 80);">Children.forEach</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Children.forEach</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Children.map</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">用法类似，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Children.map</font><font style="color:rgb(44, 62, 80);">可以返回新的数组，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Children.forEach</font><font style="color:rgb(44, 62, 80);">仅停留在遍历阶段。</font>

<font style="color:rgb(44, 62, 80);">我们将上面的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WarpComponent</font><font style="color:rgb(44, 62, 80);">方法，用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Children.forEach</font><font style="color:rgb(44, 62, 80);">改一下。</font>

```plain
function WarpComponent(props){
    React.Children.forEach(props.children,(item)=>console.log(item))
    return props.children
}
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#children-count)<font style="color:rgb(44, 62, 80);">Children.count</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的组件总数量，等同于通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">map</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forEach</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">调用回调函数的次数。对于更复杂的结果，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Children.count</font><font style="color:rgb(44, 62, 80);">可以返回同一级别子组件的数量。</font>

<font style="color:rgb(44, 62, 80);">我们还是把上述例子进行改造：</font>

```javascript
function WarpComponent(props){
  const childrenCount =  React.Children.count(props.children)
  console.log(childrenCount,'childrenCount')
  return props.children
}   
function Index(){
  return <div style={{ marginTop:'50px' }} >
  <WarpComponent>
  { new Array(3).fill(0).map((item,index) => new Array(2).fill(1).map((item,index1)=><Text key={index+index1} />)) }
                             <span>hello,world</span>
                             </WarpComponent>
                             </div>
                             }
```

**<font style="color:rgb(44, 62, 80);">效果:</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874463038-4f02f763-6a99-4c39-a592-0fbad14d0f3c.png)

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#children-toarray)<font style="color:rgb(44, 62, 80);">Children.toArray</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Children.toArray</font><font style="color:rgb(44, 62, 80);">返回，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props.children</font><font style="color:rgb(44, 62, 80);">扁平化后结果。</font>

```javascript
function WarpComponent(props){
  const newChidrenArray =  React.Children.toArray(props.children)
  console.log(newChidrenArray,'newChidrenArray')
  return newChidrenArray
}   
function Index(){
  return <div style={{ marginTop:'50px' }} >
  <WarpComponent>
  { new Array(3).fill(0).map((item,index)=>new Array(2).fill(1).map((item,index1)=><Text key={index+index1} />)) }
                             <span>hello,world</span>
                             </WarpComponent>
                             </div>
                             }
```

**<font style="color:rgb(44, 62, 80);">效果：</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874464883-514b61e8-ad1a-4367-ad40-6d3f689cd423.png)

**<font style="color:rgb(44, 62, 80);">newChidrenArray</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">,就是扁平化的数组结构。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.Children.toArray()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在拉平展开子节点列表时，更改</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值以保留嵌套数组的语义。也就是说，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toArray</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会为返回数组中的每个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">添加前缀，以使得每个元素</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的范围都限定在此函数入参数组的对象内。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#children-only)<font style="color:rgb(44, 62, 80);">Children.only</font>
<font style="color:rgb(44, 62, 80);">验证</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是否只有一个子节点（一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素），如果有则返回它，否则此方法会抛出错误。</font>

**<font style="color:rgb(44, 62, 80);">不唯一</font>**

```javascript
function WarpComponent(props){
  console.log(React.Children.only(props.children))
  return props.children
}   
function Index(){
  return <div style={{ marginTop:'50px' }} >
  <WarpComponent>
  { new Array(3).fill(0).map((item,index)=><Text key={index} />) }
                             <span>hello,world</span>
                             </WarpComponent>
                             </div>
                             }
```

**<font style="color:rgb(44, 62, 80);">效果</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874464354-10af44c8-e29c-43b3-a615-763d1715c02b.png)

**<font style="color:rgb(44, 62, 80);">唯一</font>**

```javascript
function WarpComponent(props){
  console.log(React.Children.only(props.children))
  return props.children
}   
function Index(){
  return <div style={{ marginTop:'50px' }} >
  <WarpComponent>
  <Text/>
  </WarpComponent>
  </div>
}
```

**<font style="color:rgb(44, 62, 80);">效果</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874466733-e27946b7-6fb1-4418-8d0d-b8ce25f4fdd1.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.Children.only()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不接受</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React.Children.map()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的返回值，因为它是一个数组而并不是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素。</font>

## [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#react-hooks)react-hooks
<font style="color:rgb(44, 62, 80);">对于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-hooks</font><font style="color:rgb(44, 62, 80);">,我已经写了三部曲，介绍了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-hooks</font><font style="color:rgb(44, 62, 80);">使用，自定义</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hooks</font><font style="color:rgb(44, 62, 80);">，以及</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-hooks</font><font style="color:rgb(44, 62, 80);">原理，感兴趣的同学可以去看看，文章末尾有链接，对于常用的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">api</font><font style="color:rgb(44, 62, 80);">，我这里参考了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-hooks</font><font style="color:rgb(44, 62, 80);">如何使用那篇文章。并做了相应精简化和一些内容的补充。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874466340-12cc708a-d8b6-4009-97ed-cabfaf5a6253.png)

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#usestate)<font style="color:rgb(44, 62, 80);">useState</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useState</font><font style="color:rgb(44, 62, 80);">可以弥补函数组件没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">的缺陷。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useState</font><font style="color:rgb(44, 62, 80);">可以接受一个初识值，也可以是一个函数</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">返回值作为新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">。返回一个数组，第一个值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">读取值，第二个值为改变</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatchAction</font><font style="color:rgb(44, 62, 80);">函数。</font>

<font style="color:rgb(44, 62, 80);">我们看一个例子：</font>

```jsx
const DemoState = (props) => {
   /* number为此时state读取值 ，setNumber为派发更新的函数 */
   let [number, setNumber] = useState(0) /* 0为初始值 */
   return (<div>
       <span>{ number }</span>
       <button onClick={ ()=> {
         setNumber(number+1) /* 写法一 */
         setNumber(number=>number + 1 ) /* 写法二 */
         console.log(number) /* 这里的number是不能够即时改变的  */
       } } >num++</button>
   </div>)
}
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#useeffect)<font style="color:rgb(44, 62, 80);">useEffect</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);">可以弥补函数组件没有生命周期的缺点。我们可以在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);">第一个参数回调函数中，做一些请求数据，事件监听等操作，第二个参数作为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dep</font><font style="color:rgb(44, 62, 80);">依赖项，当依赖项发生变化，重新执行第一个函数。</font>

**<font style="color:rgb(44, 62, 80);">useEffect可以用作数据交互。</font>**

```jsx
/* 模拟数据交互 */
function getUserInfo(a){
    return new Promise((resolve)=>{
        setTimeout(()=>{ 
           resolve({
               name:a,
               age:16,
           }) 
        },500)
    })
}
const DemoEffect = ({ a }) => {
    const [ userMessage , setUserMessage ] :any= useState({})
    const div= useRef()
    const [number, setNumber] = useState(0)
    /* 模拟事件监听处理函数 */
    const handleResize =()=>{}
    /* useEffect使用 ，这里如果不加限制 ，会是函数重复执行，陷入死循环*/
    useEffect(()=>{
        /* 请求数据 */
       getUserInfo(a).then(res=>{
           setUserMessage(res)
       })
       /* 操作dom  */
       console.log(div.current) /* div */
       /* 事件监听等 */
        window.addEventListener('resize', handleResize)
    /* 只有当props->a和state->number改变的时候 ,useEffect副作用函数重新执行 ，如果此时数组为空[]，证明函数只有在初始化的时候执行一次相当于componentDidMount */
    },[ a ,number ])
    return (<div ref={div} >
        <span>{ userMessage.name }</span>
        <span>{ userMessage.age }</span>
        <div onClick={ ()=> setNumber(1) } >{ number }</div>
    </div>)
}
```

**<font style="color:rgb(44, 62, 80);">useEffect可以用作事件监听，还有一些基于</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font>****<font style="color:rgb(44, 62, 80);">的操作。</font>**<font style="color:rgb(44, 62, 80);">,别忘了在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);">第一个参数回调函数，返一个函数用于清除事件监听等操作。</font>

```jsx
const DemoEffect = ({ a }) => {
    /* 模拟事件监听处理函数 */
    const handleResize =()=>{}
    useEffect(()=>{
       /* 定时器 延时器等 */
       const timer = setInterval(()=>console.log(666),1000)
       /* 事件监听 */
       window.addEventListener('resize', handleResize)
       /* 此函数用于清除副作用 */
       return function(){
           clearInterval(timer) 
           window.removeEventListener('resize', handleResize)
       }
    },[ a ])
    return (<div  >
    </div>)
}
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#usememo)<font style="color:rgb(44, 62, 80);">useMemo</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);">接受两个参数，第一个参数是一个函数，返回值用于产生</font>**<font style="color:rgb(44, 62, 80);">保存值</font>**<font style="color:rgb(44, 62, 80);">。 第二个参数是一个数组，作为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dep</font><font style="color:rgb(44, 62, 80);">依赖项，数组里面的依赖项发生变化，重新执行第一个函数，产生</font>**<font style="color:rgb(44, 62, 80);">新的值</font>**<font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">应用场景：</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">1 缓存一些值，避免重新执行上下文</font>**

```javascript
const number = useMemo(()=>{
  /** ....大量的逻辑运算 **/
  return number
},[ props.number ]) // 只有 props.number 改变的时候，重新计算number的值。
```

**<font style="color:rgb(44, 62, 80);">2 减少不必要的</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font>****<font style="color:rgb(44, 62, 80);">循环</font>**

```javascript
/* 用 useMemo包裹的list可以限定当且仅当list改变的时候才更新此list，这样就可以避免selectList重新循环 */
{useMemo(() => (
  <div>{
    selectList.map((i, v) => (
      <span
                   className={style.listSpan}
key={v} >
  {i.patentName} 
  </span>
))}
  </div>
), [selectList])}
```

**<font style="color:rgb(44, 62, 80);">3 减少子组件渲染</font>**

```javascript
/* 只有当props中，list列表改变的时候，子组件才渲染 */
const  goodListChild = useMemo(()=> <GoodList list={ props.list } /> ,[ props.list ])
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#usecallback)<font style="color:rgb(44, 62, 80);">useCallback</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useCallback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">接收的参数都是一样，都是在其依赖项发生变化后才执行，都是返回缓存的值，区别在于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">返回的是函数运行的结果，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useCallback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">返回的是函数。 返回的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callback</font><font style="color:rgb(44, 62, 80);">可以作为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">回调函数传递给子组件。</font>

```javascript
/* 用react.memo */
const DemoChildren = React.memo((props)=>{
  /* 只有初始化的时候打印了 子组件更新 */
  console.log('子组件更新')
  useEffect(()=>{
    props.getInfo('子组件')
  },[])
  return <div>子组件</div>
})
const DemoUseCallback=({ id })=>{
  const [number, setNumber] = useState(1)
  /* 此时usecallback的第一参数 (sonName)=>{ console.log(sonName) }
     经过处理赋值给 getInfo */
  const getInfo  = useCallback((sonName)=>{
    console.log(sonName)
  },[id])
  return <div>
    {/* 点击按钮触发父组件更新 ，但是子组件没有更新 */}
    <button onClick={ ()=>setNumber(number+1) } >增加</button>
    <DemoChildren getInfo={getInfo} />
    </div>
}
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#useref)<font style="color:rgb(44, 62, 80);">useRef</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useRef</font><font style="color:rgb(44, 62, 80);">的作用：</font>

+ <font style="color:rgb(44, 62, 80);">一 是可以用来获取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">元素，或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);">组件实例 。</font>
+ <font style="color:rgb(44, 62, 80);">二</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-hooks原理</font><font style="color:rgb(44, 62, 80);">文章中讲过，创建</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useRef</font><font style="color:rgb(44, 62, 80);">时候，会创建一个原始对象，只要函数组件不被销毁，原始对象就会一直存在，那么我们可以利用这个特性，来通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useRef</font><font style="color:rgb(44, 62, 80);">保存一些数据。</font>

```jsx
const DemoUseRef = ()=>{
    const dom= useRef(null)
    const handerSubmit = ()=>{
        /*  <div >表单组件</div>  dom 节点 */
        console.log(dom.current)
    }
    return <div>
        {/* ref 标记当前dom节点 */}
        <div ref={dom} >表单组件</div>
        <button onClick={()=>handerSubmit()} >提交</button> 
    </div>
}
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#uselayouteffect)<font style="color:rgb(44, 62, 80);">useLayoutEffect</font>
**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font>****<font style="color:rgb(44, 62, 80);">执行顺序:</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件更新挂载完成 -> 浏览器</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">绘制完成 -> 执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">回调。</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useLayoutEffect</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">执行顺序:</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件更新挂载完成 -> 执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useLayoutEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">回调-> 浏览器</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">绘制完成。</font>

<font style="color:rgb(44, 62, 80);">所以说</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useLayoutEffect</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">代码可能会阻塞浏览器的绘制 。我们写的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">effect</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useLayoutEffect</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">在底层会被分别打上</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PassiveEffect</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HookLayout</font><font style="color:rgb(44, 62, 80);">，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commit</font><font style="color:rgb(44, 62, 80);">阶段区分出，在什么时机执行。</font>

```jsx
const DemoUseLayoutEffect = () => {
    const target = useRef()
    useLayoutEffect(() => {
        /*我们需要在dom绘制之前，移动dom到制定位置*/
        const { x ,y } = getPositon() /* 获取要移动的 x,y坐标 */
        animate(target.current,{ x,y })
    }, []);
    return (
        <div >
            <span ref={ target } className="animate"></span>
        </div>
    )
}
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#usereducer)<font style="color:rgb(44, 62, 80);">useReducer</font>
<font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-hooks</font><font style="color:rgb(44, 62, 80);">原理那篇文章中讲解到，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useState</font><font style="color:rgb(44, 62, 80);">底层就是一个简单版的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useReducer</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useReducer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">接受的第一个参数是一个函数，我们可以认为它就是一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的参数就是常规</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reducer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里面的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">,返回改变后的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useReducer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">第二个参数为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的初始值 返回一个数组，数组的第一项就是更新之后</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值 ，第二个参数是派发更新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数。</font>

<font style="color:rgb(44, 62, 80);">我们来看一下</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useReducer</font><font style="color:rgb(44, 62, 80);">如何使用：</font>

```javascript
const DemoUseReducer = ()=>{
  /* number为更新后的state值,  dispatchNumbner 为当前的派发函数 */
  const [ number , dispatchNumbner ] = useReducer((state,action)=>{
    const { payload , name  } = action
    /* return的值为新的state */
    switch(name){
      case 'add':
        return state + 1
      case 'sub':
        return state - 1 
      case 'reset':
        return payload       
    }
    return state
  },0)
  return <div>
    当前值：{ number }
  { /* 派发更新 */ }
  <button onClick={()=>dispatchNumbner({ name:'add' })} >增加</button>
  <button onClick={()=>dispatchNumbner({ name:'sub' })} >减少</button>
  <button onClick={()=>dispatchNumbner({ name:'reset' ,payload:666 })} >赋值</button>
{ /* 把dispatch 和 state 传递给子组件  */ }
<MyChildren  dispatch={ dispatchNumbner } State={{ number }} />
  </div>
}
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#usecontext)<font style="color:rgb(44, 62, 80);">useContext</font>
<font style="color:rgb(44, 62, 80);">我们可以使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useContext</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，来获取父级组件传递过来的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">context</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值，这个当前值就是最近的父级组件</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Provider</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">设置的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">值，useContext</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">参数一般是由</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createContext</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方式引入 ,也可以父级上下文</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">context</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">传递 ( 参数为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">context</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">)。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useContext</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以代替</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">context.Consumer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来获取</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Provider</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中保存的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值</font>

```jsx
/* 用useContext方式 */
const DemoContext = ()=> {
    const value:any = useContext(Context)
    /* my name is alien */
return <div> my name is { value.name }</div>
}
/* 用Context.Consumer 方式 */
const DemoContext1 = ()=>{
    return <Context.Consumer>
         {/*  my name is alien  */}
        { (value)=> <div> my name is { value.name }</div> }
    </Context.Consumer>
}

export default ()=>{
    return <div>
        <Context.Provider value={{ name:'alien' , age:18 }} >
            <DemoContext />
            <DemoContext1 />
        </Context.Provider>
    </div>
}
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#useimperativehandle)<font style="color:rgb(44, 62, 80);">useImperativeHandle</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useImperativeHandle</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以配合</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwardRef</font><font style="color:rgb(44, 62, 80);">自定义暴露给父组件的实例值。这个很有用，我们知道，对于子组件，如果是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);">类组件，我们可以通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">获取类组件的实例，但是在子组件是函数组件的情况，如果我们不能直接通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">的，那么此时</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useImperativeHandle</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwardRef</font><font style="color:rgb(44, 62, 80);">配合就能达到效果。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useImperativeHandle</font><font style="color:rgb(44, 62, 80);">接受三个参数：</font>

+ <font style="color:rgb(44, 62, 80);">第一个参数ref: 接受</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forWardRef</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">传递过来的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">第二个参数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createHandle</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">：处理函数，返回值作为暴露给父组件的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">对象。</font>
+ <font style="color:rgb(44, 62, 80);">第三个参数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">deps</font><font style="color:rgb(44, 62, 80);">:依赖项</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">deps</font><font style="color:rgb(44, 62, 80);">，依赖项更改形成新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">对象。</font>

**<font style="color:rgb(44, 62, 80);">我们来模拟给场景，用</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useImperativeHandle</font>****<font style="color:rgb(44, 62, 80);">，使得父组件能让子组件中的</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">input</font>****<font style="color:rgb(44, 62, 80);">自动赋值并聚焦。</font>**

```javascript
function Son (props,ref) {
  console.log(props)
  const inputRef = useRef(null)
  const [ inputValue , setInputValue ] = useState('')
  useImperativeHandle(ref,()=>{
    const handleRefs = {
      /* 声明方法用于聚焦input框 */
      onFocus(){
        inputRef.current.focus()
      },
      /* 声明方法用于改变input的值 */
      onChangeValue(value){
        setInputValue(value)
      }
    }
    return handleRefs
  },[])
  return <div>
    <input
  placeholder="请输入内容"
  ref={inputRef}
  value={inputValue}
    />
    </div>
}

const ForwarSon = forwardRef(Son)

class Index extends React.Component{
  cur = null
  handerClick(){
    const { onFocus , onChangeValue } =this.cur
    onFocus()
    onChangeValue('let us learn React!')
  }
  render(){
    return <div style={{ marginTop:'50px' }} >
  <ForwarSon ref={cur => (this.cur = cur)} />
  <button onClick={this.handerClick.bind(this)} >操控子组件</button>
  </div>
}
}
```

**<font style="color:rgb(44, 62, 80);">效果:</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874466916-d202d866-94da-42de-ac21-92b1a1e673a9.png)

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#usedebugvalue)<font style="color:rgb(44, 62, 80);">useDebugValue</font>
```plain
useDebugValue` 可用于在 `React` 开发者工具中显示自定义 `hook` 的标签。这个`hooks`目的就是检查自定义`hooks
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);
  // ...
  // 在开发者工具中的这个 Hook 旁边显示标签
  // e.g. "FriendStatus: Online"
  useDebugValue(isOnline ? 'Online' : 'Offline');

  return isOnline;
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们不推荐你向每个自定义 Hook 添加 debug 值。当它作为共享库的一部分时才最有价值。在某些情况下，格式化值的显示可能是一项开销很大的操作。除非需要检查 Hook，否则没有必要这么做。因此，useDebugValue 接受一个格式化函数作为可选的第二个参数。该函数只有在 Hook 被检查时才会被调用。它接受 debug 值作为参数，并且会返回一个格式化的显示值。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#usetransition)<font style="color:rgb(44, 62, 80);">useTransition</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useTransition</font><font style="color:rgb(44, 62, 80);">允许延时由</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">改变而带来的视图渲染。避免不必要的渲染。它还允许组件将速度较慢的数据获取更新推迟到随后渲染，以便能够立即渲染更重要的更新。</font>

```plain
const TIMEOUT_MS = { timeoutMs: 2000 }
const [startTransition, isPending] = useTransition(TIMEOUT_MS)
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useTransition</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">接受一个对象，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">timeoutMs</font><font style="color:rgb(44, 62, 80);">代码需要延时的时间。</font>
+ <font style="color:rgb(44, 62, 80);">返回一个数组。</font>**<font style="color:rgb(44, 62, 80);">第一个参数：</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是一个接受回调的函数。我们用它来告诉</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">需要推迟的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">第二个参数：</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一个布尔值。表示是否正在等待，过度状态的完成(延时</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">的更新)。</font>

<font style="color:rgb(44, 62, 80);">下面我们引入官网的列子，来了解</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useTransition</font><font style="color:rgb(44, 62, 80);">的使用。</font>

```javascript
const SUSPENSE_CONFIG = { timeoutMs: 2000 };

function App() {
  const [resource, setResource] = useState(initialResource);
  const [startTransition, isPending] = useTransition(SUSPENSE_CONFIG);
  return (
    <>
    <button
  disabled={isPending}
  onClick={() => {
    startTransition(() => {
      const nextUserId = getNextId(resource.userId);
      setResource(fetchProfileData(nextUserId));
    });
  }}
    >
    Next
    </button>
  {isPending ? " 加载中..." : null}
  <Suspense fallback={<Spinner />}>
  <ProfilePage resource={resource} />
  </Suspense>
  </>
);
}
```

<font style="color:rgb(44, 62, 80);">在这段代码中，我们使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">startTransition</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">包装了我们的数据获取。这使我们可以立即开始获取用户资料的数据，同时推迟下一个用户资料页面以及其关联的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Spinner</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的渲染 2 秒钟（</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">timeoutMs</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中显示的时间）。</font>

<font style="color:rgb(44, 62, 80);">这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">api</font><font style="color:rgb(44, 62, 80);">目前处于实验阶段，没有被完全开放出来。</font>

## [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#react-dom)react-dom
<font style="color:rgb(44, 62, 80);">接下来，我们来一起研究</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-dom</font><font style="color:rgb(44, 62, 80);">中比较重要的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">api</font><font style="color:rgb(44, 62, 80);">。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874468783-a3a65f29-a155-48a2-bbd0-c15e8c1de614.png)

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#render)<font style="color:rgb(44, 62, 80);">render</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是我们最常用的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-dom</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">api</font><font style="color:rgb(44, 62, 80);">，用于渲染一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">元素，一般</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">项目我们都用它，渲染根部容器</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">app</font><font style="color:rgb(44, 62, 80);">。</font>

```plain
ReactDOM.render(element, container[, callback])
```

**<font style="color:rgb(44, 62, 80);">使用</font>**

```jsx
ReactDOM.render(
    < App / >,
    document.getElementById('app')
)
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOM.render</font><font style="color:rgb(44, 62, 80);">会控制</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">container</font><font style="color:rgb(44, 62, 80);">容器节点里的内容，但是不会修改容器节点本身。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#hydrate)<font style="color:rgb(44, 62, 80);">hydrate</font>
<font style="color:rgb(44, 62, 80);">服务端渲染用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hydrate</font><font style="color:rgb(44, 62, 80);">。用法与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">相同，但它用于在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ReactDOMServer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">渲染的容器中对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的内容进行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hydrate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">操作。</font>

```plain
ReactDOM.hydrate(element, container[, callback])
```

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#createportal)<font style="color:rgb(44, 62, 80);">createPortal</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">提供了一种将子节点渲染到存在于父组件以外的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点的优秀的方案。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createPortal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以把当前组件或</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">element</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素的子节点，渲染到组件之外的其他地方。</font>

<font style="color:rgb(44, 62, 80);">那么具体应用到什么场景呢？</font>

<font style="color:rgb(44, 62, 80);">比如一些全局的弹窗组件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">model</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><Model/></font><font style="color:rgb(44, 62, 80);">组件一般都写在我们的组件内部，倒是真正挂载的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">，都是在外层容器，比如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">body</font><font style="color:rgb(44, 62, 80);">上。此时就很适合</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createPortal</font><font style="color:rgb(44, 62, 80);">API。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createPortal</font><font style="color:rgb(44, 62, 80);">接受两个参数：</font>

```plain
ReactDOM.createPortal(child, container)
```

<font style="color:rgb(44, 62, 80);">第一个：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">child</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是任何可渲染的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">子元素 第二个：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">container</font><font style="color:rgb(44, 62, 80);">是一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素。</font>

<font style="color:rgb(44, 62, 80);">接下来我们实践一下：</font>

```javascript
function WrapComponent({ children }){
  const domRef = useRef(null)
  const [ PortalComponent, setPortalComponent ] = useState(null)
  React.useEffect(()=>{
    setPortalComponent( ReactDOM.createPortal(children,domRef.current) )
  },[])
  return <div> 
    <div className="container" ref={ domRef } ></div>
  { PortalComponent }
  </div>
}

class Index extends React.Component{
  render(){
    return <div style={{ marginTop:'50px' }} >
  <WrapComponent>
  <div  >hello,world</div>
  </WrapComponent>
  </div>
}
}
```

**<font style="color:rgb(44, 62, 80);">效果</font>**![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874527334-a9fe6e78-dd88-4a86-9050-b1c02ab46661.png)

<font style="color:rgb(44, 62, 80);">我们可以看到，我们</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">实际在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">container</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之外挂载的，但是已经被</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createPortal</font><font style="color:rgb(44, 62, 80);">渲染到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">container</font><font style="color:rgb(44, 62, 80);">中。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#unstable-batchedupdates)<font style="color:rgb(44, 62, 80);">unstable_batchedUpdates</font>
<font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-legacy</font><font style="color:rgb(44, 62, 80);">模式下，对于事件，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">事件有批量更新来处理功能,但是这一些非常规的事件中，批量更新功能会被打破。所以我们可以用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react-dom</font><font style="color:rgb(44, 62, 80);">中提供的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unstable_batchedUpdates</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来进行批量更新。</font>

**<font style="color:rgb(44, 62, 80);">一次点击实现的批量更新</font>**

```javascript
class Index extends React.Component{
  constructor(props){
    super(props)
    this.state={
      numer:1,
    }
  }
  handerClick=()=>{
    this.setState({ numer : this.state.numer + 1 })
    console.log(this.state.numer)
    this.setState({ numer : this.state.numer + 1 })
    console.log(this.state.numer)
    this.setState({ numer : this.state.numer + 1 })
    console.log(this.state.numer)
  }
  render(){
    return <div  style={{ marginTop:'50px' }} > 
  <button onClick={ this.handerClick } >click me</button>
  </div>
}
}
```

**<font style="color:rgb(44, 62, 80);">效果</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874515426-f0143ff8-8447-40e7-ad92-0e8eb4b65537.png)

<font style="color:rgb(44, 62, 80);">渲染次数一次。</font>

**<font style="color:rgb(44, 62, 80);">批量更新条件被打破</font>**

```javascript
handerClick=()=>{
  Promise.resolve().then(()=>{
    this.setState({ numer : this.state.numer + 1 })
    console.log(this.state.numer)
    this.setState({ numer : this.state.numer + 1 })
    console.log(this.state.numer)
    this.setState({ numer : this.state.numer + 1 })
    console.log(this.state.numer)
  })
}
```

**<font style="color:rgb(44, 62, 80);">效果</font>**![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874506920-4686c8c0-5f83-4eb7-a5fa-4aa057686e93.png)

<font style="color:rgb(44, 62, 80);">渲染次数三次。</font>

**<font style="color:rgb(44, 62, 80);">unstable_batchedUpdate助力</font>**

```javascript
handerClick=()=>{
  Promise.resolve().then(()=>{
    ReactDOM.unstable_batchedUpdates(()=>{
      this.setState({ numer : this.state.numer + 1 })
      console.log(this.state.numer)
      this.setState({ numer : this.state.numer + 1 })
      console.log(this.state.numer)
      this.setState({ numer : this.state.numer + 1 })
      console.log(this.state.numer)
    }) 
  })
}
```

<font style="color:rgb(44, 62, 80);">渲染次数一次,完美解决批量更新问题。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#flushsync)<font style="color:rgb(44, 62, 80);">flushSync</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flushSync</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以将回调函数中的更新任务，放在一个较高的优先级中。我们知道</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">设定了很多不同优先级的更新任务。如果一次更新任务在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flushSync</font><font style="color:rgb(44, 62, 80);">回调函数内部，那么将获得一个较高优先级的更新。比如</font>

```plain
ReactDOM.flushSync(()=>{
    /* 此次更新将设置一个较高优先级的更新 */
    this.setState({ name: 'alien'  })
})
```

<font style="color:rgb(44, 62, 80);">为了让大家理解</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flushSync</font><font style="color:rgb(44, 62, 80);">，我这里做一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">demo</font><font style="color:rgb(44, 62, 80);">奉上，</font>

```plain
/* flushSync */
import ReactDOM from 'react-dom'
class Index extends React.Component{
    state={ number:0 }
    handerClick=()=>{
        setTimeout(()=>{
            this.setState({ number: 1  })
        })
        this.setState({ number: 2  })
        ReactDOM.flushSync(()=>{
            this.setState({ number: 3  })
        })
        this.setState({ number: 4  })
    }
    render(){
        const { number } = this.state
        console.log(number) // 打印什么？？
        return <div>
            <div>{ number }</div>
            <button onClick={this.handerClick} >测试flushSync</button>
        </div>
    }
}
```

<font style="color:rgb(44, 62, 80);">先不看答案，点击一下按钮，打印什么呢？</font>

**<font style="color:rgb(44, 62, 80);">我们来点击一下看看</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874469135-6ee54d04-9fed-4bcd-b44a-37519e9205f6.png)

<font style="color:rgb(44, 62, 80);">打印 0 3 4 1 ，相信不难理解为什么这么打印了。</font>

+ <font style="color:rgb(44, 62, 80);">首先</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flushSync</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.setState({ number: 3 })</font><font style="color:rgb(44, 62, 80);">设定了一个高优先级的更新，所以3 先被打印</font>
+ <font style="color:rgb(44, 62, 80);">2 4 被批量更新为 4</font>

<font style="color:rgb(44, 62, 80);">相信这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">demo</font><font style="color:rgb(44, 62, 80);">让我们更深入了解了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flushSync</font><font style="color:rgb(44, 62, 80);">。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#finddomnode)<font style="color:rgb(44, 62, 80);">findDOMNode</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">findDOMNode</font><font style="color:rgb(44, 62, 80);">用于访问组件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">元素节点，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">推荐使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">模式，不期望使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">findDOMNode</font><font style="color:rgb(44, 62, 80);">。</font>

```plain
ReactDOM.findDOMNode(component)
```

<font style="color:rgb(44, 62, 80);">注意的是：</font>

+ <font style="color:rgb(44, 62, 80);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">findDOMNode</font><font style="color:rgb(44, 62, 80);">只能用在已经挂载的组件上。</font>
+ <font style="color:rgb(44, 62, 80);">2 如果组件渲染内容为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">，那么</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">findDOMNode</font><font style="color:rgb(44, 62, 80);">返回值也是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">findDOMNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不能用于函数组件。</font>

<font style="color:rgb(44, 62, 80);">接下来让我们看一下，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">findDOMNode</font><font style="color:rgb(44, 62, 80);">具体怎么使用的：</font>

```plain
class Index extends React.Component{
    handerFindDom=()=>{
        console.log(ReactDOM.findDOMNode(this))
    }
    render(){
        return <div style={{ marginTop:'100px' }} >
            <div>hello,world</div>
            <button onClick={ this.handerFindDom } >获取容器dom</button>
        </div>
    }
}
```

**<font style="color:rgb(44, 62, 80);">效果：</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874470730-ae32b2c8-2671-4206-95b1-bb8c16f9d604.png)

<font style="color:rgb(44, 62, 80);">我们完全可以将外层容器用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">来标记，获取捕获原生的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">节点。</font>

### [](https://www.123fe.net/principle-docs/react/26-React%E5%85%A8%E9%83%A8api%E8%A7%A3%E8%AF%BB.html#unmountcomponentatnode)<font style="color:rgb(44, 62, 80);">unmountComponentAtNode</font>
<font style="color:rgb(44, 62, 80);">从</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中卸载组件，会将其事件处理器和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一并清除。 如果指定容器上没有对应已挂载的组件，这个函数什么也不会做。如果组件被移除将会返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，如果没有组件可被移除将会返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">我们来简单举例看看</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unmountComponentAtNode</font><font style="color:rgb(44, 62, 80);">如何使用？</font>

```plain
function Text(){
    return <div>hello,world</div>
}

class Index extends React.Component{
    node = null
    constructor(props){
       super(props)
       this.state={
           numer:1,
       }
    }
    componentDidMount(){
        /*  组件初始化的时候，创建一个 container 容器 */
        ReactDOM.render(<Text/> , this.node )
    }
    handerClick=()=>{
       /* 点击卸载容器 */ 
       const state =  ReactDOM.unmountComponentAtNode(this.node)
       console.log(state)
    }
    render(){
        return <div  style={{ marginTop:'50px' }}  > 
             <div ref={ ( node ) => this.node = node  }  ></div>  
            <button onClick={ this.handerClick } >click me</button>
        </div>
    }
}
```

**<font style="color:rgb(44, 62, 80);">效果</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874472890-a725565c-0696-4d4c-a871-b99e10af2778.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718874474217-a2f17dcb-d709-4cdd-b020-e0f8ba818f94.png)

<font style="color:rgb(44, 62, 80);">  
</font>

