### <font style="color:rgb(44, 62, 80);">vue3 对 vue2 有什么优势</font>
+ <font style="color:rgb(44, 62, 80);">性能更好（编译优化、使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">proxy</font><font style="color:rgb(44, 62, 80);">等）</font>
+ <font style="color:rgb(44, 62, 80);">体积更小</font>
+ <font style="color:rgb(44, 62, 80);">更好的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">TS</font><font style="color:rgb(44, 62, 80);">支持</font>
+ <font style="color:rgb(44, 62, 80);">更好的代码组织</font>
+ <font style="color:rgb(44, 62, 80);">更好的逻辑抽离</font>
+ <font style="color:rgb(44, 62, 80);">更多新功能</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#vue3-%E5%92%8C-vue2-%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">vue3 和 vue2 的生命周期有什么区别</font>
**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Options API</font>****<font style="color:rgb(44, 62, 80);">生命周期</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeDestroy</font><font style="color:rgb(44, 62, 80);">改为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeUnmount</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">destroyed</font><font style="color:rgb(44, 62, 80);">改为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">umounted</font>
+ <font style="color:rgb(44, 62, 80);">其他沿用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue2</font><font style="color:rgb(44, 62, 80);">生命周期</font>

**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Composition API</font>****<font style="color:rgb(44, 62, 80);">生命周期</font>**

```javascript
import { onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue'

export default {
  name: 'LifeCycles',
  props: {
    msg: String
  },
  // setup等于 beforeCreate 和 created
  setup() {
    console.log('setup')

    onBeforeMount(() => {
      console.log('onBeforeMount')
    })
    onMounted(() => {
      console.log('onMounted')
    })
    onBeforeUpdate(() => {
      console.log('onBeforeUpdate')
    })
    onUpdated(() => {
      console.log('onUpdated')
    })
    onBeforeUnmount(() => {
      console.log('onBeforeUnmount')
    })
    onUnmounted(() => {
      console.log('onUnmounted')
    })
  },

  // 兼容vue2生命周期 options API和composition API生命周期二选一
  beforeCreate() {
    console.log('beforeCreate')
  },
  created() {
    console.log('created')
  },
  beforeMount() {
    console.log('beforeMount')
  },
  mounted() {
    console.log('mounted')
  },
  beforeUpdate() {
    console.log('beforeUpdate')
  },
  updated() {
    console.log('updated')
  },
  // beforeDestroy 改名
  beforeUnmount() {
    console.log('beforeUnmount')
  },
  // destroyed 改名
  unmounted() {
    console.log('unmounted')
  }
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E5%A6%82%E4%BD%95%E7%90%86%E8%A7%A3composition-api%E5%92%8Coptions-api)<font style="color:rgb(44, 62, 80);">如何理解Composition API和Options API</font>
**<font style="color:rgb(44, 62, 80);">composition API对比Option API</font>**

+ <font style="color:rgb(44, 62, 80);">Composition API带来了什么</font>
    - <font style="color:rgb(44, 62, 80);">更好的代码组织</font>
    - <font style="color:rgb(44, 62, 80);">更好的逻辑复用</font>
    - <font style="color:rgb(44, 62, 80);">更好的类型推导</font>
+ <font style="color:rgb(44, 62, 80);">Composition API和Options API如何选择</font>
    - <font style="color:rgb(44, 62, 80);">不建议共用，会引起混乱</font>
    - <font style="color:rgb(44, 62, 80);">小型项目、业务逻辑简单，用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Option API</font><font style="color:rgb(44, 62, 80);">成本更小一些</font>
    - <font style="color:rgb(44, 62, 80);">中大型项目、逻辑复杂，用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Composition API</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#ref%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8)<font style="color:rgb(44, 62, 80);">ref如何使用</font>
**<font style="color:rgb(44, 62, 80);">ref</font>**

+ <font style="color:rgb(44, 62, 80);">生成值类型的响应式数据</font>
+ <font style="color:rgb(44, 62, 80);">可用于模板和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactive</font>
+ <font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.value</font><font style="color:rgb(44, 62, 80);">修改值</font>

```vue
<template>
    <p>ref demo {{ageRef}} {{state.name}}</p>
</template>

<script>
import { ref, reactive } from 'vue'

