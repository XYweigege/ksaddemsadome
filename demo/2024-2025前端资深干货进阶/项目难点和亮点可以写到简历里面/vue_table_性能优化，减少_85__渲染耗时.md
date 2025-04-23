#  
## 背景
<font style="color:rgb(30, 107, 184);">Table</font> 表格组件在 Web 开发中的应用随处可见，不过当表格数据量大后，伴随而来的是性能问题：渲染的 DOM 太多，渲染和交互都会有一定程度的卡顿。

通常，我们有两种优化表格的方式：一种是分页，另一种是虚拟滚动。这两种方式的优化思路都是减少 DOM 渲染的数量。在我们公司的项目中，会选择分页的方式，因为虚拟滚动不能正确的读出行的数量，会有 Accessibility 的问题。

记得 19 年的时候，我在 Zoom 已经推行了基于 Vue.js 的前后端分离的优化方案，并且基于 ElementUI 组件库开发了 ZoomUI。其中我们在重构用户管理页面的时候使用了 ZoomUI 的 <font style="color:rgb(30, 107, 184);">Table</font> 组件替换了之前老的用 jQuery 开发的 <font style="color:rgb(30, 107, 184);">Table</font> 组件。

因为绝大部分场景 <font style="color:rgb(30, 107, 184);">Table</font> 组件都是分页的，所以并不会有性能问题。但是在某个特殊场景下：基于关键词的搜索，可能会出现 200 * 20 条结果且不分页的情况，且表格是有一列是带有 <font style="color:rgb(30, 107, 184);">checkbox</font> 的，也就是可以选中某些行进行操作。

当我们去点选其中一行时，发现过了好久才选中，有明显的卡顿感，而之前的 jQuery 版本却没有这类问题，这一比较令人大跌眼镜。难道好好的技术重构，却要牺牲用户体验吗？

## Table 组件第一次优化尝试
既然有性能问题，那么我们的第一时间的思路应该是要找出产生性能问题的原因。

### 列展示优化
首先，ZoomUI 渲染的 DOM 数量是要多于 jQuery 渲染的 <font style="color:rgb(30, 107, 184);">Table</font> 的，因此第一个思考方向是让 <font style="color:rgb(30, 107, 184);">Table</font> 组件**尽可能地减少 DOM 的渲染数量**。

20 列数据通常在屏幕下是展示不全的，老的 jQuery Table 实现很简单，底部有滚动条，而 ZoomUI 在这种列可滚动的场景下，支持了左右列的固定，这样在左右滑动过程中，可以固定某些列一直展示，用户体验更好，但这样的实现是有一定代价的。

想要实现这种固定列的布局，ElementUI 用了 6 个 <font style="color:rgb(30, 107, 184);">table</font> 标签来实现，那么为什么需要 6 个 <font style="color:rgb(30, 107, 184);">table</font> 标签呢？

首先，为了让 <font style="color:rgb(30, 107, 184);">Table</font> 组件支持丰富的表头功能，表头和表体都是各自用一个 <font style="color:rgb(30, 107, 184);">table</font> 标签来实现。因此对于一个表格来说，就会有 2 个 <font style="color:rgb(30, 107, 184);">table</font> 标签，那么再加上左侧 <font style="color:rgb(30, 107, 184);">fixed</font> 的表格，和右侧 <font style="color:rgb(30, 107, 184);">fixed</font> 的表格，总共有 6 个 <font style="color:rgb(30, 107, 184);">table</font> 标签。

在 ElementUI 实现中，左侧 <font style="color:rgb(30, 107, 184);">fixed</font> 表格和右侧 <font style="color:rgb(30, 107, 184);">fixed</font> 表格从 DOM 上都渲染了完整的列，然后从样式上控制它们的显隐：

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557992608-fb6a620a-765a-495a-a912-4db9c3df08b1.webp)

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557992604-439dd2ba-680d-4208-8c18-cef3105c7441.webp)  


但这么实现是有性能浪费的，因为完全不需要渲染这么多列，实际上只需要渲染固定展示的列的 DOM，然后做好高度同步即可。ZoomUI 就是这么实现的，效果如下：  


