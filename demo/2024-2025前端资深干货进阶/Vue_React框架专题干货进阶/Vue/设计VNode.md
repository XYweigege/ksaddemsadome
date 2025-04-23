<br/>color1
<font style="color:rgb(44, 62, 80);">一个组件的产出是 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，渲染器(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Renderer</font><font style="color:rgb(44, 62, 80);">)的渲染目标也是 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">。可见 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 在框架设计的整个环节中都非常重要，甚至</font>**<font style="color:rgb(44, 62, 80);">设计 </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>****<font style="color:rgb(44, 62, 80);"> 本身就是在设计框架</font>**<font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 的设计还会对后续算法的性能产生影响。本节对 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 进行一定的设计，尝试用 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 描述各类渲染内容。</font>

<br/>

## 用 VNode 描述真实 DOM
<font style="color:rgb(44, 62, 80);">一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签有它的名字、属性、事件、样式、子节点等诸多信息，这些内容都需要在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中体现，我们可以用如下对象来描述一个红色背景的正方形</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素：</font>

```javascript
const elementVNode = {
  tag: 'div',
  data: {
    style: {
      width: '100px',
      height: '100px',
      backgroundColor: 'red'
    }
  }
}
```

<font style="color:rgb(44, 62, 80);">我们使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性来存储标签的名字，用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性来存储该标签的附加信息，比如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">style</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);">、事件等，通常我们把一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性称为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">为了描述子节点，我们需要给</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象添加</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，如下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象用来描述一个有子节点的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素：</font>

 

```javascript
const elementVNode = {
  tag: 'div',
  data: null,
  children: {
    tag: 'span',
    data: null
  }
}
```

<font style="color:rgb(44, 62, 80);">若有多个子节点，则可以把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性设计为一个数组：</font>

<font style="color:rgb(44, 62, 80);background-color:rgb(40, 44, 52);"></font>

```javascript
const elementVNode = {
  tag: 'div',
  data: null,
  children: [
    {
      tag: 'h1',
      data: null
    },
    {
      tag: 'p',
      data: null
    }
  ]
}
```

<font style="color:rgb(44, 62, 80);">除了标签元素之外，DOM 中还有文本节点，我们可以用如下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象来描述一个文本节点：</font>

```javascript
const textVNode = {
  tag: null,
  data: null,
  children: '文本内容'
}
```

<font style="color:rgb(44, 62, 80);">如上，由于文本节点没有标签名字，所以它的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">。由于文本节点也无需用额外的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来描述附加属性，所以其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值也是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">唯一需要注意的是我们使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性来存储一个文本节点的文本内容。有的同学可能会问：“可不可以新建一个属性</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来存储文本内容呢？”</font>

```javascript
const textVNode = {
  tag: null,
  data: null,
  children: null,
  text: '文本内容'
}
```

<font style="color:rgb(44, 62, 80);">这完全没有问题，这取决于你如何设计，但是</font>**<font style="color:rgb(44, 62, 80);">尽可能的在保证语义能够说得通的情况下复用属性，会使</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">对象更加轻量</font>**<font style="color:rgb(44, 62, 80);">，所以我们采取使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性来存储文本内容的方案。</font>

<font style="color:rgb(44, 62, 80);">如下是一个以文本节点作为子节点的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象：</font>

```javascript
const elementVNode = {
  tag: 'div',
  data: null,
  children: {
    tag: null,
    data: null,
    children: '文本内容'
  }
}
```

## [](https://www.123fe.net/principle-docs/vue/04-%E8%AE%BE%E8%AE%A1%20VNode.html#%E7%94%A8-vnode-%E6%8F%8F%E8%BF%B0%E6%8A%BD%E8%B1%A1%E5%86%85%E5%AE%B9)用 VNode 描述抽象内容
<font style="color:rgb(44, 62, 80);">什么是抽象内容呢？组件就属于抽象内容，比如你在 模板 或</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jsx</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中使用了一个组件，如下：</font>

