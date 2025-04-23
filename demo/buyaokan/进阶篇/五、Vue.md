### <font style="color:rgb(44, 62, 80);">1 Vue 响应式原理</font>
<br/>color1
<font style="color:rgb(85, 85, 85);">Vue 的响应式原理是核心是通过 ES5 的保护对象的 </font><font style="color:rgb(199, 37, 78);">Object.defindeProperty</font><font style="color:rgb(85, 85, 85);"> 中的访问器属性中的 get 和 set 方法，data 中声明的属性都被添加了访问器属性，当读取 data 中的数据时自动调用 get 方法，当修改 data 中的数据时，自动调用 set 方法，检测到数据的变化，会通知观察者 Wacher，观察者 Wacher自动触发重新render 当前组件（子组件不会重新渲染）,生成新的虚拟 DOM 树，Vue 框架会遍历并对比新虚拟 DOM 树和旧虚拟 DOM 树中每个节点的差别，并记录下来，最后，加载操作，将所有记录的不同点，局部修改到真实 DOM树上。</font>

<br/>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781805245-22aff169-4c9b-4bbd-82cb-e7dbaed74033.png)

+ <font style="color:rgb(44, 62, 80);">虚拟DOM (Virtaul DOM): 用 js 对象模拟的，保存当前视图内所有 DOM 节点对象基本描述属性和节点间关系的树结构。用 js 对象，描述每个节点，及其父子关系，形成虚拟 DOM 对象树结构。</font>
+ <font style="color:rgb(44, 62, 80);">因为只要在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中声明的基本数据类型的数据，基本不存在数据不响应问题，所以重点介绍数组和对象在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(44, 62, 80);">中的数据响应问题，vue可以检测对象属性的修改，但无法监听数组的所有变动及对象的新增和删除，只能使用数组变异方法及</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$set</font><font style="color:rgb(44, 62, 80);">方法。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781805545-4ca09364-85d8-4cca-8d37-62a6df8ce0cd.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以看到，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arrayMethods</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">首先继承了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，然后对数组中所有能改变数组自身的方法，如</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">push</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pop</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">等这些方法进行重写。重写后的方法会先执行它们本身原有的逻辑，并对能增加数组长度的 3 个方法</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">push</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unshift</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">splice</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法做了判断，获取到插入的值，然后把新添加的值变成一个响应式对象，并且再调用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ob.dep.notify()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">手动触发依赖通知，这就很好地解释了用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vm.items.splice</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">newLength</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">) 方法可以检测到变化</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">总结：Vue 采用数据劫持结合发布—订阅模式的方法，通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来劫持各个属性的 setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1718781805742-9749dbcc-7078-40a1-b219-674a1743d868.jpeg)

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Observer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">遍历数据对象，给所有属性加上</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getter</font><font style="color:rgb(44, 62, 80);">，监听数据的变化</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">compile</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">订阅者是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Observer</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Compile</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之间通信的桥梁，主要做的事情</font>

+ <font style="color:rgb(44, 62, 80);">在自身实例化时往属性订阅器 (</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dep</font><font style="color:rgb(44, 62, 80);">) 里面添加自己</font>
+ <font style="color:rgb(44, 62, 80);">待属性变动</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dep.notice()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">通知时，调用自身的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">update()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法，并触发</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Compile</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中绑定的回调</font>

**<font style="color:rgb(44, 62, 80);">Object.defineProperty()</font>**<font style="color:rgb(44, 62, 80);">，那么它的用法是什么，以及优缺点是什么呢？</font>

+ <font style="color:rgb(44, 62, 80);">可以检测对象中数据发生的修改</font>
+ <font style="color:rgb(44, 62, 80);">对于复杂的对象，层级很深的话，是不友好的，需要经行深度监听，这样子就需要递归到底，这也是它的缺点。</font>
+ <font style="color:rgb(44, 62, 80);">对于一个对象中，如果你新增加属性，删除属性，**Object.defineProperty()**是不能观测到的，那么应该如何解决呢？可以通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.set()</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.delete()</font><font style="color:rgb(44, 62, 80);">来实现。</font>

```javascript
// 模拟 Vue 中的 data 选项 
let data = {
  msg: 'hello'
}
// 模拟 Vue 的实例 
let vm = {}
// 数据劫持:当访问或者设置 vm 中的成员的时候，做一些干预操作
Object.defineProperty(vm, 'msg', {
  // 可枚举(可遍历)
  enumerable: true,
  // 可配置(可以使用 delete 删除，可以通过 defineProperty 重新定义) 
  configurable: true,
  // 当获取值的时候执行 
  get () {
    console.log('get: ', data.msg)
    return data.msg 
  },
  // 当设置值的时候执行 
  set (newValue) {
    console.log('set: ', newValue) 
    if (newValue === data.msg) {
      return
    }
    data.msg = newValue
    // 数据更改，更新 DOM 的值 
    document.querySelector('app').textContent = data.msg
  } 
})

// 测试
vm.msg = 'Hello World' 
console.log(vm.msg)
```

**<font style="color:rgb(44, 62, 80);">Vue3.x响应式数据原理</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue3.x</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">改用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">替代</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。因为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以直接监听</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">对象和数组</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的变化，并且有多达13种拦截方法。并且作为新标准将受到浏览器厂商重点持续的性能优化。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(44, 62, 80);">只会代理对象的第一层，那么</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue3</font><font style="color:rgb(44, 62, 80);">又是怎样处理这个问题的呢？</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">判断当前</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Reflect.get的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">返回值是否为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，如果是则再通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactive</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法做代理， 这样就实现了深度观测。</font>

**<font style="color:rgb(44, 62, 80);">监测数组的时候可能触发多次get/set，那么如何防止触发多次呢？</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们可以判断</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是否为当前被代理对象</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">target</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">自身属性，也可以判断旧值与新值是否相等，只有满足以上两个条件之一时，才有可能执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">trigger</font>

```javascript
// 模拟 Vue 中的 data 选项 
let data = {
  msg: 'hello',
  count: 0 
}
// 模拟 Vue 实例
let vm = new Proxy(data, {
  // 当访问 vm 的成员会执行
  get (target, key) {
    console.log('get, key: ', key, target[key])
    return target[key]
  },
  // 当设置 vm 的成员会执行
  set (target, key, newValue) {
    console.log('set, key: ', key, newValue)
    if (target[key] === newValue) {
      return
    }
    target[key] = newValue
    document.querySelector('app').textContent = target[key]
  }
})

// 测试
vm.msg = 'Hello World'
console.log(vm.msg)
```

**<font style="color:rgb(44, 62, 80);">Proxy 相比于 defineProperty 的优势</font>**

+ <font style="color:rgb(44, 62, 80);">数组变化也能监听到</font>
+ <font style="color:rgb(44, 62, 80);">不需要深度遍历监听</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中新增的功能，可以用来自定义对象中的操作</font>

```javascript
let p = new Proxy(target, handler);
// `target` 代表需要添加代理的对象
// `handler` 用来自定义对象中的操作
// 可以很方便的使用 Proxy 来实现一个数据绑定和监听

let onWatch = (obj, setBind, getLogger) => {
  let handler = {
    get(target, property, receiver) {
      getLogger(target, property)
      return Reflect.get(target, property, receiver);
    },
    set(target, property, value, receiver) {
      setBind(value);
      return Reflect.set(target, property, value);
    }
  };
  return new Proxy(obj, handler);
};

let obj = { a: 1 }
let value
let p = onWatch(obj, (v) => {
  value = v
}, (target, property) => {
  console.log(`Get '${property}' = ${target[property]}`);
})
p.a = 2 // bind `value` to `2`
p.a // -> Get 'a' = 2
```

