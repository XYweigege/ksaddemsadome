<br/>color1
`PM`提的一个需求, 某个`table`被用户输入了一些搜搜条件并且浏览到了第3页, 那如果我跳转到其他路由后返回当前页面, 希望搜索条件还在, 并且仍处于第三页, 这不就是`vue`里面的`keep-alive`标签吗, 但我当前的项目是用`rreact`编写的。

<br/>



### **<font style="color:rgb(0, 0, 0);">一、插件调研</font>**
比较好用的 <font style="color:rgb(51, 51, 51);">react-activation，issues也比较少并且不'致命', 并且可以支持组件级别的缓存( 其实它做不到, 还是有bug ), 我尝试着使用到自己团队的项目里后效果还可以, 但是由于此插件没有大团队支持并且内部全是中文, 最后也没有进行使用</font>

### **<font style="color:rgb(0, 0, 0);">二、核心原理</font>**
<font style="color:rgb(51, 51, 51);"> react的虚拟dom结构是一棵树, 这棵树的某个节点被移除会导致所有子节点也被销毁 所以写代码时才需要用 Memo进行包裹。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539205638-f9ce22bf-c142-47a7-bcbe-42e5477c1a14.png)

<font style="color:rgb(51, 51, 51);">比如我想缓存</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B2组件"</font>`<font style="color:rgb(51, 51, 51);">的状态, 那其实要做的就是让</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B组件"</font>`<font style="color:rgb(51, 51, 51);">被销毁时 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B2组件不被销毁"</font>`<font style="color:rgb(51, 51, 51);">, 从图上可知当</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B组件"</font>`<font style="color:rgb(51, 51, 51);">被销毁时</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"A组件"</font>`<font style="color:rgb(51, 51, 51);">是不会被销毁的, 因为</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"A组件"</font>`<font style="color:rgb(51, 51, 51);">不在</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B组件"</font>`<font style="color:rgb(51, 51, 51);">的下级, 所以我们要做的就是让</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"A组件"</font>`<font style="color:rgb(51, 51, 51);">来生成</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B2组件"</font>`<font style="color:rgb(51, 51, 51);">, 再把</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B2"</font>`<font style="color:rgb(51, 51, 51);">组件插入到</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B组件内部"</font>`<font style="color:rgb(51, 51, 51);">。</font>

<font style="color:rgb(51, 51, 51);">所谓的在"A组件"下渲染, 就是在</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"A组件"</font>`<font style="color:rgb(51, 51, 51);">里面:</font>

```jsx
function A(){
  return (
    <div>
      <B1></B1>
    </div>
  )
}
```

<font style="color:rgb(51, 51, 51);">再使用 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">appendChild</font>`<font style="color:rgb(51, 51, 51);"> 将</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">div</font>`<font style="color:rgb(51, 51, 51);">里面的dom元素全部转移到</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B组件"</font>`<font style="color:rgb(51, 51, 51);">里面即可。</font>

<font style="color:rgb(51, 51, 51);"></font>

### **<font style="color:rgb(0, 0, 0);">三、appendChild后react依然正常执行</font>**
<font style="color:rgb(51, 51, 51);">虽然使用</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">appendChild</font>`<font style="color:rgb(51, 51, 51);">把</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"A组件"</font>`<font style="color:rgb(51, 51, 51);">里面的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">元素插入到</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B组件"</font>`<font style="color:rgb(51, 51, 51);">, 但是</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">react</font>`<font style="color:rgb(51, 51, 51);">内部的各种渲染已经完成, 比如我们在 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B1组件"</font>`<font style="color:rgb(51, 51, 51);"> 内使用 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">useState</font>`<font style="color:rgb(51, 51, 51);"> 定义了一个变量叫 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">'n'</font>`<font style="color:rgb(51, 51, 51);"> , 当 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">'n'</font>`<font style="color:rgb(51, 51, 51);"> 变化时触发的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">变化也都已经被</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">react</font>`<font style="color:rgb(51, 51, 51);">记录, 所以不会影响每次进行</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom diff</font>`<font style="color:rgb(51, 51, 51);"> 后的元素操作。</font>

<font style="color:rgb(51, 51, 51);">并且在</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"A组件"</font>`<font style="color:rgb(51, 51, 51);">下面也可以使用 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"Consumer"</font>`<font style="color:rgb(51, 51, 51);"> 接收到</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"A组件"</font>`<font style="color:rgb(51, 51, 51);">外部的 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"Provider"</font>`<font style="color:rgb(51, 51, 51);">, 但也引出一个问题, 就是如果不是</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"A组件"</font>`<font style="color:rgb(51, 51, 51);">外的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"Provider"</font>`<font style="color:rgb(51, 51, 51);">无法被接收到, 下面是</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">react-actication</font>`<font style="color:rgb(51, 51, 51);">的处理方式:</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539260126-a1822ade-83af-4f8f-b5b9-459552be2fb0.png)

<font style="color:rgb(51, 51, 51);">image.png</font>

<font style="color:rgb(51, 51, 51);">其实这样侵入</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">react</font>`<font style="color:rgb(51, 51, 51);">源代码逻辑的操作还是要慎重, 我们也可以用粗俗一点的方式稍微代替一下, 主要利用 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Provider</font>`<font style="color:rgb(51, 51, 51);"> 可以重复写的特性, 将</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Provider</font>`<font style="color:rgb(51, 51, 51);">与其</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">value</font>`<font style="color:rgb(51, 51, 51);">传入进去实现</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">context</font>`<font style="color:rgb(51, 51, 51);">的正常, 但是这样也显然是不友好的。</font>

