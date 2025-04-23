<br/>color1
<font style="color:rgb(85, 85, 85);">节流函数原理:指频繁触发事件时，只会在指定的时间段内执行事件回调，即触发事件间隔大于等于指定的时间才会执行回调函数。总结起来就是：</font>**<font style="color:rgb(85, 85, 85);">事件，按照一段时间的间隔来进行触发</font>**<font style="color:rgb(85, 85, 85);">。</font>

<br/>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718787233804-d904aaf6-5ee4-439e-9741-bba813a76e27.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718787233907-e95c32e1-6878-4d7a-adcf-50419e68a16d.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">像dom的拖拽，如果用消抖的话，就会出现卡顿的感觉，因为只在停止的时候执行了一次，这个时候就应该用节流，在一定时间内多次执行，会流畅很多</font>

**<font style="color:rgb(44, 62, 80);">手写简版</font>**

<font style="color:rgb(44, 62, 80);">使用时间戳的节流函数会在第一次触发事件时立即执行，以后每过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">wait</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">秒之后才执行一次，并且最后一次触发事件不会被执行</font>

**<font style="color:rgb(44, 62, 80);">时间戳方式：</font>**

```javascript
// func是用户传入需要防抖的函数
// wait是等待时间
const throttle = (func, wait = 50) => {
  // 上一次执行该函数的时间
  let lastTime = 0
  return function(...args) {
    // 当前时间
    let now = +new Date()
    // 将当前时间和上一次执行函数时间对比
    // 如果差值大于设置的等待时间就执行函数
    if (now - lastTime > wait) {
      lastTime = now
      func.apply(this, args)
    }
  }
}

setInterval(
  throttle(() => {
    console.log(1)
  }, 500),
  1
)
```

**<font style="color:rgb(44, 62, 80);">定时器方式：</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">使用定时器的节流函数在第一次触发时不会执行，而是在 delay 秒之后才执行，当最后一次停止触发后，还会再执行一次函数</font>

```javascript
function throttle(func, delay){
  var timer = 0;
  return function(){
    var context = this;
    var args = arguments;
    if(timer) return // 当前有任务了，直接返回
    timer = setTimeout(function(){
      func.apply(context, args);
      timer = 0;
    },delay);
  }
}
```

**<font style="color:rgb(44, 62, 80);">适用场景：</font>**

+ <font style="color:rgb(44, 62, 80);">拖拽场景：固定时间内只执行一次，防止超高频次触发位置变动。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素的拖拽功能实现（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mousemove</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">缩放场景：监控浏览器</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resize</font>
+ <font style="color:rgb(44, 62, 80);">滚动场景：监听滚动</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scroll</font><font style="color:rgb(44, 62, 80);">事件判断是否到页面底部自动加载更多</font>
+ <font style="color:rgb(44, 62, 80);">动画场景：避免短时间内多次触发动画引起性能问题</font>

**<font style="color:rgb(44, 62, 80);">总结</font>**

+ **<font style="color:rgb(44, 62, 80);">函数防抖</font>**<font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">限制执行次数，多次密集的触发只执行一次</font>
    - <font style="color:rgb(44, 62, 80);">将几次操作合并为一次操作进行。原理是维护一个计时器，规定在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">delay</font><font style="color:rgb(44, 62, 80);">时间后触发函数，但是在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">delay</font><font style="color:rgb(44, 62, 80);">时间内再次触发的话，就会取消之前的计时器而重新设置。这样一来，只有最后一次操作能被触发。</font>
+ **<font style="color:rgb(44, 62, 80);">函数节流</font>**<font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">限制执行的频率，按照一定的时间间隔有节奏的执行</font>
    - <font style="color:rgb(44, 62, 80);">使得一定时间内只触发一次函数。原理是通过判断是否到达一定时间来触发函数。</font>

