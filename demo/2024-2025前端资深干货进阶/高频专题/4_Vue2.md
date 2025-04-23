### <font style="color:rgb(44, 62, 80);">响应式原理</font>
**<font style="color:rgb(44, 62, 80);">响应式</font>**

+ <font style="color:rgb(44, 62, 80);">组件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">数据一旦变化，立刻触发视图的更新</font>
+ <font style="color:rgb(44, 62, 80);">实现数据驱动视图的第一步</font>
+ <font style="color:rgb(44, 62, 80);">核心</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">API</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font>
    - **<font style="color:rgb(44, 62, 80);">缺点</font>**
        * <font style="color:rgb(44, 62, 80);">深度监听，需要递归到底，一次计算量大</font>
        * <font style="color:rgb(44, 62, 80);">无法监听新增属性、删除属性（使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.set</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.delete</font><font style="color:rgb(44, 62, 80);">可以）</font>
        * <font style="color:rgb(44, 62, 80);">无法监听原生数组，需要重写数组原型</font>

```javascript
// 触发更新视图
function updateView() {
  console.log('视图更新')
}

// 重新定义数组原型
const oldArrayProperty = Array.prototype
// 创建新对象，原型指向 oldArrayProperty ，再扩展新的方法不会影响原型
const arrProto = Object.create(oldArrayProperty);
['push', 'pop', 'shift', 'unshift', 'splice'].forEach(methodName => {
  arrProto[methodName] = function () {
    updateView() // 触发视图更新
    oldArrayProperty[methodName].call(this, ...arguments)
    // Array.prototype.push.call(this, ...arguments)
  }
})

// 重新定义属性，监听起来
function defineReactive(target, key, value) {
  // 深度监听
  observer(value)

  // 核心 API
  Object.defineProperty(target, key, {
    get() {
      return value
    },
    set(newValue) {
      if (newValue !== value) {
        // 深度监听
        observer(newValue)

        // 设置新值
        // 注意，value 一直在闭包中，此处设置完之后，再 get 时也是会获取最新的值
        value = newValue

        // 触发更新视图
        updateView()
      }
    }
  })
}

// 监听对象属性
function observer(target) {
  if (typeof target !== 'object' || target === null) {
    // 不是对象或数组
    return target
  }

  // 污染全局的 Array 原型
  // Array.prototype.push = function () {
  //     updateView()
  //     ...
  // }

  if (Array.isArray(target)) {
    target.__proto__ = arrProto
  }

  // 重新定义各个属性（for in 也可以遍历数组）
  for (let key in target) {
    defineReactive(target, key, target[key])
  }
}

// 准备数据
const data = {
  name: 'zhangsan',
  age: 20,
  info: {
    address: 'shenzhen' // 需要深度监听
  },
  nums: [10, 20, 30]
}

// 监听数据
observer(data)

// 测试
// data.name = 'lisi'
// data.age = 21
// // console.log('age', data.age)
// data.x = '100' // 新增属性，监听不到 —— 所以有 Vue.set
// delete data.name // 删除属性，监听不到 —— 所有已 Vue.delete
// data.info.address = '上海' // 深度监听
data.nums.push(4) // 监听数组
```

```javascript
// proxy-demo

// const data = {
//     name: 'zhangsan',
//     age: 20,
// }
const data = ['a', 'b', 'c']

const proxyData = new Proxy(data, {
  get(target, key, receiver) {
    // 只处理本身（非原型的）属性
    const ownKeys = Reflect.ownKeys(target)
    if (ownKeys.includes(key)) {
      console.log('get', key) // 监听
    }

    const result = Reflect.get(target, key, receiver)
    return result // 返回结果
  },
  set(target, key, val, receiver) {
    // 重复的数据，不处理
    if (val === target[key]) {
      return true
    }

    const result = Reflect.set(target, key, val, receiver)
    console.log('set', key, val)
    // console.log('result', result) // true
    return result // 是否设置成功
  },
  deleteProperty(target, key) {
    const result = Reflect.deleteProperty(target, key)
    console.log('delete property', key)
    // console.log('result', result) // true
    return result // 是否删除成功
  }
})
```

### [](https://www.123fe.net/docs/base/high-frequency.html#vdom%E5%92%8Cdiff%E7%AE%97%E6%B3%95)<font style="color:rgb(44, 62, 80);">vdom和diff算法</font>
**<font style="color:rgb(44, 62, 80);">1. vdom</font>**

+ **<font style="color:rgb(44, 62, 80);">背景</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">操作非常耗时</font>
    - <font style="color:rgb(44, 62, 80);">以前用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jQuery</font><font style="color:rgb(44, 62, 80);">，可以自行控制</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">操作时机，手动调整</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);">是数据驱动视图，如何有效控制</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">操作</font>
+ **<font style="color:rgb(44, 62, 80);">解决方案VDOM</font>**
    - <font style="color:rgb(44, 62, 80);">有了一定的复杂度，想减少计算次数比较难</font>
    - <font style="color:rgb(44, 62, 80);">能不能把计算，更多的转移为JS计算？因为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">执行速度很快</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">模拟</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">结构，计算出最小的变更，操作</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font>
+ **<font style="color:rgb(44, 62, 80);">用JS模拟DOM结构</font>**<font style="color:rgb(44, 62, 80);">  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783869731-8b35835e-1263-448c-a841-36fbef55713e.png)
+ **<font style="color:rgb(44, 62, 80);">通过snabbdom学习vdom</font>**
    - <font style="color:rgb(44, 62, 80);">简洁强大的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);">库</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue2</font><font style="color:rgb(44, 62, 80);">参考它实现的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font>
    - **<font style="color:rgb(44, 62, 80);">snabbdom</font>**
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);">函数</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);">数据结构</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);">函数</font>
+ **<font style="color:rgb(44, 62, 80);">vdom总结</font>**
    - <font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">模拟</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">结构（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);">）</font>
    - <font style="color:rgb(44, 62, 80);">新旧</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);">对比，得出最小的更新范围，有效控制</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">操作</font>
    - <font style="color:rgb(44, 62, 80);">数据驱动视图模式下，有效控制</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">操作</font>

**<font style="color:rgb(44, 62, 80);">2. diff算法</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">算法是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);">中最核心、最关键的部分</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">算法能在日常使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);">中提现出来（如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">）</font>

**<font style="color:rgb(44, 62, 80);">树的diff的时间复杂度O(n^3)</font>**

+ <font style="color:rgb(44, 62, 80);">第一，遍历</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tree1</font>
+ <font style="color:rgb(44, 62, 80);">第二，遍历</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tree2</font>
+ <font style="color:rgb(44, 62, 80);">第三，排序</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1000</font><font style="color:rgb(44, 62, 80);">个节点，要计算</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">亿次，算法不可用</font>

**<font style="color:rgb(44, 62, 80);">优化时间复杂度到O(n)</font>**

+ <font style="color:rgb(44, 62, 80);">只比较同一层级，不跨级比较</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);">不相同，则直接删掉重建，不再深度比较</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tag</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">相同，则认为是相同节点，不再深度比较</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783870797-389e60db-ad16-4c2a-9caa-58235d648655.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783873211-f2b38518-9da7-4754-ad09-7feaf9d03a0f.png)

