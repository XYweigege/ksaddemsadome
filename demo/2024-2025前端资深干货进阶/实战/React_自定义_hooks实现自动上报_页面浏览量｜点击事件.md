

<font style="color:rgb(77, 77, 77);">通过自定义 hooks，来 </font>**<font style="color:rgb(77, 77, 77);">控制监听 DOM 元素，分清楚依赖关系</font>**

```jsx
export const LogContext = createContext({});

export const useLog = () => {
  /* 定义一些公共参数 */
  const message = useContext(LogContext);
  const listenDOM = useRef(null);

  /* 分清依赖关系 */
  const reportMessage = useCallback(
    function (data, type) {
      if (type === "pv") {
        // 页面浏览量上报
        console.log("组件 pv 上报", message);
      } else if (type === "click") {
        // 点击上报
        console.log("组件 click 上报", message, data);
      }
    },
    [message]
  );

  useEffect(() => {
    const handleClick = function (e) {
      reportMessage(e.target, "click");
    };
    if (listenDOM.current) {
      listenDOM.current.addEventListener("click", handleClick);
    }

    return function () {
      listenDOM.current &&
        listenDOM.current.removeEventListener("click", handleClick);
    };
  }, [reportMessage]);

  return [listenDOM, reportMessage];
};

```

**<font style="color:rgb(77, 77, 77);">在上面的代码中，使用到了如下4个 React Hooks：</font>**

<br/>color1
+ <font style="color:rgb(79, 79, 79);">使用</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">useContext</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">获取埋点的公共信息，当公共信息改变时，会统一更新。</font>
+ <font style="color:rgb(79, 79, 79);">使用</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">useRef</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">获取 DOM 元素。</font>
+ <font style="color:rgb(79, 79, 79);">使用</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">useCallback</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">缓存上报信息</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">reportMessage</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">方法，里面获取</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">useContext</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">内容。把</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">context</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">作为依赖项，当依赖项发生改变时，重新声明</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">reportMessage</font><font style="color:rgb(79, 79, 79);"> </font><font style="color:rgb(79, 79, 79);">函数。</font>
+ <font style="color:rgb(79, 79, 79);">使用 useEffect 监听 DOM 事件，把 reportMessage 作为依赖项，在 useEffect 中进行事件绑定，返回的销毁函数用于解除绑定。</font>

<br/>

**<font style="color:rgb(77, 77, 77);">依赖关系</font>**<font style="color:rgb(77, 77, 77);">：context 发生改变 -> 让引入 context 的 reportMessage 重新声明 -> 让绑定 DOM 事件监听的 useEffect 里面能够绑定最新的 reportMessage</font>

<font style="color:rgb(77, 77, 77);"></font>

<font style="color:rgb(77, 77, 77);">使用自定义 hooks：</font>

```jsx
import React, { useState } from "react";
import { LogContext, useLog } from "./hooks/useLog";

const Home = () => {
  const [dom, reportMessage] = useLog();
  return (
    <div>
      {/* 监听内部点击 */}
      <div ref={dom}>
        <button> 按钮 1 (内部点击) </button>
        <button> 按钮 2 (内部点击) </button>
        <button> 按钮 3 (内部点击) </button>
      </div>
      {/* 外部点击 */}
      <button
        onClick={() => {
          console.log(reportMessage);
        }}
        >
        外部点击
      </button>
    </div>
  );
};
// 阻断 useState 的更新效应
const Index = React.memo(Home);

const App = () => {
  const [value, setValue] = useState({});
  return (
    <LogContext.Provider value={value}>
      <Index />
      <button onClick={() => setValue({ cat: "小猫", color: "棕色" })}>
        点击
      </button>
    </LogContext.Provider>
  );
};

export default App;
```

<font style="color:rgb(77, 77, 77);">如上，当 context 发生改变时，能够达到正常上报的效果。小细节：使用 React.memo 来阻断 App 组件改变 state 给 Home 组件带来的更新效应。</font>