export default {
    name: 'Ref',
    setup() {
        const ageRef = ref(20) // 值类型 响应式
        const nameRef = ref('test')

        const state = reactive({
            name: nameRef
        })

        setTimeout(() => {
            console.log('ageRef', ageRef.value)

            ageRef.value = 25 // .value 修改值
            nameRef.value = 'testA'
        }, 1500);

        return {
            ageRef,
            state
        }
    }
}
</script>
```

```vue
<!-- ref获取dom节点 -->
<template>
    <p ref="elemRef">我是一行文字</p>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
    name: 'RefTemplate',
    setup() {
        const elemRef = ref(null)

        onMounted(() => {
            console.log('ref template', elemRef.value.innerHTML, elemRef.value)
        })

        return {
            elemRef
        }
    }
}
</script>
```

### [](https://www.123fe.net/docs/base/high-frequency.html#toref%E5%92%8Ctorefs%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8%E5%92%8C%E6%9C%80%E4%BD%B3%E6%96%B9%E5%BC%8F)<font style="color:rgb(44, 62, 80);">toRef和toRefs如何使用和最佳方式</font>
**<font style="color:rgb(44, 62, 80);">toRef</font>**

+ <font style="color:rgb(44, 62, 80);">针对一个响应式对象（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactive</font><font style="color:rgb(44, 62, 80);">封装的）的一个属性，创建一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">，具有响应式</font>
+ <font style="color:rgb(44, 62, 80);">两者保持引用关系</font>

**<font style="color:rgb(44, 62, 80);">toRefs</font>**

+ <font style="color:rgb(44, 62, 80);">将响应式对象（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactive</font><font style="color:rgb(44, 62, 80);">封装的）转化为普通对象</font>
+ <font style="color:rgb(44, 62, 80);">对象的每个属性都是对象的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font>
+ <font style="color:rgb(44, 62, 80);">两者保持引用关系</font>

<font style="color:rgb(44, 62, 80);">合成函数返回响应式对象</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784005515-5d07ca81-514e-4d31-89b3-b55b324d7a1f.png)

**<font style="color:rgb(44, 62, 80);">最佳使用方式</font>**

+ <font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactive</font><font style="color:rgb(44, 62, 80);">做对象的响应式，用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">做值类型响应式（基本类型）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">中返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toRefs(state)</font><font style="color:rgb(44, 62, 80);">，或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toRef(state, 'prop')</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">的变量命名都用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">xxRef</font>
+ <font style="color:rgb(44, 62, 80);">合成函数返回响应式对象时，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toRefs</font><font style="color:rgb(44, 62, 80);">，有助于使用方对数据进行解构时，不丢失响应式</font>

```vue
<template>
    <p>toRef demo - {{ageRef}} - {{state.name}} {{state.age}}</p>
</template>

<script>
import { ref, toRef, reactive } from 'vue'

export default {
    name: 'ToRef',
    setup() {
        const state = reactive({
            age: 20,
            name: 'test'
        })

        const age1 = computed(() => {
            return state.age + 1
        })

        // toRef 如果用于普通对象（非响应式对象），产出的结果不具备响应式
        // const state = {
        //     age: 20,
        //     name: 'test'
        // }
        // 一个响应式对象state其中一个属性要单独拿出来实现响应式用toRef
        const ageRef = toRef(state, 'age')

        setTimeout(() => {
            state.age = 25
        }, 1500)

        setTimeout(() => {
            ageRef.value = 30 // .value 修改值
        }, 3000)

        return {
            state,
            ageRef
        }
    }
}
</script>
```

```vue
<template>
    <p>toRefs demo {{age}} {{name}}</p>
</template>

<script>
import { ref, toRef, toRefs, reactive } from 'vue'

export default {
    name: 'ToRefs',
    setup() {
        const state = reactive({
            age: 20,
            name: 'test'
        })

        const stateAsRefs = toRefs(state) // 将响应式对象，变成普通对象

        // const { age: ageRef, name: nameRef } = stateAsRefs // 每个属性，都是 ref 对象
        // return {
        //     ageRef,
        //     nameRef
        // }

        setTimeout(() => {
            state.age = 25
        }, 1500)

        return stateAsRefs
    }
}
</script>
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81ref%E3%80%81toref%E3%80%81torefs)<font style="color:rgb(44, 62, 80);">深入理解为什么需要ref、toRef、toRefs</font>
**<font style="color:rgb(44, 62, 80);">为什么需要用 ref</font>**

+ <font style="color:rgb(44, 62, 80);">返回值类型，会丢失响应式</font>
+ <font style="color:rgb(44, 62, 80);">如在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">computed</font><font style="color:rgb(44, 62, 80);">、合成函数，都有可能返回值类型</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);">如不定义</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">，用户将制造</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">，反而更混乱</font>

**<font style="color:rgb(44, 62, 80);">为何ref需要.value属性</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">是一个对象（不丢失响应式），</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">存储值</font>
+ <font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.value</font><font style="color:rgb(44, 62, 80);">属性的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">set</font><font style="color:rgb(44, 62, 80);">实现响应式</font>
+ <font style="color:rgb(44, 62, 80);">用于模板、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactive</font><font style="color:rgb(44, 62, 80);">时，不需要</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.value</font><font style="color:rgb(44, 62, 80);">，其他情况都要</font>

