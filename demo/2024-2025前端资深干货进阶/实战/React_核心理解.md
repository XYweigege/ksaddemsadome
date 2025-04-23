# <font style="color:rgb(51, 51, 51);">react16之前</font>更新
<br/>color1
React Fiber是16版本之后的一种更新机制，使用链表取代了树，是一种fiber数据结构，其有三个指针，分别指向了父节点、子节点、兄弟节点，当中断的时候会记录下当前的节点，然后继续更新，而15版本中的DOM stack不能有中断操作，它把组件渲染的工作分片，到时会主动让出渲染主线程；提炼fiber的关键词，大概给出如下几点：

fiber是一种数据结构。

fiber使用父子关系以及next的妙用，以链表形式模拟了传统调用栈。

fiber是一种调度让出机制，只在有剩余时间的情况下运行。

fiber实现了增量渲染，在浏览器允许的情况下一点点拼凑出最终渲染效果。

fiber实现了并发，为任务赋予不同优先级，保证了一有时间总是做最高优先级的事，而不是先来先占位死板的去执行。

fiber有协调与提交两个阶段，协调包含了fiber创建与diff更新，此过程可暂停。而提交必须同步执行，保证渲染不卡顿。

<br/>

# <font style="color:rgb(51, 51, 51);">react17更新</font>
### <font style="color:rgb(51, 51, 51);">1、新的JSX转换，不需要手动引入react</font>
**<font style="color:rgb(51, 51, 51);">React 16：</font>**

<font style="color:rgb(51, 51, 51);">babel-loader会预编译JSX为 </font>**<font style="color:rgb(51, 51, 51);">React.createElement(...)</font>**

**<font style="color:rgb(51, 51, 51);">React 17：</font>**

<font style="color:rgb(51, 51, 51);">React 17中的 JSX 转换不会将 JSX 转换为 </font>**<font style="color:rgb(51, 51, 51);">React.createElement</font>**<font style="color:rgb(51, 51, 51);">，而是自动从 React 的 package 中引入</font>react<font style="color:rgb(51, 51, 51);">并调用。</font>

<font style="color:rgb(51, 51, 51);">另外此次升级不会改变 JSX 语法，旧的 JSX 转换也将继续工作。</font>

### <font style="color:rgb(51, 51, 51);">2、事件代理更改</font>
<font style="color:rgb(51, 51, 51);">在React 17中，将不再在后台的文档级别附加事件处理程序，不在document对象上绑定事件，改为绑定于每个react应用的rootNode节点，因为各个应用的rootNode肯定不同，所以这样可以使多个版本的react应用同时安全的存在于页面中，不会因为事件绑定系统起冲突。react应用之间也可以安全的进行嵌套。</font>

### <font style="color:rgb(51, 51, 51);">3、事件池(event pooling)的改变</font>
<font style="color:rgb(51, 51, 51);">React 17去除了事件池(event pooling)，不在需要e.persist()，现在可以直接在异步事件中（回掉或timeout等）拿到事件对象，操作更加直观，不会令人迷惑。e.persist()仍然可用，但是不会有任何效果。</font>

### <font style="color:rgb(51, 51, 51);">4、异步执行</font>
<font style="color:rgb(51, 51, 51);">React 17将副作用清理函数(useEffect)改为异步执行，即在浏览器渲染完毕后执行。</font>

### <font style="color:rgb(51, 51, 51);">5、forwardRef 和 memo组件的行为</font>
<font style="color:rgb(51, 51, 51);">React 17中forwardRef 和 memo组件的行为会与常规函数组件和class组件保持一致。它们在返回undefined时会报错。</font>

<font style="color:rgb(51, 51, 51);"></font>

# react18更新
### <font style="color:rgb(33, 37, 41);">并发模式</font>
<font style="color:rgb(33, 37, 41);">v18的新特性是使用现代浏览器的特性构建的，彻底放弃对 IE 的支持。</font>

