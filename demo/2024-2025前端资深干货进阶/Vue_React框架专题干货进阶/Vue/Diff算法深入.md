## 一、前言
<font style="color:rgb(44, 62, 80);">有同学问：能否详细说一下 diff 算法。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">简单说：diff 算法是一种优化手段，将前后两个模块进行差异化比较，修补(更新)差异的过程叫做 patch，也叫打补丁。</font>

<font style="color:rgb(44, 62, 80);">详细的说，请阅读这篇文章，有疑问的地方欢迎联系「松宝写代码」一起讨论。</font>

<font style="color:rgb(44, 62, 80);">文章主要解决的问题：</font>

+ <font style="color:rgb(44, 62, 80);">1、为什么要说这个 diff 算法？</font>
+ <font style="color:rgb(44, 62, 80);">2、虚拟 dom 的 diff 算法</font>
+ <font style="color:rgb(44, 62, 80);">3、为什么使用虚拟 dom？</font>
+ <font style="color:rgb(44, 62, 80);">4、diff 算法的复杂度和特点？</font>
+ <font style="color:rgb(44, 62, 80);">5、vue 的模板文件是如何被编译渲染的？</font>
+ <font style="color:rgb(44, 62, 80);">6、vue2.x 和 vue3.x 中的 diff 有区别吗</font>
+ <font style="color:rgb(44, 62, 80);">7、diff 算法的源头 snabbdom 算法</font>
+ <font style="color:rgb(44, 62, 80);">8、diff 算法与 snabbdom 算法的差异地方？</font>

## [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#%E4%BA%8C%E3%80%81%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E8%AF%B4%E8%BF%99%E4%B8%AA-diff-%E7%AE%97%E6%B3%95)二、为什么要说这个 diff 算法？
<font style="color:rgb(44, 62, 80);">因为 diff 算法是 vue2.x ， vue3.x 以及 react 中关键核心点，理解 diff 算法，更有助于理解各个框架本质。</font>

<font style="color:rgb(44, 62, 80);">说到「diff 算法」，不得不说「虚拟 Dom」，因为这两个息息相关。</font>

<font style="color:rgb(44, 62, 80);">比如：</font>

+ <font style="color:rgb(44, 62, 80);">vue 的响应式原理？</font>
+ <font style="color:rgb(44, 62, 80);">vue 的 template 文件是如何被编译的？</font>
+ <font style="color:rgb(44, 62, 80);">介绍一下 Virtual Dom 算法？</font>
+ <font style="color:rgb(44, 62, 80);">为什么要用 virtual dom 呢？</font>
+ <font style="color:rgb(44, 62, 80);">diff 算法复杂度以及最大的特点？</font>
+ <font style="color:rgb(44, 62, 80);">vue2.x 的 diff 算法中节点比较情况？</font>

<font style="color:rgb(44, 62, 80);">等等</font>

## [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#%E4%B8%89%E3%80%81%E8%99%9A%E6%8B%9F-dom-%E7%9A%84-diff-%E7%AE%97%E6%B3%95)三、虚拟 dom 的 diff 算法
<font style="color:rgb(44, 62, 80);">我们先来说说虚拟 Dom，就是通过 JS 模拟实现 DOM ，接下来难点就是如何判断旧对象和新对象之间的差异。</font>

<font style="color:rgb(44, 62, 80);">Dom 是多叉树结构，如果需要完整的对比两棵树的差异，那么算法的时间复杂度 O(n ^ 3)，这个复杂度很难让人接收，尤其在 n 很大的情况下，于是 React 团队优化了算法，实现了 O(n) 的复杂度来对比差异。</font>

<font style="color:rgb(44, 62, 80);">实现 O(n) 复杂度的关键就是只对比同层的节点，而不是跨层对比，这也是考虑到在实际业务中很少会去跨层的移动 DOM 元素。</font>

<font style="color:rgb(44, 62, 80);">虚拟 DOM 差异算法的步骤分为 2 步：</font>

+ <font style="color:rgb(44, 62, 80);">首先从上至下，从左往右遍历对象，也就是树的深度遍历，这一步中会给每个节点添加索引，便于最后渲染差异</font>
+ <font style="color:rgb(44, 62, 80);">一旦节点有子元素，就去判断子元素是否有不同</font>

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#_3-1-vue-%E4%B8%AD-diff-%E7%AE%97%E6%B3%95)**<font style="color:rgb(44, 62, 80);">3.1 vue 中 diff 算法</font>**
<font style="color:rgb(44, 62, 80);">实际 diff 算法比较中，节点比较主要有 5 种规则的比较</font>

+ <font style="color:rgb(44, 62, 80);">1、如果新旧 VNode 都是静态的，同时它们的 key 相同（代表同一节点），并且新的 VNode 是 clone 或者是标记了 once（标记 v-once 属性，只渲染一次），那么只需要替换 elm 以及 componentInstance 即可。</font>
+ <font style="color:rgb(44, 62, 80);">2、新老节点均有 children 子节点，则对子节点进行 diff 操作，调用 updateChildren，这个 updateChildren 也是 diff 的核心。</font>
+ <font style="color:rgb(44, 62, 80);">3、如果老节点没有子节点而新节点存在子节点，先清空老节点 DOM 的文本内容，然后为当前 DOM 节点加入子节点。</font>
+ <font style="color:rgb(44, 62, 80);">4、当新节点没有子节点而老节点有子节点的时候，则移除该 DOM 节点的所有子节点。</font>
+ <font style="color:rgb(44, 62, 80);">5、当新老节点都无子节点的时候，只是文本的替换</font>