```html
<div>
  <MyComponent />
</div>
```

<font style="color:rgb(44, 62, 80);">你的意图并不是要在页面中渲染一个名为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MyComponent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的标签元素，而是要渲染</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MyComponent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件所产出的内容。</font>

<font style="color:rgb(44, 62, 80);">但我们仍然需要使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来描述</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><MyComponent/></font><font style="color:rgb(44, 62, 80);">，并给此类用来描述组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">添加一个标识，以便在挂载的时候有办法区分一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">到底是普通的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签还是组件。</font>

<font style="color:rgb(44, 62, 80);">我们可以使用如下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象来描述上面的模板：</font>

<font style="color:rgb(44, 62, 80);background-color:rgb(40, 44, 52);">  
</font>

```javascript
const elementVNode = {
  tag: 'div',
  data: null,
  children: {
    tag: MyComponent,
    data: null
  }
}
```

<font style="color:rgb(44, 62, 80);">如上，用来描述组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值引用的就是组件类(或函数)本身，而不是标签名称字符串。所以理论上：</font>**<font style="color:rgb(44, 62, 80);">我们可以通过检查</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">属性值是否是字符串来确定一个</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">是否是普通标签</font>**<font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">除了组件之外，还有两种抽象的内容需要描述，即</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);">。我们先来了解一下什么是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以及它所解决的问题。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的寓意是要渲染一个片段，假设我们有如下模板：</font>



```html
<template>
  <table>
    <tr>
      <Columns />
    </tr>
  </table>
</template>
```

<font style="color:rgb(44, 62, 80);">组件</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Columns</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会返回多个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><td></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素：</font>

```html
<template>
  <td></td>
  <td></td>
  <td></td>
</template>
```

<font style="color:rgb(44, 62, 80);">大家思考一个问题，如上模板的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">如何表示？如果模板中只有一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">td</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签，即只有一个根元素，这很容易表示：</font>

```javascript
const elementVNode = {
  tag: 'td',
  data: null
}
```

<font style="color:rgb(44, 62, 80);">但是模板中不仅仅只有一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">td</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签，而是有多个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">td</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签，即多个根元素，这如何表示？此时我们就需要引入一个抽象元素，也就是我们要介绍的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);background-color:rgb(40, 44, 52);">  
</font>

```javascript
const Fragment = Symbol()
const fragmentVNode = {
  // tag 属性值是一个唯一标识
  tag: Fragment,
  data: null,
  children: [
    {
      tag: 'td',
      data: null
    },
    {
      tag: 'td',
      data: null
    },
    {
      tag: 'td',
      data: null
    }
  ]
}
```

<font style="color:rgb(44, 62, 80);">如上，我们把所有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">td</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签都作为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fragmentVNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的子节点，根元素并不是一个实实在在的真实 DOM，而是一个抽象的标识，即</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">当渲染器在渲染</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">时，如果发现该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);">，就只需要把该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的子节点渲染到页面。</font>

**<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">TIP</font>**

<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">在上面的代码中</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fragmentVNode.tag</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">属性的值是一个通过</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">创建的唯一标识，但实际上我们更倾向于给</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">对象添加一个</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">属性，用来代表该</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">的类型，这在本章的后面会详细说明。</font>

<font style="color:rgb(44, 62, 80);">再来看看</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);">，什么是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">呢？</font>

<font style="color:rgb(44, 62, 80);">一句话：它允许你把内容渲染到任何地方。其应用场景是，假设你要实现一个蒙层组件</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><Overlay/></font><font style="color:rgb(44, 62, 80);">，要求是该组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">z-index</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的层级最高，这样无论在哪里使用都希望它能够遮住全部内容，你可能会将其用在任何你需要蒙层的地方。</font>

```html
<template>
  <div id="box" style="z-index: -1;">
    <Overlay />
  </div>
</template>
```

