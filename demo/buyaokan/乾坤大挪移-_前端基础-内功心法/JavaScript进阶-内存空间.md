<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">中三种重要的数据结构, 如图:</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1718959431956-f3588bfb-0c39-4981-a4a8-2f4c9e2104fd.jpeg)

<font style="color:rgb(44, 62, 80);">(图片来源</font>[前端九五六-Javascript 内存空间管理](https://juejin.im/post/5d870e6651882517a0319aa8)<font style="color:rgb(44, 62, 80);">)</font>

### [](https://www.123fe.net/blog-docs/javaScript2/18-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4.html#%E6%A0%88%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)<font style="color:rgb(44, 62, 80);">栈数据结构</font>
<font style="color:rgb(44, 62, 80);">其实在</font>[《JavaScript执行上下文》](https://juejin.im/post/5db85b866fb9a0207d4cbf92)<font style="color:rgb(44, 62, 80);">中我就已经提到了</font>**<font style="color:rgb(238, 63, 77);">执行栈</font>**<font style="color:rgb(44, 62, 80);">, 让我们一起来回顾一下:</font>

**<font style="color:rgb(238, 63, 77);">栈的特点</font>**<font style="color:rgb(44, 62, 80);">: 后进先出（LIFO）的结构.</font>

<font style="color:rgb(44, 62, 80);">（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LIFO</font><font style="color:rgb(44, 62, 80);">:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">last-in, first-out</font><font style="color:rgb(44, 62, 80);">，类似于向乒乓球桶中放球，最先放入的球最后取出）</font>

<font style="color:rgb(44, 62, 80);">这里还是贴上一张网图方便大家理解的好:</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959431897-a74b3ef7-86b5-48c6-b10f-042d524ae626.png)

<font style="color:rgb(44, 62, 80);">栈中的数据就像是一个个乒乓球, 最先进去的最后出来.</font>

**<font style="color:rgb(238, 63, 77);">注</font>****<font style="color:rgb(238, 63, 77);">⚠️</font>**

<font style="color:rgb(44, 62, 80);">这里所说的</font>**<font style="color:rgb(238, 63, 77);">进栈</font>**<font style="color:rgb(44, 62, 80);">和</font>**<font style="color:rgb(238, 63, 77);">出栈</font>**<font style="color:rgb(44, 62, 80);">不是指赋值算进, 使用算出. 而是指</font>**<font style="color:rgb(238, 63, 77);">赋值算进, 被清理算出</font>**<font style="color:rgb(44, 62, 80);">, 而且位于同一函数作用域下的变量, 应该是在栈的同一层.</font>

<font style="color:rgb(44, 62, 80);">所谓的变量存储于栈内存中的栈，传统意义上说指的是由内存自动创建分配的空间，例如函数的参数值与局部变量，只是其操作方式类似于栈操作，所以叫栈内存。</font>

<font style="color:rgb(44, 62, 80);">比如函数调用其实就相当于栈的形式:</font>

**<font style="color:rgb(238, 63, 77);">例子</font>****<font style="color:rgb(238, 63, 77);">🌰</font>**<font style="color:rgb(44, 62, 80);">:</font>

```javascript
function fn1() {
	console.log(1)
  fn2()
}
function fn2() {
	console.log(2)
	fn3()
}
function fn3() {
	console.log(3)
}
fn1()
```

<font style="color:rgb(44, 62, 80);">如上, 声明的顺序是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1, 2, 3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">, 但是释放的顺序是为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">3, 2, 1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">.</font>

<font style="color:rgb(44, 62, 80);">这里释放按照这个顺序是因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">3</font><font style="color:rgb(44, 62, 80);">最先执行完, 所以最先被释放.</font>

### [](https://www.123fe.net/blog-docs/javaScript2/18-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4.html#%E5%A0%86%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)<font style="color:rgb(44, 62, 80);">堆数据结构</font>
<font style="color:rgb(44, 62, 80);">一种树状结构。好比</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">格式中的数据，你有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);">，我有对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">, 就立马返给你。</font>

<font style="color:rgb(44, 62, 80);">因为我们知道</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">格式的存储是无序的, 所以没有先后顺序, 所以它是一种绝对公平的数据结构.</font>

<font style="color:rgb(44, 62, 80);">如图所示:</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959431900-560e32cd-5c11-419c-84bf-626d815965e4.png)

### [](https://www.123fe.net/blog-docs/javaScript2/18-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4.html#%E9%98%9F%E5%88%97%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)<font style="color:rgb(44, 62, 80);">队列数据结构</font>
<font style="color:rgb(44, 62, 80);">队列数据结构不同于堆, 队列是一种**先进先出(FIFO)**的数据结构.</font>

<font style="color:rgb(44, 62, 80);">它也是**事件循环(Event Loop)**的基础结构.</font>

<font style="color:rgb(44, 62, 80);">如图所示:</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959432076-91e7c12c-3943-410e-9492-a05b475137ad.png)

<font style="color:rgb(44, 62, 80);">最先进入队列的任务最先出来, 类似于你排队买票, 排在前面的人先买.</font>

