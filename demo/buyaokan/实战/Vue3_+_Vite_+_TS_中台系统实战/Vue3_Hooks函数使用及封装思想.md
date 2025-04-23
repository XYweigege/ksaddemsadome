### 一. 什么是hooks函数？
<br/>color1
专业解释：Vue 3中的Hooks函数是一种用于在组件中共享可复用逻辑的方式。

大白话：将单独功能的js代码抽离出来， 加工成公共函数，从而达到逻辑复用。

<br/>

### <font style="color:rgb(79, 79, 79);">二、如何封装一个hooks函数</font>
<font style="color:rgb(77, 77, 77);">例如实现一个点击按钮获取body的宽度和高度</font>

```html
<script setup lang="ts">
import { reactive } from "vue";
 
const data = reactive({
  width: 0,
  height:0
})
 
const getViewportSize = () => {
  data.width = document.body.clientWidth;
  data.height = document.body.clientHeight;
}
</script>
 
<template>
  <button @click="getViewportSize" > 获取窗口大小 </button>
    <div>
      <div>宽度 ： {{data.width}}</div>
      <div>宽度 ： {{data.height}}</div>
    </div>
</template>
```

**<font style="color:rgb(77, 77, 77);">离逻辑，封装成hooks函数</font>**

**<font style="color:rgb(77, 77, 77);">hooks封装规范：</font>**

**<font style="color:rgb(77, 77, 77);">1. 新建hooks文件；</font>**

**<font style="color:rgb(77, 77, 77);">2. 函数名前缀加上use；</font>**

**<font style="color:rgb(77, 77, 77);">3. 合理利用Vue提供的响应式函数及生命周期；</font>**

**<font style="color:rgb(77, 77, 77);">4. 暴露出 变量 和 方法 提供外部需要时使用；</font>**

<font style="color:rgb(77, 77, 77);">src/hooks/index.js</font>

```javascript
import { reactive } from "vue";

export function useVwSize() {

  const data = reactive({
    width: 0,
    height:0
  })

  const getViewportSize = () => {
    data.width = document.body.clientWidth;
    data.height = document.body.clientHeight;
  }

  return {
    data,getViewportSize
  }
}
```

<font style="color:rgb(77, 77, 77);">index.</font><font style="color:rgb(78, 161, 219) !important;">vue</font><font style="color:rgb(77, 77, 77);"> 使用hooks</font>

```javascript
<script setup lang="ts">
import { useVwSize } from "@/hooks";
const { data, getViewportSize } = useVwSize();
</script>
 
<template>
  <button @click="getViewportSize">获取窗口大小</button>
  <div>
    <div>宽度 ： {{ data.width }}</div>
    <div>宽度 ： {{ data.height }}</div>
  </div>
</template>
```

### <font style="color:rgb(79, 79, 79);">三、Hooks 常用 Demo</font>
#### <font style="color:rgb(79, 79, 79);">（1）</font>[<font style="color:rgb(79, 79, 79);">验证码</font>](https://so.csdn.net/so/search?q=%E9%AA%8C%E8%AF%81%E7%A0%81&spm=1001.2101.3001.7020)<font style="color:rgb(79, 79, 79);">倒计时</font>
```javascript
 
/**
 *  倒计时
 *  @param {Number} second 倒计时秒数
 *  @return {Number} count 倒计时秒数
 *  @return {Function} countDown 倒计时函数
 *  @example
 *  const { count, countDown } = useCountDown()
 *  countDown(60)
 * <div>{{ count }}</div>
 */
 
export function useCountDown() {
    const count = ref(0);
    const timer = ref(null);
    const countDown = (second= 60 , ck = () => {}) => {
      if (count.value === 0 && timer.value === null) {
        ck();
        count.value = second;
        timer.value = setInterval(() => {
          count.value--;
          if (count.value === 0) {
            clearInterval(timer.value);
            timer.value = null
          }
        }, 1000);
      }
    };
   
    onBeforeMount(() => {
      timer.value && clearInterval(timer.value);
    });
   
    return {
      count,
      countDown,
    };
  }
```

```vue
<script setup lang="ts">
  import {useCountDown} from "@/hooks";

  // 倒计时
  const { count, countDown } = useCountDown();

  const sendCode = () => {
    console.log("发送验证码");
  };

</script>

<button :disabled="count!=0" @click="countDown(5,sendCode)">倒计时 {{ count || '' }} </button>
```

#### <font style="color:rgb(79, 79, 79);">（2）防抖</font>
```javascript

/**
 * @params {Function} fn  需要防抖的函数 delay 防抖时间
 * @returns {Function} debounce 防抖函数
 * @example  
 * const { debounce } = useDebounce()
 * const fn = () => { console.log('防抖') }
 * const debounceFn = debounce(fn, 1000)
 * debounceFn()
 * 
 */

export function useDebounce() {
  const debounce = (fn, delay) => {
    let timer = null;
    return function () {
      if (timer)  clearTimeout(timer);
      timer = setTimeout(() => {
        fn.apply(this, arguments);
      }, delay);
    };
  };

  return { debounce };
}

```

```vue
<script setup lang="ts">
  import { useDebounce } from "@/hooks";
  // 防抖
  const { debounce } = useDebounce()

  const fn = () => {
    console.log('点击了哈');
  }
  const debounceClick =  debounce(fn,1000)
    <button @click="debounceClick">防抖点击</button>
</script>
```

#### <font style="color:rgb(79, 79, 79);">（3）节流</font>
```javascript
/**
 * @params {Function} fn  需要节流的函数 delay 节流时间
 * @returns {Function} throttle 节流函数
 * @example
 * const { throttle } = useThrottle()
 * const fn = () => { console.log('节流') }
 * const throttleFn = throttle(fn, 1000)
 * throttleFn()
 *  
 * 
 *  */
export function useThrottle() {
  const throttle = (fn, delay) => {
    let timer = null;
    return function () {
      if (!timer) {
        timer = setTimeout(() => {
          fn.apply(this, arguments);
          timer = null;
        }, delay);
      }
    };
  };

  return { throttle };
}
```

```vue
<script setup lang="ts">
  import { useThrottle} from "@/hooks";
  const fn = () => {
    console.log('点击了哈');
  }
  const { throttle } = useThrottle()
  const throttleClick =  throttle(fn,1000)
</script>

 <button @click="throttleClick">节流点击</button>
```

