# js隔离的实现原理

在微前端架构下，不同的子应用（微应用）运行在同一个主应用的环境下，共享同一个 window 对象、全局事件、全局变量。为了避免相互污染，三大微前端框架（qiankun、Wujie、micro-app）都引入了 JS 隔离机制

 🔹 1️⃣ Qiankun 的 JS 隔离机制 

基于 Proxy 沙箱 和 Snapshot 沙箱。

 🧩 1.1 Proxy 沙箱 

 🔍 1.2 Snapshot 沙箱 

●这是 Qiankun 的另一种隔离模式，适合不支持 Proxy 的低版本浏览器。

●主要通过在挂载和卸载时，保存和恢复 window 的状态：

```

// 激活前保存 window 的状态
const snapshot = { ...window };

// 激活后还原 window 的状态
Object.keys(snapshot).forEach((key) => {
  window[key] = snapshot[key];
});
```





 ✅ 总结 

●主要靠 Proxy 代理 window 对象。

●变量修改、事件注册都在沙箱内部独立存在。

●卸载时自动恢复，防止污染全局。

 🔹 2️⃣ Wujie 的 JS 隔离机制 

基于 iframe 沙箱 和 Proxy 沙箱 的双重隔离。

 🧩 2.1 iframe 沙箱 

●Wujie 的核心隔离是基于 <iframe>。

●<iframe> 天然隔离 JS 作用域、CSS 和 DOM。

●每个子应用在一个独立的 iframe 内执行：

```

<iframe src="http://localhost:7102" sandbox="allow-scripts allow-same-origin"></iframe>
```



●Wujie 会自动设置 <iframe> 的 sandbox 属性，屏蔽跨域访问，防止主应用被修改。

 🧩 2.2 Proxy 沙箱 

●Wujie 内部通过 Proxy 封装了 iframe 的 window，对子应用内部的修改都映射到 iframe 的作用域中。

●示例：

```

const proxyWindow = new Proxy(iframe.contentWindow, {
  get(target, prop) {
    return target[prop];
  },
  set(target, prop, value) {
    target[prop] = value;
    return true;
  },
});
```



 ✅ 总结 

●基于原生 iframe，完全独立的 JS 沙箱。

●通过 Proxy 代理对 window 的操作，避免污染。

●隔离最彻底，适合跨域场景。

 🔹 3️⃣ micro-app 的 JS 隔离机制 

基于 Proxy 沙箱 + Patch DOM API。

 🧩 3.1 Proxy 沙箱 

 🧩 3.2 Patch DOM API 

 ✅ 总结 

●通过 Proxy 实现 JS 隔离，通过 DOM Patch 实现更细粒度的控制。

●自动隔离 DOM 事件，保证不同子应用互不干扰。

●兼容性更好，支持 IE11 以上。

 🔹 三者隔离对比 

| 特性         | Qiankun            | Wujie                       | micro-app             |
| ------------ | ------------------ | --------------------------- | --------------------- |
| 隔离方式     | Proxy 沙箱         | iframe + Proxy 沙箱         | Proxy + Patch DOM API |
| 全局变量隔离 | 强，基于 Proxy     | 最强，基于 iframe 隔离      | 强，基于 Proxy        |
| DOM 操作隔离 | 需要手动 ScopedCSS | iframe 内天然隔离           | 自动 Patch DOM        |
| 事件隔离     | 沙箱内独立         | iframe 内独立               | 自动隔离              |
| 兼容性       | 高（不支持 IE11）  | 中（部分新特性需 Polyfill） | 高（支持 IE11 以上）  |
| 隔离彻底性   | 中                 | 最彻底                      | 高                    |

若有收获，就点个赞吧