**<font style="color:rgb(44, 62, 80);">总结</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781805782-25d2c753-5f0d-456a-b93c-5670675e3a34.png)

+ <font style="color:rgb(44, 62, 80);">Vue</font>
    - <font style="color:rgb(44, 62, 80);">记录传入的选项，设置</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$data/$el</font>
    - <font style="color:rgb(44, 62, 80);">把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的成员注入到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例</font>
    - <font style="color:rgb(44, 62, 80);">负责调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Observer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实现数据响应式处理(数据劫持)</font>
    - <font style="color:rgb(44, 62, 80);">负责调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Compiler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">编译指令/插值表达式等</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Observer</font>
    - <font style="color:rgb(44, 62, 80);">数据劫持</font>
        * <font style="color:rgb(44, 62, 80);">负责把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的成员转换成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getter/setter</font>
        * <font style="color:rgb(44, 62, 80);">负责把多层属性转换成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getter/setter</font>
        * <font style="color:rgb(44, 62, 80);">如果给属性赋值为新对象，把新对象的成员设置为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getter/setter</font>
    - <font style="color:rgb(44, 62, 80);">添加</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dep</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的依赖关系</font>
    - <font style="color:rgb(44, 62, 80);">数据变化发送通知</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Compiler</font>
    - <font style="color:rgb(44, 62, 80);">负责编译模板，解析指令/插值表达式</font>
    - <font style="color:rgb(44, 62, 80);">负责页面的首次渲染过程</font>
    - <font style="color:rgb(44, 62, 80);">当数据变化后重新渲染</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dep</font>
    - <font style="color:rgb(44, 62, 80);">收集依赖，添加订阅者(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watcher</font><font style="color:rgb(44, 62, 80);">)</font>
    - <font style="color:rgb(44, 62, 80);">通知所有订阅者</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font>
    - <font style="color:rgb(44, 62, 80);">自身实例化的时候往</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dep</font><font style="color:rgb(44, 62, 80);">对象中添加自己</font>
    - <font style="color:rgb(44, 62, 80);">当数据变化</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dep</font><font style="color:rgb(44, 62, 80);">通知所有的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例更新视图</font>

### [](https://www.123fe.net/docs/base/improve.html#_2-%E5%8F%91%E5%B8%83%E8%AE%A2%E9%98%85%E6%A8%A1%E5%BC%8F%E5%92%8C%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F)<font style="color:rgb(44, 62, 80);">2 发布订阅模式和观察者模式</font>
**<font style="color:rgb(44, 62, 80);">1. 发布/订阅模式</font>**

+ **<font style="color:rgb(44, 62, 80);">发布/订阅模式</font>**
    - <font style="color:rgb(44, 62, 80);">订阅者</font>
    - <font style="color:rgb(44, 62, 80);">发布者</font>
    - <font style="color:rgb(44, 62, 80);">信号中心</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们假定，存在一个"信号中心"，某个任务执行完成，就向信号中心"发布"(publish)一个信 号，其他任务可以向信号中心"订阅"(subscribe)这个信号，从而知道什么时候自己可以开始执 行。这就叫做"发布/订阅模式"(publish-subscribe pattern)</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">Vue 的自定义事件</font>

```javascript
let vm = new Vue()
vm.$on('dataChange', () => { console.log('dataChange')})
vm.$on('dataChange', () => { 
  console.log('dataChange1')
}) 
vm.$emit('dataChange')
```

<font style="color:rgb(44, 62, 80);">兄弟组件通信过程</font>

```javascript
// eventBus.js
// 事件中心
let eventHub = new Vue()

// ComponentA.vue
// 发布者
addTodo: function () {
  // 发布消息(事件)
  eventHub.$emit('add-todo', { text: this.newTodoText }) 
  this.newTodoText = ''
}
// ComponentB.vue
// 订阅者
created: function () {
  // 订阅消息(事件)
  eventHub.$on('add-todo', this.addTodo)
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">模拟 Vue 自定义事件的实现</font>

```javascript
class EventEmitter {
  constructor(){
    // { eventType: [ handler1, handler2 ] }
    this.subs = {}
  }
  // 订阅通知
  $on(eventType, fn) {
    this.subs[eventType] = this.subs[eventType] || []
    this.subs[eventType].push(fn)
  }
  // 发布通知
  $emit(eventType) {
    if(this.subs[eventType]) {
      this.subs[eventType].forEach(v=>v())
    }
  }
}

// 测试
var bus = new EventEmitter()

// 注册事件
bus.$on('click', function () {
  console.log('click')
})

bus.$on('click', function () {
  console.log('click1')
})

// 触发事件 
bus.$emit('click')
```

**<font style="color:rgb(44, 62, 80);">2. 观察者模式</font>**

+ <font style="color:rgb(44, 62, 80);">观察者(订阅者) --</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Watcher</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">update()</font><font style="color:rgb(44, 62, 80);">:当事件发生时，具体要做的事情</font>
+ <font style="color:rgb(44, 62, 80);">目标(发布者) --</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dep</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">subs</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数组:存储所有的观察者</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">addSub()</font><font style="color:rgb(44, 62, 80);">:添加观察者</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">notify()</font><font style="color:rgb(44, 62, 80);">:当事件发生，调用所有观察者的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">update()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法</font>
+ <font style="color:rgb(44, 62, 80);">没有事件中心</font>

```javascript
// 目标(发布者) 
// Dependency
class Dep {
  constructor () {
    // 存储所有的观察者
    this.subs = []
  }
  // 添加观察者
  addSub (sub) {
    if (sub && sub.update) {
      this.subs.push(sub)
    }
  }
  // 通知所有观察者
  notify () {
    this.subs.forEach(sub => sub.update())
  }
}

// 观察者(订阅者)
class Watcher {
  update () {
    console.log('update')
  }
}