<font style="color:rgb(51, 51, 51);">所以 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">react-activation</font>`<font style="color:rgb(51, 51, 51);"> 官网才会注明下面这段话:</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539260071-63ffee02-99fb-435d-bf52-0796183780c0.png)



### **<font style="color:rgb(0, 0, 0);">自己实现的插件的架构设计</font>**
```jsx
const RootComponent: React.FC = () => (
        <KeepAliveProvider>
            <Router>
                <Routes>
                    <Route path={'/'} element={
                          <Keeper cacheId={'home'}> <Home/> </Keeper>
                        }
                    />
                </Routes>
            </Router>
        </KeepAliveProvider>
  )
```

<font style="color:rgb(51, 51, 51);">我们使用 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">KeepAliveProvider</font>`<font style="color:rgb(51, 51, 51);"> 组件来储存需要被缓存的组件的相关信息, 并且用来渲染被缓存的组件, 也就是充当"A组件"的角色。</font>

`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">KeepAliveProvider</font>`<font style="color:rgb(51, 51, 51);">组件内部使用 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Keeper</font>`<font style="color:rgb(51, 51, 51);"> 组件来标记组件应该渲染在哪里? 也就是要用 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Keeper</font>`<font style="color:rgb(51, 51, 51);"> 将</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B1组件"</font>`<font style="color:rgb(51, 51, 51);">+</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B2组件"</font>`<font style="color:rgb(51, 51, 51);">包裹起来, 这样我们就知道初始化好的组件该放到哪里。</font>

`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">cacheId</font>`<font style="color:rgb(51, 51, 51);">也就是缓存的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">id</font>`<font style="color:rgb(51, 51, 51);">, 每个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">id</font>`<font style="color:rgb(51, 51, 51);">对应一个组件的缓存信息, 后续会用来监控每个缓存的组件是否被"激活", 以及清理组件缓存。</font>

### **<font style="color:rgb(0, 0, 0);">KeepAliveProvider开发</font>**
<font style="color:rgb(51, 51, 51);">这里先列出一个"概念代码", 因为直接看完整的代码会晕掉。</font>

```jsx
import CacheContext from './cacheContext'
const KeepAliveProvider: React.FC = (props) => {
 const [catheStates, dispatch]: any = useReducer(cacheReducer, {})
     const mount = useCallback(
        ({ cacheId, reactElement }) => {
            if (!catheStates || !catheStates[cacheId]) {
                dispatch({
                    type: cacheTypes.CREATE,
                    payload: {
                        cacheId,
                        reactElement
                    }
                })
            }
        },
        [catheStates]
    )
 return (
        <CacheContext.Provider value={{ catheStates, mount }}>
            {props.children}
            {Object.values(catheStates).map((item: any) => {
                const { cacheId = '', reactElement } = item
                const cacheState = catheStates[`${cacheId}`];
                const handleDivDom = (divDom: Element) => {
                     const doms = Array.from(divDom.childNodes)
                        if (doms?.length) {
                            dispatch({
                                type: cacheTypes.CREATED,
                                payload: {
                                    cacheId,
                                    doms
                                }
                            })
                        }
                }
                return (
                    <div 
                     key={`${cacheId}`} 
                     id={`cache-外层渲染-${cacheId}`} 
                     ref={(divDom) => divDom && handleDivDom(divDom)}>
                        {reactElement}
                    </div>
        </CacheContext.Provider>
    )
}

export default KeepAliveProvider
```

##### **<font style="color:rgb(0, 0, 0);">代码讲解</font>**
###### **<font style="color:rgb(0, 0, 0);">1. catheStates 存储所有的缓存信息</font>**
<font style="color:rgb(51, 51, 51);">它的数据格式如下:</font>

**<font style="color:rgb(0, 0, 0);"></font>**

```javascript
{
  cacheId: 缓存id,
  reactElement: 真正要渲染的内容,
  status: 状态,
  doms?: dom元素,
 }
```

###### **<font style="color:rgb(0, 0, 0);">2. mount 用来初始化组件</font>**
<font style="color:rgb(51, 51, 51);">将组件状态变为 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">'CREATE'</font>`<font style="color:rgb(51, 51, 51);">, 并且将要渲染的组件储存起来, 就是上图里面</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"B1组件"</font>`<font style="color:rgb(51, 51, 51);">,</font>

```javascript
const mount = useCallback(({ cacheId, reactElement }) => {
            if (!catheStates || !catheStates[cacheId]) {
                dispatch({
                    type: cacheTypes.CREATE,
                    payload: {
                        cacheId,
                        reactElement}
                })
            }
        },
        [catheStates]
    )
```

###### **<font style="color:rgb(0, 0, 0);">3. CacheContext 传递与储存信息</font>**
`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">CacheContext</font>`<font style="color:rgb(51, 51, 51);"> 是我们专门创建用来储存数据的, 他会向各个 Keeper 分发各种方法。</font>



```javascript
import React from "react";
let CacheContext = React.createContext()
export default CacheContext;
```

###### **<font style="color:rgb(0, 0, 0);">4. {props.children} 渲染 KeepAliveProvider 标签中的内容</font>**
###### **<font style="color:rgb(0, 0, 0);">5. div渲染需要缓存的组件</font>**
<font style="color:rgb(51, 51, 51);">这里放一个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">div</font>`<font style="color:rgb(51, 51, 51);">作为渲染组件的</font>[<font style="color:rgb(51, 51, 51);">容器</font>](https://cloud.tencent.com/product/tke?from_column=20065&from=20065)<font style="color:rgb(51, 51, 51);">, 当我们可以获取到这个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">div</font>`<font style="color:rgb(51, 51, 51);">的实例时则对其</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">childNodes</font>`<font style="color:rgb(51, 51, 51);">储存到</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">catheStates</font>`<font style="color:rgb(51, 51, 51);">, 但是这里有个问题, 这种写法只能处理同步渲染的子组件, 如果组件异步渲染则无法储存正确的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">childNodes</font>`<font style="color:rgb(51, 51, 51);">。</font>

###### **<font style="color:rgb(0, 0, 0);">6. 异步渲染的组件</font>**
<font style="color:rgb(51, 51, 51);">假设有如下这种异步的组件, 则无法获取到正确的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">节点, 所以如果</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">childNodes</font>`<font style="color:rgb(51, 51, 51);">为空, 我们需要监听</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">的状态, 当</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">内被插入元素时执行。</font>

 

