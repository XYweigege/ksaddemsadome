## Vue3的虚拟dom
先说结论，静态标记，`upadte`性能提升1.3~2倍，`ssr`提升2~3倍，怎么做到的呢

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1729668587035-0a572366-ebfe-428b-aaf7-b7d21948edc4.webp)

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1729668586782-9ccf4f1c-9e52-4bbc-aeef-9c759f2212f7.webp)

### 编译模板的静态标记
我们来看一段很常见的代码

```html
<div id="app">
  <h1>大伟聊前端</h1>
  <p>今天天气真不错</p>
  <div>{{name}}</div>
</div>
```

vue2中会解析

```javascript
function render() {
  with(this) {
    return _c('div', {
      attrs: {
        "id": "app"
      }
    }, [_c('h1', [_v("大伟聊前端")]), _c('p', [_v("今天天气真不错")]), _c('div', [_v(
      _s(name))])])
  }
}
```

其中前面两个标签是完全静态的，后续的渲染中不会产生任何变化，`Vue2`中依然使用`_c`新建成`vdom`，在`diff`的时候需要对比，有一些额外的性能损耗

我们看下vue3中的[解析结果](https://template-explorer.vuejs.org/#eyJzcmMiOiI8ZGl2IGlkPVwiYXBwXCI+XG4gICAgPGgxPuWkp+S8n+iBiuWJjeerrzwvaDE+XG4gICAgPHA+5LuK5aSp5aSp5rCU55yf5LiN6ZSZPC9wPlxuICAgIDxkaXYgOmlkPVwidXNlcmlkXCJcIj57e25hbWV9fTwvZGl2PlxuXG48L2Rpdj5cbiIsInNzciI6ZmFsc2UsIm9wdGlvbnMiOnsiaG9pc3RTdGF0aWMiOnRydWUsImNhY2hlSGFuZGxlcnMiOnRydWUsIm9wdGltaXplQmluZGluZ3MiOmZhbHNlfX0=)

```javascript
import { createVNode as _createVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createBlock as _createBlock } from "vue"

export function render(_ctx, _cache) {
  return (_openBlock(), _createBlock("div", { id: "app" }, [
    _createVNode("h1", null, "技术摸鱼"),
    _createVNode("p", null, "今天天气真不错"),
    _createVNode("div", null, _toDisplayString(_ctx.name), 1 /* TEXT */)
  ]))
}
// Check the console for the AST
```

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1729668586746-ab99efe5-99d6-4c37-b08d-9cb5b2b2f22e.webp)

最后一个`_createVNode`第四个参数1，只有带这个参数的，才会被真正的追踪，静态节点不需要遍历，这个就是vue3优秀性能的主要来源，再看复杂一点的

```html
<div id="app">
  <h1>大伟聊前端</h1>
  <p>今天天气真不错</p>

  <div>{{name}}</div>
  <div :class="{red:isRed}">test1</div>
  <button @click="handleClick">test2</button>
  <input type="text" v-model="name">

</div>
```