// 测试
let dep = new Dep()
let watcher = new Watcher()
dep.addSub(watcher) 
dep.notify()
```

**<font style="color:rgb(44, 62, 80);">3. 总结</font>**

+ **<font style="color:rgb(44, 62, 80);">观察者模式</font>**<font style="color:rgb(44, 62, 80);">是由具体目标调度，比如当事件触发，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dep</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就会去调用观察者的方法，所以观察者模 式的订阅者与发布者之间是存在依赖的</font>
+ **<font style="color:rgb(44, 62, 80);">发布/订阅模式</font>**<font style="color:rgb(44, 62, 80);">由统一调度中心调用，因此发布者和订阅者不需要知道对方的存在</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781805433-3c9e198f-3cf3-464f-b60b-84115878a44e.png)

### [](https://www.123fe.net/docs/base/improve.html#_3-%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BD%BF%E7%94%A8-virtual-dom)<font style="color:rgb(44, 62, 80);">3 为什么使用 Virtual DOM</font>
+ <font style="color:rgb(44, 62, 80);">手动操作</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">比较麻烦，还需要考虑浏览器兼容性问题，虽然有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jQuery</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等库简化</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">操作，但是随着项目的复杂 DOM 操作复杂提升</font>
+ <font style="color:rgb(44, 62, 80);">为了简化</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的复杂操作于是出现了各种</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MVVM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">框架，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MVVM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">框架解决了视图和状态的同步问题</font>
+ <font style="color:rgb(44, 62, 80);">为了简化视图的操作我们可以使用模板引擎，但是模板引擎没有解决跟踪状态变化的问题，于是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">出现了</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的好处是当状态改变时不需要立即更新 DOM，只需要创建一个虚拟树来描述</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内部将弄清楚如何有效(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">)的更新</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font>
+ <font style="color:rgb(44, 62, 80);">虚拟</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以维护程序的状态，跟踪上一次的状态</font>
+ <font style="color:rgb(44, 62, 80);">通过比较前后两次状态的差异更新真实</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font>

**<font style="color:rgb(44, 62, 80);">虚拟 DOM 的作用</font>**

+ <font style="color:rgb(44, 62, 80);">维护视图和状态的关系</font>
+ <font style="color:rgb(44, 62, 80);">复杂视图情况下提升渲染性能</font>
+ <font style="color:rgb(44, 62, 80);">除了渲染</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以外，还可以实现</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSR(Nuxt.js/Next.js)</font><font style="color:rgb(44, 62, 80);">、原生应用(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Weex/React Native</font><font style="color:rgb(44, 62, 80);">)、小程序(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mpvue/uni-app</font><font style="color:rgb(44, 62, 80);">)等</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781808336-d1fc9fe7-c0fb-483f-957b-e3ec23b9ef31.png)

### [](https://www.123fe.net/docs/base/improve.html#_4-vdom-%E4%B8%89%E4%B8%AA-part)<font style="color:rgb(44, 62, 80);">4 VDOM：三个 part</font>
+ <font style="color:rgb(44, 62, 80);">虚拟节点类，将真实</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">节点用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象的形式进行展示，并提供</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法，将虚拟节点渲染成真实</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font>
+ <font style="color:rgb(44, 62, 80);">节点</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">比较：对虚拟节点进行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">层面的计算，并将不同的操作都记录到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">re-render</font><font style="color:rgb(44, 62, 80);">：解析</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象，进行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">re-render</font>

**<font style="color:rgb(44, 62, 80);">补充1��VDOM 的必要性？</font>**

+ **<font style="color:rgb(44, 62, 80);">创建真实DOM的代价高</font>**<font style="color:rgb(44, 62, 80);">：真实的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">node</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实现的属性很多，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">仅仅实现一些必要的属性，相比起来，创建一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的成本比较低。</font>
+ **<font style="color:rgb(44, 62, 80);">触发多次浏览器重绘及回流</font>**<font style="color:rgb(44, 62, 80);">：使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，相当于加了一个缓冲，让一次数据变动所带来的所有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">node</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">变化，先在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vnode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中进行修改，然后</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之后对所有产生差异的节点集中一次对</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM tree</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">进行修改，以减少浏览器的重绘及回流。</font>

**<font style="color:rgb(44, 62, 80);">补充2：vue 为什么采用 vdom？</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">引入</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在性能方面的考量仅仅是一方面。</font>

+ <font style="color:rgb(44, 62, 80);">性能受场景的影响是非常大的，不同的场景可能造成不同实现方案之间成倍的性能差距，所以依赖细粒度绑定及</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">哪个的性能更好还真不是一个容易下定论的问题。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之所以引入了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(44, 62, 80);">，更重要的原因是为了解耦</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);">依赖，这带来两个非常重要的好处是：</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不再依赖</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">解析器进行模版解析，可以进行更多的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AOT</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">工作提高运行时效率：通过模版</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AOT</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">编译，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的运行时体积可以进一步压缩，运行时效率可以进一步提升；</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以渲染到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以外的平台，实现</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSR</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、同构渲染这些高级特性，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Weex</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">等框架应用的就是这一特性。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">综上，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在性能上的收益并不是最主要的，更重要的是它使得</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">具备了现代框架应有的高级特性。</font>

### [](https://www.123fe.net/docs/base/improve.html#_5-vue-%E5%92%8C-react%E6%8A%80%E6%9C%AF%E9%80%89%E5%9E%8B)<font style="color:rgb(44, 62, 80);">5 vue 和 react技术选型</font>
**<font style="color:rgb(44, 62, 80);">相同点：</font>**

1. <font style="color:rgb(44, 62, 80);">数据驱动页面，提供响应式的试图组件</font>
2. <font style="color:rgb(44, 62, 80);">都有virtual DOM,组件化的开发，通过props参数进行父子之间组件传递数据，都实现了webComponents规范</font>
3. <font style="color:rgb(44, 62, 80);">数据流动单向，都支持服务器的渲染SSR</font>
4. <font style="color:rgb(44, 62, 80);">都有支持native的方法，react有React native， vue有wexx</font>

**<font style="color:rgb(44, 62, 80);">不同点：</font>**

1. <font style="color:rgb(44, 62, 80);">数据绑定：Vue实现了双向的数据绑定，react数据流动是单向的</font>
2. <font style="color:rgb(44, 62, 80);">数据渲染：大规模的数据渲染，react更快</font>
3. <font style="color:rgb(44, 62, 80);">使用场景：React配合Redux架构适合大规模多人协作复杂项目，Vue适合小快的项目</font>
4. <font style="color:rgb(44, 62, 80);">开发风格：react推荐做法jsx + inline style把html和css都写在js了</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">vue是采用webpack +vue-loader单文件组件格式，html, js, css同一个文件</font>

### [](https://www.123fe.net/docs/base/improve.html#_6-nexttick)<font style="color:rgb(44, 62, 80);">6 nextTick</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nextTick</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以让我们在下次</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">更新循环结束之后执行延迟回调，用于获得更新后的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nextTick</font><font style="color:rgb(44, 62, 80);">主要使用了宏任务和微任务。根据执行环境分别尝试采用</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MutationObserver</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font>
+ <font style="color:rgb(44, 62, 80);">如果以上都不行则采用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">定义了一个异步方法，多次调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nextTick</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会将方法存入队列中，通过这个异步方法清空当前队列</font>

### [](https://www.123fe.net/docs/base/improve.html#_7-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)<font style="color:rgb(44, 62, 80);">7 生命周期</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781808805-64e4cbce-f95c-4aa5-a4e3-70d1e8ad2553.png)

_**<font style="color:rgb(44, 62, 80);">init</font>**_

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">initLifecycle/Event</font><font style="color:rgb(44, 62, 80);">，往vm上挂载各种属性</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callHook: beforeCreated</font><font style="color:rgb(44, 62, 80);">: 实例刚创建</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">initInjection/initState</font><font style="color:rgb(44, 62, 80);">: 初始化注入和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">响应性</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created: 创建完成，属性已经绑定， 但还未生成真实</font><font style="color:rgb(44, 62, 80);">dom`</font>
+ <font style="color:rgb(44, 62, 80);">进行元素的挂载：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$el / vm.$mount()</font>
+ <font style="color:rgb(44, 62, 80);">是否有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">template</font><font style="color:rgb(44, 62, 80);">: 解析成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render function</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">*.vue</font><font style="color:rgb(44, 62, 80);">文件:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue-loader</font><font style="color:rgb(44, 62, 80);">会将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><template></font><font style="color:rgb(44, 62, 80);">编译成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render function</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeMount</font><font style="color:rgb(44, 62, 80);">: 模板编译/挂载之前</font>
+ <font style="color:rgb(44, 62, 80);">执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render function</font><font style="color:rgb(44, 62, 80);">，生成真实的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">，并替换到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom tree</font><font style="color:rgb(44, 62, 80);">中</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font><font style="color:rgb(44, 62, 80);">: 组件已挂载</font>

