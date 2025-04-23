## 一、React Router基础之history
### [](https://www.123fe.net/principle-docs/react/01-React%20router%E5%8E%9F%E7%90%86.html#_1-1-history%E4%BB%8B%E7%BB%8D)1.1 History介绍
<font style="background-color:rgb(249, 242, 244);">history</font><font style="background-color:rgb(255, 249, 249);">是一个独立的第三方js库，可以用来兼容在不同浏览器、不同环境下对历史记录的管理，拥有统一的</font><font style="background-color:rgb(249, 242, 244);">API</font><font style="background-color:rgb(255, 249, 249);">。具体来说里面的</font><font style="background-color:rgb(249, 242, 244);">history</font><font style="background-color:rgb(255, 249, 249);">分为三类</font>

+ 老浏览器的<font style="background-color:rgb(249, 242, 244);">history</font>: 主要通过<font style="background-color:rgb(249, 242, 244);">hash</font>来实现，对应<font style="background-color:rgb(249, 242, 244);">createHashHistory</font>
+ 高版本浏览器: 通过<font style="background-color:rgb(249, 242, 244);">html5</font>里面的<font style="background-color:rgb(249, 242, 244);">history</font>，对应<font style="background-color:rgb(249, 242, 244);">createBrowserHistory</font>
+ <font style="background-color:rgb(249, 242, 244);">node</font>环境下: 主要存储在<font style="background-color:rgb(249, 242, 244);">memeory</font>里面，对应<font style="background-color:rgb(249, 242, 244);">createMemoryHistory</font>

<font style="background-color:rgb(255, 249, 249);">上面针对不同的环境提供了三个</font><font style="background-color:rgb(249, 242, 244);">API</font><font style="background-color:rgb(255, 249, 249);">，但是三个</font><font style="background-color:rgb(249, 242, 244);">API</font><font style="background-color:rgb(255, 249, 249);">有一些共性的操作，将其抽象了一个公共的文件</font><font style="background-color:rgb(249, 242, 244);">createHistory</font>

```javascript
// 内部的抽象实现
function createHistory(options={}) {
  ...
  return {
    listenBefore, // 内部的hook机制，可以在location发生变化前执行某些行为，AOP的实现
    listen, // location发生改变时触发回调
    transitionTo, // 执行location的改变
    push, // 改变location
    replace,
    go,
    goBack,
    goForward,
    createKey, // 创建location的key，用于唯一标示该location，是随机生成的
    createPath,
    createHref,
    createLocation, // 创建location
  }
}
```

<font style="background-color:rgb(255, 249, 249);">上述这些方式是</font><font style="background-color:rgb(249, 242, 244);">history</font><font style="background-color:rgb(255, 249, 249);">内部最基础的方法，</font><font style="background-color:rgb(249, 242, 244);">createHashHistory</font><font style="background-color:rgb(255, 249, 249);">、</font><font style="background-color:rgb(249, 242, 244);">createBrowserHistory</font><font style="background-color:rgb(255, 249, 249);">、</font><font style="background-color:rgb(249, 242, 244);">createMemoryHistory</font><font style="background-color:rgb(255, 249, 249);">只是覆盖其中的某些方法而已。其中需要注意的是，此时的</font><font style="background-color:rgb(249, 242, 244);">location</font><font style="background-color:rgb(255, 249, 249);">跟浏览器原生的</font><font style="background-color:rgb(249, 242, 244);">location</font><font style="background-color:rgb(255, 249, 249);">是不相同的，最大的区别就在于里面多了</font><font style="background-color:rgb(249, 242, 244);">key</font><font style="background-color:rgb(255, 249, 249);">字段，</font><font style="background-color:rgb(249, 242, 244);">history</font><font style="background-color:rgb(255, 249, 249);">内部通过</font><font style="background-color:rgb(249, 242, 244);">key</font><font style="background-color:rgb(255, 249, 249);">来进行</font><font style="background-color:rgb(249, 242, 244);">location</font><font style="background-color:rgb(255, 249, 249);">的操作</font>

```javascript
function createLocation() {
  return {
    pathname, // url的基本路径
    search, // 查询字段
    hash, // url中的hash值
    state, // url对应的state字段
    action, // 分为 push、replace、pop三种
    key // 生成方法为: Math.random().toString(36).substr(2, length)
  }
}
```

### [](https://www.123fe.net/principle-docs/react/01-React%20router%E5%8E%9F%E7%90%86.html#_1-2-%E5%86%85%E9%83%A8%E8%A7%A3%E6%9E%90)1.2 内部解析
<font style="background-color:rgb(255, 249, 249);">三个</font><font style="background-color:rgb(249, 242, 244);">API</font><font style="background-color:rgb(255, 249, 249);">的大致的技术实现如下</font>