**<font style="color:rgb(44, 62, 80);">diff过程细节</font>**

+ <font style="color:rgb(44, 62, 80);">新旧节点都有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">，执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateChildren</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">对比</font><font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783863401-bf133920-f97e-46d3-a2bb-95e9f3c586df.png)
    - <font style="color:rgb(44, 62, 80);">开始和开始对比--头头</font>
    - <font style="color:rgb(44, 62, 80);">结束和结束对比--尾尾</font>
    - <font style="color:rgb(44, 62, 80);">开始和结束对比--头尾</font>
    - <font style="color:rgb(44, 62, 80);">结束和开始对比--尾头</font>
    - <font style="color:rgb(44, 62, 80);">以上四个都未命中：拿新节点</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，能否对应上</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">oldCh</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的某个节点的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font>
+ <font style="color:rgb(44, 62, 80);">新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">有，旧</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">无：清空旧</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text</font><font style="color:rgb(44, 62, 80);">节点，新增新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">节点</font>
+ <font style="color:rgb(44, 62, 80);">旧</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">有，新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font><font style="color:rgb(44, 62, 80);">无：移除旧</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">children</font>
+ <font style="color:rgb(44, 62, 80);">否则旧</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text</font><font style="color:rgb(44, 62, 80);">有，设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text</font><font style="color:rgb(44, 62, 80);">为空</font>

**<font style="color:rgb(44, 62, 80);">vdom和diff算法总结</font>**

+ <font style="color:rgb(44, 62, 80);">细节不重要，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updateChildren</font><font style="color:rgb(44, 62, 80);">的过程也不重要，不要深究</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);">的核心概念很重要：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);">存在的价值更重要，数据驱动视图，控制</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">操作</font>