<font style="color:rgb(44, 62, 80);">部分源码</font><font style="color:rgb(44, 62, 80);"> </font>[https://github.com/vuejs/vue/blob/8a219e3d4cfc580bbb3420344600801bd9473390/src/core/vdom/patch.jsL501(opens new window)](https://link.zhihu.com/?target=https%3A//github.com/vuejs/vue/blob/8a219e3d4cfc580bbb3420344600801bd9473390/src/core/vdom/patch.js%23L501)<font style="color:rgb(44, 62, 80);">如下：</font>

```javascript
function patchVnode(oldVnode, vnode, insertedVnodeQueue, ownerArray, index, removeOnly) {
  if (oldVnode === vnode) {
    return;
  }

  if (isDef(vnode.elm) && isDef(ownerArray)) {
    // clone reused vnode
    vnode = ownerArray[index] = cloneVNode(vnode);
  }

  const elm = (vnode.elm = oldVnode.elm);

  if (isTrue(oldVnode.isAsyncPlaceholder)) {
    if (isDef(vnode.asyncFactory.resolved)) {
      hydrate(oldVnode.elm, vnode, insertedVnodeQueue);
    } else {
      vnode.isAsyncPlaceholder = true;
    }
    return;
  }
  if (
    isTrue(vnode.isStatic) &&
    isTrue(oldVnode.isStatic) &&
    vnode.key === oldVnode.key &&
    (isTrue(vnode.isCloned) || isTrue(vnode.isOnce))
  ) {
    vnode.componentInstance = oldVnode.componentInstance;
    return;
  }

  let i;
  const data = vnode.data;
  if (isDef(data) && isDef((i = data.hook)) && isDef((i = i.prepatch))) {
    i(oldVnode, vnode);
  }

  const oldCh = oldVnode.children;
  const ch = vnode.children;
  if (isDef(data) && isPatchable(vnode)) {
    for (i = 0; i < cbs.update.length; ++i) cbs.update[i](oldVnode, vnode);
    if (isDef((i = data.hook)) && isDef((i = i.update))) i(oldVnode, vnode);
  }
  if (isUndef(vnode.text)) {
    // 定义了子节点，且不相同，用diff算法对比
    if (isDef(oldCh) && isDef(ch)) {
      if (oldCh !== ch) updateChildren(elm, oldCh, ch, insertedVnodeQueue, removeOnly);
      // 新节点有子元素。旧节点没有
    } else if (isDef(ch)) {
      if (process.env.NODE_ENV !== 'production') {
        // 检查key
        checkDuplicateKeys(ch);
      }
      // 清空旧节点的text属性
      if (isDef(oldVnode.text)) nodeOps.setTextContent(elm, '');
      // 添加新的Vnode
      addVnodes(elm, null, ch, 0, ch.length - 1, insertedVnodeQueue);
      // 如果旧节点的子节点有内容，新的没有。那么直接删除旧节点子元素的内容
    } else if (isDef(oldCh)) {
      removeVnodes(oldCh, 0, oldCh.length - 1);
      // 如上。只是判断是否为文本节点
    } else if (isDef(oldVnode.text)) {
      nodeOps.setTextContent(elm, '');
    }
    // 如果文本节点不同，替换节点内容
  } else if (oldVnode.text !== vnode.text) {
    nodeOps.setTextContent(elm, vnode.text);
  }
  if (isDef(data)) {
    if (isDef((i = data.hook)) && isDef((i = i.postpatch))) i(oldVnode, vnode);
  }
}
```

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#_3-2-react-diff-%E7%AE%97%E6%B3%95)**<font style="color:rgb(44, 62, 80);">3.2 React diff 算法</font>**
<font style="color:rgb(44, 62, 80);">在 reconcileChildren 函数的入参中</font>

```javascript
workInProgress.child = reconcileChildFibers(
  workInProgress,
  current.child,
  nextChildren,
  renderLanes,
);
```

+ <font style="color:rgb(44, 62, 80);">workInProgress：作为父节点传入，新生成的第一个 fiber 的 return 会被指向它。</font>
+ <font style="color:rgb(44, 62, 80);">current.child：旧 fiber 节点，diff 生成新 fiber 节点时会用新生成的 ReactElement 和它作比较。</font>
+ <font style="color:rgb(44, 62, 80);">nextChildren：新生成的 ReactElement，会以它为标准生成新的 fiber 节点。</font>
+ <font style="color:rgb(44, 62, 80);">renderLanes：本次的渲染优先级，最终会被挂载到新 fiber 的 lanes 属性上。</font>

<font style="color:rgb(44, 62, 80);">diff 的两个主体是：oldFiber（current.child）和 newChildren（nextChildren，新的 ReactElement），它们是两个不一样的数据结构。</font>

<font style="color:rgb(44, 62, 80);">部分源码</font>

```javascript
function reconcileChildrenArray(
  returnFiber: Fiber,
  currentFirstChild: Fiber | null,
  newChildren: Array<*>,
  lanes: Lanes,
): Fiber | null {
  /* * returnFiber：currentFirstChild的父级fiber节点
   * currentFirstChild：当前执行更新任务的WIP（fiber）节点
   * newChildren：组件的render方法渲染出的新的ReactElement节点
   * lanes：优先级相关
   * */
  // resultingFirstChild是diff之后的新fiber链表的第一个fiber。
  let resultingFirstChild: Fiber | null = null;
  // resultingFirstChild是新链表的第一个fiber。
  // previousNewFiber用来将后续的新fiber接到第一个fiber之后
  let previousNewFiber: Fiber | null = null;

  // oldFiber节点，新的child节点会和它进行比较
  let oldFiber = currentFirstChild;
  // 存储固定节点的位置
  let lastPlacedIndex = 0;
  // 存储遍历到的新节点的索引
  let newIdx = 0;
  // 记录目前遍历到的oldFiber的下一个节点
  let nextOldFiber = null;

  // 该轮遍历来处理节点更新，依据节点是否可复用来决定是否中断遍历
  for (; oldFiber !== null && newIdx < newChildren.length; newIdx++) {
    // newChildren遍历完了，oldFiber链没有遍历完，此时需要中断遍历
    if (oldFiber.index > newIdx) {
      nextOldFiber = oldFiber;
      oldFiber = null;
    } else {
      // 用nextOldFiber存储当前遍历到的oldFiber的下一个节点
      nextOldFiber = oldFiber.sibling;
    }
    // 生成新的节点，判断key与tag是否相同就在updateSlot中
    // 对DOM类型的元素来说，key 和 tag都相同才会复用oldFiber
    // 并返回出去，否则返回null
    const newFiber = updateSlot(returnFiber, oldFiber, newChildren[newIdx], lanes);

    // newFiber为 null说明 key 或 tag 不同，节点不可复用，中断遍历
    if (newFiber === null) {
      if (oldFiber === null) {
        // oldFiber 为null说明oldFiber此时也遍历完了
        // 是以下场景，D为新增节点
        // 旧 A - B - C
        // 新 A - B - C - D oldFiber = nextOldFiber;
      }
      break;
    }
    if (shouldTrackSideEffects) {
      // shouldTrackSideEffects 为true表示是更新过程
      if (oldFiber && newFiber.alternate === null) {
        // newFiber.alternate 等同于 oldFiber.alternate
        // oldFiber为WIP节点，它的alternate 就是 current节点
        // oldFiber存在，并且经过更新后的新fiber节点它还没有current节点,
        // 说明更新后展现在屏幕上不会有current节点，而更新后WIP
        // 节点会称为current节点，所以需要删除已有的WIP节点
        deleteChild(returnFiber, oldFiber);
      }
    }
    // 记录固定节点的位置
    lastPlacedIndex = placeChild(newFiber, lastPlacedIndex, newIdx);
    // 将新fiber连接成以sibling为指针的单向链表
    if (previousNewFiber === null) {
      resultingFirstChild = newFiber;
    } else {
      previousNewFiber.sibling = newFiber;
    }
    previousNewFiber = newFiber;
    // 将oldFiber节点指向下一个，与newChildren的遍历同步移动
    oldFiber = nextOldFiber;
  }

  // 处理节点删除。新子节点遍历完，说明剩下的oldFiber都是没用的了，可以删除.
  if (newIdx === newChildren.length) {
    // newChildren遍历结束，删除掉oldFiber链中的剩下的节点
    deleteRemainingChildren(returnFiber, oldFiber);
    return resultingFirstChild;
  }

  // 处理新增节点。旧的遍历完了，能复用的都复用了，所以意味着新的都是新插入的了
  if (oldFiber === null) {
    for (; newIdx < newChildren.length; newIdx++) {
      // 基于新生成的ReactElement创建新的Fiber节点
      const newFiber = createChild(returnFiber, newChildren[newIdx], lanes);
      if (newFiber === null) {
        continue;
      }
      // 记录固定节点的位置lastPlacedIndex
      lastPlacedIndex = placeChild(newFiber, lastPlacedIndex, newIdx);
      // 将新生成的fiber节点连接成以sibling为指针的单向链表
      if (previousNewFiber === null) {
        resultingFirstChild = newFiber;
      } else {
        previousNewFiber.sibling = newFiber;
      }
      previousNewFiber = newFiber;
    }
    return resultingFirstChild;
  }
  // 执行到这是都没遍历完的情况，把剩余的旧子节点放入一个以key为键,值为oldFiber节点的map中
  // 这样在基于oldFiber节点新建新的fiber节点时，可以通过key快速地找出oldFiber
  const existingChildren = mapRemainingChildren(returnFiber, oldFiber);

  // 节点移动
  for (; newIdx < newChildren.length; newIdx++) {
    // 基于map中的oldFiber节点来创建新fiber
    const newFiber = updateFromMap(
      existingChildren,
      returnFiber,
      newIdx,
      newChildren[newIdx],
      lanes,
    );
    if (newFiber !== null) {
      if (shouldTrackSideEffects) {
        if (newFiber.alternate !== null) {
          // 因为newChildren中剩余的节点有可能和oldFiber节点一样,只是位置换了，
          // 但也有可能是是新增的.

          // 如果newFiber的alternate不为空，则说明newFiber不是新增的。
          // 也就说明着它是基于map中的oldFiber节点新建的,意味着oldFiber已经被使用了,所以需
          // 要从map中删去oldFiber
          existingChildren.delete(newFiber.key === null ? newIdx : newFiber.key);
        }
      }

      // 移动节点，多节点diff的核心，这里真正会实现节点的移动
      lastPlacedIndex = placeChild(newFiber, lastPlacedIndex, newIdx);
      // 将新fiber连接成以sibling为指针的单向链表
      if (previousNewFiber === null) {
        resultingFirstChild = newFiber;
      } else {
        previousNewFiber.sibling = newFiber;
      }
      previousNewFiber = newFiber;
    }
  }
  if (shouldTrackSideEffects) {
    // 此时newChildren遍历完了，该移动的都移动了，那么删除剩下的oldFiber
    existingChildren.forEach((child) => deleteChild(returnFiber, child));
  }
  return resultingFirstChild;
}
```

## [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#%E5%9B%9B%E3%80%81%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BD%BF%E7%94%A8%E8%99%9A%E6%8B%9F-dom)四、为什么使用虚拟 dom？
<font style="color:rgb(44, 62, 80);">很多时候手工优化 dom 确实会比 virtual dom 效率高，对于比较简单的 dom 结构用手工优化没有问题，但当页面结构很庞大，结构很复杂时，手工优化会花去大量时间，而且可维护性也不高，不能保证每个人都有手工优化的能力。至此，virtual dom 的解决方案应运而生。</font>

<font style="color:rgb(44, 62, 80);">virtual dom 是“解决过多的操作 dom 影响性能”的一种解决方案。</font>

<font style="color:rgb(44, 62, 80);">virtual dom 很多时候都不是最优的操作，但它具有普适性，在效率、可维护性之间达到平衡。</font>

**<font style="color:rgb(44, 62, 80);">virutal dom 的意义：</font>**

+ <font style="color:rgb(44, 62, 80);">1、提供一种简单对象去代替复杂的 dom 对象，从而优化 dom 操作</font>
+ <font style="color:rgb(44, 62, 80);">2、提供一个中间层，js 去写 ui，ios 安卓之类的负责渲染，就像 reactNative 一样。</font>

## [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#%E4%BA%94%E3%80%81diff-%E7%AE%97%E6%B3%95%E7%9A%84%E5%A4%8D%E6%9D%82%E5%BA%A6%E5%92%8C%E7%89%B9%E7%82%B9)五、diff 算法的复杂度和特点？
<font style="color:rgb(44, 62, 80);">vue2.x 的 diff 位于 patch.js 文件中，该算法来源于 snabbdom，复杂度为 O(n)。了解 diff 过程可以让我们更高效的使用框架。react 的 diff 其实和 vue 的 diff 大同小异。</font>

<font style="color:rgb(44, 62, 80);">最大特点：比较只会在同层级进行, 不会跨层级比较。</font>

```javascript
<!-- 之前 -->
  <div>              <!-- 层级1 -->
  <p>              <!-- 层级2 -->
  <b> aoy </b>   <!-- 层级3 -->
  <span>diff</Span>
  </p>
  </div>

  <!-- 之后 -->
  <div>             <!-- 层级1 -->
  <p>              <!-- 层级2 -->
  <b> aoy </b>   <!-- 层级3 -->
  </p>
  <span>diff</Span>
  </div>
```

<font style="color:rgb(44, 62, 80);">对比之前和之后：可能期望将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><span></font><font style="color:rgb(44, 62, 80);">直接移动到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);">的后边，这是最优的操作。</font>