+ <font style="background-color:rgb(249, 242, 244);">createBrowserHistory</font>: 利用<font style="background-color:rgb(249, 242, 244);">HTML5</font>里面的<font style="background-color:rgb(249, 242, 244);">history</font>
+ <font style="background-color:rgb(249, 242, 244);">createHashHistory</font>: 通过<font style="background-color:rgb(249, 242, 244);">hash</font>来存储在不同状态下的<font style="background-color:rgb(249, 242, 244);">history</font>信息
+ <font style="background-color:rgb(249, 242, 244);">createMemoryHistory</font>: 在内存中进行历史记录的存储`

#### [](https://www.123fe.net/principle-docs/react/01-React%20router%E5%8E%9F%E7%90%86.html#_1-2-1-%E6%89%A7%E8%A1%8Curl%E5%89%8D%E8%BF%9B)1.2.1 执行URL前进
+ <font style="background-color:rgb(249, 242, 244);">createBrowserHistory</font>: <font style="background-color:rgb(249, 242, 244);">pushState</font>、<font style="background-color:rgb(249, 242, 244);">replaceState</font>
+ <font style="background-color:rgb(249, 242, 244);">createHashHistory</font>: <font style="background-color:rgb(249, 242, 244);">location.hash=*** location.replace()</font>
+ <font style="background-color:rgb(249, 242, 244);">createMemoryHistory</font>: 在内存中进行历史记录的存储

```javascript
// 伪代码

