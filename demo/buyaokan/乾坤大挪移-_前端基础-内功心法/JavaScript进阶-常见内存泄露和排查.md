## 前言
这一章节给大家介绍的知识点相对比较简单, 但是却是非常重要的. 而且也是在面试过程中经常会被问到的一部分内容.

通过此次阅读你可以学习到:

+ 4种常见的内存泄露
+ 内存泄露的识别方法

## [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#_4%E7%A7%8D%E5%B8%B8%E8%A7%81%E7%9A%84%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2)4种常见的内存泄露
其实在实际开发中, 我们很容易不经意的就写出内存泄露的代码, 比如以下几种情况可能都是你遇到过的.

### [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#%E4%B8%80%E3%80%81%E6%84%8F%E5%A4%96%E7%9A%84%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F)一、意外的全局变量
#### [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#%E6%9C%AA%E5%A3%B0%E6%98%8E%E7%9A%84%E5%8F%98%E9%87%8F)未声明的变量
当我们在一个函数中给一个变量赋值但是却没有声明它时:

```javascript
function fn () {
    a = "Actually, I'm a global variable"
}
```

此时变量<font style="background-color:rgb(249, 242, 244);">a</font>相当于是<font style="background-color:rgb(249, 242, 244);">window</font>对象下的一个变量:

```javascript
function fn () {
    window.a = "Actually, I'm a global variable"
}
```

而之前我们已经说了全局变量是很难被垃圾回收器回收的, 所以要是有这种意外的全局变量应该要避免.

#### [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#%E4%BD%BF%E7%94%A8this%E5%88%9B%E5%BB%BA%E7%9A%84%E5%8F%98%E9%87%8F)使用<font style="background-color:rgb(249, 242, 244);">this</font>创建的变量
还有一种情况是这样的:

```javascript
function fn () {
    this.a = "Actually, I'm a global variable"
}
fn();
```

我们知道, 这里<font style="background-color:rgb(249, 242, 244);">this</font>的指向是<font style="background-color:rgb(249, 242, 244);">window</font>, 因此此时创建的<font style="background-color:rgb(249, 242, 244);">a</font>变量也会被挂载到<font style="background-color:rgb(249, 242, 244);">window</font>对象下.

避免此情况的解决办法是<font style="background-color:rgb(249, 242, 244);">在 JavaScript 文件头部或者函数的顶部加上 'use strict'</font>, 开启严格模式, 使得<font style="background-color:rgb(249, 242, 244);">this</font>的指向为<font style="background-color:rgb(249, 242, 244);">undefined</font>, 这样就可以避免了.

<font style="background-color:rgb(255, 249, 249);">当然如果你必须使用全局变量存储大量数据时，确保用完以后把它设置为 null 或者重新定义。</font>

### [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#%E4%BA%8C%E3%80%81%E8%A2%AB%E9%81%97%E5%BF%98%E7%9A%84%E8%AE%A1%E6%97%B6%E5%99%A8%E6%88%96%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0)二、被遗忘的计时器或回调函数
#### [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#%E5%AE%9A%E6%97%B6%E5%99%A8%E5%BC%95%E8%B5%B7)定时器引起
当我们在代码中使用定时器也有可能会造成内存泄露:

```javascript
var serverData = loadData()
setInterval(function() {
	var renderer = document.getElementById('renderer')
	if(renderer) {
		renderer.innerHTML = JSON.stringify(serverData)
	}
}, 5000)
```

上面的例子🌰中, 节点<font style="background-color:rgb(249, 242, 244);">renderer</font>引用了<font style="background-color:rgb(249, 242, 244);">serverData</font>.在节点<font style="background-color:rgb(249, 242, 244);">renderer</font>或者数据不再需要时，定时器依旧指向这些数据。所以哪怕当<font style="background-color:rgb(249, 242, 244);">renderer</font>节点被移除后，interval 仍旧存活并且垃圾回收器没办法回收，它的依赖也没办法被回收，除非终止定时器。

#### [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#%E5%AF%B9%E8%B1%A1%E8%A7%82%E5%AF%9F%E8%80%85)对象观察者
还有一个就是关于观察者模式的案例:

```javascript
var btn = document.getElementById('btn');
function onClick (element) {
    element.innerHTMl = "I'm innerHTML"
}
btn.addEventListener('click', onClick);
```

对于上面观察者的例子，一旦它们不再需要（或者关联的对象变成不可达），明确地移除它们非常重要。老的 IE 6 是无法处理循环引用的。因为老版本的 IE 是无法检测 DOM 节点与 JavaScript 代码之间的循环引用，会导致内存泄漏。

但是，现代的浏览器（包括 IE 和 Microsoft Edge）使用了更先进的垃圾回收算法（标记清除），已经可以正确检测和处理循环引用了。即回收节点内存时，不必非要调用 removeEventListener 了。

### [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#%E4%B8%89%E3%80%81%E8%84%B1%E7%A6%BBdom%E7%9A%84%E5%BC%95%E7%94%A8)三、脱离DOM的引用
这种造成内存泄露的原因简单来说就是:

如果把DOM 存成字典（JSON 键值对）或者数组，此时，同样的 DOM 元素存在两个引用：一个在 DOM 树中，另一个在字典中。那么将来需要把两个引用都清除。

比如下面这个例子:

```javascript
// 在对象中引用DOM
var elements = {
    btn: document.getElementById('btn')
}

function doSomeThing () {
    elements.btn.click();
}

function removeBtn () {
    // 将body中的btn移除, 也就是移除 DOM树中的btn
    document.body.removeChild(document.getElementById('button'));
    // 但是此时全局变量elements还是保留了对btn的引用, btn还是存在于内存中,不能被GC回收
}
```

上面👆这种情况, 可以手动将引用给清除: <font style="background-color:rgb(249, 242, 244);">elements.btn = null</font>.

### [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#%E5%9B%9B%E3%80%81%E9%97%AD%E5%8C%85)四、闭包
还有一种情况就是我们之前提到过的闭包. 也就是局部变量销毁时, 闭包的这种情况.

首先让我们明确一点: **闭包的关键就是匿名函数能够访问父级作用域中的变量**.

来看一个简单的例子🌰:

```javascript
function fn () {
    var a = "I'm a";
    return function () {
        console.log(a);
    };
}
```

因为变量<font style="background-color:rgb(249, 242, 244);">a</font>被<font style="background-color:rgb(249, 242, 244);">fn()</font>函数内的匿名函数所引用, 因此这种变量是不会被回收的.

那就有人问了, 即使这样会造成什么不妥吗? 在上面👆这个案例中当然看不出有什么. 若是将情况变得复杂一些呢?

```javascript
var globalVar = null; // 全局变量
var fn = function () {
    var originVal = globalVar; // 局部变量
    var unused = function () { // 未使用的函数
        if (originVal) {
            console.log('call')
        }
    }
    globalVar = {
        longStr: new Array(1000000).join('*'),
        someThing: function () {
            console.log('someThing')
        }
    }
}
setInterval(fn, 100);
```

先请你花上一分钟看看上面的案例, 你会发现:

1. 每次调用<font style="background-color:rgb(249, 242, 244);">fn</font>函数的时候都会产生一个新的对象<font style="background-color:rgb(249, 242, 244);">originVal</font>;
2. 变量<font style="background-color:rgb(249, 242, 244);">unused</font>是一个引用了<font style="background-color:rgb(249, 242, 244);">originVal</font>的闭包;
3. <font style="background-color:rgb(249, 242, 244);">unused</font>虽然未被使用, 但是它引用的<font style="background-color:rgb(249, 242, 244);">originVal</font>迫使它留在内存中, 并不会被回收.

解决办法是: 可以在<font style="background-color:rgb(249, 242, 244);">fn</font>的最底部, 将<font style="background-color:rgb(249, 242, 244);">originVal</font>设置成<font style="background-color:rgb(249, 242, 244);">null</font>.

## [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E7%9A%84%E8%AF%86%E5%88%AB%E6%96%B9%E6%B3%95)内存泄露的识别方法
上面👆介绍了这么多种可能会造成内存泄露的情况, 那么有没有什么实际的办法让我们看到内存泄露的表现呢?

