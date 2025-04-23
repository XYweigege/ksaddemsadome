## <font style="color:rgb(31, 35, 40);">一、Object.defineProperty</font>
<font style="color:rgb(31, 35, 40);">定义：</font>`<font style="color:rgb(31, 35, 40);">Object.defineProperty()</font>`<font style="color:rgb(31, 35, 40);"> 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象</font>

##### <font style="color:rgb(31, 35, 40);">为什么能实现响应式</font>
<font style="color:rgb(31, 35, 40);">通过</font>`<font style="color:rgb(31, 35, 40);">defineProperty</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">两个属性，</font>`<font style="color:rgb(31, 35, 40);">get</font>`<font style="color:rgb(31, 35, 40);">及</font>`<font style="color:rgb(31, 35, 40);">set</font>`

+ <font style="color:rgb(31, 35, 40);">get</font>

<font style="color:rgb(31, 35, 40);">属性的 getter 函数，当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 this 对象（由于继承关系，这里的this并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值</font>

+ <font style="color:rgb(31, 35, 40);">set</font>

<font style="color:rgb(31, 35, 40);">属性的 setter 函数，当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 this 对象。默认为 undefined</font>

<font style="color:rgb(31, 35, 40);">下面通过代码展示：</font>

<font style="color:rgb(31, 35, 40);">定义一个响应式函数</font>`<font style="color:rgb(31, 35, 40);">defineReactive</font>`

```javascript
function update() {
  app.innerText = obj.foo
}

function defineReactive(obj, key, val) {
  Object.defineProperty(obj, key, {
    get() {
      console.log(`get ${key}:${val}`);
      return val
    },
    set(newVal) {
      if (newVal !== val) {
        val = newVal
        update()
      }
    }
  })
}
```

<font style="color:rgb(31, 35, 40);">调用</font>`<font style="color:rgb(31, 35, 40);">defineReactive</font>`<font style="color:rgb(31, 35, 40);">，数据发生变化触发</font>`<font style="color:rgb(31, 35, 40);">update</font>`<font style="color:rgb(31, 35, 40);">方法，实现数据响应式</font>

```javascript
const obj = {}
defineReactive(obj, 'foo', '')
setTimeout(()=>{
  obj.foo = new Date().toLocaleTimeString()
},1000)
```

<font style="color:rgb(31, 35, 40);">在对象存在多个</font>`<font style="color:rgb(31, 35, 40);">key</font>`<font style="color:rgb(31, 35, 40);">情况下，需要进行遍历</font>

```javascript
function observe(obj) {
  if (typeof obj !== 'object' || obj == null) {
    return
  }
  Object.keys(obj).forEach(key => {
    defineReactive(obj, key, obj[key])
  })
}
```

<font style="color:rgb(31, 35, 40);">如果存在嵌套对象的情况，还需要在</font>`<font style="color:rgb(31, 35, 40);">defineReactive</font>`<font style="color:rgb(31, 35, 40);">中进行递归</font>

```javascript
function defineReactive(obj, key, val) {
  observe(val)
  Object.defineProperty(obj, key, {
    get() {
      console.log(`get ${key}:${val}`);
      return val
    },
    set(newVal) {
      if (newVal !== val) {
        val = newVal
        update()
      }
    }
  })
}
```

<font style="color:rgb(31, 35, 40);">当给</font>`<font style="color:rgb(31, 35, 40);">key</font>`<font style="color:rgb(31, 35, 40);">赋值为对象的时候，还需要在</font>`<font style="color:rgb(31, 35, 40);">set</font>`<font style="color:rgb(31, 35, 40);">属性中进行递归</font>

```javascript
set(newVal) {
  if (newVal !== val) {
    observe(newVal) // 新值是对象的情况
    notifyUpdate()
  }
}
```

<font style="color:rgb(31, 35, 40);">上述例子能够实现对一个对象的基本响应式，但仍然存在诸多问题</font>

<font style="color:rgb(31, 35, 40);">现在对一个对象进行删除与添加属性操作，无法劫持到</font>

```javascript
const obj = {
  foo: "foo",
  bar: "bar"
}
observe(obj)
delete obj.foo // no ok
obj.jar = 'xxx' // no ok
```

<font style="color:rgb(31, 35, 40);">当</font><font style="color:rgb(31, 35, 40);">我们对一个数组进行监听的时候，并不那么好使了</font>

```javascript
const arrData = [1,2,3,4,5];
arrData.forEach((val,index)=>{
  defineProperty(arrData,index,val)
})
arrData.push() // no ok
arrData.pop()  // no ok
arrDate[0] = 99 // ok
```

<font style="color:rgb(31, 35, 40);">可以看到数据的</font>`<font style="color:rgb(31, 35, 40);">api</font>`<font style="color:rgb(31, 35, 40);">无法劫持到，从而无法实现数据响应式，</font>

<font style="color:rgb(31, 35, 40);">所以在</font>`<font style="color:rgb(31, 35, 40);">Vue2</font>`<font style="color:rgb(31, 35, 40);">中，增加了</font>`<font style="color:rgb(31, 35, 40);">set</font>`<font style="color:rgb(31, 35, 40);">、</font>`<font style="color:rgb(31, 35, 40);">delete</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">API，并且对数组</font>`<font style="color:rgb(31, 35, 40);">api</font>`<font style="color:rgb(31, 35, 40);">方法进行一个重写</font>