<font style="color:rgb(33, 37, 41);">v17 和 v18 的区别就是：从同步不可中断更新变成了异步可中断更新，v17可以通过一些试验性的API开启并发模式，而v18则全面开启并发模式。</font>

<font style="color:rgb(33, 37, 41);">并发模式可帮助应用保持响应，并根据用户的设备性能和网速进行适当的调整，该模式通过使渲染可中断来修复阻塞渲染限制。在 Concurrent 模式中，React 可以同时更新多个状态。</font>

<font style="color:rgb(33, 37, 41);">这里参考下文区分几个概念：</font>

+ **<font style="color:rgb(33, 37, 41);">并发模式</font>**<font style="color:rgb(33, 37, 41);">是实现</font>**<font style="color:rgb(33, 37, 41);">并发更新</font>**<font style="color:rgb(33, 37, 41);">的基本前提</font>
+ <font style="color:rgb(33, 37, 41);">v18 中，以是否使用</font>**<font style="color:rgb(33, 37, 41);">并发特性</font>**<font style="color:rgb(33, 37, 41);">作为是否开启</font>**<font style="color:rgb(33, 37, 41);">并发更新</font>**<font style="color:rgb(33, 37, 41);">的依据。</font>
+ **<font style="color:rgb(33, 37, 41);">并发特性</font>**<font style="color:rgb(33, 37, 41);">指开启</font>**<font style="color:rgb(33, 37, 41);">并发模式</font>**<font style="color:rgb(33, 37, 41);">后才能使用的特性，比如：</font>useDeferredValue<font style="color:rgb(33, 37, 41);">/</font>useTransition

### <font style="color:rgb(33, 37, 41);">更新 render API</font>
<font style="color:rgb(33, 37, 41);">v18 使用 ReactDOM.createRoot() 创建一个新的根元素进行渲染，使用该 API，会自动启用并发模式。如果你升级到v18，但没有使用</font>ReactDOM.createRoot()<font style="color:rgb(33, 37, 41);">代替</font>ReactDOM.render()<font style="color:rgb(33, 37, 41);">时，控制台会打印错误日志要提醒你使用React，该警告也意味此项变更没有造成breaking change，而可以并存，当然尽量是不建议。</font>

```javascript
// v17
import ReactDOM from 'react-dom'
import App from './App'

ReactDOM.render(<App />, document.getElementById('root'))

// v18
import ReactDOM from 'react-dom/client'
import App from './App'

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(<App />)
```

### <font style="color:rgb(33, 37, 41);">自动批处理</font>
<font style="color:rgb(33, 37, 41);">批处理是指 React 将多个状态更新，聚合到一次 render 中执行，以提升性能</font>

<font style="color:rgb(33, 37, 41);">在v17的批处理只会在事件处理函数中实现，而在Promise链、异步代码、原生事件处理函数中失效。而v18则所有的更新都会自动进行批处理。</font>

```javascript
// v17
const handleBatching = () => {
  // re-render 一次，这就是批处理的作用
  setCount((c) => c + 1)
  setFlag((f) => !f)
}

// re-render两次
setTimeout(() => {
   setCount((c) => c + 1)
   setFlag((f) => !f)
}, 0)


// v18
const handleBatching = () => {
  // re-render 一次
  setCount((c) => c + 1)
  setFlag((f) => !f)
}
// 自动批处理：re-render 一次
setTimeout(() => {
   setCount((c) => c + 1)
   setFlag((f) => !f)
}, 0)

```

<font style="color:rgb(33, 37, 41);"></font>

### <font style="color:rgb(33, 37, 41);">Suspense 支持 SSR</font>
<font style="color:rgb(33, 37, 41);">SSR 一次页面渲染的流程：</font>

1. <font style="color:rgb(33, 37, 41);">服务器获取页面所需数据</font>
2. <font style="color:rgb(33, 37, 41);">将组件渲染成 HTML 形式作为响应返回</font>
3. <font style="color:rgb(33, 37, 41);">客户端加载资源</font>
4. <font style="color:rgb(33, 37, 41);">（hydrate）执行 JS，并生成页面最终内容</font>