```javascript
// snabbdom源码位于 src/snabbdom.ts
/* global module, document, Node */
import { Module } from './modules/module';
import vnode, { VNode } from './vnode';
import * as is from './is';
import htmlDomApi, { DOMAPI } from './htmldomapi';

type NonUndefined<T> = T extends undefined ? never : T;

function isUndef (s: any): boolean { return s === undefined; }
function isDef<A> (s: A): s is NonUndefined<A> { return s !== undefined; }

type VNodeQueue = VNode[];

const emptyNode = vnode('', {}, [], undefined, undefined);

function sameVnode (vnode1: VNode, vnode2: VNode): boolean {
  // key 和 sel 都相等
  // undefined === undefined // true
  return vnode1.key === vnode2.key && vnode1.sel === vnode2.sel;
}

function isVnode (vnode: any): vnode is VNode {
  return vnode.sel !== undefined;
}

type KeyToIndexMap = {[key: string]: number};

type ArraysOf<T> = {
  [K in keyof T]: Array<T[K]>;
}

type ModuleHooks = ArraysOf<Module>;

function createKeyToOldIdx (children: VNode[], beginIdx: number, endIdx: number): KeyToIndexMap {
  const map: KeyToIndexMap = {};
  for (let i = beginIdx; i <= endIdx; ++i) {
    const key = children[i]?.key;
    if (key !== undefined) {
      map[key] = i;
    }
  }
  return map;
}

const hooks: Array<keyof Module> = ['create', 'update', 'remove', 'destroy', 'pre', 'post'];

export { h } from './h';
export { thunk } from './thunk';

export function init (modules: Array<Partial<Module>>, domApi?: DOMAPI) {
  let i: number, j: number, cbs = ({} as ModuleHooks);

  const api: DOMAPI = domApi !== undefined ? domApi : htmlDomApi;

  for (i = 0; i < hooks.length; ++i) {
    cbs[hooks[i]] = [];
    for (j = 0; j < modules.length; ++j) {
      const hook = modules[j][hooks[i]];
      if (hook !== undefined) {
        (cbs[hooks[i]] as any[]).push(hook);
      }
    }
  }

  function emptyNodeAt (elm: Element) {
    const id = elm.id ? '' + elm.id : '';
    const c = elm.className ? '.' + elm.className.split(' ').join('.') : '';
    return vnode(api.tagName(elm).toLowerCase() + id + c, {}, [], undefined, elm);
  }

  function createRmCb (childElm: Node, listeners: number) {
    return function rmCb () {
      if (--listeners === 0) {
        const parent = api.parentNode(childElm);
        api.removeChild(parent, childElm);
      }
    };
  }

  function createElm (vnode: VNode, insertedVnodeQueue: VNodeQueue): Node {
    let i: any, data = vnode.data;
    if (data !== undefined) {
      const init = data.hook?.init;
      if (isDef(init)) {
        init(vnode);
        data = vnode.data;
      }
    }
    let children = vnode.children, sel = vnode.sel;
    if (sel === '!') {
      if (isUndef(vnode.text)) {
        vnode.text = '';
      }
      vnode.elm = api.createComment(vnode.text!);
    } else if (sel !== undefined) {
      // Parse selector
      const hashIdx = sel.indexOf('');
      const dotIdx = sel.indexOf('.', hashIdx);
      const hash = hashIdx > 0 ? hashIdx : sel.length;
      const dot = dotIdx > 0 ? dotIdx : sel.length;
      const tag = hashIdx !== -1 || dotIdx !== -1 ? sel.slice(0, Math.min(hash, dot)) : sel;
      const elm = vnode.elm = isDef(data) && isDef(i = data.ns)
        ? api.createElementNS(i, tag)
        : api.createElement(tag);
      if (hash < dot) elm.setAttribute('id', sel.slice(hash + 1, dot));
      if (dotIdx > 0) elm.setAttribute('class', sel.slice(dot + 1).replace(/\./g, ' '));
      for (i = 0; i < cbs.create.length; ++i) cbs.create[i](emptyNode, vnode);
      if (is.array(children)) {
        for (i = 0; i < children.length; ++i) {
          const ch = children[i];
          if (ch != null) {
            api.appendChild(elm, createElm(ch as VNode, insertedVnodeQueue));
          }
        }
      } else if (is.primitive(vnode.text)) {
        api.appendChild(elm, api.createTextNode(vnode.text));
      }
      const hook = vnode.data!.hook;
      if (isDef(hook)) {
        hook.create?.(emptyNode, vnode);
        if (hook.insert) {
          insertedVnodeQueue.push(vnode);
        }
      }
    } else {
      vnode.elm = api.createTextNode(vnode.text!);
    }
    return vnode.elm;
  }

  function addVnodes (
    parentElm: Node,
    before: Node | null,
    vnodes: VNode[],
    startIdx: number,
    endIdx: number,
    insertedVnodeQueue: VNodeQueue
  ) {
    for (; startIdx <= endIdx; ++startIdx) {
      const ch = vnodes[startIdx];
      if (ch != null) {
        api.insertBefore(parentElm, createElm(ch, insertedVnodeQueue), before);
      }
    }
  }

  function invokeDestroyHook (vnode: VNode) {
    const data = vnode.data;
    if (data !== undefined) {
      data?.hook?.destroy?.(vnode);
      for (let i = 0; i < cbs.destroy.length; ++i) cbs.destroy[i](vnode);
      if (vnode.children !== undefined) {
        for (let j = 0; j < vnode.children.length; ++j) {
          const child = vnode.children[j];
          if (child != null && typeof child !== "string") {
            invokeDestroyHook(child);
          }
        }
      }
    }
  }

  function removeVnodes (parentElm: Node,
    vnodes: VNode[],
    startIdx: number,
    endIdx: number): void {
    for (; startIdx <= endIdx; ++startIdx) {
      let listeners: number, rm: () => void, ch = vnodes[startIdx];
      if (ch != null) {
        if (isDef(ch.sel)) {
          invokeDestroyHook(ch); // hook 操作

          // 移除 DOM 元素
          listeners = cbs.remove.length + 1;
          rm = createRmCb(ch.elm!, listeners);
          for (let i = 0; i < cbs.remove.length; ++i) cbs.remove[i](ch, rm);
          const removeHook = ch?.data?.hook?.remove;
          if (isDef(removeHook)) {
            removeHook(ch, rm);
          } else {
            rm();
          }
        } else { // Text node
          api.removeChild(parentElm, ch.elm!);
        }
      }
    }
  }

  // diff算法核心
  function updateChildren (parentElm: Node,
    oldCh: VNode[],
    newCh: VNode[],
    insertedVnodeQueue: VNodeQueue) {
    let oldStartIdx = 0, newStartIdx = 0;
    let oldEndIdx = oldCh.length - 1;
    let oldStartVnode = oldCh[0];
    let oldEndVnode = oldCh[oldEndIdx];
    let newEndIdx = newCh.length - 1;
    let newStartVnode = newCh[0];
    let newEndVnode = newCh[newEndIdx];
    let oldKeyToIdx: KeyToIndexMap | undefined;
    let idxInOld: number;
    let elmToMove: VNode;
    let before: any;

    while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
      if (oldStartVnode == null) {
        oldStartVnode = oldCh[++oldStartIdx]; // Vnode might have been moved left
      } else if (oldEndVnode == null) {
        oldEndVnode = oldCh[--oldEndIdx];
      } else if (newStartVnode == null) {
        newStartVnode = newCh[++newStartIdx];
      } else if (newEndVnode == null) {
        newEndVnode = newCh[--newEndIdx];

      // 开始和开始对比--头头
      } else if (sameVnode(oldStartVnode, newStartVnode)) {
        patchVnode(oldStartVnode, newStartVnode, insertedVnodeQueue);
        oldStartVnode = oldCh[++oldStartIdx];
        newStartVnode = newCh[++newStartIdx];
      
      // 结束和结束对比--尾尾
      } else if (sameVnode(oldEndVnode, newEndVnode)) {
        patchVnode(oldEndVnode, newEndVnode, insertedVnodeQueue);
        oldEndVnode = oldCh[--oldEndIdx];
        newEndVnode = newCh[--newEndIdx];

      // 开始和结束对比--头尾
      } else if (sameVnode(oldStartVnode, newEndVnode)) { // Vnode moved right
        patchVnode(oldStartVnode, newEndVnode, insertedVnodeQueue);
        api.insertBefore(parentElm, oldStartVnode.elm!, api.nextSibling(oldEndVnode.elm!));
        oldStartVnode = oldCh[++oldStartIdx];
        newEndVnode = newCh[--newEndIdx];

      // 结束和开始对比--尾头
      } else if (sameVnode(oldEndVnode, newStartVnode)) { // Vnode moved left
        patchVnode(oldEndVnode, newStartVnode, insertedVnodeQueue);
        api.insertBefore(parentElm, oldEndVnode.elm!, oldStartVnode.elm!);
        oldEndVnode = oldCh[--oldEndIdx];
        newStartVnode = newCh[++newStartIdx];

      // 以上四个都未命中
      } else {
        if (oldKeyToIdx === undefined) {
          oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx);
        }
        // 拿新节点 key ，能否对应上 oldCh 中的某个节点的 key
        idxInOld = oldKeyToIdx[newStartVnode.key as string];
  
        // 没对应上
        if (isUndef(idxInOld)) { // New element
          api.insertBefore(parentElm, createElm(newStartVnode, insertedVnodeQueue), oldStartVnode.elm!);
          newStartVnode = newCh[++newStartIdx];
        
        // 对应上了
        } else {
          // 对应上 key 的节点
          elmToMove = oldCh[idxInOld];

          // sel 是否相等（sameVnode 的条件）
          if (elmToMove.sel !== newStartVnode.sel) {
            // New element
            api.insertBefore(parentElm, createElm(newStartVnode, insertedVnodeQueue), oldStartVnode.elm!);
          
          // sel 相等，key 相等
          } else {
            patchVnode(elmToMove, newStartVnode, insertedVnodeQueue);
            oldCh[idxInOld] = undefined as any;
            api.insertBefore(parentElm, elmToMove.elm!, oldStartVnode.elm!);
          }
          newStartVnode = newCh[++newStartIdx];
        }
      }
    }
    if (oldStartIdx <= oldEndIdx || newStartIdx <= newEndIdx) {
      if (oldStartIdx > oldEndIdx) {
        before = newCh[newEndIdx + 1] == null ? null : newCh[newEndIdx + 1].elm;
        addVnodes(parentElm, before, newCh, newStartIdx, newEndIdx, insertedVnodeQueue);
      } else {
        removeVnodes(parentElm, oldCh, oldStartIdx, oldEndIdx);
      }
    }
  }

  function patchVnode (oldVnode: VNode, vnode: VNode, insertedVnodeQueue: VNodeQueue) {
    // 执行 prepatch hook
    const hook = vnode.data?.hook;
    hook?.prepatch?.(oldVnode, vnode);

    // 设置 vnode.elem
    const elm = vnode.elm = oldVnode.elm!;
  
    // 旧 children
    let oldCh = oldVnode.children as VNode[];
    // 新 children
    let ch = vnode.children as VNode[];

    if (oldVnode === vnode) return;
  
    // hook 相关
    if (vnode.data !== undefined) {
      for (let i = 0; i < cbs.update.length; ++i) cbs.update[i](oldVnode, vnode);
      vnode.data.hook?.update?.(oldVnode, vnode);
    }

    // vnode.text === undefined （vnode.children 一般有值）
    if (isUndef(vnode.text)) {
      // 新旧都有 children
      if (isDef(oldCh) && isDef(ch)) {
        if (oldCh !== ch) updateChildren(elm, oldCh, ch, insertedVnodeQueue);
      // 新 children 有，旧 children 无 （旧 text 有）
      } else if (isDef(ch)) {
        // 清空 text
        if (isDef(oldVnode.text)) api.setTextContent(elm, '');
        // 添加 children
        addVnodes(elm, null, ch, 0, ch.length - 1, insertedVnodeQueue);
      // 旧 child 有，新 child 无
      } else if (isDef(oldCh)) {
        // 移除 children
        removeVnodes(elm, oldCh, 0, oldCh.length - 1);
      // 旧 text 有
      } else if (isDef(oldVnode.text)) {
        api.setTextContent(elm, '');
      }

    // else : vnode.text !== undefined （vnode.children 无值）
    } else if (oldVnode.text !== vnode.text) {
      // 移除旧 children
      if (isDef(oldCh)) {
        removeVnodes(elm, oldCh, 0, oldCh.length - 1);
      }
      // 设置新 text
      api.setTextContent(elm, vnode.text!);
    }
    hook?.postpatch?.(oldVnode, vnode);
  }

  return function patch (oldVnode: VNode | Element, vnode: VNode): VNode {
    let i: number, elm: Node, parent: Node;
    const insertedVnodeQueue: VNodeQueue = [];
    // 执行 pre hook
    for (i = 0; i < cbs.pre.length; ++i) cbs.pre[i]();

    // 第一个参数不是 vnode
    if (!isVnode(oldVnode)) {
      // 创建一个空的 vnode ，关联到这个 DOM 元素
      oldVnode = emptyNodeAt(oldVnode);
    }

    // 相同的 vnode（key 和 sel 都相等）
    if (sameVnode(oldVnode, vnode)) {
      // vnode 对比
      patchVnode(oldVnode, vnode, insertedVnodeQueue);
    
    // 不同的 vnode ，直接删掉重建
    } else {
      elm = oldVnode.elm!;
      parent = api.parentNode(elm);

      // 重建
      createElm(vnode, insertedVnodeQueue);

      if (parent !== null) {
        api.insertBefore(parent, vnode.elm!, api.nextSibling(elm));
        removeVnodes(parent, [oldVnode], 0, 0);
      }
    }

    for (i = 0; i < insertedVnodeQueue.length; ++i) {
      insertedVnodeQueue[i].data!.hook!.insert!(insertedVnodeQueue[i]);
    }
    for (i = 0; i < cbs.post.length; ++i) cbs.post[i]();
    return vnode;
  };
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E6%A8%A1%E6%9D%BF%E7%BC%96%E8%AF%91)<font style="color:rgb(44, 62, 80);">模板编译</font>
**<font style="color:rgb(44, 62, 80);">前置知识</font>**

+ <font style="color:rgb(44, 62, 80);">模板是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(44, 62, 80);">开发中最常用的，即与使用相关联的原理</font>
+ <font style="color:rgb(44, 62, 80);">它不是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);">，有指令、插值、JS表达式，能实现循环、判断，因此模板一定转为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">代码，即模板编译</font>
+ <font style="color:rgb(44, 62, 80);">面试不会直接问，但会通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">组件渲染和更新过程</font><font style="color:rgb(44, 62, 80);">考察</font>

**<font style="color:rgb(44, 62, 80);">模板编译</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue template compiler</font><font style="color:rgb(44, 62, 80);">将模板编译为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">函数</font>
+ <font style="color:rgb(44, 62, 80);">执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">函数，生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font>
+ <font style="color:rgb(44, 62, 80);">基于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);">在执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">webpack vue loader</font><font style="color:rgb(44, 62, 80);">，会在开发环境下编译模板</font>

**<font style="color:rgb(44, 62, 80);">with语法</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783865801-32dd46ed-7f8e-4bc0-a4b3-c2a7bb24e387.png)

+ <font style="color:rgb(44, 62, 80);">改变</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{}</font><font style="color:rgb(44, 62, 80);">内自由变量的查找规则，当做</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj</font><font style="color:rgb(44, 62, 80);">属性来查找</font>
+ <font style="color:rgb(44, 62, 80);">如果找不到匹配的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj</font><font style="color:rgb(44, 62, 80);">属性，就会报错</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">with</font><font style="color:rgb(44, 62, 80);">要慎用，它打破了作用域规则，易读性变差</font>

**<font style="color:rgb(44, 62, 80);">vue组件中使用render代替template</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783868141-89b68cdd-46ef-4a43-ba05-35247666be7d.png)

```javascript
// 执行 node index.js