<font style="color:rgb(44, 62, 80);">但是实际的 diff 操作是：</font>

+ <font style="color:rgb(44, 62, 80);">1、移除</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);">里的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><span></font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">2、在创建一个新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><span></font><font style="color:rgb(44, 62, 80);">插到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);">的后边。 因为新加的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><span></font><font style="color:rgb(44, 62, 80);">在层级 2，旧的在层级 3，属于不同层级的比较。</font>

## [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#%E5%85%AD%E3%80%81vue-%E7%9A%84%E6%A8%A1%E6%9D%BF%E6%96%87%E4%BB%B6%E6%98%AF%E5%A6%82%E4%BD%95%E8%A2%AB%E7%BC%96%E8%AF%91%E6%B8%B2%E6%9F%93%E7%9A%84)六、vue 的模板文件是如何被编译渲染的？
<font style="color:rgb(44, 62, 80);">vue 中也使用 diff 算法，有必要了解一下 Vue 是如何工作的。通过这个问题，我们可以很好的掌握，diff 算法在整个编译过程中，哪个环节，做了哪些操作，然后使用 diff 算法后输出什么？</font>

<font style="color:rgb(44, 62, 80);">解释：</font>

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#_1%E3%80%81mount-%E5%87%BD%E6%95%B0)**<font style="color:rgb(44, 62, 80);">1、mount 函数</font>**
<font style="color:rgb(44, 62, 80);">mount 函数主要是获取 template，然后进入 compileToFunctions 函数。</font>

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#_2%E3%80%81compiletofunction-%E5%87%BD%E6%95%B0)**<font style="color:rgb(44, 62, 80);">2、compileToFunction 函数</font>**
<font style="color:rgb(44, 62, 80);">compileToFunction 函数主要是将 template 编译成 render 函数。首先读取缓存，没有缓存就调用 compile 方法拿到 render 函数的字符串形式，在通过 new Function 的方式生成 render 函数。</font>