![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557992605-8d93a0c8-d4c4-45e6-91db-8eae28a1140f.webp)

当然，仅仅减少 <font style="color:rgb(30, 107, 184);">fixed</font> 表格渲染的列，性能的提升还不够明显，有没有办法在列的渲染这个维度继续优化呢？  


这就是从业务层面的优化了，对于一个 20 列的表格，往往关键的列并没有多少，那么我们可不可以初次渲染仅仅渲染关键的列，其它列通过配置方式的渲染呢？

根据上述需求，我给 <font style="color:rgb(30, 107, 184);">Table</font> 组件添加了如下功能：

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557992645-10a2a39a-ee13-46c2-8d36-68caeb617c0b.webp)

<font style="color:rgb(30, 107, 184);">Table</font> 组件新增一个 <font style="color:rgb(30, 107, 184);">initDisplayedColumn</font> 属性，通过它可以配置初次渲染的列，同时当用户修改了初次渲染的列，会在前端存储下来，便于下一次的渲染。

通过这种方式，我们就可以少渲染一些列。显然，列渲染少了，表格整体渲染的 DOM 数就会变少，对性能也会有一定的提升。

### 更新渲染的优化
当然，仅仅通过优化列的渲染还是不够的，我们遇到的问题是当点选某一行引起的渲染卡顿，为什么会引起卡顿呢？

为了定位该问题，我用 <font style="color:rgb(30, 107, 184);">Table</font> 组件创建了一个 1000 * 7 的表格，开启了 Chrome 的 Performance 面板记录 <font style="color:rgb(30, 107, 184);">checkbox</font> 点选前后的性能。

在经过几次 <font style="color:rgb(30, 107, 184);">checkbox</font> 选择框的点选后，可以看到如下火焰图：

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557992561-918bef3f-7dd0-47c8-84d9-fc227b931730.webp)

其中黄色部分是 <font style="color:rgb(30, 107, 184);">Scripting</font> 脚本的执行时间，紫色部分是 <font style="color:rgb(30, 107, 184);">Rendering</font> 所占的时间。我们再截取一次更新的过程：

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557993029-94ed2576-c2f4-47b7-982f-1918fa9cc79f.webp)

然后观察 JS 脚本执行的 Call Tree，发现时间主要花在了 **<font style="color:rgb(30, 107, 184);">Table</font>**** ****组件的更新渲染上**：  


![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557993069-8aac93f2-ea5d-405c-b45f-28292e6511f2.webp)

我们发现组件的 <font style="color:rgb(30, 107, 184);">render to vnode</font> 花费的时间约 600ms；<font style="color:rgb(30, 107, 184);">vnode patch to DOM</font> 花费的时间约 160ms。

为什么会需要这么长时间呢，因为点选了 <font style="color:rgb(30, 107, 184);">checkbox</font>，在组件内部修改了其维护的选中状态数据，而整个组件的 <font style="color:rgb(30, 107, 184);">render</font> 过程中又访问了这个状态数据，因此当这个数据修改后，会引发整个组件的重新渲染。

而又由于有 1000 * 7 条数据，因此整个表格需要循环 1000 * 7 次去创建最内部的 <font style="color:rgb(30, 107, 184);">td</font>，整个过程就会耗时较长。

那么循环的内部是不是有优化的空间呢？对于 ElementUI 的 <font style="color:rgb(30, 107, 184);">Table</font> 组件，这里有非常大的优化空间。