const compiler = require('vue-template-compiler')

// 插值
const template = `<p>{{message}}</p>`
with(this){return _c('p', [_v(_s(message))])}
// this就是vm的实例, message等变量会从vm上读取，触发getter
// _c => createElement 也就是h函数 => 返回vnode
// _v => createTextVNode 
// _s => toString 
// 也就是这样 with(this){return createElement('p',[createTextVNode(toString(message))])}

// h -> vnode
// createElement -> vnode

// 表达式
const template = `<p>{{flag ? message : 'no message found'}}</p>`
// with(this){return _c('p',[_v(_s(flag ? message : 'no message found'))])}

// 属性和动态属性
const template = `
    <div id="div1" class="container">
        <img :src="imgUrl"/>
    </div>
`
with(this){return _c('div',
                     {staticClass:"container",attrs:{"id":"div1"}},
                     [
                       _c('img',{attrs:{"src":imgUrl}})])}

// 条件
const template = `
    <div>
        <p v-if="flag === 'a'">A</p>
        <p v-else>B</p>
    </div>
`
with(this){return _c('div',[(flag === 'a')?_c('p',[_v("A")]):_c('p',[_v("B")])])}

// 循环
const template = `
    <ul>
        <li v-for="item in list" :key="item.id">{{item.title}}</li>
    </ul>
`
with(this){return _c('ul',_l((list),function(item){return _c('li',{key:item.id},[_v(_s(item.title))])}),0)}

// 事件
const template = `
    <button @click="clickHandler">submit</button>
`
with(this){return _c('button',{on:{"click":clickHandler}},[_v("submit")])}

// v-model
const template = `<input type="text" v-model="name">`
// 主要看 input 事件
with(this){return _c('input',{directives:[{name:"model",rawName:"v-model",value:(name),expression:"name"}],attrs:{"type":"text"},domProps:{"value":(name)},on:{"input":function($event){if($event.target.composing)return;name=$event.target.value}}})}

// render 函数
// 返回 vnode
// patch

// 编译
const res = compiler.compile(template)
console.log(res.render)

// ---------------分割线--------------

