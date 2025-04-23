
<font style="color:rgb(85, 85, 85);">自从有了 </font><font style="color:rgb(199, 37, 78);">VNode</font><font style="color:rgb(85, 85, 85);"> ，开发页面的方式就变成了书写 </font><font style="color:rgb(199, 37, 78);">VNode</font><font style="color:rgb(85, 85, 85);">，但如果日常开发中需要手写 </font><font style="color:rgb(199, 37, 78);">VNode</font><font style="color:rgb(85, 85, 85);"> ，那绝对是反人类的，在“组件的本质”一章中我们使用了 </font><font style="color:rgb(199, 37, 78);">snabbdom</font><font style="color:rgb(85, 85, 85);"> 的 </font><font style="color:rgb(199, 37, 78);">h</font><font style="color:rgb(85, 85, 85);"> 函数来辅助讲解一些小例子，</font><font style="color:rgb(199, 37, 78);">h</font><font style="color:rgb(85, 85, 85);"> 函数作为创建 </font><font style="color:rgb(199, 37, 78);">VNode</font><font style="color:rgb(85, 85, 85);"> 对象的函数封装，在一定程度上改善了这个问题，但却没有解决本质问题，这也是为什么我们需要模板或 </font><font style="color:rgb(199, 37, 78);">jsx</font><font style="color:rgb(85, 85, 85);"> 的原因。但 </font><font style="color:rgb(199, 37, 78);">h</font><font style="color:rgb(85, 85, 85);"> 函数依然很重要，因为无论是模板还是 </font><font style="color:rgb(199, 37, 78);">jsx</font><font style="color:rgb(85, 85, 85);"> 都需要经过编译，那么是直接编译成 </font><font style="color:rgb(199, 37, 78);">VNode</font><font style="color:rgb(85, 85, 85);"> 树好呢？还是编译成由 </font><font style="color:rgb(199, 37, 78);">h</font><font style="color:rgb(85, 85, 85);"> 函数组成的调用集合好呢？这个其实很难说，但可以肯定的一点是，我们将可</font>**<font style="color:rgb(85, 85, 85);">公用、灵活、复杂的逻辑封装成函数，并交给运行时</font>**<font style="color:rgb(85, 85, 85);">，这能够大大降低编译器的书写难度，甚至经过编译器生成的代码也具有一定的可读性，而 </font><font style="color:rgb(199, 37, 78);">h</font><font style="color:rgb(85, 85, 85);"> 函数就是众多运行时函数中的一个。</font>

<br/>

## 在VNode创建时确定其类型 - flags
<font style="color:rgb(44, 62, 80);">一个最简单的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数如下：</font>

```javascript
function h() {
  return {
    _isVNode: true,
    flags: VNodeFlags.ELEMENT_HTML,
    tag: 'h1',
    data: null,
    children: null,
    childFlags: ChildrenFlags.NO_CHILDREN,
    el: null
  }
}
```

<font style="color:rgb(44, 62, 80);">这个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数只能用来创建一个空的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><h1></h1></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签，可以说没有任何意义。为了让</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数更加灵活，我们可以增加一些参数，问题是哪些内容应该提取到参数中呢？如果提取的参数多了，就会导致函数的使用不便，如果提取的参数少了又会导致函数的功能不足，所以这也是一个探索的过程。实际上只需要把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">提取为参数即可：</font>

```javascript
function h(tag, data = null, children = null) {
  //...
}
```

<font style="color:rgb(44, 62, 80);">我们来看看为什么三个参数就能满足需求，对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_isVNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，它的值始终都为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">，所以不需要提取到参数中。对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，我们可以通过检查</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值的特征来确定该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值，如下：</font>

```javascript
function h(tag, data = null, children = null) {
  let flags = null
  if (typeof tag === 'string') {
    flags = tag === 'svg' ? VNodeFlags.ELEMENT_SVG : VNodeFlags.ELEMENT_HTML
  }
}
```

<font style="color:rgb(44, 62, 80);">如上代码所示，如果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是字符串则可以确定该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是标签元素，再次通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag === 'svg'</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">进一步判断是否是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SVG</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签，从而确定了该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型。</font>

**<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">TIP</font>**

<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">请注意区分下文中出现的</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">，有时指的是</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">对象的</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">属性，有时指的是</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">函数的第一个参数。</font>