```javascript
function HomePage() {
    const [show, setShow] = useState(false)
    useEffect(() => {
        setShow(true)
    }, [])
    return show ? <div>home</div>: null;
 }
```

<font style="color:rgb(51, 51, 51);">将handleDivDom方法的代码做一些修改:</font>

 

```javascript
let initDom = false
const handleDivDom = (divDom: Element) => {
    handleDOMCreated()
    !initDom && divDom.addEventListener('DOMNodeInserted', handleDOMCreated)
    function handleDOMCreated() {
        if (!cacheState?.doms) {
            const doms = Array.from(divDom.childNodes)
            if (doms?.length) {
                initDom = true
                dispatch({
                    type: cacheTypes.CREATED,
                    payload: {
                        cacheId,
                        doms
                    }
                })
            }
        }
    }
}
```

<font style="color:rgb(51, 51, 51);">当没有获取到 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">childNodes</font>`<font style="color:rgb(51, 51, 51);"> 则为</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">div</font>`<font style="color:rgb(51, 51, 51);">添加 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"DOMNodeInserted"</font>`<font style="color:rgb(51, 51, 51);">事件, 来监测是否有</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">插入到了</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">div</font>`<font style="color:rgb(51, 51, 51);">内部。</font>

<font style="color:rgb(51, 51, 51);">所以总结来说, 上述代码就是负责了初始化相关数据, 并且负责渲染组件, 但是具体渲染什么组件还需要我们使用</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Keeper</font>`<font style="color:rgb(51, 51, 51);">组件。</font>

### **<font style="color:rgb(0, 0, 0);">编写渲染占位的Keeper</font>**
<font style="color:rgb(51, 51, 51);">在使用插件的时候, 我们实际需要被缓存的组件都是写在</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Keeper</font>`<font style="color:rgb(51, 51, 51);">组件里的, 就像下面这种写法:</font>



```javascript
<Keeper cacheId="home">
  <Home />
  <User />
  <footer>footer</footer>
</Keeper>
```

<font style="color:rgb(51, 51, 51);">此时我们并不要真的在</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Keeper</font>`<font style="color:rgb(51, 51, 51);">组件里面来渲染组件, 把 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">props.children</font>`<font style="color:rgb(51, 51, 51);"> 储存起来, 在</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Keeper</font>`<font style="color:rgb(51, 51, 51);">里面放一个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">div</font>`<font style="color:rgb(51, 51, 51);">来占位, 并且当检测到有数据中有需要被缓存的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">时, 则使用 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">appendChild</font>`<font style="color:rgb(51, 51, 51);"> 把</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">放到自己的内部。</font>



```javascript
import React, { useContext, useEffect } from 'react'
import CacheContext from './cacheContext'

export default function Keeper(props: any) {
    const { cacheId } = props
    const divRef = React.useRef(null)
    const { catheStates, dispatch, mount } = useContext(CacheContext)
    useEffect(() => {
        const catheState = catheStates[cacheId]
        if (catheState && catheState.doms) {
            const doms = catheState.doms
            doms.forEach((dom: any) => {
                (divRef?.current as any)?.appendChild?.dom
            })
        } else {
            mount({
                cacheId,
                reactElement: props.children
            })
        }
    }, [catheStates])
    return <div id={`keeper-原始位置-${cacheId}`} ref={divRef}></div>
}
```

<font style="color:rgb(51, 51, 51);">这里会多出一个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">div</font>`<font style="color:rgb(51, 51, 51);">, 我也没发现太好的办法, 我尝试使用</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">doms</font>`<font style="color:rgb(51, 51, 51);">把这个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">div</font>`<font style="color:rgb(51, 51, 51);">元素替换掉, 这就会导致没有</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">react</font>`<font style="color:rgb(51, 51, 51);">的数据驱动了, 也尝试将这个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);"> 设置 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">"hidden = true"</font>`<font style="color:rgb(51, 51, 51);"> 然后将</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">doms</font>`<font style="color:rgb(51, 51, 51);">插入到这个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">div</font>`<font style="color:rgb(51, 51, 51);">的兄弟节点, 但最后也没成功。</font>

### **<font style="color:rgb(0, 0, 0);">七、Portals属性介绍</font>**
<font style="color:rgb(51, 51, 51);">看到网上有些插件没有使用 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">appendChild</font>`<font style="color:rgb(51, 51, 51);"> 而是使用</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">react</font>`<font style="color:rgb(51, 51, 51);">提供的 来实现的, 感觉挺好玩的就在这里也聊一下。</font>

`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Portal</font>`<font style="color:rgb(51, 51, 51);"> 提供了一种将子节点渲染到存在于父组件以外的 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">DOM</font>`<font style="color:rgb(51, 51, 51);"> 节点的优秀的方案, 直白说就是可以指定我要把 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">child</font>`<font style="color:rgb(51, 51, 51);"> 渲染到哪个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">元素中, 用法如下:</font>



```javascript
ReactDOM.createPortal(child, "目标dom")
```

