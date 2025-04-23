## Vue 生命周期
<font style="color:rgb(0, 0, 0);">从我个人的开发经验来说，Vue2 和 Vue3 其实生命周期钩子，在选项式api模式下大差不差对比表格:</font>

| **<font style="color:rgb(0, 0, 0);">Vue2选项式</font>** | **<font style="color:rgb(0, 0, 0);">Vue3选项式</font>** | **<font style="color:rgb(0, 0, 0);">Vue3组合式</font>** | **<font style="color:rgb(0, 0, 0);">作用</font>** |
| :--- | :--- | :--- | :--- |
| <font style="color:rgb(0, 0, 0);">beforeCreate</font> | <font style="color:rgb(0, 0, 0);">beforeCreate</font> | <font style="color:rgb(0, 0, 0);">setup()</font> | <font style="color:rgb(0, 0, 0);">在实例初始化之后,数据观测和事件配置之前被调用</font> |
| <font style="color:rgb(0, 0, 0);">created</font> | <font style="color:rgb(0, 0, 0);">created</font> | <font style="color:rgb(0, 0, 0);">onBeforeMount</font> | <font style="color:rgb(0, 0, 0);">在实例创建完成后被立即调用,可在此执行数据初始化</font> |
| <font style="color:rgb(0, 0, 0);">beforeMount</font> | <font style="color:rgb(0, 0, 0);">beforeMount</font> | <font style="color:rgb(0, 0, 0);">onBeforeMount</font> | <font style="color:rgb(0, 0, 0);">在挂载开始之前被调用,相关render函数首次被调用</font> |
| <font style="color:rgb(0, 0, 0);">mounted</font> | <font style="color:rgb(0, 0, 0);">mounted</font> | <font style="color:rgb(0, 0, 0);">onMounted</font> | <font style="color:rgb(0, 0, 0);">组件挂载到DOM后调用,可获取DOM节点</font> |
| <font style="color:rgb(0, 0, 0);">beforeUpdate</font> | <font style="color:rgb(0, 0, 0);">beforeUpdate</font> | <font style="color:rgb(0, 0, 0);">onBeforeUpdate</font> | <font style="color:rgb(0, 0, 0);">数据更新时,虚拟DOM重新渲染和打补丁之前调用</font> |
| <font style="color:rgb(0, 0, 0);">updated</font> | <font style="color:rgb(0, 0, 0);">updated</font> | <font style="color:rgb(0, 0, 0);">onUpdated</font> | <font style="color:rgb(0, 0, 0);">组件DOM更新后调用,可执行依赖于DOM的操作</font> |
| <font style="color:rgb(0, 0, 0);">activated</font> | <font style="color:rgb(0, 0, 0);">activated</font> | <font style="color:rgb(0, 0, 0);">onActivated</font> | <font style="color:rgb(0, 0, 0);">keep-alive组件激活时调用</font> |
| <font style="color:rgb(0, 0, 0);">deactivated</font> | <font style="color:rgb(0, 0, 0);">deactivated</font> | <font style="color:rgb(0, 0, 0);">onDeactivated</font> | <font style="color:rgb(0, 0, 0);">keep-alive组件停用时调用</font> |
| <font style="color:rgb(0, 0, 0);">beforeDestroy</font> | <font style="color:rgb(0, 0, 0);">beforeUnmount</font> | <font style="color:rgb(0, 0, 0);">onBeforeUnmount</font> | <font style="color:rgb(0, 0, 0);">实例销毁之前调用,可执行清理操作</font> |
| <font style="color:rgb(0, 0, 0);">destroyed</font> | <font style="color:rgb(0, 0, 0);">unmounted</font> | <font style="color:rgb(0, 0, 0);">onUnmounted</font> | <font style="color:rgb(0, 0, 0);">实例销毁后调用,调用后实例指向销毁,所有东西可回收</font> |
| <font style="color:rgb(0, 0, 0);">errorCaptured</font> | <font style="color:rgb(0, 0, 0);">errorCaptured</font> | <font style="color:rgb(0, 0, 0);">onErrorCaptured</font> | <font style="color:rgb(0, 0, 0);">捕获子孙组件错误时被调用</font> |


### <font style="color:rgba(0, 0, 0, 0.85);">为什么 beforeCreate 对标的是 setup</font>
+ <font style="color:rgb(239, 112, 96);">beforeCreate</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">在实例被创建之后,</font><font style="color:rgb(239, 112, 96);">data</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">和</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(239, 112, 96);">methods</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">还未初始化之前调用</font>
+ <font style="color:rgb(239, 112, 96);">setup</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">在组件创建之后,</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(239, 112, 96);">data</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">和</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(239, 112, 96);">methods</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">初始化之前被调用</font>