**<font style="color:rgb(44, 62, 80);">为什么需要toRef和toRefs</font>**

+ **<font style="color:rgb(44, 62, 80);">初衷</font>**<font style="color:rgb(44, 62, 80);">：不丢失响应式的情况下，把对象数据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">分解/扩散</font>
+ **<font style="color:rgb(44, 62, 80);">前端</font>**<font style="color:rgb(44, 62, 80);">：针对的是响应式对象（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactive</font><font style="color:rgb(44, 62, 80);">封装的）非普通对象</font>
+ <font style="color:rgb(44, 62, 80);">注意：</font>**<font style="color:rgb(44, 62, 80);">不创造</font>**<font style="color:rgb(44, 62, 80);">响应式，而是</font>**<font style="color:rgb(44, 62, 80);">延续</font>**<font style="color:rgb(44, 62, 80);">响应式</font>

```vue
<template>
    <p>why ref demo {{state.age}} - {{age1}}</p>
</template>

<script>
import { ref, toRef, toRefs, reactive, computed } from 'vue'

function useFeatureX() {
    const state = reactive({
        x: 1,
        y: 2
    })

    return toRefs(state)
}

export default {
    name: 'WhyRef',
    setup() {
        // 解构不丢失响应式
        const { x, y } = useFeatureX()

        const state = reactive({
            age: 20,
            name: 'test'
        })

        // computed 返回的是一个类似于 ref 的对象，也有 .value
        const age1 = computed(() => {
            return state.age + 1
        })

        setTimeout(() => {
            state.age = 25
        }, 1500)

        return {
            state,
            age1,
            x,
            y
        }
    }
}
</script>
```

### [](https://www.123fe.net/docs/base/high-frequency.html#vue3%E5%8D%87%E7%BA%A7%E4%BA%86%E5%93%AA%E4%BA%9B%E9%87%8D%E8%A6%81%E5%8A%9F%E8%83%BD)<font style="color:rgb(44, 62, 80);">vue3升级了哪些重要功能</font>
**<font style="color:rgb(44, 62, 80);">1. createApp</font>**

```javascript
// vue2
const app = new Vue({/**选项**/})
Vue.use(/****/)
Vue.mixin(/****/)
Vue.component(/****/)
Vue.directive(/****/)

// vue3
const app = createApp({/**选项**/})
app.use(/****/)
app.mixin(/****/)
app.component(/****/)
app.directive(/****/)
```

**<font style="color:rgb(44, 62, 80);">2. emits属性</font>**

```javascript
// 父组件
<Hello :msg="msg" @onSayHello="sayHello">

  // 子组件
  export default {
    name: 'Hello',
    props: {
      msg: String
    },
    emits: ['onSayHello'], // 声明emits
    setup(props, {emit}) {
      emit('onSayHello', 'aaa')
    }
  }
```

**<font style="color:rgb(44, 62, 80);">3. 多事件</font>**

```vue
<!-- 定义多个事件 -->
<button @click="one($event),two($event)">提交</button>
```

**<font style="color:rgb(44, 62, 80);">4. Fragment</font>**

```vue
<!-- vue2 -->
<template>
    <div>
        <h2>{{title}}</h2>
        <p>test</p>
    </div>
</template>

<!-- vue3：不在使用div节点包裹 -->
<template>
    <h2>{{title}}</h2>
    <p>test</p>
</template>
```

**<font style="color:rgb(44, 62, 80);">5. 移除.sync</font>**

```vue
<!-- vue2 -->
<MyComponent :title.sync="title" />

<!-- vue3 简写 -->
<MyComponent v-model:title="title" />
<!-- 非简写 -->
<MyComponent :title="title" @update:title="title = $event" />
```

**<font style="color:rgb(44, 62, 80);">.sync用法</font>**

<font style="color:rgb(44, 62, 80);">父组件把属性给子组件，子组件修改了后还能同步到父组件中来</font>

```vue
<template>
  <button @click="close">关闭</button>
</template>
<script>
export default {
    props: {
        isVisible: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        close () {
            this.$emit('update:isVisible', false);
        }
    }
};
</script>
```

```vue
<!-- 父组件使用 -->
<chlid-component :isVisible.sync="isVisible"></chlid-component>
```

```vue
<text-doc :title="doc.title" @update:title="doc.title = $event"></text-doc>

<!-- 为了方便期间，为这种模式提供一个简写 .sync -->
<text-doc :title.sync="doc.title" />
```

**<font style="color:rgb(44, 62, 80);">6. 异步组件的写法</font>**