<font style="color:rgb(44, 62, 80);">如上，不幸的事情发生了，在没有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的情况下，上面的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><Overlay/></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件的内容只能渲染到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">id="box"</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签下，这就会导致蒙层的层级失效甚至布局都可能会受到影响。</font>

<font style="color:rgb(44, 62, 80);">其实解决办法也很简单，假如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><Overlay/></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件要渲染的内容不受 DOM 层级关系限制，即可以渲染到任何位置，该问题将迎刃而解。</font>

<font style="color:rgb(44, 62, 80);">使用 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> 可以这样编写 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><Overlay/></font><font style="color:rgb(44, 62, 80);"> 组件的模板：</font>

```html
<template>
  <Portal target="app-root">
    <div class="overlay"></div>
  </Portal>
</template>
```

<font style="color:rgb(44, 62, 80);">其最终效果是，无论你在何处使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><Overlay/></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件，它都会把内容渲染到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">id="app-root"</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的元素下。由此可知，所谓</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就是把子节点渲染到给定的目标，我们可以使用如下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象来描述上面这段模板：</font>



```plain
const Portal = Symbol()
const portalVNode = {
  tag: Portal,
  data: {
    target: 'app-root'
  },
  children: {
    tag: 'div',
    data: {
      class: 'overlay'
    }
  }
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Portal</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Fragment</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类似，都需要一个唯一的标识，来区分其类型，目的是告诉渲染器如何渲染该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">。</font>

## [](https://www.123fe.net/principle-docs/vue/04-%E8%AE%BE%E8%AE%A1%20VNode.html#vnode-%E7%9A%84%E7%A7%8D%E7%B1%BB)VNode 的种类
<font style="color:rgb(44, 62, 80);">当</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">描述不同的事物时，其属性的值也各不相同。比如一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签的描述，那么其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值就是一个字符串，即标签的名字；如果是组件的描述，那么其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值则引用组件类(或函数)本身；如果是文本节点的描述，那么其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性值为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">最终我们发现，</font>**<font style="color:rgb(44, 62, 80);">不同类型的</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">拥有不同的设计</font>**<font style="color:rgb(44, 62, 80);">，这些差异积少成多，所以我们完全可以将它们分门别类。</font>

<font style="color:rgb(44, 62, 80);">总的来说，我们可以把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">分成五类，分别是：</font>**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html/svg</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">元素</font>**<font style="color:rgb(44, 62, 80);">、</font>**<font style="color:rgb(44, 62, 80);">组件</font>**<font style="color:rgb(44, 62, 80);">、</font>**<font style="color:rgb(44, 62, 80);">纯文本</font>**<font style="color:rgb(44, 62, 80);">、</font>**<font style="color:rgb(44, 62, 80);">Fragment</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以及</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">Portal</font>**<font style="color:rgb(44, 62, 80);">：</font>

<font style="color:rgb(44, 62, 80);">如上图所示，我们可以把组件细分为</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">有状态组件</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">函数式组件</font>**<font style="color:rgb(44, 62, 80);">。同时有状态组件还可以细分为三部分：</font>**<font style="color:rgb(44, 62, 80);">普通的有状态组件</font>**<font style="color:rgb(44, 62, 80);">、</font>**<font style="color:rgb(44, 62, 80);">需要被 keepAlive 的有状态组件</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以及</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">已经被 keepAlive 的有状态组件</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">但无论是普通的有状态组件还是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">keepAlive</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">相关的有状态组件，它们都是</font>**<font style="color:rgb(44, 62, 80);">有状态组件</font>**<font style="color:rgb(44, 62, 80);">。所以我们在设计</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">时可以将它们作为一类看待。</font>

## [](https://www.123fe.net/principle-docs/vue/04-%E8%AE%BE%E8%AE%A1%20VNode.html#%E4%BD%BF%E7%94%A8-flags-%E4%BD%9C%E4%B8%BA-vnode-%E7%9A%84%E6%A0%87%E8%AF%86)使用 flags 作为 VNode 的标识
<font style="color:rgb(44, 62, 80);">既然</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">有类别之分，我们就有必要使用一个唯一的标识，来标明某一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属于哪一类。同时给</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">添加</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">也是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">算法的优化手段之一。</font>

<font style="color:rgb(44, 62, 80);">比如在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue2</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中区分</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素还是组件亦或是普通文本，是这样做的：</font>

+ <font style="color:rgb(44, 62, 80);">1、拿到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">后先尝试把它当作组件去处理，如果成功地创建了组件，那说明该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就是组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>
+ <font style="color:rgb(44, 62, 80);">2、如果没能成功地创建组件，则检查</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode.tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是否有定义，如果有定义则当作普通标签处理</font>
+ <font style="color:rgb(44, 62, 80);">3、如果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode.tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">没有定义则检查是否是注释节点</font>
+ <font style="color:rgb(44, 62, 80);">4、如果不是注释节点，则会把它当作文本节点对待</font>

<font style="color:rgb(44, 62, 80);">以上这些判断都是在挂载(或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);">)阶段进行的，换句话说，一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">到底描述的是什么是在挂载或</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的时候才知道的。这就带来了两个难题：</font>**<font style="color:rgb(44, 62, 80);">无法从</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AOT</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">的层面优化</font>**<font style="color:rgb(44, 62, 80);">、</font>**<font style="color:rgb(44, 62, 80);">开发者无法手动优化</font>**<font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">为了解决这个问题，我们的思路是在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">创建的时候就把该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标明，这样在挂载或</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">阶段通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以直接避免掉很多消耗性能的判断，我们先提前感受一下渲染器的代码：</font>

```javascript
if (flags & VNodeFlags.ELEMENT) {
  // VNode 是普通标签
  mountElement(/* ... */)
} else if (flags & VNodeFlags.COMPONENT) {
  // VNode 是组件
  mountComponent(/* ... */)
} else if (flags & VNodeFlags.TEXT) {
  // VNode 是纯文本
  mountText(/* ... */)
}
```

<font style="color:rgb(44, 62, 80);">如上，采用了位运算，在一次挂载任务中如上判断很可能大量的进行，使用位运算在一定程度上再次拉升了运行时性能。</font>

**<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">TIP</font>**

<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">实际上</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue3</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">在</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">的优化上采用的就是</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font>[inferno(opens new window)](https://github.com/infernojs/inferno)<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">的手段。具体如何做我们会在后面的章节介绍。</font>

<font style="color:rgb(44, 62, 80);">这就意味着我们在设计</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象时，应该包含</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">字段：</font>

```javascript
// VNode 对象
{
  flags: ...
}
```

## [](https://www.123fe.net/principle-docs/vue/04-%E8%AE%BE%E8%AE%A1%20VNode.html#%E6%9E%9A%E4%B8%BE%E5%80%BC-vnodeflags)枚举值 VNodeFlags
<font style="color:rgb(44, 62, 80);">那么一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以是哪些值呢？那就看</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">有哪些种类就好了，每一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">种类我们都为其分配一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值即可，我们把它设计成一个枚举值并取名为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeFlags</font><font style="color:rgb(44, 62, 80);">，在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">javascript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里就用一个对象来表示即可：</font>

```javascript
const VNodeFlags = {
  // html 标签
  ELEMENT_HTML: 1,
  // SVG 标签
  ELEMENT_SVG: 1 << 1,

  // 普通有状态组件
  COMPONENT_STATEFUL_NORMAL: 1 << 2,
  // 需要被keepAlive的有状态组件
  COMPONENT_STATEFUL_SHOULD_KEEP_ALIVE: 1 << 3,
  // 已经被keepAlive的有状态组件
  COMPONENT_STATEFUL_KEPT_ALIVE: 1 << 4,
  // 函数式组件
  COMPONENT_FUNCTIONAL: 1 << 5,

  // 纯文本
  TEXT: 1 << 6,
  // Fragment
  FRAGMENT: 1 << 7,
  // Portal
  PORTAL: 1 << 8
}
```

<font style="color:rgb(44, 62, 80);">如上这些枚举属性所代表的意义能够与下面的图片一一对应上：</font>

<font style="color:rgb(44, 62, 80);">我们注意到，这些枚举属性的值基本都是通过将十进制数字</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">左移不同的位数得来的。根据这些基本的枚举属性值，我们还可以派生出额外的三个标识：</font>

```javascript
// html 和 svg 都是标签元素，可以用 ELEMENT 表示
VNodeFlags.ELEMENT = VNodeFlags.ELEMENT_HTML | VNodeFlags.ELEMENT_SVG
// 普通有状态组件、需要被keepAlive的有状态组件、已经被keepAlice的有状态组件 都是“有状态组件”，统一用 COMPONENT_STATEFUL 表示
VNodeFlags.COMPONENT_STATEFUL =
  VNodeFlags.COMPONENT_STATEFUL_NORMAL |
  VNodeFlags.COMPONENT_STATEFUL_SHOULD_KEEP_ALIVE |
  VNodeFlags.COMPONENT_STATEFUL_KEPT_ALIVE