<font style="color:rgb(0, 0, 0);">所以</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(239, 112, 96);">setup</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">对应于</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(239, 112, 96);">beforeCreate</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">钩子。</font>

### <font style="color:rgba(0, 0, 0, 0.85);">为什么 created 对标的是 onBeforeMount</font>
+ <font style="color:rgb(239, 112, 96);">created</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">在组件实例被创建之后调用,这个时候还没有开始</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(239, 112, 96);">DOM</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">的挂载,</font><font style="color:rgb(239, 112, 96);">data</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">数据对象就已经被初始化好了。</font>
+ <font style="color:rgb(239, 112, 96);">onBeforeMount</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">会在组件挂载到</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(239, 112, 96);">DOM</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">之前调用,这个时候数据已经初始化完成,但是还没有开始</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(239, 112, 96);">DOM</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">渲染。</font>

<font style="color:rgb(0, 0, 0);">所以其功能与</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(239, 112, 96);">created</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">类似,都是表示实例初始化完成,但还未开始</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(239, 112, 96);">DOM</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">渲染。</font>

## 组件间的通信方式
<font style="color:rgb(0, 0, 0);">这个算是很容易被问到的，但是又不怎么问的！</font>

| **<font style="color:rgb(0, 0, 0);">通信方式</font>** | **<font style="color:rgb(0, 0, 0);">说明</font>** | **<font style="color:rgb(0, 0, 0);">优点</font>** | **<font style="color:rgb(0, 0, 0);">缺点</font>** |
| :--- | :--- | :--- | :--- |
| <font style="color:rgb(0, 0, 0);">事件总线</font> | <font style="color:rgb(0, 0, 0);">利用空Vue实例作为消息总线</font> | <font style="color:rgb(0, 0, 0);">简单，低耦合</font> | <font style="color:rgb(0, 0, 0);">难维护，调试难度大</font> |
| <font style="color:rgb(0, 0, 0);">provide/inject</font> | <font style="color:rgb(0, 0, 0);">依赖注入，可跨多层级</font> | <font style="color:rgb(0, 0, 0);">低耦合，方便访问父级数据</font> | <font style="color:rgb(0, 0, 0);">无法响应式，只适用于父子孙组件间</font> |
| <font style="color:rgb(0, 0, 0);">本地存储</font> | <font style="color:rgb(0, 0, 0);">localStorage、sessionStorage</font> | <font style="color:rgb(0, 0, 0);">通用简单</font> | <font style="color:rgb(0, 0, 0);">没有响应式，需要手动同步</font> |
| <font style="color:rgb(0, 0, 0);">状态管理工具</font> | <font style="color:rgb(0, 0, 0);">Vuex、Pinia等</font> | <font style="color:rgb(0, 0, 0);">集中状态管理，高效调试</font> | <font style="color:rgb(0, 0, 0);">学习和构建成本较高</font> |
| <font style="color:rgb(0, 0, 0);">父子组件通信</font> | <font style="color:rgb(0, 0, 0);">props down， events up</font> | <font style="color:rgb(0, 0, 0);">天然的Vue组件通信方式</font> | <font style="color:rgb(0, 0, 0);">只能单向，父子组件间才有效</font> |


## 动态组件
<font style="color:rgb(0, 0, 0);">通过使用</font><font style="color:rgb(239, 112, 96);"><component></font><font style="color:rgb(0, 0, 0);">并动态绑定is属性,可以实现动态切换多个组件的功能。</font>

```javascript
// 组件对象
const Foo = { /* ... */ } 
const Bar = { /* ... */ }

  // 动态组件
  <component :is="currentComponent"/>

  data() {
  return {
    currentComponent: 'Foo'
  }
}
```

## 异步组件
<font style="color:rgb(0, 0, 0);">异步组件通过定义一个返回Promise的工厂函数,实现组件的异步加载。</font>

```javascript
const AsyncComponent = () => ({
  // 组件加载中         
  component: import('./MyComponent.vue'),
  // 加载失败时使用的组件
  error: ErrorComponent,
  // 展示加载时组件的延时时间
  delay: 200,
  // 加载组件时的提示
  loading: LoadingComponent,
})
```

<font style="color:rgb(0, 0, 0);">然后在组件中使用:</font>

```plain
<async-component></async-component>
```