```javascript
// vue2写法
new Vue({
  components: {
    'my-component': ()=>import('./my-component.vue')
  }
})
```

```javascript
// vue3写法
import {createApp, defineAsyncComponent} from 'vue'

export default {
  components: {
    AsyncComponent: defineAsyncComponent(()=>import('./AsyncComponent.vue'))
  }
}
```

**<font style="color:rgb(44, 62, 80);">7. 移除filter</font>**

```vue
<!-- 以下filter在vue3中不可用了 -->

<!-- 在花括号中 -->
{{message | capitalize}}

<!-- 在v-bind中 -->
<div v-bind:id="rawId | formatId"></div>
```

**<font style="color:rgb(44, 62, 80);">8. Teleport</font>**

```vue
<button @click="modalOpen = true">
 open
</button>

<!-- 通过teleport把弹窗放到body下 -->
<teleport to="body">
 <div v-if="modalOpen" classs="modal">
   <div>
     teleport弹窗，父元素是body
     <button @click="modalOpen = false">close</button>
   </div>
 </div>
</teleport>
```

**<font style="color:rgb(44, 62, 80);">9. Suspense</font>**

```html
<Suspense>
 <template>
    <!-- 异步组件 -->
   <Test1 />  
 </template>
 <!-- fallback是一个具名插槽，即Suspense内部有两个slot，一个具名插槽fallback -->
 <template fallback>
    loading...
 </template>
</Suspense>
```

**<font style="color:rgb(44, 62, 80);">10. Composition API</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactive</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">readonly</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watch</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watchEffect</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font>
+ <font style="color:rgb(44, 62, 80);">生命周期钩子函数</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#composition-api-%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E9%80%BB%E8%BE%91%E5%A4%8D%E7%94%A8)<font style="color:rgb(44, 62, 80);">Composition API 如何实现逻辑复用</font>
+ <font style="color:rgb(44, 62, 80);">抽离逻辑代码到一个函数</font>
+ <font style="color:rgb(44, 62, 80);">函数命名约定为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useXx</font><font style="color:rgb(44, 62, 80);">格式（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Hooks</font><font style="color:rgb(44, 62, 80);">也是）</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">中引用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useXx</font><font style="color:rgb(44, 62, 80);">函数</font>

```vue
<template>
    <p>mouse position {{x}} {{y}}</p>
</template>

<script>
import { reactive } from 'vue'
import useMousePosition from './useMousePosition'
// import useMousePosition2 from './useMousePosition'

export default {
    name: 'MousePosition',
    setup() {
        const { x, y } = useMousePosition()
        return {
            x,
            y
        }

        // const state = useMousePosition2()
        // return {
        //     state
        // }
    }
}
</script>
```

```javascript
import { reactive, ref, onMounted, onUnmounted } from 'vue'

function useMousePosition() {
  const x = ref(0)
  const y = ref(0)

  function update(e) {
    x.value = e.pageX
    y.value = e.pageY
  }

  onMounted(() => {
    console.log('useMousePosition mounted')
    window.addEventListener('mousemove', update)
  })

  onUnmounted(() => {
    console.log('useMousePosition unMounted')
    window.removeEventListener('mousemove', update)
  })

  // 合成函数尽量返回ref或toRefs(state)  state = reactive({})
  // 这样在使用的时候可以解构但不丢失响应式
  return {
    x,
    y
  }
}

// function useMousePosition2() {
//     const state = reactive({
//         x: 0,
//         y: 0
//     })

//     function update(e) {
//         state.x = e.pageX
//         state.y = e.pageY
//     }

//     onMounted(() => {
//         console.log('useMousePosition mounted')
//         window.addEventListener('mousemove', update)
//     })

//     onUnmounted(() => {
//         console.log('useMousePosition unMounted')
//         window.removeEventListener('mousemove', update)
//     })

//     return state
// }

export default useMousePosition
// export default useMousePosition2
```

### [](https://www.123fe.net/docs/base/high-frequency.html#vue3%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E5%93%8D%E5%BA%94%E5%BC%8F)<font style="color:rgb(44, 62, 80);">Vue3如何实现响应式</font>
+ <font style="color:rgb(44, 62, 80);">回顾</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue2</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font>
+ <font style="color:rgb(44, 62, 80);">缺点</font>
    - <font style="color:rgb(44, 62, 80);">深度监听对象需要一次性递归</font>
    - <font style="color:rgb(44, 62, 80);">无法监听新增属性、删除属性(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.set</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.delete</font><font style="color:rgb(44, 62, 80);">)</font>
    - <font style="color:rgb(44, 62, 80);">无法监听原生数组，需要特殊处理</font>