// createBrowserHistory(HTML5)中的前进实现
function finishTransition(location) {
  ...
  const historyState = { key };
  ...
  if (location.action === 'PUSH') ) {
    window.history.pushState(historyState, null, path);
  } else {
    window.history.replaceState(historyState, null, path)
  }
}
// createHashHistory的内部实现
function finishTransition(location) {
  ...
  if (location.action === 'PUSH') ) {
    window.location.hash = path;
  } else {
    window.location.replace(
      window.location.pathname + window.location.search + '' + path
    );
  }
}
// createMemoryHistory的内部实现
entries = [];
function finishTransition(location) {
  ...
  switch (location.action) {
    case 'PUSH':
      entries.push(location);
      break;
    case 'REPLACE':
      entries[current] = location;
      break;
  }
```

#### [](https://www.123fe.net/principle-docs/react/01-React%20router%E5%8E%9F%E7%90%86.html#_1-2-2-%E6%A3%80%E6%B5%8Burl%E5%9B%9E%E9%80%80)1.2.2 检测URL回退
+ <font style="background-color:rgb(249, 242, 244);">createBrowserHistory</font>: <font style="background-color:rgb(249, 242, 244);">popstate</font>
+ <font style="background-color:rgb(249, 242, 244);">createHashHistory</font>: <font style="background-color:rgb(249, 242, 244);">hashchange</font>
+ <font style="background-color:rgb(249, 242, 244);">createMemoryHistory</font>:因为是在内存中操作，跟浏览器没有关系，不涉及<font style="background-color:rgb(249, 242, 244);">UI</font>层面的事情，所以可以直接进行历史信息的回退

```plain
// 伪代码

// createBrowserHistory(HTML5)中的后退检测
function startPopStateListener({ transitionTo }) {
  function popStateListener(event) {
    ...
    transitionTo( getCurrentLocation(event.state) );
  }
  addEventListener(window, 'popstate', popStateListener);
  ...
}
 
// createHashHistory的后退检测
function startPopStateListener({ transitionTo }) {
  function hashChangeListener(event) {
    ...
    transitionTo( getCurrentLocation(event.state) );
  }
  addEventListener(window, 'hashchange', hashChangeListener);
  ...
}
// createMemoryHistory的内部实现
function go(n) {
  if (n) {
    ...
    current += n;
  const currentLocation = getCurrentLocation();
  // change action to POP
  history.transitionTo({ ...currentLocation, action: POP });
  }
}
```

#### [](https://www.123fe.net/principle-docs/react/01-React%20router%E5%8E%9F%E7%90%86.html#_1-2-3-state%E7%9A%84%E5%AD%98%E5%82%A8)1.2.3 state的存储
<font style="background-color:rgb(255, 249, 249);">为了维护</font><font style="background-color:rgb(249, 242, 244);">state</font><font style="background-color:rgb(255, 249, 249);">的状态，将其存储在</font><font style="background-color:rgb(249, 242, 244);">sessionStorage</font><font style="background-color:rgb(255, 249, 249);">里面:</font>

```javascript
// createBrowserHistory/createHashHistory中state的存储
function saveState(key, state) {
  ...
  window.sessionStorage.setItem(createKey(key), JSON.stringify(state));
}
function readState(key) {
  ...
  json = window.sessionStorage.getItem(createKey(key));
  return JSON.parse(json);
}
// createMemoryHistory仅仅在内存中，所以操作比较简单
const storage = createStateStorage(entries); // storage = {entry.key: entry.state}

function saveState(key, state) {
  storage[key] = state
}
function readState(key) {
  return storage[key]
}
```

### [](https://www.123fe.net/principle-docs/react/01-React%20router%E5%8E%9F%E7%90%86.html#_1-3-%E5%B0%8F%E7%BB%93)1.3 小结
**路由原理**

<font style="background-color:rgb(255, 249, 249);">前端路由实现起来其实很简单，本质就是监听</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">URL</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">的变化，然后匹配路由规则，显示相应的页面，并且无须刷新。目前单页面使用的路由就只有两种实现方式</font>

+ <font style="background-color:rgb(249, 242, 244);">hash</font> 模式
+ <font style="background-color:rgb(249, 242, 244);">history</font> 模式
+ <font style="background-color:rgb(249, 242, 244);">www.test.com//</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">就是</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">Hash URL</font><font style="background-color:rgb(255, 249, 249);">，当</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);"></font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">后面的哈希值发生变化时，不会向服务器请求数据，可以通过</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">hashchange</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">事件来监听到</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">URL</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">的变化，从而进行跳转页面。</font>
+ <font style="background-color:rgb(249, 242, 244);">History</font><font style="background-color:rgb(255, 249, 249);">模式是</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">HTML5</font><font style="background-color:rgb(255, 249, 249);">新推出的功能，比之</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">Hash URL</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">更加美观</font>

## [](https://www.123fe.net/principle-docs/react/01-React%20router%E5%8E%9F%E7%90%86.html#%E4%BA%8C%E3%80%81react-router%E7%9A%84%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86)二、react-router的基本原理
<font style="background-color:rgb(255, 249, 249);">实现</font><font style="background-color:rgb(249, 242, 244);">URL</font><font style="background-color:rgb(255, 249, 249);">与</font><font style="background-color:rgb(249, 242, 244);">UI</font><font style="background-color:rgb(255, 249, 249);">界面的同步。其中在</font><font style="background-color:rgb(249, 242, 244);">react-router</font><font style="background-color:rgb(255, 249, 249);">中，</font><font style="background-color:rgb(249, 242, 244);">URL</font><font style="background-color:rgb(255, 249, 249);">对应</font><font style="background-color:rgb(249, 242, 244);">Location</font><font style="background-color:rgb(255, 249, 249);">对象，而</font><font style="background-color:rgb(249, 242, 244);">UI</font><font style="background-color:rgb(255, 249, 249);">是由</font><font style="background-color:rgb(249, 242, 244);">react components</font><font style="background-color:rgb(255, 249, 249);">来决定的，这样就转变成</font><font style="background-color:rgb(249, 242, 244);">location</font><font style="background-color:rgb(255, 249, 249);">与</font><font style="background-color:rgb(249, 242, 244);">components</font><font style="background-color:rgb(255, 249, 249);">之间的同步问题</font>

### [](https://www.123fe.net/principle-docs/react/01-React%20router%E5%8E%9F%E7%90%86.html#_2-1-%E4%BC%98%E7%82%B9)2.1 优点
+ 与<font style="background-color:rgb(249, 242, 244);">React</font>融为一体,专为<font style="background-color:rgb(249, 242, 244);">react</font>量身打造，编码风格与<font style="background-color:rgb(249, 242, 244);">react</font>保持一致，例如路由的配置可以通过<font style="background-color:rgb(249, 242, 244);">component</font>来实现
+ 不需要手工维护路由<font style="background-color:rgb(249, 242, 244);">state</font>，使代码变得简单
+ 强大的路由管理机制，体现在如下方面
    - 路由配置: 可以通过组件、配置对象来进行路由的配置
    - 路由切换: 可以通过<font style="background-color:rgb(249, 242, 244);"><Link></font> <font style="background-color:rgb(249, 242, 244);">Redirect</font>进行路由的切换
    - 路由加载: 可以同步记载，也可以异步加载，这样就可以实现按需加载
+ 使用方式: 不仅可以在浏览器端的使用，而且可以在服务器端的使用

### [](https://www.123fe.net/principle-docs/react/01-React%20router%E5%8E%9F%E7%90%86.html#_2-2-react-router%E5%85%B7%E4%BD%93%E5%AE%9E%E7%8E%B0)2.2 react-router具体实现
<font style="background-color:rgb(249, 242, 244);">react-router</font><font style="background-color:rgb(255, 249, 249);">在</font><font style="background-color:rgb(249, 242, 244);">history</font><font style="background-color:rgb(255, 249, 249);">库的基础上，实现了</font><font style="background-color:rgb(249, 242, 244);">URL</font><font style="background-color:rgb(255, 249, 249);">与</font><font style="background-color:rgb(249, 242, 244);">UI</font><font style="background-color:rgb(255, 249, 249);">的同步，分为两个层次来描述具体的实现。</font>

**组件层面描述实现过程**

<font style="background-color:rgb(255, 249, 249);">在</font><font style="background-color:rgb(249, 242, 244);">react-router</font><font style="background-color:rgb(255, 249, 249);">中最主要的</font><font style="background-color:rgb(249, 242, 244);">component</font><font style="background-color:rgb(255, 249, 249);">是</font><font style="background-color:rgb(249, 242, 244);">Router RouterContext Link</font><font style="background-color:rgb(255, 249, 249);">，</font><font style="background-color:rgb(249, 242, 244);">history</font><font style="background-color:rgb(255, 249, 249);">库起到了中间桥梁的作用</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871407561-13085221-b9c5-43c3-ac98-10f4836ba37f.png)

<font style="background-color:rgb(255, 249, 249);">以</font><font style="background-color:rgb(249, 242, 244);">browserHistory</font><font style="background-color:rgb(255, 249, 249);">(一种</font><font style="background-color:rgb(249, 242, 244);">history</font><font style="background-color:rgb(255, 249, 249);">类型:一个</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">history</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">知道如何去监听浏览器地址栏的变化， 并解析这个</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">URL</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">转化为</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">location</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">对象)为例 :</font>

+ <font style="background-color:rgb(249, 242, 244);">browserHistory</font>进行路由<font style="background-color:rgb(249, 242, 244);">state</font>管理,主要通过<font style="background-color:rgb(249, 242, 244);">sessionStorage</font>

```javascript
//保存　路由state(router state)
function saveState(key, state) {
  ...
  window.sessionStorage.setItem(createKey(key), JSON.stringify(state));
}
//读取路由state
function readState(key) {
  ...
  json = window.sessionStorage.getItem(createKey(key));
  return JSON.parse(json);
}
```

<font style="background-color:rgb(255, 249, 249);">其中</font><font style="background-color:rgb(249, 242, 244);">saveState</font><font style="background-color:rgb(255, 249, 249);">函数传进来的</font><font style="background-color:rgb(249, 242, 244);">state</font><font style="background-color:rgb(255, 249, 249);">是个</font><font style="background-color:rgb(249, 242, 244);">json</font><font style="background-color:rgb(255, 249, 249);">对象，如：</font>

```plain
{route: '/about'} ///假设此时的location为'/about'
```

<font style="background-color:rgb(255, 249, 249);">进行路由匹配，最终渲染对应的组件</font>

```javascript
const About = React.createClass({/*...*/}) //About 组件
const Inbox = React.createClass({/*...*/}) //Inbox 组件
const Home = React.createClass({/*...*/}) //Home组件
render() {
  let Child
  switch (this.state.route) {
    case '/about': Child = About; break;
    case '/inbox': Child = Inbox; break;
    default:      Child = Home;
  }

  return (
    <div>
    <h1>App</h1>
    <ul>
    <li><a href="/about">About</a></li>
    <li><a href="/inbox">Inbox</a></li>
    </ul>
    <Child/>
    </div>
  )
}
})

