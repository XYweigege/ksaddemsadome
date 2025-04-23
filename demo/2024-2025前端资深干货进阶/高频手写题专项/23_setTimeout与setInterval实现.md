### <font style="color:rgb(44, 62, 80);">setTimeout 模拟实现 setInterval</font>
<font style="color:rgb(44, 62, 80);">题目描述:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">用来实现循环定时调用 可能会存在一定的问题 能用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">解决吗</font>

<font style="color:rgb(44, 62, 80);">实现代码如下:</font>

```javascript
function mySetInterval(fn, t) {
  let timerId = null;
  function interval() {
    fn();
    timerId = setTimeout(interval, t); // 递归调用
  }
  timerId = setTimeout(interval, t); // 首次调用
  return {
    // 利用闭包的特性 保存timerId
    cancel:() => {
      clearTimeout(timerId)
    }
  }
}
```

```javascript
// 测试
var a = mySetInterval(()=>{
  console.log(111);
},1000)
var b = mySetInterval(() => {
  console.log(222)
}, 1000)

// 终止定时器
a.cancel()
b.cancel()
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为什么要用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">模拟实现</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">？</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的缺陷是什么？</font>

```javascript
setInterval(fn(), N);
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上面这句代码的意思其实是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fn()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">将会在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">N</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">秒之后被推入任务队列。在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">被推入任务队列时，如果在它前面有很多任务或者某个任务等待时间较长比如网络请求等，那么这个定时器的执行时间和我们预定它执行的时间可能并不一致</font>

```javascript
// 最常见的出现的就是，当我们需要使用 ajax 轮询服务器是否有新数据时，必定会有一些人会使用 setInterval，然而无论网络状况如何，它都会去一遍又一遍的发送请求，最后的间隔时间可能和原定的时间有很大的出入

// 做一个网络轮询，每一秒查询一次数据。
let startTime = new Date().getTime();
let count = 0;

setInterval(() => {
  let i = 0;
  while (i++ < 10000000); // 假设的网络延迟
  count++;
  console.log(
    "与原设定的间隔时差了：",
    new Date().getTime() - (startTime + count * 1000),
    "毫秒"
  );
}, 1000)

// 输出：
// 与原设定的间隔时差了： 567 毫秒
// 与原设定的间隔时差了： 552 毫秒
// 与原设定的间隔时差了： 563 毫秒
// 与原设定的间隔时差了： 554 毫秒(2次)
// 与原设定的间隔时差了： 564 毫秒
// 与原设定的间隔时差了： 602 毫秒
// 与原设定的间隔时差了： 573 毫秒
// 与原设定的间隔时差了： 633 毫秒
```

**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">再次强调</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，定时器指定的时间间隔，表示的是何时将定时器的代码添加到消息队列，而不是何时执行代码。所以真正何时执行代码的时间是不能保证的，取决于何时被主线程的事件循环取到，并执行。</font>

```plain
setInterval(function, N)
//即：每隔N秒把function事件推到消息队列中
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718788508546-fc83b8b1-dc1f-4cfa-a950-de6432aaab01.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上图可见，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">每隔</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100ms</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">往队列中添加一个事件；</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100ms</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">后，添加</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">T1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">定时器代码至队列中，主线程中还有任务在执行，所以等待，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">some event</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">执行结束后执行</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">T1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">定时器代码；又过了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100ms</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">T2</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">定时器被添加到队列中，主线程还在执行</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">T1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">代码，所以等待；又过了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100ms</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，理论上又要往队列里推一个定时器代码，但由于此时</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">T2</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">还在队列中，所以</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">T3</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不会被添加（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">T3</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">被跳过），结果就是此时被跳过；这里我们可以看到，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">T1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">定时器执行结束后马上执行了 T2 代码，所以并没有达到定时器的效果</font>

**<font style="color:rgb(44, 62, 80);">setInterval有两个缺点</font>**

+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);">时，某些间隔会被跳过</font>
+ <font style="color:rgb(44, 62, 80);">可能多个定时器会连续执行</font>

**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以这么理解</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">：每个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">产生的任务会直接</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">push</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">到任务队列中；而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在每次把任务</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">push</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">到任务队列前，都要进行一下判断(看上次的任务是否仍在队列中)。因而我们一般用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">模拟</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，来规避掉上面的缺点</font>

### [](https://www.123fe.net/docs/base/handwritten.html#_2-setinterval-%E6%A8%A1%E6%8B%9F%E5%AE%9E%E7%8E%B0-settimeout)<font style="color:rgb(44, 62, 80);">2 setInterval 模拟实现 setTimeout</font>
```javascript
const mySetTimeout = (fn, t) => {
  const timer = setInterval(() => {
    clearInterval(timer);
    fn();
  }, t);
};
```

```javascript
// 测试
// mySetTimeout(()=>{
//   console.log(1);
// },1000)
```