// 从 vue 源码中找到缩写函数的含义
function installRenderHelpers (target) {
  target._o = markOnce;
  target._n = toNumber;
  target._s = toString;
  target._l = renderList;
  target._t = renderSlot;
  target._q = looseEqual;
  target._i = looseIndexOf;
  target._m = renderStatic;
  target._f = resolveFilter;
  target._k = checkKeyCodes;
  target._b = bindObjectProps;
  target._v = createTextVNode;
  target._e = createEmptyVNode;
  target._u = resolveScopedSlots;
  target._g = bindObjectListeners;
  target._d = bindDynamicKeys;
  target._p = prependModifier;
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#vue%E7%BB%84%E4%BB%B6%E6%B8%B2%E6%9F%93%E8%BF%87%E7%A8%8B)<font style="color:rgb(44, 62, 80);">Vue组件渲染过程</font>
**<font style="color:rgb(44, 62, 80);">前言</font>**

+ <font style="color:rgb(44, 62, 80);">一个组件渲染到页面，修改</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">触发更新（数据驱动视图）</font>
+ <font style="color:rgb(44, 62, 80);">其背后原理是什么，需要掌握哪些点</font>
+ <font style="color:rgb(44, 62, 80);">考察对流程了解的全面程度</font>

**<font style="color:rgb(44, 62, 80);">回顾三大核心知识点</font>**

+ **<font style="color:rgb(44, 62, 80);">响应式</font>**<font style="color:rgb(44, 62, 80);">：监听</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">属性</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getter</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setter</font><font style="color:rgb(44, 62, 80);">（包括数组）</font>
+ **<font style="color:rgb(44, 62, 80);">模板编译</font>**<font style="color:rgb(44, 62, 80);">：模板到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">函数，再到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font>
+ **<font style="color:rgb(44, 62, 80);">vdom</font>**<font style="color:rgb(44, 62, 80);">：两种用法</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch(elem,vnode)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">首次渲染</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);">到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">container</font><font style="color:rgb(44, 62, 80);">上</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch(vnode、newVnode)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);">取更新旧的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font>
+ <font style="color:rgb(44, 62, 80);">搞定这三点核心原理，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(44, 62, 80);">原理不是问题</font>

**<font style="color:rgb(44, 62, 80);">组件渲染更新过程</font>**

+ **<font style="color:rgb(44, 62, 80);">1. 初次渲染过程</font>**
    - <font style="color:rgb(44, 62, 80);">解析模板为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">函数（或在开发环境已经完成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue-loader</font><font style="color:rgb(44, 62, 80);">）</font>
    - <font style="color:rgb(44, 62, 80);">触发响应式，监听</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">属性</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getter</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setter</font>
    - <font style="color:rgb(44, 62, 80);">执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">函数（执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">函数过程中，会获取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">的属性触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getter</font><font style="color:rgb(44, 62, 80);">），生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);">，在执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch(elem,vnode)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">elem</font><font style="color:rgb(44, 62, 80);">组件对应的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">节点</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">const template = <p></p></font>
        * <font style="color:rgb(44, 62, 80);">编译为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">函数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">with(this){return _c('p', [_v(_s(message))])}</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vm</font><font style="color:rgb(44, 62, 80);">的实例,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">message</font><font style="color:rgb(44, 62, 80);">等变量会从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vm</font><font style="color:rgb(44, 62, 80);">上读取，触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getter</font><font style="color:rgb(44, 62, 80);">进行依赖收集</font>

```javascript
export default {
  data() {
    return {
      message: 'hello' // render函数执行过程中会获取message变量值，触发getter
    }
  }
}
```

+ **<font style="color:rgb(44, 62, 80);">2. 更新过程</font>**
    - <font style="color:rgb(44, 62, 80);">修改</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">，触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setter</font><font style="color:rgb(44, 62, 80);">（此前在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getter</font><font style="color:rgb(44, 62, 80);">中已被监听）</font>
    - <font style="color:rgb(44, 62, 80);">重新执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);">函数，生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newVnode</font>
    - <font style="color:rgb(44, 62, 80);">在调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch(oldVnode, newVnode)</font><font style="color:rgb(44, 62, 80);">算出最小差异，进行更新</font>
+ **<font style="color:rgb(44, 62, 80);">3. 完成流程图</font>**<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783873098-bb2e18ac-1fad-4366-ab4f-274883d4b0df.png)

**<font style="color:rgb(44, 62, 80);">异步渲染</font>**

+ <font style="color:rgb(44, 62, 80);">汇总</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">的修改，一次更新视图</font>
+ <font style="color:rgb(44, 62, 80);">减少</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">操作次数，提高性能</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783875189-54368608-7dc2-4901-8b55-06c93e206da0.png)

```plain
methods: {
    addItem() {
        this.list.push(`${Date.now()}`)
        this.list.push(`${Date.now()}`)
        this.list.push(`${Date.now()}`)

        // 1.页面渲染是异步的，$nextTick待渲染完在回调
        // 2.页面渲染时会将data的修改做整合，多次data修改也只会渲染一次
        this.$nextTick(()=>{
            const ulElem = this.$refs.ul
            console.log(ulElem.childNotes.length)
        })
    }
}
```

**<font style="color:rgb(44, 62, 80);">总结</font>**

+ <font style="color:rgb(44, 62, 80);">渲染和响应式的关系</font>
+ <font style="color:rgb(44, 62, 80);">渲染和模板编译的关系</font>
+ <font style="color:rgb(44, 62, 80);">渲染和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);">的关系</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#vue%E7%BB%84%E4%BB%B6%E4%B9%8B%E9%97%B4%E9%80%9A%E4%BF%A1%E6%96%B9%E5%BC%8F%E6%9C%89%E5%93%AA%E4%BA%9B)<font style="color:rgb(44, 62, 80);">Vue组件之间通信方式有哪些</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">Vue 组件间通信是面试常考的知识点之一，这题有点类似于开放题，你回答出越多方法当然越加分，表明你对 Vue 掌握的越熟练。</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">Vue 组件间通信只要指以下 3 类通信</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">父子组件通信</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">隔代组件通信</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">兄弟组件通信</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，下面我们分别介绍每种通信方式且会说明此种方法可适用于哪类组件间通信</font>

**<font style="color:rgb(44, 62, 80);">组件传参的各种方式</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783877819-00a2396a-bf6b-4900-9e99-57413d85dd49.png)

**<font style="color:rgb(44, 62, 80);">组件通信常用方式有以下几种</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props / $emit</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">适用 父子组件通信</font>**
    - <font style="color:rgb(44, 62, 80);">父组件向子组件传递数据是通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prop</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">传递的，子组件传递数据给父组件是通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$emit</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">触发事件来做到的</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$parent / $children(vue3废弃)</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">适用 父子组件通信</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">：如果在普通的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素上使用，引用指向的就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素；如果用在子组件上，引用就指向组件实例</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$parent / $children</font><font style="color:rgb(44, 62, 80);">：访问父组件的属性或方法 / 访问子组件的属性或方法</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">EventBus （$emit / $on）</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">适用于 父子、隔代、兄弟组件通信</font>**
    - <font style="color:rgb(44, 62, 80);">这种方法通过一个空的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例作为中央事件总线（事件中心），用它来触发事件和监听事件，从而实现任何组件间的通信，包括父子、隔代、兄弟组件</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$attrs / $listeners(vue3废弃)</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">适用于 隔代组件通信</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$attrs</font><font style="color:rgb(44, 62, 80);">：包含了父作用域中不被</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prop</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所识别 (且获取) 的特性绑定 (</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">style</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">除外 )。当一个组件没有声明任何</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prop</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">时，这里会包含所有父作用域的绑定 (</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">style</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">除外 )，并且可以通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">v-bind="$attrs"</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">传入内部组件。通常配合</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inheritAttrs</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选项一起使用，多余的属性不会被解析到标签上</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$listeners</font><font style="color:rgb(44, 62, 80);">：包含了父作用域中的 (不含</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.native</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">修饰器的)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">v-on</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">事件监听器。它可以通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">v-on="$listeners"</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">传入内部组件</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">provide / inject</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">适用于 隔代组件通信</font>**
    - <font style="color:rgb(44, 62, 80);">祖先组件中通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">provider</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来提供变量，然后在子孙组件中通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inject</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来注入变量。</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">provide / inject</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">API 主要解决了跨级组件间的通信问题，</font>**<font style="color:rgb(44, 62, 80);">不过它的使用场景，主要是子组件获取上级组件的状态</font>**<font style="color:rgb(44, 62, 80);">，跨级组件间建立了一种主动提供与依赖注入的关系</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$root</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">适用于 隔代组件通信</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">访问根组件中的属性或方法，是根组件，不是父组件。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$root</font><font style="color:rgb(44, 62, 80);">只对根组件有用</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vuex</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">适用于 父子、隔代、兄弟组件通信</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vuex</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是一个专为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">应用程序开发的状态管理模式。每一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vuex</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">应用的核心就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);">（仓库）。“store” 基本上就是一个容器，它包含着你的应用中大部分的状态 (</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">)</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vuex</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的状态存储是响应式的。当</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件从</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中读取状态的时候，若</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的状态发生变化，那么相应的组件也会相应地得到高效更新。</font>
    - <font style="color:rgb(44, 62, 80);">改变</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的状态的唯一途径就是显式地提交 (</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commit</font><font style="color:rgb(44, 62, 80);">)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mutation</font><font style="color:rgb(44, 62, 80);">。这样使得我们可以方便地跟踪每一个状态的变化。</font>