<font style="color:rgb(119, 119, 119);">react官网是这样描述的: 一个 portal 的典型用例是当父组件有 overflow: hidden 或 z-index 样式时，但你需要子组件能够在视觉上“跳出”其容器。例如，对话框、悬浮卡以及提示框：</font>

<font style="color:rgb(51, 51, 51);">由于这里需要指定在哪里渲染 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">child</font>`<font style="color:rgb(51, 51, 51);">, 所以大需要有明确的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">child</font>`<font style="color:rgb(51, 51, 51);">属性与目标</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">, 但是我们这个插件可能更适合异步操作, 也就是我们只是将数据放在 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">catheStates</font>`<font style="color:rgb(51, 51, 51);"> 里面, 需要取的时候来取, 而不是渲染时就要明确指定的形式来设计。</font>

### **<font style="color:rgb(0, 0, 0);">八、监控缓存被激活</font>**
<font style="color:rgb(51, 51, 51);">我们要实时监控到底哪个组件被"激活", "激活"的定义是组件被初始化后被缓存起来, 之后的每次使用缓存都叫"激活", 并且每次组件被激活调用 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">activeCache</font>`<font style="color:rgb(51, 51, 51);"> 方法来告诉用户当前哪个组件被"激活"了。</font>

<font style="color:rgb(51, 51, 51);">为什么要告诉用户哪个组件被激活了? 大家可以想想这样一个场景, 用户点击了</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">table</font>`<font style="color:rgb(51, 51, 51);">的第三条数据的编辑按钮跳转到编辑页面, 编辑后返回列表页, 此时可能需要我们更新一下列表里第三条的状态, 此时就需要知道哪些组件被激活了。</font>

<font style="color:rgb(51, 51, 51);">还有一种情况如下图所示, 这是一种鼠标悬停会出现</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">tip</font>`<font style="color:rgb(51, 51, 51);">提示语, 如果此时点击按钮发生跳转页面会导致, 当你返回列表页面时这个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">tip</font>`<font style="color:rgb(51, 51, 51);">竟然还在....</font>

<font style="color:rgb(51, 51, 51);">当然我指的不是</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">element-ui</font>`<font style="color:rgb(51, 51, 51);">, 是我们自己的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">ui</font>`<font style="color:rgb(51, 51, 51);">库, 当时看了一下原因, 是因为这个组件只有检测到鼠标离开某些元素才会让</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">tip</font>`<font style="color:rgb(51, 51, 51);">消失, 但是跳页了并且当前页面的所有</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">dom</font>`<font style="color:rgb(51, 51, 51);">被 </font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">keep-alive</font>`<font style="color:rgb(51, 51, 51);">被缓存下来了, 导致了这个</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">tip</font>`<font style="color:rgb(51, 51, 51);">没有被清理。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539386434-df653bc5-529c-4359-88da-b006718d5101.png)

<font style="color:rgb(51, 51, 51);">image.png</font>

<font style="color:rgb(51, 51, 51);">它的代码如下:</font>



```javascript
`    useEffect(() => {
        const catheState = catheStates[cacheId]
        if (catheState && catheState.doms) {
            console.log('激活了:', cacheId)
            activeCache(cacheId)
        }
    }, [])
```

<font style="color:rgb(51, 51, 51);">之所以</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">useEffect</font>`<font style="color:rgb(51, 51, 51);">的参数只传了个空数组, 因为每次组件被"激活"都可以执行, 因为每次</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Keeper</font>`<font style="color:rgb(51, 51, 51);">组件每次会被销毁的, 所以这里可以执行。</font>

###### **<font style="color:rgb(0, 0, 0);">最终使用演示</font>**
<font style="color:rgb(51, 51, 51);">在组件中使用来检测指定的组件是否被更新, 第一个参数是要监测的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">id</font>`<font style="color:rgb(51, 51, 51);">, 也就是</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">Keeper</font>`<font style="color:rgb(51, 51, 51);">身上的</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">cacheId</font>`<font style="color:rgb(51, 51, 51);">, 第二个参数是</font>`<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">callback</font>`<font style="color:rgb(51, 51, 51);">。</font>

<font style="color:rgb(51, 51, 51);">用户使用插件时, 可以在自己的组件内按下面的写法来进行监控:</font>



```javascript
useEffect(() => {
        const cb = () => {
            console.log('home被激活了')
        }
        cacheWatch(['home'], cb)
        return () => {
            removeCacheWatch(['home'], cb)
        }
    }, [])
```

###### **<font style="color:rgb(0, 0, 0);">具体实现</font>**
<font style="color:rgb(51, 51, 51);">在KeepAliveProvider中定义activeCache方法:</font>

<font style="color:rgb(51, 51, 51);">每次激活组件, 就去数组内寻找监听方法进行执行。</font>



```javascript
const [activeCacheObj, setActiveCacheObj] = useState<any>({})
    const activeCache = useCallback(
        (cacheId) => {
            if (activeCacheObj[cacheId]) {
                activeCacheObj[cacheId].forEach((fn: any) => {
                    fn(cacheId)
                })
            }
        },
        [catheStates, activeCacheObj]
    )
```

<font style="color:rgb(51, 51, 51);">添加一个检测方法:</font>

<font style="color:rgb(51, 51, 51);">每次都把callback放到对应的对象身上。</font>

```javascript
const cacheWatch = useCallback(
     (ids: string[], fn) => {
        ids.forEach((id: string) => {
            if (activeCacheObj[id]) {
                activeCacheObj[id].push(fn)
            } else {
                activeCacheObj[id] = [fn]
            }
        })
        setActiveCacheObj({
            ...activeCacheObj
        })
      },
     [activeCacheObj]
    )
```

<font style="color:rgb(51, 51, 51);">还要有一个移除监控的方法:</font>

