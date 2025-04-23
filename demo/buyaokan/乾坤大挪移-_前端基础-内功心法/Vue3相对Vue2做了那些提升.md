## <font style="color:rgb(31, 35, 40);">一、编译阶段</font>
回顾`**<font style="color:#DF2A3F;">Vue2</font>**`，<font style="color:rgb(31, 35, 40);">我们知道每个组件实例都对应一个 </font>`**<font style="color:#DF2A3F;">watcher</font>**`<font style="color:rgb(31, 35, 40);"> 实例，它会在组件渲染的过程中把用到的数据</font>`<font style="color:rgb(31, 35, 40);">property</font>`<font style="color:rgb(31, 35, 40);">记录为依赖，当依赖发生改变，触发</font>`<font style="color:rgb(31, 35, 40);">setter</font>`<font style="color:rgb(31, 35, 40);">，则会通知</font>`<font style="color:rgb(31, 35, 40);">watcher</font>`<font style="color:rgb(31, 35, 40);">，从而使关联的组件重新渲染</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1729652866951-679edca8-e339-4075-878b-a3ed53c7f9be.png)

<font style="color:rgb(31, 35, 40);">试想一下，一个组件结构如下图</font>

```vue
<template>
  <div id="content">
    <p class="text">静态文本</p>
    <p class="text">静态文本</p>
    <p class="text">{{ message }}</p>
    <p class="text">静态文本</p>
    ...
    <p class="text">静态文本</p>
  </div>
</template>
```

<font style="color:rgb(31, 35, 40);">可以看到，组件内部只有一个动态节点，剩余一堆都是静态节点，所以这里很多 </font>`<font style="color:rgb(31, 35, 40);">diff</font>`<font style="color:rgb(31, 35, 40);"> 和遍历其实都是不需要的，造成性能浪费</font>

<font style="color:rgb(31, 35, 40);">因此，</font>`<font style="color:rgb(31, 35, 40);">Vue3</font>`<font style="color:rgb(31, 35, 40);">在编译阶段，做了进一步优化。主要有如下：</font>

+ <font style="color:rgb(31, 35, 40);">diff算法优化</font>
+ <font style="color:rgb(31, 35, 40);">静态提升</font>
+ <font style="color:rgb(31, 35, 40);">事件监听缓存</font>
+ <font style="color:rgb(31, 35, 40);">SSR优化</font>

#### <font style="color:rgb(31, 35, 40);">diff算法优化</font>
`<font style="color:rgb(31, 35, 40);">vue3</font>`<font style="color:rgb(31, 35, 40);">在</font>`<font style="color:rgb(31, 35, 40);">diff</font>`<font style="color:rgb(31, 35, 40);">算法中相比</font>`<font style="color:rgb(31, 35, 40);">vue2</font>`<font style="color:rgb(31, 35, 40);">增加了静态标记</font>

<font style="color:rgb(31, 35, 40);">关于这个静态标记，其作用是为了会发生变化的地方添加一个</font>`<font style="color:rgb(31, 35, 40);">flag</font>`<font style="color:rgb(31, 35, 40);">标记，下次发生变化的时候直接找该地方进行比较</font>

<font style="color:rgb(31, 35, 40);">下图这里，已经标记静态节点的</font>`<font style="color:rgb(31, 35, 40);">p</font>`<font style="color:rgb(31, 35, 40);">标签在</font>`<font style="color:rgb(31, 35, 40);">diff</font>`<font style="color:rgb(31, 35, 40);">过程中则不会比较，把性能进一步提高</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1729653459862-e6509d67-c614-43bc-9c5b-7c8b95c3c0b2.png)

<font style="color:rgb(31, 35, 40);">关于静态类型枚举如下</font>