**<font style="color:rgb(44, 62, 80);">update</font>**

+ <font style="color:rgb(44, 62, 80);">执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">算法，比对改变是否需要触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI</font><font style="color:rgb(44, 62, 80);">更新</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flushScheduleQueue</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watcher.before</font><font style="color:rgb(44, 62, 80);">: 触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeUpdate</font><font style="color:rgb(44, 62, 80);">钩子 -</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watcher.run()</font><font style="color:rgb(44, 62, 80);">: 执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watcher</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">notify</font><font style="color:rgb(44, 62, 80);">，通知所有依赖项更新UI</font>
+ <font style="color:rgb(44, 62, 80);">触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updated</font><font style="color:rgb(44, 62, 80);">钩子: 组件已更新</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">actived / deactivated(keep-alive)</font><font style="color:rgb(44, 62, 80);">: 不销毁，缓存，组件激活与失活</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">destroy</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeDestroy</font><font style="color:rgb(44, 62, 80);">: 销毁开始</font>
    - <font style="color:rgb(44, 62, 80);">销毁自身且递归销毁子组件以及事件监听</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">remove()</font><font style="color:rgb(44, 62, 80);">: 删除节点</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watcher.teardown()</font><font style="color:rgb(44, 62, 80);">: 清空依赖</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vm.$off()</font><font style="color:rgb(44, 62, 80);">: 解绑监听</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">destroyed</font><font style="color:rgb(44, 62, 80);">: 完成后触发钩子</font>

| <font style="color:rgb(44, 62, 80);">Vue2</font> | <font style="color:rgb(44, 62, 80);">Vue3</font> |
| :--- | :--- |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font> | <font style="color:rgb(44, 62, 80);">❌</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">(替代)</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font> | <font style="color:rgb(44, 62, 80);">❌</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">(替代)</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeMount</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onBeforeMount</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mounted</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onMounted</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeUpdate</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onBeforeUpdate</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">updated</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nUpdated</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeDestroy</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onBeforeUnmount</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">destroyed</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onUnmounted</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">errorCaptured</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onErrorCaptured</font> |
| <font style="color:rgb(44, 62, 80);">-</font> | <font style="color:rgb(44, 62, 80);">🎉</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onRenderTracked</font> |
| <font style="color:rgb(44, 62, 80);">-</font> | <font style="color:rgb(44, 62, 80);">🎉</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onRenderTriggered</font> |


<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上面是vue的声明周期的简单梳理，接下来我们直接以代码的形式来完成vue的初始化</font>

```javascript
new Vue({})

// 初始化Vue实例
function _init() {
  // 挂载属性
  initLifeCycle(vm) 
  // 初始化事件系统，钩子函数等
  initEvent(vm) 
  // 编译slot、vnode
  initRender(vm) 
  // 触发钩子
  callHook(vm, 'beforeCreate')
  // 添加inject功能
  initInjection(vm)
  // 完成数据响应性 props/data/watch/computed/methods
  initState(vm)
  // 添加 provide 功能
  initProvide(vm)
  // 触发钩子
  callHook(vm, 'created')

  // 挂载节点
  if (vm.$options.el) {
    vm.$mount(vm.$options.el)
  }
}

// 挂载节点实现
function mountComponent(vm) {
  // 获取 render function
  if (!this.options.render) {
    // template to render
    // Vue.compile = compileToFunctions
    let { render } = compileToFunctions() 
    this.options.render = render
  }
  // 触发钩子
  callHook('beforeMounte')
  // 初始化观察者
  // render 渲染 vdom， 
  vdom = vm.render()
  // update: 根据 diff 出的 patchs 挂载成真实的 dom 
  vm._update(vdom)
  // 触发钩子  
  callHook(vm, 'mounted')
}

// 更新节点实现
funtion queueWatcher(watcher) {
  nextTick(flushScheduleQueue)
}

// 清空队列
function flushScheduleQueue() {
  // 遍历队列中所有修改
  for(){
    // beforeUpdate
    watcher.before()

    // 依赖局部更新节点
    watcher.update() 
    callHook('updated')
  }
}

// 销毁实例实现
Vue.prototype.$destory = function() {
  // 触发钩子
  callHook(vm, 'beforeDestory')
  // 自身及子节点
  remove() 
  // 删除依赖
  watcher.teardown() 
  // 删除监听
  vm.$off() 
  // 触发钩子
  callHook(vm, 'destoryed')
}
```

### [](https://www.123fe.net/docs/base/improve.html#_8-vue-router)<font style="color:rgb(44, 62, 80);">8 vue-router</font>
**<font style="color:rgb(44, 62, 80);">mode</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hash</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">history</font>

**<font style="color:rgb(44, 62, 80);">跳转</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.$router.push()</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><router-link to=""></router-link></font>

**<font style="color:rgb(44, 62, 80);">占位</font>**

```vue
<router-view></router-view>
```

**<font style="color:rgb(44, 62, 80);">vue-router源码实现</font>**

+ <font style="color:rgb(44, 62, 80);">作为一个插件存在:实现</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VueRouter</font><font style="color:rgb(44, 62, 80);">类和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">install</font><font style="color:rgb(44, 62, 80);">方法</font>
+ <font style="color:rgb(44, 62, 80);">实现两个全局组件:</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">router-view</font><font style="color:rgb(44, 62, 80);">用于显示匹配组件内容，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">router-link</font><font style="color:rgb(44, 62, 80);">用于跳转</font>
+ <font style="color:rgb(44, 62, 80);">监控</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">url</font><font style="color:rgb(44, 62, 80);">变化:监听</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hashchange</font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">popstate</font><font style="color:rgb(44, 62, 80);">事件</font>
+ <font style="color:rgb(44, 62, 80);">响应最新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">url</font><font style="color:rgb(44, 62, 80);">:创建一个响应式的属性</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">current</font><font style="color:rgb(44, 62, 80);">，当它改变时获取对应组件并显示</font>

```javascript
// 我们的插件：
// 1.实现一个Router类并挂载期实例
// 2.实现两个全局组件router-link和router-view
let Vue;

class VueRouter {
  // 核心任务：
  // 1.监听url变化
  constructor(options) {
    this.$options = options;

    // 缓存path和route映射关系
    // 这样找组件更快
    this.routeMap = {}
    this.$options.routes.forEach(route => {
      this.routeMap[route.path] = route
    })

    // 数据响应式
    // 定义一个响应式的current，则如果他变了，那么使用它的组件会rerender
    Vue.util.defineReactive(this, 'current', '')

    // 请确保onHashChange中this指向当前实例
    window.addEventListener('hashchange', this.onHashChange.bind(this))
    window.addEventListener('load', this.onHashChange.bind(this))
  }

  onHashChange() {
    // console.log(window.location.hash);
    this.current = window.location.hash.slice(1) || '/'
  }
}

// 插件需要实现install方法
// 接收一个参数，Vue构造函数，主要用于数据响应式
VueRouter.install = function (_Vue) {
  // 保存Vue构造函数在VueRouter中使用
  Vue = _Vue

  // 任务1：使用混入来做router挂载这件事情
  Vue.mixin({
    beforeCreate() {
      // 只有根实例才有router选项
      if (this.$options.router) {
        Vue.prototype.$router = this.$options.router
      }

    }
  })

  // 任务2：实现两个全局组件
  // router-link: 生成一个a标签，在url后面添加
  // <a href="/about">aaaa</a>
  // <router-link to="/about">aaa</router-link>
  Vue.component('router-link', {
    props: {
      to: {
        type: String,
        required: true
      },
    },
    render(h) {
      // h(tag, props, children)
      return h('a',
               { attrs: { href: '' + this.to } },
               this.$slots.default
              )
      // 使用jsx
      // return <a href={''+this.to}>{this.$slots.default}</a>
    }
  })
  Vue.component('router-view', {
    render(h) {
      // 根据current获取组件并render
      // current怎么获取?
      // console.log('render',this.$router.current);
      // 获取要渲染的组件
      let component = null
      const { routeMap, current } = this.$router
      if (routeMap[current]) {
        component = routeMap[current].component
      }
      return h(component)
    }
  })
}

export default VueRouter
```

