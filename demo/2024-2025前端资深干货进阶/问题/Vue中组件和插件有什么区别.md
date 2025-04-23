### <font style="color:rgb(31, 35, 40);">编写形式</font>
#### <font style="color:rgb(31, 35, 40);">编写组件</font>
<font style="color:rgb(31, 35, 40);">编写一个组件，可以有很多方式，我们最常见的就是</font>`<font style="color:rgb(31, 35, 40);">vue</font>`<font style="color:rgb(31, 35, 40);">单文件的这种格式，每一个</font>`<font style="color:rgb(31, 35, 40);">.vue</font>`<font style="color:rgb(31, 35, 40);">文件我们都可以看成是一个组件</font>

`<font style="color:rgb(31, 35, 40);">vue</font>`<font style="color:rgb(31, 35, 40);">文件标准格式</font>

```vue
<template>
</template>
<script>
  export default{ 
    ...
      }
</script>
<style>
</style>
```

<font style="color:rgb(31, 35, 40);">我们还可以通过</font>`<font style="color:rgb(31, 35, 40);">template</font>`<font style="color:rgb(31, 35, 40);">属性来编写一个组件，如果组件内容多，我们可以在外部定义</font>`<font style="color:rgb(31, 35, 40);">template</font>`<font style="color:rgb(31, 35, 40);">组件内容，如果组件内容并不多，我们可直接写在</font>`<font style="color:rgb(31, 35, 40);">template</font>`<font style="color:rgb(31, 35, 40);">属性上</font>

```javascript
<template id="testComponent">     // 组件显示的内容
  <div>component!</div>   
</template>

Vue.component('componentA',{ 
  template: '#testComponent'  
    template: `<div>component</div>`  // 组件内容少可以通过这种形式
})
```

#### <font style="color:rgb(31, 35, 40);">编写插件</font>
`<font style="color:rgb(31, 35, 40);">vue</font>`<font style="color:rgb(31, 35, 40);">插件的实现应该暴露一个</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">install</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">方法。这个方法的第一个参数是</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">Vue</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">构造器，第二个参数是一个可选的选项对象</font>

```javascript
MyPlugin.install = function (Vue, options) {
  // 1. 添加全局方法或 property
  Vue.myGlobalMethod = function () {
    // 逻辑...
  }

  // 2. 添加全局资源
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 逻辑...
    }
    ...
  })

  // 3. 注入组件选项
  Vue.mixin({
    created: function () {
      // 逻辑...
    }
      ...
      })

  // 4. 添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 逻辑...
  }
}
```

### <font style="color:rgb(31, 35, 40);">注册形式</font>
#### <font style="color:rgb(31, 35, 40);">组件注册</font>
`<font style="color:rgb(31, 35, 40);">vue</font>`<font style="color:rgb(31, 35, 40);">组件注册主要分为全局注册与局部注册</font>

<font style="color:rgb(31, 35, 40);">全局注册通过</font>`<font style="color:rgb(31, 35, 40);">Vue.component</font>`<font style="color:rgb(31, 35, 40);">方法，第一个参数为组件的名称，第二个参数为传入的配置项</font>

<font style="color:rgb(31, 35, 40);">Vue.component('my-component-name', { /* ... */ })</font>

<font style="color:rgb(31, 35, 40);">局部注册只需在用到的地方通过</font>`<font style="color:rgb(31, 35, 40);">components</font>`<font style="color:rgb(31, 35, 40);">属性注册一个组件</font>

```vue
const component1 = {...} // 定义一个组件

  export default {
  components:{
  component1   // 局部注册
  }
}
```

#### <font style="color:rgb(31, 35, 40);">插件注册</font>
<font style="color:rgb(31, 35, 40);">插件的注册通过</font>`<font style="color:rgb(31, 35, 40);">Vue.use()</font>`<font style="color:rgb(31, 35, 40);">的方式进行注册（安装），第一个参数为插件的名字，第二个参数是可选择的配置项</font>

<font style="color:rgb(31, 35, 40);">Vue.use(插件名字,{ /* ... */} )</font>

<font style="color:rgb(31, 35, 40);">注意的是：</font>

<font style="color:rgb(31, 35, 40);">注册插件的时候，需要在调用</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">new Vue()</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">启动应用之前完成</font>

`<font style="color:rgb(31, 35, 40);">Vue.use</font>`<font style="color:rgb(31, 35, 40);">会自动阻止多次注册相同插件，只会注册一次</font>

  


### <font style="color:rgb(31, 35, 40);">使用场景</font>
<font style="color:rgb(31, 35, 40);">组件</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">(Component)</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">是用来构成你的</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">App</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">的业务模块，它的目标是</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">App.vue</font>`

<font style="color:rgb(31, 35, 40);">插件</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">(Plugin)</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">是用来增强你的技术栈的功能模块，它的目标是</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">Vue</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">本身</font>

<font style="color:rgb(31, 35, 40);">简单来说，插件就是指对</font>`<font style="color:rgb(31, 35, 40);">Vue</font>`<font style="color:rgb(31, 35, 40);">的功能的增强或补充</font>

