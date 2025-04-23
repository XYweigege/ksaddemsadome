<br/>color1
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">由</font><font style="color:rgb(85, 85, 85);">于ES5环境没有</font><font style="color:rgb(199, 37, 78);">block</font><font style="color:rgb(85, 85, 85);">的概念，所以是无法百分百实现</font><font style="color:rgb(199, 37, 78);">const</font><font style="color:rgb(85, 85, 85);">，只能是挂载到某个对象下，要么是全局的</font><font style="color:rgb(199, 37, 78);">windo</font><font style="color:rgb(85, 85, 85);">w，要么就是自定义一个</font><font style="color:rgb(199, 37, 78);">object</font><font style="color:rgb(85, 85, 85);">来当容器</font>

<br/>

```javascript
var __const = function __const (data, value) {
  window.data = value // 把要定义的data挂载到window下，并赋值value
  Object.defineProperty(window, data, { // 利用Object.defineProperty的能力劫持当前对象，并修改其属性描述符
    enumerable: false,
    configurable: false,
    get: function () {
      return value
    },
    set: function (data) {
      if (data !== value) { // 当要对当前属性进行赋值时，则抛出错误！
        throw new TypeError('Assignment to constant variable.')
      } else {
        return value
      }
    }
  })
}
__const('a', 10)
console.log(a)
delete a
console.log(a)
for (let item in window) { // 因为const定义的属性在global下也是不存在的，所以用到了enumerable: false来模拟这一功能
  if (item === 'a') { // 因为不可枚举，所以不执行
    console.log(window[item])
  }
}
a = 20 // 报错
```

<br/>color1
<font style="color:rgb(199, 37, 78);">Vue</font><font style="color:rgb(85, 85, 85);">目前双向绑定的核心实现思路就是利用</font><font style="color:rgb(199, 37, 78);">Object.defineProperty</font><font style="color:rgb(85, 85, 85);">对</font><font style="color:rgb(199, 37, 78);">get</font><font style="color:rgb(85, 85, 85);">跟</font><font style="color:rgb(199, 37, 78);">set</font><font style="color:rgb(85, 85, 85);">进行劫持，监听用户对属性进行调用以及赋值时的具体情况，从而实现的双向绑定</font>

<br/>