```javascript
// 有缓存的话就直接在缓存里面拿
const key = options && options.delimiters ? String(options.delimiters) + template : template;
if (cache[key]) {
  return cache[key];
}
const res = {};
const compiled = compile(template, options); // compile 后面会详细讲
res.render = makeFunction(compiled.render); //通过 new Function 的方式生成 render 函数并缓存
const l = compiled.staticRenderFns.length;
res.staticRenderFns = new Array(l);
for (let i = 0; i < l; i++) {
  res.staticRenderFns[i] = makeFunction(compiled.staticRenderFns[i]);
}

// ......

return (cache[key] = res); // 记录至缓存中
```

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#_3%E3%80%81compile-%E5%87%BD%E6%95%B0)**<font style="color:rgb(44, 62, 80);">3、compile 函数</font>**
<font style="color:rgb(44, 62, 80);">compile 函数将 template 编译成 render 函数的字符串形式。后面我们主要讲解 render</font>

<font style="color:rgb(44, 62, 80);">完成 render 方法生成后，会进入到 mount 进行 DOM 更新。该方法核心逻辑如下：</font>

```javascript
// 触发 beforeMount 生命周期钩子
callHook(vm, 'beforeMount');
// 重点：新建一个 Watcher 并赋值给 vm._watcher
vm._watcher = new Watcher(
  vm,
  function updateComponent() {
    vm._update(vm._render(), hydrating);
  },
  noop,
);
hydrating = false;
// manually mounted instance, call mounted on self
// mounted is called for render-created child components in its inserted hook
if (vm.$vnode == null) {
  vm._isMounted = true;
  callHook(vm, 'mounted');
}
return vm;
```