**<font style="color:rgb(44, 62, 80);">根据组件之间关系讨论组件通信最为清晰有效</font>**

+ <font style="color:rgb(44, 62, 80);">父子组件：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">props</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$emit</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$parent</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font>
+ <font style="color:rgb(44, 62, 80);">兄弟组件：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$parent</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eventbus</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vuex</font>
+ <font style="color:rgb(44, 62, 80);">跨层级关系：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eventbus</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vuex</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">provide+inject</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$attrs + $listeners</font><font style="color:rgb(44, 62, 80);">/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$root</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#vue%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%96%B9%E6%B3%95%E6%9C%89%E5%93%AA%E4%BA%9B)<font style="color:rgb(44, 62, 80);">Vue的生命周期方法有哪些</font>
1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例有一个完整的生命周期，也就是从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">开始创建</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">初始化数据</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">编译模版</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">挂载Dom -> 渲染</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">更新 -> 渲染</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">卸载</font><font style="color:rgb(44, 62, 80);">等一系列过程，我们称这是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">的生命周期</font>
2. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">生命周期总共分为8个阶段</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">创建前/后</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">载入前/后</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">更新前/后</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">销毁前/后</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">=></font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">=></font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeMount</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">=></font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Mounted</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">=></font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeUpdate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">=></font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updated</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">=></font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeDestroy</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">=></font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">destroyed</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">keep-alive</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">下：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">activated</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">deactivated</font>

| <font style="color:rgb(44, 62, 80);">生命周期vue2</font> | <font style="color:rgb(44, 62, 80);">生命周期vue3</font> | <font style="color:rgb(44, 62, 80);">描述</font> |
| --- | --- | --- |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font> | <font style="color:rgb(44, 62, 80);">在实例初始化之后，数据观测(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data observer</font><font style="color:rgb(44, 62, 80);">) 之前被调用。</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font> | <font style="color:rgb(44, 62, 80);">实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data observer</font><font style="color:rgb(44, 62, 80);">)，属性和方法的运算，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watch/event</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">事件回调。这里没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$el</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeMount</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeMount</font> | <font style="color:rgb(44, 62, 80);">在挂载开始之前被调用：相关的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数首次被调用</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">el</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">被新创建的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vm.$el</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">替换，并挂载到实例上去之后调用该钩子</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeUpdate</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeUpdate</font> | <font style="color:rgb(44, 62, 80);">组件数据更新之前调用，发生在虚拟</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">打补丁之前</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updated</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updated</font> | <font style="color:rgb(44, 62, 80);">由于数据更改导致的虚拟</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">重新渲染和打补丁，在这之后会调用该钩子</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeDestroy</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeUnmount</font> | <font style="color:rgb(44, 62, 80);">实例销毁之前调用。在这一步，实例仍然完全可用</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">destroyed</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unmounted</font> | <font style="color:rgb(44, 62, 80);">实例销毁后调用。调用后，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。</font> |


<font style="color:rgb(44, 62, 80);">其他几个生命周期</font>

| <font style="color:rgb(44, 62, 80);">生命周期vue2</font> | <font style="color:rgb(44, 62, 80);">生命周期vue3</font> | <font style="color:rgb(44, 62, 80);">描述</font> |
| --- | --- | --- |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">activated</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">activated</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">keep-alive</font><font style="color:rgb(44, 62, 80);">专属，组件被激活时调用</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">deactivated</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">deactivated</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">keep-alive</font><font style="color:rgb(44, 62, 80);">专属，组件被销毁时调用</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">errorCaptured</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">errorCaptured</font> | <font style="color:rgb(44, 62, 80);">捕获一个来自子孙组件的错误时被调用</font> |
| <font style="color:rgb(44, 62, 80);">-</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">renderTracked</font> | <font style="color:rgb(44, 62, 80);">调试钩子，响应式依赖被收集时调用</font> |
| <font style="color:rgb(44, 62, 80);">-</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">renderTriggered</font> | <font style="color:rgb(44, 62, 80);">调试钩子，响应式依赖被触发时调用</font> |
| <font style="color:rgb(44, 62, 80);">-</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">serverPrefetch</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ssr only</font><font style="color:rgb(44, 62, 80);">，组件实例在服务器上被渲染前调用</font> |


1. **<font style="color:rgb(44, 62, 80);">要掌握每个生命周期内部可以做什么事</font>**
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">初始化</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(44, 62, 80);">实例，进行数据观测。执行时组件实例还未创建，通常用于插件开发中执行一些初始化任务</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件初始化完毕，可以访问各种数据，获取接口数据等</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeMount</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">此阶段</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vm.el</font><font style="color:rgb(44, 62, 80);">虽已完成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">初始化，但并未挂载在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">el</font><font style="color:rgb(44, 62, 80);">选项上</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例已经挂载完成，可以进行一些</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">操作</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeUpdate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">更新前，可用于获取更新前各种状态。此时</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">view</font><font style="color:rgb(44, 62, 80);">层还未更新，可用于获取更新前各种状态。可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updated</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">完成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">view</font><font style="color:rgb(44, 62, 80);">层的更新，更新后，所有状态已是最新。可以执行依赖于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。 该钩子在服务器端渲染期间不被调用。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">destroyed</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以执行一些优化操作,清空定时器，解除绑定事件</font>
+ <font style="color:rgb(44, 62, 80);">vue3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeunmount</font><font style="color:rgb(44, 62, 80);">：实例被销毁前调用，可用于一些定时器或订阅的取消</font>
+ <font style="color:rgb(44, 62, 80);">vue3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unmounted</font><font style="color:rgb(44, 62, 80);">：销毁一个实例。可清理它与其它实例的连接，解绑它的全部指令及事件监听器</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783879360-e7821ced-61fe-4319-bdc5-d066ea959731.png)