当然是有的. 现在常用的是以下2种方式:

+ <font style="background-color:rgb(249, 242, 244);">Chrome</font>浏览器的控制台<font style="background-color:rgb(249, 242, 244);">Performance</font>或<font style="background-color:rgb(249, 242, 244);">Memory</font>
+ <font style="background-color:rgb(249, 242, 244);">Node</font>提供的<font style="background-color:rgb(249, 242, 244);">process.memoryUsage</font>方法

### [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#chrome%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84%E6%8E%A7%E5%88%B6%E5%8F%B0performance%E6%88%96memory)<font style="background-color:rgb(249, 242, 244);">Chrome</font>浏览器的控制台<font style="background-color:rgb(249, 242, 244);">Performance</font>或<font style="background-color:rgb(249, 242, 244);">Memory</font>
Chrome 浏览器查看内存占用，按照以下步骤操作。

1. 在网页上右键, 点击“检查”打开控制台(<font style="background-color:rgb(249, 242, 244);">Mac</font>快捷键<font style="background-color:rgb(249, 242, 244);">option+command+i</font>);
2. 选择<font style="background-color:rgb(249, 242, 244);">Performance</font>面板(很多教材中用的是<font style="background-color:rgb(249, 242, 244);">Timeline</font>面板, 不知道是不是版本的原因);
3. 勾选<font style="background-color:rgb(249, 242, 244);">Memory</font>, 然后点击左上角的小黑点<font style="background-color:rgb(249, 242, 244);">Record</font>开始录制;
4. 点击弹窗中的<font style="background-color:rgb(249, 242, 244);">Stop</font>结束录制, 面板上就会显示这段时间的内存占用情况。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959500447-c5a10716-f2e7-46b6-a198-907c2e20a872.png) ![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959500473-89e7d79e-a42d-4be8-8e5f-a9c994b7e90c.png)

如果内存的使用情况一直在做增量, 那么就是内存泄露了:

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959500463-659d6e4b-3391-4a48-bee6-f2ea8cb3940a.png)

或者可以参考这个[《记录一次定时器及闭包问题造成的内存泄漏》](https://juejin.im/post/5d80854d5188253264365f11)中的方法进行检查.

### [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#node%E6%8F%90%E4%BE%9B%E7%9A%84process-memoryusage%E6%96%B9%E6%B3%95)<font style="background-color:rgb(249, 242, 244);">Node</font>提供的<font style="background-color:rgb(249, 242, 244);">process.memoryUsage</font>方法
另一个就是<font style="background-color:rgb(249, 242, 244);">Node</font>提供的<font style="background-color:rgb(249, 242, 244);">process.memoryUsage</font>方法, 这一块我用的不是很多, 这里就贴上教材:

```javascript
console.log(process.memoryUsage());
// { rss: 27709440,
//  heapTotal: 5685248,
//  heapUsed: 3449392,
//  external: 8772 }
```

process.memoryUsage返回一个对象，包含了 Node 进程的内存占用信息。该对象包含四个字段，单位是字节，含义如下:

```plain
rss（resident set size）：所有内存占用，包括指令区和堆栈。
heapTotal："堆"占用的内存，包括用到的和没用到的。
heapUsed：用到的堆的部分。
external： V8 引擎内部的 C++ 对象占用的内存。
```

判断内存泄露, 是看<font style="background-color:rgb(249, 242, 244);">heapUsed</font>字段.

## [](https://www.123fe.net/blog-docs/javaScript2/16-JavaScript%E8%BF%9B%E9%98%B6_%E5%B8%B8%E8%A7%81%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%8F%8A%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D.html#%E6%80%BB%E7%BB%93)总结
总的来说, 常见的内存泄露包括:

+ 意外的全局变量
+ 被遗忘的定时器或回调函数
+ 脱离DOM的引用
+ 闭包中重复创建的变量

如何避免内存泄露:

+ 注意程序逻辑，避免“死循环”之类的
+ 减少不必要的全局变量，或者生命周期较长的对象，及时对无用的数据进行垃圾回收
+ 避免创建过多的对象 原则：不用了的东西要及时归还

