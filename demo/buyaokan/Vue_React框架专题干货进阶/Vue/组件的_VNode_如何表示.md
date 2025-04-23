**<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">TIP</font>**

<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">在后续行文时，将统一使用</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">来简称</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是真实 DOM 的描述，比如我们可以用如下对象描述一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签：</font>

```javascript
const elementVnode = {
  tag: 'div'
}
```

<font style="color:rgb(44, 62, 80);">想要把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">elementVnode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">渲染成真实 DOM，我们还需要一个渲染器(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Renderer</font><font style="color:rgb(44, 62, 80);">)：</font>

```javascript
function render(vnode, container) {}
```

<font style="color:rgb(44, 62, 80);">渲染器接收两个参数，分别是将要渲染的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和 元素挂载点(真实 DOM 被渲染的位置)。</font>

<font style="color:rgb(44, 62, 80);">为了渲染如上的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签，我们可以这样调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数：</font>

```javascript
// 把 elementVnode 渲染到 id 为 app 的元素下
render(elementVnode, document.getElementById('app'))
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数的实现也很简单：</font>

```javascript
function render(vnode, container) {
  mountElement(vnode, container)
}

function mountElement(vnode, container) {
  // 创建元素
  const el = document.createElement(vnode.tag)
  // 将元素添加到容器
  container.appendChild(el)
}
```

<font style="color:rgb(44, 62, 80);">如上，在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数内调用了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountElement</font><font style="color:rgb(44, 62, 80);">，它的作用是根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">创建真实 DOM 并将其添加到容器中。</font>

<font style="color:rgb(44, 62, 80);">这段代码对于标准</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签是可以正常工作的(不考虑 SVG)，但并不适用于组件。为了能够渲染组件，我们需要思考：</font>**<font style="color:rgb(44, 62, 80);">组件的</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">应该如何表示？</font>**

<font style="color:rgb(44, 62, 80);">对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来说，其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值就是标签的名字，但如果是组件的话，其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值应该是什么呢？</font>

<font style="color:rgb(44, 62, 80);">很简单，我们可以将其指向组件自身。假设我们有如下组件：</font>

```javascript
class MyComponent {
  render() {
    // render 函数产出 VNode
    return {
      tag: 'div'
    }
  }
}
```

<font style="color:rgb(44, 62, 80);">如上，我们使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">定义了一个类，它是一个组件(有状态组件)，我们可以使用如下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来描述它：</font>

```javascript
const componentVnode = {
  tag: MyComponent
}
```

<font style="color:rgb(44, 62, 80);">如上，直接将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值指向组件自身。但想要正确地渲染该组件，我们还需要修改</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">：</font>

```javascript
function render(vnode, container) {
  if (typeof vnode.tag === 'string') {
    // html 标签
    mountElement(vnode, container)
  } else {
    // 组件
    mountComponent(vnode, container)
  }
}
```

<font style="color:rgb(44, 62, 80);">如上，通过</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">判断</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode.tag</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">是否是字符串</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来区分：一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">到底是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签还是组件。如果是组件的话，调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountComponent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数挂载组件，而非</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountElement</font><font style="color:rgb(44, 62, 80);">，如下是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountComponent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数的实现：</font>

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

<font style="color:rgb(44, 62, 80);">道理很简单，由于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode.tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指向组件类，所以我们创建一个组件实例，接着调用其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数产出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">并将其添加到实例属性</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">instance.$vnode</font><font style="color:rgb(44, 62, 80);">，最后借用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mountElement</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数完成标签的挂载即可。</font>

<font style="color:rgb(44, 62, 80);">实际上为了便于理解，本节代码的简化程度很大，更加详细严谨的代码会在后续的具体章节中体现。不过这足以说明问题：</font>**<font style="color:rgb(44, 62, 80);">可以让</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">的</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">属性指向组件本身，从而使用</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">来描述组件</font>**<font style="color:rgb(44, 62, 80);">。</font>

**<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">TIP</font>**

<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">完整代码&在线体验地址：</font>[https://codesandbox.io/s/8817m3zz5j](https://codesandbox.io/s/8817m3zz5j)