<font style="color:rgb(44, 62, 80);">对于 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);"> 类型的 </font>**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>**<font style="color:rgb(44, 62, 80);">，它的 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> 属性值为 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">，但是纯文本类型的 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 其 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> 属性值也是 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">，所以为了区分，我们可以增加一个唯一的标识，当 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> 函数的第一个参数(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);">)的值等于该标识的时候，则意味着创建的是 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);"> 类型的 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
// 唯一标识
export const Fragment = Symbol()
function h(tag, data = null, children = null) {
  let flags = null
  if (typeof tag === 'string') {
    flags = tag === 'svg' ? VNodeFlags.ELEMENT_SVG : VNodeFlags.ELEMENT_HTML
  } else if (tag === Fragment) {
    flags = VNodeFlags.FRAGMENT
  }
}
```

<font style="color:rgb(44, 62, 80);">这时我们可以像如下这样调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数创建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
import { h, Fragment } from 'vue'

h(Fragment, null, children)
```

<font style="color:rgb(44, 62, 80);">类似的，对于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型的</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>**<font style="color:rgb(44, 62, 80);">，它的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值也可以是字符串，这就会与普通标签元素类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">冲突，导致无法区分一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">到底是普通标签元素还是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);">，所以我们参考</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的实现方式，增加一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标识：</font>

<font style="color:rgb(44, 62, 80);background-color:rgb(40, 44, 52);"></font>

```javascript
export const Fragment = Symbol()
export const Portal = Symbol()
function h(tag, data = null, children = null) {
  let flags = null
  if (typeof tag === 'string') {
    flags = tag === 'svg' ? VNodeFlags.ELEMENT_SVG : VNodeFlags.ELEMENT_HTML
  } else if (tag === Fragment) {
    flags = VNodeFlags.FRAGMENT
  } else if (tag === Portal) {
    flags = VNodeFlags.PORTAL
    tag = data && data.target
  }
}
```

<font style="color:rgb(44, 62, 80);">这里需要注意的一点是，类型为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值存储的是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">挂载的目标，即</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">target</font><font style="color:rgb(44, 62, 80);">。通常模板在经过编译后，我们把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">target</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数据存储在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中，所以我们看到如上代码中包含</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag = data && data.target</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">如果一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值不满足以上全部条件，那只有一种可能了，即该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是组件。有的同学可能会说，也有可能是文本节点啊，没错，但是我们很少会用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数去创建一个文本节点，而是单纯的使用字符串，然后在内部实现中检测到该字符串的寓意是文本节点的时候会为其自动创建一个纯文本的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象，例如：</font>

<font style="color:rgb(44, 62, 80);background-color:rgb(40, 44, 52);"></font>

```javascript
h(
  'div',
  {
    style: { color: 'red' }
  },
  '我是文本'
)
```

<font style="color:rgb(44, 62, 80);">如上代码所示，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数的第三个参数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值是一段文本字符串，这时在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数内部会为这段文本字符串创建一个与之相符的纯文本</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象。</font>

<font style="color:rgb(44, 62, 80);">我们回过头来，继续讨论当一个 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 是组件时，如何根据 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> 属性确定该 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 对象的 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> 值，很简单如下：</font>

```javascript
// 省略...
function h(tag, data = null, children = null) {
  let flags = null
  if (typeof tag === 'string') {
    flags = tag === 'svg' ? VNodeFlags.ELEMENT_SVG : VNodeFlags.ELEMENT_HTML
  } else if (tag === Fragment) {
    flags = VNodeFlags.FRAGMENT
  } else if (tag === Portal) {
    flags = VNodeFlags.PORTAL
    tag = data && data.target
  } else {
    // 兼容 Vue2 的对象式组件
    if (tag !== null && typeof tag === 'object') {
      flags = tag.functional
        ? VNodeFlags.COMPONENT_FUNCTIONAL       // 函数式组件
        : VNodeFlags.COMPONENT_STATEFUL_NORMAL  // 有状态组件
    } else if (typeof tag === 'function') {
      // Vue3 的类组件
      flags = tag.prototype && tag.prototype.render
        ? VNodeFlags.COMPONENT_STATEFUL_NORMAL  // 有状态组件
        : VNodeFlags.COMPONENT_FUNCTIONAL       // 函数式组件
    }
  }
}
```

<font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue2</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中用一个对象作为组件的描述，而在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中，有状态组件是一个继承了基类的类。所以如果是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue2</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的对象式组件，我们通过检查该对象的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">functional</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的真假来判断该组件是否是函数式组件。在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中，因为有状态组件会继承基类，所以通过原型链判断其原型中是否有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数的定义来确定该组件是否是有状态组件。</font>