<font style="color:rgb(0, 0, 0);">当异步组件加载成功后，将显示该组件，否则展示fallback组件。</font>

<font style="color:rgb(0, 0, 0);">异步组件常用于路由按需加载和代码分割。</font>

## keep-alive
<font style="color:rgb(0, 0, 0);">keep-alive是Vue提供的一个内置组件，可以使被包含的组件保留状态，避免反复重渲染，使用 keep-alive 进行缓存的组件会多两个生命周期钩子函数：activated、deactivated</font>

```javascript
<!-- 使用keep-alive包裹动态组件 -->
  <keep-alive>
  <component :is="currentComponent"></component> 
  </keep-alive>

  <!-- 动态切换组件 -->
  <button @click="currentComponent = 'A'">Show A</button>
  <button @click="currentComponent = 'B'">Show B</button>
```

### <font style="color:rgba(0, 0, 0, 0.85);">实现机制</font>
1. <font style="color:rgb(1, 1, 1);">keep-alive组件会在内部维护一个对象</font>
    - <font style="color:rgb(1, 1, 1);">cache：用来缓存已经创建的组件实例</font>
2. <font style="color:rgb(1, 1, 1);">在组件切换时，优先获取include内的组件，过滤exclude内的组件，然后再检查缓存中是否已经有实例</font>
    - <font style="color:rgb(1, 1, 1);">如果有则取出重用</font>
    - <font style="color:rgb(1, 1, 1);">如果没有缓存，则正常创建新实例，并存储到缓存中。</font>
3. <font style="color:rgb(1, 1, 1);">在组件销毁时，不会立即执行销毁，而是将其保存在缓存中（也要判断include和exclude）</font>
4. <font style="color:rgb(1, 1, 1);">keep-alive 会拦截组件的钩子函数；在适当时机调用 activated 和 deactivated 钩子</font>
5. <font style="color:rgb(1, 1, 1);">当缓存数量超过上限时，会释放最近最久未使用的缓存实例</font>

## slot
<font style="color:rgb(0, 0, 0);">slot 是我们在自定义组件，或者使用组件时候最喜欢用到的一个语法了</font>

### <font style="color:rgba(0, 0, 0, 0.85);">具名插槽</font>
<font style="color:rgb(0, 0, 0);">base-layout：</font>

```javascript
<div class="container">
  <header>
  <slot name="header"></slot>
  </header>
  <main>
  <slot></slot>
  </main>
  <footer>
  <slot name="footer"></slot>
  </footer>
  </div>
```

<font style="color:rgb(0, 0, 0);">一个不带</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(239, 112, 96);">name</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">的</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(239, 112, 96);"><slot></font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">出口会带有隐含的名字</font><font style="color:rgb(239, 112, 96);">“default”</font>

<font style="color:rgb(0, 0, 0);">使用：</font>

```javascript
<base-layout>
  <template v-slot:header>
  <h1>header</h1>
  </template>

  <p>paragraph</p>

  <template v-slot:footer>
  <p>footer</p>
  </template>
  </base-layout>
```

### <font style="color:rgba(0, 0, 0, 0.85);">作用域插槽</font>
<font style="color:rgb(0, 0, 0);">有时让插槽内容能够访问子组件中才有的数据是很有用的</font>

<font style="color:rgb(0, 0, 0);">current-user：</font>

```javascript
<span>
  <slot v-bind:user="user">
  {{ user.lastName }}
</slot>
  </span>
```

<font style="color:rgb(0, 0, 0);">使用：</font>

```javascript
<current-user>
  <template v-slot:default="{ user }">
  {{ user.firstName }}
</template>
  </current-user>
```

### <font style="color:rgba(0, 0, 0, 0.85);">动态插槽</font>
<font style="color:rgb(0, 0, 0);">我们可以动态配置 slotName 来进行插槽配置</font>

```javascript
<base-layout>
  <template v-slot:[slotName]>
  ...
  </template>
  </base-layout>
```

### <font style="color:rgba(0, 0, 0, 0.85);">插槽是如何渲染的呢？</font>
1. <font style="color:black;">编译阶段</font>
    - <font style="color:rgb(1, 1, 1);">子组件模板中</font><font style="color:rgb(239, 112, 96);"><slot></font><font style="color:rgb(1, 1, 1);">会生成一个Slot AST节点</font>
    - <font style="color:rgb(1, 1, 1);">父组件v-slot会生成Template AST节点</font>
    - <font style="color:rgb(1, 1, 1);">两者都会标注slot名称,建立关联</font>