```javascript
export const enum PatchFlags {
  TEXT = 1,// 动态的文本节点
    CLASS = 1 << 1,  // 2 动态的 class
    STYLE = 1 << 2,  // 4 动态的 style
    PROPS = 1 << 3,  // 8 动态属性，不包括类名和样式
    FULL_PROPS = 1 << 4,  // 16 动态 key，当 key 变化时需要完整的 diff 算法做比较
    HYDRATE_EVENTS = 1 << 5,  // 32 表示带有事件监听器的节点
    STABLE_FRAGMENT = 1 << 6,   // 64 一个不会改变子节点顺序的 Fragment
    KEYED_FRAGMENT = 1 << 7, // 128 带有 key 属性的 Fragment
    UNKEYED_FRAGMENT = 1 << 8, // 256 子节点没有 key 的 Fragment
    NEED_PATCH = 1 << 9,   // 512
    DYNAMIC_SLOTS = 1 << 10,  // 动态 solt
    HOISTED = -1,  // 特殊标志是负整数表示永远不会用作 diff
    BAIL = -2 // 一个特殊的标志，指代差异算法
}
```

**具体什么是静态标记（编译模版的静态标记） demo**

很常见的一段代码

```javascript
<div id="app">
    <h1>大伟聊前端</h1>
    <p>今天天气真不错</p>
    <div>{{name}}</div>
</div>

```

<font style="color:rgb(77, 77, 77);">vue2中会解析</font>

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

<br/>color1
<font style="color:rgb(77, 77, 77);">其中前面两个标签是完全静态的，后续的渲染中不会产生任何变化，</font>  
<font style="color:rgb(77, 77, 77);">Vue2中依然使用_c新建成vdom，在diff的时候需要对比，有一些额外的性能损耗</font>

<br/>

<font style="color:rgb(77, 77, 77);">vue3中的解析结果</font>

```javascript
import { createVNode as _createVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createBlock as _createBlock } from "vue"

export function render(_ctx, _cache) {
  return (_openBlock(), _createBlock("div", { id: "app" }, [
    _createVNode("h1", null, "大伟聊前端"),
    _createVNode("p", null, "今天天气真不错"),
    _createVNode("div", null, _toDisplayString(_ctx.name), 1 /* TEXT */)
  ]))
}
// Check the console for the AST

```

<br/>color1
**<font style="color:rgb(77, 77, 77);">_createVNode新增第四个参数：</font>**

+ <font style="color:rgba(0, 0, 0, 0.75);">如果是静态节点，则不添加第四个参数，则在对比时不遍历，</font>
+ <font style="color:rgba(0, 0, 0, 0.75);">如果非静态节点，则根据动态变化的地方，添加不同的标记，比如：text，props等动态变化，则添加不同的标记，这样再diff的时候，只需要对比text或者props，不用再做无谓的props遍历，这样update性能就会提高。</font>
+ <font style="color:rgba(0, 0, 0, 0.75);">如果一个节点是有多个动态变化的地方，比如说既有id的动态绑定，又有text的绑定，根据位运算符进行组合即可，比如：</font>

<br/>

```javascript
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
    _createVNode("h1", null, "技术摸鱼"),
    _createVNode("p", null, "今天天气真不错"),
    _createVNode("div", {
      id: _ctx.userid,
      "\"": ""
    }, _toDisplayString(_ctx.name), 9 /* TEXT, PROPS */, ["id"])
  ]))
}

```

<br/>color1
<font style="color:rgb(77, 77, 77);">text是1，props是8，组合在一起就是9，我们可以简单的通过位运算来判定需要做text和props的判断, 按位与即可，只要不是0就是需要比较</font>

<br/>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1729666295051-eec1800e-02c5-4c5a-905a-c7ac059fdfb7.png)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1729666799969-399a1254-faed-4b39-9feb-f54df5eecf8b.png)

#### <font style="color:rgb(31, 35, 40);">静态提升(hoistStatic)</font>
**<font style="color:#DF2A3F;">V</font>****<font style="color:#DF2A3F;">ue2.x</font>**<font style="color:rgb(37, 41, 51);">中无论元素是否参与更新，每次都会重新创建然后渲染</font>

**<font style="color:#DF2A3F;">Vue3</font>**<font style="color:rgb(31, 35, 40);"> 中对不参与更新的元素，会做静态提升，只会被创建一次，在渲染时直接复用</font>

<font style="color:rgb(31, 35, 40);">这样就免去了重复的创建节点，大型应用会受益于这个改动，免去了重复的创建操作，优化了运行时候的内存占用</font>

```html
<span>你好</span>
<div>{{ message }}</div>
```

<font style="color:rgb(31, 35, 40);">没有做静态提升之前</font>