### [](https://www.123fe.net/docs/base/improve.html#_9-vuex)<font style="color:rgb(44, 62, 80);">9 vuex</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">Vuex 集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以可预测的方式发生变化</font>

**<font style="color:rgb(44, 62, 80);">核心概念</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">: 状态中心</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mutations</font><font style="color:rgb(44, 62, 80);">: 更改状态</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">actions</font><font style="color:rgb(44, 62, 80);">: 异步更改状态</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getters</font><font style="color:rgb(44, 62, 80);">: 获取状态</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">modules</font><font style="color:rgb(44, 62, 80);">: 将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">分成多个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">modules</font><font style="color:rgb(44, 62, 80);">，便于管理</font>
1. <font style="color:rgb(44, 62, 80);">状态</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">- state</font>**

<font style="color:rgb(44, 62, 80);">state保存应用状态</font>

```javascript
export default new Vuex.Store({ state: { counter:0 },})
```

1. <font style="color:rgb(44, 62, 80);">状态变更</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">- mutations</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mutations</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">用于修改状态，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">store.js</font>

```javascript
export default new Vuex.Store({
  mutations:
    {
      add(state) {
        state.counter++
      }
    }
})
```

1. <font style="color:rgb(44, 62, 80);">派生状态</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">- getters</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从state派生出新状态，类似计算属性</font>

```javascript
export default new Vuex.Store({
  getters:
    {
      doubleCounter(state) { // 计算剩余数量 return state.counter * 2;
      }
    }
})
```

1. <font style="color:rgb(44, 62, 80);">动作</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(44, 62, 80);">- actions</font>**

<font style="color:rgb(44, 62, 80);">加业务逻辑，类似于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">controller</font>

```javascript
export default new Vuex.Store({
  actions:
    {
      add({
        commit
      }) {
        setTimeout(() = >{}
                   }
                   })
```

<font style="color:rgb(44, 62, 80);">测试代码:</font>

```html
<p @click="$store.commit('add')">counter: {{$store.state.counter}}</p>
<p @click="$store.dispatch('add')">async counter: {{$store.state.counter}}</p>
<p>double:{{$store.getters.doubleCounter}}</p>
```

**<font style="color:rgb(44, 62, 80);">vuex原理解析</font>**

+ <font style="color:rgb(44, 62, 80);">实现一个插件:声明</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Store</font><font style="color:rgb(44, 62, 80);">类，挂载</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$store</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Store</font><font style="color:rgb(44, 62, 80);">具体实现:</font>
    - <font style="color:rgb(44, 62, 80);">创建响应式的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">，保存</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mutations</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">actions</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getters</font>
    - <font style="color:rgb(44, 62, 80);">实现</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">commit</font><font style="color:rgb(44, 62, 80);">根据用户传入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">type</font><font style="color:rgb(44, 62, 80);">执行对应</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mutation</font>
    - <font style="color:rgb(44, 62, 80);">实现</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatch</font><font style="color:rgb(44, 62, 80);">根据用户传入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">type</font><font style="color:rgb(44, 62, 80);">执行对应</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">action</font><font style="color:rgb(44, 62, 80);">，同时传递上下文</font>
    - <font style="color:rgb(44, 62, 80);">实现</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getters</font><font style="color:rgb(44, 62, 80);">，按照</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getters</font><font style="color:rgb(44, 62, 80);">定义对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">state</font><font style="color:rgb(44, 62, 80);">做派生</font>

```javascript
// 目标1：实现Store类，管理state（响应式的），commit方法和dispatch方法
// 目标2：封装一个插件，使用更容易使用
let Vue;

class Store {
  constructor(options) {
    // 定义响应式的state
    // this.$store.state.xx
    // 借鸡生蛋
    this._vm = new Vue({
      data: {
        $$state: options.state
      }
    })

    this._mutations = options.mutations
    this._actions = options.actions

    // 绑定this指向
    this.commit = this.commit.bind(this)
    this.dispatch = this.dispatch.bind(this)
  }

  // 只读
  get state() {
    return this._vm._data.$$state
  }

  set state(val) {
    console.error('不能直接赋值呀，请换别的方式！！天王盖地虎！！');

  }

  // 实现commit方法，可以修改state
  commit(type, payload) {
    // 拿出mutations中的处理函数执行它
    const entry = this._mutations[type]
    if (!entry) {
      console.error('未知mutaion类型');
      return
    }

    entry(this.state, payload)
  }

  dispatch(type, payload) {
    const entry = this._actions[type]

    if (!entry) {
      console.error('未知action类型');
      return
    }

    // 上下文可以传递当前store实例进去即可
    entry(this, payload)
  }
}

function install(_Vue){
  Vue = _Vue

  // 混入store实例
  Vue.mixin({
    beforeCreate() {
      if (this.$options.store) {
        Vue.prototype.$store = this.$options.store
      }
    }
  })
}

// { Store, install }相当于Vuex
// 它必须实现install方法
export default { Store, install }
```

### [](https://www.123fe.net/docs/base/improve.html#_10-vue3%E5%B8%A6%E6%9D%A5%E7%9A%84%E6%96%B0%E7%89%B9%E6%80%A7-%E4%BA%AE%E7%82%B9)<font style="color:rgb(44, 62, 80);">10 vue3带来的新特性/亮点</font>
**<font style="color:rgb(44, 62, 80);">1. 压缩包体积更小</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当前最小化并被压缩的 Vue 运行时大小约为 20kB（2.6.10 版为 22.8kB）。Vue 3.0捆绑包的大小大约会减少一半，即只有10kB！</font>

**<font style="color:rgb(44, 62, 80);">2. Object.defineProperty -> Proxy</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font><font style="color:rgb(44, 62, 80);">是一个相对比较昂贵的操作，因为它直接操作对象的属性，颗粒度比较小。将它替换为es6的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(44, 62, 80);">，在目标对象之上架了一层拦截，代理的是对象而不是对象的属性。这样可以将原本对对象属性的操作变为对整个对象的操作，颗粒度变大。</font>
+ <font style="color:rgb(44, 62, 80);">javascript引擎在解析的时候希望对象的结构越稳定越好，如果对象一直在变，可优化性降低，proxy不需要对原始对象做太多操作。</font>

**<font style="color:rgb(44, 62, 80);">3. Virtual DOM 重构</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">vdom的本质是一个抽象层，用javascript描述界面渲染成什么样子。react用jsx，没办法检测出可以优化的动态代码，所以做时间分片，vue中足够快的话可以不用时间分片</font>