2. <font style="color:black;">渲染阶段</font>
    - <font style="color:rgb(1, 1, 1);">Vue组件的_render方法会先执行子组件的render</font>
    - <font style="color:rgb(1, 1, 1);">render里遇到slot标记会生成comment节点占位</font>
    - <font style="color:rgb(1, 1, 1);">然后执行scoped slot的render,生成父组件传递的slot内容</font>
    - <font style="color:rgb(1, 1, 1);">最后在_update方法Patch时,找到对应评论节点插入内容</font>
3. <font style="color:black;">核心流程</font>
    - <font style="color:rgb(1, 1, 1);">parse -> slot AST + slot内容AST关联</font>
    - <font style="color:rgb(1, 1, 1);">render子组件 -> 插槽节点</font>
    - <font style="color:rgb(1, 1, 1);">render父组件内容 -> slot内容</font>
    - <font style="color:rgb(1, 1, 1);">patch时插入关联的内容</font>

## 异步更新队列
<font style="color:rgb(0, 0, 0);">这里引用官方的一句话：</font>

<font style="color:black;background-color:rgb(255, 249, 249);">可能你还没有注意到，Vue 在更新 DOM 时是异步执行的。只要侦听到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。</font>

### <font style="color:rgba(0, 0, 0, 0.85);">为什么需要异步更新DOM呢？</font>
<font style="color:rgb(0, 0, 0);">假设一个场景</font>

<font style="color:rgb(0, 0, 0);">html</font>

```javascript
<div>{{ title }}</div>
```

<font style="color:rgb(0, 0, 0);">js</font>

```javascript
test() {
  for(let i = 0; i < 100; i++){
    this.title = `第${i}个标题`
  }
}
...
mounted(){
  test()
}
```

<font style="color:rgb(0, 0, 0);">这里我们在</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(239, 112, 96);">test</font><font style="color:rgb(0, 0, 0);">中使用修改了</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(239, 112, 96);">title</font><font style="color:rgb(0, 0, 0);">，假设一下，如果没有异步更新这个</font><font style="color:rgb(239, 112, 96);">dom</font><font style="color:rgb(0, 0, 0);">，那么就要操作100次，为了避免这种无意义的性能消耗，Vue再侦听到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。</font>

<font style="color:rgb(0, 0, 0);">如果在同一事件循环中多次更新DOM,会导致不必要的计算和DOM操作。将它们 defer 到下一个事件循环执行,可以有效减少开销。</font>

<font style="color:rgb(0, 0, 0);">如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作是非常重要的。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作</font>

### <font style="color:rgba(0, 0, 0, 0.85);">nextTick</font>
<font style="color:rgb(0, 0, 0);">nextTick 相信大家都在项目中或多或少的用过几次吧！</font>

<font style="color:rgb(0, 0, 0);">nextTick: 在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。</font>

### <font style="color:rgba(0, 0, 0, 0.85);">nextTick 实现原理</font>
<font style="color:rgb(0, 0, 0);">可以简述如下:</font>

1. <font style="color:rgb(1, 1, 1);">nextTick接收一个回调函数作为参数</font>
2. <font style="color:rgb(1, 1, 1);">内部会维护一个异步回调队列数组</font>
3. <font style="color:rgb(1, 1, 1);">将传入的回调推入这个异步队列</font>
4. <font style="color:rgb(1, 1, 1);">在微任务(promise.then/MutationObserver)空闲时刻执行队列中的回调</font>
5. <font style="color:rgb(1, 1, 1);">达成在DOM更新后执行回调的效果</font>

#### <font style="color:rgba(0, 0, 0, 0.85);">写个例子</font>
```javascript
let callbacks = [] // 异步回调队列

function nextTick(cb) {
  callbacks.push(cb) // 推入回调队列

  // 微任务执行callbacks
  Promise.resolve().then(flushCallbacks) 
}

function flushCallbacks() {
  callbacks.forEach(cb => cb()) // 执行队列回调
  callbacks = [] // 重置队列
}
```

## Vue3 ref 和 reactive 区别，如何选择?
<font style="color:rgb(0, 0, 0);">个人认为</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(239, 112, 96);">ref</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">和</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(239, 112, 96);">reactive</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">其实 没必要这个都抛出来给我们用，容易造成一些使用困扰，</font><font style="color:rgb(239, 112, 96);">ref</font><font style="color:rgb(0, 0, 0);">我觉就好，不过存在即合理，还是细说一下区别</font>