<font style="color:rgb(31, 35, 40);">还有一个问题则是，如果存在深层的嵌套对象关系，需要深层的进行监听，造成了性能的极大问题</font>

### <font style="color:rgb(31, 35, 40);">小结</font>
+ <font style="color:rgb(31, 35, 40);">检测不到对象属性的添加和删除</font>
+ <font style="color:rgb(31, 35, 40);">数组</font>`<font style="color:rgb(31, 35, 40);">API</font>`<font style="color:rgb(31, 35, 40);">方法无法监听到</font>
+ <font style="color:rgb(31, 35, 40);">需要对每个属性进行遍历监听，如果嵌套对象，需要深层监听，造成性能问题</font>

## <font style="color:rgb(31, 35, 40);">二、proxy</font>
`<font style="color:rgb(31, 35, 40);">Proxy</font>`<font style="color:rgb(31, 35, 40);">的监听是针对一个对象的，那么对这个对象的所有操作会进入监听操作，这就完全可以代理所有属性了</font>

<font style="color:rgb(31, 35, 40);">在</font>`<font style="color:rgb(31, 35, 40);">ES6</font>`<font style="color:rgb(31, 35, 40);">系列中，我们详细讲解过</font>`<font style="color:rgb(31, 35, 40);">Proxy</font>`<font style="color:rgb(31, 35, 40);">的使用，就不再述说了</font>

<font style="color:rgb(31, 35, 40);">下面通过代码进行展示：</font>

<font style="color:rgb(31, 35, 40);">定义一个响应式方法</font>`<font style="color:rgb(31, 35, 40);">reactive</font>`

```javascript
function reactive(obj) {
  if (typeof obj !== 'object' && obj != null) {
    return obj
  }
  // Proxy相当于在对象外层加拦截
  const observed = new Proxy(obj, {
    get(target, key, receiver) {
      const res = Reflect.get(target, key, receiver)
      console.log(`获取${key}:${res}`)
      return res
    },
    set(target, key, value, receiver) {
      const res = Reflect.set(target, key, value, receiver)
      console.log(`设置${key}:${value}`)
      return res
    },
    deleteProperty(target, key) {
      const res = Reflect.deleteProperty(target, key)
      console.log(`删除${key}:${res}`)
      return res
    }
  })
  return observed
}
```

<font style="color:rgb(31, 35, 40);">测试一下简单数据的操作，发现都能劫持</font>

```javascript
const state = reactive({
  foo: 'foo'
})
// 1.获取
state.foo // ok
// 2.设置已存在属性
state.foo = 'fooooooo' // ok
// 3.设置不存在属性
state.dong = 'dong' // ok
// 4.删除属性
delete state.dong // ok
```

<font style="color:rgb(31, 35, 40);">再测试嵌套对象情况，这时候发现就不那么 OK 了</font>

```javascript
const state = reactive({
  bar: { a: 1 }
})

// 设置嵌套对象属性
state.bar.a = 10 // no ok
```

<font style="color:rgb(31, 35, 40);">如果要解决，需要在</font>`<font style="color:rgb(31, 35, 40);">get</font>`<font style="color:rgb(31, 35, 40);">之上再进行一层代理</font>

```javascript
function reactive(obj) {
  if (typeof obj !== 'object' && obj != null) {
    return obj
  }
  // Proxy相当于在对象外层加拦截
  const observed = new Proxy(obj, {
    get(target, key, receiver) {
      const res = Reflect.get(target, key, receiver)
      console.log(`获取${key}:${res}`)
      return isObject(res) ? reactive(res) : res
    },
    return observed
}
```