+ **<font style="color:rgb(44, 62, 80);">传统vdom的性能瓶颈：</font>**
    - <font style="color:rgb(44, 62, 80);">虽然 Vue 能够保证触发更新的组件最小化，但在单个组件内部依然需要遍历该组件的整个 vdom 树。</font>
    - <font style="color:rgb(44, 62, 80);">传统 vdom 的性能跟模版大小正相关，跟动态节点的数量无关。在一些组件整个模版内只有少量动态节点的情况下，这些遍历都是性能的浪费。</font>
    - <font style="color:rgb(44, 62, 80);">JSX 和手写的 render function 是完全动态的，过度的灵活性导致运行时可以用于优化的信息不足</font>
+ **<font style="color:rgb(44, 62, 80);">那为什么不直接抛弃vdom呢？</font>**
    - <font style="color:rgb(44, 62, 80);">高级场景下手写</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">render function</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">获得更强的表达力</font>
    - <font style="color:rgb(44, 62, 80);">生成的代码更简洁</font>
    - <font style="color:rgb(44, 62, 80);">兼容2.x</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">vue的特点是底层为Virtual DOM，上层包含有大量静态信息的模版。为了兼容手写 render function，最大化利用模版静态信息，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue3.0采用了动静结合的解决方案</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，将vdom的操作颗粒度变小，每次触发更新不再以组件为单位进行遍历，主要更改如下</font>

+ <font style="color:rgb(44, 62, 80);">将模版基于动态节点指令切割为嵌套的区块</font>
+ <font style="color:rgb(44, 62, 80);">每个区块内部的节点结构是固定的</font>
+ <font style="color:rgb(44, 62, 80);">每个区块只需要以一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">追踪自身包含的动态节点</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">vue3.0将 vdom 更新性能由与模版整体大小相关提升为与动态内容的数量相关</font>

**<font style="color:rgb(44, 62, 80);">Vue 3.0 动静结合的 Dom diff</font>**

+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">Vue3.0 提出动静结合的 DOM diff 思想，动静结合的 DOM diff其实是在预编译阶段进行了优化。之所以能够做到预编译优化，是因为 Vue core 可以静态分析 template，在解析模版时，整个 parse 的过程是利用正则表达式顺序解析模板，当解析到开始标签、闭合标签和文本的时候都会分别执行对应的回调函数，来达到构造 AST 树的目的。</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">借助预编译过程，Vue 可以做到的预编译优化就很强大了。比如在预编译时标记出模版中可能变化的组件节点，再次进行渲染前 diff 时就可以跳过“永远不会变化的节点”，而只需要对比“可能会变化的动态节点”。这也就是动静结合的 DOM diff 将 diff 成本与模版大小正相关优化到与动态节点正相关的理论依据。</font>

**<font style="color:rgb(44, 62, 80);">4. Performance</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">vue3在性能方面比vue2快了2倍。</font>

+ <font style="color:rgb(44, 62, 80);">重写了虚拟DOM的实现</font>
+ <font style="color:rgb(44, 62, 80);">运行时编译</font>
+ <font style="color:rgb(44, 62, 80);">update性能提高</font>
+ <font style="color:rgb(44, 62, 80);">SSR速度提高</font>

**<font style="color:rgb(44, 62, 80);">5. Tree-shaking support</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">vue3中的核心api都支持了tree-shaking，这些api都是通过包引入的方式而不是直接在实例化时就注入，只会对使用到的功能或特性进行打包（按需打包），这意味着更多的功能和更小的体积。</font>

**<font style="color:rgb(44, 62, 80);">6. Composition API</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">vue2中，我们一般会采用mixin来复用逻辑代码，用倒是挺好用的，不过也存在一些问题：例如代码来源不清晰、方法属性等冲突。基于此在vue3中引入了Composition API（组合API），使用纯函数分隔复用代码。和React中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hooks</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的概念很相似</font>

+ <font style="color:rgb(44, 62, 80);">更好的逻辑复用和代码组织</font>
+ <font style="color:rgb(44, 62, 80);">更好的类型推导</font>

```html
<template>
    <div>X: {{ x }}</div>
    <div>Y: {{ y }}</div>
</template>

<script>
import { defineComponent, onMounted, onUnmounted, ref } from "vue";

const useMouseMove = () => {
    const x = ref(0);
    const y = ref(0);

    function move(e) {
        x.value = e.clientX;
        y.value = e.clientY;
    }

    onMounted(() => {
        window.addEventListener("mousemove", move);
    });

    onUnmounted(() => {
        window.removeEventListener("mousemove", move);
    });

    return { x, y };
};

export default defineComponent({
    setup() {
        const { x, y } = useMouseMove();

        return { x, y };
    }
});
</script>
```

**<font style="color:rgb(44, 62, 80);">7. 新增的三个组件Fragment、Teleport、Suspense</font>**

**<font style="color:rgb(44, 62, 80);">Fragment</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在书写vue2时，由于组件必须只有一个根节点，很多时候会添加一些没有意义的节点用于包裹。Fragment组件就是用于解决这个问题的（这和React中的Fragment组件是一样的）。</font>

<font style="color:rgb(44, 62, 80);">这意味着现在可以这样写组件了。</font>

```html
/* App.vue */
<template>
  <header>...</header>
  <main v-bind="$attrs">...</main>
  <footer>...</footer>
</template>

<script>
export default {};
</script>
```

<font style="color:rgb(44, 62, 80);">或者这样</font>

```plain
// app.js
import { defineComponent, h, Fragment } from 'vue';

export default defineComponent({
    render() {
        return h(Fragment, {}, [
            h('header', {}, ['...']),
            h('main', {}, ['...']),
            h('footer', {}, ['...']),
        ]);
    }
});
```

**<font style="color:rgb(44, 62, 80);">Teleport</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">Teleport其实就是React中的Portal。Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。</font>

<font style="color:rgb(44, 62, 80);">一个 portal 的典型用例是当父组件有 overflow: hidden 或 z-index 样式时，但你需要子组件能够在视觉上“跳出”其容器。例如，对话框、悬浮卡以及提示框。</font>

```html
/* App.vue */
<template>
    <div>123</div>
    <Teleport to="container">
        Teleport
    </Teleport>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
    setup() {}
});
</script>

/* index.html */
<div id="app"></div>
<div id="container"></div>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781809579-4344f034-10e1-4e4a-9847-df98acb1db1e.png)

**<font style="color:rgb(44, 62, 80);">Suspense</font>**

<font style="color:rgb(44, 62, 80);">同样的，这和React中的Supense是一样的。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Suspense</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">让你的组件在渲染之前进行“等待”，并在等待时显示 fallback 的内容</font>

```html
// App.vue
<template>
    <Suspense>
        <template default>
            <AsyncComponent />
        </template>
        <template fallback>
            Loading...
        </template>
    </Suspense>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import AsyncComponent from './AsyncComponent.vue';

export default defineComponent({
    name: "App",
    
    components: {
        AsyncComponent
    }
});
</script>

// AsyncComponent.vue
<template>
    <div>Async Component</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

const sleep = () => {
    return new Promise(resolve => setTimeout(resolve, 1000));
};