### <font style="color:rgba(0, 0, 0, 0.85);">区别</font>
| **<font style="color:rgb(0, 0, 0);">ref()</font>** | **<font style="color:rgb(0, 0, 0);">reactive()</font>** |
| :--- | :--- |
| <font style="color:rgb(0, 0, 0);">✅</font><font style="color:rgb(0, 0, 0);">支持基本数据类型+引用数据类型</font> | <font style="color:rgb(0, 0, 0);">❌</font><font style="color:rgb(0, 0, 0);">只支持对象和数组(引用数据类型)</font> |
| <font style="color:rgb(0, 0, 0);">❌</font><font style="color:rgb(0, 0, 0);">在</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);"><script></font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">和</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);"><template></font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">使用方式不同(script中要</font><font style="color:rgb(0, 0, 0);">.value</font><font style="color:rgb(0, 0, 0);">)</font> | <font style="color:rgb(0, 0, 0);">✅</font><font style="color:rgb(0, 0, 0);">在</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);"><script></font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">和</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);"><template></font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">中无差别使用</font> |
| <font style="color:rgb(0, 0, 0);">✅</font><font style="color:rgb(0, 0, 0);">重新分配一个新对象</font>**<font style="color:rgb(0, 0, 0);">不会</font>**<font style="color:rgb(0, 0, 0);">失去响应</font> | <font style="color:rgb(0, 0, 0);">❌</font><font style="color:rgb(0, 0, 0);">重新分配一个新对象会丢失响应性</font> |
| <font style="color:rgb(0, 0, 0);">需要使用</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">.value</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">访问属性</font> | <font style="color:rgb(0, 0, 0);">能直接访问属性</font> |
| <font style="color:rgb(0, 0, 0);">✅</font><font style="color:rgb(0, 0, 0);">传入函数时,不会失去响应</font> | <font style="color:rgb(0, 0, 0);">❌</font><font style="color:rgb(0, 0, 0);">将对象传入函数时,失去响应</font> |
| <font style="color:rgb(0, 0, 0);">✅</font><font style="color:rgb(0, 0, 0);">解构对象时会丢失响应性,需使用toRefs</font> | <font style="color:rgb(0, 0, 0);">❌</font><font style="color:rgb(0, 0, 0);">解构时会丢失响应性,需使用toRefs</font> |


## Vue 模板编译的原理
<font style="color:rgb(0, 0, 0);">响应式是 Vue中很重要的一环，但是模板编译也是很重要的一环，从面试的角度来说，Vue的模板编译主要是这几个步骤：</font>

+ **<font style="color:black;">解析</font>**<font style="color:black;">：解析器将模板解析为抽象语树 AST，只有将模板解析成 AST 后，才能基于它做优化或者生成代码字符串</font>
    - <font style="color:rgb(1, 1, 1);">使用正则等方式解析模板字符串,生成 AST 抽象语法树</font>
    - <font style="color:rgb(1, 1, 1);">遍历 AST,生成渲染函数</font>
+ **<font style="color:black;">优化</font>**<font style="color:black;">：优化抽象语法树</font>
    - <font style="color:rgb(1, 1, 1);">检测子节点中是否是纯静态节点</font>
    - <font style="color:rgb(1, 1, 1);">对数据访问点进行转换，生成 getter/setter，实现响应式</font>
    - <font style="color:rgb(1, 1, 1);">使用缓存存放已经编译好的渲染函数，避免重复编译</font>
+ **<font style="color:black;">生成</font>**<font style="color:black;">：将渲染函数打包生成新函数，返回函数的字符串形式</font>
    - <font style="color:rgb(1, 1, 1);">依赖响应式系统触发更新，执行渲染函数重新渲染</font>
    - <font style="color:rgb(1, 1, 1);">通过 diff 算法对比新旧节点，最小化更新实际 DOM</font>