+ <font style="color:rgb(44, 62, 80);">学习</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">proxy</font><font style="color:rgb(44, 62, 80);">语法</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue3</font><font style="color:rgb(44, 62, 80);">中如何使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">proxy</font><font style="color:rgb(44, 62, 80);">实现响应式</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#proxy-%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)<font style="color:rgb(44, 62, 80);">Proxy 基本使用</font>
```javascript
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

### [](https://www.123fe.net/docs/base/high-frequency.html#vue3%E7%94%A8proxy-%E5%AE%9E%E7%8E%B0%E5%93%8D%E5%BA%94%E5%BC%8F)<font style="color:rgb(44, 62, 80);">vue3用Proxy 实现响应式</font>
+ <font style="color:rgb(44, 62, 80);">深度监听，性能更好（获取到哪一层才触发响应式</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);">，不是一次性递归）</font>
+ <font style="color:rgb(44, 62, 80);">可监听</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">新增/删除</font><font style="color:rgb(44, 62, 80);">属性</font>
+ <font style="color:rgb(44, 62, 80);">可监听数组变化</font>

```javascript
// 创建响应式
function reactive(target = {}) {
  if (typeof target !== 'object' || target == null) {
    // 不是对象或数组，则返回
    return target
  }

  // 代理配置
  const proxyConf = {
    get(target, key, receiver) {
      // 只处理本身（非原型的）属性
      const ownKeys = Reflect.ownKeys(target)
      if (ownKeys.includes(key)) {
        console.log('get', key) // 监听
      }

      const result = Reflect.get(target, key, receiver)

      // 深度监听
      // 性能如何提升的？获取到哪一层才触发响应式get，不是一次性递归
      return reactive(result)
    },
    set(target, key, val, receiver) {
      // 重复的数据，不处理
      if (val === target[key]) {
        return true
      }

      const ownKeys = Reflect.ownKeys(target)
      if (ownKeys.includes(key)) {
        console.log('已有的 key', key)
      } else {
        console.log('新增的 key', key)
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
  }

  // 生成代理对象
  const observed = new Proxy(target, proxyConf)
  return observed
}

// 测试数据
const data = {
  name: 'zhangsan',
  age: 20,
  info: {
    city: 'shenshen',
    a: {
      b: {
        c: {
          d: {
            e: 100
          }
        }
      }
    }
  }
}

const proxyData = reactive(data)
```

### [](https://www.123fe.net/docs/base/high-frequency.html#v-model%E5%8F%82%E6%95%B0%E7%9A%84%E7%94%A8%E6%B3%95)<font style="color:rgb(44, 62, 80);">v-model参数的用法</font>
```vue
<!-- UserInfo组件 -->
<template>
    <input :value="name" @input="$emit('update:name', $event.target.value)"/>
    <input :value="age" @input="$emit('update:age', $event.target.value)"/>
</template>

<script>
export default {
    name: 'UserInfo',
    props: {
        name: String,
        age: String
    }
}
</script>
```

```vue
<!-- 使用 -->
<user-info
    v-model:name="name"
    v-model:age="age"
></user-info>
```

### [](https://www.123fe.net/docs/base/high-frequency.html#watch%E5%92%8Cwatcheffect%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">watch和watchEffect的区别</font>
+ <font style="color:rgb(44, 62, 80);">两者都可以监听</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">data</font><font style="color:rgb(44, 62, 80);">属性变化</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watch</font><font style="color:rgb(44, 62, 80);">需要明确监听哪个属性</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">watchEffect</font><font style="color:rgb(44, 62, 80);">会根据其中的属性，自动监听其变化</font>

```vue
<template>
    <p>watch vs watchEffect</p>
    <p>{{numberRef}}</p>
    <p>{{name}} {{age}}</p>
</template>

<script>
import { reactive, ref, toRefs, watch, watchEffect } from 'vue'

export default {
    name: 'Watch',
    setup() {
        const numberRef = ref(100)
        const state = reactive({
            name: 'test',
            age: 20
        })

        watchEffect(() => {
            // 初始化时，一定会执行一次（收集要监听的数据）
            console.log('hello watchEffect')
        })
        watchEffect(() => {
            console.log('state.name', state.name)
        })
        watchEffect(() => {
            console.log('state.age', state.age)
        })
        watchEffect(() => {
            console.log('state.age', state.age)
            console.log('state.name', state.name)
        })
        setTimeout(() => {
            state.age = 25
        }, 1500)
        setTimeout(() => {
            state.name = 'testA'
        }, 3000)
        

        // ref直接写
        // watch(numberRef, (newNumber, oldNumber) => {
        //     console.log('ref watch', newNumber, oldNumber)
        // }
        // // , {
        // //     immediate: true // 初始化之前就监听，可选
        // // }
        // )

        // setTimeout(() => {
        //     numberRef.value = 200
        // }, 1500)

        // watch(
        //     // 第一个参数，确定要监听哪个属性
        //     () => state.age,

        //     // 第二个参数，回调函数
        //     (newAge, oldAge) => {
        //         console.log('state watch', newAge, oldAge)
        //     },

        //     // 第三个参数，配置项
        //     {
        //         immediate: true, // 初始化之前就监听，可选
        //         // deep: true // 深度监听
        //     }
        // )

        // setTimeout(() => {
        //     state.age = 25
        // }, 1500)
        // setTimeout(() => {
        //     state.name = 'PoetryA'
        // }, 3000)

        return {
            numberRef,
            ...toRefs(state)
        }
    }
}
</script>
```

### [](https://www.123fe.net/docs/base/high-frequency.html#setup%E4%B8%AD%E5%A6%82%E4%BD%95%E8%8E%B7%E5%8F%96%E7%BB%84%E4%BB%B6%E5%AE%9E%E4%BE%8B)<font style="color:rgb(44, 62, 80);">setup中如何获取组件实例</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">和其他</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">composition API</font><font style="color:rgb(44, 62, 80);">中没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font>
+ <font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getCurrentInstance</font><font style="color:rgb(44, 62, 80);">获取当前实例</font>
+ <font style="color:rgb(44, 62, 80);">若使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">options API</font><font style="color:rgb(44, 62, 80);">可以照常使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font>

```javascript
import { onMounted, getCurrentInstance } from 'vue'

export default {
  name: 'GetInstance',
  data() {
    return {
      x: 1,
      y: 2
    }
  }, 
  setup() { // setup是created beforeCreate 合集 组件还没正式初始化
    console.log('this1', this) // undefined

    onMounted(() => {
      console.log('this in onMounted', this) // undefined
      console.log('x', instance.data.x) // 1  onMounted中组件已经初始化了
    })

    const instance = getCurrentInstance()
    console.log('instance', instance)
  },
  mounted() {
    console.log('this2', this)
    console.log('y', this.y)
  }
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#vue3%E4%B8%BA%E4%BD%95%E6%AF%94vue2%E5%BF%AB)<font style="color:rgb(44, 62, 80);">Vue3为何比Vue2快</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">proxy</font><font style="color:rgb(44, 62, 80);">响应式：深度监听，性能更好（获取到哪一层才触发响应式</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);">，不是一次性递归）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PatchFlag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">动态节点做标志</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HoistStatic</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将静态节点的定义，提升到父作用域，缓存起来。多个相邻的静态节点，会被合并起来</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CacheHandler</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">事件缓存</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SSR</font><font style="color:rgb(44, 62, 80);">优化: 静态节点不走</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font><font style="color:rgb(44, 62, 80);">逻辑，直接输出字符串，动态节点才走</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Tree-shaking</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">根据模板的内容动态</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">import</font><font style="color:rgb(44, 62, 80);">不同的内容，不需要就不</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">import</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#%E4%BB%80%E4%B9%88%E6%98%AFpatchflag)<font style="color:rgb(44, 62, 80);">什么是PatchFlag</font>
+ <font style="color:rgb(44, 62, 80);">模板编译时，动态节点做标记</font>
+ <font style="color:rgb(44, 62, 80);">标记，分为不同类型，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Text</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PROPS</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CLASS</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">diff</font><font style="color:rgb(44, 62, 80);">算法时，可区分静态节点，以及不同类型的动态节点</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718784006356-e6643888-d412-4928-a950-de539570f34c.png)

```vue
<!-- https://vue-next-template-explorer.netlify.app 中打开查看编译结果 -->

<div>
  <span>hello vue3</span>
  <span>{{msg}}</span>
  <span :class="name">poetry</span>
  <span :id="name">poetry</span>
  <span :id="name">{{msg}}</span>
  <span :id="name" :msg="msg">poetry</span>
</div>
```

```javascript
// 编译后结果

import { createElementVNode as _createElementVNode, toDisplayString as _toDisplayString, normalizeClass as _normalizeClass, openBlock as _openBlock, createElementBlock as _createElementBlock } from "vue"

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createElementBlock("div", null, [
    _createElementVNode("span", null, "hello vue3"),
    _createElementVNode("span", null, _toDisplayString(_ctx.msg), 1 /* TEXT */), // 文本标记1
    _createElementVNode("span", {
      class: _normalizeClass(_ctx.name)
    }, "poetry", 2 /* CLASS */), // class标记2
    _createElementVNode("span", { id: _ctx.name }, "poetry", 8 /* PROPS */, ["id"]), // 属性props标记8
    _createElementVNode("span", { id: _ctx.name }, _toDisplayString(_ctx.msg), 9 /* TEXT, PROPS */, ["id"]), // 文本和属性组合标记9
    _createElementVNode("span", {
      id: _ctx.name,
      msg: _ctx.msg
    }, "poetry", 8 /* PROPS */, ["id", "msg"]) // 属性组合标记
  ]))
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E4%BB%80%E4%B9%88%E6%98%AFhoiststatic%E5%92%8Ccachehandler)<font style="color:rgb(44, 62, 80);">什么是HoistStatic和CacheHandler</font>
**<font style="color:rgb(44, 62, 80);">HoistStatic</font>**

+ <font style="color:rgb(44, 62, 80);">将静态节点的定义，提升到父作用域，缓存起来</font>
+ <font style="color:rgb(44, 62, 80);">多个相邻的静态节点，会被合并起来</font>
+ <font style="color:rgb(44, 62, 80);">典型的拿空间换时间的优化策略</font>

```vue
<!-- https://vue-next-template-explorer.netlify.app 中打开查看编译结果：options开启hoistStatic -->
<div>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>{{msg}}</span>
</div>
```

```javascript
// 编译结果

import { createElementVNode as _createElementVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createElementBlock as _createElementBlock } from "vue"

// 之后函数怎么执行，这些变量都不会被重复定义一遍
const _hoisted_1 = /*__PURE__*/_createElementVNode("span", null, "hello vue3", -1 /* HOISTED */)
const _hoisted_2 = /*__PURE__*/_createElementVNode("span", null, "hello vue3", -1 /* HOISTED */)
const _hoisted_3 = /*__PURE__*/_createElementVNode("span", null, "hello vue3", -1 /* HOISTED */)

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createElementBlock("div", null, [
    _hoisted_1,
    _hoisted_2,
    _hoisted_3,
    _createElementVNode("span", null, _toDisplayString(_ctx.msg), 1 /* TEXT */)
  ]))
}
```

```vue
<!-- https://vue-next-template-explorer.netlify.app 中打开查看编译结果：options开启hoistStatic -->
<!-- 当相同的节点达到一定阈值后会被vue3合并起来 -->
<div>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>{{msg}}</span>
</div>
```

```javascript
// 编译之后

import { createElementVNode as _createElementVNode, toDisplayString as _toDisplayString, createStaticVNode as _createStaticVNode, openBlock as _openBlock, createElementBlock as _createElementBlock } from "vue"

// 多个相邻的静态节点，会被合并起来
const _hoisted_1 = /*__PURE__*/_createStaticVNode("<span>hello vue3</span><span>hello vue3</span><span>hello vue3</span><span>hello vue3</span><span>hello vue3</span><span>hello vue3</span><span>hello vue3</span><span>hello vue3</span><span>hello vue3</span><span>hello vue3</span>", 10)

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createElementBlock("div", null, [
    _hoisted_1,
    _createElementVNode("span", null, _toDisplayString(_ctx.msg), 1 /* TEXT */)
  ]))
}
```

**<font style="color:rgb(44, 62, 80);">CacheHandler</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">缓存事件</font>

```vue
<!-- https://vue-next-template-explorer.netlify.app 中打开查看编译结果：options开启cacheHandler -->
<div>
  <span @click="clickHandler">hello vue3</span>