解析的结果 [在线预览](https://template-explorer.vuejs.org/#eyJzcmMiOiI8ZGl2IGlkPVwiYXBwXCI+XG4gICAgPGgxPuWkp+S8n+iBiuWJjeerrzwvaDE+XG4gICAgPHA+5LuK5aSp5aSp5rCU55yf5LiN6ZSZPC9wPlxuXG4gICAgPGRpdj57e25hbWV9fTwvZGl2PlxuICAgIDxkaXYgOmNsYXNzPVwie3JlZDppc1JlZH1cIj50ZXN0MTwvZGl2PlxuICAgIDxidXR0b24gQGNsaWNrPVwiaGFuZGxlQ2xpY2tcIj50ZXN0MjwvYnV0dG9uPlxuICAgIFxuICAgIFxuPC9kaXY+Iiwic3NyIjpmYWxzZSwib3B0aW9ucyI6eyJvcHRpbWl6ZUJpbmRpbmdzIjpmYWxzZX19)

```javascript

import { createVNode as _createVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createBlock as _createBlock } from "vue"

export function render(_ctx, _cache) {
  return (_openBlock(), _createBlock("div", { id: "app" }, [
    _createVNode("h1", null, "大伟聊前端"),
    _createVNode("p", null, "今天天气真不错"),
    _createVNode("div", null, _toDisplayString(_ctx.name), 1 /* TEXT */),
    _createVNode("div", {
      class: {red:_ctx.isRed}
    }, "test1", 2 /* CLASS */),
    _createVNode("button", { onClick: _ctx.handleClick }, "test2", 8 /* PROPS */, ["onClick"])
  ]))
}

// Check the console for the AST
```

_createVNode出第四个参数出现了别的数字，根据后面注释也很容易猜出，根据`text`，`props`等不同的标记，这样再diff的时候，只需要对比`text`或者`props`，不用再做无畏的`props`遍历, 优秀！ 借鉴一下劝退大兄弟的注释

```javascript
export const enum PatchFlags {
  TEXT = 1,// 表示具有动态textContent的元素
    CLASS = 1 << 1,  // 表示有动态Class的元素
    STYLE = 1 << 2,  // 表示动态样式（静态如style="color: red"，也会提升至动态）
    PROPS = 1 << 3,  // 表示具有非类/样式动态道具的元素。
    FULL_PROPS = 1 << 4,  // 表示带有动态键的道具的元素，与上面三种相斥
    HYDRATE_EVENTS = 1 << 5,  // 表示带有事件监听器的元素
    STABLE_FRAGMENT = 1 << 6,   // 表示其子顺序不变的片段（没懂）。 
    KEYED_FRAGMENT = 1 << 7, // 表示带有键控或部分键控子元素的片段。
    UNKEYED_FRAGMENT = 1 << 8, // 表示带有无key绑定的片段
    NEED_PATCH = 1 << 9,   // 表示只需要非属性补丁的元素，例如ref或hooks
    DYNAMIC_SLOTS = 1 << 10,  // 表示具有动态插槽的元素
    }
```

如果同时有`props`和`text`的绑定呢， 位运算组合即可

```html
<div id="app">
  <h1>大伟聊前端</h1>
  <p>今天天气真不错</p>
  <div :id="userid"">{{name}}</div>
</div>
```

```javascript
import { createVNode as _createVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createBlock as _createBlock } from "vue"

export function render(_ctx, _cache) {
  return (_openBlock(), _createBlock("div", { id: "app" }, [
    _createVNode("h1", null, "大伟聊前端"),
    _createVNode("p", null, "今天天气真不错"),
    _createVNode("div", {
      id: _ctx.userid,
      "\"": ""
    }, _toDisplayString(_ctx.name), 9 /* TEXT, PROPS */, ["id"])
  ]))
}

// Check the console for the AST
```

`text`是1，`props`是8，组合在一起就是9，我们可以简单的通过位运算来判定需要做`text`和`props`的判断, 按位与即可，只要不是0就是需要比较

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1729668586735-f1ce645e-2a9e-45ce-8abf-4a449f494a76.webp)

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1729668586736-04feb95b-e120-471c-898b-2c2c384bb3fc.webp)

 位运算来做类型组合 本身就是一个最佳实践，react大兄弟也是一样的 [代码](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Ffacebook%2Freact%2Fblob%2Fmaster%2Fpackages%2Freact-dom%2Fsrc%2Fevents%2FEventSystemFlags.js)

```javascript
export const PLUGIN_EVENT_SYSTEM = 1;
export const RESPONDER_EVENT_SYSTEM = 1 << 1;
export const USE_EVENT_SYSTEM = 1 << 2;
export const IS_TARGET_PHASE_ONLY = 1 << 3;
export const IS_PASSIVE = 1 << 4;
export const PASSIVE_NOT_SUPPORTED = 1 << 5;
export const IS_REPLAYED = 1 << 6;
export const IS_FIRST_ANCESTOR = 1 << 7;
export const LEGACY_FB_SUPPORT = 1 << 8;
```