### <font style="color:rgba(0, 0, 0, 0.85);">Vue、React、Angular 模板编译方式优缺点</font>
| **<font style="color:rgb(0, 0, 0);">框架</font>** | **<font style="color:rgb(0, 0, 0);">模板语法</font>** | **<font style="color:rgb(0, 0, 0);">编译方式</font>** | **<font style="color:rgb(0, 0, 0);">学习曲线</font>** |
| :--- | :--- | :--- | :--- |
| <font style="color:rgb(0, 0, 0);">Vue.js</font> | <font style="color:rgb(0, 0, 0);">简单 HTML-like</font> | <font style="color:rgb(0, 0, 0);">运行时和构建时编译</font> | <font style="color:rgb(0, 0, 0);">低</font> |
| <font style="color:rgb(0, 0, 0);">   </font> | <font style="color:rgb(0, 0, 0);">易于理解</font> | <font style="color:rgb(0, 0, 0);">   </font> | <font style="color:rgb(0, 0, 0);">   </font> |
| <font style="color:rgb(0, 0, 0);">React</font> | <font style="color:rgb(0, 0, 0);">JSX (JavaScript XML)</font> | <font style="color:rgb(0, 0, 0);">编译为 JavaScript</font> | <font style="color:rgb(0, 0, 0);">中等</font> |
| <font style="color:rgb(0, 0, 0);">   </font> | <font style="color:rgb(0, 0, 0);">嵌入 JavaScript 中</font> | <font style="color:rgb(0, 0, 0);">   </font> | <font style="color:rgb(0, 0, 0);">   </font> |
| <font style="color:rgb(0, 0, 0);">Angular</font> | <font style="color:rgb(0, 0, 0);">复杂，基于 HTML</font> | <font style="color:rgb(0, 0, 0);">预编译 (Ahead of Time)</font> | <font style="color:rgb(0, 0, 0);">较高</font> |
| <font style="color:rgb(0, 0, 0);">   </font> | <font style="color:rgb(0, 0, 0);">双向数据绑定</font> | <font style="color:rgb(0, 0, 0);">   </font> | <font style="color:rgb(0, 0, 0);">   </font> |


## Vue diff 算法的过程
### <font style="color:rgba(0, 0, 0, 0.85);">Vue2</font>
<font style="color:rgb(0, 0, 0);">关于Vue2 的 diff 算法个人的理解上是：</font>

+ <font style="color:rgb(1, 1, 1);">深度优先+双指针（头尾交叉对比）的 diff</font>
    - <font style="color:rgb(1, 1, 1);">在对比其子节点数组时，vue对每个子节点数组使用了两个指针，分别指向头尾，然后不断向中间靠拢来进行对比，</font>
    - <font style="color:rgb(1, 1, 1);">这样做的目的是尽量复用真实dom，尽量少的销毁和创建真实dom</font>
    - <font style="color:rgb(1, 1, 1);">之后的节点对比就是深度优先对比的步骤</font>
    - <font style="color:rgb(1, 1, 1);">逐层比较新旧虚拟 DOM 树的节点，对于每一层，它会按照顺序比较节点，找出差异，并标记需要更新的地方。这样的遍历方式有助于更快地发现差异，因为它会首先比较同一层级的节点，然后再递归到下一层级。</font>
    - <font style="color:rgb(1, 1, 1);">通过虚拟节点的key和tag来进行判断是否相同节点</font>
    - <font style="color:rgb(1, 1, 1);">如果相同则将旧节点关联的真实dom的挂到新节点上，然后根据需要更新属性到真实dom，然后再对比其子节点数组</font>
    - <font style="color:rgb(1, 1, 1);">如果不相同，则按照新节点的信息递归创建所有真实dom，同时挂到对应虚拟节点上，然后移除掉旧的dom</font>
    - <font style="color:black;">深度优先（同层比较）：</font>
    - <font style="color:black;">双指针：</font>

<font style="color:rgb(0, 0, 0);">这样一直递归的遍历下去，直到整棵树完成对比。</font>

#### <font style="color:rgba(0, 0, 0, 0.85);">流程图</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718769174048-7494d31f-df99-4269-981d-384f220824df.png)