</div>
```

```javascript
// 编译之后

import { createElementVNode as _createElementVNode, openBlock as _openBlock, createElementBlock as _createElementBlock } from "vue"

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createElementBlock("div", null, [
    _createElementVNode("span", {
      onClick: _cache[0] || (_cache[0] = (...args) => (_ctx.clickHandler && _ctx.clickHandler(...args)))
    }, "hello vue3")
  ]))
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#ssr%E5%92%8Ctree-shaking%E7%9A%84%E4%BC%98%E5%8C%96)<font style="color:rgb(44, 62, 80);">SSR和Tree-shaking的优化</font>
**<font style="color:rgb(44, 62, 80);">SSR优化</font>**

+ <font style="color:rgb(44, 62, 80);">静态节点直接输出，绕过了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vdom</font>
+ <font style="color:rgb(44, 62, 80);">动态节点，还是需要动态渲染</font>

```vue
<!-- https://vue-next-template-explorer.netlify.app 中打开查看编译结果：options开启ssr -->
<div>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>hello vue3</span>
  <span>{{msgs}}</span>
</div>
```

```javascript
// 编译之后

import { mergeProps as _mergeProps } from "vue"
import { ssrRenderAttrs as _ssrRenderAttrs, ssrInterpolate as _ssrInterpolate } from "vue/server-renderer"

export function ssrRender(_ctx, _push, _parent, _attrs, $props, $setup, $data, $options) {
  const _cssVars = { style: { color: _ctx.color }}
  _push(`<div${
    _ssrRenderAttrs(_mergeProps(_attrs, _cssVars))
  }><span>hello vue3</span><span>hello vue3</span><span>hello vue3</span><span>${ // 静态节点直接输出
    _ssrInterpolate(_ctx.msgs)
  }</span></div>`)
}
```