```html
<div id="app">{{name}}</div>
<script>
    const vm = new Vue({
        data(){
            return {name:'poetries'}
        },
        el: 'app',
        beforeCreate(){
            // 数据观测(data observer) 和 event/watcher 事件配置之前被调用。
            console.log('beforeCreate');
        },
        created(){
            // 属性和方法的运算， watch/event 事件回调。这里没有$el
            console.log('created')
        },
        beforeMount(){
            // 相关的 render 函数首次被调用。
            console.log('beforeMount')
        },
        mounted(){
            // 被新创建的 vm.$el 替换
            console.log('mounted')
        },
        beforeUpdate(){
            //  数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。
            console.log('beforeUpdate')
        },
        updated(){
            //  由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
            console.log('updated')
        },
        beforeDestroy(){
            // 实例销毁之前调用 实例仍然完全可用
            console.log('beforeDestroy')
        },
        destroyed(){ 
            // 所有东西都会解绑定，所有的事件监听器会被移除
            console.log('destroyed')
        }
    });
    setTimeout(() => {
        vm.name = 'poetry';
        setTimeout(() => {
            vm.$destroy()  
        }, 1000);
    }, 1000);
</script>
```

1. <font style="color:rgb(44, 62, 80);">组合式API生命周期钩子</font>

<font style="color:rgb(44, 62, 80);">你可以通过在生命周期钩子前面加上 “</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">on</font><font style="color:rgb(44, 62, 80);">” 来访问组件的生命周期钩子。</font>

<font style="color:rgb(44, 62, 80);">下表包含如何在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内部调用生命周期钩子：</font>

| <font style="color:rgb(44, 62, 80);">选项式 API</font> | <font style="color:rgb(44, 62, 80);">Hook inside setup</font> |
| --- | --- |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font> | <font style="color:rgb(44, 62, 80);">不需要*</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font> | <font style="color:rgb(44, 62, 80);">不需要*</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeMount</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onBeforeMount</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onMounted</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeUpdate</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onBeforeUpdate</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updated</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onUpdated</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeUnmount</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onBeforeUnmount</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unmounted</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onUnmounted</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">errorCaptured</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onErrorCaptured</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">renderTracked</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onRenderTracked</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">renderTriggered</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onRenderTriggered</font> |


<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">因为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是围绕</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">生命周期钩子运行的，所以不需要显式地定义它们。换句话说，在这些钩子中编写的任何代码都应该直接在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数中编写</font>

```javascript
export default {
  setup() {
    // mounted
    onMounted(() => {
      console.log('Component is mounted!')
    })
  }
}
```

**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font>****<font style="color:rgb(44, 62, 80);">和</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font>****<font style="color:rgb(44, 62, 80);">谁先执行？</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font><font style="color:rgb(44, 62, 80);">:组件被创建出来，组件的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">methods</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">还没初始化好</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">：在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font><font style="color:rgb(44, 62, 80);">之间执行</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font><font style="color:rgb(44, 62, 80);">:组件被创建出来，组件的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">methods</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">已经初始化好了</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">由于在执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的时候，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">还没有创建好，所以在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数内我们是无法使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">methods</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的。所以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为了让我们避免错误的使用，直接将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数内的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">执行指向</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font>

```javascript
import { ref } from "vue"
export default {
  // setup函数是组合api的入口函数，注意在组合api中定义的变量或者方法，要在template响应式需要return{}出去
  setup(){
    let count = ref(1)
    function myFn(){
      count.value +=1
    }
    return {count,myFn}
  },

}
```

1. <font style="color:rgb(44, 62, 80);">其他问题</font>
+ **<font style="color:rgb(44, 62, 80);">什么是vue生命周期？</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">Vue 实例从创建到销毁的过程，就是生命周期。从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、销毁等一系列过程，称之为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的生命周期。</font>
+ **<font style="color:rgb(44, 62, 80);">vue生命周期的作用是什么？</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">它的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑。</font>
+ **<font style="color:rgb(44, 62, 80);">vue生命周期总共有几个阶段？</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">它可以总共分为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">8</font><font style="color:rgb(44, 62, 80);">个阶段：创建前/后、载入前/后、更新前/后、销毁前/销毁后。</font>
+ **<font style="color:rgb(44, 62, 80);">第一次页面加载会触发哪几个钩子？</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会触发下面这几个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeMount</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">。</font>
+ **<font style="color:rgb(44, 62, 80);">你的接口请求一般放在哪个生命周期中？</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">接口请求一般放在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font><font style="color:rgb(44, 62, 80);">中，但需要注意的是服务端渲染时不支持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font><font style="color:rgb(44, 62, 80);">，需要放到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font><font style="color:rgb(44, 62, 80);">中</font>
+ **<font style="color:rgb(44, 62, 80);">DOM 渲染在哪个周期中就已经完成？</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font><font style="color:rgb(44, 62, 80);">中，</font>
    - <font style="color:rgb(44, 62, 80);">注意</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vm.$nextTick</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">替换掉</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font>

```javascript
mounted: function () {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been rendered
  })
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E5%A6%82%E4%BD%95%E7%BB%9F%E4%B8%80%E7%9B%91%E5%90%ACvue%E7%BB%84%E4%BB%B6%E6%8A%A5%E9%94%99)<font style="color:rgb(44, 62, 80);">如何统一监听Vue组件报错</font>
+ **<font style="color:rgb(44, 62, 80);">window.onerror</font>**
    - <font style="color:rgb(44, 62, 80);">全局监听所有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">错误，包括异步错误</font>
    - <font style="color:rgb(44, 62, 80);">但是它是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">级别的，识别不了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">组件信息，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">内部的错误还是用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">来监听</font>
    - <font style="color:rgb(44, 62, 80);">捕捉一些</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">监听不到的错误</font>
+ **<font style="color:rgb(44, 62, 80);">errorCaptured生命周期</font>**
    - <font style="color:rgb(44, 62, 80);">监听所有</font>**<font style="color:rgb(44, 62, 80);">下级组件</font>**<font style="color:rgb(44, 62, 80);">的错误</font>
    - <font style="color:rgb(44, 62, 80);">返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">会阻止向上传播到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onerror</font>
+ **<font style="color:rgb(44, 62, 80);">errorHandler配置</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">全局错误监听，所有组件错误都会汇总到这里</font>
    - <font style="color:rgb(44, 62, 80);">但</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">errorCaptured</font><font style="color:rgb(44, 62, 80);">返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">，不会传播到这里</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onerror</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">errorHandler</font><font style="color:rgb(44, 62, 80);">互斥，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onerror</font><font style="color:rgb(44, 62, 80);">不会在被触发，这里都是全局错误监听了</font>
+ **<font style="color:rgb(44, 62, 80);">异步错误</font>**
    - <font style="color:rgb(44, 62, 80);">异步回调里的错误，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">errorHandler</font><font style="color:rgb(44, 62, 80);">监听不到</font>
    - <font style="color:rgb(44, 62, 80);">需要使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onerror</font>
+ **<font style="color:rgb(44, 62, 80);">总结</font>**
    - <font style="color:rgb(44, 62, 80);">实际工作中，三者结合使用</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(44, 62, 80);">（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(44, 62, 80);">没有被</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">catch</font><font style="color:rgb(44, 62, 80);">的报错，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onunhandledrejection</font><font style="color:rgb(44, 62, 80);">监听）和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">异步，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(44, 62, 80);">里面监听不了</font>

```javascript
window.addEventListener("unhandledrejection", event => {
  // 捕获 Promise 没有 catch 的错误
  console.info('unhandledrejection----', event)
})
Promise.reject('错误信息')
// .catch(e => console.info(e)) // catch 住了，就不会被 unhandledrejection 捕获
```

    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">errorCaptured</font><font style="color:rgb(44, 62, 80);">监听一些重要的、有风险组件的错误</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onerror</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">errorCaptured</font><font style="color:rgb(44, 62, 80);">候补全局监听</font>

```javascript
// main.js
const app = createApp(App)