```javascript
const removeCacheWatch = (ids: string[], fn: any) => {
        ids.forEach((id: string) => {
            if (activeCacheObj[id]) {
                const index = activeCacheObj[id].indexOf(fn)
                activeCacheObj.splice(index, 1)
            }
        })
        setActiveCacheObj({
            ...activeCacheObj
        })
    }
```

<font style="color:rgb(51, 51, 51);">删除缓存的方法, 需要在 cacheReducer 里面增加删除方法, 注意这里需要每个remove所有dom, 而不是仅对 cacheStates 的数据进行删除。</font>



```javascript
case cacheTypes.DESTROY:
    if (cacheStates[payload.cacheId]) {
        const doms = cacheStates?.[payload.cacheId]?.doms
        if (doms) {
            doms.forEach((element) => {
                element.remove()
            })
        }
    }
    delete cacheStates[payload.cacheId]
    return {
        ...cacheStates
    }
```

## <font style="color:rgb(33, 37, 41);">九、保留页面scroll</font>
<font style="color:rgb(33, 37, 41);">比如页面上的</font>`table`<font style="color:rgb(33, 37, 41);">里有</font>`100`<font style="color:rgb(33, 37, 41);">条数据, 我们想看第100条数据, 那就要滚动不少距离, 不少场景这种滚动距离也是有必要保留的。</font>

<font style="color:rgb(33, 37, 41);">这里使用的方法其实比较传统啦, 首先在</font>`KeepAliveProvider`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">下发一个处理滚动的方法:</font>

```javascript
const handleScroll = useCallback(
  (cacheId, event) => {
    if (catheStates?.[cacheId]) {
      const target = event.target
      const scrolls = catheStates[cacheId].scrolls
      scrolls[target] = target.scrollTop
    }
  },
  [catheStates]
)
```

<font style="color:rgb(33, 37, 41);">在</font>`Keeper`<font style="color:rgb(33, 37, 41);">组件里面接收并执行:</font>

```javascript
const { dispatch, mount, handleScroll } = useContext(CacheContext)

useEffect(() => {
  const onScroll = handleScroll.bind(null, cacheId)
    (divRef?.current as any)?.addEventListener?.('scroll', onScroll, true)
  return (divRef?.current as any)?.addEventListener?.('scroll', onScroll, true)
}, [handleScroll])
```

<font style="color:rgb(33, 37, 41);">在Keeper里面将滚动属性赋予元素:</font>

```javascript
useEffect(() => {
  const catheState = catheStates[cacheId]
  if (catheState && catheState.doms) {
    const doms = catheState.doms
    doms.forEach((dom: any) => {
      (divRef?.current as any)?.appendChild?.(dom)
    })

    // 新增
    doms.forEach((dom: any) => {
      if (catheState.scrolls[dom]) {
        dom.scrollTop = catheState.scrolls[dom]
      }
    })
  } else {
    mount({
      cacheId,
      reactElement: props.children
    })
  }
}, [catheStates])
```

<font style="color:rgb(33, 37, 41);">这里如果不主动增加赋予</font>`scroll`<font style="color:rgb(33, 37, 41);">的方法的话, 滚动距离是不会被保存的, 因为</font>`Keeper`<font style="color:rgb(33, 37, 41);">每次都是新的。</font>

## <font style="color:rgb(33, 37, 41);">十、KeepAliveProvider内部 Keeper子组件内部的CacheContext</font>
<font style="color:rgb(33, 37, 41);">我们是把组件渲染在</font><font style="color:rgb(33, 37, 41);"> </font>`KeepAliveProvider`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">里面, 那么如果某个</font>`Provider`<font style="color:rgb(33, 37, 41);">是在</font><font style="color:rgb(33, 37, 41);"> </font>`KeepAliveProvider`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">内部定义的, 则</font>`KeepAliveProvider`<font style="color:rgb(33, 37, 41);">级别的组件是无法使用</font><font style="color:rgb(33, 37, 41);"> </font>`Consumer`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">拿到这个值的。</font>

<font style="color:rgb(33, 37, 41);">这里就引出一个问题, 如何将</font><font style="color:rgb(33, 37, 41);"> </font>`KeepAliveProvider`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">中的组件的上下文, 修改为</font>`Keeper`<font style="color:rgb(33, 37, 41);">组件的上下文。</font>

<font style="color:rgb(33, 37, 41);">这里演示一下最直接的方式, 让用户传入</font>`Provider`<font style="color:rgb(33, 37, 41);">与其</font>`value`<font style="color:rgb(33, 37, 41);">值。</font>

```javascript
<Keeper 
cacheId="home" 
context={{ Provider: Provider, value: value }}>
  <Home />
  </Keeper>
```

<font style="color:rgb(33, 37, 41);">我们拿到这两个值后直接在</font>`Keeper`<font style="color:rgb(33, 37, 41);">中修改</font>`reactElement`<font style="color:rgb(33, 37, 41);">的结构:</font>

```javascript
mount({
  cacheId,
  reactElement: context ? 
    <context.Provider 
    value={context.value}>{props.children}</context.Provider> : 
      props.children
      })
```

<font style="color:rgb(33, 37, 41);">当检测到</font>`context`<font style="color:rgb(33, 37, 41);">有值则直接在</font><font style="color:rgb(33, 37, 41);"> </font>`props.children`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">外面套一层, 当然这里存在一个多层</font>`Provider`<font style="color:rgb(33, 37, 41);">嵌套的问题没有去解决, 因为逐渐复杂起来它的实用性已经在下降了, 接下来还有新的</font>`bug`<font style="color:rgb(33, 37, 41);">来袭。</font>