export default defineComponent({
    async setup() {
        await sleep();
    }
});
</script>
```

**<font style="color:rgb(44, 62, 80);">8. Better TypeScript support</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在vue2中使用过TypesScript的童鞋应该有过体会，写起来实在是有点难受。vue3则是使用ts进行了重写，开发者使用vue3时拥有更好的类型支持和更好的编写体验。</font>

### [](https://www.123fe.net/docs/base/improve.html#_11-compositon-api)<font style="color:rgb(44, 62, 80);">11 Compositon api</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Composition API</font><font style="color:rgb(44, 62, 80);">也叫组合式API，是Vue3.x的新特性。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">通过创建 Vue 组件，我们可以将接口的可重复部分及其功能提取到可重用的代码段中。仅此一项就可以使我们的应用程序在可维护性和灵活性方面走得更远。然而，我们的经验已经证明，光靠这一点可能是不够的，尤其是当你的应用程序变得非常大的时候——想想几百个组件。在处理如此大的应用程序时，共享和重用代码变得尤为重要</font>

+ <font style="color:rgb(44, 62, 80);">Vue2.0中，随着功能的增加，组件变得越来越复杂，越来越难维护，而难以维护的根本原因是Vue的API设计迫使开发者使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watch，computed，methods</font><font style="color:rgb(44, 62, 80);">选项组织代码，而不是实际的业务逻辑。</font>
+ <font style="color:rgb(44, 62, 80);">另外Vue2.0缺少一种较为简洁的低成本的机制来完成逻辑复用，虽然可以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">minxis</font><font style="color:rgb(44, 62, 80);">完成逻辑复用，但是当</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mixin</font><font style="color:rgb(44, 62, 80);">变多的时候，会使得难以找到对应的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data、computed</font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">method</font><font style="color:rgb(44, 62, 80);">来源于哪个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mixin</font><font style="color:rgb(44, 62, 80);">，使得类型推断难以进行。</font>
+ <font style="color:rgb(44, 62, 80);">所以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Composition API</font><font style="color:rgb(44, 62, 80);">的出现，主要是也是为了解决Option API带来的问题，第一个是代码组织问题，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Compostion API</font><font style="color:rgb(44, 62, 80);">可以让开发者根据业务逻辑组织自己的代码，让代码具备更好的可读性和可扩展性，也就是说当下一个开发者接触这一段不是他自己写的代码时，他可以更好的利用代码的组织反推出实际的业务逻辑，或者根据业务逻辑更好的理解代码。</font>
+ <font style="color:rgb(44, 62, 80);">第二个是实现代码的逻辑提取与复用，当然</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mixin</font><font style="color:rgb(44, 62, 80);">也可以实现逻辑提取与复用，但是像前面所说的，多个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mixin</font><font style="color:rgb(44, 62, 80);">作用在同一个组件时，很难看出</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">property</font><font style="color:rgb(44, 62, 80);">是来源于哪个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mixin</font><font style="color:rgb(44, 62, 80);">，来源不清楚，另外，多个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mixin</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">property</font><font style="color:rgb(44, 62, 80);">存在变量命名冲突的风险。而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Composition API</font><font style="color:rgb(44, 62, 80);">刚好解决了这两个问题。</font>

**<font style="color:rgb(44, 62, 80);">通俗的讲：</font>**

<font style="color:rgb(44, 62, 80);">没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Composition API</font><font style="color:rgb(44, 62, 80);">之前vue相关业务的代码需要配置到option的特定的区域，中小型项目是没有问题的，但是在大型项目中会导致后期的维护性比较复杂，同时代码可复用性不高。Vue3.x中的composition-api就是为了解决这个问题而生的</font>

**<font style="color:rgb(44, 62, 80);">compositon api提供了以下几个函数：</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactive</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watchEffect</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watch</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">computed</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toRefs</font>
+ <font style="color:rgb(44, 62, 80);">生命周期的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hooks</font>

**<font style="color:rgb(44, 62, 80);">都说Composition API与React Hook很像，说说区别</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从React Hook的实现角度看，React Hook是根据useState调用的顺序来确定下一次重渲染时的state是来源于哪个useState，所以出现了以下限制</font>

+ <font style="color:rgb(44, 62, 80);">不能在循环、条件、嵌套函数中调用Hook</font>
+ <font style="color:rgb(44, 62, 80);">必须确保总是在你的React函数的顶层调用Hook</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect、useMemo</font><font style="color:rgb(44, 62, 80);">等函数必须手动确定依赖关系</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">而Composition API是基于Vue的响应式系统实现的，与React Hook的相比</font>

+ <font style="color:rgb(44, 62, 80);">声明在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">函数内，一次组件实例化只调用一次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">，而React Hook每次重渲染都需要调用Hook，使得React的GC比Vue更有压力，性能也相对于Vue来说也较慢</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Compositon API</font><font style="color:rgb(44, 62, 80);">的调用不需要顾虑调用顺序，也可以在循环、条件、嵌套函数中使用</font>
+ <font style="color:rgb(44, 62, 80);">响应式系统自动实现了依赖收集，进而组件的部分的性能优化由Vue内部自己完成，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Hook</font><font style="color:rgb(44, 62, 80);">需要手动传入依赖，而且必须必须保证依赖的顺序，让</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useEffect</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);">等函数正确的捕获依赖变量，否则会由于依赖不正确使得组件性能下降。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">虽然</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Compositon API</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">看起来比</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Hook</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">好用，但是其设计思想也是借鉴</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Hook</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的。</font>

### [](https://www.123fe.net/docs/base/improve.html#_12-computed-%E7%9A%84%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)<font style="color:rgb(44, 62, 80);">12 computed 的实现原理</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">computed</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">本质是一个惰性求值的观察者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">computed watcher</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。其内部通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.dirty</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性标记计算属性是否需要重新求值。</font>

+ <font style="color:rgb(44, 62, 80);">当 computed 的依赖状态发生改变时,就会通知这个惰性的 watcher,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">computed watcher</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.dep.subs.length</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">判断有没有订阅者,</font>
+ <font style="color:rgb(44, 62, 80);">有的话,会重新计算,然后对比新旧值,如果变化了,会重新渲染。 (Vue 想确保不仅仅是计算属性依赖的值发生变化，而是当计算属性</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">最终计算的值</font><font style="color:rgb(44, 62, 80);">发生变化时才会</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">触发渲染 watcher</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">重新渲染，本质上是一种优化。)</font>
+ <font style="color:rgb(44, 62, 80);">没有的话,仅仅把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.dirty = true</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">(当计算属性依赖于其他数据时，属性并不会立即重新计算，只有之后其他地方需要读取属性的时候，它才会真正计算，即具备 lazy（懒计算）特性。)</font>

### [](https://www.123fe.net/docs/base/improve.html#_13-watch-%E7%9A%84%E7%90%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">13 watch 的理解</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watch</font><font style="color:rgb(44, 62, 80);">没有缓存性，更多的是观察的作用，可以监听某些数据执行回调。当我们需要</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">深度监听对象中</font><font style="color:rgb(44, 62, 80);">的属性时，可以打开deep：true选项，这样便会对对象中的每一项进行监听。这样会带来性能问题，优化的话可以使用字符串形式监听</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">注意：Watcher : 观察者对象 , 实例分为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">渲染 watcher</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">(render watcher),</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">计算属性 watcher</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">(computed watcher),</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">侦听器 watcher</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（user watcher）三种</font>

### [](https://www.123fe.net/docs/base/improve.html#_14-vue-%E6%B8%B2%E6%9F%93%E8%BF%87%E7%A8%8B)<font style="color:rgb(44, 62, 80);">14 vue 渲染过程</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781809534-043d754c-394e-487d-a0de-1b19e40a599c.png)

+ <font style="color:rgb(44, 62, 80);">调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">compile</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数,生成 render 函数字符串 ,编译过程如下:</font>
    - <font style="color:rgb(44, 62, 80);">parse 使用大量的正则表达式对template字符串进行解析，将标签、指令、属性等转化为抽象语法树AST。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">模板 -> AST （最消耗性能）</font>
    - <font style="color:rgb(44, 62, 80);">optimize 遍历AST，找到其中的一些静态节点并进行标记，方便在页面重渲染的时候进行diff比较时，直接跳过这一些静态节点，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">优化runtime的性能</font>
    - <font style="color:rgb(44, 62, 80);">generate 将最终的AST转化为render函数字符串</font>
+ <font style="color:rgb(44, 62, 80);">调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new Watcher</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数,监听数据的变化,当数据发生变化时，Render 函数执行生成 vnode 对象</font>
+ <font style="color:rgb(44, 62, 80);">调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">patch</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法,对比新旧 vnode 对象,通过 DOM diff 算法,添加、修改、删除真正的 DOM 元素</font>

### [](https://www.123fe.net/docs/base/improve.html#_15-%E8%AF%B4%E4%B8%80%E8%AF%B4keep-alive%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)<font style="color:rgb(44, 62, 80);">15 说一说keep-alive实现原理</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">keep-alive</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">组件接受三个属性参数：</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">include</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">、</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">exclude</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">、</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">max</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">include</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指定需要缓存的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">组件name</font><font style="color:rgb(44, 62, 80);">集合，参数格式支持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">String, RegExp, Array。</font><font style="color:rgb(44, 62, 80);">当为字符串的时候，多个组件名称以逗号隔开。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">exclude</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指定不需要缓存的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">组件name</font><font style="color:rgb(44, 62, 80);">集合，参数格式和include一样。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">max</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指定最多可缓存组件的数量,超过数量删除第一个。参数格式支持String、Number。</font>

**<font style="color:rgb(44, 62, 80);">原理</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">keep-alive</font><font style="color:rgb(44, 62, 80);">实例会缓存对应组件的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font><font style="color:rgb(44, 62, 80);">,如果命中缓存，直接从缓存对象返回对应</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VNode</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LRU（Least recently used）</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">算法根据数据的历史访问记录来进行淘汰数据，其核心思想是“如果数据最近被访问过，那么将来被访问的几率也更高”。(墨菲定律：越担心的事情越会发生)</font>

### [](https://www.123fe.net/docs/base/improve.html#_16-%E4%B8%BA%E4%BB%80%E4%B9%88%E8%AE%BF%E9%97%AEdata%E5%B1%9E%E6%80%A7%E4%B8%8D%E9%9C%80%E8%A6%81%E5%B8%A6data)<font style="color:rgb(44, 62, 80);">16 为什么访问data属性不需要带data</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">vue中访问属性代理</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.data.xxx</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">转换</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this.xxx</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的实现</font>

```javascript
/** 将 某一个对象的属性 访问 映射到 对象的某一个属性成员上 */
function proxy( target, prop, key ) {
  Object.defineProperty( target, key, {
    enumerable: true,
    configurable: true,
    get () {
      return target[ prop ][ key ];
    },
    set ( newVal ) {
      target[ prop ][ key ] = newVal;
    }
  } );
}
```

### [](https://www.123fe.net/docs/base/improve.html#_17-template%E9%A2%84%E7%BC%96%E8%AF%91%E6%98%AF%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">17 template预编译是什么</font>
<font style="color:rgb(44, 62, 80);">对于 Vue 组件来说，模板编译只会在组件实例化的时候编译一次，生成渲染函数之后在也不会进行编译。因此，编译对组件的 runtime 是一种性能损耗。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">而模板编译的目的仅仅是将template转化为render function，这个过程，正好可以在项目构建的过程中完成，这样可以让实际组件在 runtime 时直接跳过模板渲染，进而提升性能，这个在项目构建的编译template的过程，就是预编译。</font>

### [](https://www.123fe.net/docs/base/improve.html#_18-%E4%BB%8B%E7%BB%8D%E4%B8%80%E4%B8%8Bvue%E4%B8%AD%E7%9A%84diff%E7%AE%97%E6%B3%95)<font style="color:rgb(44, 62, 80);">18 介绍一下Vue中的Diff算法</font>
<font style="color:rgb(44, 62, 80);">在新老虚拟DOM对比时</font>

+ <font style="color:rgb(44, 62, 80);">首先，对比节点本身，判断是否为同一节点，如果不为相同节点，则删除该节点重新创建节点进行替换</font>
+ <font style="color:rgb(44, 62, 80);">如果为相同节点，进行patchVnode，判断如何对该节点的子节点进行处理，先判断一方有子节点一方没有子节点的情况(如果新的children没有子节点，将旧的子节点移除)</font>
+ <font style="color:rgb(44, 62, 80);">比较如果都有子节点，则进行updateChildren，判断如何对这些新老节点的子节点进行操作（diff核心）。 匹配时，找到相同的子节点，递归比较子节点</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在diff中，只对同层的子节点进行比较，放弃跨级的节点比较，使得时间复杂从O(n^3)降低值O(n)，也就是说，只有当新旧children都为多个子节点时才需要用核心的Diff算法进行同层级比较。</font>

### [](https://www.123fe.net/docs/base/improve.html#_19-%E8%AF%B4%E8%AF%B4vue2-0%E5%92%8Cvue3-0%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">19 说说Vue2.0和Vue3.0有什么区别</font>
+ <font style="color:rgb(44, 62, 80);">重构响应式系统，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(44, 62, 80);">替换</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font><font style="color:rgb(44, 62, 80);">，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(44, 62, 80);">优势：</font>
    - <font style="color:rgb(44, 62, 80);">可直接监听数组类型的数据变化</font>
    - <font style="color:rgb(44, 62, 80);">监听的目标为对象本身，不需要像</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font><font style="color:rgb(44, 62, 80);">一样遍历每个属性，有一定的性能提升</font>
    - <font style="color:rgb(44, 62, 80);">可拦截</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply、ownKeys、has</font><font style="color:rgb(44, 62, 80);">等13种方法，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font><font style="color:rgb(44, 62, 80);">不行</font>
    - <font style="color:rgb(44, 62, 80);">直接实现对象属性的新增/删除</font>
+ <font style="color:rgb(44, 62, 80);">新增</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Composition API</font><font style="color:rgb(44, 62, 80);">，更好的逻辑复用和代码组织</font>
+ <font style="color:rgb(44, 62, 80);">重构</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Virtual DOM</font>
    - <font style="color:rgb(44, 62, 80);">模板编译时的优化，将一些静态节点编译成常量</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">slot</font><font style="color:rgb(44, 62, 80);">优化，将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">slot</font><font style="color:rgb(44, 62, 80);">编译为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">lazy</font><font style="color:rgb(44, 62, 80);">函数，将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">slot</font><font style="color:rgb(44, 62, 80);">的渲染的决定权交给子组件</font>
    - <font style="color:rgb(44, 62, 80);">模板中内联事件的提取并重用（原本每次渲染都重新生成内联函数）</font>
+ <font style="color:rgb(44, 62, 80);">代码结构调整，更便于Tree shaking，使得体积更小</font>
+ <font style="color:rgb(44, 62, 80);">使用Typescript替换Flow</font>