React.render(<App />, document.body)
```

**API层面描述实现过程**

<font style="background-color:rgb(255, 249, 249);">为了简单说明，只描述使用</font><font style="background-color:rgb(249, 242, 244);">browserHistory</font><font style="background-color:rgb(255, 249, 249);">的实现，</font><font style="background-color:rgb(249, 242, 244);">hashHistory</font><font style="background-color:rgb(255, 249, 249);">的实现过程是类似的，就不在说明</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871407588-ece1b7a7-b295-47ee-a6d4-8b9ddbabf77e.png)

### [](https://www.123fe.net/principle-docs/react/01-React%20router%E5%8E%9F%E7%90%86.html#_2-3-%E7%94%A8%E6%88%B7%E7%82%B9%E5%87%BB%E4%BA%86link%E7%BB%84%E4%BB%B6%E5%90%8E%E8%B7%AF%E7%94%B1%E7%B3%BB%E7%BB%9F%E4%B8%AD%E5%88%B0%E5%BA%95%E5%8F%91%E7%94%9F%E4%BA%86%E5%93%AA%E4%BA%9B%E5%8F%98%E5%8C%96)2.3 用户点击了Link组件后路由系统中到底发生了哪些变化
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871406539-9c63e95b-543a-4ad1-a047-0575e64b7cef.png)

<font style="background-color:rgb(249, 242, 244);">Link</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">组件最终会渲染为</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">HTML</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">标签</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);"><a></font><font style="background-color:rgb(255, 249, 249);">，它的</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">to</font><font style="background-color:rgb(255, 249, 249);">、</font><font style="background-color:rgb(249, 242, 244);">query</font><font style="background-color:rgb(255, 249, 249);">、</font><font style="background-color:rgb(249, 242, 244);">hash</font><font style="background-color:rgb(255, 249, 249);">属性会被组合在一起并渲染为</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">href</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">属性。虽然</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">Link</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">被渲染为超链接，但在内部实现上使用脚本拦截了浏览器的默认行为，然后调用了</font><font style="background-color:rgb(249, 242, 244);">history.pushState</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">方法</font>

+ 系统会将上述 <font style="background-color:rgb(249, 242, 244);">location</font>对象作为参数传入到 <font style="background-color:rgb(249, 242, 244);">TransitionTo</font>方法中，然后调用 <font style="background-color:rgb(249, 242, 244);">window.location.hash</font> 或者<font style="background-color:rgb(249, 242, 244);">window.history.pushState()</font> 修改了应用的 <font style="background-color:rgb(249, 242, 244);">URL</font>，这取决于你创建<font style="background-color:rgb(249, 242, 244);">history</font>对象的方式。同时会触发<font style="background-color:rgb(249, 242, 244);">history.listen</font> 中注册的事件监听器。
+ 接下来请看路由系统内部是如何修改<font style="background-color:rgb(249, 242, 244);">UI</font> 的。在得到了新的<font style="background-color:rgb(249, 242, 244);">location</font>对象后，系统内部的 <font style="background-color:rgb(249, 242, 244);">matchRoutes</font> 方法会匹配出<font style="background-color:rgb(249, 242, 244);">Route</font> 组件树中与当前<font style="background-color:rgb(249, 242, 244);">location</font>对象匹配的一个子集，并且得到了 <font style="background-color:rgb(249, 242, 244);">nextState</font>，具体的匹配算法不在这里讲解，感兴趣的同学可以点击查看，<font style="background-color:rgb(249, 242, 244);">state</font> 的结构如下

```plain
nextState = {
  location, // 当前的 location 对象
  routes, // 与 location 对象匹配的 Route 树的子集，是一个数组
  params, // 传入的 param，即 URL 中的参数
  components, // routes 中每个元素对应的组件，同样是数组
};
```

<font style="background-color:rgb(255, 249, 249);">在</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">Router</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">组件的</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">componentWillMount</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">生命周期方法中调用了</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">history.listen(listener)</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">方法。</font><font style="background-color:rgb(249, 242, 244);">listener</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">会在上述</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">matchRoutes</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">方法执行成功后执行</font><font style="background-color:rgb(249, 242, 244);">listener(nextState)</font><font style="background-color:rgb(255, 249, 249);">，</font><font style="background-color:rgb(249, 242, 244);">nextState</font><font style="background-color:rgb(255, 249, 249);">对象每个属性的具体含义已经在上述代码中注释，接下来执行</font><font style="background-color:rgb(249, 242, 244);">this.setState(nextState)</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">就可以实现重新渲染</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">Router</font><font style="background-color:rgb(255, 249, 249);">组件。举个简单的例子，当 URL（准确的说应该是</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">location.pathname</font><font style="background-color:rgb(255, 249, 249);">） 为</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">/archives/posts</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">时，应用的匹配结果如下图所示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718871406306-8ec95e67-5593-4daa-a7a3-2f8efd6a226c.png)

<font style="background-color:rgb(255, 249, 249);">到这里，系统已经完成了当用户点击一个由 </font><font style="background-color:rgb(249, 242, 244);">Link</font><font style="background-color:rgb(255, 249, 249);"> 组件渲染出的超链接到页面刷新的全过程</font>

## 