其实优化思路主要参考我之前写的 [《揭秘 Vue.js 九个性能优化技巧》](http://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651570341&idx=1&sn=2c9dab243ee542bd312b78f5b2ba9830&chksm=80251764b7529e724b70d9d186f504d4bfb6b67acbd9607e9561e237afcfb4fb357ff204cd8f&scene=21#wechat_redirect) 其中的 <font style="color:rgb(30, 107, 184);">Local variables</font> 技巧。举个例子，在 ElementUI 的 <font style="color:rgb(30, 107, 184);">Table</font> 组件中，在渲染每个 <font style="color:rgb(30, 107, 184);">td</font> 的时候，有这么一段代码：

```javascript
const data = {
  store: this.store,
  _self: this.context || this.table.$vnode.context,
  column: columnData,
  row,
  $index
}
```

这样的代码相信很多小伙伴随手就写了，但却忽视了其内部潜在的性能问题。

由于 Vue.js 响应式系统的设计，在每次访问 <font style="color:rgb(30, 107, 184);">this.store</font> 的时候，都会触发<font style="color:rgb(0, 0, 0);">响应式数据内部的 </font><font style="color:rgb(30, 107, 184);">getter</font><font style="color:rgb(0, 0, 0);"> 函数，进而执行</font>它的依赖收集，当这段代码被循环了 1000 * 7 次，就会执行 <font style="color:rgb(30, 107, 184);">this.store</font> 7000 次的依赖收集，这就造成了性能的浪费，而真正的依赖收集只需要执行一次就足够了。

解决这个问题其实也并不难，由于 <font style="color:rgb(30, 107, 184);">Table</font> 组件中的 <font style="color:rgb(30, 107, 184);">TableBody</font> 组件是用 <font style="color:rgb(30, 107, 184);">render</font> 函数写的，我们可以在组件 <font style="color:rgb(30, 107, 184);">render</font> 函数的入口处定义一些局部变量：

```javascript
render(h) {
  const { store /*...*/} = this
  const context = this.context ||  this.table.$vnode.context
}
```

然后在渲染整个 <font style="color:rgb(30, 107, 184);">render</font> 的过程中，把局部变量当作内部函数的参数传入，这样在内部渲染 <font style="color:rgb(30, 107, 184);">td</font> 的渲染中再次访问这些变量就不会触发依赖收集了：

```javascript
rowRender({store, context, /* ...其它变量 */}) {
  const data = {
    store: store,
    _self: context,
    column: columnData,
    row,
    $index,
    disableTransition,
    isSelectedRow
  }
}
```

通过这种方式，我们把类似的代码都做了修改，就实现了 <font style="color:rgb(30, 107, 184);">TableBody</font> 组件渲染函数内部访问这些响应式变量，只触发一次依赖收集的效果，从而优化了 <font style="color:rgb(30, 107, 184);">render</font> 的性能。

来看一下优化后的火焰图：

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557993062-b592b442-bbb7-4196-8537-637776f13315.webp)

从面积上看似乎 <font style="color:rgb(30, 107, 184);">Scripting</font> 的执行时间变少了，我们再来看它一次更新所需要的 JS 执行时间：

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557993174-e5f7ec3b-7a2f-48ea-8c1e-ab080ef6726f.webp)

我们发现组件的 <font style="color:rgb(30, 107, 184);">render to vnode</font> 花费的时间约 240ms；<font style="color:rgb(30, 107, 184);">vnode patch to DOM</font> 花费的时间约 127ms。

可以看到，ZoomUI <font style="color:rgb(30, 107, 184);">Table</font> 组件的 <font style="color:rgb(30, 107, 184);">render</font> 的时间和 <font style="color:rgb(30, 107, 184);">update</font> 的时间都要明显少于 ElementUI 的 <font style="color:rgb(30, 107, 184);">Table</font> 组件。<font style="color:rgb(30, 107, 184);">render</font> 时间减少是由于响应式变量依赖收集的时间大大减少，<font style="color:rgb(30, 107, 184);">update</font> 的时间的减少是因为 <font style="color:rgb(30, 107, 184);">fixed</font> 表格渲染的 DOM 数量减少。

从用户的角度来看，DOM 的更新除了 <font style="color:rgb(30, 107, 184);">Scripting</font> 的时间，还有 <font style="color:rgb(30, 107, 184);">Rendering</font> 的时间，它们是共享一个线程的，当然由于 ZoomUI <font style="color:rgb(30, 107, 184);">Table</font> 组件渲染的 DOM 数量更少，执行 <font style="color:rgb(30, 107, 184);">Rendering</font> 的时间也更短。

### 手写 benchmark
仅仅从 Performance 面板的测试并不是一个特别精确的 benchmark，我们可以针对 <font style="color:rgb(30, 107, 184);">Table</font> 组件手写一个 benchmark。