### 事件缓存
绑定的`@click`会存在缓存里 [链接](https://template-explorer.vuejs.org/#eyJzcmMiOiI8ZGl2IGlkPVwiYXBwXCI+XG4gIDxidXR0b24gQGNsaWNrPVwiaGFuZGxlQ2xpY2tcIj50ZXN0PC9idXR0b24+XG48L2Rpdj5cbiIsInNzciI6ZmFsc2UsIm9wdGlvbnMiOnsiY2FjaGVIYW5kbGVycyI6dHJ1ZSwib3B0aW1pemVCaW5kaW5ncyI6ZmFsc2V9fQ==)

```html
<div id="app">
  <button @click="handleClick">test</button>
</div>
```

```javascript

export function render(_ctx, _cache) {
  return (_openBlock(), _createBlock("div", { id: "app" }, [
    _createVNode("button", {
      onClick: _cache[1] || (_cache[1] = $event => (_ctx.handleClick($event)))
    }, "test")
  ]))
}
```

传入的事件会自动生成并缓存一个内联函数再cache里，变为一个静态节点。这样就算我们自己写内联函数，也不会导致多余的重复渲染 真是优秀啊

### 静态提升
[代码](https://template-explorer.vuejs.org/#eyJzcmMiOiI8ZGl2IGlkPVwiYXBwXCI+XG4gICAgPGgxPuWkp+S8n+iBiuWJjeerrzwvaDE+XG4gICAgPHA+5LuK5aSp5aSp5rCU55yf5LiN6ZSZPC9wPlxuXG4gICAgPGRpdj57e25hbWV9fTwvZGl2PlxuICAgIDxkaXYgOmNsYXNzPVwie3JlZDppc1JlZH1cIj50ZXN0PC9kaXY+XG5cblxuPC9kaXY+Iiwic3NyIjpmYWxzZSwib3B0aW9ucyI6eyJob2lzdFN0YXRpYyI6dHJ1ZSwiY2FjaGVIYW5kbGVycyI6dHJ1ZSwib3B0aW1pemVCaW5kaW5ncyI6ZmFsc2V9fQ==)

```html
<div id="app">
  <h1>大伟聊前端</h1>
  <p>今天天气真不错</p>
  <div>{{name}}</div>
  <div :class="{red:isRed}">test</div>
</div>
```

```javascript
const _hoisted_1 = { id: "app" }
const _hoisted_2 = _createVNode("h1", null, "大伟聊前端", -1 /* HOISTED */)
const _hoisted_3 = _createVNode("p", null, "今天天气真不错", -1 /* HOISTED */)

export function render(_ctx, _cache) {
  return (_openBlock(), _createBlock("div", _hoisted_1, [
    _hoisted_2,
    _hoisted_3,
    _createVNode("div", null, _toDisplayString(_ctx.name), 1 /* TEXT */),
    _createVNode("div", {
      class: {red:_ctx.isRed}
    }, "test", 2 /* CLASS */)
  ]))
}
```

## Vue3和React fiber 的vdom
很多人吐槽越来越像`React`，其实越来越像的`api`，代表着前端的两个方向

### Vue1.x
没有`vdom`，完全的响应式，每个数据变化，都通过响应式通知机制来新建`Watcher`干活，就像独立团规模小的时候，每个战士入伍和升职，都主动通知咱老李，管理方便

项目规模变大后，过多的`Watcher`，会导致性能的瓶颈

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1729668587092-f23d2868-4cb3-4325-b368-f83f93db9182.webp)

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1729668587128-3a4a3085-2f25-46ae-b9f2-0f3bdf412f4e.webp)

### React15x
而`React15`时代，没有响应式，数据变了，整个新数据和老的数据做`diff`，算出差异 就知道怎么去修改`dom`了，就像老李指挥室有一个模型，每次人事变更，通过对比所有人前后差异，就知道了变化， 看起来有很多计算量，但是这种`immutable`的数据结构对大型项目比较友好，而且`Vdom`抽象成功后，换成别的平台render成为了可能，无论是打鬼子还是打国军，都用一个`vdom`模式

碰到的问题一样，如果`dom`节点持续变多，每次`diff`的时间超过了`16ms`，就可能会造成卡顿（60fps）

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1729668587181-c7df7152-e6b0-48cd-8c5a-16d342bfe02b.webp)

### Vue2.x
引入vdom，控制了颗粒度，组件层面走watcher通知， 组件内部走vdom做diff，既不会有太多watcher，也不会让vdom的规模过大，diff超过16ms，真是优秀啊 就像独立团大了以后，只有营长排长级别的变动，才会通知老李，内部的自己diff管理了

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1729668587462-7be6c1e0-8375-438c-91a1-44109087113e.webp)

### React 16 Fiber
`React`走了另外一条路，既然主要问题是`diff`导致卡顿，于是`React`走了类似`cpu`调度的逻辑，把`vdom`这棵树，微观变成了链表，利用浏览器的空闲时间来做`diff`，如果超过了`16ms`，有动画或者用户交互的任务，就把主进程控制权还给浏览器，等空闲了继续，特别像等待女神的备胎

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1729668587467-2840ce6d-ed57-4ce3-9771-26f2e62f93a8.webp)

`diff`的逻辑，变成了单向的链表，任何时候主线程女神有空了，我们在继续蹭上去接盘做`diff`，大家研究下`requestIdleCallback`就知道，从浏览器角度看 是这样的

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1729669038558-5524215c-04a9-4355-9c63-5c6c04abe06d.png)

大概代码

```javascript
requestIdelCallback(myNonEssentialWork);
// 等待空闲
function myNonEssentialWork (deadline) {
  // deadline.timeRemaining()>0 主线程还有事件
  // 还有diff任务没算玩
  while (deadline.timeRemaining() > 0 && tasks.length > 0) {
    doWorkIfNeeded();
  }
  // 没时间了，把函数还回去
  if (tasks.length > 0){
    requestIdleCallback(myNonEssentialWork);
  }
}
```

### Vue3
<br/>color1
这里的静态提升和事件缓存刚才说过了，就不说了，其实我也很纳闷，这些静态标记和事件缓存，React本身也可以做，为啥就不实现了，连shouldComponentUpdate都得自己定义，为啥不把默认的组件都变成pure或者memo呢，唉，也许这就是人生把

React给你自由，Vue让你持久，可能也是现在国内Vue和React都如此受欢迎的原因把

Vue3通过Proxy响应式+组件内部vdom+静态标记，把任务颗粒度控制的足够细致，所以也不太需要time-slice了

<br/>

 

  


 