+ <font style="color:rgb(44, 62, 80);">首先会 new 一个 watcher 对象（主要是将模板与数据建立联系），在 watcher 对象创建后，</font>
+ <font style="color:rgb(44, 62, 80);">会运行传入的方法 vm._update(vm._render(), hydrating) 。 其中的 vm._render()主要作用就是运行前面 compiler 生成的 render 方法，并返回一个 vNode 对象。</font>
+ <font style="color:rgb(44, 62, 80);">vm.update() 则会对比新的 vdom 和当前 vdom，并把差异的部分渲染到真正的 DOM 树上。（watcher 背后的实现原理：vue2.x 的响应式原理）</font>

<font style="color:rgb(44, 62, 80);">上面提到的 compile 就是将 template 编译成 render 函数的字符串形式。核心代码如下：</font>

```javascript
export function compile(template: string, options: CompilerOptions): CompiledResult {
  const AST = parse(template.trim(), options); //1. parse
  optimize(AST, options); //2.optimize
  const code = generate(AST, options); //3.generate
  return {
    AST,
    render: code.render,
    staticRenderFns: code.staticRenderFns,
  };
}
```

<font style="color:rgb(44, 62, 80);">compile 这个函数主要有三个步骤组成：</font>

+ <font style="color:rgb(44, 62, 80);">parse，</font>
+ <font style="color:rgb(44, 62, 80);">optimize</font>
+ <font style="color:rgb(44, 62, 80);">generate</font>

<font style="color:rgb(44, 62, 80);">分别输出一个包含</font>

+ <font style="color:rgb(44, 62, 80);">AST 字符串</font>
+ <font style="color:rgb(44, 62, 80);">staticRenderFns 的对象字符串</font>
+ <font style="color:rgb(44, 62, 80);">render 函数 的字符串。</font>

<font style="color:rgb(44, 62, 80);">parse 函数：主要功能是</font>**<font style="color:rgb(44, 62, 80);">将 template 字符串解析成 AST（抽象语法树）</font>**<font style="color:rgb(44, 62, 80);">。前面定义的 ASTElement 的数据结构，parse 函数就是将 template 里的结构（指令，属性，标签） 转换为 AST 形式存进 ASTElement 中，最后解析生成 AST。</font>

<font style="color:rgb(44, 62, 80);">optimize 函数（src/compiler/optomizer.js）:主要功能是</font>**<font style="color:rgb(44, 62, 80);">标记静态节点</font>**<font style="color:rgb(44, 62, 80);">。后面 patch 过程中对比新旧 VNode 树形结构做优化。被标记为 static 的节点在后面的 diff 算法中会被直接忽略，不做详细比较。</font>

<font style="color:rgb(44, 62, 80);">generate 函数（src/compiler/codegen/index.js）:主要功能</font>**<font style="color:rgb(44, 62, 80);">根据 AST 结构拼接生成 render 函数的字符串</font>**<font style="color:rgb(44, 62, 80);">。</font>

```javascript
const code = AST ? genElement(AST) : '_c("div")';
staticRenderFns = prevStaticRenderFns;
onceCount = prevOnceCount;
return {
  render: `with(this){return ${code}}`, //最外层包一个 with(this) 之后返回
  staticRenderFns: currentStaticRenderFns,
};
```

<font style="color:rgb(44, 62, 80);">其中 genElement 函数（src/compiler/codgen/index.js）是根据 AST 的属性调用不同的方法生成字符串返回。</font>

**<font style="color:rgb(44, 62, 80);">总之：</font>**

<font style="color:rgb(44, 62, 80);">就是 compile 函数中三个核心步骤介绍，</font>