// 所有组件错误都会汇总到这里
// window.onerror和errorHandler互斥，window.onerror不会在被触发，这里都是全局错误监听了
// 阻止向window.onerror传播
app.config.errorHandler = (error, vm, info) => {
  console.info('errorHandler----', error, vm, info)
}
```

```javascript
// 在app.vue最上层中监控全局组件
export default {
  mounted() {
    /**
     * msg:错误的信息
     * source:哪个文件
     * line:行
     * column:列
     * error:错误的对象
     */
    // 可以监听一切js的报错， try...catch 捕获的 error ，无法被 window.onerror 监听到
    window.onerror = function (msg, source, line, column, error) {
      console.info('window.onerror----', msg, source, line, column, error)
    }
    // 用addEventListener跟window.onerror效果一样，参数不一样
    // window.addEventListener('error', event => {
    //   console.info('window error', event)
    // })
  },
  errorCaptured: (errInfo, vm, info) => {
    console.info('errorCaptured----', errInfo, vm, info)
    // 返回false会阻止向上传播到window.onerror
    // 返回false会阻止传播到errorHandler
    // return false
  },
}
```

```vue
// ErrorDemo.vue
export default {
  name: 'ErrorDemo',
  data() {
    return {
      num: 100
    }
  },
  methods: {
    clickHandler() {
      try {
        this.num() // 报错
      } catch (ex) {
        console.error('catch.....', ex)
        // try...catch 捕获的 error ，无法被 window.onerror 监听到
      }

      this.num() // 报错
    }
  },
  mounted() {
    // 被errorCaptured捕获
    // throw new Error('mounted 报错')

    // 异步报错，errorHandler、errorCaptured监听不到，vue对异步报错监听不了，需要使用window.onerror来做
    // setTimeout(() => {
    //     throw new Error('setTimeout 报错')
    // }, 1000)
  },
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E5%9C%A8%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E4%B8%AD-%E4%BD%A0%E5%AF%B9vue%E5%81%9A%E8%BF%87%E5%93%AA%E4%BA%9B%E4%BC%98%E5%8C%96)<font style="color:rgb(44, 62, 80);">在实际工作中，你对Vue做过哪些优化</font>
+ **<font style="color:rgb(44, 62, 80);">v-if和v-show</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">v-if</font><font style="color:rgb(44, 62, 80);">彻底销毁组件</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">v-show</font><font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispaly</font><font style="color:rgb(44, 62, 80);">切换</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">none</font>
    - <font style="color:rgb(44, 62, 80);">实际工作中大部分情况下使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">v-if</font><font style="color:rgb(44, 62, 80);">就好，不要过渡优化</font>
+ **<font style="color:rgb(44, 62, 80);">v-for使用key</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">不要使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">index</font>
+ **<font style="color:rgb(44, 62, 80);">使用computed缓存</font>**
+ **<font style="color:rgb(44, 62, 80);">keep-alive缓存组件</font>**
    - <font style="color:rgb(44, 62, 80);">频繁切换的组件</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tabs</font>
    - <font style="color:rgb(44, 62, 80);">不要乱用，缓存会占用更多的内存</font>
+ **<font style="color:rgb(44, 62, 80);">异步组件</font>**
    - <font style="color:rgb(44, 62, 80);">针对体积较大的组件，如编辑器、复杂表格、复杂表单</font>
    - <font style="color:rgb(44, 62, 80);">拆包，需要时异步加载，不需要时不加载</font>
    - <font style="color:rgb(44, 62, 80);">减少主包体积，首页会加载更快</font>
    - <font style="color:rgb(44, 62, 80);">演示</font>

```vue
<!-- index.vue -->
<template>
  <Child></Child>
</template>
<script>
import { defineAsyncComponent } from 'vue'
export default {
  name: 'AsyncComponent',
  components: {
    // child体积大 异步加载才有意义
    // defineAsyncComponent vue3的写法
    Child: defineAsyncComponent(() => import(/* webpackChunkName: "async-child" */ './Child.vue'))
  }
}
</>

<!-- child.vue -->
<template>
  <p>async component child</p>
</template>
<script>
export default {
  name: 'Child',
}
</script>
```

+ **<font style="color:rgb(44, 62, 80);">路由懒加载</font>**
    - <font style="color:rgb(44, 62, 80);">项目比较大，拆分路由，保证首页先加载</font>
    - <font style="color:rgb(44, 62, 80);">演示</font>

```javascript
const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home // 直接加载
  },
  {
    path: '/about',
    name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    // 路由懒加载
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]
```

+ **<font style="color:rgb(44, 62, 80);">服务端SSR</font>**
    - <font style="color:rgb(44, 62, 80);">可使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Nuxt.js</font>
    - <font style="color:rgb(44, 62, 80);">按需优化，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSR</font><font style="color:rgb(44, 62, 80);">成本比较高</font>
+ <font style="color:rgb(44, 62, 80);">实际工作中你遇到积累的业务的优化经验也可以说</font>

**<font style="color:rgb(44, 62, 80);">连环问：你在使用Vue过程中遇到过哪些坑</font>**

+ **<font style="color:rgb(44, 62, 80);">内存泄露</font>**
    - <font style="color:rgb(44, 62, 80);">全局变量、全局事件、全局定时器没有销毁</font>
    - <font style="color:rgb(44, 62, 80);">自定义事件没有销毁</font>
+ **<font style="color:rgb(44, 62, 80);">Vue2响应式的缺陷(vue3不在有)</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">后续新增属性用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.set</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">删除属性用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.delete</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue2</font><font style="color:rgb(44, 62, 80);">并不支持数组下标的响应式。也就是说</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue2</font><font style="color:rgb(44, 62, 80);">检测不到通过下标更改数组的值</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr[index] = value</font>
+ **<font style="color:rgb(44, 62, 80);">路由切换时scroll会重新回到顶部</font>**
    - <font style="color:rgb(44, 62, 80);">这是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SPA</font><font style="color:rgb(44, 62, 80);">应用的通病，不仅仅是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font>
    - <font style="color:rgb(44, 62, 80);">如，列表页滚动到第二屏，点击详情页，再返回列表页，此时列表页组件会重新渲染回到了第一页</font>
    - **<font style="color:rgb(44, 62, 80);">解决方案</font>**
        * <font style="color:rgb(44, 62, 80);">在列表页缓存翻页过的数据和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scrollTop</font><font style="color:rgb(44, 62, 80);">的值</font>
        * <font style="color:rgb(44, 62, 80);">当再次返回列表页时，渲染列表组件，执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scrollTo(xx)</font>
        * <font style="color:rgb(44, 62, 80);">终极方案：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MPA</font><font style="color:rgb(44, 62, 80);">(多页面) +</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">App WebView</font><font style="color:rgb(44, 62, 80);">(可以打开多个页面不会销毁之前的)</font>
+ <font style="color:rgb(44, 62, 80);">日常遇到问题记录总结，下次面试就能用到</font>