<font style="color:rgb(44, 62, 80);">一旦确定了一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型，那么</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数就可返回带有正确类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
function h(tag, data = null, children = null) {
  let flags = null
  // 省略用来确定 flags 的代码

  return {
    flags,
    // 其他属性...
  }
}
```

## [](https://www.123fe.net/principle-docs/vue/05-%E8%BE%85%E5%8A%A9%E5%88%9B%E5%BB%BA%20VNode%20%E7%9A%84%20h%20%E5%87%BD%E6%95%B0.html#%E5%9C%A8vnode%E5%88%9B%E5%BB%BA%E6%97%B6%E7%A1%AE%E5%AE%9A%E5%85%B6children%E7%9A%84%E7%B1%BB%E5%9E%8B)在VNode创建时确定其children的类型
<font style="color:rgb(44, 62, 80);">上文通过</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">检测</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">属性值</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来确定一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值。类似地，可以通过</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">检测</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来确定</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">childFlags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值。根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数的调用方式可以很容易地确定参数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">包含哪些值：</font>

+ <font style="color:rgb(44, 62, 80);">1、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是一个数组：</font>

```javascript
h('ul', null, [
  h('li'),
  h('li')
])
```

+ <font style="color:rgb(44, 62, 80);">2、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象：</font>

```javascript
h('div', null, h('span'))
```

+ <font style="color:rgb(44, 62, 80);">3、没有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
h('div')
```

+ <font style="color:rgb(44, 62, 80);">4、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是一个普通文本字符串：</font>

```javascript
h('div', null, '我是文本')
```

<font style="color:rgb(44, 62, 80);">以上是四种常见的 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> 函数的调用方式，根据这四种调用方式中 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> 的不同形式即可确定一个 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 对象的 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">childFlags</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(44, 62, 80);background-color:rgb(40, 44, 52);">  
</font>

```javascript
function h(tag, data = null, children = null) {
  // 省略用于确定 flags 相关的代码

  let childFlags = null
  if (Array.isArray(children)) {
    const { length } = children
    if (length === 0) {
      // 没有 children
      childFlags = ChildrenFlags.NO_CHILDREN
    } else if (length === 1) {
      // 单个子节点
      childFlags = ChildrenFlags.SINGLE_VNODE
      children = children[0]
    } else {
      // 多个子节点，且子节点使用key
      childFlags = ChildrenFlags.KEYED_VNODES
      children = normalizeVNodes(children)
    }
  } else if (children == null) {
    // 没有子节点
    childFlags = ChildrenFlags.NO_CHILDREN
  } else if (children._isVNode) {
    // 单个子节点
    childFlags = ChildrenFlags.SINGLE_VNODE
  } else {
    // 其他情况都作为文本节点处理，即单个子节点，会调用 createTextVNode 创建纯文本类型的 VNode
    childFlags = ChildrenFlags.SINGLE_VNODE
    children = createTextVNode(children + '')
  }
}
```

<font style="color:rgb(44, 62, 80);">首先，如果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是数组，则根据数组的长度来判断</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型到底是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildrenFlags.NO_CHILDREN</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildrenFlags.SINGLE_VNODE</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">还是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildrenFlags.KEYED_VNODES</font><font style="color:rgb(44, 62, 80);">。这里大家可能会有疑问：“为什么多个子节点时会直接被当做使用了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的子节点？”，这是因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是可以人为创造的，如下是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">normalizeVNodes</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数的简化：</font>

```javascript
function normalizeVNodes(children) {
  const newChildren = []
  // 遍历 children
  for (let i = 0; i < children.length; i++) {
    const child = children[i]
    if (child.key == null) {
      // 如果原来的 VNode 没有key，则使用竖线(|)与该VNode在数组中的索引拼接而成的字符串作为key
      child.key = '|' + i
    }
    newChildren.push(child)
  }
  // 返回新的children，此时 children 的类型就是 ChildrenFlags.KEYED_VNODES
  return newChildren
}
```

<font style="color:rgb(44, 62, 80);">如上，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">normalizeVNodes</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数接收</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数组作为参数，并遍历该数组，对于数组中没有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象，为其添加一个由竖线(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">|</font><font style="color:rgb(44, 62, 80);">)与该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在数组中的索引拼接而成的字符串作为该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">如果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不是数组，则判断</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是否为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null/undefined</font><font style="color:rgb(44, 62, 80);">，条件为真则说明没有子节点，所以</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">childFlags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildrenFlags.NO_CHILDREN</font><font style="color:rgb(44, 62, 80);">。如果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null/undefined</font><font style="color:rgb(44, 62, 80);">，并且</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children._isVNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为真，则说明</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就是单个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象，即单个子节点</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildrenFlags.SINGLE_VNODE</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">最后，如果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不满足以上任何条件，则会把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">作为纯文本节点的文本内容处理，这时会使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createTextVNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数创建一个纯文本类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createTextVNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数接收一个字符串作为参数，创建一个与之相符的纯文本类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，如下：</font>