## <font style="color:rgb(33, 37, 41);">十一、需要传值的组件</font>
<font style="color:rgb(33, 37, 41);">大家有没有发现上述组件所有逻辑, 都是直接写在</font>`Keeper`<font style="color:rgb(33, 37, 41);">标签里面的, 并没有任何的传值, 但是比较常见的一种场景是下面这样的:</font>

```javascript
function Root (){
  const [n, setN] = useState(1)
  return 
  (
    <>
    <button onClick={()=>setN(n+1)}>n+1</button>
    <Keeper>
    <Home n={n} />
    </Keeper>
    </>
  )
}
```

<font style="color:rgb(33, 37, 41);">这个</font>`n`<font style="color:rgb(33, 37, 41);">是</font>`Keeper`<font style="color:rgb(33, 37, 41);">外层传递给</font>`Home`<font style="color:rgb(33, 37, 41);">组件的, 这种写法下会导致</font>`n`<font style="color:rgb(33, 37, 41);">虽然变化了但是</font>`Home`<font style="color:rgb(33, 37, 41);">里面不会响应。</font>

<font style="color:rgb(33, 37, 41);">这个</font>`bug`<font style="color:rgb(33, 37, 41);">我是这样发现的, 当我把这个插件用在我们团队的项目里的一个表格为主的页面时 ,</font><font style="color:rgb(33, 37, 41);"> </font>`table`<font style="color:rgb(33, 37, 41);">一直显示是空的, 并且输入框也无法输入值, 经过测试发现其实值是有变化的, 只是没有展示在组件的</font>`dom`<font style="color:rgb(33, 37, 41);">上。</font>

<font style="color:rgb(33, 37, 41);">尝试了好久后试了下</font>`react-activation`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">很遗憾它也有相同的问题, 那其实就说明这个</font>`bug`<font style="color:rgb(33, 37, 41);">很可能无法解决或者就是这个插件本身的架构存在的问题。</font>

## <font style="color:rgb(33, 37, 41);">十二、为何这么奇怪的bug场景</font>
<font style="color:rgb(33, 37, 41);">当时这个</font>`bug`<font style="color:rgb(33, 37, 41);">折磨了我一天半的时间, 最后定位到外界的传参已经不能算是这个组件本身的参数了, 我们组件的实际渲染位置是</font>` KeepAliveProvider`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">的第一层, 而</font>`Keeper`<font style="color:rgb(33, 37, 41);">的外层还在</font>`KeepAliveProvider`<font style="color:rgb(33, 37, 41);">的更内层, 这就导致这些值的变化其实是没有能够影响到组件。</font>

<font style="color:rgb(33, 37, 41);">可以理解为这些值的变化, 比如</font>`n`<font style="color:rgb(33, 37, 41);">的变化就如同</font>`window.n`<font style="color:rgb(33, 37, 41);">的改变一样,</font><font style="color:rgb(33, 37, 41);"> </font>`react`<font style="color:rgb(33, 37, 41);">组件是不会去响应这个变化的。</font>

<font style="color:rgb(33, 37, 41);">那其实我们要做的就是让外层传入的值的变化, 可以带动组件的样式变化 (逐渐入坑!)。</font>

## <font style="color:rgb(33, 37, 41);">十三、将props单独拿出来</font>
<font style="color:rgb(33, 37, 41);">我借鉴了网上另一种</font>`keep-alive`<font style="color:rgb(33, 37, 41);">组件的写法, 把</font>`Keeper`<font style="color:rgb(33, 37, 41);">组件改为一个</font>`keeper`<font style="color:rgb(33, 37, 41);">的方法, 这个方法返回一个组件看, 这样就可以接收一个</font>`props`<font style="color:rgb(33, 37, 41);">了, 也就把变量圈定在</font>`props`<font style="color:rgb(33, 37, 41);">这个范围:</font>

```jsx
const Home = keeper(HomePage, { cacheId: 'home' })


function Root(){
  const [n, setN] = useState(1)
  return (
    <>
      <button onClick={()=>setN(n+1)}>n+1</button>
      <Home n={n}> // 此处可以传值了
      </>
      )
      }
```

<font style="color:rgb(33, 37, 41);">这样做的目的是让开发者把能够影响组件状态的参数一口气传进来, 比如之前一个</font>`Keeper`<font style="color:rgb(33, 37, 41);">里面可以有多个组件, 这种情况就不好控制哪些参数变化会导致哪些组件更新, 但以组件的方式可以明显得知组件接收到的</font>`props`<font style="color:rgb(33, 37, 41);">里面的值的改变会导致组件更新。</font>

<font style="color:rgb(33, 37, 41);">我想到的方案是, 在</font>`KeepAliveProvider`<font style="color:rgb(33, 37, 41);">里面新建</font>`propsObj`<font style="color:rgb(33, 37, 41);">, 用来专门储存每个缓存组件的</font>`props`<font style="color:rgb(33, 37, 41);">, 之所以如此设计将其单独拿出来, 是要把传参与组件的逻辑拆分开, 不少逻辑会监控</font>`catheStates`<font style="color:rgb(33, 37, 41);">的变化而执行, 但是</font>`props`<font style="color:rgb(33, 37, 41);">的变化没有必要触发这些。</font>

```jsx
const [propsObj, setPropsObj] = useState<any>();
return (
  <CacheContext.Provider value={{ setPropsObj, propsObj }}>
    {props.children}

    //.... 略
```

<font style="color:rgb(33, 37, 41);"></font>`KeepAliveProvider`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">里面的渲染需要变一个形式,</font><font style="color:rgb(33, 37, 41);"> </font>`reactElement`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">变成组件了, 别忘了名字要变成大写的。</font>

