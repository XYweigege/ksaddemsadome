 

# WuJie无界微前端使用和问题解决

[llsydn](/user/1680466685209543/posts)

2023-08-02 18,965 阅读5分钟

关注

我正在参加[「金石计划5.0」](https://juejin.cn/post/7262228689806245947)

# 一、WuJie无界微前端

## 1.WuJie说明

说明文档：[wujie-micro.github.io/doc/guide/](https://link.juejin.cn?target=https%3A%2F%2Fwujie-micro.github.io%2Fdoc%2Fguide%2F "https://wujie-micro.github.io/doc/guide/")

微前端是一种多个团队通过独立发布功能的方式来共同构建现代化 web 应用的技术手段及方法策略。

通俗来说，就是在一个`web`应用中可以独立的运行另一个`web`应用

微前端有什么使用场景呢？

> 举例
> 
> * 比如制作一个企业管理平台，把已有的采购系统和财务系统统一接入这个平台；
> * 比如有一个巨大的应用，为了降低开发和维护成本，分拆成多个小应用进行开发和部署，然后用一个平台将这些小应用集成起来；
> * 又比如一个应用使用`vue`框架开发，其中有一个比较独立的模块，开发者想尝试使用`react`框架来开发，等模块单独开发部署完，再把这个模块应用接回去

可能有人会有疑问直接使用`iframe`不就可以做到吗？

采用`iframe`的方案确实可以做到，而且优点非常明显

> 优点
> 
> * 非常简单，使用没有任何心智负担
> * `web`应用隔离的非常完美，无论是`js`、`css`、`dom`都完全隔离开来

由于其**隔离的太完美**导致缺点也非常明显

> 缺点
> 
> * 路由状态丢失，刷新一下，`iframe`的`url`状态就丢失了
> * `dom`割裂严重，弹窗只能在`iframe`内部展示，无法覆盖全局
> * `web`应用之间通信非常困难
> * 每次打开白屏时间太长，对于[SPA 应用](https://link.juejin.cn?target=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E5%258D%2595%25E9%25A1%25B5%25E5%25BA%2594%25E7%2594%25A8 "https://zh.wikipedia.org/wiki/%E5%8D%95%E9%A1%B5%E5%BA%94%E7%94%A8")来说无法接受

`WuJie无界`的方案，就是为了解决iframe存在的问题，而且还能带来更多的优势，详情可以查看`WuJie无界`官方指导文档。

## 2.WuJie快速上手

### 2.1主应用

1.安装wujie依赖

```bash
# vue2 框架
npm i wujie-vue2 -S

# vue3 框架
# npm i wujie-vue3 -S

```

> 注意：使用vue的框架，是2还是3

2.main.js引入wujie

```javascript
//引入无界微前端
// vue2
import WujieVue from "wujie-vue2";
// vue3
// import WujieVue from "wujie-vue3";

import lifecycles from "../config/lifecycle";
const { setupApp, preloadApp, bus } = WujieVue;
Vue.use(WujieVue);
const props = {
    jump: (name) => {
        router.push({ name });
    },
};

//设置子应用demo
setupApp({
    name: "demo",
    url: "/demo/home/index",
    exec: true,
    props,
    fetch: function fetch(url, options) {
        //console.log("fetch.url:=",url,options)
        return window.fetch(url, { ...options, credentials: "omit" });
    },
    ...lifecycles,
});

// 预加载
//preloadApp({
//    name: "demo",
//});

```

> 注意：**setupApp** 设置子应用的`name`和 **preloadApp** 设置的`name`要一致。
> 
> 预加载，可以极大的提升子应用首次打开速度
> 
> * 资源的预加载会占用主应用的网络线程池
> * 资源的预执行会阻塞主应用的渲染线程

> 一般来说，如果不是很介意打开的首屏时间，这里预加载可以不用加。

* lifecycle.js

```javascript
const lifecycles = {
    beforeLoad: (appWindow) => console.log(`${appWindow.__WUJIE.id} beforeLoad 生命周期`),
    beforeMount: (appWindow) => console.log(`${appWindow.__WUJIE.id} beforeMount 生命周期`),
    afterMount: (appWindow) => console.log(`${appWindow.__WUJIE.id} afterMount 生命周期`),
    beforeUnmount: (appWindow) => console.log(`${appWindow.__WUJIE.id} beforeUnmount 生命周期`),
    afterUnmount: (appWindow) => console.log(`${appWindow.__WUJIE.id} afterUnmount 生命周期`),
    activated: (appWindow) => console.log(`${appWindow.__WUJIE.id} activated 生命周期`),
    deactivated: (appWindow) => console.log(`${appWindow.__WUJIE.id} deactivated 生命周期`),
    loadError: (url, e) => console.log(`${url} 加载失败`, e),
};
export default lifecycles;

```

> 这个是生命周期函数

无界提供一整套的生命周期钩子供开发者调用

如果子应用没有做[生命周期的改造](#lifecycle)，那么 beforeMount、afterMount、beforeUnmount、afterUnmount 这四个生命周期将不会调用

### 2.2子应用

子应用改造，无界对子应用的侵入非常小，在满足跨域条件下`子应用可以不用改造`。

子应用的资源和接口的请求都在主域名发起，所以会有跨域问题。

所以这里，主应用和子应用，建议使用nginx进行代理，解决跨域的问题。

```php
//主应用
location /main {
    proxy_set_header Host    $host;
    proxy_pass http://127.0.0.1：8080;
}

//子应用
location /demo {
    proxy_set_header Host    $host;
    proxy_pass http://127.0.0.1：8081;
}

```

* 运行模式

无界有三种运行模式：[单例模式](https://link.juejin.cn?target=https%3A%2F%2Fwujie-micro.github.io%2Fdoc%2Fguide%2Fmode.html%23%25E5%258D%2595%25E4%25BE%258B%25E6%25A8%25A1%25E5%25BC%258F "https://wujie-micro.github.io/doc/guide/mode.html#%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F")、[保活模式](https://link.juejin.cn?target=https%3A%2F%2Fwujie-micro.github.io%2Fdoc%2Fguide%2Fmode.html%23%25E4%25BF%259D%25E6%25B4%25BB%25E6%25A8%25A1%25E5%25BC%258F "https://wujie-micro.github.io/doc/guide/mode.html#%E4%BF%9D%E6%B4%BB%E6%A8%A1%E5%BC%8F")、[重建模式](https://link.juejin.cn?target=https%3A%2F%2Fwujie-micro.github.io%2Fdoc%2Fguide%2Fmode.html%23%25E9%2587%258D%25E5%25BB%25BA%25E6%25A8%25A1%25E5%25BC%258F "https://wujie-micro.github.io/doc/guide/mode.html#%E9%87%8D%E5%BB%BA%E6%A8%A1%E5%BC%8F")

其中[保活模式](https://link.juejin.cn?target=https%3A%2F%2Fwujie-micro.github.io%2Fdoc%2Fguide%2Fmode.html%23%25E4%25BF%259D%25E6%25B4%25BB%25E6%25A8%25A1%25E5%25BC%258F "https://wujie-micro.github.io/doc/guide/mode.html#%E4%BF%9D%E6%B4%BB%E6%A8%A1%E5%BC%8F")、[重建模式](https://link.juejin.cn?target=https%3A%2F%2Fwujie-micro.github.io%2Fdoc%2Fguide%2Fmode.html%23%25E9%2587%258D%25E5%25BB%25BA%25E6%25A8%25A1%25E5%25BC%258F "https://wujie-micro.github.io/doc/guide/mode.html#%E9%87%8D%E5%BB%BA%E6%A8%A1%E5%BC%8F")子应用无需做任何改造工作，[单例模式](https://link.juejin.cn?target=https%3A%2F%2Fwujie-micro.github.io%2Fdoc%2Fguide%2Fmode.html%23%25E5%258D%2595%25E4%25BE%258B%25E6%25A8%25A1%25E5%25BC%258F "https://wujie-micro.github.io/doc/guide/mode.html#%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F")需要做生命周期改造

* [生命周期改造](https://link.juejin.cn?target=)

> 改造入口函数：
> 
> * 将子应用路由的创建、实例的创建渲染挂载到`window.__WUJIE_MOUNT`函数上
> * 将实例的销毁挂载到`window.__WUJIE_UNMOUNT`上
> * 如果子应用的实例化是在异步函数中进行的，在定义完生命周期函数后，请务必主动调用无界的渲染函数 `window.__WUJIE.mount()`

* vue2

```javascript
if (window.__POWERED_BY_WUJIE__) {
  let instance;
  window.__WUJIE_MOUNT = () => {
    const router = new VueRouter({ routes });
    instance = new Vue({ router, render: (h) => h(App) }).$mount("#app");
  };
  window.__WUJIE_UNMOUNT = () => {
    instance.$destroy();
  };
} else {
  new Vue({ router: new VueRouter({ routes }), render: (h) => h(App) }).$mount("#app");
}

```

* vue3

```ini
if (window.__POWERED_BY_WUJIE__) {
  let instance;
  window.__WUJIE_MOUNT = () => {
    const router = createRouter({ history: createWebHistory(), routes });
    instance = createApp(App);
    instance.use(router);
    instance.mount("#app");
  };
  window.__WUJIE_UNMOUNT = () => {
    instance.unmount();
  };
} else {
  createApp(App).use(createRouter({ history: createWebHistory(), routes })).mount("#app");
}

```

> 注意：使用vue的框架，是2还是3

### 2.3wujie使用

经过上面主应用和子应用的改造之后，就可以再主应用中，使用wujie，将子应用引入到主应用中去。

主应用，有这样的一个vue组件

* wujieDemo.vue 页面使用WujieVue引用目标项目的页面

```xml
<template> 
    <WujieVue
            width="100%"
            height="100%"
            name="demo"
            :url="demoUrl"
        />
</template>
​
<script>
 export default {
  data() {
    return {
        demoUrl: '/demo/home/index',
    }
  },
</script>

```

> 注意，上面 **WujieVue** 使用的`name`要和 **setupApp** 设置的`name`，要对应。

## 3.集成问题处理

### 3.1 window.parent问题

如果你集成的`子应用`中，也有用到`iframe`，那么在`iframe`中，使用`window.parent`，这个获取到的，不再是`子应用`了，这个window.parent将会是`主应用`。

出现这个问题，就是你可能会将一些对象，放入到window里面，那这个时候，需要兼容下这个情况。

例如：

```ini
window.Demo = {}

//这个时候，需要再加上
window.parent.Demo = window.Demo; //这个的作用，是将Demo这个对象赋值给 “主应用”

```

> 这样，在iframe中，使用window.parent.demo，就不会出现null的问题了。

### 3.2 antdv出现message不显示问题

出现这个问题，是在第一次打开子应用后，关闭后，重新再打开子应用。message消息提示框，无法显示。

出这个问题，是因为子应用，进行了生命周期改造导致。

所以这里，不建议对子应用进行生命周期改造。

...

还有很多问题，不知道怎么解决，也解决不了，最后还是放弃了...

## 4.写在最后

由于在整合的时候，遇到了不少问题，最后还是放弃了这个方式。

改用回iframe的方式，世界瞬间都清净了。

wujie你强任你强，我放弃了，放手一撑，与世无争

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/562e869c1680495883e69f6fbccdd369~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

* * *

好了，以上就是我个人的实操了。可能有些不对，大家伙，轻点喷！！！

个人理解，可能也不够全面，班门弄斧了。

好了，今天就先到这里了！！！^\_^

如果觉得有收获的，帮忙`点赞、评论、收藏`一下，再走呗！！！

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f5a08fb01ba14e8baaf6f2cc8db2b449~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)