+ <font style="color:rgb(44, 62, 80);">compile 之后我们得到 render 函数的字符串形式，后面通过 new Function 得到真正的渲染函数。</font>
+ <font style="color:rgb(44, 62, 80);">数据发生变化后，会执行 watcher 中的_update 函数（src/core/instance/lifecycle.js），_update 函数会执行这个渲染函数，输出一个新的 VNode 树形结构的数据。</font>
+ <font style="color:rgb(44, 62, 80);">然后调用 patch 函数，拿到这个新的 VNode 与旧的 VNode 进行对比，只有发生了变化的节点才会被更新到新的真实 DOM 树上。</font>

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#_4%E3%80%81patch-%E5%87%BD%E6%95%B0)**<font style="color:rgb(44, 62, 80);">4、patch 函数</font>**
<font style="color:rgb(44, 62, 80);">patch 函数 就是新旧 VNode 对比的 diff 函数，主要是为了优化 dom，通过算法使操作 dom 的行为降低到最低， diff 算法来源于 snabbdom，是 VDOM 思想的核心。snabbdom 的算法是为了 DOM 操作跨级增删节点较少的这一目标进行优化， 它只会在同层级进行，不会跨层级比较。</font>

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#%E6%80%BB%E7%BB%93%E4%B8%80%E4%B8%8B)**<font style="color:rgb(44, 62, 80);">总结一下</font>**
+ <font style="color:rgb(44, 62, 80);">compile 函数主要是将 template 转换为 AST，优化 AST，再将 AST 转换为 render 函数的字符串形式。</font>
+ <font style="color:rgb(44, 62, 80);">再通过 new Function 得到真正的 render 函数，render 函数与数据通过 Watcher 产生关联。</font>
+ <font style="color:rgb(44, 62, 80);">在数据反生变化的时候调用 patch 函数，执行 render 函数，生成新的 VNode，与旧的 VNode 进行 diff，最终更新 DOM 树。</font>

## [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#%E4%B8%83%E3%80%81vue2-x-vue3-x-react-%E4%B8%AD%E7%9A%84-diff-%E6%9C%89%E5%8C%BA%E5%88%AB%E5%90%97)七、vue2.x，vue3.x，React 中的 diff 有区别吗？
<font style="color:rgb(44, 62, 80);">总的来说：</font>