```jsx
// 旧的
// {reactElement}

// 新的
{propsObj && 
  <ReactElement {...propsObj[cacheId]}></ReactElement>}
```

<font style="color:rgb(33, 37, 41);">改装一下</font>`Keeper`<font style="color:rgb(33, 37, 41);">文件, 首先要把文件名改为</font><font style="color:rgb(33, 37, 41);"> </font>`keeper`<font style="color:rgb(33, 37, 41);">, 导出的方法要进行一下更改。</font>

```jsx
export default function (
  RealComponent: React.FunctionComponent<any>, { cacheId = '' }) {

    return function Keeper(props: any) {
      // ... 略
```

<font style="color:rgb(33, 37, 41);"></font>`Keeper`<font style="color:rgb(33, 37, 41);">内</font>`mount`<font style="color:rgb(33, 37, 41);">方法的使用也稍作调整:</font>

```jsx
mount({
  cacheId,
  ReactElement: RealComponent
})
```

<font style="color:rgb(33, 37, 41);">关键的来了, 我们要在</font>`Keeper`<font style="color:rgb(33, 37, 41);">里面监测</font>`props`<font style="color:rgb(33, 37, 41);">的变化, 来更新</font>`propsObj`<font style="color:rgb(33, 37, 41);">:</font>

```jsx
const { propsObj, setPropsObj } = useContext(CacheContext)

useEffect(() => {
  setPropsObj({
    ...propsObj,
    [cacheId]: props
  })
}, [props])
```

## <font style="color:rgb(33, 37, 41);">十四、缓存失效的bug</font>
<font style="color:rgb(33, 37, 41);">上述我们已经把插件改装了形式, 并且发现可以让如下场景正常渲染, Home组件的props是外界传入的:</font>

```jsx
const Home = keeper(HomePage, { cacheId: 'home' })

const RootComponent: React.FC = () => {
  return (
    <KeepAliveProvider>
      <Router>
        <Routes>
          <Route path={'/'} element={<Mid />} />
        </Routes>
      </Router>
    </KeepAliveProvider>
  )
}
function Mid() {
  const [n, setN] = useState(1)
  return (
    <div>
      <button onClick={() => setN(n + 1)}>n+1</button>
      <Home n={n}></Home>
    </div>
  )
}

function HomePage(props: { n: number }) {
  return <div>home {props.n}</div>
}
```

<font style="color:rgb(33, 37, 41);">但是此时如果切换页面后再返回</font>`home`<font style="color:rgb(33, 37, 41);">页面,</font><font style="color:rgb(33, 37, 41);"> </font>`home`<font style="color:rgb(33, 37, 41);">页面的缓存是会失效的。</font>

<font style="color:rgb(33, 37, 41);">其实是因为我们实时监控</font>`props`<font style="color:rgb(33, 37, 41);">的变化, 下次重新渲染时会导致</font>`props`<font style="color:rgb(33, 37, 41);">变化, 然后值就会被初始化了, 导致组件也恢复到了早期的配置, 可是.... 这不就是缓存失败了吗?</font>

<font style="color:rgb(33, 37, 41);">每次组件</font>`props`<font style="color:rgb(33, 37, 41);">被重置就会导致组件的相关数据被重置, 尝试把</font>`home`<font style="color:rgb(33, 37, 41);">组件做如下更改:</font>

```jsx
function HomePage(props: { n: number }) {
  const [x, setX] = useState(1)
  return (
    <div>
      <button onClick={() => setX(x + 1)}>x + 1</button>
      <div>home {props.n}</div>
      <div>home: x {x}</div>
    </div>
  )
}
```

<font style="color:rgb(33, 37, 41);">上述写法会导致每次激活</font>`home`<font style="color:rgb(33, 37, 41);">组件, 只能保留</font>`x`<font style="color:rgb(33, 37, 41);">的值,</font><font style="color:rgb(33, 37, 41);"> </font>`n`<font style="color:rgb(33, 37, 41);">的值会与传入的相同。</font>

<font style="color:rgb(33, 37, 41);">这种变化可能会导致</font>`bug`<font style="color:rgb(33, 37, 41);">, 假设只有</font>` n > 2 `<font style="color:rgb(33, 37, 41);">才能让</font>` x > 3`<font style="color:rgb(33, 37, 41);">, 此时我们通过点击事件让</font>` n = 5`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">,</font><font style="color:rgb(33, 37, 41);"> </font>`x = 4`<font style="color:rgb(33, 37, 41);">了, 此时切换到其他页面再回来, 就变成了</font>`n = 1, x=4`<font style="color:rgb(33, 37, 41);">, 违背了我们的初始限制条件, 以此类推在真实复杂的开发环境中此现象会导致各种奇怪的问题。</font>

## <font style="color:rgb(33, 37, 41);">十五、认知的代价</font>
<font style="color:rgb(33, 37, 41);">上面的场景可以通过开发人员自己来控制, 理想情况是</font>`keep-alive`<font style="color:rgb(33, 37, 41);">插件只用来处理不需要外界传参, 以及不会被外界参数的变化影响的组件, 但这就开始麻烦了。</font>

<font style="color:rgb(33, 37, 41);">这类问题导致开发者在插件身上要花的学习成本提高, 使用成本提高, 并且如果某个组件本来不需要传参, 我们用</font>`keep-alive`<font style="color:rgb(33, 37, 41);">包裹起来了, 后续又需要传参了, 改变的成本想想都麻烦。</font>

<font style="color:rgb(33, 37, 41);">网上现有(2022年04月10日17:16:22)组件的官网基本是没有认真的对用户讲述相关的问题, 往往都是以介绍"使用方法"与阐述自己的优势为主, 这就导致用户被莫名其妙的</font>`bug`<font style="color:rgb(33, 37, 41);">折磨。</font>

