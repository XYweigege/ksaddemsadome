<br/>color1
<font style="color:rgb(85, 85, 85);">防抖函数原理：</font>**<font style="color:rgb(85, 85, 85);">把触发非常频繁的事件合并成一次去执行</font>**<font style="color:rgb(85, 85, 85);"> 在指定时间内只执行一次回调函数，如果在指定的时间内又触发了该事件，则回调函数的执行时间会基于此刻重新开始计算</font>

<br/>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718787176742-416c6f10-0a23-42dd-8193-ab5652a94a50.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718787176899-6b48d89c-00a4-406d-bc89-2def4e551428.png)

<font style="color:rgb(44, 62, 80);">防抖动和节流本质是不一样的。</font>**<font style="color:rgb(44, 62, 80);">防抖动是将多次执行变为</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">最后一次执行</font>****<font style="color:rgb(44, 62, 80);">，节流是将多次执行变成</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">每隔一段时间执行</font>**

<br/>color1
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"></font><font style="color:rgb(85, 85, 85);">像百度搜索，就应该用防抖，当我连续不断输入时，不会发送请求；当我一段时间内不输入了，才会发送一次请求；如果小于这段时间继续输入的话，时间会重新计算，也不会发送请求。</font>

<br/>

**<font style="color:rgb(44, 62, 80);">手写简化版:</font>**

```javascript
// func是用户传入需要防抖的函数
// wait是等待时间
const debounce = (func, wait = 50) => {
  // 缓存一个定时器id
  let timer = 0
  // 这里返回的函数是每次用户实际调用的防抖函数
  // 如果已经设定过定时器了就清空上一次的定时器
  // 开始一个新的定时器，延迟执行用户传入的方法
  return function(...args) {
    if (timer) clearTimeout(timer)
    timer = setTimeout(() => {
      func.apply(this, args)
    }, wait)
  }
}
```

**<font style="color:rgb(44, 62, 80);">适用场景：</font>**

+ <font style="color:rgb(44, 62, 80);">文本输入的验证，连续输入文字后发送 AJAX 请求进行验证，验证一次就好</font>
+ <font style="color:rgb(44, 62, 80);">按钮提交场景：防止多次提交按钮，只执行最后提交的一次</font>
+ <font style="color:rgb(44, 62, 80);">服务端验证场景：表单验证需要服务端配合，只执行一段连续的输入事件的最后一次，还有搜索联想词功能类似</font>