// 有状态组件 和  函数式组件都是“组件”，用 COMPONENT 表示
VNodeFlags.COMPONENT = VNodeFlags.COMPONENT_STATEFUL | VNodeFlags.COMPONENT_FUNCTIONAL
```

<font style="color:rgb(44, 62, 80);">其中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeFlags.ELEMENT</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeFlags.COMPONENT_STATEFUL</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以及</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeFlags.COMPONENT</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是由基本标识通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">按位或(|)</font><font style="color:rgb(44, 62, 80);">运算得到的，这三个派生值将用于辅助判断。</font>

<font style="color:rgb(44, 62, 80);">有了这些</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之后，我们在创建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的时候就可以预先为其打上</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);">，以标明该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型：</font>

```javascript
// html 元素节点
const htmlVnode = {
  flags: VNodeFlags.ELEMENT_HTML,
  tag: 'div',
  data: null
}

// svg 元素节点
const svgVnode = {
  flags: VNodeFlags.ELEMENT_SVG,
  tag: 'svg',
  data: null
}

// 函数式组件
const functionalComponentVnode = {
  flags: VNodeFlags.COMPONENT_FUNCTIONAL,
  tag: MyFunctionalComponent
}

// 普通的有状态组件
const normalComponentVnode = {
  flags: VNodeFlags.COMPONENT_STATEFUL_NORMAL,
  tag: MyStatefulComponent
}