## <font style="color:rgb(31, 35, 40);">三、总结</font>
`<font style="color:rgb(31, 35, 40);">Object.defineProperty</font>`<font style="color:rgb(31, 35, 40);">只能遍历对象属性进行劫持</font>

```javascript
function observe(obj) {
  if (typeof obj !== 'object' || obj == null) {
    return
  }
  Object.keys(obj).forEach(key => {
    defineReactive(obj, key, obj[key])
  })
}
```

`<font style="color:rgb(31, 35, 40);">Proxy</font>`<font style="color:rgb(31, 35, 40);">直接可以劫持整个对象，并返回一个新对象，我们可以只操作新的对象达到响应式目的</font>

```javascript
function reactive(obj) {
  if (typeof obj !== 'object' && obj != null) {
    return obj
  }
  // Proxy相当于在对象外层加拦截
  const observed = new Proxy(obj, {
    get(target, key, receiver) {
      const res = Reflect.get(target, key, receiver)
      console.log(`获取${key}:${res}`)
      return res
    },
    set(target, key, value, receiver) {
      const res = Reflect.set(target, key, value, receiver)
      console.log(`设置${key}:${value}`)
      return res
    },
    deleteProperty(target, key) {
      const res = Reflect.deleteProperty(target, key)
      console.log(`删除${key}:${res}`)
      return res
    }
  })
  return observed
}
```

`<font style="color:rgb(31, 35, 40);">Proxy</font>`<font style="color:rgb(31, 35, 40);">可以直接监听数组的变化（</font>`<font style="color:rgb(31, 35, 40);">push</font>`<font style="color:rgb(31, 35, 40);">、</font>`<font style="color:rgb(31, 35, 40);">shift</font>`<font style="color:rgb(31, 35, 40);">、</font>`<font style="color:rgb(31, 35, 40);">splice</font>`<font style="color:rgb(31, 35, 40);">）</font>

```javascript
const obj = [1,2,3]
const proxtObj = reactive(obj)
obj.psuh(4) // ok
```

`<font style="color:rgb(31, 35, 40);">Proxy</font>`<font style="color:rgb(31, 35, 40);">有多达13种拦截方法,不限于</font>`<font style="color:rgb(31, 35, 40);">apply</font>`<font style="color:rgb(31, 35, 40);">、</font>`<font style="color:rgb(31, 35, 40);">ownKeys</font>`<font style="color:rgb(31, 35, 40);">、</font>`<font style="color:rgb(31, 35, 40);">deleteProperty</font>`<font style="color:rgb(31, 35, 40);">、</font>`<font style="color:rgb(31, 35, 40);">has</font>`<font style="color:rgb(31, 35, 40);">等等，这是</font>`<font style="color:rgb(31, 35, 40);">Object.defineProperty</font>`<font style="color:rgb(31, 35, 40);">不具备的</font>

<font style="color:rgb(31, 35, 40);">正因为</font>`<font style="color:rgb(31, 35, 40);">defineProperty</font>`<font style="color:rgb(31, 35, 40);">自身的缺陷，导致</font>`<font style="color:rgb(31, 35, 40);">Vue2</font>`<font style="color:rgb(31, 35, 40);">在实现响应式过程需要实现其他的方法辅助（如重写数组方法、增加额外</font>`<font style="color:rgb(31, 35, 40);">set</font>`<font style="color:rgb(31, 35, 40);">、</font>`<font style="color:rgb(31, 35, 40);">delete</font>`<font style="color:rgb(31, 35, 40);">方法）</font>

```javascript
// 数组重写
const originalProto = Array.prototype
const arrayProto = Object.create(originalProto)
  ['push', 'pop', 'shift', 'unshift', 'splice', 'reverse', 'sort'].forEach(method => {
    arrayProto[method] = function () {
      originalProto[method].apply(this.arguments)
      dep.notice()
    }
  });

// set、delete
Vue.set(obj,'bar','newbar')
Vue.delete(obj),'bar')
```

`<font style="color:rgb(31, 35, 40);">Proxy</font>`<font style="color:rgb(31, 35, 40);"> 不兼容IE，也没有 </font>`<font style="color:rgb(31, 35, 40);">polyfill</font>`<font style="color:rgb(31, 35, 40);">, </font>`<font style="color:rgb(31, 35, 40);">defineProperty</font>`<font style="color:rgb(31, 35, 40);"> 能支持到IE9</font>