**<font style="color:rgb(44, 62, 80);">Tree Shaking优化</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">编译时，根据不同的情况，引入不同的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">API</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，不会全部引用</font>

```vue
<!-- https://vue-next-template-explorer.netlify.app 中打开查看编译结果 -->
<div>
  <span v-if="msg">hello vue3</span>
  <input v-model="msg" />
</div>
```

```javascript
// 编译之后

// 模板编译会根据模板写法 指令 插值以及用了特别的功能去动态的import相应的接口，需要什么就import什么，这就是tree shaking
import { openBlock as _openBlock, createElementBlock as _createElementBlock, createCommentVNode as _createCommentVNode, vModelText as _vModelText, createElementVNode as _createElementVNode, withDirectives as _withDirectives } from "vue"

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createElementBlock("div", null, [
    (_ctx.msg)
    ? (_openBlock(), _createElementBlock("span", { key: 0 }, "hello vue3"))
    : _createCommentVNode("v-if", true),
    _withDirectives(_createElementVNode("input", {
      "onUpdate:modelValue": $event => ((_ctx.msg) = $event)
    }, null, 8 /* PROPS */, ["onUpdate:modelValue"]), [
      [_vModelText, _ctx.msg]
    ])
  ]))
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#vite-%E4%B8%BA%E4%BB%80%E4%B9%88%E5%90%AF%E5%8A%A8%E9%9D%9E%E5%B8%B8%E5%BF%AB)<font style="color:rgb(44, 62, 80);">Vite 为什么启动非常快</font>
+ <font style="color:rgb(44, 62, 80);">开发环境使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Es6 Module</font><font style="color:rgb(44, 62, 80);">，无需打包，非常快</font>
+ <font style="color:rgb(44, 62, 80);">生产环境使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rollup</font><font style="color:rgb(44, 62, 80);">，并不会快很多</font>

**<font style="color:rgb(44, 62, 80);">ES Module 在浏览器中的应用</font>**

```html
<p>基本演示</p>
<script type="module">
    import add from './src/add.js'

    const res = add(1, 2)
    console.log('add res', res)