### [](https://www.123fe.net/blog-docs/javaScript2/18-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4.html#%E5%8F%98%E9%87%8F%E7%9A%84%E5%AD%98%E6%94%BE)<font style="color:rgb(44, 62, 80);">变量的存放</font>
<font style="color:rgb(44, 62, 80);">通过上面的介绍我们知道了, 内存中有堆了栈, 那么</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">变量具体是存放在哪里呢?</font>

+ <font style="color:rgb(44, 62, 80);">基本数据类型保存在</font>**<font style="color:rgb(238, 63, 77);">栈</font>**<font style="color:rgb(44, 62, 80);">内存中;</font>
+ <font style="color:rgb(44, 62, 80);">引用数据类型保存在</font>**<font style="color:rgb(238, 63, 77);">堆</font>**<font style="color:rgb(44, 62, 80);">内存中.</font>
1. <font style="color:rgb(44, 62, 80);">基本数据类型6种:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Undefined、Null、Boolean、Number、String、Symbol</font><font style="color:rgb(44, 62, 80);">, 由于他们在内存中分别占有固定大小的空间, 通过按值来访问.</font>
2. <font style="color:rgb(44, 62, 80);">引用数据类型: 也就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(44, 62, 80);">对象, 它的存储分为</font>**<font style="color:rgb(238, 63, 77);">访问地址</font>**<font style="color:rgb(44, 62, 80);">和</font>**<font style="color:rgb(238, 63, 77);">实际存放的地方</font>**<font style="color:rgb(44, 62, 80);">; 访问地址是存储在</font>**<font style="color:rgb(238, 63, 77);">栈</font>**<font style="color:rgb(44, 62, 80);">中的, 当查询引用类型变量的时候, 会先从</font>**<font style="color:rgb(238, 63, 77);">栈</font>**<font style="color:rgb(44, 62, 80);">中读取内存地址(也就是访问地址), 然后再通过地址找到</font>**<font style="color:rgb(238, 63, 77);">堆</font>**<font style="color:rgb(44, 62, 80);">中的值.因此, 这种我们也把它叫为</font>**<font style="color:rgb(238, 63, 77);">引用访问</font>**<font style="color:rgb(44, 62, 80);">.</font>

_<font style="color:rgb(44, 62, 80);">一张图方便你理解</font>_<font style="color:rgb(44, 62, 80);">🤔</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959432067-14de8d9c-c3bc-4ce1-abb1-d882ec2d5d6c.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在计算机的数据结构中，栈比堆的运算速度快，Object是一个复杂的结构且可以扩展：数组可扩充，对象可添加属性，都可以增删改查。将他们放在堆中是为了不影响栈的效率。而是通过引用的方式查找到堆中的实际对象再进行操作。所以查找引用类型值的时候先去</font>**<font style="color:rgb(238, 63, 77);background-color:rgb(255, 249, 249);">栈</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">查找再去</font>**<font style="color:rgb(238, 63, 77);background-color:rgb(255, 249, 249);">堆</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">查找。</font>

### [](https://www.123fe.net/blog-docs/javaScript2/18-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4.html#%E5%8F%98%E9%87%8F%E5%AD%98%E6%94%BE%E6%A1%88%E4%BE%8B)<font style="color:rgb(44, 62, 80);">变量存放案例</font>
<font style="color:rgb(44, 62, 80);">要是你读完了上面的</font>**<font style="color:rgb(238, 63, 77);">堆栈存储</font>**<font style="color:rgb(44, 62, 80);">介绍还有点模糊的话, 我们不妨来看几个案例.</font>

**<font style="color:rgb(238, 63, 77);">案例一</font>****<font style="color:rgb(238, 63, 77);">🌰</font>**<font style="color:rgb(44, 62, 80);">:</font>

```javascript
var a = 1;
var b = a;
b = 2;
console.log(a); // a = ?
```

**<font style="color:rgb(238, 63, 77);">案例二</font>****<font style="color:rgb(238, 63, 77);">🌰</font>**<font style="color:rgb(44, 62, 80);">:</font>

```javascript
var obj1 = { a: 1, b: 2 };
var obj2 = obj1;
obj2.a = 3;
console.log(obj1.a); // obj1.a = ?
```

**<font style="color:rgb(238, 63, 77);">案例三</font>****<font style="color:rgb(238, 63, 77);">🌰</font>**<font style="color:rgb(44, 62, 80);">:</font>

```javascript
var obj1 = { a: 1 };
var obj2 = obj1;
obj1 = null;
console.log(obj2); // obj2 = ?
```

<font style="color:rgb(44, 62, 80);">上面三个案例的答案分别对应的是:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1、3、{ a: 1 }</font><font style="color:rgb(44, 62, 80);">.</font>