<font style="color:rgb(33, 37, 41);">上述流程是串行执行的，v18前的 SSR 有一个问题就是它不允许组件"等待数据"，必须收集好所有的数据，才能开始向客户端发送HTML。如果其中有一步比较慢，都会影响整体的渲染速度。</font>

<font style="color:rgb(33, 37, 41);">v18 中使用并发渲染特性扩展了</font>Suspense<font style="color:rgb(33, 37, 41);">的功能，使其支持流式 SSR，将 React 组件分解成更小的块，允许服务端一点一点返回页面，尽早发送 HTML和选择性的 hydrate， 从而可以使SSR更快的加载页面：</font>

```javascript
<Suspense fallback={<Spinner />}>
  <Comments />
</Suspense>
```



### <font style="color:rgb(33, 37, 41);">startTransition</font>
<font style="color:rgb(33, 37, 41);">Transitions 是 React 18 引入的一个全新的并发特性。它允许你将标记更新作为一个 transitions（过渡），这会告诉 React 它们可以被中断执行，并避免回到已经可见内容的 Suspense 降级方案。本质上是用于一些不是很急迫的更新上，用来进行并发控制</font>

<font style="color:rgb(33, 37, 41);">在v18之前，所有的更新任务都被视为急迫的任务，而Concurrent Mode 模式能将渲染中断，可以让高优先级的任务先更新渲染。</font>

<font style="color:rgb(33, 37, 41);">React 的状态更新可以分为两类：</font>

+ <font style="color:rgb(33, 37, 41);">紧急更新：比如点击按钮、搜索框打字是需要立即响应的行为，如果没有立即响应给用户的体验就是感觉卡顿延迟</font>
+ <font style="color:rgb(33, 37, 41);">过渡/非紧急更新：将 UI 从一个视图过渡到另一个视图。一些延迟可以接受的更新操作，不需要立即响应</font>

startTransition<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">API 允许将更新标记为非紧急事件处理，被</font>startTransition<font style="color:rgb(33, 37, 41);">包裹的会延迟更新的state，期间可能被其他紧急渲染所抢占。因为 React 会在高优先级更新渲染完成之后，才会渲染低优先级任务的更新</font>

<font style="color:rgb(33, 37, 41);">React 无法自动识别哪些更新是优先级更高的。比如用户的键盘输入操作后，setInputValue会立即更新用户的输入到界面上，是紧急更新。而setSearchQuery是根据用户输入，查询相应的内容，是非紧急的。</font>

```javascript
const [inputValue, setInputValue] = useState()

const onChange = (e)=>{
  setInputValue(e.target.value) // 更新用户输入值（用户打字交互的优先级应该要更高）
  setSearchQuery(e.target.value)  // 更新搜索列表（可能有点延迟，影响）
}

return (
  <input value={inputValue} onChange={onChange} />
)
```

<font style="color:rgb(33, 37, 41);">React无法自动识别，所以它提供了 </font>startTransition<font style="color:rgb(33, 37, 41);">让我们手动指定哪些更新是紧急的，哪些是非紧急的，从而让我们改善用户交互体验。</font>

```javascript
// 紧急的更新
setInputValue(e.target.value)
// 开启并发更新
startTransition(() => {
  setSearchQuery(input) // 非紧急的更新
})
```

### <font style="color:rgb(33, 37, 41);">useTransition</font>
<font style="color:rgb(33, 37, 41);">当有过渡任务（非紧急更新）时，我们可能需要告诉用户什么时候当前处于 pending（过渡） 状态，因此v18提供了一个带有</font>isPending<font style="color:rgb(33, 37, 41);">标志的 Hook</font><font style="color:rgb(33, 37, 41);"> </font>useTransition<font style="color:rgb(33, 37, 41);">来跟踪 transition 状态，用于过渡期。</font>

useTransition<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">执行返回一个数组。数组有两个状态值：</font>