+ <font style="color:rgb(44, 62, 80);">vue2.x 的核心 diff 算法采用双端比较的算法，同时从新旧 children 的两端开始进行比较，借助 key 可以复用的节点。</font>
+ <font style="color:rgb(44, 62, 80);">vue3.x 借鉴了一些别的算法 inferno(</font>[https://github.com/infernojs/inferno(opens new window)](https://link.zhihu.com/?target=https%3A//github.com/infernojs/inferno)<font style="color:rgb(44, 62, 80);">) 解决：1、处理相同的前置和后置元素的预处理；2、一旦需要进行 DOM 移动，我们首先要做的就是找到 source 的最长递增子序列。</font>

<font style="color:rgb(44, 62, 80);">在创建 VNode 就确定类型，以及在 mount/patch 的过程中采用位运算来判断一个 VNode 的类型，在这个优化的基础上再配合 Diff 算法，性能得到提升。</font>

<font style="color:rgb(44, 62, 80);">可以看一下 vue3.x 的源码：</font><font style="color:rgb(44, 62, 80);"> </font>[https://github.com/vuejs/vue/blob/8a219e3d4cfc580bbb3420344600801bd9473390/src/core/vdom/patch.js(opens new window)](https://link.zhihu.com/?target=https%3A//github.com/vuejs/vue/blob/8a219e3d4cfc580bbb3420344600801bd9473390/src/core/vdom/patch.js)

+ <font style="color:rgb(44, 62, 80);">react 通过 key 和 tag 来对节点进行取舍，可直接将复杂的比对拦截掉，然后降级成节点的移动和增删这样比较简单的操作。</font>

<font style="color:rgb(44, 62, 80);">对 oldFiber 和新的 ReactElement 节点的比对，将会生成新的 fiber 节点，同时标记上 effectTag，这些 fiber 会被连到 workInProgress 树中，作为新的 WIP 节点。树的结构因此被一点点地确定，而新的 workInProgress 节点也基本定型。在 diff 过后，workInProgress 节点的 beginWork 节点就完成了，接下来会进入 completeWork 阶段。</font>

## [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#%E5%85%AB%E3%80%81diff-%E7%AE%97%E6%B3%95%E7%9A%84%E6%BA%90%E5%A4%B4-snabbdom-%E7%AE%97%E6%B3%95)**八、diff 算法的源头 snabbdom 算法**
<font style="color:rgb(44, 62, 80);">snabbdom 算法：</font><font style="color:rgb(44, 62, 80);"> </font>[https://github.com/snabbdom/snabbdom(opens new window)](https://link.zhihu.com/?target=https%3A//github.com/snabbdom/snabbdom)

<font style="color:rgb(44, 62, 80);">定位：一个专注于简单性、模块化、强大功能和性能的虚拟 DOM 库。</font>

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#_1%E3%80%81snabbdom-%E4%B8%AD%E5%AE%9A%E4%B9%89-vnode-%E7%9A%84%E7%B1%BB%E5%9E%8B)**<font style="color:rgb(44, 62, 80);">1、snabbdom 中定义 Vnode 的类型</font>**
<font style="color:rgb(44, 62, 80);">snabbdom 中定义 Vnode 的类型(</font>[https://github.com/snabbdom/snabbdom/blob/27e9c4d5dca62b6dabf9ac23efb95f1b6045b2df/src/vnode.tsL12(opens new window)](https://link.zhihu.com/?target=https%3A//github.com/snabbdom/snabbdom/blob/27e9c4d5dca62b6dabf9ac23efb95f1b6045b2df/src/vnode.ts%23L12)<font style="color:rgb(44, 62, 80);">)</font>

```javascript
export interface VNode {
  sel: string | undefined; // selector的缩写
  data: VNodeData | undefined; // 下面VNodeData接口的内容
  children: Array<VNode | string> | undefined; // 子节点
  elm: Node | undefined; // element的缩写，存储了真实的HTMLElement
  text: string | undefined; // 如果是文本节点，则存储text
  key: Key | undefined; // 节点的key，在做列表时很有用
}

export interface VNodeData {
  props?: Props;
  attrs?: Attrs;
  class?: Classes;
  style?: VNodeStyle;
  dataset?: Dataset;
  on?: On;
  attachData?: AttachData;
  hook?: Hooks;
  key?: Key;
  ns?: string; // for SVGs
  fn?: () => VNode; // for thunks
  args?: any[]; // for thunks
  is?: string; // for custom elements v1
  [key: string]: any; // for any other 3rd party module
}
```

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#_2%E3%80%81init-%E5%87%BD%E6%95%B0%E5%88%86%E6%9E%90)**<font style="color:rgb(44, 62, 80);">2、init 函数分析</font>**
<font style="color:rgb(44, 62, 80);">init 函数的地址：</font>

[https://github.com/snabbdom/snabbdom/blob/27e9c4d5dca62b6dabf9ac23efb95f1b6045b2df/src/init.tsL63(opens new window)](https://link.zhihu.com/?target=https%3A//github.com/snabbdom/snabbdom/blob/27e9c4d5dca62b6dabf9ac23efb95f1b6045b2df/src/init.ts%23L63)

<font style="color:rgb(44, 62, 80);">init() 函数接收一个模块数组 modules 和可选的 domApi 对象作为参数，返回一个函数，即 patch() 函数。</font>

<font style="color:rgb(44, 62, 80);">domApi 对象的接口包含了很多 DOM 操作的方法。</font>

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#_3%E3%80%81patch-%E5%87%BD%E6%95%B0%E5%88%86%E6%9E%90)**<font style="color:rgb(44, 62, 80);">3、patch 函数分析</font>**
<font style="color:rgb(44, 62, 80);">源码：</font>

[https://github.com/snabbdom/snabbdom/blob/27e9c4d5dca62b6dabf9ac23efb95f1b6045b2df/src/init.tsL367(opens new window)](https://link.zhihu.com/?target=https%3A//github.com/snabbdom/snabbdom/blob/27e9c4d5dca62b6dabf9ac23efb95f1b6045b2df/src/init.ts%23L367)

+ <font style="color:rgb(44, 62, 80);">init() 函数返回了一个 patch() 函数</font>
+ <font style="color:rgb(44, 62, 80);">patch() 函数接收两个 VNode 对象作为参数，并返回一个新 VNode。</font>

### [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#_4%E3%80%81h-%E5%87%BD%E6%95%B0%E5%88%86%E6%9E%90)**<font style="color:rgb(44, 62, 80);">4、h 函数分析</font>**
<font style="color:rgb(44, 62, 80);">源码：</font>

[https://github.com/snabbdom/snabbdom/blob/27e9c4d5dca62b6dabf9ac23efb95f1b6045b2df/src/h.tsL33(opens new window)](https://link.zhihu.com/?target=https%3A//github.com/snabbdom/snabbdom/blob/27e9c4d5dca62b6dabf9ac23efb95f1b6045b2df/src/h.ts%23L33)

<font style="color:rgb(44, 62, 80);">h() 函数接收多种参数，其中必须有一个 sel 参数，作用是将节点内容挂载到该容器中，并返回一个新 VNode。</font>

## [](https://www.123fe.net/principle-docs/vue/16-diff%E7%AE%97%E6%B3%95%E6%B7%B1%E5%85%A5.html#%E4%B9%9D%E3%80%81diff-%E7%AE%97%E6%B3%95%E4%B8%8E-snabbdom-%E7%AE%97%E6%B3%95%E7%9A%84%E5%B7%AE%E5%BC%82%E5%9C%B0%E6%96%B9)九、diff 算法与 snabbdom 算法的差异地方？
<font style="color:rgb(44, 62, 80);">在 vue2.x 不是完全 snabbdom 算法，而是基于 vue 的场景进行了一些修改和优化，主要体现在判断 key 和 diff 部分。</font>

<font style="color:rgb(44, 62, 80);">1、在 snabbdom 中 通过 key 和 sel 就判断是否为同一节点，那么在 vue 中，增加了一些判断 在满足 key 相等的同时会判断，tag 名称是否一致，是否为注释节点，是否为异步节点，或者为 input 时候类型是否相同等。</font>

[https://github.com/vuejs/vue/blob/8a219e3d4cfc580bbb3420344600801bd9473390/src/core/vdom/patch.jsL35(opens new window)](https://link.zhihu.com/?target=https%3A//github.com/vuejs/vue/blob/8a219e3d4cfc580bbb3420344600801bd9473390/src/core/vdom/patch.js%23L35)

```javascript
/**
 * @param a 被对比节点
 * @param b  对比节点
 * 对比两个节点是否相同
 * 需要组成的条件：key相同，tag相同，是否都为注释节点，是否同事定义了data，如果是input标签，那么type必须相同
 */
function sameVnode(a, b) {
  return (
    a.key === b.key &&
    ((a.tag === b.tag &&
      a.isComment === b.isComment &&
      isDef(a.data) === isDef(b.data) &&
      sameInputType(a, b)) ||
     (isTrue(a.isAsyncPlaceholder) &&
      a.asyncFactory === b.asyncFactory &&
      isUndef(b.asyncFactory.error)))
  );
}
```

<font style="color:rgb(44, 62, 80);">2、diff 差异，patchVnode 是对比模版变化的函数，可能会用到 diff 也可能直接更新。</font>

[https://github.com/vuejs/vue/blob/8a219e3d4cfc580bbb3420344600801bd9473390/src/core/vdom/patch.jsL404(opens new window)](https://link.zhihu.com/?target=https%3A//github.com/vuejs/vue/blob/8a219e3d4cfc580bbb3420344600801bd9473390/src/core/vdom/patch.js%23L404)

```javascript
function updateChildren(
  parentElm,
  oldCh,
  newCh,
  insertedVnodeQueue,
  removeOnly
) {
  let oldStartIdx = 0;
  let newStartIdx = 0;
  let oldEndIdx = oldCh.length - 1;
  let oldStartVnode = oldCh[0];
  let oldEndVnode = oldCh[oldEndIdx];
  let newEndIdx = newCh.length - 1;
  let newStartVnode = newCh[0];
  let newEndVnode = newCh[newEndIdx];
  let oldKeyToIdx, idxInOld, vnodeToMove, refElm;
  const canMove = !removeOnly;

  if (process.env.NODE_ENV !== "production") {
    checkDuplicateKeys(newCh);
  }

  while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
    if (isUndef(oldStartVnode)) {
      oldStartVnode = oldCh[++oldStartIdx]; // Vnode has been moved left
    } else if (isUndef(oldEndVnode)) {
      oldEndVnode = oldCh[--oldEndIdx];
    } else if (sameVnode(oldStartVnode, newStartVnode)) {
      patchVnode(
        oldStartVnode,
        newStartVnode,
        insertedVnodeQueue,
        newCh,
        newStartIdx
      );
      oldStartVnode = oldCh[++oldStartIdx];
      newStartVnode = newCh[++newStartIdx];
    } else if (sameVnode(oldEndVnode, newEndVnode)) {
      patchVnode(
        oldEndVnode,
        newEndVnode,
        insertedVnodeQueue,
        newCh,
        newEndIdx
      );
      oldEndVnode = oldCh[--oldEndIdx];
      newEndVnode = newCh[--newEndIdx];
    } else if (sameVnode(oldStartVnode, newEndVnode)) {
      // Vnode moved right
      patchVnode(
        oldStartVnode,
        newEndVnode,
        insertedVnodeQueue,
        newCh,
        newEndIdx
      );
      canMove &&
        nodeOps.insertBefore(
          parentElm,
          oldStartVnode.elm,
          nodeOps.nextSibling(oldEndVnode.elm)
        );
      oldStartVnode = oldCh[++oldStartIdx];
      newEndVnode = newCh[--newEndIdx];
    } else if (sameVnode(oldEndVnode, newStartVnode)) {
      // Vnode moved left
      patchVnode(
        oldEndVnode,
        newStartVnode,
        insertedVnodeQueue,
        newCh,
        newStartIdx
      );
      canMove &&
        nodeOps.insertBefore(parentElm, oldEndVnode.elm, oldStartVnode.elm);
      oldEndVnode = oldCh[--oldEndIdx];
      newStartVnode = newCh[++newStartIdx];
    } else {
      if (isUndef(oldKeyToIdx))
        oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx);
      idxInOld = isDef(newStartVnode.key)
        ? oldKeyToIdx[newStartVnode.key]
        : findIdxInOld(newStartVnode, oldCh, oldStartIdx, oldEndIdx);
      if (isUndef(idxInOld)) {
        // New element
        createElm(
          newStartVnode,
          insertedVnodeQueue,
          parentElm,
          oldStartVnode.elm,
          false,
          newCh,
          newStartIdx
        );
      } else {
        // vnodeToMove将要移动的节点
        vnodeToMove = oldCh[idxInOld];
        if (sameVnode(vnodeToMove, newStartVnode)) {
          patchVnode(
            vnodeToMove,
            newStartVnode,
            insertedVnodeQueue,
            newCh,
            newStartIdx
          );
          oldCh[idxInOld] = undefined;
          canMove &&
            nodeOps.insertBefore(parentElm, vnodeToMove.elm, oldStartVnode.elm);
        } else {
          // same key but different element. treat as new element
          createElm(
            newStartVnode,
            insertedVnodeQueue,
            parentElm,
            oldStartVnode.elm,
            false,
            newCh,
            newStartIdx
          );
        }
      }
      // vnodeToMove将要移动的节点
      newStartVnode = newCh[++newStartIdx];
    }
  }
  // 旧节点完成，新的没完成
  if (oldStartIdx > oldEndIdx) {
    refElm = isUndef(newCh[newEndIdx + 1]) ? null : newCh[newEndIdx + 1].elm;
    addVnodes(
      parentElm,
      refElm,
      newCh,
      newStartIdx,
      newEndIdx,
      insertedVnodeQueue
    );
    // 新的完成，老的没完成
  } else if (newStartIdx > newEndIdx) {
    removeVnodes(oldCh, oldStartIdx, oldEndIdx);
  }
}
```