```javascript
function createTextVNode(text) {
  return {
    _isVNode: true,
    // flags 是 VNodeFlags.TEXT
    flags: VNodeFlags.TEXT,
    tag: null,
    data: null,
    // 纯文本类型的 VNode，其 children 属性存储的是与之相符的文本内容
    children: text,
    // 文本节点没有子节点
    childFlags: ChildrenFlags.NO_CHILDREN,
    el: null
  }
}
```

**<font style="color:rgb(44, 62, 80);">这里再次强调！！！</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，以上用于确定</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">childFlags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的代码仅限于非组件类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，因为对于组件类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来说，它并没有子节点，所有子节点都应该作为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">slots</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">存在，所以如果使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数创建一个组件类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，那么我们应该把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的内容转化为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">slots</font><font style="color:rgb(44, 62, 80);">，然后再把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">置为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">，这些内容我们会在合适的章节讲解。</font>

## [](https://www.123fe.net/principle-docs/vue/05-%E8%BE%85%E5%8A%A9%E5%88%9B%E5%BB%BA%20VNode%20%E7%9A%84%20h%20%E5%87%BD%E6%95%B0.html#%E4%BD%BF%E7%94%A8-h-%E5%87%BD%E6%95%B0%E5%88%9B%E5%BB%BA-vnode)使用 h 函数创建 VNode
**<font style="background-color:rgb(243, 245, 247);">TIP</font>**