+ isPending<font style="color:rgb(33, 37, 41);">: 指处于过渡状态，正在加载中</font>
+ startTransition<font style="color:rgb(33, 37, 41);">: 通过回调函数将状态更新包装起来告诉 React这是一个过渡任务，是一个低优先级的更新</font>

```javascript
function TransitionTest() {
  const [isPending, startTransition] = useTransition()
  const [count, setCount] = useState(0)

  function handleClick() {
    startTransition(() => {
      setCount((c) => c + 1)
    })
  }

  return (
    <div>
      {isPending && <div>spinner...</div>}
      <button onClick={handleClick}>{count}</button>
    </div>
  )
}
```

<br/>color1
<font style="color:rgb(33, 37, 41);">直观感觉这有点像 </font>setTimeout<font style="color:rgb(33, 37, 41);">，而防抖节流其实本质也是</font>setTimeout<font style="color:rgb(33, 37, 41);">，区别是防抖节流是控制了执行频率，让渲染次数减少了，而 v18的 transition 则没有减少渲染的次数。</font>

<br/>



### <font style="color:rgb(33, 37, 41);">useDeferredValue</font>
useDeferredValue<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">和</font><font style="color:rgb(33, 37, 41);"> </font>useTransition<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">一样，都是标记了一次非紧急更新。</font>useTransition<font style="color:rgb(33, 37, 41);">是处理一段逻辑，而</font>useDeferredValue<font style="color:rgb(33, 37, 41);">是产生一个新状态，它是延时状态，这个新的状态则叫 DeferredValue。所以使用</font>useDeferredValue<font style="color:rgb(33, 37, 41);">可以推迟状态的渲染</font>

useDeferredValue<font style="color:rgb(33, 37, 41);"> 接受一个值，并返回该值的新副本，该副本将推迟到紧急更新之后。如果当前渲染是一个紧急更新的结果，比如用户输入，React 将返回之前的值，然后在紧急渲染完成后渲染新的值。</font>

```javascript
function Typeahead() {
  const query = useSearchQuery('');
  const deferredQuery = useDeferredValue(query);

  // Memoizing 告诉 React 仅当 deferredQuery 改变，
  // 而不是 query 改变的时候才重新渲染
  const suggestions = useMemo(() =>
    <SearchSuggestions query={deferredQuery} />,
    [deferredQuery]
  );

  return (
    <>
      <SearchInput query={query} />
      <Suspense fallback="Loading results...">
        {suggestions}
      </Suspense>
    </>
  );
}
```

<font style="color:rgb(33, 37, 41);">这样一看，</font>useDeferredValue<font style="color:rgb(33, 37, 41);">直观就是延迟显示状态，那用防抖节流有什么区别呢？</font>

<font style="color:rgb(33, 37, 41);">如果使用防抖节流，比如延迟300ms显示则意味着所有用户都要延时，在渲染内容较少、用户CPU性能较好的情况下也是会延迟300ms，而且你要根据实际情况来调整延迟的合适值；但是</font>useDeferredValue<font style="color:rgb(33, 37, 41);">是否延迟取决于计算机的性能。</font>

**<font style="color:rgb(33, 37, 41);">useId</font>**  
useId<font style="color:rgb(33, 37, 41);">支持同一个组件在客户端和服务端生成相同的唯一的 ID，避免 hydration 的不匹配，原理就是每个 id 代表该组件在组件树中的层级结构：</font>  


```javascript
function Checkbox() {
  const id = useId()
  return (
    <>
      <label htmlFor={id}>Do you like React?</label>
      <input id={id} type="checkbox" name="react" />
    </>
  )
}
```