</script>
<script type="module">
    import { add, multi } from './src/math.js'
    console.log('add res', add(10, 20))
    console.log('multi res', multi(10, 20))
</script>
```

```html
<p>外链引用</p>
<script type="module" src="./src/index.js"></script>
```

```html
<p>远程引用</p>
<script type="module">
    import { createStore } from 'https://unpkg.com/redux@latest/es/redux.mjs' // es module规范mjs
    console.log('createStore', createStore)
</script>
```

```html
<p>动态引入</p>
<button id="btn1">load1</button>
<button id="btn2">load2</button>

<script type="module">
    document.getElementById('btn1').addEventListener('click', async () => {
        const add = await import('./src/add.js')
        const res = add.default(1, 2)
        console.log('add res', res)
    })
    document.getElementById('btn2').addEventListener('click', async () => {
        const { add, multi } = await import('./src/math.js')
        console.log('add res', add(10, 20))
        console.log('multi res', multi(10, 20))
    })
</script>
```

### [](https://www.123fe.net/docs/base/high-frequency.html#composition-api-%E5%92%8C-react-hooks-%E7%9A%84%E5%AF%B9%E6%AF%94)<font style="color:rgb(44, 62, 80);">Composition API 和 React Hooks 的对比</font>
+ <font style="color:rgb(44, 62, 80);">前者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">(相当于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">created</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">beforeCreate</font><font style="color:rgb(44, 62, 80);">的合集)只会调用一次，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Hooks</font><font style="color:rgb(44, 62, 80);">函数在渲染过程中会被多次调用</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Composition API</font><font style="color:rgb(44, 62, 80);">无需使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useMemo</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useCallback</font><font style="color:rgb(44, 62, 80);">，因为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">只会调用一次，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setup</font><font style="color:rgb(44, 62, 80);">闭包中缓存了变量</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Composition API</font><font style="color:rgb(44, 62, 80);">无需顾虑调用顺序，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React Hooks</font><font style="color:rgb(44, 62, 80);">需要保证</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hooks</font><font style="color:rgb(44, 62, 80);">的顺序一致（比如不能放在循环、判断里面）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Composition API</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ref</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reactive</font><font style="color:rgb(44, 62, 80);">比</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useState</font><font style="color:rgb(44, 62, 80);">难理解</font>