+ <font style="color:rgb(44, 62, 80);">案例一中,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a和b</font><font style="color:rgb(44, 62, 80);">都是基本数据类型, 它们的值分别存储在各自独立的栈空间中, 是互不影响的, 所以修改了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">b</font><font style="color:rgb(44, 62, 80);">的值后</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(44, 62, 80);">还是不变.</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var b = a</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的操作, 你可以理解为单纯的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">b</font><font style="color:rgb(44, 62, 80);">赋值了值</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">, 而后</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a和b</font><font style="color:rgb(44, 62, 80);">没有任何关系了.</font>
+ <font style="color:rgb(44, 62, 80);">案例二中, 创建</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj1</font><font style="color:rgb(44, 62, 80);">的时候, 在栈中存储了一个名为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj1</font><font style="color:rgb(44, 62, 80);">的变量, 同时开辟了一个堆内存用于存放了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{a: 1, b: 2}</font><font style="color:rgb(44, 62, 80);">对象,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj1</font><font style="color:rgb(44, 62, 80);">中存放的就是指向这个堆内存对象的地址. 因此</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj2</font><font style="color:rgb(44, 62, 80);">进行赋值的时候拷贝的只是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj1</font><font style="color:rgb(44, 62, 80);">中的地址, 实际上它们指向的都是堆内存的对象.在第三步改变这个对象的值的时候, 也相当于同时改变了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj1</font><font style="color:rgb(44, 62, 80);">.</font>
+ <font style="color:rgb(44, 62, 80);">案例三中, 开始时,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj1</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj2</font><font style="color:rgb(44, 62, 80);">指向的都是同一堆内存对象</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{a: 1}</font><font style="color:rgb(44, 62, 80);">, 在第三步将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj1</font><font style="color:rgb(44, 62, 80);">赋值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">仅仅只是改变了栈中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj1</font><font style="color:rgb(44, 62, 80);">的内存地址,将它变为了基本数据类型</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">, 并不会影响堆内存对象. 同样的, 你要将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj1</font><font style="color:rgb(44, 62, 80);">不赋值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">, 而是赋值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{b: 2}</font><font style="color:rgb(44, 62, 80);">, 对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj2</font><font style="color:rgb(44, 62, 80);">也还是没有影响.</font>

### [](https://www.123fe.net/blog-docs/javaScript2/18-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4.html#%E5%86%85%E5%AD%98%E7%A9%BA%E9%97%B4%E7%AE%A1%E7%90%86)<font style="color:rgb(44, 62, 80);">内存空间管理</font>
<font style="color:rgb(44, 62, 80);">在上面我们说了那么多的栈内存, 堆内存, 那么在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">中, 是怎样管理这些内存空间的呢?</font>

<font style="color:rgb(44, 62, 80);">首先, 同样的, 内存空间也是有属于自己的生命周期, 它主要分为三个阶段:</font>

1. <font style="color:rgb(44, 62, 80);">分配你所需的内存;</font>
2. <font style="color:rgb(44, 62, 80);">使用分配到的内存(读、写);</font>
3. <font style="color:rgb(44, 62, 80);">不需要的时候将其释放、归还.</font>

<font style="color:rgb(44, 62, 80);">我们可以用个例子来看一下看.</font>

**<font style="color:rgb(238, 63, 77);">案例一</font>****<font style="color:rgb(238, 63, 77);">🌰</font>**<font style="color:rgb(44, 62, 80);">:</font>

```javascript
var a = 1; // 在内存中给数值变量分配空间
alart(a + 2); // 使用内存
a = null; // 使用完后, 释放内存空间
```

<font style="color:rgb(44, 62, 80);">上面三步分别对应着三个阶段. 当然,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a = null</font><font style="color:rgb(44, 62, 80);">这个操作是我们手动将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(44, 62, 80);">的内存空间释放. 若没有这个过程,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">会自己帮我做一些释放内存的工作吗? 答案当然是肯定的.</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">有自动垃圾收集机制, 听着这个机制的名字我想大家就知道它是做什么的了, 没错就是字面意思, 它会找出那些不再继续使用的值，然后释放其占用的内存。</font>**<font style="color:rgb(238, 63, 77);">垃圾收集器</font>**<font style="color:rgb(44, 62, 80);">会每隔固定的时间段就执行一次释放操作。</font>

<font style="color:rgb(44, 62, 80);">在自动垃圾收集机制中, 最常用的就是通过</font>**<font style="color:rgb(238, 63, 77);">标记清除</font>**<font style="color:rgb(44, 62, 80);">的算法来找到哪些对象不再继续使用. 其实上面我说的将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a = null</font><font style="color:rgb(44, 62, 80);">手动释放内存其实是不准确的. 因为使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a = null</font><font style="color:rgb(44, 62, 80);">仅仅只是做了一个释放引用的操作, 让</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(44, 62, 80);">原本对应的值失去引用, 脱离执行环境, 这个值会在下一次垃圾收集器执行操作的时候被找到并释放.</font>

<font style="color:rgb(44, 62, 80);">还有一点, 在局部作用域中, 当函数执行完毕了之后, 局部变量就没有存在下去的必要了, 此时垃圾收集器知道这类变量是需要回收的, 所以很容易判断.</font>

<font style="color:rgb(44, 62, 80);">但是全局变量什么时候需要释放内存空间则很难判断，因此在我们的开发中，原则上应该避免使用全局变量。</font>