<font style="color:rgb(33, 37, 41);">传递</font>` Provider`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">的方法也有问题, 需要传递可能不是本页代码的</font>`Provider`<font style="color:rgb(33, 37, 41);">, 难受的了啊。</font>

<font style="color:rgb(33, 37, 41);">想要解决</font>`keep-alive`<font style="color:rgb(33, 37, 41);">相关问题的思路可以换一下, 最好是在</font>`react`<font style="color:rgb(33, 37, 41);">源码里支持一波, 比如可以指定某些组件不被销毁, 其实我们可以关注一下</font>`react18`<font style="color:rgb(33, 37, 41);">的后续版本, 现在这个时间段</font>`react18`<font style="color:rgb(33, 37, 41);">发布了正式版。</font>

## <font style="color:rgb(33, 37, 41);">十六、如何升级到react18</font>
###### <font style="color:rgb(33, 37, 41);">方式一: create-react-app 创建新项目</font>
<font style="color:rgb(33, 37, 41);">现阶段直接使用下面的命令, 就可创建react18项目:</font>

npx create-react-app my_react

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539747250-4e80a542-cc11-4b0d-b379-d2dff18cd8f2.png)



<font style="color:rgb(33, 37, 41);">下面这种使用</font><font style="color:rgb(33, 37, 41);"> </font>`--template`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">指定模板的还不行, 因为模板代码还没更新:</font>

```jsx
npx create-react-app my_react --template typescript
```

<font style="color:rgb(33, 37, 41);">这里可以查看所有react项目的模板</font><font style="color:rgb(33, 37, 41);"> </font>[<font style="color:rgb(33, 37, 41);">create-react-app项目可指定的模板</font>](https://link.segmentfault.com/?enc=DhIcAbIOIMH1qNXQOjUQVg%3D%3D.VfQ93tZ3q2fGhQK3z2%2F96wOsNHsSY86PKiuSvBJED0e%2B8Wo6WKoOhNJ6vG1LNPN7)<font style="color:rgb(33, 37, 41);">。</font>

###### <font style="color:rgb(33, 37, 41);">方式二: 老项目改装</font>
<font style="color:rgb(33, 37, 41);">首先直接把依赖里面的</font>`react`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">与</font><font style="color:rgb(33, 37, 41);"> </font>`react-dom`<font style="color:rgb(33, 37, 41);">的版本号改成</font><font style="color:rgb(33, 37, 41);"> </font>`"^18.0.0"`<font style="color:rgb(33, 37, 41);">即可。</font>

###### <font style="color:rgb(33, 37, 41);">两种方式都需要修改 index.js</font>
<font style="color:rgb(33, 37, 41);">启动项目会有报错信息:</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539773329-9f8dc527-da6d-4879-9876-ff88b2788760.png)<font style="color:rgb(33, 37, 41);">  
</font>

<font style="color:rgb(33, 37, 41);">旧版的</font>`index.js`<font style="color:rgb(33, 37, 41);">  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539785013-2a8631b3-7979-4871-8af7-d7c2ec1167c6.png)

<font style="color:rgb(33, 37, 41);">新版的</font>`index.js`

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539796326-06d70404-3fd4-4352-8c0a-2486a3a95f90.png)

<font style="color:rgb(33, 37, 41);">其他的没有太多更改了。</font>

## <font style="color:rgb(33, 37, 41);">十七、react18 Offscreen 组件的用法</font>
`Offscreen`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">允许</font><font style="color:rgb(33, 37, 41);"> </font>`React`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">通过隐藏组件而不是卸载组件来保持这样的状态,</font><font style="color:rgb(33, 37, 41);"> </font>`React`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">将调用与卸载时相同的生命周期钩子, 但它也会保留</font><font style="color:rgb(33, 37, 41);"> </font>`React`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">组件和</font><font style="color:rgb(33, 37, 41);"> </font>`DOM`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">元素的状态。</font>

`React Activatio`<font style="color:rgb(33, 37, 41);">n 中也推荐大家关注这个属性:</font>



![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539806188-e2b58089-8017-4ae9-bdfe-26c989069efe.png)

`Offscreen`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">是什么的官方说法可以看这篇文章里的翻译:</font><font style="color:rgb(33, 37, 41);"> </font>[<font style="color:rgb(33, 37, 41);">React v18.0新特性官方文档[中英文对照</font>](https://link.segmentfault.com/?enc=NJW5fS9yI3PodikEOtvL%2FQ%3D%3D.fmiZ%2BLimj%2FnHg3qZvXx1aFMqAHeDqawIb0r8K8yZEHOjQKJaG0HUutevMB%2FXlU%2FJ)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539813969-517f74f3-5a1c-4341-864d-a2720bd52b47.png)

`Offscreen`<font style="color:rgb(33, 37, 41);">的测试用例:</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539825779-c7872319-8f3b-4f70-960f-73da4164e71e.png)

<font style="color:rgb(33, 37, 41);">遗憾的是</font>` Offscreen`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">组件并没有在当前版本推出, 其还处于不稳定阶段, 但我们可以通过</font><font style="color:rgb(33, 37, 41);"> </font>`react18`<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">里面的测试用例来预览一下其用法:</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723539846735-f6983028-314b-4717-bb93-2a25547ba7c2.png)

<font style="color:rgb(33, 37, 41);">通过上述写法还无法看出</font>` Offscreen `<font style="color:rgb(33, 37, 41);">到底如何使用, 只知道它可能是以组件的形式出现, 并且需要传入一个mode属性, 更多用法期待官方尽快推出吧。</font>

<font style="color:rgb(51, 51, 51);"></font>

<font style="color:rgb(51, 51, 51);"></font>