我们可以先创建一个按钮，去模拟 <font style="color:rgb(30, 107, 184);">Table</font> 组件的选中操作：

```vue
<div>
       <zm-button @click="toggleSelection(computedData[1])
         ">切换第二行选中状态
         </zm-button>
         </div>
         <div>
         更新所需时间: {{ renderTime }}
</div>
```

然后实现这个 <font style="color:rgb(30, 107, 184);">toggleSelection</font> 函数：

```javascript
methods: {
  toggleSelection(row) {
    const s = window.performance.now()
    if (row) {
      this.$refs.table.toggleRowSelection(row)
    }
    setTimeout(() => {
      this.renderTime = (window.performance.now() - s).toFixed(2) + 'ms'
    })
  }
}
```

我们在点击事件的回调函数中，通过 <font style="color:rgb(30, 107, 184);">window.performance.now()</font> 记录起始时间，然后在 <font style="color:rgb(30, 107, 184);">setTimeout</font> 的回调函数中，再去通过时间差去计算整个更新渲染需要的时间。

由于 JS 的执行和 UI 渲染占用同一线程，因此在一个宏任务执行过程中，会执行这俩任务，而 <font style="color:rgb(30, 107, 184);">setTimeout 0</font> 会把对应的回调函数添加到下一个宏任务中，当该回调函数执行，说明上一个宏任务执行完毕，此时做时间差去计算性能是相对精确的。

基于手写的 benchmark 得到如下测试结果：

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557993158-78715510-b4d0-41f6-845f-8a8b436e02a8.webp)

ElementUI <font style="color:rgb(30, 107, 184);">Table</font>组件一次更新的时间约为 900ms。  


![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557993442-40d82093-07c7-4b17-aaa1-3139c7de2eb0.webp)

ZoomUI <font style="color:rgb(30, 107, 184);">Table</font>组件一次更新的时间约为 280ms，相比于 ElementUI 的 <font style="color:rgb(30, 107, 184);">Table</font> 组件，**性能提升了约三倍**。

## v-memo 的启发
经过这一番优化，基本解决了文章开头提到的问题，在 200 * 20 的表格中去选中一列，已经并无明显的卡顿感了，但相比于 jQuery 实现的 Table，效果还是要差了一点。

虽然性能优化了三倍，但我还是有个心结：明明只更新了一行数据的选中状态，却还是重新渲染了整个表格，仍然需要在组件 <font style="color:rgb(30, 107, 184);">render</font> 的过程中执行多次的循环，在 <font style="color:rgb(30, 107, 184);">patch</font> 的过程中通过 <font style="color:rgb(30, 107, 184);">diff</font> 算法来对比更新。

最近我研究了 Vue.js 3.2 <font style="color:rgb(30, 107, 184);">v-memo</font> 的实现，看完源码后，我非常激动，因为发现这个优化技巧似乎可以应用到 ZoomUI 的 Table 组件中，尽管我们的组件库是基于 Vue 2 版本开发的。

我花了一个下午的时间，经过一番尝试，果然成功了，那么具体是怎么做的呢？先不着急，我们从 <font style="color:rgb(30, 107, 184);">v-memo</font> 的实现原理说起。

### v-memo 的实现原理
<font style="color:rgb(30, 107, 184);">v-memo</font> 是 Vue.js 3.2 版本新增的指令，它可以用于普通标签，也可以用于列表，结合 <font style="color:rgb(30, 107, 184);">v-for</font> 使用，在官网文档中，有这么一段介绍：

<font style="color:rgb(30, 107, 184);">v-memo</font><font style="color:black;"> </font><font style="color:black;">仅供性能敏感场景的针对性优化，会用到的场景应该很少。渲染</font><font style="color:black;"> </font><font style="color:rgb(30, 107, 184);">v-for</font><font style="color:black;"> </font><font style="color:black;">长列表 (长度大于 1000) 可能是它最有用的场景：</font>