// Fragment
const fragmentVnode = {
  flags: VNodeFlags.FRAGMENT,
  // 注意，由于 flags 的存在，我们已经不需要使用 tag 属性来存储唯一标识
  tag: null
}

// Portal
const portalVnode = {
  flags: VNodeFlags.PORTAL,
  // 注意，由于 flags 的存在，我们已经不需要使用 tag 属性来存储唯一标识，tag 属性用来存储 Portal 的 target
  tag: target
}
```

<font style="color:rgb(44, 62, 80);">如下是利用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeFlags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">判断</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型的例子，比如判断一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是否是组件：</font>

```javascript
// 使用按位与(&)运算
functionalComponentVnode.flags & VNodeFlags.COMPONENT // 真
normalComponentVnode.flags & VNodeFlags.COMPONENT // 真
htmlVnode.flags & VNodeFlags.COMPONENT // 假
```

<font style="color:rgb(44, 62, 80);">熟悉位运算的话，理解起来很简单。这实际上是多种位运算技巧中的一个小技巧。我们可以列一个表格：</font>

| <font style="color:rgb(44, 62, 80);">VNodeFlags</font> | <font style="color:rgb(44, 62, 80);">左移运算</font> | <font style="color:rgb(44, 62, 80);">32 位的 bit 序列(出于简略，只用 9 位表示)</font> |
| --- | --- | --- |
| <font style="color:rgb(44, 62, 80);">ELEMENT_HTML</font> | <font style="color:rgb(44, 62, 80);">无</font> | <font style="color:rgb(44, 62, 80);">00000000</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font> |
| <font style="color:rgb(44, 62, 80);">ELEMENT_SVG</font> | <font style="color:rgb(44, 62, 80);">1 << 1</font> | <font style="color:rgb(44, 62, 80);">0000000</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">0</font> |
| <font style="color:rgb(44, 62, 80);">COMPONENT_STATEFUL_NORMAL</font> | <font style="color:rgb(44, 62, 80);">1 << 2</font> | <font style="color:rgb(44, 62, 80);">000000</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">00</font> |
| <font style="color:rgb(44, 62, 80);">COMPONENT_STATEFUL_SHOULD_KEEP_ALIVE</font> | <font style="color:rgb(44, 62, 80);">1 << 3</font> | <font style="color:rgb(44, 62, 80);">00000</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">000</font> |
| <font style="color:rgb(44, 62, 80);">COMPONENT_STATEFUL_KEPT_ALIVE</font> | <font style="color:rgb(44, 62, 80);">1 << 4</font> | <font style="color:rgb(44, 62, 80);">0000</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">0000</font> |
| <font style="color:rgb(44, 62, 80);">COMPONENT_FUNCTIONAL</font> | <font style="color:rgb(44, 62, 80);">1 << 5</font> | <font style="color:rgb(44, 62, 80);">000</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">00000</font> |
| <font style="color:rgb(44, 62, 80);">TEXT</font> | <font style="color:rgb(44, 62, 80);">1 << 6</font> | <font style="color:rgb(44, 62, 80);">00</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">000000</font> |
| <font style="color:rgb(44, 62, 80);">FRAGMENT</font> | <font style="color:rgb(44, 62, 80);">1 << 7</font> | <font style="color:rgb(44, 62, 80);">0</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">0000000</font> |
| <font style="color:rgb(44, 62, 80);">PORTAL</font> | <font style="color:rgb(44, 62, 80);">1 << 8</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">00000000</font> |


<font style="color:rgb(44, 62, 80);">根据上表展示的基本</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值可以很容易地得出下表：</font>

| <font style="color:rgb(44, 62, 80);">VNodeFlags</font> | <font style="color:rgb(44, 62, 80);">32 位的比特序列(出于简略，只用 9 位表示)</font> |
| --- | --- |
| <font style="color:rgb(44, 62, 80);">ELEMENT</font> | <font style="color:rgb(44, 62, 80);">0000000</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font> |
| <font style="color:rgb(44, 62, 80);">COMPONENT_STATEFUL</font> | <font style="color:rgb(44, 62, 80);">0000</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">00</font> |
| <font style="color:rgb(44, 62, 80);">COMPONENT</font> | <font style="color:rgb(44, 62, 80);">000</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">00</font> |


<font style="color:rgb(44, 62, 80);">所以很自然的，只有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeFlags.ELEMENT_HTML</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeFlags.ELEMENT_SVG</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeFlags.ELEMENT</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">进行按位与(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">&</font><font style="color:rgb(44, 62, 80);">)运算才会得到非零值，即为真。</font>

## [](https://www.123fe.net/principle-docs/vue/04-%E8%AE%BE%E8%AE%A1%20VNode.html#children-%E5%92%8C-childrenflags)children 和 ChildrenFlags
<font style="color:rgb(44, 62, 80);">DOM 是一棵树早已家至人说，既然</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是真实渲染内容的描述，那么它必然也是一棵树。在之前的设计中，我们给</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">定义了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，用来存储子</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">。大家思考一下，一个标签的子节点会有几种情况？</font>

<font style="color:rgb(44, 62, 80);">总的来说无非有以下几种：</font>

+ <font style="color:rgb(44, 62, 80);">没有子节点</font>
+ <font style="color:rgb(44, 62, 80);">只有一个子节点</font>
+ <font style="color:rgb(44, 62, 80);">多个子节点</font>
    - <font style="color:rgb(44, 62, 80);">有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font>
    - <font style="color:rgb(44, 62, 80);">无</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font>
+ <font style="color:rgb(44, 62, 80);">不知道子节点的情况</font>

<font style="color:rgb(44, 62, 80);">我们可以用一个叫做</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildrenFlags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的对象来枚举出以上这些情况，作为一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的子节点的类型标识：</font>

```javascript
const ChildrenFlags = {
  // 未知的 children 类型
  UNKNOWN_CHILDREN: 0,
  // 没有 children
  NO_CHILDREN: 1,
  // children 是单个 VNode
  SINGLE_VNODE: 1 << 1,

  // children 是多个拥有 key 的 VNode
  KEYED_VNODES: 1 << 2,
  // children 是多个没有 key 的 VNode
  NONE_KEYED_VNODES: 1 << 3
}
```

<font style="color:rgb(44, 62, 80);">由于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildrenFlags.KEYED_VNODES</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ChildrenFlags.NONE_KEYED_VNODES</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">都属于多个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，所以我们可以派生出一个“多节点”标识，以方便程序的判断：</font>

```javascript
ChildrenFlags.MULTIPLE_VNODES = ChildrenFlags.KEYED_VNODES | ChildrenFlags.NONE_KEYED_VNODES
```

<font style="color:rgb(44, 62, 80);">这样我们判断一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的子节点是否是多个子节点就变得容易多了：</font>

```javascript
someVNode.childFlags & ChildrenFlags.MULTIPLE_VNODES
```

<br/>color1
**<font style="color:rgb(44, 62, 80);">TIP</font>**

<font style="color:rgb(44, 62, 80);">为什么 </font><font style="color:rgb(199, 37, 78);">children</font><font style="color:rgb(44, 62, 80);"> 也需要标识呢？原因只有一个：</font>**<font style="color:rgb(44, 62, 80);">为了优化</font>**<font style="color:rgb(44, 62, 80);">。在后面讲解 </font><font style="color:rgb(199, 37, 78);">diff</font><font style="color:rgb(44, 62, 80);"> 算法的章节中你将会意识到，这些信息是至关重要的。</font>

<br/>

<font style="color:rgb(44, 62, 80);">在一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象中，我们使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性来存储该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型，类似的，我们将使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">childFlags</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来存储子节点的类型，我们来举一些实际的例子：</font>

```javascript
// 没有子节点的 div 标签
const elementVNode = {
  flags: VNodeFlags.ELEMENT_HTML,
  tag: 'div',
  data: null,
  children: null,
  childFlags: ChildrenFlags.NO_CHILDREN
}