```javascript
export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createBlock(_Fragment, null, [
    _createVNode("span", null, "你好"),
    _createVNode("div", null, _toDisplayString(_ctx.message), 1 /* TEXT */)
  ], 64 /* STABLE_FRAGMENT */))
}
```

<font style="color:rgb(31, 35, 40);">做了静态提升之后</font>

```javascript
const _hoisted_1 = /*#__PURE__*/_createVNode("span", null, "你好", -1 /* HOISTED */)

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createBlock(_Fragment, null, [
    _hoisted_1,
    _createVNode("div", null, _toDisplayString(_ctx.message), 1 /* TEXT */)
  ], 64 /* STABLE_FRAGMENT */))
}

// Check the console for the AST
```

<font style="color:rgb(31, 35, 40);">静态内容</font>`<font style="color:rgb(31, 35, 40);">_hoisted_1</font>`<font style="color:rgb(31, 35, 40);">被放置在</font>`<font style="color:rgb(31, 35, 40);">render</font>`<font style="color:rgb(31, 35, 40);"> 函数外，每次渲染的时候只要取 </font>`<font style="color:rgb(31, 35, 40);">_hoisted_1</font>`<font style="color:rgb(31, 35, 40);"> 即可</font>

<font style="color:rgb(31, 35, 40);">同时 </font>`<font style="color:rgb(31, 35, 40);">_hoisted_1</font>`<font style="color:rgb(31, 35, 40);"> 被打上了 </font>`<font style="color:rgb(31, 35, 40);">PatchFlag</font>`<font style="color:rgb(31, 35, 40);"> ，静态标记值为 -1 ，特殊标志是负整数表示永远不会用于 Diff</font>

#### <font style="color:rgb(31, 35, 40);">事件监听缓存</font>
<font style="color:rgb(31, 35, 40);">默认情况下绑定事件行为会被视为动态绑定，所以每次都会去追踪它的变化</font>

<font style="color:rgb(37, 41, 51);">但是因为是同一个函数，所以没有追踪变化，直接缓存起来复用即可</font>

```html
<div>
  <button @click = 'onClick'>点我</button>
</div>
```

<font style="color:rgb(31, 35, 40);">没开启事件监听器缓存</font>

```javascript
export const render = /*#__PURE__*/_withId(function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createBlock("div", null, [
    _createVNode("button", { onClick: _ctx.onClick }, "点我", 8 /* PROPS */, ["onClick"])
    // PROPS=1<<3,// 8 //动态属性，但不包含类名和样式
  ]))
})
```

<font style="color:rgb(37, 41, 51);">这里有一个8，表示着这个节点有了静态标记，有静态标记就会进行diff算法对比差异，所以会浪费时间</font>

<font style="color:rgb(31, 35, 40);">开启事件侦听器缓存后</font>

```javascript
export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createBlock("div", null, [
    _createVNode("button", {
      onClick: _cache[1] || (_cache[1] = (...args) => (_ctx.onClick(...args)))
    }, "点我")
  ]))
}
```

<font style="color:rgb(31, 35, 40);">上述发现开启了缓存后，没有了静态标记。也就是说下次</font>`<font style="color:rgb(31, 35, 40);">diff</font>`<font style="color:rgb(31, 35, 40);">算法的时候直接使用</font>

#### <font style="color:rgb(31, 35, 40);">SSR优化</font>
<font style="color:rgb(31, 35, 40);">当静态内容大到一定量级时候，会用</font>`<font style="color:rgb(31, 35, 40);">createStaticVNode</font>`<font style="color:rgb(31, 35, 40);">方法在客户端去生成一个static node，这些静态</font>`<font style="color:rgb(31, 35, 40);">node</font>`<font style="color:rgb(31, 35, 40);">，会被直接</font>`<font style="color:rgb(31, 35, 40);">innerHtml</font>`<font style="color:rgb(31, 35, 40);">，就不需要创建对象，然后根据对象渲染</font>

```html
<div>
<div>
  <span>你好</span>
</div>
...  // 很多个静态属性
<div>
  <span>{{ message }}</span>
</div>
</div>
```

<font style="color:rgb(31, 35, 40);">编译后</font>