```html
<div v-for="item in list" :key="item.id" v-memo="[item.id === selected]">
  <p>ID: {{ item.id }} - selected: {{ item.id === selected }}</p>
  <p>...more child nodes</p>
</div>
```

<font style="color:black;">当组件的</font><font style="color:black;"> </font><font style="color:rgb(30, 107, 184);">selected</font><font style="color:black;"> </font><font style="color:black;">状态发生变化时，即使绝大多数</font><font style="color:black;"> </font><font style="color:rgb(30, 107, 184);">item</font><font style="color:black;"> </font><font style="color:black;">都没有发生任何变化，大量的 VNode 仍将被创建。此处使用的</font><font style="color:black;"> </font><font style="color:rgb(30, 107, 184);">v-memo</font><font style="color:black;"> </font><font style="color:black;">本质上代表着“仅在 item 从未选中变为选中时更新它，反之亦然”。这允许每个未受影响的</font><font style="color:black;"> </font><font style="color:rgb(30, 107, 184);">item</font><font style="color:black;"> </font><font style="color:black;">重用之前的 VNode，并完全跳过差异比较。注意，我们不需要把</font><font style="color:black;"> </font><font style="color:rgb(30, 107, 184);">item.id</font><font style="color:black;"> </font><font style="color:black;">包含在记忆依赖数组里面，因为 Vue 可以自动从</font><font style="color:black;"> </font><font style="color:rgb(30, 107, 184);">item</font><font style="color:black;"> </font><font style="color:black;">的</font><font style="color:black;"> </font><font style="color:rgb(30, 107, 184);">:key</font><font style="color:black;"> </font><font style="color:black;">中把它推断出来。</font>

其实说白了 <font style="color:rgb(30, 107, 184);">v-memo</font> 的核心就是复用 <font style="color:rgb(30, 107, 184);">vnode</font>，上述模板借助于在线模板编译工具，可以看到其对应的 <font style="color:rgb(30, 107, 184);">render</font> 函数：

```javascript
import { renderList as _renderList, Fragment as _Fragment, openBlock as _openBlock, createElementBlock as _createElementBlock, toDisplayString as _toDisplayString, createElementVNode as _createElementVNode, isMemoSame as _isMemoSame, withMemo as _withMemo } from "vue"

const _hoisted_1 = /*#__PURE__*/_createElementVNode("p", null, "...more child nodes", -1 /* HOISTED */)

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(true), _createElementBlock(_Fragment, null, _renderList(_ctx.list, (item, __, ___, _cached) => {
    const _memo = ([item.id === _ctx.selected])
    if (_cached && _cached.key === item.id && _isMemoSame(_cached, _memo)) return _cached
    const _item = (_openBlock(), _createElementBlock("div", {
      key: item.id
    }, [
      _createElementVNode("p", null, "ID: " + _toDisplayString(item.id) + " - selected: " + _toDisplayString(item.id === _ctx.selected), 1 /* TEXT */),
      _hoisted_1
    ]))
    _item.memo = _memo
    return _item
  }, _cache, 0), 128 /* KEYED_FRAGMENT */))
}
```

基于 <font style="color:rgb(30, 107, 184);">v-for</font> 的列表内部是通过 <font style="color:rgb(30, 107, 184);">renderList</font> 函数来渲染的，来看它的实现：

```javascript
function renderList(source, renderItem, cache, index) {
  let ret
  const cached = (cache && cache[index])
  if (isArray(source) || isString(source)) {
    ret = new Array(source.length)
    for (let i = 0, l = source.length; i < l; i++) {
      ret[i] = renderItem(source[i], i, undefined, cached && cached[i])
    }
  }
  else if (typeof source === 'number') {
    // source 是数字
  }
  else if (isObject(source)) {
    // source 是对象
  }
  else {
    ret = []
  }
  if (cache) {
    cache[index] = ret
  }
  return ret
}
```

我们只分析 <font style="color:rgb(30, 107, 184);">source</font>，也就是列表 <font style="color:rgb(30, 107, 184);">list</font> 是数组的情况，对于每一个 <font style="color:rgb(30, 107, 184);">item</font>，会执行 <font style="color:rgb(30, 107, 184);">renderItem</font> 函数来渲染。