// 文本节点的 childFlags 始终都是 NO_CHILDREN
const textVNode = {
  tag: null,
  data: null,
  children: '我是文本',
  childFlags: ChildrenFlags.NO_CHILDREN
}

// 拥有多个使用了key的 li 标签作为子节点的 ul 标签
const elementVNode = {
  flags: VNodeFlags.ELEMENT_HTML,
  tag: 'ul',
  data: null,
  childFlags: ChildrenFlags.KEYED_VNODES,
  children: [
    {
      tag: 'li',
      data: null,
      key: 0
    },
    {
      tag: 'li',
      data: null,
      key: 1
    }
  ]
}

// 只有一个子节点的 Fragment
const elementVNode = {
  flags: VNodeFlags.FRAGMENT,
  tag: null,
  data: null,
  childFlags: ChildrenFlags.SINGLE_VNODE,
  children: {
    tag: 'p',
    data: null
  }
}
```

<font style="color:rgb(44, 62, 80);">但并非所有类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性都是用来存储子</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，比如组件的“子</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">”其实不应该作为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">而是应该作为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">slots</font><font style="color:rgb(44, 62, 80);">，所以我们会定义</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode.slots</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性来存储这些子</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">，不过目前来说我们还不需要深入探讨有关插槽的知识。</font>

## [](https://www.123fe.net/principle-docs/vue/04-%E8%AE%BE%E8%AE%A1%20VNode.html#vnodedata)VNodeData
<font style="color:rgb(44, 62, 80);">前面提到过，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指的是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，它是一个对象：</font>



```javascript
{
  flags: ...,
  tag: ...,
  // VNodeData
  data: {
    ...
  }
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">顾名思义，它就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数据，用于对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">进行描述。举个例子，假如一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签，则</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中可以包含</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">style</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以及一些事件，这样渲染器在渲染此</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">时，才知道这个标签的背景颜色、字体大小以及监听了哪些事件等等。所以从设计角度来讲，任何可以对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">进行描述的内容，我们都可以将其存放到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象中，如：</font>



```javascript
{
  flags: VNodeFlags.ELEMENT_HTML,
    tag: 'div',
    data: {
    class: ['class-a', 'active'],
      style: {
      background: 'red',
        color: 'green'
    },
    // 其他数据...
  }
}
```

<font style="color:rgb(44, 62, 80);">如果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型是组件，那么我们同样可以用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来描述组件，比如组件的事件、组件的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等等，假设有如下模板：</font>

```html
<MyComponent @some-event="handler" prop-a="1" />
```

<font style="color:rgb(44, 62, 80);">则其对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">应为：</font>



```javascript
{
  flags: VNodeFlags.COMPONENT_STATEFUL,
    tag: 'div',
    data: {
    on: {
      'some-event': handler
    },
    propA: '1'
    // 其他数据...
  }
}
```

<font style="color:rgb(44, 62, 80);">当然了，只要能够正确地对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">进行描述，具体的数据结构你可以随意设计。我们暂且不限制</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的固定格式。</font>

<font style="color:rgb(44, 62, 80);">在后续章节中，我们会根据需求逐渐地完善</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNodeData</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的设计。</font>

<font style="color:rgb(44, 62, 80);">至此，我们已经对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">完成了一定的设计，目前为止我们所设计的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象如下：</font>

```javascript
export interface VNode {
  // _isVNode 属性在上文中没有提到，它是一个始终为 true 的值，有了它，我们就可以判断一个对象是否是 VNode 对象
  _isVNode: true
  // el 属性在上文中也没有提到，当一个 VNode 被渲染为真实 DOM 之后，el 属性的值会引用该真实DOM
  el: Element | null
  flags: VNodeFlags
  tag: string | FunctionalComponent | ComponentClass | null
  data: VNodeData | null
  children: VNodeChildren
  childFlags: ChildrenFlags
}
```

<font style="color:rgb(44, 62, 80);">其中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_isVNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">el</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性在上文中没有提到，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_isVNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性是一个始终为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值，有了它，我们就可以判断一个对象是否是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">el</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">被渲染为真实 DOM 之前一直都是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">，当</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">被渲染为真实 DOM 之后，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">el</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值会引用该真实 DOM。</font>

<font style="color:rgb(44, 62, 80);">实际上，如果你看过 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue3</font><font style="color:rgb(44, 62, 80);"> 的源码，你会发现在源码中一个 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> 对象除了包含本节我们所讲到的这些属性之外，还包含诸如 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">handle</font><font style="color:rgb(44, 62, 80);"> 和 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">contextVNode</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">parentVNode</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">slots</font><font style="color:rgb(44, 62, 80);"> 等其他额外的属性。</font>