```javascript
import { mergeProps as _mergeProps } from "vue"
import { ssrRenderAttrs as _ssrRenderAttrs, ssrInterpolate as _ssrInterpolate } from "@vue/server-renderer"

export function ssrRender(_ctx, _push, _parent, _attrs, $props, $setup, $data, $options) {
  const _cssVars = { style: { color: _ctx.color }}
  _push(`<div${
    _ssrRenderAttrs(_mergeProps(_attrs, _cssVars))
  }><div><span>你好</span>...<div><span>你好</span><div><span>${
    _ssrInterpolate(_ctx.message)
  }</span></div></div>`)
}
```

#### 生成 Block tree
`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.js 2.x</font>`<font style="color:rgb(77, 77, 77);"> </font><font style="color:rgb(77, 77, 77);">的数据更新并触发</font>**<font style="color:rgb(77, 77, 77);">重新渲染的粒度是组件级的</font>**<font style="color:rgb(77, 77, 77);">。在 2.0 里，</font>**<font style="color:rgb(77, 77, 77);">渲染效率的快慢与组件大小成正相关</font>**<font style="color:rgb(77, 77, 77);">：组件越大，渲染效率越慢。并且，对于一些静态节点，又无数据更新，这些遍历对比就是对性能浪费。</font>

`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.js 3.0</font>`<font style="color:rgb(77, 77, 77);"> 做到了</font>**<font style="color:rgb(77, 77, 77);">通过编译阶段对静态模板的分析，编译生成了 Block tree</font>**<font style="color:rgb(77, 77, 77);">。  
</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Block tree 是一个基于动态节点指令将模版进行切割所形成的区块</font>`<font style="color:rgb(77, 77, 77);">，每个区块内部的节点结构是固定的， 每个区块只需要追踪自身包含的动态节点。所以，</font>**<font style="color:rgb(77, 77, 77);">在 3.0 里，渲染效率不再与模板大小 成正相关，而是与模板中动态节点的数量成正相关。</font>**  


## <font style="color:rgb(31, 35, 40);">二、源码体积</font>
<font style="color:rgb(31, 35, 40);">相比</font>`<font style="color:rgb(31, 35, 40);">Vue2</font>`<font style="color:rgb(31, 35, 40);">，</font>`<font style="color:rgb(31, 35, 40);">Vue3</font>`<font style="color:rgb(31, 35, 40);">整体体积变小了，除了移出一些不常用的API，再重要的是</font>`<font style="color:rgb(31, 35, 40);">Tree shanking</font>`

<font style="color:rgb(31, 35, 40);">任何一个函数，如</font>`<font style="color:rgb(31, 35, 40);">ref</font>`<font style="color:rgb(31, 35, 40);">、</font>`<font style="color:rgb(31, 35, 40);">reavtived</font>`<font style="color:rgb(31, 35, 40);">、</font>`<font style="color:rgb(31, 35, 40);">computed</font>`<font style="color:rgb(31, 35, 40);">等，仅仅在用到的时候才打包，没用到的模块都被摇掉，打包的整体体积变小</font>

```javascript
import { computed, defineComponent, ref } from 'vue';
export default defineComponent({
  setup(props, context) {
    const age = ref(18)

    let state = reactive({
      name: 'test'
    })

    const readOnlyAge = computed(() => age.value++) // 19

    return {
      age,
      state,
      readOnlyAge
    }
  }
});
```

## <font style="color:rgb(31, 35, 40);">三、响应式系统</font>
`<font style="color:rgb(31, 35, 40);">vue2</font>`<font style="color:rgb(31, 35, 40);">中采用 </font>`<font style="color:rgb(31, 35, 40);">defineProperty</font>`<font style="color:rgb(31, 35, 40);">来劫持整个对象，然后进行深度遍历所有属性，给每个属性添加</font>`<font style="color:rgb(31, 35, 40);">getter</font>`<font style="color:rgb(31, 35, 40);">和</font>`<font style="color:rgb(31, 35, 40);">setter</font>`<font style="color:rgb(31, 35, 40);">，实现响应式</font>

`<font style="color:rgb(31, 35, 40);">vue3</font>`<font style="color:rgb(31, 35, 40);">采用</font>`<font style="color:rgb(31, 35, 40);">proxy</font>`<font style="color:rgb(31, 35, 40);">重写了响应式系统，因为</font>`<font style="color:rgb(31, 35, 40);">proxy</font>`<font style="color:rgb(31, 35, 40);">可以对整个对象进行监听，所以不需要深度遍历</font>

+ <font style="color:rgb(31, 35, 40);">可以监听动态属性的添加</font>
+ <font style="color:rgb(31, 35, 40);">可以监听到数组的索引和数组</font>`<font style="color:rgb(31, 35, 40);">length</font>`<font style="color:rgb(31, 35, 40);">属性</font>
+ <font style="color:rgb(31, 35, 40);">可以监听删除属性</font>

<br/>color1
在`Vue2`中通过Object.defineProperty的get、set和发布订阅来完成响应式，但Object.defineProperty的get、set并不能监控深层的对象、新增属性、删除属性和数组的变化，如果存在深层的嵌套对象关系，需要深层次的进行监听，造成了性能的极大问题。  
为了解决`Vue2`中的那些问题，在`Vue3`中引入了Proxy，Proxy的监听是针对一个对象的，那么对这个对象的所有操作会进入监听操作，这就完全可以代理所有属性，包括新增属性和删除属性，并且Proxy可以监听数组的变化。

<br/>

 



### 总结<font style="color:rgb(79, 79, 79);">Vue3.0 是如何变得更快的？</font>
+ **<font style="color:rgba(0, 0, 0, 0.75);">1、diff 方法优化</font>**<font style="color:rgba(0, 0, 0, 0.75);">  
</font><font style="color:rgba(0, 0, 0, 0.75);">Vue2.x 中的虚拟 dom 是进行</font>**<font style="color:rgba(0, 0, 0, 0.75);">全量的对比</font>**<font style="color:rgba(0, 0, 0, 0.75);">。  
</font><font style="color:rgba(0, 0, 0, 0.75);">Vue3.0 中</font>**<font style="color:rgba(0, 0, 0, 0.75);">新增了静态标记</font>**<font style="color:rgba(0, 0, 0, 0.75);">（PatchFlag）：新旧虚拟结点进行对比的时候，只对比 带有静态标记的节点。这样就优化了对比量，提高了对比速度。</font>
+ **<font style="color:rgba(0, 0, 0, 0.75);">2、静态提升</font>**<font style="color:rgba(0, 0, 0, 0.75);">  
</font><font style="color:rgba(0, 0, 0, 0.75);">Vue2.x : 无论元素是否参与更新，每次都会重新创建。  
</font><font style="color:rgba(0, 0, 0, 0.75);">Vue3.0 : 对不参与更新的元素，只会被创建一次，之后会在每次渲染时候被不停的复用。</font>
+ **<font style="color:rgba(0, 0, 0, 0.75);">3、事件侦听器缓存</font>**<font style="color:rgba(0, 0, 0, 0.75);">  
</font><font style="color:rgba(0, 0, 0, 0.75);">默认情况下 onClick 会被视为动态绑定，所以每次都会去追踪它的变化，  
</font><font style="color:rgba(0, 0, 0, 0.75);">但是在Vue中，绑定的方法会被缓存起来，下次更新直接复用即可。</font>





### React和Vue的diff


![](https://cdn.nlark.com/yuque/0/2024/png/207857/1729667506341-853a3897-9229-4a92-b924-709ec792e339.png)

Vue3 通过静态标记 + 响应式 + 虚拟 dom 的方式，控制了 diff 的颗粒度，让 diff 的时间不会超过 16ms，但是 React 自上而下的 diff 过程，项目大了之后，一旦 diff 的时间超过 16.6ms，就会造成卡顿，

对此 React 交出的解决方案就是时间切片

**<font style="color:#DF2A3F;">简单的来说就是把 diff 的任务按照元素拆开，利用浏览器的空闲时间去算 diff，React 把各种优化的策略都留给了开发者，Vue 则是帮开发者做了很多优化的工作</font>**

<br/>color1
React走了另外一条路，既然主要问题是diff导致卡顿，于是React走了类似 cpu 调度的逻辑，把vdom这棵树微观变成了链表，利用浏览器的空闲时间来做diff，如果超过了16ms，有动画或者用户交互的任务，就把主进程控制权还给浏览器，等空闲了继续。实际上是在之前用不上的时间里做了diff操作。

<br/>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1729666799969-399a1254-faed-4b39-9feb-f54df5eecf8b.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_1500%2Climit_0)