1. [React.memo 和性能优化](https://codesandbox.io/s/zujianxiasuoyouzizujianhuifashengchongxinxuanran-bv70e)<font style="color:rgb(31, 35, 40);">。当某个组件状态更新时，它的所有子组件树将会重新渲染。</font>
2. [React.memo 和记忆化数据](https://codesandbox.io/s/reactmemo-and-memo-1dt59)
3. [React.memo 和 React.useMemo 优化性能](https://codesandbox.io/s/reactmemo-and-reactusememo-79txp)
4. [React.memo 和 React.useCallback 优化性能](https://codesandbox.io/s/reactusecallback-and-perf-d3k6s)
5. [React useEffect cleanup](https://codesandbox.io/s/useeffect-clean-i2fi3?file=/src/App.js)<font style="color:rgb(31, 35, 40);">。在这段代码中，示例演示 cleanup 的时机</font>
6. [React 中可以以数组的 index 作为 key 吗?](https://codesandbox.io/s/react-key-shuzudeindex-nl47k)<font style="color:rgb(31, 35, 40);">。在这段代码中，使用 index 作为 key，其中夹杂了 input，引发 bug</font>
7. [React 中以数组的 index 作为 key](https://codesandbox.io/s/index-as-key-yichangshili-pfmpk)<font style="color:rgb(31, 35, 40);">。在这段代码中，使用 index 作为 key，其中夹杂了随机数，引发了 bug</font>
8. [React 兄弟组件通信](https://codesandbox.io/s/react-xiongdizujiantongxin-f2jf6)<font style="color:rgb(31, 35, 40);">。兄弟组件在 React 中如何通信</font>
9. [React 中合成事件](https://codesandbox.io/s/syntheticevent-249x1)<font style="color:rgb(31, 35, 40);">。React 中事件为合成事件，你可以通过</font><font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">e.nativeEvent</font><font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">获取到原生事件，观察</font><font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">e.nativeEvent.currentTarget</font><font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">你将会发现 React 将所有事件都绑定在了</font><font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">#app</font><font style="color:rgb(31, 35, 40);">(React 应用挂载的根组件)</font>
10. [React 中 input.onChange 的原生事件是什么？](https://codesandbox.io/s/input-onchange-1ybhw)<font style="color:rgb(31, 35, 40);">。观察</font><font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">e.nativeEvent.type</font><font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">可知</font>
11. [React hooks 如何实现一个计数器 Counter](https://codesandbox.io/s/shiyong-react-hooks-ruheshixianyigejishuqi-counter-tc5u1)
12. [React FiberNode 数据结构](https://codesandbox.io/s/fibernode-diaoshixinxi-rqt78)<font style="color:rgb(31, 35, 40);">。贯彻</font><font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">element._owner</font><font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">可知 FiberNode 数据结构</font>
13. [React 点击按钮时自增三次](https://codesandbox.io/s/react-setstate-dianjianniuzizengsanci-b4j29)<font style="color:rgb(31, 35, 40);">。此时需使用回调函数，否则会报错</font>
14. [React 不可变数据的必要性](https://codesandbox.io/s/react-bukebianduixiangzhihanshuzujian-zppgm)<font style="color:rgb(31, 35, 40);">。</font>
15. [React 不可变数据的必要性之函数组件](https://codesandbox.io/s/react-todo-setstate-bukebianduixiang-r7qof)<font style="color:rgb(31, 35, 40);">。当在 React hooks 中 setState 两次为相同数据时，不会重新渲染</font>
16. [React 状态批量更新之事件处理](https://codesandbox.io/s/react-state-pilianggengxin-826iv)<font style="color:rgb(31, 35, 40);">。事件处理中的状态会批量更新，减少渲染次数</font>
17. [React 状态批量更新之异步请求](https://codesandbox.io/s/react-state-pilianggengxiner-jzu52)<font style="color:rgb(31, 35, 40);">。异步请求中的状态不会批量更新，将会造成多次渲染</font>
18. [React18 状态批量更新](https://codesandbox.io/s/react18-state-pilianggengxin-75ktu)<font style="color:rgb(31, 35, 40);">。在 React 18 中所有状态将会批量更新</font>
19. [React capture value](https://codesandbox.io/s/react-capture-value-ft06r)