从生成的 <font style="color:rgb(30, 107, 184);">render</font> 函数中，可以看到 <font style="color:rgb(30, 107, 184);">renderItem</font> 的实现如下：

```javascript
(item, __, ___, _cached) => {
  const _memo = ([item.id === _ctx.selected])
  if (_cached && _cached.key === item.id && _isMemoSame(_cached, _memo)) return _cached
  const _item = (_openBlock(), _createElementBlock("div", {
    key: item.id
  }, [
    _createElementVNode("p", null, "ID: " + _toDisplayString(item.id) + " - selected: " + _toDisplayString(item.id === _ctx.selected), 1 /* TEXT */),
    _hoisted_1
  ]))
  _item.memo = _memo
  return _item
}
```

在 <font style="color:rgb(30, 107, 184);">renderItem</font> 函数内部，维护了一个 <font style="color:rgb(30, 107, 184);">_memo</font> 变量，它就是用来判断是否从缓存里获取 <font style="color:rgb(30, 107, 184);">vnode</font> 的条件数组；而第四个参数 <font style="color:rgb(30, 107, 184);">_cached</font> 对应的就是 <font style="color:rgb(30, 107, 184);">item</font> 对应缓存的 <font style="color:rgb(30, 107, 184);">vnode</font>。接下来通过 <font style="color:rgb(30, 107, 184);">isMemoSame</font> 函数来判断 <font style="color:rgb(30, 107, 184);">memo</font> 是否相同，来看它的实现：

```javascript
function isMemoSame(cached, memo) {
  const prev = cached.memo
  if (prev.length != memo.length) {
    return false
  }
  for (let i = 0; i < prev.length; i++) {
    if (prev[i] !== memo[i]) {
      return false
    }
  }
  // ...
  return true
}
```

<font style="color:rgb(30, 107, 184);">isMemoSame</font> 函数内部会通过 <font style="color:rgb(30, 107, 184);">cached.memo</font> 拿到缓存的 <font style="color:rgb(30, 107, 184);">memo</font>，然后通过遍历对比每一个条件来判断和当前的 <font style="color:rgb(30, 107, 184);">memo</font> 是否相同。

而在 <font style="color:rgb(30, 107, 184);">renderItem</font> 函数的结尾，就会把 <font style="color:rgb(30, 107, 184);">_memo</font> 缓存到当前 <font style="color:rgb(30, 107, 184);">item</font> 的 <font style="color:rgb(30, 107, 184);">vnode</font> 中，便于下一次通过  <font style="color:rgb(30, 107, 184);">isMemoSame</font> 来判断这个 <font style="color:rgb(30, 107, 184);">memo</font> 是否相同，如果相同，说明该项没有变化，直接返回上一次缓存的 <font style="color:rgb(30, 107, 184);">vnode</font>。

那么这个缓存的 <font style="color:rgb(30, 107, 184);">vnode</font> 具体存储到哪里呢，原来在初始化组件实例的时候，就设计了渲染缓存：

```javascript
const instance = {
  // ...
  renderCache: []
}
```

然后在执行 <font style="color:rgb(30, 107, 184);">render</font> 函数的时候，把这个缓存当做第二个参数传入：

```javascript
const { renderCache } = instance
result = normalizeVNode(
  render.call(
    proxyToUse,
    proxyToUse,
    renderCache,
    props,
    setupState,
    data,
    ctx
  )
)
```

然后在执行 <font style="color:rgb(30, 107, 184);">renderList</font> 函数的时候，把 <font style="color:rgb(30, 107, 184);">_cahce</font> 作为第三个参数传入：

```javascript
export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(true), _createElementBlock(_Fragment, null, _renderList(_ctx.list, (item, __, ___, _cached) => {
    // renderItem 实现
  }, _cache, 0), 128 /* KEYED_FRAGMENT */))
}
```

所以实际上列表缓存的 <font style="color:rgb(30, 107, 184);">vnode</font> 都保留在 <font style="color:rgb(30, 107, 184);">_cache</font> 中，也就是 <font style="color:rgb(30, 107, 184);">instance.renderCache</font> 中。