### <font style="color:rgba(0, 0, 0, 0.85);">Vu3</font>
+ <font style="color:rgb(1, 1, 1);">双指针+最长递增子序列 diff算法</font>
    - <font style="color:rgb(1, 1, 1);">把没有比较过的新的vnode节点，建立一个数组，每个子元素都是</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(239, 112, 96);">0</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">里面的数字记录老节点的索引 ，数组索引就是新节点的索引</font>
    - <font style="color:rgb(1, 1, 1);">如果找到与当前老节点对应的新节点那么 ，将新节点的索引，赋值给newIndex</font>
    - <font style="color:rgb(1, 1, 1);">没有找到与老节点对应的新节点，卸载当前老节点</font>
    - <font style="color:rgb(1, 1, 1);">如果找到与老节点对应的新节点，把老节点的索引，记录在存放新节点的数组中</font>
    - <font style="color:rgb(1, 1, 1);">map = [0,0,0,0]</font>
    - <font style="color:rgb(1, 1, 1);">对于老的节点大于新的节点的情况 ， 对于超出的节点全部卸载 （ 这种情况说明已经patch完相同的vnode ）</font>
    - <font style="color:rgb(1, 1, 1);">如果新的节点大于老的节点数 ，对于剩下的节点全部以新的vnode处理（ 这种情况说明已经patch完相同的vnode ）。</font>
    - <font style="color:rgb(1, 1, 1);">如果第一步没有patch完，立即，从后往前开始patch ,如果发现不同立即跳出循环</font>
    - <font style="color:rgb(1, 1, 1);">从头对比找到有相同的节点 patch ，发现不同，立即跳出</font>
    - <font style="color:rgb(1, 1, 1);">预处理前前置节点</font>
    - <font style="color:rgb(1, 1, 1);">预处理后置节点</font>
    - <font style="color:rgb(1, 1, 1);">处理仅有新增节点的情况</font>
    - <font style="color:rgb(1, 1, 1);">处理仅有删除节点的情况</font>
    - <font style="color:rgb(1, 1, 1);">处理新增、删除、移动混合的情况</font>
    1. <font style="color:rgb(1, 1, 1);">如果节点发生移动 记录已经移动了</font>
    2. <font style="color:rgb(1, 1, 1);">patch新老节点 找到新的节点进行patch节点</font>

## Vue 常见的性能优化方式（结合项目场景）
<font style="color:rgb(0, 0, 0);">在实际项目中，Vue 的性能优化需要根据具体的场景和需求来选择合适的策略。以下是一些常见的 Vue 性能优化方式，结合项目场景进行总结：</font>

1. <font style="color:rgb(1, 1, 1);">使用生产环境构建：</font>
    - <font style="color:rgb(1, 1, 1);">在生产环境中使用 Vue 的生产版本，以减少体积和提高性能。</font>
2. <font style="color:rgb(1, 1, 1);">异步组件和路由懒加载：</font>
    - <font style="color:rgb(1, 1, 1);">对于大型项目，使用异步组件和路由懒加载，以分割代码并实现按需加载，减小初始加载体积。</font>
3. <font style="color:rgb(1, 1, 1);">合理使用 v-if 和 v-show：</font>
    - <font style="color:rgb(1, 1, 1);">对于频繁切换的元素，使用 v-show，对于不经常切换的元素，使用 v-if，以减少 DOM 元素的挂载和卸载。</font>
4. <font style="color:rgb(1, 1, 1);">合理使用 v-for：</font>
    - <font style="color:rgb(1, 1, 1);">遍历大数据集时，避免在模板中访问复杂度较高的属性，最好在数据源中进行预处理。如果数据不变，可以考虑使用 Object.freeze 冻结对象，以防止 Vue 的响应式系统监听它。</font>
5. <font style="color:rgb(1, 1, 1);">合理使用计算属性和 Watch：</font>
    - <font style="color:rgb(1, 1, 1);">将复杂的计算逻辑放入计算属性，避免在模板中进行复杂的计算。使用 deep 选项和 immediate 选项来优化 Watcher。</font>
6. <font style="color:rgb(1, 1, 1);">合理使用事件委托：</font>
    - <font style="color:rgb(1, 1, 1);">在父组件上使用事件委托，将事件处理推移到父组件上，以减少子组件的监听器数量。</font>
7. <font style="color:rgb(1, 1, 1);">合理使用 keep-alive：</font>
    - <font style="color:rgb(1, 1, 1);">对于频繁切换的组件，可以考虑使用</font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(239, 112, 96);"><keep-alive></font><font style="color:rgb(1, 1, 1);"> </font><font style="color:rgb(1, 1, 1);">缓存组件实例，以减少组件的销毁和重新创建。</font>
8. <font style="color:rgb(1, 1, 1);">合理使用缓存</font>
    - <font style="color:rgb(1, 1, 1);">利用缓存机制，例如在数据请求结果中使用缓存，以避免不必要的重复请求。</font>
9. <font style="color:rgb(1, 1, 1);">合理使用过渡效果和动画：</font>
    - <font style="color:rgb(1, 1, 1);">控制过渡效果和动画的触发时机，避免在大量元素上使用过渡效果，以提高性能。</font>
10. <font style="color:rgb(1, 1, 1);">优化网络请求 -   使用合适的数据加载方式，例如分页加载或滚动加载，以降低页面初始化时的请求量。</font>
11. <font style="color:rgb(1, 1, 1);">性能监控和分析 -   使用工具进行性能监控和分析，例如 Chrome DevTools、Vue DevTools 等，及时发现和解决性能问题</font>
12. <font style="color:rgb(1, 1, 1);">使用 Object.freeze() 进行不需要进行响应式的数据进行优化（vue2）</font>