<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">本章关于</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">函数的完整代码&在线体验地址：</font>[https://codesandbox.io/s/6x2nvmmxn3(opens new window)](https://codesandbox.io/s/6x2nvmmxn3)

<font style="color:rgb(44, 62, 80);">假设有如下模板：</font>

```html
<template>
  <div>
    <span></span>
  </div>
</template>
```

<font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数来创建与之相符的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
const elementVNode = h('div', null, h('span'))
```

<font style="color:rgb(44, 62, 80);">得到的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象如下：</font>

```javascript
const elementVNode = {
  _isVNode: true,
  flags: 1, // VNodeFlags.ELEMENT_HTML
  tag: 'div',
  data: null,
  children: {
    _isVNode: true,
    flags: 1, // VNodeFlags.ELEMENT_HTML
    tag: 'span',
    data: null,
    children: null,
    childFlags: 1, // ChildrenFlags.NO_CHILDREN
    el: null
  },
  childFlags: 2, // ChildrenFlags.SINGLE_VNODE
  el: null
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">---- 我是一条分割线(^o^)/~ ---</font>

<font style="color:rgb(44, 62, 80);">假设有如下模板：</font>

```html
<template>
  <div>我是文本</div>
</template>
```

<font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数来创建与之相符的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
const elementWithTextVNode = h('div', null, '我是文本')
```

<font style="color:rgb(44, 62, 80);">得到的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象如下：</font>

```javascript
const elementWithTextVNode = {
  _isVNode: true,
  flags: 1, // VNodeFlags.ELEMENT_HTML
  tag: 'div',
  data: null,
  children: {
    _isVNode: true,
    flags: 64,  // VNodeFlags.TEXT
    tag: null,
    data: null,
    children: '我是文本',
    childFlags: 1, // ChildrenFlags.NO_CHILDREN
    el: null
  },
  childFlags: 2, // ChildrenFlags.SINGLE_VNODE
  el: null
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">---- 我是一条分割线(^o^)/~ ---</font>

<font style="color:rgb(44, 62, 80);">假设有如下模板：</font>

```html
<template>
  <td></td>
  <td></td>
</template>
```

<font style="color:rgb(44, 62, 80);">我们在之前的章节中曾经遇到过此模板，我们知道这种模板拥有多个根节点，它是一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);">，我们可以像如下这样使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数来创建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
import { h, Fragment } from './h'
const fragmentVNode = h(Fragment, null, [
  h('td'), h('td')
])
```

<font style="color:rgb(44, 62, 80);">得到的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象如下：</font>

```javascript
const fragmentVNode = {
  _isVNode: true,
  flags: 128, // VNodeFlags.FRAGMENT
  data: null,
  children: [
    {
      _isVNode: true,
      flags: 1, // VNodeFlags.ELEMENT_HTML
      tag: 'td',
      data: null,
      children: null,
      childFlags: 1,  // ChildrenFlags.NO_CHILDREN
      key: '|0', // 自动生成的 key
      el: null
    },
    {
      _isVNode: true,
      flags: 1, // VNodeFlags.ELEMENT_HTML
      tag: 'td',
      data: null,
      children: null,
      childFlags: 1,  // ChildrenFlags.NO_CHILDREN
      key: '|1', // 自动生成的 key
      el: null
    }
  ],
  childFlags: 4, // ChildrenFlags.KEYED_VNODES
  el: null
}
```

<font style="color:rgb(44, 62, 80);">观察如上</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象可以发现，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数组中的每一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">都自动添加了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">---- 我是一条分割线(^o^)/~ ---</font>

<font style="color:rgb(44, 62, 80);">假设有如下模板：</font>

```html
<template>
  <Portal target="box">
    <h1></h1>
  </Portal>
</template>
```

<font style="color:rgb(44, 62, 80);">这段模板是一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);">，并且会将其内容渲染到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">id="box"</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的元素下。我们可以像如下这样使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数来创建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
import { h, Portal } from './h'
const portalVNode = h(
  Portal,
  {
    target: 'box'
  },
  h('h1')
)
```

<font style="color:rgb(44, 62, 80);">得到的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象如下：</font>

```javascript
const portalVNode = {
  _isVNode: true,
  flags: 256, // VNodeFlags.PORTAL
  tag: 'box',  // 类型为 Portal 的 VNode，其 tag 属性值等于 data.target
  data: { target: 'box' },
  children: {
    _isVNode: true,
    flags: 1, // VNodeFlags.ELEMENT_HTML
    tag: 'h1',
    data: null,
    children: null,
    childFlags: 1, // ChildrenFlags.NO_CHILDREN
    el: null
  },
  childFlags: 2, // ChildrenFlags.SINGLE_VNODE
  el: null
}
```

<font style="color:rgb(44, 62, 80);">如上</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象所示，类型为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值等于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data.target</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">---- 我是一条分割线(^o^)/~ ---</font>

<font style="color:rgb(44, 62, 80);">假设有如下模板：</font>

```html
<template>
  <MyFunctionalComponent>
    <div></div>
  </MyFunctionalComponent>
</template>
```

<font style="color:rgb(44, 62, 80);">这段模板中包含了一个函数式组件，并为该组件提供了一个空的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签作为默认的插槽内容，我们尝试用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数创建与该模板相符的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
// 一个函数式组件
function MyFunctionalComponent() {}

// 传递给 h 函数的第一个参数就是组件函数本身
const functionalComponentVNode = h(MyFunctionalComponent, null, h('div'))
```

<font style="color:rgb(44, 62, 80);">来看一下我们最终得到的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象：</font>

```javascript
const functionalComponentVNode = {
  _isVNode: true,
  flags: 32,  // VNodeFlags.COMPONENT_FUNCTIONAL
  tag: MyFunctionalComponent, // tag 属性值引用组件函数
  data: null,
  children: {
    _isVNode: true,
    flags: 1,
    tag: 'div',
    data: null,
    children: null,
    childFlags: 1,
    el: null
  },
  childFlags: 2, // ChildrenFlags.SINGLE_VNODE
  el: null
}
```

<font style="color:rgb(44, 62, 80);">大家观察如上</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象，其中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值就是组件函数本身，另外</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">functionalComponentVNode.children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值就是作为默认插槽的空</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，我们暂且这样设计，因为它不会影响我们对渲染器的理解，等到讲解插槽的章节时再来说明：为什么我们不使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性来存储插槽内容，以及我们应该如何使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来描述插槽。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">---- 我是一条分割线(^o^)/~ ---</font>

<font style="color:rgb(44, 62, 80);">我们再来使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数创建一个有状态组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
// 有状态组件
class MyStatefulComponent {}
const statefulComponentVNode = h(MyStatefulComponent, null, h('div'))
```

<font style="color:rgb(44, 62, 80);">我们将得到如下 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(44, 62, 80);background-color:rgb(40, 44, 52);">  
</font>

```javascript
const statefulComponentVNode = {
  _isVNode: true,
  flags: 32,  // VNodeFlags.COMPONENT_FUNCTIONAL
  tag: MyStatefulComponent,
  data: null,
  children: {
    _isVNode: true,
    flags: 1,
    tag: 'div',
    data: null,
    children: null,
    childFlags: 1,
    el: null
  },
  childFlags: 2,
  el: null
}
```

<font style="color:rgb(44, 62, 80);">观察</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">statefulComponentVNode.flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值，我们明明使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">创建的是有状态组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，为什么最终产生的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是函数式组件呢？别急，大家还记得</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数是如何区分有状态组件和函数式组件的吗？如下是我们之前编写的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的一段用来区分函数式组件和有状态组件的代码：</font>

```javascript
// Vue3 的类组件
flags =
  tag.prototype && tag.prototype.render
  ? VNodeFlags.COMPONENT_STATEFUL_NORMAL // 有状态组件
  : VNodeFlags.COMPONENT_FUNCTIONAL // 函数式组件
```

<font style="color:rgb(44, 62, 80);">只有当组件的原型上拥有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数时才会把它当作有状态组件，这里我们再次说明为什么要这样设计，实际上我们在编写有状态组件时，通常需要继承一个框架提供好的基础组件，如下：</font>

```javascript
class MyStatefulComponent extends Component {}
```

<font style="color:rgb(44, 62, 80);">其中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件就是基础组件，而基础组件中会包含</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，如下是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Component</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件的实现：</font>

```javascript
class Component {
  render() {}
}
```

<font style="color:rgb(44, 62, 80);">那么基础组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数有什么用呢？我们知道对于一个组件来说它的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数就是它的一切，假设一个组件没有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，那么请问该组件存在的意义是什么？它要渲染的又是什么？所以在设计框架的时候，组件是必须要拥有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数的，如果没有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数我们需要打印错误信息来提示用户，这个“警示”的工作就是由基础组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数来完成的，如下：</font>

```javascript
class Component {
  render() {
    throw '组件缺少 render 函数'
  }
}
```

<font style="color:rgb(44, 62, 80);">它是如何起作用的呢？还记得我们在“组件的本质”一章中曾经讲到过的挂载组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountComponent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数吗：</font>

```javascript
function mountComponent(vnode, container) {
  // 创建组件实例
  const instance = new vnode.tag()
  // 渲染
  instance.$vnode = instance.render()
  // 挂载
  mountElement(instance.$vnode, container)
}
```

<font style="color:rgb(44, 62, 80);">在挂载组件时我们会创建组件实例，并调用组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，由于任何组件都会继承基础组件，所以一旦组件没有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数，那么基础组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数将被调用，此时就会抛出一个异常提示用户：“你的组件缺少</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数”。</font>

<font style="color:rgb(44, 62, 80);">实际上，基础组件还会做更多的任务，本章不会展开讨论。大家只需要知道</font>**<font style="color:rgb(44, 62, 80);">在设计有状态组件时，我们会设计一个基础组件，所有组件都会继承基础组件，并且基础组件拥有用来报告错误信息的</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">函数</font>**<font style="color:rgb(44, 62, 80);">，这就是我们可以通过以下代码来区分函数式组件和有状态组件的原因：</font>

```javascript
// Vue3 的类组件
flags =
  tag.prototype && tag.prototype.render
  ? VNodeFlags.COMPONENT_STATEFUL_NORMAL // 有状态组件
  : VNodeFlags.COMPONENT_FUNCTIONAL // 函数式组件
```

<font style="color:rgb(44, 62, 80);">了解了这些，我们再来使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数创建有状态组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，如下：</font>

```javascript
// 有状态组件应该继承 Component
class MyStatefulComponent extends Component {}
const statefulComponentVNode = h(MyStatefulComponent, null, h('div'))
```

<font style="color:rgb(44, 62, 80);">此时我们得到的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象如下：</font>

```javascript
const statefulComponentVNode = {
  _isVNode: true,
  flags: 4, // VNodeFlags.COMPONENT_STATEFUL_NORMAL
  data: null,
  children: {
    _isVNode: true,
    flags: 1,
    tag: 'div',
    data: null,
    children: null,
    childFlags: 1,
    el: null
  },
  childFlags: 2,
  el: null
}
```

<font style="color:rgb(44, 62, 80);">可以看到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">statefulComponentVNode.flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值已经修正了。</font>

<font style="color:rgb(44, 62, 80);">至此，我们的 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);"> 函数已经可以创建任何类型的 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 对象了，有了 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 对象，我们下一步要做的就是将 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 对象渲染成真实 DOM，下一章我们将开启渲染器之旅。</font>