那么为啥使用缓存的 <font style="color:rgb(30, 107, 184);">vnode</font> 就能优化 <font style="color:rgb(30, 107, 184);">patch</font> 过程呢，因为在 <font style="color:rgb(30, 107, 184);">patch</font> 函数执行的时候，如果遇到新旧 <font style="color:rgb(30, 107, 184);">vnode</font> 相同，就直接返回，什么也不用做了。

```javascript
const patch = (n1, n2, container, anchor = null, parentComponent = null, parentSuspense = null, isSVG = false, slotScopeIds = null, optimized = false) => {
  if(n1 === n2) {
    return
  }
  // ...
}
```

显然，由于使用缓存的 <font style="color:rgb(30, 107, 184);">vnode</font>，它们**指向同一个对象引用**，直接返回，节约了后续执行 <font style="color:rgb(30, 107, 184);">patch</font> 过程的时间。

### 在 Table 组件的应用
<font style="color:rgb(30, 107, 184);">v-memo</font> 的优化思路很简单，就是复用缓存的 <font style="color:rgb(30, 107, 184);">vnode</font>，这是一种空间换时间的优化思路。

那么，前面我们提到在表格组件中选择状态没有变化的行，是不是也可以从缓存中获取呢？

顺着这思路，我给 <font style="color:rgb(30, 107, 184);">Table</font> 组件设计了 <font style="color:rgb(30, 107, 184);">useMemo</font> 这个 <font style="color:rgb(30, 107, 184);">prop</font>，它其实是专门用于有选择列的场景。

然后在 <font style="color:rgb(30, 107, 184);">TableBody</font> 组件的 <font style="color:rgb(30, 107, 184);">created</font> 钩子函数中，创建了用于缓存的对象：

```javascript
created() {
  if (this.table.useMemo) {
    if (!this.table.rowKey) {
      throw new Error('for useMemo, row-key is required.')
    }
    this.vnodeCache = []
  }
}
```

这里之所以把 <font style="color:rgb(30, 107, 184);">vnodeCache</font> 定义到 <font style="color:rgb(30, 107, 184);">created</font> 钩子函数中，是因为它并不需要变成响应式对象。

另外注意，我们会根据每一行的 <font style="color:rgb(30, 107, 184);">key</font> 作为缓存的 <font style="color:rgb(30, 107, 184);">key</font>，因此 <font style="color:rgb(30, 107, 184);">Table</font> 组件的 <font style="color:rgb(30, 107, 184);">rowKey</font> 属性是必须的。

然后在渲染每一行的过程中，添加了 <font style="color:rgb(30, 107, 184);">useMemo</font> 相关的逻辑：

```javascript
function rowRender({ /* 各种变量参数 */}) {
  let memo
  const key = this.getKeyOfRow({ row, rowIndex: $index, rowKey })
  let cached
  if (useMemo) {
    cached = this.vnodeCache[key]
    const currentSelection = store.states.selection
    if (cached && !this.isRowSelectionChanged(row, cached.memo, currentSelection)) {
      return cached
    }
    memo = currentSelection.slice()
  }
  // 渲染 row，返回对应的 vnode
  const ret = rowVnode
  if (useMemo && columns.length) {
    ret.memo = memo
    this.vnodeCache[key] = ret
  }
  return ret
}
```

这里的 <font style="color:rgb(30, 107, 184);">memo</font> 变量用于记录已选中的行数据，并且它也会在函数最后存储到 <font style="color:rgb(30, 107, 184);">vnode</font> 的 <font style="color:rgb(30, 107, 184);">memo</font>，便于下一次的比对。

在每次渲染 <font style="color:rgb(30, 107, 184);">row</font> 的 <font style="color:rgb(30, 107, 184);">vnode</font> 前，会根据 <font style="color:rgb(30, 107, 184);">row</font> 对应的 <font style="color:rgb(30, 107, 184);">key</font> 尝试从缓存中取；如果缓存中存在，再通过 <font style="color:rgb(30, 107, 184);">isRowSelectionChanged</font> 来判断行的选中状态是否改变；如果没有改变，则直接返回缓存的 <font style="color:rgb(30, 107, 184);">vnode</font>。