## Vuex
<font style="color:rgb(0, 0, 0);">Vuex有几个很重要的概念：</font>

+ <font style="color:rgb(1, 1, 1);">State</font>
    - <font style="color:rgb(1, 1, 1);">存储应用状态的地方，是响应式的，即当 State 发生变化时，与之相关的组件将自动更新</font>
+ <font style="color:rgb(1, 1, 1);">Getter</font>
    - <font style="color:rgb(1, 1, 1);">类似于计算属性</font>
+ <font style="color:rgb(1, 1, 1);">Mutation</font>
    - <font style="color:rgb(1, 1, 1);">是用于变更状态的唯一途径</font>
    - <font style="color:rgb(1, 1, 1);">每个 Mutation 都有一个字符串的事件类型（type）和一个回调函数，用于实际的状态变更</font>
+ <font style="color:rgb(1, 1, 1);">Action</font>
    - <font style="color:rgb(1, 1, 1);">Action 提交的是 Mutation，而不是直接变更状态</font>
    - <font style="color:rgb(1, 1, 1);">Action 可以包含任意异步操作</font>
+ <font style="color:rgb(1, 1, 1);">Module</font>
    - <font style="color:rgb(1, 1, 1);">将 Vuex 的状态划分为模块，每个模块都有自己的 State、Getter、Mutation 和 Action</font>

### <font style="color:rgba(0, 0, 0, 0.85);">Mutation 和 Action 的区别</font>
| **<font style="color:rgb(0, 0, 0);">特点</font>** | **<font style="color:rgb(0, 0, 0);">Mutation</font>** | **<font style="color:rgb(0, 0, 0);">Action</font>** |
| :--- | :--- | :--- |
| <font style="color:rgb(0, 0, 0);">同步/异步</font> | <font style="color:rgb(0, 0, 0);">同步</font> | <font style="color:rgb(0, 0, 0);">异步</font> |
| <font style="color:rgb(0, 0, 0);">直接/间接</font> | <font style="color:rgb(0, 0, 0);">直接修改状态</font> | <font style="color:rgb(0, 0, 0);">通过提交 Mutation 间接修改状态</font> |


#### <font style="color:rgba(0, 0, 0, 0.85);">为什么Mutation是同步，Action是异步？</font>
1. <font style="color:rgb(1, 1, 1);">Vue2的响应性不完整，不能监听数组的变化、对象属性的变化，</font>
2. <font style="color:rgb(1, 1, 1);">上面也有说过，Action是调用Mutation间接修改状态，如果Mutation是异步的那么会导致一个问题就是我们知道Mutation是何时调用的，却不知道State的值是何时被需修改的。（这也主要是因为Vue2的响应式有些缺陷导致的）</font>
3. <font style="color:rgb(1, 1, 1);">同步变化，vuex 内部调用 Mutation 之前记录状态，然后调用 Mutation ，然后获得修改后的状态。</font>

## Vue-router
<font style="color:rgb(0, 0, 0);">这个比较简单，我们需要记住几个东西：</font>

1. <font style="color:rgb(1, 1, 1);">hash模式</font>
2. <font style="color:rgb(1, 1, 1);">history模式</font>

### <font style="color:rgba(0, 0, 0, 0.85);">对比</font>
| **<font style="color:rgb(0, 0, 0);">   </font>** | **<font style="color:rgb(0, 0, 0);">hash</font>** | **<font style="color:rgb(0, 0, 0);">history</font>** |
| :--- | :--- | :--- |
| <font style="color:rgb(0, 0, 0);">表现形式</font> | **<font style="color:rgb(255, 53, 2);">http://aaa/#/user/id</font>** | **<font style="color:rgb(255, 53, 2);">http://aaa/user/id</font>** |
| <font style="color:rgb(0, 0, 0);">基于api</font> | <font style="color:rgb(0, 0, 0);">onhashchange</font> | <font style="color:rgb(0, 0, 0);">配合</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">history.pushState</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">+</font><font style="color:rgb(0, 0, 0);"> </font><font style="color:rgb(0, 0, 0);">window.addEventListener("popstate", ()=> {})</font> |
| <font style="color:rgb(0, 0, 0);">配置方式</font> | <font style="color:rgb(0, 0, 0);">不需要后端</font> | **<font style="color:rgb(255, 53, 2);">需要后端协助</font>** |


**<font style="color:rgb(255, 53, 2);"></font>**