如果没有命中缓存或者是行选择状态改变，则会去重新渲染拿到新的 <font style="color:rgb(30, 107, 184);">rowVnode</font>，然后更新到 <font style="color:rgb(30, 107, 184);">vnodeCache</font> 中。

当然，这种实现相比于 <font style="color:rgb(30, 107, 184);">v-memo</font> 没有那么通用，只去对比行选中的状态而不去对比其它数据的变化。你可能会问，如果这一行某列的数据修改了，但选中状态没变，再走缓存不就不对了吗？

确实存在这个问题，但是在我们的使用场景中，遇到数据修改，是会发送一个异步请求到后端，然获取新的数据再来更新表格数据。因此我只需要观测表格数据的变化清空 <font style="color:rgb(30, 107, 184);">vnodeCache</font> 即可：

```javascript
watch: {
  'store.states.data'() {
    if (this.table.useMemo) {
      this.vnodeCache = []
    }
  }
}
```

此外，我们支持列的可选则渲染功能，以及在窗口发生变化时，隐藏列也可能发生变化，于是在这两种场景下，也需要清空 <font style="color:rgb(30, 107, 184);">vnodeCache</font>：

```javascript
watch:{
  'store.states.columns'() {
    if (this.table.useMemo) {
      this.vnodeCache = []
    }
  },
  columnsHidden(newVal, oldVal) {
    if (this.table.useMemo && !valueEquals(newVal, oldVal)) {
      this.vnodeCache = []
    }
  }
}
```

以上实现就是基于 <font style="color:rgb(30, 107, 184);">v-memo</font> 的思路实现表格组件的性能优化。我们从火焰图上看一下它的效果：

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557993437-745fa8b7-5a3c-4d6f-a893-b58852e23367.webp)

我们发现黄色的 <font style="color:rgb(30, 107, 184);">Scripting</font> 时间几乎没有了，再来看它一次更新所需要的 JS 执行时间：

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557993452-3a4a92e0-75d9-4f73-82d8-b023c76bb537.webp)

我们发现组件的 <font style="color:rgb(30, 107, 184);">render to vnode</font> 花费的时间约 20ms；<font style="color:rgb(30, 107, 184);">vnode patch to DOM</font> 花费的时间约 1ms，整个更新渲染过程， JS 的执行时间大幅减少。  


另外，我们通过<font style="color:rgb(30, 107, 184);">benchmark</font> 测试，得到如下结果：

![](https://cdn.nlark.com/yuque/0/2024/webp/207857/1719557993532-ded9c1fa-8ad9-4bdf-be89-391276696cb4.webp)

优化后，ZoomUI <font style="color:rgb(30, 107, 184);">Table</font>组件一次更新的时间约为 <font style="color:rgb(30, 107, 184);">80ms</font>，相比于 ElementUI 的 <font style="color:rgb(30, 107, 184);">Table</font> 组件，**性能提升了约十倍**。

这个优化效果还是相当惊人的，并且从性能上已经不输 jQuery Table 了，我两年的心结也随之解开了。

## 总结
<font style="color:rgb(30, 107, 184);">Table</font> 表格性能提升主要是三个方面：减少 DOM 数量、优化 <font style="color:rgb(30, 107, 184);">render</font> 过程以及复用 <font style="color:rgb(30, 107, 184);">vnode</font>。有些时候，我们还可以从业务角度思考，去做一些优化。

虽然 <font style="color:rgb(30, 107, 184);">useMemo</font> 的实现还比较粗糙，但它目前已满足我们的使用场景了，并且当数据量越大，渲染的行列数越多，这种优化效果就越明显。如果未来有更多的需求，更新迭代就好。

由于一些原因，我们公司仍然在使用 Vue 2，但这并不妨碍我去学习 Vue 3，了解它一些新特性的实现原理以及设计思想，能让我开拓不少思路。

从分析定位问题到最终解决问题，希望这篇文章能给你在组件的性能优化方面提供一些思路，并应用到日常工作中。

  


