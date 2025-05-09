### <font style="color:rgb(44, 62, 80);">1 类型及检测方式</font>
**<font style="color:rgb(44, 62, 80);">1. JS内置类型</font>**

<font style="color:rgb(44, 62, 80);">JavaScript 的数据类型有下图所示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781081067-62115ea1-f449-4198-9d85-6327df959daa.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">其中，前 7 种类型为基础类型，最后</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1 种（Object）为引用类型</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，也是你需要重点关注的，因为它在日常工作中是使用得最频繁，也是需要关注最多技术细节的数据类型</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">一共有8种数据类型，其中有7种基本数据类型：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Undefined</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Null</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Boolean</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Number</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">String</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol</font><font style="color:rgb(44, 62, 80);">（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">es6</font><font style="color:rgb(44, 62, 80);">新增，表示独一无二的值）和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BigInt</font><font style="color:rgb(44, 62, 80);">（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">es10</font><font style="color:rgb(44, 62, 80);">新增）；</font>
+ <font style="color:rgb(44, 62, 80);">1种引用数据类型——</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(44, 62, 80);">（Object本质上是由一组无序的名值对组成的）。里面包含</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function、Array、Date</font><font style="color:rgb(44, 62, 80);">等。JavaScript不支持任何创建自定义类型的机制，而所有值最终都将是上述 8 种数据类型之一。</font>
    - **<font style="color:rgb(44, 62, 80);">引用数据类型:</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(44, 62, 80);">（包含普通对象-</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(44, 62, 80);">，数组对象-</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array</font><font style="color:rgb(44, 62, 80);">，正则对象-</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">RegExp</font><font style="color:rgb(44, 62, 80);">，日期对象-</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Date</font><font style="color:rgb(44, 62, 80);">，数学函数-</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Math</font><font style="color:rgb(44, 62, 80);">，函数对象-</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Function</font><font style="color:rgb(44, 62, 80);">）</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在这里，我想先请你重点了解下面两点，因为各种 JavaScript 的数据类型最后都会在初始化之后放在不同的内存中，因此上面的数据类型大致可以分成两类来进行存储：</font>

+ **<font style="color:rgb(44, 62, 80);">原始数据类型</font>**<font style="color:rgb(44, 62, 80);">：基础类型存储在栈内存，被引用或拷贝时，会创建一个完全相等的变量；占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储。</font>
+ **<font style="color:rgb(44, 62, 80);">引用数据类型</font>**<font style="color:rgb(44, 62, 80);">：引用类型存储在堆内存，存储的是地址，多个引用指向同一个地址，这里会涉及一个“共享”的概念；占据空间大、大小不固定。引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。</font>

**<font style="color:rgb(44, 62, 80);">JavaScript 中的数据是如何存储在内存中的？</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在 JavaScript 中，原始类型的赋值会完整复制变量值，而引用类型的赋值是复制引用地址。</font>

<font style="color:rgb(44, 62, 80);">在 JavaScript 的执行过程中， 主要有三种类型内存空间，分别是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">代码空间</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">栈空间</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">堆空间</font><font style="color:rgb(44, 62, 80);">。其中的代码空间主要是存储可执行代码的，原始类型(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Number、String、Null、Undefined、Boolean、Symbol、BigInt</font><font style="color:rgb(44, 62, 80);">)的数据值都是直接保存在“栈”中的，引用类型(Object)的值是存放在“堆”中的。因此在栈空间中(执行上下文)，原始类型存储的是变量的值，而引用类型存储的是其在"堆空间"中的地址，当 JavaScript 需要访问该数据的时候，是通过栈中的引用地址来访问的，相当于多了一道转手流程。</font>

<font style="color:rgb(44, 62, 80);">在编译过程中，如果 JavaScript 引擎判断到一个闭包，也会在堆空间创建换一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">“closure(fn)”</font><font style="color:rgb(44, 62, 80);">的对象（这是一个内部对象，JavaScript 是无法访问的），用来保存闭包中的变量。所以闭包中的变量是存储在“堆空间”中的。</font>

<font style="color:rgb(44, 62, 80);">JavaScript 引擎需要用栈来维护程序执行期间上下文的状态，如果栈空间大了话，所有的数据都存放在栈空间里面，那么会影响到上下文切换的效率，进而又影响到整个程序的执行效率。通常情况下，栈空间都不会设置太大，主要用来存放一些原始类型的小数据。而引用类型的数据占用的空间都比较大，所以这一类数据会被存放到堆中，堆空间很大，能存放很多大的数据，不过缺点是分配内存和回收内存都会占用一定的时间。因此需要“栈”和“堆”两种空间。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">题目一：初出茅庐</font>

```plain
let a = {
  name: 'lee',
  age: 18
}
let b = a;
console.log(a.name);  //第一个console
b.name = 'son';
console.log(a.name);  //第二个console
console.log(b.name);  //第三个console
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这道题比较简单，我们可以看到第一个 console 打出来 name 是 'lee'，这应该没什么疑问；但是在执行了 b.name='son' 之后，结果你会发现 a 和 b 的属性 name 都是 'son'，第二个和第三个打印结果是一样的，这里就体现了引用类型的“共享”的特性，即这两个值都存在同一块内存中共享，一个发生了改变，另外一个也随之跟着变化。</font>

<font style="color:rgb(44, 62, 80);">你可以直接在 Chrome 控制台敲一遍，深入理解一下这部分概念。下面我们再看一段代码，它是比题目一稍复杂一些的对象属性变化问题。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">题目二：渐入佳境</font>

```plain
let a = {
  name: 'Julia',
  age: 20
}
function change(o) {
  o.age = 24;
  o = {
    name: 'Kath',
    age: 30
  }
  return o;
}
let b = change(a);     // 注意这里没有new，后面new相关会有专门文章讲解
console.log(b.age);    // 第一个console
console.log(a.age);    // 第二个console
```

<font style="color:rgb(44, 62, 80);">这道题涉及了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);">，你通过上述代码可以看到第一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">console</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的结果是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">30</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">b</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">最后打印结果是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{name: "Kath", age: 30}</font><font style="color:rgb(44, 62, 80);">；第二个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">console</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的返回结果是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">24</font><font style="color:rgb(44, 62, 80);">，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">最后的打印结果是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{name: "Julia", age: 24}</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">是不是和你预想的有些区别？你要注意的是，这里的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">带来了不一样的东西。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">原因在于：函数传参进来的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">o</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，传递的是对象在堆中的内存地址值，通过调用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">o.age = 24</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（第 7 行代码）确实改变了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">age</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性；但是第 12 行代码的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">却又把</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">o</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">变成了另一个内存地址，将</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{name: "Kath", age: 30}</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">存入其中，最后返回</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">b</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的值就变成了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{name: "Kath", age: 30}</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。而如果把第 12 行去掉，那么</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">b</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就会返回</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font>

**<font style="color:rgb(44, 62, 80);">2. 数据类型检测</font>**

**<font style="color:rgb(44, 62, 80);">（1）typeof</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">typeof 对于原始类型来说，除了 null 都可以显示正确的类型</font>

```plain
console.log(typeof 2);               // number
console.log(typeof true);            // boolean
console.log(typeof 'str');           // string
console.log(typeof []);              // object     []数组的数据类型在 typeof 中被解释为 object
console.log(typeof function(){});    // function
console.log(typeof {});              // object
console.log(typeof undefined);       // undefined
console.log(typeof null);            // object     null 的数据类型被 typeof 解释为 object
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">typeof</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对于对象来说，除了函数都会显示</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，所以说</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">typeof</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">并不能准确判断变量到底是什么类型,所以想判断一个对象的正确类型，这时候可以考虑使用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">instanceof</font>

**<font style="color:rgb(44, 62, 80);">（2）instanceof</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">instanceof</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以正确的判断对象的类型，因为内部机制是通过判断对象的原型链中是不是能找到类型的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font>

```plain
console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false  
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true    
// console.log(undefined instanceof Undefined);
// console.log(null instanceof Null);
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">instanceof</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以准确地判断复杂引用数据类型，但是不能正确判断基础数据类型；</font>
+ <font style="color:rgb(44, 62, 80);">而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">typeof</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">也存在弊端，它虽然可以判断基础数据类型（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">除外），但是引用数据类型中，除了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型以外，其他的也无法判断</font>

```plain
// 我们也可以试着实现一下 instanceof
function _instanceof(left, right) {
    // 由于instance要检测的是某对象，需要有一个前置判断条件
    //基本数据类型直接返回false
    if(typeof left !== 'object' || left === null) return false;

    // 获得类型的原型
    let prototype = right.prototype
    // 获得对象的原型
    left = left.__proto__
    // 判断对象的类型是否等于类型的原型
    while (true) {
    	if (left === null)
    		return false
    	if (prototype === left)
    		return true
    	left = left.__proto__
    }
}

console.log('test', _instanceof(null, Array)) // false
console.log('test', _instanceof([], Array)) // true
console.log('test', _instanceof('', Array)) // false
console.log('test', _instanceof({}, Object)) // true
```

**<font style="color:rgb(44, 62, 80);">（3）constructor</font>**

```plain
console.log((2).constructor === Number); // true
console.log((true).constructor === Boolean); // true
console.log(('str').constructor === String); // true
console.log(([]).constructor === Array); // true
console.log((function() {}).constructor === Function); // true
console.log(({}).constructor === Object); // true
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这里有一个坑，如果我创建一个对象，更改它的原型，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">constructor</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就会变得不可靠了</font>

```plain
function Fn(){};
 
Fn.prototype=new Array();
 
var f=new Fn();
 
console.log(f.constructor===Fn);    // false
console.log(f.constructor===Array); // true
```

**<font style="color:rgb(44, 62, 80);">（4）Object.prototype.toString.call()</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toString()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的原型方法，调用该方法，可以统一返回格式为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">“[object Xxx]”</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的字符串，其中</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Xxx</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就是对象的类型。对于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象，直接调用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toString()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就能返回</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[object Object]</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">；而对于其他对象，则需要通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来调用，才能返回正确的类型信息。我们来看一下代码。</font>

```plain
Object.prototype.toString({})       // "[object Object]"
Object.prototype.toString.call({})  // 同上结果，加上call也ok
Object.prototype.toString.call(1)    // "[object Number]"
Object.prototype.toString.call('1')  // "[object String]"
Object.prototype.toString.call(true)  // "[object Boolean]"
Object.prototype.toString.call(function(){})  // "[object Function]"
Object.prototype.toString.call(null)   //"[object Null]"
Object.prototype.toString.call(undefined) //"[object Undefined]"
Object.prototype.toString.call(/123/g)    //"[object RegExp]"
Object.prototype.toString.call(new Date()) //"[object Date]"
Object.prototype.toString.call([])       //"[object Array]"
Object.prototype.toString.call(document)  //"[object HTMLDocument]"
Object.prototype.toString.call(window)   //"[object Window]"

// 从上面这段代码可以看出，Object.prototype.toString.call() 可以很好地判断引用类型，甚至可以把 document 和 window 都区分开来。
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">实现一个全局通用的数据类型判断方法，来加深你的理解，代码如下</font>

```plain
function getType(obj){
  let type  = typeof obj;
  if (type !== "object") {    // 先进行typeof判断，如果是基础数据类型，直接返回
    return type;
  }
  // 对于typeof返回结果是object的，再进行如下的判断，正则返回结果
  return Object.prototype.toString.call(obj).replace(/^\[object (\S+)\]$/, '$1');  // 注意正则中间有个空格
}
/* 代码验证，需要注意大小写，哪些是typeof判断，哪些是toString判断？思考下 */
getType([])     // "Array" typeof []是object，因此toString返回
getType('123')  // "string" typeof 直接返回
getType(window) // "Window" toString返回
getType(null)   // "Null"首字母大写，typeof null是object，需toString来判断
getType(undefined)   // "undefined" typeof 直接返回
getType()            // "undefined" typeof 直接返回
getType(function(){}) // "function" typeof能判断，因此首字母小写
getType(/123/g)      //"RegExp" toString返回
```

**<font style="color:rgb(44, 62, 80);">小结</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">typeof</font>
    - <font style="color:rgb(44, 62, 80);">直接在计算机底层基于数据类型的值（二进制）进行检测</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">typeof null</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">原因是对象存在在计算机中，都是以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">000</font><font style="color:rgb(44, 62, 80);">开始的二进制存储，所以检测出来的结果是对象</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">typeof</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">普通对象/数组对象/正则对象/日期对象 都是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">typeof NaN === 'number'</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">instanceof</font>
    - <font style="color:rgb(44, 62, 80);">检测当前实例是否属于这个类的</font>
    - <font style="color:rgb(44, 62, 80);">底层机制：只要当前类出现在实例的原型上，结果都是true</font>
    - <font style="color:rgb(44, 62, 80);">不能检测基本数据类型</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">constructor</font>
    - <font style="color:rgb(44, 62, 80);">支持基本类型</font>
    - <font style="color:rgb(44, 62, 80);">constructor可以随便改，也不准</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.prototype.toString.call([val])</font>
    - <font style="color:rgb(44, 62, 80);">返回当前实例所属类信息</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">判断</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Target</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的类型，单单用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">typeof</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">并无法完全满足，这其实并不是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bug</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，本质原因是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的万物皆对象的理论。因此要真正完美判断时，我们需要区分对待:</font>

+ <font style="color:rgb(44, 62, 80);">基本类型(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">): 使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">String(null)</font>
+ <font style="color:rgb(44, 62, 80);">基本类型(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">string / number / boolean / undefined</font><font style="color:rgb(44, 62, 80);">) +</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);">: - 直接使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">typeof</font><font style="color:rgb(44, 62, 80);">即可</font>
+ <font style="color:rgb(44, 62, 80);">其余引用类型(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array / Date / RegExp Error</font><font style="color:rgb(44, 62, 80);">): 调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toString</font><font style="color:rgb(44, 62, 80);">后根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[object XXX]</font><font style="color:rgb(44, 62, 80);">进行判断</font>

**<font style="color:rgb(44, 62, 80);">3. 数据类型转换</font>**

<font style="color:rgb(44, 62, 80);">我们先看一段代码，了解下大致的情况。</font>

```plain
'123' == 123   // false or true?
'' == null    // false or true?
'' == 0        // false or true?
[] == 0        // false or true?
[] == ''       // false or true?
[] == ![]      // false or true?
null == undefined //  false or true?
Number(null)     // 返回什么？
Number('')      // 返回什么？
parseInt('');    // 返回什么？
{}+10           // 返回什么？
let obj = {
    [Symbol.toPrimitive]() {
        return 200;
    },
    valueOf() {
        return 300;
    },
    toString() {
        return 'Hello';
    }
}
console.log(obj + 200); // 这里打印出来是多少？
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">首先我们要知道，在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中类型转换只有三种情况，分别是：</font>

+ <font style="color:rgb(44, 62, 80);">转换为布尔值</font>
+ <font style="color:rgb(44, 62, 80);">转换为数字</font>
+ <font style="color:rgb(44, 62, 80);">转换为字符串</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781081485-7ef67c56-8a25-4934-aff0-b11fe149bd8d.png)

**<font style="color:rgb(44, 62, 80);">转Boolean</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在条件判断时，除了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NaN</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">''</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">-0</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，其他所有值都转为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，包括所有对象</font>

```plain
Boolean(0)          //false
Boolean(null)       //false
Boolean(undefined)  //false
Boolean(NaN)        //false
Boolean(1)          //true
Boolean(13)         //true
Boolean('12')       //true
```

**<font style="color:rgb(44, 62, 80);">对象转原始类型</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象在转换类型的时候，会调用内置的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[[ToPrimitive]]</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数，对于该函数来说，算法逻辑一般来说如下</font>

+ <font style="color:rgb(44, 62, 80);">如果已经是原始类型了，那就不需要转换了</font>
+ <font style="color:rgb(44, 62, 80);">调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">x.valueOf()</font><font style="color:rgb(44, 62, 80);">，如果转换为基础类型，就返回转换的值</font>
+ <font style="color:rgb(44, 62, 80);">调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">x.toString()</font><font style="color:rgb(44, 62, 80);">，如果转换为基础类型，就返回转换的值</font>
+ <font style="color:rgb(44, 62, 80);">如果都没有返回原始类型，就会报错</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当然你也可以重写</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol.toPrimitive</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，该方法在转原始类型时调用优先级最高。</font>

```plain
let a = {
  valueOf() {
    return 0
  },
  toString() {
    return '1'
  },
  [Symbol.toPrimitive]() {
    return 2
  }
}
1 + a // => 3
```

**<font style="color:rgb(44, 62, 80);">四则运算符</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">它有以下几个特点：</font>

+ <font style="color:rgb(44, 62, 80);">运算中其中一方为字符串，那么就会把另一方也转换为字符串</font>
+ <font style="color:rgb(44, 62, 80);">如果一方不是字符串或者数字，那么会将它转换为数字或者字符串</font>

```plain
1 + '1' // '11'
true + true // 2
4 + [1,2,3] // "41,2,3"
```

+ <font style="color:rgb(44, 62, 80);">对于第一行代码来说，触发特点一，所以将数字</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">转换为字符串，得到结果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">'11'</font>
+ <font style="color:rgb(44, 62, 80);">对于第二行代码来说，触发特点二，所以将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">转为数字</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font>
+ <font style="color:rgb(44, 62, 80);">对于第三行代码来说，触发特点二，所以将数组通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toString</font><font style="color:rgb(44, 62, 80);">转为字符串</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1,2,3</font><font style="color:rgb(44, 62, 80);">，得到结果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">41,2,3</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">另外对于加法还需要注意这个表达式</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">'a' + + 'b'</font>

```plain
'a' + + 'b' // -> "aNaN"
```

+ <font style="color:rgb(44, 62, 80);">因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">+ 'b'</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NaN</font><font style="color:rgb(44, 62, 80);">，所以结果为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">"aNaN"</font><font style="color:rgb(44, 62, 80);">，你可能也会在一些代码中看到过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">+ '1'</font><font style="color:rgb(44, 62, 80);">的形式来快速获取</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">number</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型。</font>
+ <font style="color:rgb(44, 62, 80);">那么对于除了加法的运算符来说，只要其中一方是数字，那么另一方就会被转为数字</font>

```plain
4 * '3' // 12
4 * [] // 0
4 * [1, 2] // NaN
```

**<font style="color:rgb(44, 62, 80);">比较运算符</font>**

+ <font style="color:rgb(44, 62, 80);">如果是对象，就通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toPrimitive</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">转换对象</font>
+ <font style="color:rgb(44, 62, 80);">如果是字符串，就通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unicode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">字符索引来比较</font>

```plain
let a = {
  valueOf() {
    return 0
  },
  toString() {
    return '1'
  }
}
a > -1 // true
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在以上代码中，因为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是对象，所以会通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">valueOf</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">转换为原始类型再比较值。</font>

**<font style="color:rgb(44, 62, 80);">强制类型转换</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">强制类型转换方式包括</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Number()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">parseInt()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">parseFloat()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toString()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">String()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Boolean()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，这几种方法都比较类似</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Number()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的强制转换规则</font>
+ <font style="color:rgb(44, 62, 80);">如果是布尔值，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">分别被转换为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">如果是数字，返回自身；</font>
+ <font style="color:rgb(44, 62, 80);">如果是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">，返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">如果是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">，返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NaN</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">如果是字符串，遵循以下规则：如果字符串中只包含数字（或者是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0X / 0x</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">开头的十六进制数字字符串，允许包含正负号），则将其转换为十进制；如果字符串中包含有效的浮点格式，将其转换为浮点数值；如果是空字符串，将其转换为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">；如果不是以上格式的字符串，均返回 NaN；</font>
+ <font style="color:rgb(44, 62, 80);">如果是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol</font><font style="color:rgb(44, 62, 80);">，抛出错误；</font>
+ <font style="color:rgb(44, 62, 80);">如果是对象，并且部署了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[Symbol.toPrimitive]</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，那么调用此方法，否则调用对象的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">valueOf()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法，然后依据前面的规则转换返回的值；如果转换的结果是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NaN</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，则调用对象的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toString()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法，再次依照前面的顺序转换返回对应的值。</font>

```plain
Number(true);        // 1
Number(false);       // 0
Number('0111');      //111
Number(null);        //0
Number('');          //0
Number('1a');        //NaN
Number(-0X11);       //-17
Number('0X11')       //17
```

**<font style="color:rgb(44, 62, 80);">Object 的转换规则</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象转换的规则，会先调用内置的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[ToPrimitive]</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数，其规则逻辑如下：</font>

+ <font style="color:rgb(44, 62, 80);">如果部署了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol.toPrimitive</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法，优先调用再返回；</font>
+ <font style="color:rgb(44, 62, 80);">调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">valueOf()</font><font style="color:rgb(44, 62, 80);">，如果转换为基础类型，则返回；</font>
+ <font style="color:rgb(44, 62, 80);">调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toString()</font><font style="color:rgb(44, 62, 80);">，如果转换为基础类型，则返回；</font>
+ <font style="color:rgb(44, 62, 80);">如果都没有返回基础类型，会报错。</font>

```plain
var obj = {
  value: 1,
  valueOf() {
    return 2;
  },
  toString() {
    return '3'
  },
  [Symbol.toPrimitive]() {
    return 4
  }
}
console.log(obj + 1); // 输出5
// 因为有Symbol.toPrimitive，就优先执行这个；如果Symbol.toPrimitive这段代码删掉，则执行valueOf打印结果为3；如果valueOf也去掉，则调用toString返回'31'(字符串拼接)
// 再看两个特殊的case：
10 + {}
// "10[object Object]"，注意：{}会默认调用valueOf是{}，不是基础类型继续转换，调用toString，返回结果"[object Object]"，于是和10进行'+'运算，按照字符串拼接规则来，参考'+'的规则C
[1,2,undefined,4,5] + 10
// "1,2,,4,510"，注意[1,2,undefined,4,5]会默认先调用valueOf结果还是这个数组，不是基础数据类型继续转换，也还是调用toString，返回"1,2,,4,5"，然后再和10进行运算，还是按照字符串拼接规则，参考'+'的第3条规则
```

**<font style="color:rgb(44, 62, 80);">'==' 的隐式类型转换规则</font>**

+ <font style="color:rgb(44, 62, 80);">如果类型相同，无须进行类型转换；</font>
+ <font style="color:rgb(44, 62, 80);">如果其中一个操作值是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">，那么另一个操作符必须为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">，才会返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">，否则都返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">如果其中一个是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型，那么返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">两个操作值如果为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">string</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和 number 类型，那么就会将字符串转换为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">number</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">如果一个操作值是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">boolean</font><font style="color:rgb(44, 62, 80);">，那么转换成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">number</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">如果一个操作值为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">且另一方为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">string</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">number</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">symbol</font><font style="color:rgb(44, 62, 80);">，就会把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">转为原始类型再进行判断（调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">valueOf/toString</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法进行转换）。</font>

```plain
null == undefined       // true  规则2
null == 0               // false 规则2
'' == null              // false 规则2
'' == 0                 // true  规则4 字符串转隐式转换成Number之后再对比
'123' == 123            // true  规则4 字符串转隐式转换成Number之后再对比
0 == false              // true  e规则 布尔型隐式转换成Number之后再对比
1 == true               // true  e规则 布尔型隐式转换成Number之后再对比
var a = {
  value: 0,
  valueOf: function() {
    this.value++;
    return this.value;
  }
};
// 注意这里a又可以等于1、2、3
console.log(a == 1 && a == 2 && a ==3);  //true f规则 Object隐式转换
// 注：但是执行过3遍之后，再重新执行a==3或之前的数字就是false，因为value已经加上去了，这里需要注意一下
```

**<font style="color:rgb(44, 62, 80);">'+' 的隐式类型转换规则</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">'+' 号操作符，不仅可以用作数字相加，还可以用作字符串拼接。仅当 '+' 号两边都是数字时，进行的是加法运算；如果两边都是字符串，则直接拼接，无须进行隐式类型转换。</font>

+ <font style="color:rgb(44, 62, 80);">如果其中有一个是字符串，另外一个是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或布尔型，则调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toString()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法进行字符串拼接；如果是纯对象、数组、正则等，则默认调用对象的转换方法会存在优先级，然后再进行拼接。</font>
+ <font style="color:rgb(44, 62, 80);">如果其中有一个是数字，另外一个是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">、布尔型或数字，则会将其转换成数字进行加法运算，对象的情况还是参考上一条规则。</font>
+ <font style="color:rgb(44, 62, 80);">如果其中一个是字符串、一个是数字，则按照字符串规则进行拼接</font>

```plain
1 + 2        // 3  常规情况
'1' + '2'    // '12' 常规情况
// 下面看一下特殊情况
'1' + undefined   // "1undefined" 规则1，undefined转换字符串
'1' + null        // "1null" 规则1，null转换字符串
'1' + true        // "1true" 规则1，true转换字符串
'1' + 1n          // '11' 比较特殊字符串和BigInt相加，BigInt转换为字符串
1 + undefined     // NaN  规则2，undefined转换数字相加NaN
1 + null          // 1    规则2，null转换为0
1 + true          // 2    规则2，true转换为1，二者相加为2
1 + 1n            // 错误  不能把BigInt和Number类型直接混合相加
'1' + 3           // '13' 规则3，字符串拼接
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">整体来看，如果数据中有字符串，JavaScript 类型转换还是更倾向于转换成字符串，因为第三条规则中可以看到，在字符串和数字相加的过程中最后返回的还是字符串，这里需要关注一下</font>

**<font style="color:rgb(44, 62, 80);">null 和 undefined 的区别？</font>**

+ <font style="color:rgb(44, 62, 80);">首先</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Undefined</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">都是基本数据类型，这两个基本数据类型分别都只有一个值，就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">代表的含义是未定义，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">代表的含义是空对象（其实不是真的对象，请看下面的注意！）。一般变量声明了但还没有定义的时候会返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">主要用于赋值给一些可能会返回对象的变量，作为初始化。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">其实 null 不是对象，虽然 typeof null 会输出 object，但是这只是 JS 存在的一个悠久 Bug。在 JS 的最初版本中使用的是 32 位系统，为了性能考虑使用低位存储变量的类型信息，000 开头代表是对象，然而 null 表示为全零，所以将它错误的判断为 object 。虽然现在的内部类型判断代码已经改变了，但是对于这个 Bug 却是一直流传下来。</font>

+ <font style="color:rgb(44, 62, 80);">undefined 在 js 中不是一个保留字，这意味着我们可以使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来作为一个变量名，这样的做法是非常危险的，它会影响我们对 undefined 值的判断。但是我们可以通过一些方法获得安全的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值，比如说</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">void 0</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">当我们对两种类型使用 typeof 进行判断的时候，Null 类型化会返回 “object”，这是一个历史遗留的问题。当我们使用双等号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。</font>

### [](https://www.123fe.net/docs/base/improve.html#_2-this)<font style="color:rgb(44, 62, 80);">2 This</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不同情况的调用，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">指向分别如何。顺带可以提一下</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">es6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中箭头函数没有</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">,</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">,</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">super</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">等，这些只依赖包含箭头函数最接近的函数</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们先来看几个函数调用的场景</font>

```plain
function foo() {
  console.log(this.a)
}
var a = 1
foo()

const obj = {
  a: 2,
  foo: foo
}
obj.foo()

const c = new foo()
```

+ <font style="color:rgb(44, 62, 80);">对于直接调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">foo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来说，不管</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">foo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数被放在了什么地方，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一定是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font>
+ <font style="color:rgb(44, 62, 80);">对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj.foo()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来说，我们只需要记住，谁调用了函数，谁就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">，所以在这个场景下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">foo</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象</font>
+ <font style="color:rgb(44, 62, 80);">对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的方式来说，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">被永远绑定在了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">c</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">上面，不会被任何方式改变</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">说完了以上几种情况，其实很多代码中的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">应该就没什么问题了，下面让我们看看箭头函数中的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font>

```plain
function a() {
  return () => {
    return () => {
      console.log(this)
    }
  }
}
console.log(a()()())
```

+ <font style="color:rgb(44, 62, 80);">首先箭头函数其实是没有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的，箭头函数中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">只取决包裹箭头函数的第一个普通函数的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">。在这个例子中，因为包裹箭头函数的第一个普通函数是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(44, 62, 80);">，所以此时的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font><font style="color:rgb(44, 62, 80);">。另外对箭头函数使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(44, 62, 80);">这类函数是无效的。</font>
+ <font style="color:rgb(44, 62, 80);">最后种情况也就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这些改变上下文的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">API</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">了，对于这些函数来说，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">取决于第一个参数，如果第一个参数为空，那么就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">那么说到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(44, 62, 80);">，不知道大家是否考虑过，如果对一个函数进行多次</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(44, 62, 80);">，那么上下文会是什么呢？</font>

```plain
let a = {}
let fn = function () { console.log(this) }
fn.bind().bind(a)() // => ?
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如果你认为输出结果是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，那么你就错了，其实我们可以把上述代码转换成另一种形式</font>

```plain
// fn.bind().bind(a) 等于
let fn2 = function fn1() {
  return function() {
    return fn.apply()
  }.apply(a)
}
fn2()
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以从上述代码中发现，不管我们给函数</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">几次，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fn</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">永远由第一次</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">决定，所以结果永远是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font>

```plain
let a = { name: 'poetries' }
function foo() {
  console.log(this.name)
}
foo.bind(a)() // => 'poetries'
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以上就是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的规则了，但是可能会发生多个规则同时出现的情况，这时候不同的规则之间会根据优先级最高的来决定</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">最终指向哪里。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">首先，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的方式优先级最高，接下来是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这些函数，然后是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj.foo()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这种调用方式，最后是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">foo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这种调用方式，同时，箭头函数的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">一旦被绑定，就不会再被任何方式所改变。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781081620-cb5cc5a5-98b7-4400-96ff-bfc71f024972.png)

**<font style="color:rgb(44, 62, 80);">函数执行改变this</font>**

+ <font style="color:rgb(44, 62, 80);">由于 JS 的设计原理: 在函数中，可以引用运行环境中的变量。因此就需要一个机制来让我们可以在函数体内部获取当前的运行环境，这便是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">因此要明白</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">指向，其实就是要搞清楚 函数的运行环境，说人话就是，谁调用了函数。例如</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj.fn()</font><font style="color:rgb(44, 62, 80);">，便是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">调用了函数，既函数中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this === obj</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fn()</font><font style="color:rgb(44, 62, 80);">，这里可以看成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.fn()</font><font style="color:rgb(44, 62, 80);">，因此</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this === window</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">但这种机制并不完全能满足我们的业务需求，因此提供了三种方式可以手动修改</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的指向:</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call: fn.call(target, 1, 2)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply: fn.apply(target, [1, 2])</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind: fn.bind(target)(1,2)</font>

### [](https://www.123fe.net/docs/base/improve.html#_3-apply-call-bind-%E5%8E%9F%E7%90%86)<font style="color:rgb(44, 62, 80);">3 apply/call/bind 原理</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781081657-a85fae4f-462f-4d92-ab30-0a24334799b3.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call、apply</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是挂在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Function</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象上的三个方法，调用这三个方法的必须是一个函数。</font>

```plain
func.call(thisArg, param1, param2, ...)
func.apply(thisArg, [param1,param2,...])
func.bind(thisArg, param1, param2, ...)
```

+ <font style="color:rgb(44, 62, 80);">在浏览器里，在全局范围内this 指向window对象；</font>
+ <font style="color:rgb(44, 62, 80);">在函数中，this永远指向最后调用他的那个对象；</font>
+ <font style="color:rgb(44, 62, 80);">构造函数中，this指向new出来的那个新的对象；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call、apply、bind</font><font style="color:rgb(44, 62, 80);">中的this被强绑定在指定的那个对象上；</font>
+ <font style="color:rgb(44, 62, 80);">箭头函数中this比较特殊,箭头函数this为父作用域的this，不是调用时的this.要知道前四种方式,都是调用时确定,也就是动态的,而箭头函数的this指向是静态的,声明的时候就确定了下来；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply、call、bind</font><font style="color:rgb(44, 62, 80);">都是js给函数内置的一些API，调用他们可以为函数指定this的执行,同时也可以传参。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781081625-0bca8cf4-3b4e-4e75-b161-c3cf57120707.png)

```plain
let a = {
    value: 1
}
function getValue(name, age) {
    console.log(name)
    console.log(age)
    console.log(this.value)
}
getValue.call(a, 'poe', '24')
getValue.apply(a, ['poe', '24'])
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和其他两个方法作用也是一致的，只是该方法会返回一个函数。并且我们可以通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">实现柯里化</font>

**<font style="color:rgb(44, 62, 80);">方法的应用场景</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">下面几种应用场景，你多加体会就可以发现它们的理念都是“借用”方法的思路。我们来看看都有哪些。</font>

1. <font style="color:rgb(44, 62, 80);">判断数据类型</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.prototype.toString</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来判断类型是最合适的，借用它我们几乎可以判断所有类型的数据</font>

```plain
function getType(obj){
  let type  = typeof obj;
  if (type !== "object") {
    return type;
  }
  return Object.prototype.toString.call(obj).replace(/^$/, '$1');
}
```

1. <font style="color:rgb(44, 62, 80);">类数组借用方法</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">类数组因为不是真正的数组，所有没有数组类型上自带的种种方法，所以我们就可以利用一些方法去借用数组的方法，比如借用数组的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">push</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法，看下面的一段代码。</font>

```plain
var arrayLike = { 
  0: 'java',
  1: 'script',
  length: 2
} 
Array.prototype.push.call(arrayLike, 'jack', 'lily'); 
console.log(typeof arrayLike); // 'object'
console.log(arrayLike);
// {0: "java", 1: "script", 2: "jack", 3: "lily", length: 4}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的方法来借用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array 原型链上的 push</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法，可以实现一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">类数组的 push</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法，给</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arrayLike</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">添加新的元素</font>

1. <font style="color:rgb(44, 62, 80);">获取数组的最大 / 最小值</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们可以用 apply 来实现数组中判断最大 / 最小值，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">直接传递数组作为调用方法的参数，也可以减少一步展开数组，可以直接使用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Math.max、Math.min</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来获取数组的最大值 / 最小值，请看下面这段代码。</font>

```plain
let arr = [13, 6, 10, 11, 16];
const max = Math.max.apply(Math, arr); 
const min = Math.min.apply(Math, arr);
 
console.log(max);  // 16
console.log(min);  // 6
```

**<font style="color:rgb(44, 62, 80);">实现一个 bind 函数</font>**

<font style="color:rgb(44, 62, 80);">对于实现以下几个函数，可以从几个方面思考</font>

+ <font style="color:rgb(44, 62, 80);">不传入第一个参数，那么默认为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font>
+ <font style="color:rgb(44, 62, 80);">改变了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指向，让新的对象可以执行该函数。那么思路是否可以变成给新的对象添加一个函数，然后在执行完以后删除？</font>

```plain
Function.prototype.myBind = function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  var _this = this
  var args = [...arguments].slice(1)
  // 返回一个函数
  return function F() {
    // 因为返回了一个函数，我们可以 new F()，所以需要判断
    if (this instanceof F) {
      return new _this(...args, ...arguments)
    }
    return _this.apply(context, args.concat(...arguments))
  }
}
```

**<font style="color:rgb(44, 62, 80);">实现一个 call 函数</font>**

```plain
Function.prototype.myCall = function (context) {
  var context = context || window
  // 给 context 添加一个属性
  // getValue.call(a, 'pp', '24') => a.fn = getValue
  context.fn = this
  // 将 context 后面的参数取出来
  var args = [...arguments].slice(1)
  // getValue.call(a, 'pp', '24') => a.fn('pp', '24')
  var result = context.fn(...args)
  // 删除 fn
  delete context.fn
  return result
}
```

**<font style="color:rgb(44, 62, 80);">实现一个 apply 函数</font>**

```plain
Function.prototype.myApply = function(context = window, ...args) {
  // this-->func  context--> obj  args--> 传递过来的参数

  // 在context上加一个唯一值不影响context上的属性
  let key = Symbol('key')
  context[key] = this; // context为调用的上下文,this此处为函数，将这个函数作为context的方法
  // let args = [...arguments].slice(1)   //第一个参数为obj所以删除,伪数组转为数组
  
  let result = context[key](...args); 
  delete context[key]; // 不删除会导致context属性越来越多
  return result;
}
```

```plain
// 使用
function f(a,b){
 console.log(a,b)
 console.log(this.name)
}
let obj={
 name:'张三'
}
f.myApply(obj,[1,2])  //arguments[1]
```

### [](https://www.123fe.net/docs/base/improve.html#_4-%E5%8F%98%E9%87%8F%E6%8F%90%E5%8D%87)<font style="color:rgb(44, 62, 80);">4 变量提升</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当执行</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">代码时，会生成执行环境，只要代码不是写在函数中的，就是在全局执行环境中，函数中的代码会产生函数执行环境，只此两种执行环境。</font>

```plain
b() // call b
console.log(a) // undefined

var a = 'Hello world'

function b() {
    console.log('call b')
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">想必以上的输出大家肯定都已经明白了，这是因为函数和变量提升的原因。通常提升的解释是说将声明的代码移动到了顶部，这其实没有什么错误，便于大家理解。但是更准确的解释应该是：在生成执行环境时，会有两个阶段。第一个阶段是创建的阶段，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">解释器会找出需要提升的变量和函数，并且给他们提前在内存中开辟好空间，函数的话会将整个函数存入内存中，变量只声明并且赋值为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，所以在第二个阶段，也就是代码执行阶段，我们可以直接提前使用</font>

+ <font style="color:rgb(44, 62, 80);">在提升的过程中，相同的函数会覆盖上一个函数，并且函数优先于变量提升</font>

```plain
b() // call b second

function b() {
    console.log('call b fist')
}
function b() {
    console.log('call b second')
}
var b = 'Hello world'
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会产生很多错误，所以在 ES6中引入了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不能在声明前使用，但是这并不是常说的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不会提升，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">提升了，在第一阶段内存也已经为他开辟好了空间，但是因为这个声明的特性导致了并不能在声明前使用</font>

### [](https://www.123fe.net/docs/base/improve.html#_5-%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87)<font style="color:rgb(44, 62, 80);">5 执行上下文</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当执行 JS 代码时，会产生三种执行上下文</font>

+ <font style="color:rgb(44, 62, 80);">全局执行上下文</font>
+ <font style="color:rgb(44, 62, 80);">函数执行上下文</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eval</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">执行上下文</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">每个执行上下文中都有三个重要的属性</font>

+ <font style="color:rgb(44, 62, 80);">变量对象（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VO</font><font style="color:rgb(44, 62, 80);">），包含变量、函数声明和函数的形参，该属性只能在全局上下文中访问</font>
+ <font style="color:rgb(44, 62, 80);">作用域链（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">采用词法作用域，也就是说变量的作用域是在定义时就决定了）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font>

```plain
var a = 10
function foo(i) {
  var b = 20
}
foo()
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对于上述代码，执行栈中有两个上下文：全局上下文和函数 foo 上下文。</font>

```plain
stack = [
    globalContext,
    fooContext
]
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对于全局上下文来说，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VO</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">大概是这样的</font>

```plain
globalContext.VO === globe
globalContext.VO = {
    a: undefined,
	foo: <Function>,
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对于函数</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">foo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来说，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VO</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不能访问，只能访问到活动对象（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AO</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）</font>

```plain
fooContext.VO === foo.AO
fooContext.AO {
    i: undefined,
	b: undefined,
    arguments: <>
}
// arguments 是函数独有的对象(箭头函数没有)
// 该对象是一个伪数组，有 `length` 属性且可以通过下标访问元素
// 该对象中的 `callee` 属性代表函数本身
// `caller` 属性代表函数的调用者
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对于作用域链，可以把它理解成包含自身变量对象和上级变量对象的列表，通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[[Scope]]</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性查找上级变量</font>

```plain
fooContext.[[Scope]] = [
    globalContext.VO
]
fooContext.Scope = fooContext.[[Scope]] + fooContext.VO
fooContext.Scope = [
    fooContext.VO,
    globalContext.VO
]
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">接下来让我们看一个老生常谈的例子，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var</font>

```plain
b() // call b
console.log(a) // undefined

var a = 'Hello world'

function b() {
	console.log('call b')
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">想必以上的输出大家肯定都已经明白了，这是因为函数和变量提升的原因。通常提升的解释是说将声明的代码移动到了顶部，这其实没有什么错误，便于大家理解。但是更准确的解释应该是：在生成执行上下文时，会有两个阶段。第一个阶段是创建的阶段（具体步骤是创建</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VO</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">），</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">解释器会找出需要提升的变量和函数，并且给他们提前在内存中开辟好空间，函数的话会将整个函数存入内存中，变量只声明并且赋值为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，所以在第二个阶段，也就是代码执行阶段，我们可以直接提前使用。</font>

+ <font style="color:rgb(44, 62, 80);">在提升的过程中，相同的函数会覆盖上一个函数，并且函数优先于变量提升</font>

```plain
b() // call b second

function b() {
	console.log('call b fist')
}
function b() {
	console.log('call b second')
}
var b = 'Hello world'
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会产生很多错误，所以在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中引入了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不能在声明前使用，但是这并不是常说的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不会提升，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">提升了声明但没有赋值，因为临时死区导致了并不能在声明前使用。</font>

+ <font style="color:rgb(44, 62, 80);">对于非匿名的立即执行函数需要注意以下一点</font>

```plain
var foo = 1
(function foo() {
    foo = 10
    console.log(foo)
}()) // -> ƒ foo() { foo = 10 ; console.log(foo) }
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">因为当</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">解释器在遇到非匿名的立即执行函数时，会创建一个辅助的特定对象，然后将函数名称作为这个对象的属性，因此函数内部才可以访问到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">foo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，但是这个值又是只读的，所以对它的赋值并不生效，所以打印的结果还是这个函数，并且外部的值也没有发生更改。</font>

```plain
specialObject = {};

Scope = specialObject + Scope;

foo = new FunctionExpression;
foo.[[Scope]] = Scope;
specialObject.foo = foo; // {DontDelete}, {ReadOnly}

delete Scope[0]; // remove specialObject from the front of scope chain
```

**<font style="color:rgb(44, 62, 80);">总结</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">执行上下文可以简单理解为一个对象:</font>

**<font style="color:rgb(44, 62, 80);">它包含三个部分:</font>**

+ <font style="color:rgb(44, 62, 80);">变量对象(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VO</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(44, 62, 80);">作用域链(词法作用域)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">指向</font>

**<font style="color:rgb(44, 62, 80);">它的类型:</font>**

+ <font style="color:rgb(44, 62, 80);">全局执行上下文</font>
+ <font style="color:rgb(44, 62, 80);">函数执行上下文</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eval</font><font style="color:rgb(44, 62, 80);">执行上下文</font>

**<font style="color:rgb(44, 62, 80);">代码执行过程:</font>**

+ <font style="color:rgb(44, 62, 80);">创建 全局上下文 (</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">global EC</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(44, 62, 80);">全局执行上下文 (</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">caller</font><font style="color:rgb(44, 62, 80);">) 逐行 自上而下 执行。遇到函数时，函数执行上下文 (</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callee</font><font style="color:rgb(44, 62, 80);">) 被</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">push</font><font style="color:rgb(44, 62, 80);">到执行栈顶层</font>
+ <font style="color:rgb(44, 62, 80);">函数执行上下文被激活，成为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">active EC</font><font style="color:rgb(44, 62, 80);">, 开始执行函数中的代码，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">caller</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">被挂起</font>
+ <font style="color:rgb(44, 62, 80);">函数执行完后，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callee</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">被</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pop</font><font style="color:rgb(44, 62, 80);">移除出执行栈，控制权交还全局上下文 (</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">caller</font><font style="color:rgb(44, 62, 80);">)，继续执行</font>

### [](https://www.123fe.net/docs/base/improve.html#_6-%E4%BD%9C%E7%94%A8%E5%9F%9F)<font style="color:rgb(44, 62, 80);">6 作用域</font>
+ <font style="color:rgb(44, 62, 80);">作用域： 作用域是定义变量的区域，它有一套访问变量的规则，这套规则来管理浏览器引擎如何在当前作用域以及嵌套的作用域中根据变量（标识符）进行变量查找</font>
+ <font style="color:rgb(44, 62, 80);">作用域链： 作用域链的作用是保证对执行环境有权访问的所有变量和函数的有序访问，通过作用域链，我们可以访问到外层环境的变量和 函数。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">作用域链的本质上是一个指向变量对象的指针列表。变量对象是一个包含了执行环境中所有变量和函数的对象。作用域链的前 端始终都是当前执行上下文的变量对象。全局执行上下文的变量对象（也就是全局对象）始终是作用域链的最后一个对象。</font>

+ <font style="color:rgb(44, 62, 80);">当我们查找一个变量时，如果当前执行环境中没有找到，我们可以沿着作用域链向后查找</font>
+ <font style="color:rgb(44, 62, 80);">作用域链的创建过程跟执行上下文的建立有关....</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">作用域可以理解为变量的可访问性，总共分为三种类型，分别为：</font>

+ <font style="color:rgb(44, 62, 80);">全局作用域</font>
+ <font style="color:rgb(44, 62, 80);">函数作用域</font>
+ <font style="color:rgb(44, 62, 80);">块级作用域，ES6 中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">const</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就可以产生该作用域</font>

<font style="color:rgb(44, 62, 80);">其实看完前面的闭包、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这部分内部的话，应该基本能了解作用域的一些应用。</font>

<font style="color:rgb(44, 62, 80);">一旦我们将这些作用域嵌套起来，就变成了另外一个重要的知识点「作用域链」，也就是 JS 到底是如何访问需要的变量或者函数的。</font>

+ <font style="color:rgb(44, 62, 80);">首先作用域链是在定义时就被确定下来的，和箭头函数里的 this 一样，后续不会改变，JS 会一层层往上寻找需要的内容。</font>
+ <font style="color:rgb(44, 62, 80);">其实作用域链这个东西我们在闭包小结中已经看到过它的实体了：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[[Scopes]]</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781082635-254a3520-4ce2-4edb-80bc-8d6e96cdd6b1.png)

<font style="color:rgb(44, 62, 80);">图中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[[Scopes]]</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是个数组，作用域的一层层往上寻找就等同于遍历</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[[Scopes]]</font><font style="color:rgb(44, 62, 80);">。</font>

**<font style="color:rgb(44, 62, 80);">1. 全局作用域</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">全局变量是挂载在 window 对象下的变量，所以在网页中的任何位置你都可以使用并且访问到这个全局变量</font>

```plain
var globalName = 'global';
function getName() { 
  console.log(globalName) // global
  var name = 'inner'
  console.log(name) // inner
} 
getName();
console.log(name); // 
console.log(globalName); //global
function setName(){ 
  vName = 'setName';
}
setName();
console.log(vName); // setName
```

+ <font style="color:rgb(44, 62, 80);">从这段代码中我们可以看到，globalName 这个变量无论在什么地方都是可以被访问到的，所以它就是全局变量。而在 getName 函数中作为局部变量的 name 变量是不具备这种能力的</font>
+ <font style="color:rgb(44, 62, 80);">当然全局作用域有相应的缺点，我们定义很多全局变量的时候，会容易引起变量命名的冲突，所以在定义变量的时候应该注意作用域的问题。</font>

**<font style="color:rgb(44, 62, 80);">2. 函数作用域</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数中定义的变量叫作函数变量，这个时候只能在函数内部才能访问到它，所以它的作用域也就是函数的内部，称为函数作用域</font>

```plain
function getName () {
  var name = 'inner';
  console.log(name); //inner
}
getName();
console.log(name);
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">除了这个函数内部，其他地方都是不能访问到它的。同时，当这个函数被执行完之后，这个局部变量也相应会被销毁。所以你会看到在 getName 函数外面的 name 是访问不到的</font>

**<font style="color:rgb(44, 62, 80);">3. 块级作用域</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">ES6 中新增了块级作用域，最直接的表现就是新增的 let 关键词，使用 let 关键词定义的变量只能在块级作用域中被访问，有“暂时性死区”的特点，也就是说这个变量在定义之前是不能被使用的。</font>

<font style="color:rgb(44, 62, 80);">在 JS 编码过程中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">if 语句</font><font style="color:rgb(44, 62, 80);">及</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">语句后面</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{...}</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这里面所包括的，就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">块级作用域</font>

```plain
console.log(a) //a is not defined
if(true){
  let a = '123'；
  console.log(a)； // 123
}
console.log(a) //a is not defined
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从这段代码可以看出，变量 a 是在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">if 语句{...}</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中由</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let 关键词</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">进行定义的变量，所以它的作用域是 if 语句括号中的那部分，而在外面进行访问 a 变量是会报错的，因为这里不是它的作用域。所以在 if 代码块的前后输出 a 这个变量的结果，控制台会显示 a 并没有定义</font>

### [](https://www.123fe.net/docs/base/improve.html#_7-%E9%97%AD%E5%8C%85)<font style="color:rgb(44, 62, 80);">7 闭包</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">闭包其实就是一个可以访问其他函数内部变量的函数。创建闭包的最常见的方式就是在一个函数内创建另一个函数，创建的函数可以 访问到当前函数的局部变量。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781083303-443af934-8b27-40dd-a8a8-a62c56568af0.png)

<font style="color:rgb(44, 62, 80);">因为通常情况下，函数内部变量是无法在外部访问的（即全局变量和局部变量的区别），因此使用闭包的作用，就具备实现了能在外部访问某个函数内部变量的功能，让这些内部变量的值始终可以保存在内存中。下面我们通过代码先来看一个简单的例子</font>

```plain
function fun1() {
	var a = 1;
	return function(){
		console.log(a);
	};
}
fun1();
var result = fun1();
result();  // 1

// 结合闭包的概念，我们把这段代码放到控制台执行一下，就可以发现最后输出的结果是 1（即 a 变量的值）。那么可以很清楚地发现，a 变量作为一个 fun1 函数的内部变量，正常情况下作为函数内的局部变量，是无法被外部访问到的。但是通过闭包，我们最后还是可以拿到 a 变量的值
```

**<font style="color:rgb(44, 62, 80);">闭包有两个常用的用途</font>**

+ <font style="color:rgb(44, 62, 80);">闭包的第一个用途是使我们在函数外部能够访问到函数内部的变量。通过使用闭包，我们可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量，可以使用这种方法来创建私有变量。</font>
+ <font style="color:rgb(44, 62, 80);">函数的另一个用途是使已经运行结束的函数上下文中的变量对象继续留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会被回收。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">其实闭包的本质就是作用域链的一个特殊的应用，只要了解了作用域链的创建过程，就能够理解闭包的实现原理。</font>

```plain
let a = 1
// fn 是闭包
function fn() {
  console.log(a);
}

function fn1() {
  let a = 1
  // 这里也是闭包
  return () => {
    console.log(a);
  }
}
const fn2 = fn1()
fn2()
```

+ <font style="color:rgb(44, 62, 80);">大家都知道闭包其中一个作用是访问私有变量，就比如上述代码中的 fn2 访问到了 fn1 函数中的变量 a。但是此时 fn1 早已销毁，我们是如何访问到变量 a 的呢？不是都说原始类型是存放在栈上的么，为什么此时却没有被销毁掉？</font>
+ <font style="color:rgb(44, 62, 80);">接下来笔者会根据浏览器的表现来重新理解关于原始类型存放位置的说法。</font>
+ <font style="color:rgb(44, 62, 80);">先来说下数据存放的正确规则是：局部、占用空间确定的数据，一般会存放在栈中，否则就在堆中（也有例外）。 那么接下来我们可以通过 Chrome 来帮助我们验证这个说法说法。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781083290-56212353-3568-463b-b0c8-0c09df09e4d0.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上图中画红框的位置我们能看到一个内部的对象</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[[Scopes]]</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，其中存放着变量 a，该对象是被存放在堆上的，其中包含了闭包、全局对象等等内容，因此我们能通过闭包访问到本该销毁的变量。</font>

<font style="color:rgb(44, 62, 80);">另外最开始我们对于闭包的定位是：假如一个函数能访问外部的变量，那么这个函数它就是一个闭包，因此接下来我们看看在全局下的表现是怎么样的。</font>

```plain
let a = 1
var b = 2
// fn 是闭包
function fn() {
  console.log(a, b);
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781083376-4dce342e-c0d6-422d-8f74-d914e034b9d9.png)

<font style="color:rgb(44, 62, 80);">从上图我们能发现全局下声明的变量，如果是 var 的话就直接被挂到 globe 上，如果是其他关键字声明的话就被挂到 Script 上。虽然这些内容同样还是存在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[[Scopes]]</font><font style="color:rgb(44, 62, 80);">，但是全局变量应该是存放在静态区域的，因为全局变量无需进行垃圾回收，等需要回收的时候整个应用都没了。</font>

<font style="color:rgb(44, 62, 80);">只有在下图的场景中，原始类型才可能是被存储在栈上。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这里为什么要说可能，是因为 JS 是门动态类型语言，一个变量声明时可以是原始类型，马上又可以赋值为对象类型，然后又回到原始类型。这样频繁的在堆栈上切换存储位置，内部引擎是不是也会有什么优化手段，或者干脆全部都丢堆上？只有 const 声明的原始类型才一定存在栈上？当然这只是笔者的一个推测，暂时没有深究，读者可以忽略这段瞎想</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781084140-8b88d818-1bfd-4b1d-8512-87f9ecfaaefd.png)

<font style="color:rgb(44, 62, 80);">因此笔者对于原始类型存储位置的理解为：局部变量才是被存储在栈上，全局变量存在静态区域上，其它都存储在堆上。</font>

<font style="color:rgb(44, 62, 80);">当然这个理解是建立的 Chrome 的表现之上的，在不同的浏览器上因为引擎的不同，可能存储的方式还是有所变化的。</font>

**<font style="color:rgb(44, 62, 80);">闭包产生的原因</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们在前面介绍了作用域的概念，那么你还需要明白作用域链的基本概念。其实很简单，当访问一个变量时，代码解释器会首先在当前的作用域查找，如果没找到，就去父级作用域去查找，直到找到该变量或者不存在父级作用域中，这样的链路就是作用域链</font>

<font style="color:rgb(44, 62, 80);">需要注意的是，每一个子函数都会拷贝上级的作用域，形成一个作用域的链条。那么我们还是通过下面的代码来详细说明一下作用域链</font>

```plain
var a = 1;
function fun1() {
  var a = 2
  function fun2() {
    var a = 3;
    console.log(a);//3
  }
}
```

+ <font style="color:rgb(44, 62, 80);">从中可以看出，fun1 函数的作用域指向全局作用域（window）和它自己本身；fun2 函数的作用域指向全局作用域 （window）、fun1 和它本身；而作用域是从最底层向上找，直到找到全局作用域 window 为止，如果全局还没有的话就会报错。</font>
+ <font style="color:rgb(44, 62, 80);">那么这就很形象地说明了什么是作用域链，即当前函数一般都会存在上层函数的作用域的引用，那么他们就形成了一条作用域链。</font>
+ <font style="color:rgb(44, 62, 80);">由此可见，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">闭包产生的本质就是：当前环境中存在指向父级作用域的引用</font><font style="color:rgb(44, 62, 80);">。那么还是拿上的代码举例。</font>

```plain
function fun1() {
  var a = 2
  function fun2() {
    console.log(a);  //2
  }
  return fun2;
}
var result = fun1();
result();
```

+ <font style="color:rgb(44, 62, 80);">从上面这段代码可以看出，这里 result 会拿到父级作用域中的变量，输出 2。因为在当前环境中，含有对 fun2 函数的引用，fun2 函数恰恰引用了 window、fun1 和 fun2 的作用域。因此 fun2 函数是可以访问到 fun1 函数的作用域的变量。</font>
+ <font style="color:rgb(44, 62, 80);">那是不是只有返回函数才算是产生了闭包呢？其实也不是，回到闭包的本质，我们只需要让父级作用域的引用存在即可，因此还可以这么改代码，如下所示</font>

```plain
var fun3;
function fun1() {
  var a = 2
  fun3 = function() {
    console.log(a);
  }
}
fun1();
fun3();
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以看出，其中实现的结果和前一段代码的效果其实是一样的，就是在给 fun3 函数赋值后，fun3 函数就拥有了 window、fun1 和 fun3 本身这几个作用域的访问权限；然后还是从下往上查找，直到找到 fun1 的作用域中存在 a 这个变量；因此输出的结果还是 2，最后产生了闭包，形式变了，本质没有改变。</font>

<font style="color:rgb(44, 62, 80);">因此最后</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">返回的不管是不是函数，也都不能说明没有产生闭包</font>

**<font style="color:rgb(44, 62, 80);">闭包的表现形式</font>**

1. <font style="color:rgb(44, 62, 80);">返回一个函数</font>
2. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">在定时器、事件监听、Ajax 请求、Web Workers 或者任何异步中，只要使用了回调函数，实际上就是在使用闭包</font><font style="color:rgb(44, 62, 80);">。请看下面这段代码，这些都是平常开发中用到的形式</font>

```plain
// 定时器
setTimeout(function handler(){
  console.log('1');
}，1000);
// 事件监听
$('app').click(function(){
  console.log('Event Listener');
});
```

1. <font style="color:rgb(44, 62, 80);">作为函数参数传递的形式，比如下面的例子。</font>

```plain
var a = 1;
function foo(){
  var a = 2;
  function baz(){
    console.log(a);
  }
  bar(baz);
}
function bar(fn){
  // 这就是闭包
  fn();
}
foo();  // 输出2，而不是1
```

1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IIFE（立即执行函数），创建了闭包，保存了全局作用域（window）和当前函数的作用域</font><font style="color:rgb(44, 62, 80);">，因此可以输出全局的变量，如下所示</font>

```plain
var a = 2;
(function IIFE(){
  console.log(a);  // 输出2
})();
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">IIFE 这个函数会稍微有些特殊，算是一种自执行匿名函数，这个匿名函数拥有独立的作用域。这不仅可以避免了外界访问此 IIFE 中的变量，而且又不会污染全局作用域，我们经常能在高级的 JavaScript 编程中看见此类函数。</font>

**<font style="color:rgb(44, 62, 80);">如何解决循环输出问题？</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在互联网大厂的面试中，解决循环输出问题是比较高频的面试题，一般都会给一段这样的代码让你来解释</font>

```plain
for(var i = 1; i <= 5; i ++){
  setTimeout(function() {
    console.log(i)
  }, 0)
}
```

<font style="color:rgb(44, 62, 80);">上面这段代码执行之后，从控制台执行的结果可以看出来，结果输出的是 5 个 6，那么一般面试官都会先问为什么都是 6？我想让你实现输出 1、2、3、4、5 的话怎么办呢？</font>

<font style="color:rgb(44, 62, 80);">因此结合本讲所学的知识我们来思考一下，应该怎么给面试官一个满意的解释。你可以围绕这两点来回答。</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为宏任务，由于 JS 中单线程</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eventLoop 机制</font><font style="color:rgb(44, 62, 80);">，在主线程同步任务执行完后才去执行宏任务，因此</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">循环结束后 setTimeout 中的回调才依次执行</font>
+ <font style="color:rgb(44, 62, 80);">因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数也是一种闭包，往上找它的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">父级作用域链就是 window</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">变量 i 为 window 上的全局变量</font><font style="color:rgb(44, 62, 80);">，开始执行 setTimeout 之前变量 i 已经就是 6 了，因此最后输出的连续就都是 6。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">那么我们再来看看如何按顺序依次输出 1、2、3、4、5 呢？</font>

1. <font style="color:rgb(44, 62, 80);">利用 IIFE</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以利用 IIFE（立即执行函数），当每次 for 循环时，把此时的变量 i 传递到定时器中，然后执行，改造之后的代码如下。</font>

```plain
for(var i = 1;i <= 5;i++){
  (function(j){
    setTimeout(function timer(){
      console.log(j)
    }, 0)
  })(i)
}
```

1. <font style="color:rgb(44, 62, 80);">使用 ES6 中的 let</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">ES6 中新增的 let 定义变量的方式，使得 ES6 之后 JS 发生革命性的变化，让 JS 有了块级作用域，代码的作用域以块级为单位进行执行。通过改造后的代码，可以实现上面想要的结果。</font>

```plain
for(let i = 1; i <= 5; i++){
  setTimeout(function() {
    console.log(i);
  },0)
}
```

1. <font style="color:rgb(44, 62, 80);">定时器传入第三个参数</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">setTimeout 作为经常使用的定时器，它是存在第三个参数的，日常工作中我们经常使用的一般是前两个，一个是回调函数，另外一个是时间，而第三个参数用得比较少。那么结合第三个参数，调整完之后的代码如下。</font>

```plain
for(var i=1;i<=5;i++){
  setTimeout(function(j) {
    console.log(j)
  }, 0, i)
}
```

<font style="color:rgb(44, 62, 80);">从中可以看到，第三个参数的传递，可以改变 setTimeout 的执行逻辑，从而实现我们想要的结果，这也是一种解决循环输出问题的途径</font>

**<font style="color:rgb(44, 62, 80);">常见考点</font>**

+ <font style="color:rgb(44, 62, 80);">闭包能考的很多，概念和笔试题都会考。</font>
+ <font style="color:rgb(44, 62, 80);">概念题就是考考闭包是什么了。</font>
+ <font style="color:rgb(44, 62, 80);">笔试题的话基本都会结合上异步，比如最常见的：</font>

```plain
for (var i = 0; i < 6; i++) {
  setTimeout(() => {
    console.log(i)
  })
}
```

<font style="color:rgb(44, 62, 80);">这道题会问输出什么，有哪几种方式可以得到想要的答案？</font>

### [](https://www.123fe.net/docs/base/improve.html#_8-new%E7%9A%84%E5%8E%9F%E7%90%86)<font style="color:rgb(44, 62, 80);">8 New的原理</font>
**<font style="color:rgb(44, 62, 80);">常见考点</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">做了那些事？</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">返回不同的类型时会有什么表现？</font>
+ <font style="color:rgb(44, 62, 80);">手写 new 的实现过程</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">new 关键词的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">主要作用就是执行一个构造函数、返回一个实例对象</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，在 new 的过程中，根据构造函数的情况，来确定是否可以接受参数的传递。下面我们通过一段代码来看一个简单的 new 的例子</font>

```plain
function Person(){
   this.name = 'Jack';
}
var p = new Person(); 
console.log(p.name)  // Jack
```

<font style="color:rgb(44, 62, 80);">这段代码比较容易理解，从输出结果可以看出，p 是一个通过 person 这个构造函数生成的一个实例对象，这个应该很容易理解。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">操作符可以帮助我们构建出一个实例，并且绑定上 this，内部执行步骤可大概分为以下几步：</font>

1. <font style="color:rgb(44, 62, 80);">创建一个新对象</font>
2. <font style="color:rgb(44, 62, 80);">对象连接到构造函数原型上，并绑定</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">（this 指向新对象）</font>
3. <font style="color:rgb(44, 62, 80);">执行构造函数代码（为这个新对象添加属性）</font>
4. <font style="color:rgb(44, 62, 80);">返回新对象</font>

<font style="color:rgb(44, 62, 80);">在第四步返回新对象这边有一个情况会例外：</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">那么问题来了，如果不用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个关键词，结合上面的代码改造一下，去掉</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，会发生什么样的变化呢？我们再来看下面这段代码</font>

```plain
function Person(){
  this.name = 'Jack';
}
var p = Person();
console.log(p) // undefined
console.log(name) // Jack
console.log(p.name) // 'name' of undefined
```

+ <font style="color:rgb(44, 62, 80);">从上面的代码中可以看到，我们没有使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个关键词，返回的结果就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">。其中由于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">代码在默认情况下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的指向是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font><font style="color:rgb(44, 62, 80);">，那么</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">name</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的输出结果就为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Jack</font><font style="color:rgb(44, 62, 80);">，这是一种不存在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">关键词的情况。</font>
+ <font style="color:rgb(44, 62, 80);">那么当构造函数中有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一个对象的操作，结果又会是什么样子呢？我们再来看一段在上面的基础上改造过的代码。</font>

```plain
function Person(){
   this.name = 'Jack'; 
   return {age: 18}
}
var p = new Person(); 
console.log(p)  // {age: 18}
console.log(p.name) // undefined
console.log(p.age) // 18
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">通过这段代码又可以看出，当构造函数最后</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">出来的是一个和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">无关的对象时，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new 命令会直接返回这个新对象</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">而不是通过 new 执行步骤生成的 this 对象</font>

<font style="color:rgb(44, 62, 80);">但是这里要求构造函数必须是返回一个对象，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">如果返回的不是对象，那么还是会按照 new 的实现步骤，返回新生成的对象</font><font style="color:rgb(44, 62, 80);">。接下来还是在上面这段代码的基础之上稍微改动一下</font>

```plain
function Person(){
   this.name = 'Jack'; 
   return 'tom';
}
var p = new Person(); 
console.log(p)  // {name: 'Jack'}
console.log(p.name) // Jack
```

<font style="color:rgb(44, 62, 80);">可以看出，当构造函数中</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的不是一个对象时，那么它还是会根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">关键词的执行逻辑，生成一个新的对象（绑定了最新</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">），最后返回出来</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">因此我们总结一下：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new 关键词执行之后总是会返回一个对象，要么是实例对象，要么是 return 语句指定的对象</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781085635-11932b84-cffd-4463-895b-2ca2cf602d63.png)

**<font style="color:rgb(44, 62, 80);">手工实现New的过程</font>**

```plain
function create(fn, ...args) {
  if(typeof fn !== 'function') {
    throw 'fn must be a function';
  }
	// 1、用new Object() 的方式新建了一个对象obj
  // var obj = new Object()
	// 2、给该对象的__proto__赋值为fn.prototype，即设置原型链
  // obj.__proto__ = fn.prototype

  // 1、2步骤合并
  // 创建一个空对象，且这个空对象继承构造函数的 prototype 属性
  // 即实现 obj.__proto__ === constructor.prototype
  var obj = Object.create(fn.prototype);

	// 3、执行fn，并将obj作为内部this。使用 apply，改变构造函数 this 的指向到新建的对象，这样 obj 就可以访问到构造函数中的属性
  var res = fn.apply(obj, args);
	// 4、如果fn有返回值，则将其作为new操作返回内容，否则返回obj
	return res instanceof Object ? res : obj;
};
```

+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.create</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj 的</font><font style="color:rgb(44, 62, 80);">proto</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">指向为构造函数的原型</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法，将构造函数内的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指向为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">create</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">返回时，使用三目运算符决定返回结果。</font>

<font style="color:rgb(44, 62, 80);">我们知道，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">构造函数如果有显式返回值，且返回值为对象类型，那么构造函数返回结果不再是目标实例</font>

<font style="color:rgb(44, 62, 80);">如下代码：</font>

```plain
function Person(name) {
  this.name = name
  return {1: 1}
}
const person = new Person(Person, 'lucas')
console.log(person)
// {1: 1}
```

<font style="color:rgb(44, 62, 80);">测试</font>

```plain
//使用create代替new
function Person() {...}
// 使用内置函数new
var person = new Person(1,2)

// 使用手写的new，即create
var person = create(Person, 1,2)
```

**<font style="color:rgb(44, 62, 80);">new 被调用后大致做了哪几件事情</font>**

+ <font style="color:rgb(44, 62, 80);">让实例可以访问到私有属性；</font>
+ <font style="color:rgb(44, 62, 80);">让实例可以访问构造函数原型（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">constructor.prototype</font><font style="color:rgb(44, 62, 80);">）所在原型链上的属性；</font>
+ <font style="color:rgb(44, 62, 80);">构造函数返回的最后结果是引用数据类型。</font>

### [](https://www.123fe.net/docs/base/improve.html#_9-%E5%8E%9F%E5%9E%8B-%E5%8E%9F%E5%9E%8B%E9%93%BE)<font style="color:rgb(44, 62, 80);">9 原型/原型链</font>
**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font>****<font style="color:rgb(44, 62, 80);">和prototype关系</font>**<font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">constructor</font><font style="color:rgb(44, 62, 80);">是</font>**<font style="color:rgb(44, 62, 80);">对象</font>**<font style="color:rgb(44, 62, 80);">独有的。</font><font style="color:rgb(44, 62, 80);">2️⃣</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);">属性是</font>**<font style="color:rgb(44, 62, 80);">函数</font>**<font style="color:rgb(44, 62, 80);">独有的</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在 js 中我们是使用构造函数来新建一个对象的，每一个构造函数的内部都有一个 prototype 属性值，这个属性值是一个对象，这个对象包含了可以由该构造函数的所有实例共享的属性和方法。当我们使用构造函数新建一个对象后，在这个对象的内部将包含一个指针，这个指针指向构造函数的 prototype 属性对应的值，在 ES5 中这个指针被称为对象的原型。一般来说我们是不应该能够获取到这个值的，但是现在浏览器中都实现了 proto 属性来让我们访问这个属性，但是我们最好不要使用这个属性，因为它不是规范中规定的。ES5 中新增了一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.getPrototypeOf()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法，我们可以通过这个方法来获取对象的原型。</font>

<font style="color:rgb(44, 62, 80);">当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链的概念。原型链的尽头一般来说都是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.prototype</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所以这就是我们新建的对象为什么能够使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">toString()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等方法的原因。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">特点：JavaScript 对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与 之相关的对象也会继承这一改变</font>

+ <font style="color:rgb(44, 62, 80);">原型(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);">): 一个简单的对象，用于实现对象的 属性继承。可以简单的理解成对象的爹。在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Firefox</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Chrome</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中，每个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">对象中都包含一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);">(非标准)的属性指向它爹(该对象的原型)，可</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj.__proto__</font><font style="color:rgb(44, 62, 80);">进行访问。</font>
+ <font style="color:rgb(44, 62, 80);">构造函数: 可以通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);">来 新建一个对象 的函数。</font>
+ <font style="color:rgb(44, 62, 80);">实例: 通过构造函数和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);">创建出来的对象，便是实例。 实例通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);">指向原型，通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">constructor</font><font style="color:rgb(44, 62, 80);">指向构造函数。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为例，我们常用的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">便是一个构造函数，因此我们可以通过它构建实例。</font>

```plain
// 实例
const instance = new Object()
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">则此时， 实例为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">instance</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">, 构造函数为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，我们知道，构造函数拥有一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的属性指向原型，因此原型为:</font>

```plain
// 原型
const prototype = Object.prototype
```

**<font style="color:rgb(44, 62, 80);">这里我们可以来看出三者的关系:</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">实例.__proto__ === 原型</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">原型.constructor === 构造函数</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">构造函数.prototype === 原型</font>

```plain
// 这条线其实是是基于原型进行获取的，可以理解成一条基于原型的映射线
// 例如: 
// const o = new Object()
// o.constructor === Object   --> true
// o.__proto__ = null;
// o.constructor === Object   --> false
实例.constructor === 构造函数
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781086855-cb726271-20ad-450a-9911-116ec107a40d.png)

**<font style="color:rgb(44, 62, 80);">原型链</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">原型链是由原型对象组成，每个对象都有</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性，指向了创建该对象的构造函数的原型，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">将对象连接起来组成了原型链。是一个用来实现继承和共享属性的有限的对象链</font>

+ <font style="color:rgb(44, 62, 80);">属性查找机制: 当查找对象的属性时，如果实例对象自身不存在该属性，则沿着原型链往上一级查找，找到时则输出，不存在时，则继续沿着原型链往上一级查找，直至最顶级的原型对象</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.prototype</font><font style="color:rgb(44, 62, 80);">，如还是没找到，则输出</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">属性修改机制: 只会修改实例对象本身的属性，如果不存在，则进行添加该属性，如果需要修改原型的属性时，则可以用:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">b.prototype.x = 2</font><font style="color:rgb(44, 62, 80);">；但是这样会造成所有继承于该对象的实例的属性发生改变。</font>

**<font style="color:rgb(44, 62, 80);">js 获取原型的方法</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p.proto</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p.constructor.prototype</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.getPrototypeOf(p)</font>

**<font style="color:rgb(44, 62, 80);">总结</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781086924-ceb50a3c-eed8-4465-b8f9-8b5f44b60ece.png)

+ <font style="color:rgb(44, 62, 80);">每个函数都有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，除了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Function.prototype.bind()</font><font style="color:rgb(44, 62, 80);">，该属性指向原型。</font>
+ <font style="color:rgb(44, 62, 80);">每个对象都有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，指向了创建该对象的构造函数的原型。其实这个属性指向了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[[prototype]]</font><font style="color:rgb(44, 62, 80);">，但是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[[prototype]]</font><font style="color:rgb(44, 62, 80);">是内部属性，我们并不能访问到，所以使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_proto_</font><font style="color:rgb(44, 62, 80);">来访问。</font>
+ <font style="color:rgb(44, 62, 80);">对象可以通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来寻找不属于该对象的属性，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将对象连接起来组成了原型链。</font>

### [](https://www.123fe.net/docs/base/improve.html#_10-%E7%BB%A7%E6%89%BF)<font style="color:rgb(44, 62, 80);">10 继承</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781088304-6b6b8594-dbd9-4cd0-971f-fb6738aeaf1c.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">涉及面试题：原型如何实现继承？</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Class</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如何实现继承？</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Class</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">本质是什么？</font>

<font style="color:rgb(44, 62, 80);">首先先来讲下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);">，其实在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">中并不存在类，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">只是语法糖，本质还是函数</font>

```plain
class Person {}
Person instanceof Function // true
```

**<font style="color:rgb(44, 62, 80);">组合继承</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">组合继承是最常用的继承方式</font>

```plain
function Parent(value) {
  this.val = value
}
Parent.prototype.getValue = function() {
  console.log(this.val)
}
function Child(value) {
  Parent.call(this, value)
}
Child.prototype = new Parent()

const child = new Child(1)

child.getValue() // 1
child instanceof Parent // true
```

+ <font style="color:rgb(44, 62, 80);">以上继承的方式核心是在子类的构造函数中通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Parent.call(this)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">继承父类的属性，然后改变子类的原型为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new Parent()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来继承父类的函数。</font>
+ <font style="color:rgb(44, 62, 80);">这种继承方式优点在于构造函数可以传参，不会与父类引用属性共享，可以复用父类的函数，但是也存在一个缺点就是在继承父类函数的时候调用了父类构造函数，导致子类的原型上多了不需要的父类属性，存在内存上的浪费</font>

**<font style="color:rgb(44, 62, 80);">寄生组合继承</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这种继承方式对组合继承进行了优化，组合继承缺点在于继承父类函数时调用了构造函数，我们只需要优化掉这点就行了</font>

```plain
function Parent(value) {
  this.val = value
}
Parent.prototype.getValue = function() {
  console.log(this.val)
}

function Child(value) {
  Parent.call(this, value)
}
Child.prototype = Object.create(Parent.prototype, {
  constructor: {
    value: Child,
    enumerable: false,
    writable: true,
    configurable: true
  }
})

const child = new Child(1)

child.getValue() // 1
child instanceof Parent // true
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以上继承实现的核心就是将父类的原型赋值给了子类，并且将构造函数设置为子类，这样既解决了无用的父类属性问题，还能正确的找到子类的构造函数。</font>

**<font style="color:rgb(44, 62, 80);">Class 继承</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以上两种继承方式都是通过原型去解决的，在 ES6 中，我们可以使用 class 去实现继承，并且实现起来很简单</font>

```plain
class Parent {
  constructor(value) {
    this.val = value
  }
  getValue() {
    console.log(this.val)
  }
}
class Child extends Parent {
  constructor(value) {
    super(value)
    this.val = value
  }
}
let child = new Child(1)
child.getValue() // 1
child instanceof Parent // true
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">实现继承的核心在于使用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">extends</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">表明继承自哪个父类，并且在子类构造函数中必须调用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">super</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，因为这段代码可以看成</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Parent.call(this, value)</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

**<font style="color:rgb(44, 62, 80);">ES5 和 ES6 继承的区别：</font>**

+ <font style="color:rgb(44, 62, 80);">ES6 继承的子类需要调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">super()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">才能拿到子类，ES5 的话是通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这种绑定的方式</font>
+ <font style="color:rgb(44, 62, 80);">类声明不会提升，和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这些一致</font>

```plain
function Super() {}
Super.prototype.getNumber = function() {
  return 1
}

function Sub() {}
Sub.prototype = Object.create(Super.prototype, {
  constructor: {
    value: Sub,
    enumerable: false,
    writable: true,
    configurable: true
  }
})
let s = new Sub()
s.getNumber()
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以下详细讲解几种常见的继承方式</font>

**<font style="color:rgb(44, 62, 80);">1. 方式1: 借助call</font>**

```plain
function Parent1(){
    this.name = 'parent1';
  }
  function Child1(){
    Parent1.call(this);
    this.type = 'child1'
  }
  console.log(new Child1);
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这样写的时候子类虽然能够拿到父类的属性值，但是问题是父类原型对象中一旦存在方法那么子类无法继承。那么引出下面的方法。</font>

**<font style="color:rgb(44, 62, 80);">2. 方式2: 借助原型链</font>**

```plain
function Parent2() {
    this.name = 'parent2';
    this.play = [1, 2, 3]
  }
  function Child2() {
    this.type = 'child2';
  }
  Child2.prototype = new Parent2();

  console.log(new Child2());
```

<font style="color:rgb(44, 62, 80);">看似没有问题，父类的方法和属性都能够访问，但实际上有一个潜在的不足。举个例子：</font>

```plain
var s1 = new Child2();
var s2 = new Child2();
s1.play.push(4);
console.log(s1.play, s2.play);
```

<font style="color:rgb(44, 62, 80);">可以看到控制台：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781089069-0ee0ad98-bfb9-4162-8b21-b0daf001987a.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">明明我只改变了s1的play属性，为什么s2也跟着变了呢？很简单，因为两个实例使用的是同一个原型对象。</font>

<font style="color:rgb(44, 62, 80);">那么还有更好的方式么？</font>

**<font style="color:rgb(44, 62, 80);">3. 方式3：将前两种组合</font>**

```plain
function Parent3 () {
    this.name = 'parent3';
    this.play = [1, 2, 3];
  }
  function Child3() {
    Parent3.call(this);
    this.type = 'child3';
  }
  Child3.prototype = new Parent3();
  var s3 = new Child3();
  var s4 = new Child3();
  s3.play.push(4);
  console.log(s3.play, s4.play);
```

<font style="color:rgb(44, 62, 80);">可以看到控制台：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781089123-da1981ee-6054-46e1-80ee-43fba9341b5a.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之前的问题都得以解决。但是这里又徒增了一个新问题，那就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Parent3</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的构造函数会多执行了一次（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Child3.prototype = new Parent3();</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）。这是我们不愿看到的。那么如何解决这个问题？</font>

**<font style="color:rgb(44, 62, 80);">4. 方式4: 组合继承的优化1</font>**

```plain
function Parent4 () {
    this.name = 'parent4';
    this.play = [1, 2, 3];
  }
  function Child4() {
    Parent4.call(this);
    this.type = 'child4';
  }
  Child4.prototype = Parent4.prototype;
```

<font style="color:rgb(44, 62, 80);">这里让将父类原型对象直接给到子类，父类构造函数只执行一次，而且父类属性和方法均能访问，但是我们来测试一下：</font>

```plain
var s3 = new Child4();
var s4 = new Child4();
console.log(s3)
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781089855-beb26acf-d56e-4cb6-bd26-1b7f71bdeecb.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">子类实例的构造函数是Parent4，显然这是不对的，应该是Child4。</font>

**<font style="color:rgb(44, 62, 80);">5. 方式5(最推荐使用): 组合继承的优化2</font>**

```plain
function Parent5 () {
    this.name = 'parent5';
    this.play = [1, 2, 3];
  }
  function Child5() {
    Parent5.call(this);
    this.type = 'child5';
  }
  Child5.prototype = Object.create(Parent5.prototype);
  Child5.prototype.constructor = Child5;
```

<font style="color:rgb(44, 62, 80);">这是最推荐的一种方式，接近完美的继承，它的名字也叫做寄生组合继承。</font>

**<font style="color:rgb(44, 62, 80);">6. ES6的extends被编译后的JavaScript代码</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">ES6的代码最后都是要在浏览器上能够跑起来的，这中间就利用了babel这个编译工具，将ES6的代码编译成ES5让一些不支持新语法的浏览器也能运行。</font>

<font style="color:rgb(44, 62, 80);">那最后编译成了什么样子呢？</font>

```plain
function _possibleConstructorReturn(self, call) {
    // ...
    return call && (typeof call === 'object' || typeof call === 'function') ? call : self;
}

function _inherits(subClass, superClass) {
    // ...
    //看到没有
    subClass.prototype = Object.create(superClass && superClass.prototype, {
        constructor: {
            value: subClass,
            enumerable: false,
            writable: true,
            configurable: true
        }
    });
    if (superClass) Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass;
}


var Parent = function Parent() {
    // 验证是否是 Parent 构造出来的 this
    _classCallCheck(this, Parent);
};

var Child = (function (_Parent) {
    _inherits(Child, _Parent);

    function Child() {
        _classCallCheck(this, Child);

        return _possibleConstructorReturn(this, (Child.__proto__ || Object.getPrototypeOf(Child)).apply(this, arguments));
    }

    return Child;
}(Parent));
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">核心是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">_inherits</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数，可以看到它采用的依然也是第五种方式————寄生组合继承方式，同时证明了这种方式的成功。不过这里加了一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.setPrototypeOf(subClass, superClass)</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，这是用来干啥的呢？</font>

<font style="color:rgb(44, 62, 80);">答案是用来继承父类的静态方法。这也是原来的继承方式疏忽掉的地方。</font>

**<font style="color:rgb(44, 62, 80);">追问: 面向对象的设计一定是好的设计吗？</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不一定。从继承的角度说，这一设计是存在巨大隐患的。</font>

### [](https://www.123fe.net/docs/base/improve.html#_11-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1)<font style="color:rgb(44, 62, 80);">11 面向对象</font>
**<font style="color:rgb(44, 62, 80);">编程思想</font>**

+ <font style="color:rgb(44, 62, 80);">基本思想是使用对象，类，继承，封装等基本概念来进行程序设计</font>
+ <font style="color:rgb(44, 62, 80);">优点</font>
    - <font style="color:rgb(44, 62, 80);">易维护</font>
        * <font style="color:rgb(44, 62, 80);">采用面向对象思想设计的结构，可读性高，由于继承的存在，即使改变需求，那么维护也只是在局部模块，所以维护起来是非常方便和较低成本的</font>
    - <font style="color:rgb(44, 62, 80);">易扩展</font>
    - <font style="color:rgb(44, 62, 80);">开发工作的重用性、继承性高，降低重复工作量。</font>
    - <font style="color:rgb(44, 62, 80);">缩短了开发周期</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">一般面向对象包含：继承，封装，多态，抽象</font>

**<font style="color:rgb(44, 62, 80);">1. 对象形式的继承</font>**

<font style="color:rgb(44, 62, 80);">浅拷贝</font>

```plain
var Person = {
    name: 'poetry',
    age: 18,
    address: {
        home: 'home',
        office: 'office',
    }
    sclools: ['x','z'],
};

var programer = {
    language: 'js',
};

function extend(p, c){
    var c = c || {};
    for( var prop in p){
        c[prop] = p[prop];
    }
}
extend(Person, programer);
programer.name;  // poetry
programer.address.home;  // home
programer.address.home = 'house';  //house
Person.address.home;  // house
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从上面的结果看出，浅拷贝的缺陷在于修改了子对象中引用类型的值，会影响到父对象中的值，因为在浅拷贝中对引用类型的拷贝只是拷贝了地址，指向了内存中同一个副本</font>

<font style="color:rgb(44, 62, 80);">深拷贝</font>

```plain
function extendDeeply(p, c){
    var c = c || {};
    for (var prop in p){
        if(typeof p[prop] === "object"){
            c[prop] = (p[prop].constructor === Array)?[]:{};
            extendDeeply(p[prop], c[prop]);
        }else{
            c[prop] = p[prop];
        }
    }
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">利用递归进行深拷贝，这样子对象的修改就不会影响到父对象</font>

```plain
extendDeeply(Person, programer);
programer.address.home = 'poetry';
Person.address.home; // home
```

**<font style="color:rgb(44, 62, 80);">利用call和apply继承</font>**

```plain
function Parent(){
    this.name = "abc";
    this.address = {home: "home"};
}
function Child(){
    Parent.call(this);
    this.language = "js"; 
}
```

**<font style="color:rgb(44, 62, 80);">ES5中的Object.create()</font>**

```plain
var p = { name : 'poetry'};
var obj = Object.create(p);
obj.name; // poetry
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.create()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">作为new操作符的替代方案是ES5之后才出来的。我们也可以自己模拟该方法：</font>

```plain
//模拟Object.create()方法
function myCreate(o){
    function F(){};
    F.prototype = o;
    o = new F();
    return o;
}
var p = { name : 'poetry'};
var obj = myCreate(p);
obj.name; // poetry
```

<font style="color:rgb(44, 62, 80);">目前，各大浏览器的最新版本（包括IE9）都部署了这个方法。如果遇到老式浏览器，可以用下面的代码自行部署</font>

```plain
if (!Object.create) {
　　　　Object.create = function (o) {
　　　　　　 function F() {}
　　　　　　F.prototype = o;
　　　　　　return new F();
　　　　};
　　}
```

**<font style="color:rgb(44, 62, 80);">2. 类的继承</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.create()</font>

```plain
function Person(name, age){}
Person.prototype.headCount = 1;
Person.prototype.eat = function(){
    console.log('eating...');
}
function Programmer(name, age, title){}

Programmer.prototype = Object.create(Person.prototype); //建立继承关系
Programmer.prototype.constructor = Programmer;  // 修改constructor的指向
```

**<font style="color:rgb(44, 62, 80);">调用父类方法</font>**

```plain
function Person(name, age){
    this.name = name;
    this.age = age;
}
Person.prototype.headCount = 1;
Person.prototype.eat = function(){
    console.log('eating...');
}

function Programmer(name, age, title){
    Person.apply(this, arguments); // 调用父类的构造器
}


Programmer.prototype = Object.create(Person.prototype);
Programmer.prototype.constructor = Programmer;

Programmer.prototype.language = "js";
Programmer.prototype.work = function(){
    console.log('i am working code in '+ this.language);
    Person.prototype.eat.apply(this, arguments); // 调用父类上的方法
}
```

**<font style="color:rgb(44, 62, 80);">3. 封装</font>**

+ <font style="color:rgb(44, 62, 80);">命名空间</font>
    - <font style="color:rgb(44, 62, 80);">js是没有命名空间的，因此可以用对象模拟</font>

```plain
var app = {};  // 命名空间app
//模块1
app.module1 = {
    name: 'poetry',
    f: function(){
        console.log('hi robot');
    }
};
app.module1.name; // "poetry"
app.module1.f();  // hi robot
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象的属性外界是可读可写 如何来达到封装的额目的？答：可通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">闭包+局部变量</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来完成</font>

+ <font style="color:rgb(44, 62, 80);">在构造函数内部声明局部变量 和普通方法</font>
+ <font style="color:rgb(44, 62, 80);">因为作用域的关系 只有构造函数内的方法</font>
+ <font style="color:rgb(44, 62, 80);">才能访问局部变量 而方法对于外界是开放的</font>
+ <font style="color:rgb(44, 62, 80);">因此可以通过方法来访问 原本外界访问不到的局部变量 达到函数封装的目的</font>

```plain
function Girl(name,age){
	var love = '小明';//love 是局部变量 准确说不属于对象 属于这个函数的额激活对象 函数调用时必将产生一个激活对象 love在激活对象身上   激活对象有作用域的关系 有办法访问  加一个函数提供外界访问
	this.name = name;
	this.age = age;
	this.say = function () {
		return love;
	};

	this.movelove = function (){
		love = '小轩'; //35
	}

} 

var g = new Girl('yinghong',22);

console.log(g);
console.log(g.say());//小明
console.log(g.movelove());//undefined  因为35行没有返回
console.log(g.say());//小轩



function fn(){
	function t(){
		//var age = 22;//声明age变量 在t的激活对象上
		age = 22;//赋值操作 t的激活对象上找age属性 ，找不到 找fn的激活对象....再找到 最终找到window.age = 22;
				//不加var就是操作window全局属性
	
	}
	t();
}
console.log(fn());//undefined
```

**<font style="color:rgb(44, 62, 80);">4. 静态成员</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">面向对象中的静态方法-静态属性：没有new对象 也能引用静态方法属性</font>

```plain
function Person(name){
    var age = 100;
    this.name = name;
}
//静态成员
Person.walk = function(){
    console.log('static');
};
Person.walk();  // static
```

**<font style="color:rgb(44, 62, 80);">5. 私有与公有</font>**

```plain
function Person(id){
    // 私有属性与方法
    var name = 'poetry';
    var work = function(){
        console.log(this.id);
    };
    //公有属性与方法
    this.id = id;
    this.say = function(){
        console.log('say hello');
        work.call(this);
    };
};
var p1 = new Person(123);
p1.name; // undefined
p1.id;  // 123
p1.say();  // say hello 123
```

**<font style="color:rgb(44, 62, 80);">6. 模块化</font>**

```plain
var moduleA;
moduleA = function() {
    var prop = 1;

    function func() {}

    return {
        func: func,
        prop: prop
    };
}(); // 立即执行匿名函数
```

**<font style="color:rgb(44, 62, 80);">7. 多态</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">多态:同一个父类继承出来的子类各有各的形态</font>

```plain
function Cat(){
	this.eat = '肉';
}

function Tiger(){
	this.color = '黑黄相间';
}

function Cheetah(){
	this.color = '报文';
}

function Lion(){
	this.color = '土黄色';
}

Tiger.prototype =  Cheetah.prototype = Lion.prototype = new Cat();//共享一个祖先 Cat

var T = new Tiger();
var C = new Cheetah();
var L = new Lion();

console.log(T.color);
console.log(C.color);
console.log(L.color);


console.log(T.eat);
console.log(C.eat);
console.log(L.eat);
```

**<font style="color:rgb(44, 62, 80);">8. 抽象类</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在构造器中</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">throw new Error('')</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">; 抛异常。这样防止这个类被直接调用</font>

```plain
function DetectorBase() {
    throw new Error('Abstract class can not be invoked directly!');
}

DetectorBase.prototype.detect = function() {
    console.log('Detection starting...');
};
DetectorBase.prototype.stop = function() {
    console.log('Detection stopped.');
};
DetectorBase.prototype.init = function() {
    throw new Error('Error');
};

// var d = new DetectorBase();
// Uncaught Error: Abstract class can not be invoked directly!

function LinkDetector() {}
LinkDetector.prototype = Object.create(DetectorBase.prototype);
LinkDetector.prototype.constructor = LinkDetector;

var l = new LinkDetector();
console.log(l); //LinkDetector {}__proto__: LinkDetector
l.detect(); //Detection starting...
l.init(); //Uncaught Error: Error
```

### [](https://www.123fe.net/docs/base/improve.html#_12-%E4%BA%8B%E4%BB%B6%E6%9C%BA%E5%88%B6)<font style="color:rgb(44, 62, 80);">12 事件机制</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">涉及面试题：事件的触发过程是怎么样的？知道什么是事件代理嘛？</font>

**<font style="color:rgb(44, 62, 80);">1. 简介</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">事件流是一个事件沿着特定数据结构传播的过程。冒泡和捕获是事件流在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中两种不同的传播方法</font>

**<font style="color:rgb(44, 62, 80);">事件流有三个阶段</font>**

+ <font style="color:rgb(44, 62, 80);">事件捕获阶段</font>
+ <font style="color:rgb(44, 62, 80);">处于目标阶段</font>
+ <font style="color:rgb(44, 62, 80);">事件冒泡阶段</font>

**<font style="color:rgb(44, 62, 80);">事件捕获</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">事件捕获（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event capturing</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）：通俗的理解就是，当鼠标点击或者触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">事件时，浏览器会从根节点开始由外到内进行事件传播，即点击了子元素，如果父元素通过事件捕获方式注册了对应的事件的话，会先触发父元素绑定的事件</font>

**<font style="color:rgb(44, 62, 80);">事件冒泡</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">事件冒泡（dubbed bubbling）：与事件捕获恰恰相反，事件冒泡顺序是由内到外进行事件传播，直到根节点</font>

<font style="color:rgb(44, 62, 80);">无论是事件捕获还是事件冒泡，它们都有一个共同的行为，就是事件传播</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781089815-536dfdb1-e7c2-4eb6-9767-3aa338db5b4c.png)

**<font style="color:rgb(44, 62, 80);">2. 捕获和冒泡</font>**

```html
<div id="div1">
  <div id="div2"></div>
</div>

<script>
    let div1 = document.getElementById('div1');
    let div2 = document.getElementById('div2');
    
    div1.onClick = function(){
        alert('1')
    }
    
    div2.onClick = function(){
        alert('2');
    }

</script>
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当点击</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div2</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">时，会弹出两个弹出框。在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ie8/9/10</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">chrome</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">浏览器，会先弹出”2”再弹出“1”，这就是事件冒泡：事件从最底层的节点向上冒泡传播。事件捕获则跟事件冒泡相反</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">W3C的标准是先捕获再冒泡，</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">addEventListener</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的第三个参数决定把事件注册在捕获（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）还是冒泡(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)</font>

**<font style="color:rgb(44, 62, 80);">3. 事件对象</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781089885-30f92068-1428-460a-a529-49da83fe15a2.png)

**<font style="color:rgb(44, 62, 80);">4. 事件流阻止</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在一些情况下需要阻止事件流的传播，阻止默认动作的发生</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event.preventDefault()</font><font style="color:rgb(44, 62, 80);">：取消事件对象的默认动作以及继续传播。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event.stopPropagation()/ event.cancelBubble = true</font><font style="color:rgb(44, 62, 80);">：阻止事件冒泡。</font>

**<font style="color:rgb(44, 62, 80);">事件的阻止在不同浏览器有不同处理</font>**

+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">下使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event.returnValue= false</font><font style="color:rgb(44, 62, 80);">，</font>
+ <font style="color:rgb(44, 62, 80);">在非</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">下则使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event.preventDefault()</font><font style="color:rgb(44, 62, 80);">进行阻止</font>

**<font style="color:rgb(44, 62, 80);">preventDefault与stopPropagation的区别</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">preventDefault</font><font style="color:rgb(44, 62, 80);">告诉浏览器不用执行与事件相关联的默认动作（如表单提交）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stopPropagation</font><font style="color:rgb(44, 62, 80);">是停止事件继续冒泡，但是对IE9以下的浏览器无效</font>

**<font style="color:rgb(44, 62, 80);">5. 事件注册</font>**

+ <font style="color:rgb(44, 62, 80);">通常我们使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">addEventListener</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">注册事件，该函数的第三个参数可以是布尔值，也可以是对象。对于布尔值</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useCapture</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">参数来说，该参数默认值为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">useCapture</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">决定了注册的事件是捕获事件还是冒泡事件</font>
+ <font style="color:rgb(44, 62, 80);">一般来说，我们只希望事件只触发在目标上，这时候可以使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stopPropagation</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来阻止事件的进一步传播。通常我们认为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stopPropagation</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是用来阻止事件冒泡的，其实该函数也可以阻止捕获事件。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stopImmediatePropagation</font><font style="color:rgb(44, 62, 80);">（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM3</font><font style="color:rgb(44, 62, 80);">级新增事件） 同样也能实现阻止事件冒泡和捕获，但是还能阻止该事件目标执行别的注册事件</font>

**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stopImmediatePropagation()</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(44, 62, 80);">和</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stopPropagation()</font>****<font style="color:rgb(44, 62, 80);">的区别在哪儿呢</font>**<font style="color:rgb(44, 62, 80);">？后者只会阻止冒泡或者是捕获。 但是前者除此之外还会阻止该元素的其他事件发生，但是后者就不会阻止其他事件的发生。</font>

```plain
node.addEventListener('click',(event) =>{
	event.stopImmediatePropagation()
	console.log('冒泡')
},false);
node.addEventListener('click',(event) => {
	console.log('冒泡2 ')
},false)
```

**<font style="color:rgb(44, 62, 80);">6. 事件委托</font>**

+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);">中性能优化的其中一个主要思想是减少</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">操作。</font>
+ <font style="color:rgb(44, 62, 80);">节省内存</font>
+ <font style="color:rgb(44, 62, 80);">不需要给子节点注销事件</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">假设有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">li</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，每个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">li</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">有相同的点击事件。如果为每</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">个Li</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">都添加事件，则会造成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">访问次数过多，引起浏览器重绘与重排的次数过多，性能则会降低。 使用事件委托则可以解决这样的问题</font>

**<font style="color:rgb(44, 62, 80);">原理</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">实现事件委托是利用了事件的冒泡原理实现的。当我们为最外层的节点添加点击事件，那么里面的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ul</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">li</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的点击事件都会冒泡到最外层节点上，委托它代为执行事件</font>

```html
<ul id="ul">
    <li>1</li>
    <li>2</li>
    <li>3</li>
</ul>
<script>
  window.onload = function(){
    var ulEle = document.getElementById('ul');
    ul.onclick = function(ev){
        //兼容IE
        ev = ev || window.event;
        var target = ev.target || ev.srcElement;
        
        if(target.nodeName.toLowerCase() == 'li'){
            alert( target.innerHTML);
        }
        
    }
  }
</script>
```

### [](https://www.123fe.net/docs/base/improve.html#_13-%E6%A8%A1%E5%9D%97%E5%8C%96)<font style="color:rgb(44, 62, 80);">13 模块化</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">js 中现在比较成熟的有四种模块加载方案：</font>

+ <font style="color:rgb(44, 62, 80);">第一种是 CommonJS 方案，它通过 require 来引入模块，通过 module.exports 定义模块的输出接口。这种模块加载方案是服务器端的解决方案，它是以同步的方式来引入模块的，因为在服务端文件都存储在本地磁盘，所以读取非常快，所以以同步的方式加载没有问题。但如果是在浏览器端，由于模块的加载是使用网络请求，因此使用异步加载的方式更加合适。</font>
+ <font style="color:rgb(44, 62, 80);">第二种是 AMD 方案，这种方案采用异步加载的方式来加载模块，模块的加载不影响后面语句的执行，所有依赖这个模块的语句都定义在一个回调函数里，等到加载完成后再执行回调函数。require.js 实现了 AMD 规范</font>
+ <font style="color:rgb(44, 62, 80);">第三种是 CMD 方案，这种方案和 AMD 方案都是为了解决异步模块加载的问题，sea.js 实现了 CMD 规范。它和require.js的区别在于模块定义时对依赖的处理不同和对依赖模块的执行时机的处理不同。</font>
+ <font style="color:rgb(44, 62, 80);">第四种方案是 ES6 提出的方案，使用 import 和 export 的形式来导入导出模块</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在有</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Babel</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的情况下，我们可以直接使用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的模块化</font>

```plain
// file a.js
export function a() {}
export function b() {}
// file b.js
export default function() {}

import {a, b} from './a.js'
import XXX from './b.js'
```

**<font style="color:rgb(44, 62, 80);">CommonJS</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CommonJs</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">独有的规范，浏览器中使用就需要用到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Browserify</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">解析了。</font>

```plain
// a.js
module.exports = {
    a: 1
}
// or
exports.a = 1

// b.js
var module = require('./a.js')
module.a // -> log 1
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在上述代码中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">module.exports</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">exports</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">很容易混淆，让我们来看看大致内部实现</font>

```plain
var module = require('./a.js')
module.a
// 这里其实就是包装了一层立即执行函数，这样就不会污染全局变量了，
// 重要的是 module 这里，module 是 Node 独有的一个变量
module.exports = {
    a: 1
}
// 基本实现
var module = {
  exports: {} // exports 就是个空对象
}
// 这个是为什么 exports 和 module.exports 用法相似的原因
var exports = module.exports
var load = function (module) {
    // 导出的东西
    var a = 1
    module.exports = a
    return module.exports
};
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">再来说说</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">module.exports</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">exports</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，用法其实是相似的，但是不能对</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">exports</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">直接赋值，不会有任何效果。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CommonJS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中的模块化的两者区别是：</font>

+ <font style="color:rgb(44, 62, 80);">前者支持动态导入，也就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">require(${path}/xx.js)</font><font style="color:rgb(44, 62, 80);">，后者目前不支持，但是已有提案,前者是同步导入，因为用于服务端，文件都在本地，同步导入即使卡住主线程影响也不大。</font>
+ <font style="color:rgb(44, 62, 80);">而后者是异步导入，因为用于浏览器，需要下载文件，如果也采用同步导入会对渲染有很大影响</font>
+ <font style="color:rgb(44, 62, 80);">前者在导出时都是值拷贝，就算导出的值变了，导入的值也不会改变，所以如果想更新值，必须重新导入一次。</font>
+ <font style="color:rgb(44, 62, 80);">但是后者采用实时绑定的方式，导入导出的值都指向同一个内存地址，所以导入值会跟随导出值变化</font>
+ <font style="color:rgb(44, 62, 80);">后者会编译成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">require/exports</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来执行的</font>

**<font style="color:rgb(44, 62, 80);">AMD</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AMD</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是由</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">RequireJS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">提出的</font>

**<font style="color:rgb(44, 62, 80);">AMD 和 CMD 规范的区别？</font>**

+ <font style="color:rgb(44, 62, 80);">第一个方面是在模块定义时对依赖的处理不同。AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块。而 CMD 推崇就近依赖，只有在用到某个模块的时候再去 require。</font>
+ <font style="color:rgb(44, 62, 80);">第二个方面是对依赖模块的执行时机处理不同。首先 AMD 和 CMD 对于模块的加载方式都是异步加载，不过它们的区别在于模块的执行时机，AMD 在依赖模块加载完成后就直接执行依赖模块，依赖模块的执行顺序和我们书写的顺序不一定一致。而 CMD在依赖模块加载完成后并不执行，只是下载而已，等到所有的依赖模块都加载好后，进入回调函数逻辑，遇到 require 语句的时候才执行对应的模块，这样模块的执行顺序就和我们书写的顺序保持一致了。</font>

```plain
// CMD
define(function(require, exports, module) {
  var a = require("./a");
  a.doSomething();
  // 此处略去 100 行
  var b = require("./b"); // 依赖可以就近书写
  b.doSomething();
  // ...
});

// AMD 默认推荐
define(["./a", "./b"], function(a, b) {
  // 依赖必须一开始就写好
  a.doSomething();
  // 此处略去 100 行
  b.doSomething();
  // ...
})
```

+ **<font style="color:rgb(44, 62, 80);">AMD</font>**<font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requirejs</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在推广过程中对模块定义的规范化产出，提前执行，推崇依赖前置</font>
+ **<font style="color:rgb(44, 62, 80);">CMD</font>**<font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">seajs</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在推广过程中对模块定义的规范化产出，延迟执行，推崇依赖就近</font>
+ **<font style="color:rgb(44, 62, 80);">CommonJs</font>**<font style="color:rgb(44, 62, 80);">：模块输出的是一个值的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">copy</font><font style="color:rgb(44, 62, 80);">，运行时加载，加载的是一个对象（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">module.exports</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性），该对象只有在脚本运行完才会生成</font>
+ **<font style="color:rgb(44, 62, 80);">ES6 Module</font>**<font style="color:rgb(44, 62, 80);">：模块输出的是一个值的引用，编译时输出接口，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(44, 62, 80);">模块不是对象，它对外接口只是一种静态定义，在代码静态解析阶段就会生成。</font>

**<font style="color:rgb(44, 62, 80);">谈谈对模块化开发的理解</font>**

+ <font style="color:rgb(44, 62, 80);">我对模块的理解是，一个模块是实现一个特定功能的一组方法。在最开始的时候，js 只实现一些简单的功能，所以并没有模块的概念，但随着程序越来越复杂，代码的模块化开发变得越来越重要。</font>
+ <font style="color:rgb(44, 62, 80);">由于函数具有独立作用域的特点，最原始的写法是使用函数来作为模块，几个函数作为一个模块，但是这种方式容易造成全局变量的污染，并且模块间没有联系。</font>
+ <font style="color:rgb(44, 62, 80);">后面提出了对象写法，通过将函数作为一个对象的方法来实现，这样解决了直接使用函数作为模块的一些缺点，但是这种办法会暴露所有的所有的模块成员，外部代码可以修改内部属性的值。</font>
+ <font style="color:rgb(44, 62, 80);">现在最常用的是立即执行函数的写法，通过利用闭包来实现模块私有作用域的建立，同时不会对全局作用域造成污染。</font>

### [](https://www.123fe.net/docs/base/improve.html#_14-iterator%E8%BF%AD%E4%BB%A3%E5%99%A8)<font style="color:rgb(44, 62, 80);">14 Iterator迭代器</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Iterator</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（迭代器）是一种接口，也可以说是一种规范。为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Iterator</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。</font>

<font style="color:rgb(44, 62, 80);">Iterator语法：</font>

```plain
const obj = {
    [Symbol.iterator]:function(){}
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[Symbol.iterator]</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性名是固定的写法，只要拥有了该属性的对象，就能够用迭代器的方式进行遍历。</font>

+ <font style="color:rgb(44, 62, 80);">迭代器的遍历方法是首先获得一个迭代器的指针，初始时该指针指向第一条数据之前，接着通过调用 next 方法，改变指针的指向，让其指向下一条数据</font>
+ <font style="color:rgb(44, 62, 80);">每一次的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">next</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">都会返回一个对象，该对象有两个属性</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">代表想要获取的数据</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">done</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">布尔值，false表示当前指针指向的数据有值，true表示遍历已经结束</font>

**<font style="color:rgb(44, 62, 80);">Iterator 的作用有三个：</font>**

+ <font style="color:rgb(44, 62, 80);">创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。</font>
+ <font style="color:rgb(44, 62, 80);">第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员。</font>
+ <font style="color:rgb(44, 62, 80);">第二次调用指针对象的next方法，指针就指向数据结构的第二个成员。</font>
+ <font style="color:rgb(44, 62, 80);">不断调用指针对象的next方法，直到它指向数据结构的结束位置。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">每一次调用next方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含value和done两个属性的对象。其中，value属性是当前成员的值，done属性是一个布尔值，表示遍历是否结束。</font>

```plain
let arr = [{num:1},2,3]
let it = arr[Symbol.iterator]() // 获取数组中的迭代器
console.log(it.next())  // { value: Object { num: 1 }, done: false }
console.log(it.next())  // { value: 2, done: false }
console.log(it.next())  // { value: 3, done: false }
console.log(it.next())  // { value: undefined, done: true }
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象没有布局Iterator接口，无法使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for of</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">遍历。下面使得对象具备Iterator接口</font>

+ <font style="color:rgb(44, 62, 80);">一个数据结构只要有Symbol.iterator属性，就可以认为是“可遍历的”</font>
+ <font style="color:rgb(44, 62, 80);">原型部署了Iterator接口的数据结构有三种，具体包含四种，分别是数组，类似数组的对象，Set和Map结构</font>

**<font style="color:rgb(44, 62, 80);">为什么对象（Object）没有部署Iterator接口呢？</font>**

+ <font style="color:rgb(44, 62, 80);">一是因为对象的哪个属性先遍历，哪个属性后遍历是不确定的，需要开发者手动指定。然而遍历遍历器是一种线性处理，对于非线性的数据结构，部署遍历器接口，就等于要部署一种线性转换</font>
+ <font style="color:rgb(44, 62, 80);">对对象部署</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Iterator</font><font style="color:rgb(44, 62, 80);">接口并不是很必要，因为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Map</font><font style="color:rgb(44, 62, 80);">弥补了它的缺陷，又正好有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Iteraotr</font><font style="color:rgb(44, 62, 80);">接口</font>

```plain
let obj = {
    id: '123',
    name: '张三',
    age: 18,
    gender: '男',
    hobbie: '睡觉'
}

obj[Symbol.iterator] = function () {
    let keyArr = Object.keys(obj)
    let index = 0
    return {
        next() {
            return index < keyArr.length ? {
                value: {
                    key: keyArr[index],
                    val: obj[keyArr[index++]]
                }
            } : {
                done: true
            }
        }
    }
}

for (let key of obj) {
  console.log(key)
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781089937-694176bb-6631-4a7f-b486-b11c72247062.png)

### [](https://www.123fe.net/docs/base/improve.html#_15-promise)<font style="color:rgb(44, 62, 80);">15 Promise</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这里你谈</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的时候，除了将他解决的痛点以及常用的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">API</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之外，最好进行拓展把</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eventloop</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">带进来好好讲一下，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">microtask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">(微任务)、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">macrotask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">(任务) 的执行顺序，如果看过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">源码，最好可以谈一谈 原生</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是如何实现的。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的关键点在于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callback</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的两个参数，一个是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resovle</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，一个是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reject</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。还有就是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的链式调用（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.then()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，每一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">then</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">都是一个责任人）</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">新增的语法，解决了回调地狱的问题。</font>
+ <font style="color:rgb(44, 62, 80);">可以把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">看成一个状态机。初始是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pending</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">状态，可以通过函数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resolve</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reject</font><font style="color:rgb(44, 62, 80);">，将状态转变为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resolved</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rejected</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">状态，状态一旦改变就不能再次变化。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">then</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数会返回一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例，并且该返回值是一个新的实例而不是之前的实例。因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规范规定除了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pending</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">状态，其他状态是不可以改变的，如果返回的是一个相同实例的话，多个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">then</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">调用就失去意义了。 对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">then</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来说，本质上可以把它看成是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flatMap</font>

**<font style="color:rgb(44, 62, 80);">1. Promise 的基本情况</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">简单来说它就是一个容器，里面保存着某个未来才会结束的事件（通常是异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息</font>

<font style="color:rgb(44, 62, 80);">一般 Promise 在执行过程中，必然会处于以下几种状态之一。</font>

+ <font style="color:rgb(44, 62, 80);">待定（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pending</font><font style="color:rgb(44, 62, 80);">）：初始状态，既没有被完成，也没有被拒绝。</font>
+ <font style="color:rgb(44, 62, 80);">已完成（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fulfilled</font><font style="color:rgb(44, 62, 80);">）：操作成功完成。</font>
+ <font style="color:rgb(44, 62, 80);">已拒绝（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rejected</font><font style="color:rgb(44, 62, 80);">）：操作失败。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">待定状态的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象执行的话，最后要么会通过一个值完成，要么会通过一个原因被拒绝。当其中一种情况发生时，我们用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">then</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法排列起来的相关处理程序就会被调用。因为最后</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.prototype.then</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.prototype.catch</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法返回的是一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">， 所以它们可以继续被链式调用</font>

<font style="color:rgb(44, 62, 80);">关于 Promise 的状态流转情况，有一点值得注意的是，内部状态改变之后不可逆，你需要在编程过程中加以注意。文字描述比较晦涩，我们直接通过一张图就能很清晰地看出 Promise 内部状态流转的情况</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781090063-71ed7756-3e5b-4731-aa52-b9aad58d838f.png)

<font style="color:rgb(44, 62, 80);">从上图可以看出，我们最开始创建一个新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">返回给</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，然后开始执行，状态是 pending，当执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resolve</font><font style="color:rgb(44, 62, 80);">之后状态就切换为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fulfilled</font><font style="color:rgb(44, 62, 80);">，执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reject</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之后就变为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rejected</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的状态</font>

**<font style="color:rgb(44, 62, 80);">2. Promise 的静态方法</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">all 方法</font>
    - <font style="color:rgb(44, 62, 80);">语法：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.all（iterable）</font>
    - <font style="color:rgb(44, 62, 80);">参数： 一个可迭代对象，如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array</font><font style="color:rgb(44, 62, 80);">。</font>
    - <font style="color:rgb(44, 62, 80);">描述： 此方法对于汇总多个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的结果很有用，在 ES6 中可以将多个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.all</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">异步请求并行操作，返回结果一般有下面两种情况。</font>
        * <font style="color:rgb(44, 62, 80);">当所有结果成功返回时按照请求顺序返回成功结果。</font>
        * <font style="color:rgb(44, 62, 80);">当其中有一个失败方法时，则进入失败方法</font>
+ <font style="color:rgb(44, 62, 80);">我们来看下业务的场景，对于下面这个业务场景页面的加载，将多个请求合并到一起，用 all 来实现可能效果会更好，请看代码片段</font>

```plain
// 在一个页面中需要加载获取轮播列表、获取店铺列表、获取分类列表这三个操作，页面需要同时发出请求进行页面渲染，这样用 `Promise.all` 来实现，看起来更清晰、一目了然。


//1.获取轮播数据列表
function getBannerList(){
  return new Promise((resolve,reject)=>{
      setTimeout(function(){
        resolve('轮播数据')
      },300) 
  })
}
//2.获取店铺列表
function getStoreList(){
  return new Promise((resolve,reject)=>{
    setTimeout(function(){
      resolve('店铺数据')
    },500)
  })
}
//3.获取分类列表
function getCategoryList(){
  return new Promise((resolve,reject)=>{
    setTimeout(function(){
      resolve('分类数据')
    },700)
  })
}
function initLoad(){ 
  Promise.all([getBannerList(),getStoreList(),getCategoryList()])
  .then(res=>{
    console.log(res) 
  }).catch(err=>{
    console.log(err)
  })
} 
initLoad()
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">allSettled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.allSettled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的语法及参数跟</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.all</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类似，其参数接受一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的数组，返回一个新的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">唯一的不同在于，执行完之后不会失败</font><font style="color:rgb(44, 62, 80);">，也就是说当</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.allSettled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">全部处理完成后，我们可以拿到每个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的状态，而不管其是否处理成功</font>
+ <font style="color:rgb(44, 62, 80);">我们来看一下用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">allSettled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实现的一段代码</font>

```plain
const resolved = Promise.resolve(2);
const rejected = Promise.reject(-1);
const allSettledPromise = Promise.allSettled([resolved, rejected]);
allSettledPromise.then(function (results) {
  console.log(results);
});
// 返回结果：
// [
//    { status: 'fulfilled', value: 2 },
//    { status: 'rejected', reason: -1 }
// ]
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从上面代码中可以看到，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.allSettled</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">最后返回的是一个数组，记录传进来的参数中每个 Promise 的返回值，这就是和 all 方法不太一样的地方。</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">any</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法</font>
    - <font style="color:rgb(44, 62, 80);">语法：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.any（iterable）</font>
    - <font style="color:rgb(44, 62, 80);">参数：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">iterable</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可迭代的对象，例如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array</font><font style="color:rgb(44, 62, 80);">。</font>
    - <font style="color:rgb(44, 62, 80);">描述：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">any</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法返回一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">，只要参数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例有一个变成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fulfilled</font><font style="color:rgb(44, 62, 80);">状态，最后</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">any</font><font style="color:rgb(44, 62, 80);">返回的实例就会变成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fulfilled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">状态；如果所有参数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例都变成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rejected</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">状态，包装实例就会变成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rejected</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">状态。</font>

```plain
const resolved = Promise.resolve(2);
const rejected = Promise.reject(-1);
const anyPromise = Promise.any([resolved, rejected]);
anyPromise.then(function (results) {
  console.log(results);
});
// 返回结果：
// 2
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从改造后的代码中可以看出，只要其中一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">变成</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fulfilled</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">状态，那么</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">any</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">最后就返回这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p romise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。由于上面</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resolved</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个 Promise 已经是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resolve</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的了，故最后返回结果为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">2</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">race</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法</font>
    - <font style="color:rgb(44, 62, 80);">语法：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.race（iterable）</font>
    - <font style="color:rgb(44, 62, 80);">参数：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">iterable</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可迭代的对象，例如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array</font><font style="color:rgb(44, 62, 80);">。</font>
    - <font style="color:rgb(44, 62, 80);">描述：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">race</font><font style="color:rgb(44, 62, 80);">方法返回一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">，只要参数的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之中有一个实例率先改变状态，则</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">race</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的返回状态就跟着改变。那个率先改变的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例的返回值，就传递给</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">race</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的回调函数</font>
+ <font style="color:rgb(44, 62, 80);">我们来看一下这个业务场景，对于图片的加载，特别适合用 race 方法来解决，将图片请求和超时判断放到一起，用 race 来实现图片的超时判断。请看代码片段。</font>

```plain
//请求某个图片资源
function requestImg(){
  var p = new Promise(function(resolve, reject){
    var img = new Image();
    img.onload = function(){ resolve(img); }
    img.src = 'http://www.baidu.com/img/flexible/logo/pc/result.png';
  });
  return p;
}
//延时函数，用于给请求计时
function timeout(){
  var p = new Promise(function(resolve, reject){
    setTimeout(function(){ reject('图片请求超时'); }, 5000);
  });
  return p;
}
Promise.race([requestImg(), timeout()])
.then(function(results){
  console.log(results);
})
.catch(function(reason){
  console.log(reason);
});


// 从上面的代码中可以看出，采用 Promise 的方式来判断图片是否加载成功，也是针对 Promise.race 方法的一个比较好的业务场景
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781090794-8fca82cf-36a6-4858-a119-a3d46059e9c0.png)

**<font style="color:rgb(44, 62, 80);">promise手写实现，面试够用版：</font>**

```plain
function myPromise(constructor){
    let self=this;
    self.status="pending" //定义状态改变前的初始状态
    self.value=undefined;//定义状态为resolved的时候的状态
    self.reason=undefined;//定义状态为rejected的时候的状态
    function resolve(value){
        //两个==="pending"，保证了状态的改变是不可逆的
       if(self.status==="pending"){
          self.value=value;
          self.status="resolved";
       }
    }
    function reject(reason){
        //两个==="pending"，保证了状态的改变是不可逆的
       if(self.status==="pending"){
          self.reason=reason;
          self.status="rejected";
       }
    }
    //捕获构造异常
    try{
       constructor(resolve,reject);
    }catch(e){
       reject(e);
    }
}
// 定义链式调用的then方法
myPromise.prototype.then=function(onFullfilled,onRejected){
   let self=this;
   switch(self.status){
      case "resolved":
        onFullfilled(self.value);
        break;
      case "rejected":
        onRejected(self.reason);
        break;
      default:       
   }
}
```

### [](https://www.123fe.net/docs/base/improve.html#_16-generator)<font style="color:rgb(44, 62, 80);">16 Generator</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Generator</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中新增的语法，和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">一样，都可以用来异步编程。Generator函数可以说是Iterator接口的具体实现方式。Generator 最大的特点就是可以控制函数的执行。</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function*</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">用来声明一个函数是生成器函数，它比普通的函数声明多了一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">*</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">*</font><font style="color:rgb(44, 62, 80);">的位置比较随意可以挨着</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">关键字，也可以挨着函数名</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">yield</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">产出的意思，这个关键字只能出现在生成器函数体内，但是生成器中也可以没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">yield</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">关键字，函数遇到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">yield</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的时候会暂停，并把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">yield</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">后面的表达式结果抛出去</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">next</font><font style="color:rgb(44, 62, 80);">作用是将代码的控制权交还给生成器函数</font>

```plain
function *foo(x) {
  let y = 2 * (yield (x + 1))
  let z = yield (y / 3)
  return (x + y + z)
}
let it = foo(5)
console.log(it.next())   // => {value: 6, done: false}
console.log(it.next(12)) // => {value: 8, done: false}
console.log(it.next(13)) // => {value: 42, done: true}
```

<font style="color:rgb(44, 62, 80);">上面这个示例就是一个Generator函数，我们来分析其执行过程：</font>

+ <font style="color:rgb(44, 62, 80);">首先 Generator 函数调用时它会返回一个迭代器</font>
+ <font style="color:rgb(44, 62, 80);">当执行第一次 next 时，传参会被忽略，并且函数暂停在 yield (x + 1) 处，所以返回 5 + 1 = 6</font>
+ <font style="color:rgb(44, 62, 80);">当执行第二次 next 时，传入的参数等于上一个 yield 的返回值，如果你不传参，yield 永远返回 undefined。此时 let y = 2 * 12，所以第二个 yield 等于 2 * 12 / 3 = 8</font>
+ <font style="color:rgb(44, 62, 80);">当执行第三次 next 时，传入的参数会传递给 z，所以 z = 13, x = 5, y = 24，相加等于 42</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">yield</font><font style="color:rgb(44, 62, 80);">实际就是暂缓执行的标示，每执行一次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">next()</font><font style="color:rgb(44, 62, 80);">，相当于指针移动到下一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">yield</font><font style="color:rgb(44, 62, 80);">位置</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781090289-7de01575-ee2c-4a5a-a4a8-9915e8486148.png)

**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">总结一下</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Generator</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">提供的一种异步编程解决方案。通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">yield</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">标识位和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">next()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法调用，实现函数的分段执行</font>

**<font style="color:rgb(44, 62, 80);">遍历器对象生成函数，最大的特点是可以交出函数的执行权</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">关键字与函数名之间有一个星号；</font>
+ <font style="color:rgb(44, 62, 80);">函数体内部使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">yield</font><font style="color:rgb(44, 62, 80);">表达式，定义不同的内部状态；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">next</font><font style="color:rgb(44, 62, 80);">指针移向下一个状态</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这里你可以说说</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Generator</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的异步编程，以及它的语法糖</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">async</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">awiat</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，传统的异步编程。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之前，异步编程大致如下</font>

+ <font style="color:rgb(44, 62, 80);">回调函数</font>
+ <font style="color:rgb(44, 62, 80);">事件监听</font>
+ <font style="color:rgb(44, 62, 80);">发布/订阅</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">传统异步编程方案之一：协程，多个线程互相协作，完成异步任务。</font>

```plain
// 使用 * 表示这是一个 Generator 函数
// 内部可以通过 yield 暂停代码
// 通过调用 next 恢复执行
function* test() {
  let a = 1 + 2;
  yield 2;
  yield 3;
}
let b = test();
console.log(b.next()); // >  { value: 2, done: false }
console.log(b.next()); // >  { value: 3, done: false }
console.log(b.next()); // >  { value: undefined, done: true }
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从以上代码可以发现，加上</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">*</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的函数执行后拥有了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">next</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数，也就是说函数执行后返回了一个对象。每次调用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">next</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数可以继续执行被暂停的代码。以下是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Generator</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数的简单实现</font>

```plain
// cb 也就是编译过的 test 函数
function generator(cb) {
  return (function() {
    var object = {
      next: 0,
      stop: function() {}
    };

    return {
      next: function() {
        var ret = cb(object);
        if (ret === undefined) return { value: undefined, done: true };
        return {
          value: ret,
          done: false
        };
      }
    };
  })();
}
// 如果你使用 babel 编译后可以发现 test 函数变成了这样
function test() {
  var a;
  return generator(function(_context) {
    while (1) {
      switch ((_context.prev = _context.next)) {
        // 可以发现通过 yield 将代码分割成几块
        // 每次执行 next 函数就执行一块代码
        // 并且表明下次需要执行哪块代码
        case 0:
          a = 1 + 2;
          _context.next = 4;
          return 2;
        case 4:
          _context.next = 6;
          return 3;
		// 执行完毕
        case 6:
        case "end":
          return _context.stop();
      }
    }
  });
}
```

### [](https://www.123fe.net/docs/base/improve.html#_17-async-await)<font style="color:rgb(44, 62, 80);">17 async/await</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Generator</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数的语法糖。有更好的语义、更好的适用性、返回值是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

+ <font style="color:rgb(44, 62, 80);">await 和 promise 一样，更多的是考笔试题，当然偶尔也会问到和 promise 的一些区别。</font>
+ <font style="color:rgb(44, 62, 80);">await 相比直接使用 Promise 来说，优势在于处理 then 的调用链，能够更清晰准确的写出代码。缺点在于滥用 await 可能会导致性能问题，因为 await 会阻塞代码，也许之后的异步代码并不依赖于前者，但仍然需要等待前者完成，导致代码失去了并发性，此时更应该使用 Promise.all。</font>
+ <font style="color:rgb(44, 62, 80);">一个函数如果加上 async ，那么该函数就会返回一个 Promise</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">async => *</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">await => yield</font>

```plain
// 基本用法

async function timeout (ms) {
  await new Promise((resolve) => {
    setTimeout(resolve, ms)    
  })
}
async function asyncConsole (value, ms) {
  await timeout(ms)
  console.log(value)
}
asyncConsole('hello async and await', 1000)
```

<font style="color:rgb(44, 62, 80);">下面来看一个使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">await</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的代码。</font>

```plain
var a = 0
var b = async () => {
  a = a + await 10
  console.log('2', a) // -> '2' 10
  a = (await 10) + a
  console.log('3', a) // -> '3' 20
}
b()
a++
console.log('1', a) // -> '1' 1
```

+ <font style="color:rgb(44, 62, 80);">首先函数</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">b</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">先执行，在执行到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">await 10</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之前变量</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">还是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">，因为在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">await</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内部实现了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">generators</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">generators</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会保留堆栈中东西，所以这时候</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a = 0</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">被保存了下来</font>
+ <font style="color:rgb(44, 62, 80);">因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">await</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是异步操作，遇到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">await</font><font style="color:rgb(44, 62, 80);">就会立即返回一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pending</font><font style="color:rgb(44, 62, 80);">状态的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">对象，暂时返回执行代码的控制权，使得函数外的代码得以继续执行，所以会先执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">console.log('1', a)</font>
+ <font style="color:rgb(44, 62, 80);">这时候同步代码执行完毕，开始执行异步代码，将保存下来的值拿出来使用，这时候</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a = 10</font>
+ <font style="color:rgb(44, 62, 80);">然后后面就是常规执行代码了</font>

**<font style="color:rgb(44, 62, 80);">优缺点：</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">async/await</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的优势在于处理 then 的调用链，能够更清晰准确的写出代码，并且也能优雅地解决回调地狱问题。当然也存在一些缺点，因为 await 将异步代码改造成了同步代码，如果多个异步代码没有依赖性却使用了 await 会导致性能上的降低。</font>

**<font style="color:rgb(44, 62, 80);">async原理</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">async/await</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">语法糖就是使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Generator</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">函数+自动执行器来运作的</font>

```plain
// 定义了一个promise，用来模拟异步请求，作用是传入参数++
function getNum(num){
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(num+1)
        }, 1000)
    })
}

//自动执行器，如果一个Generator函数没有执行完，则递归调用
function asyncFun(func){
  var gen = func();

  function next(data){
    var result = gen.next(data);
    if (result.done) return result.value;
    result.value.then(function(data){
      next(data);
    });
  }

  next();
}

// 所需要执行的Generator函数，内部的数据在执行完成一步的promise之后，再调用下一步
var func = function* (){
  var f1 = yield getNum(1);
  var f2 = yield getNum(f1);
  console.log(f2) ;
};
asyncFun(func);
```

+ <font style="color:rgb(44, 62, 80);">在执行的过程中，判断一个函数的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(44, 62, 80);">是否完成，如果已经完成，将结果传入下一个函数，继续重复此步骤</font>
+ <font style="color:rgb(44, 62, 80);">每一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">next()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法返回值的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性为一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象，所以我们为其添加</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">then</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法， 在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">then</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法里面接着运行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">next</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法挪移遍历器指针，直到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Generator</font><font style="color:rgb(44, 62, 80);">函数运行完成</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781091567-7d064f2e-fe95-4458-9bad-f18e35e85160.png)

### [](https://www.123fe.net/docs/base/improve.html#_18-%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF)<font style="color:rgb(44, 62, 80);">18 事件循环</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781091483-714cc301-a136-4b59-b8e6-e4d4439890b3.png)

+ <font style="color:rgb(44, 62, 80);">默认代码从上到下执行，执行环境通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);">来执行（宏任务）</font>
+ <font style="color:rgb(44, 62, 80);">在代码执行过程中，调用定时器</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">click</font><font style="color:rgb(44, 62, 80);">事件...不会立即执行，需要等待当前代码全部执行完毕</font>
+ <font style="color:rgb(44, 62, 80);">给异步方法划分队列，分别存放到微任务（立即存放）和宏任务（时间到了或事情发生了才存放）到队列中</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);">执行完毕后，会清空所有的微任务</font>
+ <font style="color:rgb(44, 62, 80);">微任务执行完毕后，会渲染页面（不是每次都调用）</font>
+ <font style="color:rgb(44, 62, 80);">再去宏任务队列中看有没有到达时间的，拿出来其中一个执行</font>
+ <font style="color:rgb(44, 62, 80);">执行完毕后，按照上述步骤不停的循环</font>

**<font style="color:rgb(44, 62, 80);">例子</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781091655-f4e9dfa4-4d5e-49e5-a864-0a0ac179c44d.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781091551-02d9db93-ebc7-445a-84d6-91f9659e80bf.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">自动执行的情况 会输出 listener1 listener2 task1 task2</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781092738-17db7204-0efa-42ff-b477-df18f894810b.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781094143-f22d550d-6692-47da-b012-c727bf023d29.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如果手动点击click 会一个宏任务取出来一个个执行，先执行click的宏任务，取出微任务去执行。会输出 listener1 task1 listener2 task2</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781094176-a449fa56-81fa-4f60-ac34-062a09cc8633.png)

```plain
console.log(1)

async function asyncFunc(){
  console.log(2)
  // await xx ==> promise.resolve(()=>{console.log(3)}).then()
  // console.log(3) 放到promise.resolve或立即执行
  await console.log(3) 
  // 相当于把console.log(4)放到了then promise.resolve(()=>{console.log(3)}).then(()=>{
  //   console.log(4)
  // })
  // 微任务谁先注册谁先执行
  console.log(4)
}

setTimeout(()=>{console.log(5)})

const promise = new Promise((resolve,reject)=>{
  console.log(6)
  resolve(7)
})

promise.then(d=>{console.log(d)})

asyncFunc()

console.log(8)

// 输出 1 6 2 3 8 7 4 5
```

**<font style="color:rgb(44, 62, 80);">1. 浏览器事件循环</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">涉及面试题：异步代码执行顺序？解释一下什么是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Event Loop</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">？</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">js代码执行过程中会有很多任务，这些任务总的分成两类：</font>

+ <font style="color:rgb(44, 62, 80);">同步任务</font>
+ <font style="color:rgb(44, 62, 80);">异步任务</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当我们打开网站时，网页的渲染过程就是一大堆同步任务，比如页面骨架和页面元素的渲染。而像加载图片音乐之类占用资源大耗时久的任务，就是异步任务。，我们用导图来说明：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781095230-483f912b-9fae-428e-b7f2-d87c7d2a2f5b.png)

**<font style="color:rgb(44, 62, 80);">我们解释一下这张图：</font>**

+ <font style="color:rgb(44, 62, 80);">同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。</font>
+ <font style="color:rgb(44, 62, 80);">当指定的事情完成时，Event Table会将这个函数移入Event Queue。</font>
+ <font style="color:rgb(44, 62, 80);">主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。</font>
+ <font style="color:rgb(44, 62, 80);">上述过程会不断重复，也就是常说的Event Loop(事件循环)。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">那主线程执行栈何时为空呢？js引擎存在monitoring process进程，会持续不断的检查主线程执行栈是否为空，一旦为空，就会去Event Queue那里检查是否有等待被调用的函数</font>

<font style="color:rgb(44, 62, 80);">以上就是js运行的整体流程</font>

**<font style="color:rgb(44, 62, 80);">面试中该如何回答呢？ 下面是我个人推荐的回答：</font>**

+ <font style="color:rgb(44, 62, 80);">首先js 是单线程运行的，在代码执行的时候，通过将不同函数的执行上下文压入执行栈中来保证代码的有序执行</font>
+ <font style="color:rgb(44, 62, 80);">在执行同步代码的时候，如果遇到了异步事件，js 引擎并不会一直等待其返回结果，而是会将这个事件挂起，继续执行执行栈中的其他任务</font>
+ <font style="color:rgb(44, 62, 80);">当同步事件执行完毕后，再将异步事件对应的回调加入到与当前执行栈中不同的另一个任务队列中等待执行</font>
+ <font style="color:rgb(44, 62, 80);">任务队列可以分为宏任务对列和微任务对列，当当前执行栈中的事件执行完毕后，js 引擎首先会判断微任务对列中是否有任务可以执行，如果有就将微任务队首的事件压入栈中执行</font>
+ <font style="color:rgb(44, 62, 80);">当微任务对列中的任务都执行完成后再去判断宏任务对列中的任务。</font>

```plain
setTimeout(function() {
  console.log(1)
}, 0);
new Promise(function(resolve, reject) {
  console.log(2);
  resolve()
}).then(function() {
  console.log(3)
});
process.nextTick(function () {
  console.log(4)
})
console.log(5)
```

+ <font style="color:rgb(44, 62, 80);">第一轮：主线程开始执行，遇到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">，将setTimeout的回调函数丢到宏任务队列中，在往下执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new Promise</font><font style="color:rgb(44, 62, 80);">立即执行，输出2，then的回调函数丢到微任务队列中，再继续执行，遇到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(44, 62, 80);">，同样将回调函数扔到微任务队列，再继续执行，输出5，当所有同步任务执行完成后看有没有可以执行的微任务，发现有then函数和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nextTick</font><font style="color:rgb(44, 62, 80);">两个微任务，先执行哪个呢？</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(44, 62, 80);">指定的异步任务总是发生在所有异步任务之前，因此先执行process.nextTick输出4然后执行then函数输出3，第一轮执行结束。</font>
+ <font style="color:rgb(44, 62, 80);">第二轮：从宏任务队列开始，发现setTimeout回调，输出1执行完毕，因此结果是25431</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在执行的过程中会产生执行环境，这些执行环境会被顺序的加入到执行栈中。如果遇到异步的代码，会被挂起并加入到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Task</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（有多种</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">task</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">） 队列中。一旦执行栈为空，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Event</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Loop</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就会从</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Task</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">队列中拿出需要执行的代码并放入执行栈中执行，所以本质上来说</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中的异步还是同步行为</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781097179-19815dd7-213d-4ada-a7e0-b483336e5a6b.png)

```plain
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

console.log('script end');
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不同的任务源会被分配到不同的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Task</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">队列中，任务源可以分为 微任务（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">microtask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">） 和 宏任务（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">macrotask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）。在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">规范中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">microtask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">称为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jobs</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">macrotask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">称为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">task</font>

```javascript
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

new Promise((resolve) => {
    console.log('Promise')
    resolve()
}).then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});

console.log('script end');
// script start => Promise => script end => promise1 => promise2 => setTimeout
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以上代码虽然</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">写在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之前，但是因为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属于微任务而</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属于宏任务</font>

**<font style="color:rgb(44, 62, 80);">微任务</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.observe</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MutationObserver</font>
    - ![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781097954-417aa605-b4c5-4bac-b559-801c5095f8b7.png)

**<font style="color:rgb(44, 62, 80);">宏任务</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">I/O</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">网络请求完成、文件读写完成事件</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI rendering</font>
+ <font style="color:rgb(44, 62, 80);">用户交互事件（比如鼠标点击、滚动页面、放大缩小等）</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">宏任务中包括了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，浏览器会先执行一个宏任务，接下来有异步代码的话就先执行微任务</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781098244-95bce182-a31e-45d0-adce-62d60714194c.png)

**<font style="color:rgb(44, 62, 80);">所以正确的一次 Event loop 顺序是这样的</font>**

+ <font style="color:rgb(44, 62, 80);">执行同步代码，这属于宏任务</font>
+ <font style="color:rgb(44, 62, 80);">执行栈为空，查询是否有微任务需要执行</font>
+ <font style="color:rgb(44, 62, 80);">执行所有微任务</font>
+ <font style="color:rgb(44, 62, 80);">必要的话渲染 UI</font>
+ <font style="color:rgb(44, 62, 80);">然后开始下一轮</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Event loop</font><font style="color:rgb(44, 62, 80);">，执行宏任务中的异步代码</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">通过上述的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Event loop</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">顺序可知，如果宏任务中的异步代码有大量的计算并且需要操作</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的话，为了更快的响应界面响应，我们可以把操作</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">放入微任务中</font>

+ <font style="color:rgb(44, 62, 80);">JavaScript 引擎首先从宏任务队列（macrotask queue）中取出第一个任务</font>
+ <font style="color:rgb(44, 62, 80);">执行完毕后，再将微任务（microtask queue）中的所有任务取出，按照顺序分别全部执行（这里包括不仅指开始执行时队列里的微任务），如果在这一步过程中产生新的微任务，也需要执行；</font>
+ <font style="color:rgb(44, 62, 80);">然后再从宏任务队列中取下一个，执行完毕后，再次将 microtask queue 中的全部取出，循环往复，直到两个 queue 中的任务都取完。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781100038-4c067392-2dbd-4a18-b526-4339f47da084.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">总结起来就是：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">一次 Eventloop 循环会处理一个宏任务和所有这次循环中产生的微任务</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

**<font style="color:rgb(44, 62, 80);">2. Node 中的 Event loop</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当 Node.js 开始启动时，会初始化一个 Eventloop，处理输入的代码脚本，这些脚本会进行 API 异步调用，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法会开始处理事件循环。下面就是 Node.js 官网提供的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Eventloop</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">事件循环参考流程</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Event loop</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和浏览器中的不相同。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Event loop</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">分为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">6</font><font style="color:rgb(44, 62, 80);">个阶段，它们会按照顺序反复运行</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781100121-5fe1ae86-fbbc-4303-a732-55897725ce67.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781100119-79bbe270-d519-44a7-ad5c-850b93598e84.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781100066-a5725dc5-773b-465c-8219-fcab43c6bfed.png)

+ <font style="color:rgb(44, 62, 80);">每次执行执行一个宏任务后会清空微任务（执行顺序和浏览器一致，在node11版本以上）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">node中的微任务，当前执行栈的底部，优先级比</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(44, 62, 80);">要高</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">整个流程分为六个阶段，当这六个阶段执行完一次之后，才可以算得上执行了一次 Eventloop 的循环过程。我们来分别看下这六个阶段都做了哪些事情。</font>

+ **<font style="color:rgb(44, 62, 80);">Timers 阶段</font>**<font style="color:rgb(44, 62, 80);">：这个阶段执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);">的回调函数，简单理解就是由这两个函数启动的回调函数。</font>
+ **<font style="color:rgb(44, 62, 80);">I/O callbacks 阶段</font>**<font style="color:rgb(44, 62, 80);">：这个阶段主要执行系统级别的回调函数，比如 TCP 连接失败的回调。</font>
+ **<font style="color:rgb(44, 62, 80);">idle，prepare 阶段</font>**<font style="color:rgb(44, 62, 80);">：仅系统内部使用，你只需要知道有这 2 个阶段就可以。</font>
+ **<font style="color:rgb(44, 62, 80);">poll 阶段</font>**<font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">poll</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">阶段是一个重要且复杂的阶段，几乎所有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">I/O</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">相关的回调，都在这个阶段执行（除了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以及一些因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">exception</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">意外关闭产生的回调）。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">检索新的 I/O 事件，执行与 I/O 相关的回调</font><font style="color:rgb(44, 62, 80);">，其他情况 Node.js 将在适当的时候在此阻塞。这也是最复杂的一个阶段，所有的事件循环以及回调处理都在这个阶段执行。这个阶段的主要流程如下图所示。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781101232-76319320-6f89-4b9c-8396-cf61a7a2bf85.png)

+ **<font style="color:rgb(44, 62, 80);">check 阶段</font>**<font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">回调函数在这里执行，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">并不是立马执行，而是当事件循环</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">poll 中没有新的事件处理时就执行该部分</font><font style="color:rgb(44, 62, 80);">，如下代码所示。</font>

```plain
const fs = require('fs');
setTimeout(() => { // 新的事件循环的起点
    console.log('1'); 
}, 0);
setImmediate( () => {
    console.log('setImmediate 1');
});
/// fs.readFile 将会在 poll 阶段执行
fs.readFile('./test.conf', {encoding: 'utf-8'}, (err, data) => {
    if (err) throw err;
    console.log('read file success');
});
/// 该部分将会在首次事件循环中执行
Promise.resolve().then(()=>{
    console.log('poll callback');
});
// 首次事件循环执行
console.log('2');
```

<font style="color:rgb(44, 62, 80);">在这一代码中有一个非常奇特的地方，就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之后输出。有以下几点原因：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如果不设置时间或者设置时间为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，则会默认为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1ms</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">主流程执行完成后，超过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1ms</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">时，会将</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">回调函数逻辑插入到待执行回调函数</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">poll</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">队列中；</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">由于当前</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">poll</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">队列中存在可执行回调函数，因此需要先执行完，待完全执行完成后，才会执行</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">check：setImmediate</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">因此这也验证了这句话，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">先执行回调函数，再执行 setImmediate</font>

+ **<font style="color:rgb(44, 62, 80);">close callbacks 阶段</font>**<font style="color:rgb(44, 62, 80);">：执行一些关闭的回调函数，如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">socket.on('close', ...)</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">除了把 Eventloop 的宏任务细分到不同阶段外。node 还引入了一个新的任务队列</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Process.nextTick()</font>

<font style="color:rgb(44, 62, 80);">可以认为，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Process.nextTick()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会在上述各个阶段结束时，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">进入下一个阶段之前立即执行</font><font style="color:rgb(44, 62, 80);">（优先级甚至超过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">microtask</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">队列）</font>

**<font style="color:rgb(44, 62, 80);">事件循环的主要包含微任务和宏任务。具体是怎么进行循环的呢</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781101767-f70f7123-de7c-4d11-bd6d-20282541fbe7.png)

+ **<font style="color:rgb(44, 62, 80);">微任务</font>**<font style="color:rgb(44, 62, 80);">：在 Node.js 中微任务包含 2 种——</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">微任务在事件循环中优先级是最高的</font><font style="color:rgb(44, 62, 80);">，因此在同一个事件循环中有其他任务存在时，优先执行微任务队列。并且</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick 和 Promise</font><font style="color:rgb(44, 62, 80);">也存在优先级，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">高于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font>
+ **<font style="color:rgb(44, 62, 80);">宏任务</font>**<font style="color:rgb(44, 62, 80);">：在 Node.js 中宏任务包含 4 种——</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">I/O</font><font style="color:rgb(44, 62, 80);">。宏任务在微任务执行之后执行，因此在同一个事件循环周期内，如果既存在微任务队列又存在宏任务队列，那么优先将微任务队列清空，再执行宏任务队列</font>

<font style="color:rgb(44, 62, 80);">我们可以看到有一个核心的主线程，它的执行阶段主要处理三个核心逻辑。</font>

+ <font style="color:rgb(44, 62, 80);">同步代码。</font>
+ <font style="color:rgb(44, 62, 80);">将异步任务插入到微任务队列或者宏任务队列中。</font>
+ <font style="color:rgb(44, 62, 80);">执行微任务或者宏任务的回调函数。在主线程处理回调函数的同时，也需要判断是否插入微任务和宏任务。根据优先级，先判断微任务队列是否存在任务，存在则先执行微任务，不存在则判断在宏任务队列是否有任务，有则执行。</font>

```plain
const fs = require('fs');
// 首次事件循环执行
console.log('start');
/// 将会在新的事件循环中的阶段执行
fs.readFile('./test.conf', {encoding: 'utf-8'}, (err, data) => {
    if (err) throw err;
    console.log('read file success');
});
setTimeout(() => { // 新的事件循环的起点
    console.log('setTimeout'); 
}, 0);
/// 该部分将会在首次事件循环中执行
Promise.resolve().then(()=>{
    console.log('Promise callback');
});
/// 执行 process.nextTick
process.nextTick(() => {
    console.log('nextTick callback');
});
// 首次事件循环执行
console.log('end');
```

<font style="color:rgb(44, 62, 80);">分析下上面代码的执行过程</font>

+ <font style="color:rgb(44, 62, 80);">第一个事件循环主线程发起，因此先执行同步代码，所以先输出 start，然后输出 end</font>
+ <font style="color:rgb(44, 62, 80);">第一个事件循环主线程发起，因此先执行同步代码，所以先输出 start，然后输出 end；</font>
+ <font style="color:rgb(44, 62, 80);">再从上往下分析，遇到微任务，插入微任务队列，遇到宏任务，插入宏任务队列，分析完成后，微任务队列包含：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.resolve 和 process.nextTick</font><font style="color:rgb(44, 62, 80);">，宏任务队列包含：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fs.readFile 和 setTimeout</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">先执行微任务队列，但是根据优先级，先执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick 再执行 Promise.resolve</font><font style="color:rgb(44, 62, 80);">，所以先输出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nextTick callback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">再输出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise callback</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">再执行宏任务队列，根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">宏任务插入先后顺序执行 setTimeout 再执行 fs.readFile</font><font style="color:rgb(44, 62, 80);">，这里需要注意，先执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">由于其回调时间较短，因此回调也先执行，并非是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">先执行所以才先执行回调函数，但是它执行需要时间肯定大于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1ms</font><font style="color:rgb(44, 62, 80);">，所以虽然</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fs.readFile</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">先于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">执行，但是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">执行更快，所以先输出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，最后输出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">read file success</font><font style="color:rgb(44, 62, 80);">。</font>

```plain
// 输出结果
start
end
nextTick callback
Promise callback
setTimeout
read file success
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781103071-cf5665a6-2522-40c8-9169-c97a8e39deb4.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当微任务和宏任务又产生新的微任务和宏任务时，又应该如何处理呢？如下代码所示：</font>

```plain
const fs = require('fs');
setTimeout(() => { // 新的事件循环的起点
    console.log('1'); 
    fs.readFile('./config/test.conf', {encoding: 'utf-8'}, (err, data) => {
        if (err) throw err;
        console.log('read file sync success');
    });
}, 0);
/// 回调将会在新的事件循环之前
fs.readFile('./config/test.conf', {encoding: 'utf-8'}, (err, data) => {
    if (err) throw err;
    console.log('read file success');
});
/// 该部分将会在首次事件循环中执行
Promise.resolve().then(()=>{
    console.log('poll callback');
});
// 首次事件循环执行
console.log('2');
```

<font style="color:rgb(44, 62, 80);">在上面代码中，有 2 个宏任务和 1 个微任务，宏任务是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout 和 fs.readFile</font><font style="color:rgb(44, 62, 80);">，微任务是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.resolve</font><font style="color:rgb(44, 62, 80);">。</font>

+ <font style="color:rgb(44, 62, 80);">整个过程优先执行主线程的第一个事件循环过程，所以先执行同步逻辑，先输出 2。</font>
+ <font style="color:rgb(44, 62, 80);">接下来执行微任务，输出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">poll callback</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">再执行宏任务中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fs.readFile 和 setTimeout</font><font style="color:rgb(44, 62, 80);">，由于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fs.readFile</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">优先级高，先执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fs.readFile</font><font style="color:rgb(44, 62, 80);">。但是处理时间长于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1ms</font><font style="color:rgb(44, 62, 80);">，因此会先执行</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的回调函数，输出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">。这个阶段在执行过程中又会产生新的宏任务</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fs.readFile</font><font style="color:rgb(44, 62, 80);">，因此又将该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fs.readFile 插入宏任务队列</font>
+ <font style="color:rgb(44, 62, 80);">最后由于只剩下宏任务了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fs.readFile</font><font style="color:rgb(44, 62, 80);">，因此执行该宏任务，并等待处理完成后的回调，输出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">read file sync success</font><font style="color:rgb(44, 62, 80);">。</font>

```plain
// 结果
2
poll callback
1
read file success
read file sync success
```

**<font style="color:rgb(44, 62, 80);">Process.nextick() 和 Vue 的 nextick</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781102940-f44fcf5f-8d9b-491f-baf5-c57fbc067b89.png)

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node.js</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和浏览器端宏任务队列的另一个很重要的不同点是，浏览器端任务队列每轮事件循环仅出队一个回调函数接着去执行微任务队列；而</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node.js</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">端只要轮到执行某个宏任务队列，则会执行完队列中所有的当前任务，但是当前轮次新添加到队尾的任务则会等到下一轮次才会执行。</font>

```plain
setTimeout(() => {
    console.log('setTimeout');
}, 0);
setImmediate(() => {
    console.log('setImmediate');
})
// 这里可能会输出 setTimeout，setImmediate
// 可能也会相反的输出，这取决于性能
// 因为可能进入 event loop 用了不到 1 毫秒，这时候会执行 setImmediate
// 否则会执行 setTimeout
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上面介绍的都是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">macrotask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的执行情况，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">microtask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会在以上每个阶段完成后立即执行</font>

```plain
setTimeout(()=>{
    console.log('timer1')

    Promise.resolve().then(function() {
        console.log('promise1')
    })
}, 0)

setTimeout(()=>{
    console.log('timer2')

    Promise.resolve().then(function() {
        console.log('promise2')
    })
}, 0)

// 以上代码在浏览器和 node 中打印情况是不同的
// 浏览器中一定打印 timer1, promise1, timer2, promise2
// node 中可能打印 timer1, timer2, promise1, promise2
// 也可能打印 timer1, promise1, timer2, promise2
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会先于其他</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">microtask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">执行</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781104563-6df157ff-323f-43e4-b132-9af40e80e779.png)

```plain
setTimeout(() => {
 console.log("timer1");

 Promise.resolve().then(function() {
   console.log("promise1");
 });
}, 0);

// poll阶段执行
fs.readFile('./test',()=>{
  // 在poll阶段里面 如果有setImmediate优先执行，setTimeout处于事件循环顶端 poll下面就是setImmediate
  setTimeout(()=>console.log('setTimeout'),0)
  setImmediate(()=>console.log('setImmediate'),0)
})

process.nextTick(() => {
 console.log("nextTick");
});
// nextTick, timer1, promise1,setImmediate,setTimeout
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">microtask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来说，它会在以上每个阶段完成前清空</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">microtask</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">队列，下图中的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Tick</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就代表了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">microtask</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781103873-30e85c9d-1a04-4e57-a5dd-063e4dfab439.png)

**<font style="color:rgb(44, 62, 80);">谁来启动这个循环过程，循环条件是什么？</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当 Node.js 启动后，会初始化事件循环，处理已提供的输入脚本，它可能会先调用一些异步的 API、调度定时器，或者</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">process.nextTick()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，然后再开始处理事件循环。因此可以这样理解，Node.js 进程启动后，就发起了一个新的事件循环，也就是事件循环的起点。</font>

<font style="color:rgb(44, 62, 80);">总结来说，Node.js 事件循环的发起点有 4 个：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node.js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">启动后；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">回调函数；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">回调函数；</font>
+ <font style="color:rgb(44, 62, 80);">也可能是一次</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">I/O</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">后的回调函数。</font>

**<font style="color:rgb(44, 62, 80);">无限循环有没有终点</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当所有的微任务和宏任务都清空的时候，虽然当前没有任务可执行了，但是也并不能代表循环结束了。因为可能存在当前还未回调的异步 I/O，所以这个循环是没有终点的，只要进程在，并且有新的任务存在，就会去执行</font>

**<font style="color:rgb(44, 62, 80);">Node.js 是单线程的还是多线程的？</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">主线程是单线程执行的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，但是 Node.js</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">存在多线程执行</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，多线程包括</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout 和异步 I/O 事件</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。其实 Node.js 还存在其他的线程，包括</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">垃圾回收、内存优化</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">等</font>

**<font style="color:rgb(44, 62, 80);">EventLoop 对渲染的影响</font>**

+ <font style="color:rgb(44, 62, 80);">想必你之前在业务开发中也遇到过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdlecallback 和 requestAnimationFrame</font><font style="color:rgb(44, 62, 80);">，这两个函数在我们之前的内容中没有讲过，但是当你开始考虑它们在 Eventloop 的生命周期的哪一步触发，或者这两个方法的回调会在微任务队列还是宏任务队列执行的时候，才发现好像没有想象中那么简单。这两个方法其实也并不属于 JS 的原生方法，而是浏览器宿主环境提供的方法，因为它们牵扯到另一个问题：渲染。</font>
+ <font style="color:rgb(44, 62, 80);">我们知道浏览器作为一个复杂的应用是多线程工作的，除了运行 JS 的线程外，还有渲染线程、定时器触发线程、HTTP 请求线程，等等。JS 线程可以读取并且修改 DOM，而渲染线程也需要读取 DOM，这是一个典型的多线程竞争临界资源的问题。所以浏览器就把这两个线程设计成互斥的，即同时只能有一个线程在执行</font>
+ <font style="color:rgb(44, 62, 80);">渲染原本就不应该出现在 Eventloop 相关的知识体系里，但是因为 Eventloop 显然是在讨论 JS 如何运行的问题，而渲染则是浏览器另外一个线程的工作。但是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font><font style="color:rgb(44, 62, 80);">的出现却把这两件事情给关联起来</font>
+ <font style="color:rgb(44, 62, 80);">通过调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">我们可以在下次渲染之前执行回调函数。那下次渲染具体是哪个时间点呢？渲染和 Eventloop 有什么关系呢？</font>
    - <font style="color:rgb(44, 62, 80);">简单来说，就是在每一次</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Eventloop</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的末尾，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">判断当前页面是否处于渲染时机，就是重新渲染</font>
+ <font style="color:rgb(44, 62, 80);">有屏幕的硬件限制，比如 60Hz 刷新率，简而言之就是 1 秒刷新了 60 次，16.6ms 刷新一次。这个时候浏览器的渲染间隔时间就没必要小于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">16.6ms</font><font style="color:rgb(44, 62, 80);">，因为就算渲染了屏幕上也看不到。当然浏览器也不能保证一定会每 16.6ms 会渲染一次，因为还会受到处理器的性能、JavaScript 执行效率等其他因素影响。</font>
+ <font style="color:rgb(44, 62, 80);">回到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font><font style="color:rgb(44, 62, 80);">，这个 API 保证在下次浏览器渲染之前一定会被调用，实际上我们完全可以把它看成是一个高级版的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font><font style="color:rgb(44, 62, 80);">。它们都是在一段时间后执行回调，但是前者的间隔时间是由浏览器自己不断调整的，而后者只能由用户指定。这样的特性也决定了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">更适合用来做针对每一帧来修改的动画效果</font>
+ <font style="color:rgb(44, 62, 80);">当然</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Eventloop</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里的宏任务，或者说它并不在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Eventloop</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的生命周期里，只是浏览器又开放的一个在渲染之前发生的新的 hook。另外需要注意的是微任务的认知概念也需要更新，在执行 animation callback 时也有可能产生微任务（比如 promise 的 callback），会放到 animation queue 处理完后再执行。所以微任务并不是像之前说的那样在每一轮 Eventloop 后处理，而是在 JS 的函数调用栈清空后处理</font>

<font style="color:rgb(44, 62, 80);">但是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdlecallback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">却是一个更好理解的概念。当宏任务队列中没有任务可以处理时，浏览器可能存在“空闲状态”。这段空闲时间可以被</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdlecallback</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">利用起来执行一些优先级不高、不必立即执行的任务，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781104403-7d173fad-20f3-46a5-898e-948bf0441311.png)

### [](https://www.123fe.net/docs/base/improve.html#_19-%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6)<font style="color:rgb(44, 62, 80);">19 垃圾回收</font>
+ <font style="color:rgb(44, 62, 80);">对于在JavaScript中的字符串，对象，数组是没有固定大小的，只有当对他们进行动态分配存储时，解释器就会分配内存来存储这些数据，当JavaScript的解释器消耗完系统中所有可用的内存时，就会造成系统崩溃。</font>
+ <font style="color:rgb(44, 62, 80);">内存泄漏，在某些情况下，不再使用到的变量所占用内存没有及时释放，导致程序运行中，内存越占越大，极端情况下可以导致系统崩溃，服务器宕机。</font>
+ <font style="color:rgb(44, 62, 80);">JavaScript有自己的一套垃圾回收机制，JavaScript的解释器可以检测到什么时候程序不再使用这个对象了（数据），就会把它所占用的内存释放掉。</font>
+ <font style="color:rgb(44, 62, 80);">针对JavaScript的来及回收机制有以下两种方法（常用）：标记清除，引用计数</font>
+ <font style="color:rgb(44, 62, 80);">标记清除</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">v8 的垃圾回收机制基于分代回收机制，这个机制又基于世代假说，这个假说有两个特点，一是新生的对象容易早死，另一个是不死的对象会活得更久。基于这个假说，v8 引擎将内存分为了新生代和老生代。</font>

+ <font style="color:rgb(44, 62, 80);">新创建的对象或者只经历过一次的垃圾回收的对象被称为新生代。经历过多次垃圾回收的对象被称为老生代。</font>
+ <font style="color:rgb(44, 62, 80);">新生代被分为 From 和 To 两个空间，To 一般是闲置的。当 From 空间满了的时候会执行 Scavenge 算法进行垃圾回收。当我们执行垃圾回收算法的时候应用逻辑将会停止，等垃圾回收结束后再继续执行。</font>

**<font style="color:rgb(44, 62, 80);">这个算法分为三步：</font>**

+ <font style="color:rgb(44, 62, 80);">首先检查 From 空间的存活对象，如果对象存活则判断对象是否满足晋升到老生代的条件，如果满足条件则晋升到老生代。如果不满足条件则移动 To 空间。</font>
+ <font style="color:rgb(44, 62, 80);">如果对象不存活，则释放对象的空间。</font>
+ <font style="color:rgb(44, 62, 80);">最后将 From 空间和 To 空间角色进行交换。</font>

**<font style="color:rgb(44, 62, 80);">新生代对象晋升到老生代有两个条件：</font>**

+ <font style="color:rgb(44, 62, 80);">第一个是判断是对象否已经经过一次 Scavenge 回收。若经历过，则将对象从 From 空间复制到老生代中；若没有经历，则复制到 To 空间。</font>
+ <font style="color:rgb(44, 62, 80);">第二个是 To 空间的内存使用占比是否超过限制。当对象从 From 空间复制到 To 空间时，若 To 空间使用超过 25%，则对象直接晋升到老生代中。设置 25% 的原因主要是因为算法结束后，两个空间结束后会交换位置，如果 To 空间的内存太小，会影响后续的内存分配。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">老生代采用了标记清除法和标记压缩法。标记清除法首先会对内存中存活的对象进行标记，标记结束后清除掉那些没有标记的对象。由于标记清除后会造成很多的内存碎片，不便于后面的内存分配。所以了解决内存碎片的问题引入了标记压缩法。</font>

<font style="color:rgb(44, 62, 80);">由于在进行垃圾回收的时候会暂停应用的逻辑，对于新生代方法由于内存小，每次停顿的时间不会太长，但对于老生代来说每次垃圾回收的时间长，停顿会造成很大的影响。 为了解决这个问题 V8 引入了增量标记的方法，将一次停顿进行的过程分为了多步，每次执行完一小步就让运行逻辑执行一会，就这样交替运行</font>

### [](https://www.123fe.net/docs/base/improve.html#_20-%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2)<font style="color:rgb(44, 62, 80);">20 内存泄露</font>
+ <font style="color:rgb(44, 62, 80);">意外的全局变量: 无法被回收</font>
+ <font style="color:rgb(44, 62, 80);">定时器: 未被正确关闭，导致所引用的外部变量无法被释放</font>
+ <font style="color:rgb(44, 62, 80);">事件监听: 没有正确销毁 (低版本浏览器可能出现)</font>
+ <font style="color:rgb(44, 62, 80);">闭包</font>
    - <font style="color:rgb(44, 62, 80);">第一种情况是我们由于使用未声明的变量，而意外的创建了一个全局变量，而使这个变量一直留在内存中无法被回收。</font>
    - <font style="color:rgb(44, 62, 80);">第二种情况是我们设置了setInterval定时器，而忘记取消它，如果循环函数有对外部变量的引用的话，那么这个变量会被一直留在内存中，而无法被回收。</font>
    - <font style="color:rgb(44, 62, 80);">第三种情况是我们获取一个DOM元素的引用，而后面这个元素被删除，由于我们一直保留了对这个元素的引用，所以它也无法被回收。</font>
    - <font style="color:rgb(44, 62, 80);">第四种情况是不合理的使用闭包，从而导致某些变量一直被留在内存当中。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">引用:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素被删除时，内存中的引用未被正确清空</font>
+ <font style="color:rgb(44, 62, 80);">控制台</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">console.log</font><font style="color:rgb(44, 62, 80);">打印的东西</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">chrome</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">timeline</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">进行内存标记，可视化查看内存的变化情况，找出异常点。</font>

[内存泄露排查方法(opens new window)](https://juejin.cn/post/6947841638118998029?utm_source=gold_browser_extension)

### [](https://www.123fe.net/docs/base/improve.html#_21-%E6%B7%B1%E6%B5%85%E6%8B%B7%E8%B4%9D)<font style="color:rgb(44, 62, 80);">21 深浅拷贝</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781106397-5def51db-5976-41bf-bc2a-293435e1c024.png)

**<font style="color:rgb(44, 62, 80);">1. 浅拷贝的原理和实现</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">自己创建一个新的对象，来接受你要重新复制或引用的对象值。如果对象属性是基本的数据类型，复制的就是基本类型的值给新对象；但如果属性是引用数据类型，复制的就是内存中的地址，如果其中一个对象改变了这个内存中的地址，肯定会影响到另一个对象</font>

**<font style="color:rgb(44, 62, 80);">方法一：object.assign</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object.assign</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是 ES6 中</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的一个方法，该方法可以用于 JS 对象的合并等多个用途，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">其中一个用途就是可以进行浅拷贝</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。该方法的第一个参数是拷贝的目标对象，后面的参数是拷贝的来源对象（也可以是多个来源）。</font>

```plain
object.assign 的语法为：Object.assign(target, ...sources)
```

<font style="color:rgb(44, 62, 80);">object.assign 的示例代码如下：</font>

```plain
let target = {};
let source = { a: { b: 1 } };
Object.assign(target, source);
console.log(target); // { a: { b: 1 } };
```

**<font style="color:rgb(44, 62, 80);">但是使用 object.assign 方法有几点需要注意</font>**

+ <font style="color:rgb(44, 62, 80);">它不会拷贝对象的继承属性；</font>
+ <font style="color:rgb(44, 62, 80);">它不会拷贝对象的不可枚举的属性；</font>
+ <font style="color:rgb(44, 62, 80);">可以拷贝</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型的属性。</font>

```plain
let obj1 = { a:{ b:1 }, sym:Symbol(1)}; 
Object.defineProperty(obj1, 'innumerable' ,{
    value:'不可枚举属性',
    enumerable:false
});
let obj2 = {};
Object.assign(obj2,obj1)
obj1.a.b = 2;
console.log('obj1',obj1);
console.log('obj2',obj2);
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781107040-4d97818f-0442-4a94-bee2-2d45dc72bf64.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从上面的样例代码中可以看到，利用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object.assign</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">也可以拷贝</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">类型的对象，但是如果到了对象的第二层属性 obj1.a.b 这里的时候，前者值的改变也会影响后者的第二层属性的值，说明其中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">依旧存在着访问共同堆内存的问题</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，也就是说</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">这种方法还不能进一步复制，而只是完成了浅拷贝的功能</font>

**<font style="color:rgb(44, 62, 80);">方法二：扩展运算符方式</font>**

+ <font style="color:rgb(44, 62, 80);">我们也可以利用 JS 的扩展运算符，在构造对象的同时完成浅拷贝的功能。</font>
+ <font style="color:rgb(44, 62, 80);">扩展运算符的语法为：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let cloneObj = { ...obj };</font>

```plain
/* 对象的拷贝 */
let obj = {a:1,b:{c:1}}
let obj2 = {...obj}
obj.a = 2
console.log(obj)  //{a:2,b:{c:1}} console.log(obj2); //{a:1,b:{c:1}}
obj.b.c = 2
console.log(obj)  //{a:2,b:{c:2}} console.log(obj2); //{a:1,b:{c:2}}
/* 数组的拷贝 */
let arr = [1, 2, 3];
let newArr = [...arr]; //跟arr.slice()是一样的效果
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">扩展运算符 和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object.assign</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">有同样的缺陷，也就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">实现的浅拷贝的功能差不多</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，但是如果属性都是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">基本类型的值，使用扩展运算符进行浅拷贝会更加方便</font>

**<font style="color:rgb(44, 62, 80);">方法三：concat 拷贝数组</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">数组的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">concat</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法其实也是浅拷贝，所以连接一个含有引用类型的数组时，需要注意修改原数组中的元素的属性，因为它会影响拷贝之后连接的数组。不过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">concat</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">只能用于数组的浅拷贝，使用场景比较局限。代码如下所示。</font>

```plain
let arr = [1, 2, 3];
let newArr = arr.concat();
newArr[1] = 100;
console.log(arr);  // [ 1, 2, 3 ]
console.log(newArr); // [ 1, 100, 3 ]
```

**<font style="color:rgb(44, 62, 80);">方法四：slice 拷贝数组</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">slice</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法也比较有局限性，因为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">它仅仅针对数组类型</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">slice方法会返回一个新的数组对象</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，这一对象由该方法的前两个参数来决定原数组截取的开始和结束时间，是不会影响和改变原始数组的。</font>

```plain
slice 的语法为：arr.slice(begin, end);
```

```plain
let arr = [1, 2, {val: 4}];
let newArr = arr.slice();
newArr[2].val = 1000;
console.log(arr);  //[ 1, 2, { val: 1000 } ]
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从上面的代码中可以看出，这就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">浅拷贝的限制所在了——它只能拷贝一层对象</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。如果</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">存在对象的嵌套，那么浅拷贝将无能为力</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。因此深拷贝就是为了解决这个问题而生的，它能解决多层对象嵌套问题，彻底实现拷贝</font>

**<font style="color:rgb(44, 62, 80);">手工实现一个浅拷贝</font>**

<font style="color:rgb(44, 62, 80);">根据以上对浅拷贝的理解，如果让你自己实现一个浅拷贝，大致的思路分为两点：</font>

+ <font style="color:rgb(44, 62, 80);">对基础类型做一个最基本的一个拷贝；</font>
+ <font style="color:rgb(44, 62, 80);">对引用类型开辟一个新的存储，并且拷贝一层对象属性。</font>

```plain
const shallowClone = (target) => {
  if (typeof target === 'object' && target !== null) {
    const cloneTarget = Array.isArray(target) ? []: {};
    for (let prop in target) {
      if (target.hasOwnProperty(prop)) {
          cloneTarget[prop] = target[prop];
      }
    }
    return cloneTarget;
  } else {
    return target;
  }
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">利用类型判断，针对引用类型的对象进行 for 循环遍历对象属性赋值给目标对象的属性，基本就可以手工实现一个浅拷贝的代码了</font>

**<font style="color:rgb(44, 62, 80);">2. 深拷贝的原理和实现</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">浅拷贝只是创建了一个新的对象，复制了原有对象的基本类型的值，而引用数据类型只拷贝了一层属性，再深层的还是无法进行拷贝</font><font style="color:rgb(44, 62, 80);">。深拷贝则不同，对于复杂引用数据类型，其在堆内存中完全开辟了一块内存地址，并将原有的对象完全复制过来存放。</font>

<font style="color:rgb(44, 62, 80);">这两个对象是相互独立、不受影响的，彻底实现了内存上的分离。总的来说，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">深拷贝的原理可以总结如下</font><font style="color:rgb(44, 62, 80);">：</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">将一个对象从内存中完整地拷贝出来一份给目标对象，并从堆内存中开辟一个全新的空间存放新对象，且新对象的修改并不会改变原对象，二者实现真正的分离。</font>

**<font style="color:rgb(44, 62, 80);">方法一：乞丐版（JSON.stringify）</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON.stringify()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是目前开发过程中最简单的深拷贝方法，其实就是把一个对象序列化成为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的字符串，并将对象里面的内容转换成字符串，最后再用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON.parse()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的方法将</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">字符串生成一个新的对象</font>

```plain
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE
```

**<font style="color:rgb(44, 62, 80);">但是该方法也是有局限性的</font>**<font style="color:rgb(44, 62, 80);">：</font>

+ <font style="color:rgb(44, 62, 80);">会忽略</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font>
+ <font style="color:rgb(44, 62, 80);">会忽略</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">symbol</font>
+ <font style="color:rgb(44, 62, 80);">不能序列化函数</font>
+ <font style="color:rgb(44, 62, 80);">无法拷贝不可枚举的属性</font>
+ <font style="color:rgb(44, 62, 80);">无法拷贝对象的原型链</font>
+ <font style="color:rgb(44, 62, 80);">拷贝</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">RegExp</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">引用类型会变成空对象</font>
+ <font style="color:rgb(44, 62, 80);">拷贝</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Date</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">引用类型会变成字符串</font>
+ <font style="color:rgb(44, 62, 80);">对象中含有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NaN</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Infinity</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以及</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">-Infinity</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">序列化的结果会变成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font>
+ <font style="color:rgb(44, 62, 80);">不能解决循环引用的对象，即对象成环 (</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj[key] = obj</font><font style="color:rgb(44, 62, 80);">)。</font>

```plain
function Obj() { 
  this.func = function () { alert(1) }; 
  this.obj = {a:1};
  this.arr = [1,2,3];
  this.und = undefined; 
  this.reg = /123/; 
  this.date = new Date(0); 
  this.NaN = NaN;
  this.infinity = Infinity;
  this.sym = Symbol(1);
} 
let obj1 = new Obj();
Object.defineProperty(obj1,'innumerable',{ 
  enumerable:false,
  value:'innumerable'
});
console.log('obj1',obj1);
let str = JSON.stringify(obj1);
let obj2 = JSON.parse(str);
console.log('obj2',obj2);
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781106972-b818d356-db27-458d-9511-dc7cbe8c00a3.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">使用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON.stringify</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法实现深拷贝对象，虽然到目前为止还有很多无法实现的功能，但是这种方法足以满足日常的开发需求，并且是最简单和快捷的。而对于其他的也要实现深拷贝的，比较麻烦的属性对应的数据类型，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON.stringify</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">暂时还是无法满足的，那么就需要下面的几种方法了</font>

**<font style="color:rgb(44, 62, 80);">方法二：基础版（手写递归实现）</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">下面是一个实现 deepClone 函数封装的例子，通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for in</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">遍历传入参数的属性值，如果值是引用类型则再次递归调用该函数，如果是基础数据类型就直接复制</font>

```plain
let obj1 = {
  a:{
    b:1
  }
}
function deepClone(obj) { 
  let cloneObj = {}
  for(let key in obj) {                 //遍历
    if(typeof obj[key] ==='object') { 
      cloneObj[key] = deepClone(obj[key])  //是对象就再次调用该函数递归
    } else {
      cloneObj[key] = obj[key]  //基本类型的话直接复制值
    }
  }
  return cloneObj
}
let obj2 = deepClone(obj1);
obj1.a.b = 2;
console.log(obj2);   //  {a:{b:1}}
```

<font style="color:rgb(44, 62, 80);">虽然利用递归能实现一个深拷贝，但是同上面的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON.stringify</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一样，还是有一些问题没有完全解决，例如：</font>

+ <font style="color:rgb(44, 62, 80);">这个深拷贝函数并不能复制不可枚举的属性以及</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型；</font>
+ <font style="color:rgb(44, 62, 80);">这种方法</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">只是针对普通的引用类型的值做递归复制</font><font style="color:rgb(44, 62, 80);">，而对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array、Date、RegExp、Error、Function</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这样的引用类型并不能正确地拷贝；</font>
+ <font style="color:rgb(44, 62, 80);">对象的属性里面成环，即</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">循环引用没有解决</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">这种基础版本的写法也比较简单，可以应对大部分的应用情况。但是你在面试的过程中，如果只能写出这样的一个有缺陷的深拷贝方法，有可能不会通过。</font>

<font style="color:rgb(44, 62, 80);">所以为了“拯救”这些缺陷，下面我带你一起看看改进的版本，以便于你可以在面试种呈现出更好的深拷贝方法，赢得面试官的青睐。</font>

**<font style="color:rgb(44, 62, 80);">方法三：改进版（改进后递归实现）</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">针对上面几个待解决问题，我先通过四点相关的理论告诉你分别应该怎么做。</font>

+ <font style="color:rgb(44, 62, 80);">针对能够遍历对象的不可枚举属性以及</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型，我们可以使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Reflect.ownKeys</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法；</font>
+ <font style="color:rgb(44, 62, 80);">当参数为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Date、RegExp</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型，则直接生成一个新的实例返回；</font>
+ <font style="color:rgb(44, 62, 80);">利用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getOwnPropertyDescriptors</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法可以获得对象的所有属性，以及对应的特性，顺便结合</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.create</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法创建一个新对象，并继承传入原对象的原型链；</font>
+ <font style="color:rgb(44, 62, 80);">利用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WeakMap</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">类型作为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Hash</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">表，因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WeakMap</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是弱引用类型，可以有效防止内存泄漏（你可以关注一下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Map</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">weakMap</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的关键区别，这里要用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">weakMap</font><font style="color:rgb(44, 62, 80);">），作为检测循环引用很有帮助，如果存在循环，则引用直接返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WeakMap</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">存储的值</font>

<font style="color:rgb(44, 62, 80);">如果你在考虑到循环引用的问题之后，还能用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WeakMap</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来很好地解决，并且向面试官解释这样做的目的，那么你所展示的代码，以及你对问题思考的全面性，在面试官眼中应该算是合格的了</font>

**<font style="color:rgb(44, 62, 80);">实现深拷贝</font>**

```javascript
const isComplexDataType = obj => (typeof obj === 'object' || typeof obj === 'function') && (obj !== null)

const deepClone = function (obj, hash = new WeakMap()) {
  if (obj.constructor === Date) {
    return new Date(obj)       // 日期对象直接返回一个新的日期对象
  }

  if (obj.constructor === RegExp){
    return new RegExp(obj)     //正则对象直接返回一个新的正则对象
  }

  //如果循环引用了就用 weakMap 来解决
  if (hash.has(obj)) {
    return hash.get(obj)
  }
  let allDesc = Object.getOwnPropertyDescriptors(obj)

  //遍历传入参数所有键的特性
  let cloneObj = Object.create(Object.getPrototypeOf(obj), allDesc)

  // 把cloneObj原型复制到obj上
  hash.set(obj, cloneObj)

  for (let key of Reflect.ownKeys(obj)) { 
    cloneObj[key] = (isComplexDataType(obj[key]) && typeof obj[key] !== 'function') ? deepClone(obj[key], hash) : obj[key]
  }
  return cloneObj
}
```

```javascript
// 下面是验证代码
let obj = {
  num: 0,
  str: '',
  boolean: true,
  unf: undefined,
  nul: null,
  obj: { name: '我是一个对象', id: 1 },
  arr: [0, 1, 2],
  func: function () { console.log('我是一个函数') },
  date: new Date(0),
  reg: new RegExp('/我是一个正则/ig'),
  [Symbol('1')]: 1,
};
Object.defineProperty(obj, 'innumerable', {
  enumerable: false, value: '不可枚举属性' }
                     );
obj = Object.create(obj, Object.getOwnPropertyDescriptors(obj))
obj.loop = obj    // 设置loop成循环引用的属性
let cloneObj = deepClone(obj)
cloneObj.arr.push(4)
console.log('obj', obj)
console.log('cloneObj', cloneObj)
```

<font style="color:rgb(44, 62, 80);">我们看一下结果，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cloneObj</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的基础上进行了一次深拷贝，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cloneObj</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数组进行了修改，并未影响到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj.arr</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的变化，如下图所示</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781106977-1d22a350-a0a3-45f6-9e41-0ccc14af960c.png)

### [](https://www.123fe.net/docs/base/improve.html#_22-%E8%8A%82%E6%B5%81%E4%B8%8E%E9%98%B2%E6%8A%96)<font style="color:rgb(44, 62, 80);">22 节流与防抖</font>
+ <font style="color:rgb(44, 62, 80);">函数防抖 是指在事件被触发 n 秒后再执行回调，如果在这 n 秒内事件又被触发，则重新计时。这可以使用在一些点击请求的事件上，避免因为用户的多次点击向后端发送多次请求。</font>
+ <font style="color:rgb(44, 62, 80);">函数节流 是指规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。节流可以使用在 scroll 函数的事件监听上，通过事件节流来降低事件调用的频率。</font>

```javascript
// 函数防抖的实现
function debounce(fn, wait) {
  var timer = null;

  return function() {
    var context = this,
      args = arguments;

    // 如果此时存在定时器的话，则取消之前的定时器重新记时
    if (timer) {
      clearTimeout(timer);
      timer = null;
    }

    // 设置定时器，使事件间隔指定事件后执行
    timer = setTimeout(() => {
      fn.apply(context, args);
    }, wait);
  };
}

// 函数节流的实现;
function throttle(fn, delay) {
  var preTime = Date.now();

  return function() {
    var context = this,
      args = arguments,
      nowTime = Date.now();

    // 如果两次时间间隔超过了指定时间，则执行函数。
    if (nowTime - preTime >= delay) {
      preTime = Date.now();
      return fn.apply(context, args);
    }
  };
}
```

### [](https://www.123fe.net/docs/base/improve.html#_23-proxy%E4%BB%A3%E7%90%86)<font style="color:rgb(44, 62, 80);">23 Proxy代理</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">proxy在目标对象的外层搭建了一层拦截，外界对目标对象的某些操作，必须通过这层拦截</font>

```plain
var proxy = new Proxy(target, handler);
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new Proxy()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">表示生成一个Proxy实例，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">target</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">参数表示所要拦截的目标对象，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">handler</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">参数也是一个对象，用来定制拦截行为</font>

```javascript
var target = {
  name: 'poetries'
};
var logHandler = {
  get: function(target, key) {
    console.log(`${key} 被读取`);
    return target[key];
  },
  set: function(target, key, value) {
    console.log(`${key} 被设置为 ${value}`);
    target[key] = value;
  }
}
var targetWithLog = new Proxy(target, logHandler);

targetWithLog.name; // 控制台输出：name 被读取
targetWithLog.name = 'others'; // 控制台输出：name 被设置为 others

console.log(target.name); // 控制台输出: others
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">targetWithLog</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">读取属性的值时，实际上执行的是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">logHandler.get</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">：在控制台输出信息，并且读取被代理对象</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">target</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的属性。</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">targetWithLog</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">设置属性值时，实际上执行的是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">logHandler.set</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">：在控制台输出信息，并且设置被代理对象</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">target</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的属性的值</font>

```javascript
// 由于拦截函数总是返回35，所以访问任何属性都得到35
var proxy = new Proxy({}, {
  get: function(target, property) {
    return 35;
  }
});

proxy.time // 35
proxy.name // 35
proxy.title // 35
```

**<font style="color:rgb(44, 62, 80);">Proxy 实例也可以作为其他对象的原型对象</font>**

```javascript
var proxy = new Proxy({}, {
  get: function(target, property) {
    return 35;
  }
});

let obj = Object.create(proxy);
obj.time // 35
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">proxy</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象的原型，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象本身并没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">time</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性，所以根据原型链，会在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">proxy</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象上读取该属性，导致被拦截</font>

**<font style="color:rgb(44, 62, 80);">Proxy的作用</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对于代理模式</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的作用主要体现在三个方面</font>

+ <font style="color:rgb(44, 62, 80);">拦截和监视外部对对象的访问</font>
+ <font style="color:rgb(44, 62, 80);">降低函数或类的复杂度</font>
+ <font style="color:rgb(44, 62, 80);">在复杂操作前对操作进行校验或对所需资源进行管理</font>

**<font style="color:rgb(44, 62, 80);">Proxy所能代理的范围--handler</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">实际上 handler 本身就是ES6所新设计的一个对象.它的作用就是用来 自定义代理对象的各种可代理操作 。它本身一共有13中方法,每种方法都可以代理一种操作.其13种方法如下</font>

```javascript
// 在读取代理对象的原型时触发该操作，比如在执行 Object.getPrototypeOf(proxy) 时。
handler.getPrototypeOf()

// 在设置代理对象的原型时触发该操作，比如在执行 Object.setPrototypeOf(proxy, null) 时。
handler.setPrototypeOf()


// 在判断一个代理对象是否是可扩展时触发该操作，比如在执行 Object.isExtensible(proxy) 时。
handler.isExtensible()


// 在让一个代理对象不可扩展时触发该操作，比如在执行 Object.preventExtensions(proxy) 时。
handler.preventExtensions()

// 在获取代理对象某个属性的属性描述时触发该操作，比如在执行 Object.getOwnPropertyDescriptor(proxy, "foo") 时。
handler.getOwnPropertyDescriptor()


// 在定义代理对象某个属性时的属性描述时触发该操作，比如在执行 Object.defineProperty(proxy, "foo", {}) 时。
andler.defineProperty()


// 在判断代理对象是否拥有某个属性时触发该操作，比如在执行 "foo" in proxy 时。
handler.has()

// 在读取代理对象的某个属性时触发该操作，比如在执行 proxy.foo 时。
handler.get()


// 在给代理对象的某个属性赋值时触发该操作，比如在执行 proxy.foo = 1 时。
handler.set()

// 在删除代理对象的某个属性时触发该操作，比如在执行 delete proxy.foo 时。
handler.deleteProperty()

// 在获取代理对象的所有属性键时触发该操作，比如在执行 Object.getOwnPropertyNames(proxy) 时。
handler.ownKeys()

// 在调用一个目标对象为函数的代理对象时触发该操作，比如在执行 proxy() 时。
handler.apply()


// 在给一个目标对象为构造函数的代理对象构造实例时触发该操作，比如在执行new proxy() 时。
handler.construct()
```

**<font style="color:rgb(44, 62, 80);">为何Proxy不能被Polyfill</font>**

+ <font style="color:rgb(44, 62, 80);">如class可以用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);">模拟；</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font><font style="color:rgb(44, 62, 80);">可以用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callback</font><font style="color:rgb(44, 62, 80);">模拟</font>
+ <font style="color:rgb(44, 62, 80);">但是proxy不能用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty</font><font style="color:rgb(44, 62, 80);">模拟</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">目前谷歌的polyfill只能实现部分的功能，如get、set https://github.com/GoogleChrome/proxy-polyfill</font>

```javascript
// commonJS require
const proxyPolyfill = require('proxy-polyfill/src/proxy')();

// Your environment may also support transparent rewriting of commonJS to ES6:
import ProxyPolyfillBuilder from 'proxy-polyfill/src/proxy';
const proxyPolyfill = ProxyPolyfillBuilder();

// Then use...
const myProxy = new proxyPolyfill(...);
```

### [](https://www.123fe.net/docs/base/improve.html#_24-ajax)<font style="color:rgb(44, 62, 80);">24 Ajax</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">它是一种异步通信的方法，通过直接由 js 脚本向服务器发起 http 通信，然后根据服务器返回的数据，更新网页的相应部分，而不用刷新整个页面的一种方法。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781109023-385a0bc6-9d9e-4a42-99b2-8f7f4b2afef5.png)

**<font style="color:rgb(44, 62, 80);">面试手写（原生）：</font>**

```javascript
//1：创建Ajax对象
var xhr = window.XMLHttpRequest?new XMLHttpRequest():new ActiveXObject('Microsoft.XMLHTTP');// 兼容IE6及以下版本
//2：配置 Ajax请求地址
xhr.open('get','index.xml',true);
//3：发送请求
xhr.send(null); // 严谨写法
//4:监听请求，接受响应
xhr.onreadysatechange=function(){
  if(xhr.readySate==4&&xhr.status==200 || xhr.status==304 )
    console.log(xhr.responsetXML)
}
```

**<font style="color:rgb(44, 62, 80);">jQuery写法</font>**

```javascript
$.ajax({
  type:'post',
  url:'',
  async:ture,//async 异步  sync  同步
  data:data,//针对post请求
  dataType:'jsonp',
  success:function (msg) {

  },
  error:function (error) {

  }
})
```

**<font style="color:rgb(44, 62, 80);">promise 封装实现：</font>**

```javascript
// promise 封装实现：

function getJSON(url) {
  // 创建一个 promise 对象
  let promise = new Promise(function(resolve, reject) {
    let xhr = new XMLHttpRequest();

    // 新建一个 http 请求
    xhr.open("GET", url, true);

    // 设置状态的监听函数
    xhr.onreadystatechange = function() {
      if (this.readyState !== 4) return;

      // 当请求成功或失败时，改变 promise 的状态
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };

    // 设置错误监听函数
    xhr.onerror = function() {
      reject(new Error(this.statusText));
    };

    // 设置响应的数据类型
    xhr.responseType = "json";

    // 设置请求头信息
    xhr.setRequestHeader("Accept", "application/json");

    // 发送 http 请求
    xhr.send(null);
  });

  return promise;
}
```

### [](https://www.123fe.net/docs/base/improve.html#_25-%E6%B7%B1%E5%85%A5%E6%95%B0%E7%BB%84)<font style="color:rgb(44, 62, 80);">25 深入数组</font>
**<font style="color:rgb(44, 62, 80);">一、梳理数组 API</font>**

**<font style="color:rgb(44, 62, 80);">1.</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array.of</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array.of</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">用于将参数依次转化为数组中的一项，然后返回这个新数组，而不管这个参数是数字还是其他。它基本上与 Array 构造器功能一致，唯一的区别就在单个数字参数的处理上</font>

```javascript
Array.of(8.0); // [8]
Array(8.0); // [empty × 8]
Array.of(8.0, 5); // [8, 5]
Array(8.0, 5); // [8, 5]
Array.of('8'); // ["8"]
Array('8'); // ["8"]
```

**<font style="color:rgb(44, 62, 80);">2.</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array.from</font>**

<font style="color:rgb(44, 62, 80);">从语法上看，Array.from 拥有 3 个参数：</font>

+ <font style="color:rgb(44, 62, 80);">类似数组的对象，必选；</font>
+ <font style="color:rgb(44, 62, 80);">加工函数，新生成的数组会经过该函数的加工再返回；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">作用域，表示加工函数执行时</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值。</font>

<font style="color:rgb(44, 62, 80);">这三个参数里面第一个参数是必选的，后两个参数都是可选的。我们通过一段代码来看看它的用法。</font>

```javascript
var obj = {0: 'a', 1: 'b', 2:'c', length: 3};
Array.from(obj, function(value, index){
  console.log(value, index, this, arguments.length);
  return value.repeat(3);   //必须指定返回值，否则返回 undefined
}, obj);

// return 的 value 重复了三遍，最后返回的数组为 ["aaa","bbb","ccc"]


// 如果这里不指定 this 的话，加工函数完全可以是一个箭头函数。上述代码可以简写为如下形式。
Array.from(obj, (value) => value.repeat(3));
//  控制台返回 (3) ["aaa", "bbb", "ccc"]
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">除了上述</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">obj</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">对象以外，拥有迭代器的对象还包括</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">String、Set、Map</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">等，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array.from</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">统统可以处理，请看下面的代码。</font>

```javascript
// String
Array.from('abc');         // ["a", "b", "c"]
// Set
Array.from(new Set(['abc', 'def'])); // ["abc", "def"]
// Map
Array.from(new Map([[1, 'ab'], [2, 'de']])); 
// [[1, 'ab'], [2, 'de']]
```

**<font style="color:rgb(44, 62, 80);">3.</font>****<font style="color:rgb(44, 62, 80);"> </font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array 的判断</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在 ES5 提供该方法之前，我们至少有如下 5 种方式去判断一个变量是否为数组。</font>

```javascript
var a = [];
// 1.基于instanceof
a instanceof Array;
// 2.基于constructor
a.constructor === Array;
// 3.基于Object.prototype.isPrototypeOf
Array.prototype.isPrototypeOf(a);
// 4.基于getPrototypeOf
Object.getPrototypeOf(a) === Array.prototype;
// 5.基于Object.prototype.toString
Object.prototype.toString.apply(a) === '[object Array]';
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">ES6 之后新增了一个</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array.isArray</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法，能直接判断数据类型是否为数组，但是如果 isArray 不存在，那么</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array.isArray</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的 polyfill 通常可以这样写：</font>

```javascript
if (!Array.isArray){
  Array.isArray = function(arg){
    return Object.prototype.toString.call(arg) === '[object Array]';
  };
}
```

**<font style="color:rgb(44, 62, 80);">4. 改变自身的方法</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">基于 ES6，会改变自身值的方法一共有</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">9</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">个，分别为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pop、push、reverse、shift、sort、splice、unshift，以及两个 ES6 新增的方法 copyWithin 和 fill</font>

```javascript
// pop方法
var array = ["cat", "dog", "cow", "chicken", "mouse"];
var item = array.pop();
console.log(array); // ["cat", "dog", "cow", "chicken"]
console.log(item); // mouse
// push方法
var array = ["football", "basketball",  "badminton"];
var i = array.push("golfball");
console.log(array); 
// ["football", "basketball", "badminton", "golfball"]
console.log(i); // 4
// reverse方法
var array = [1,2,3,4,5];
var array2 = array.reverse();
console.log(array); // [5,4,3,2,1]
console.log(array2===array); // true
// shift方法
var array = [1,2,3,4,5];
var item = array.shift();
console.log(array); // [2,3,4,5]
console.log(item); // 1
// unshift方法
var array = ["red", "green", "blue"];
var length = array.unshift("yellow");
console.log(array); // ["yellow", "red", "green", "blue"]
console.log(length); // 4
// sort方法
var array = ["apple","Boy","Cat","dog"];
var array2 = array.sort();
console.log(array); // ["Boy", "Cat", "apple", "dog"]
console.log(array2 == array); // true
// splice方法
var array = ["apple","boy"];
var splices = array.splice(1,1);
console.log(array); // ["apple"]
console.log(splices); // ["boy"]
// copyWithin方法
var array = [1,2,3,4,5]; 
var array2 = array.copyWithin(0,3);
console.log(array===array2,array2);  // true [4, 5, 3, 4, 5]
// fill方法
var array = [1,2,3,4,5];
var array2 = array.fill(10,0,3);
console.log(array===array2,array2); 
// true [10, 10, 10, 4, 5], 可见数组区间[0,3]的元素全部替换为10
```

**<font style="color:rgb(44, 62, 80);">5. 不改变自身的方法</font>**

<font style="color:rgb(44, 62, 80);">基于 ES7，不会改变自身的方法也有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">9</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">个，分别为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">concat、join、slice、toString、toLocaleString、indexOf、lastIndexOf、未形成标准的 toSource，以及 ES7 新增的方法 includes</font><font style="color:rgb(44, 62, 80);">。</font>

```javascript
// concat方法
var array = [1, 2, 3];
var array2 = array.concat(4,[5,6],[7,8,9]);
console.log(array2); // [1, 2, 3, 4, 5, 6, 7, 8, 9]
console.log(array); // [1, 2, 3], 可见原数组并未被修改
// join方法
var array = ['We', 'are', 'Chinese'];
console.log(array.join()); // "We,are,Chinese"
console.log(array.join('+')); // "We+are+Chinese"
// slice方法
var array = ["one", "two", "three","four", "five"];
console.log(array.slice()); // ["one", "two", "three","four", "five"]
console.log(array.slice(2,3)); // ["three"]
// toString方法
var array = ['Jan', 'Feb', 'Mar', 'Apr'];
var str = array.toString();
console.log(str); // Jan,Feb,Mar,Apr
// tolocalString方法
var array= [{name:'zz'}, 123, "abc", new Date()];
var str = array.toLocaleString();
console.log(str); // [object Object],123,abc,2016/1/5 下午1:06:23
// indexOf方法
var array = ['abc', 'def', 'ghi','123'];
console.log(array.indexOf('def')); // 1
// includes方法
var array = [-0, 1, 2];
console.log(array.includes(+0)); // true
console.log(array.includes(1)); // true
var array = [NaN];
console.log(array.includes(NaN)); // true
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">其中 includes 方法需要注意的是，如果元素中有 0，那么在判断过程中不论是 +0 还是 -0 都会判断为 True，这里的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">includes 忽略了 +0 和 -0</font>

**<font style="color:rgb(44, 62, 80);">6. 数组遍历的方法</font>**

<font style="color:rgb(44, 62, 80);">基于 ES6，不会改变自身的遍历方法一共有 12 个，分别为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forEach、every、some、filter、map、reduce、reduceRight，以及 ES6 新增的方法 entries、find、findIndex、keys、values</font>

```javascript
// forEach方法
var array = [1, 3, 5];
var obj = {name:'cc'};
var sReturn = array.forEach(function(value, index, array){
  array[index] = value;
  console.log(this.name); // cc被打印了三次, this指向obj
},obj);
console.log(array); // [1, 3, 5]
console.log(sReturn); // undefined, 可见返回值为undefined
// every方法
var o = {0:10, 1:8, 2:25, length:3};
var bool = Array.prototype.every.call(o,function(value, index, obj){
  return value >= 8;
},o);
console.log(bool); // true
// some方法
var array = [18, 9, 10, 35, 80];
var isExist = array.some(function(value, index, array){
  return value > 20;
});
console.log(isExist); // true 
// map 方法
var array = [18, 9, 10, 35, 80];
array.map(item => item + 1);
console.log(array);  // [19, 10, 11, 36, 81]
// filter 方法
var array = [18, 9, 10, 35, 80];
var array2 = array.filter(function(value, index, array){
  return value > 20;
});
console.log(array2); // [35, 80]
// reduce方法
var array = [1, 2, 3, 4];
var s = array.reduce(function(previousValue, value, index, array){
  return previousValue * value;
},1);
console.log(s); // 24
// ES6写法更加简洁
array.reduce((p, v) => p * v); // 24
// reduceRight方法 (和reduce的区别就是从后往前累计)
var array = [1, 2, 3, 4];
array.reduceRight((p, v) => p * v); // 24
// entries方法
var array = ["a", "b", "c"];
var iterator = array.entries();
console.log(iterator.next().value); // [0, "a"]
console.log(iterator.next().value); // [1, "b"]
console.log(iterator.next().value); // [2, "c"]
console.log(iterator.next().value); // undefined, 迭代器处于数组末尾时, 再迭代就会返回undefined
// find & findIndex方法
var array = [1, 3, 5, 7, 8, 9, 10];
function f(value, index, array){
  return value%2==0;     // 返回偶数
}
function f2(value, index, array){
  return value > 20;     // 返回大于20的数
}
console.log(array.find(f)); // 8
console.log(array.find(f2)); // undefined
console.log(array.findIndex(f)); // 4
console.log(array.findIndex(f2)); // -1
// keys方法
[...Array(10).keys()];     // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
[...new Array(10).keys()]; // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
// values方法
var array = ["abc", "xyz"];
var iterator = array.values();
console.log(iterator.next().value);//abc
console.log(iterator.next().value);//xyz
```

**<font style="color:rgb(44, 62, 80);">7. 总结</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781108969-e67ad11a-50c5-457c-a30d-9edc898b5e93.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这些方法之间存在很多共性，如下：</font>

+ <font style="color:rgb(44, 62, 80);">所有插入元素的方法，比如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">push、unshift</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一律返回数组新的长度；</font>
+ <font style="color:rgb(44, 62, 80);">所有删除元素的方法，比如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pop、shift、splice</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一律返回删除的元素，或者返回删除的多个元素组成的数组；</font>
+ <font style="color:rgb(44, 62, 80);">部分遍历方法，比如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forEach、every、some、filter、map、find、findIndex</font><font style="color:rgb(44, 62, 80);">，它们都包含</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function(value,index,array){}</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">thisArg</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这样两个形参。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">数组和字符串方法</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781108899-521405b2-29f9-480a-acbf-a871bef50d89.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781110426-8430f316-d76f-44e7-9db9-295020aa1ca1.png)

**<font style="color:rgb(44, 62, 80);">二、理解JS的类数组</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在 JavaScript 中有哪些情况下的对象是类数组呢？主要有以下几种</font>

+ <font style="color:rgb(44, 62, 80);">函数里面的参数对象</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getElementsByTagName/ClassName/Name</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">获得的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTMLCollection</font>
+ <font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">querySelector</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">获得的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NodeList</font>

**<font style="color:rgb(44, 62, 80);">1. arguments对象</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">arguments对象是函数中传递的参数值的集合。它是一个类似数组的对象，因为它有一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">length</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性，我们可以使用数组索引表示法</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments[1]</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来访问单个值，但它没有数组中的内置方法，如：forEach、reduce、filter和map。</font>

```javascript
function foo(name, age, sex) {
  console.log(arguments);
  console.log(typeof arguments);
  console.log(Object.prototype.toString.call(arguments));
}
foo('jack', '18', 'male');
```

<font style="color:rgb(44, 62, 80);">这段代码比较容易，就是直接将这个函数的 arguments 在函数内部打印出来，那么我们看下这个 arguments 打印出来的结果，请看控制台的这张截图。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781110369-055a7aa2-0157-4216-bc0d-c1d2b56823f5.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从结果中可以看到，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">typeof</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">返回的是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.prototype.toString.call</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">返回的结果是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">'[object arguments]'</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，可以看出来返回的不是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">'[object array]'</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，说明</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和数组还是有区别的。</font>

<font style="color:rgb(44, 62, 80);">我们可以使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array.prototype.slice</font><font style="color:rgb(44, 62, 80);">将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);">对象转换成一个数组。</font>

```plain
function one() {
  return Array.prototype.slice.call(arguments);
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">注意:箭头函数中没有arguments对象。</font>

```javascript
function one() {
  return arguments;
}
const two = function () {
  return arguments;
}
const three = function three() {
  return arguments;
}

const four = () => arguments;

four(); // Throws an error  - arguments is not defined
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当我们调用函数four时，它会抛出一个ReferenceError: arguments is not defined error。使用rest语法，可以解决这个问题。</font>

```javascript
const four = (...args) => args;
```

<font style="color:rgb(44, 62, 80);">这会自动将所有参数值放入数组中。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">arguments 不仅仅有一个 length 属性，还有一个 callee 属性，我们接下来看看这个 callee 是干什么的，代码如下所示</font>

```javascript
function foo(name, age, sex) {
  console.log(arguments.callee);
}
foo('jack', '18', 'male');
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781110963-1306a318-2233-453d-910b-d35bed70fe32.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从控制台可以看到，输出的就是函数自身，如果在函数内部直接执行调用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callee</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的话，那它就会不停地执行当前函数，直到执行到内存溢出</font>

**<font style="color:rgb(44, 62, 80);">2. HTMLCollection</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">HTMLCollection 简单来说是 HTML DOM 对象的一个接口，这个接口包含了获取到的 DOM 元素集合，返回的类型是类数组对象，如果用 typeof 来判断的话，它返回的是 'object'。它是及时更新的，当文档中的 DOM 变化时，它也会随之变化。</font>

<font style="color:rgb(44, 62, 80);">描述起来比较抽象，还是通过一段代码来看下</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTMLCollection</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">最后返回的是什么，我们先随便找一个页面中有 form 表单的页面，在控制台中执行下述代码</font>

```javascript
var elem1, elem2;
// document.forms 是一个 HTMLCollection
elem1 = document.forms[0];
elem2 = document.forms.item(0);
console.log(elem1);
console.log(elem2);
console.log(typeof elem1);
console.log(Object.prototype.toString.call(elem1));
```

<font style="color:rgb(44, 62, 80);">在这个有 form 表单的页面执行上面的代码，得到的结果如下。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781112466-53886c41-5d8c-43b2-b5da-64717c95a1a9.png)

<font style="color:rgb(44, 62, 80);">可以看到，这里打印出来了页面第一个 form 表单元素，同时也打印出来了判断类型的结果，说明打印的判断的类型和 arguments 返回的也比较类似，typeof 返回的都是 'object'，和上面的类似。</font>

<font style="color:rgb(44, 62, 80);">另外需要注意的一点就是 HTML DOM 中的 HTMLCollection 是即时更新的，当其所包含的文档结构发生改变时，它会自动更新。下面我们再看最后一个 NodeList 类数组。</font>

**<font style="color:rgb(44, 62, 80);">3. NodeList</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">NodeList 对象是节点的集合，通常是由 querySlector 返回的。NodeList 不是一个数组，也是一种类数组。虽然 NodeList 不是一个数组，但是可以使用 for...of 来迭代。在一些情况下，NodeList 是一个实时集合，也就是说，如果文档中的节点树发生变化，NodeList 也会随之变化。我们还是利用代码来理解一下 Nodelist 这种类数组。</font>

```javascript
var list = document.querySelectorAll('input[type=checkbox]');
for (var checkbox of list) {
  checkbox.checked = true;
}
console.log(list);
console.log(typeof list);
console.log(Object.prototype.toString.call(list));
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从上面的代码执行的结果中可以发现，我们是通过有 CheckBox 的页面执行的代码，在结果可中输出了一个 NodeList 类数组，里面有一个 CheckBox 元素，并且我们判断了它的类型，和上面的 arguments 与 HTMLCollection 其实是类似的，执行结果如下图所示。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781112507-faf626fb-679e-4b14-bf88-097faa920161.png)

**<font style="color:rgb(44, 62, 80);">4. 类数组应用场景</font>**

1. <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">遍历参数操作</font>

<font style="color:rgb(44, 62, 80);">我们在函数内部可以直接获取</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个类数组的值，那么也可以对于参数进行一些操作，比如下面这段代码，我们可以将函数的参数默认进行求和操作。</font>

```javascript
function add() {
  var sum =0,
    len = arguments.length;
  for(var i = 0; i < len; i++){
    sum += arguments[i];
  }
  return sum;
}
add()                           // 0
add(1)                          // 1
add(1，2)                       // 3
add(1,2,3,4);                   // 10
```

1. <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">定义链接字符串函数</font>

<font style="color:rgb(44, 62, 80);">我们可以通过 arguments 这个例子定义一个函数来连接字符串。这个函数唯一正式声明了的参数是一个字符串，该参数指定一个字符作为衔接点来连接字符串。该函数定义如下。</font>

```javascript
// 这段代码说明了，你可以传递任意数量的参数到该函数，并使用每个参数作为列表中的项创建列表进行拼接。从这个例子中也可以看出，我们可以在日常编码中采用这样的代码抽象方式，把需要解决的这一类问题，都抽象成通用的方法，来提升代码的可复用性
function myConcat(separa) {
  var args = Array.prototype.slice.call(arguments, 1);
  return args.join(separa);
}
myConcat(", ", "red", "orange", "blue");
// "red, orange, blue"
myConcat("; ", "elephant", "lion", "snake");
// "elephant; lion; snake"
myConcat(". ", "one", "two", "three", "four", "five");
// "one. two. three. four. five"
```

1. <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">传递参数使用</font>

```javascript
// 使用 apply 将 foo 的参数传递给 bar
function foo() {
  bar.apply(this, arguments);
}
function bar(a, b, c) {
  console.log(a, b, c);
}
foo(1, 2, 3)   //1 2 3
```

**<font style="color:rgb(44, 62, 80);">5. 如何将类数组转换成数组</font>**

1. <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">类数组借用数组方法转数组</font>

```javascript
function sum(a, b) {
  let args = Array.prototype.slice.call(arguments);
  // let args = [].slice.call(arguments); // 这样写也是一样效果
  console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2);  // 3
function sum(a, b) {
  let args = Array.prototype.concat.apply([], arguments);
  console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2);  // 3
```

1. <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">ES6 的方法转数组</font>

```javascript
function sum(a, b) {
  let args = Array.from(arguments);
  console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2);    // 3
function sum(a, b) {
  let args = [...arguments];
  console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2);    // 3
function sum(...args) {
  console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2);    // 3
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array.from</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6 的展开运算符</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，都可以把</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个类数组转换成数组</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">args</font>

<font style="color:rgb(44, 62, 80);">类数组和数组的异同点</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781113228-50b9af8e-fa98-4fbb-9f30-3869bc457aed.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在前端工作中，开发者往往会忽视对类数组的学习，其实在高级 JavaScript 编程中经常需要将类数组向数组转化，尤其是一些比较复杂的开源项目，经常会看到函数中处理参数的写法，例如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[].slice.call(arguments)</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这行代码。</font>

**<font style="color:rgb(44, 62, 80);">三、实现数组扁平化的 6 种方式</font>**

**<font style="color:rgb(44, 62, 80);">1. 方法一：普通的递归实</font>**

<font style="color:rgb(44, 62, 80);">普通的递归思路很容易理解，就是通过循环递归的方式，一项一项地去遍历，如果每一项还是一个数组，那么就继续往下遍历，利用递归程序的方法，来实现数组的每一项的连接。我们来看下这个方法是如何实现的，如下所示</font>

```javascript
// 方法1
var a = [1, [2, [3, 4, 5]]];
function flatten(arr) {
  let result = [];

  for(let i = 0; i < arr.length; i++) {
    if(Array.isArray(arr[i])) {
      result = result.concat(flatten(arr[i]));
    } else {
      result.push(arr[i]);
    }
  }
  return result;
}
flatten(a);  //  [1, 2, 3, 4，5]
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">从上面这段代码可以看出，最后返回的结果是扁平化的结果，这段代码核心就是循环遍历过程中的递归操作，就是在遍历过程中发现数组元素还是数组的时候进行递归操作，把数组的结果通过数组的 concat 方法拼接到最后要返回的 result 数组上，那么最后输出的结果就是扁平化后的数组</font>

**<font style="color:rgb(44, 62, 80);">2. 方法二：利用 reduce 函数迭代</font>**

<font style="color:rgb(44, 62, 80);">从上面普通的递归函数中可以看出，其实就是对数组的每一项进行处理，那么我们其实也可以用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reduce</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来实现数组的拼接，从而简化第一种方法的代码，改造后的代码如下所示。</font>

```javascript
// 方法2
var arr = [1, [2, [3, 4]]];
function flatten(arr) {
  return arr.reduce(function(prev, next){
    return prev.concat(Array.isArray(next) ? flatten(next) : next)
  }, [])
}
console.log(flatten(arr));//  [1, 2, 3, 4，5]
```

**<font style="color:rgb(44, 62, 80);">3. 方法三：扩展运算符实现</font>**

<font style="color:rgb(44, 62, 80);">这个方法的实现，采用了扩展运算符和 some 的方法，两者共同使用，达到数组扁平化的目的，还是来看一下代码</font>

```plain
// 方法3
var arr = [1, [2, [3, 4]]];
function flatten(arr) {
    while (arr.some(item => Array.isArray(item))) {
        arr = [].concat(...arr);
    }
    return arr;
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]
```

<font style="color:rgb(44, 62, 80);">从执行的结果中可以发现，我们先用数组的 some 方法把数组中仍然是组数的项过滤出来，然后执行 concat 操作，利用 ES6 的展开运算符，将其拼接到原数组中，最后返回原数组，达到了预期的效果。</font>

<font style="color:rgb(44, 62, 80);">前三种实现数组扁平化的方式其实是最基本的思路，都是通过最普通递归思路衍生的方法，尤其是前两种实现方法比较类似。值得注意的是 reduce 方法，它可以在很多应用场景中实现，由于 reduce 这个方法提供的几个参数比较灵活，能解决很多问题，所以是值得熟练使用并且精通的</font>

**<font style="color:rgb(44, 62, 80);">4. 方法四：split 和 toString 共同处理</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们也可以通过 split 和 toString 两个方法，来共同实现数组扁平化，由于数组会默认带一个 toString 的方法，所以可以把数组直接转换成逗号分隔的字符串，然后再用 split 方法把字符串重新转换为数组，如下面的代码所示。</font>

```plain
// 方法4
var arr = [1, [2, [3, 4]]];
function flatten(arr) {
    return arr.toString().split(',');
}
console.log(flatten(arr)); //  [1, 2, 3, 4]
```

<font style="color:rgb(44, 62, 80);">通过这两个方法可以将多维数组直接转换成逗号连接的字符串，然后再重新分隔成数组，你可以在控制台执行一下查看结果。</font>

**<font style="color:rgb(44, 62, 80);">5. 方法五：调用 ES6 中的 flat</font>**

<font style="color:rgb(44, 62, 80);">我们还可以直接调用 ES6 中的 flat 方法，可以直接实现数组扁平化。先来看下 flat 方法的语法：</font>

```plain
arr.flat([depth])
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">其中 depth 是 flat 的参数，depth 是可以传递数组的展开深度（默认不填、数值是 1），即展开一层数组。那么如果多层的该怎么处理呢？参数也可以传进 Infinity，代表不论多少层都要展开。那么我们来看下，用 flat 方法怎么实现，请看下面的代码。</font>

```plain
// 方法5
var arr = [1, [2, [3, 4]]];
function flatten(arr) {
  return arr.flat(Infinity);
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]
```

+ <font style="color:rgb(44, 62, 80);">可以看出，一个嵌套了两层的数组，通过将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flat</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的参数设置为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Infinity</font><font style="color:rgb(44, 62, 80);">，达到了我们预期的效果。其实同样也可以设置成 2，也能实现这样的效果。</font>
+ <font style="color:rgb(44, 62, 80);">因此，你在编程过程中，发现对数组的嵌套层数不确定的时候，最好直接使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Infinity</font><font style="color:rgb(44, 62, 80);">，可以达到扁平化。下面我们再来看最后一种场景</font>

**<font style="color:rgb(44, 62, 80);">6. 方法六：正则和 JSON 方法共同处理</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们在第四种方法中已经尝试了用 toString 方法，其中仍然采用了将 JSON.stringify 的方法先转换为字符串，然后通过正则表达式过滤掉字符串中的数组的方括号，最后再利用 JSON.parse 把它转换成数组。请看下面的代码</font>

```plain
// 方法 6
let arr = [1, [2, [3, [4, 5]]], 6];
function flatten(arr) {
  let str = JSON.stringify(arr);
  str = str.replace(/(\[|\])/g, '');
  str = '[' + str + ']';
  return JSON.parse(str); 
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]
```

<font style="color:rgb(44, 62, 80);">可以看到，其中先把传入的数组转换成字符串，然后通过正则表达式的方式把括号过滤掉，这部分正则的表达式你不太理解的话，可以看看下面的图片</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781114569-03b1d742-96bd-4992-8fd8-83387cea2ee5.png)

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">通过这个在线网站 https://regexper.com/ 可以把正则分析成容易理解的可视化的逻辑脑图。其中我们可以看到，匹配规则是：全局匹配（g）左括号或者右括号，将它们替换成空格，最后返回处理后的结果。之后拿着正则处理好的结果重新在外层包裹括号，最后通过 JSON.parse 转换成数组返回。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781114667-fc899d72-5ec4-48fd-906e-f351168a4a39.png)

**<font style="color:rgb(44, 62, 80);">四、如何用 JS 实现各种数组排序</font>**

<font style="color:rgb(44, 62, 80);">数据结构算法中排序有很多种，常见的、不常见的，至少包含十种以上。根据它们的特性，可以大致分为两种类型：比较类排序和非比较类排序。</font>

+ **<font style="color:rgb(44, 62, 80);">比较类排序</font>**<font style="color:rgb(44, 62, 80);">：通过比较来决定元素间的相对次序，其时间复杂度不能突破</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">O(nlogn)</font><font style="color:rgb(44, 62, 80);">，因此也称为非线性时间比较类排序。</font>
+ **<font style="color:rgb(44, 62, 80);">非比较类排序</font>**<font style="color:rgb(44, 62, 80);">：不通过比较来决定元素间的相对次序，它可以突破基于比较排序的时间下界，以线性时间运行，因此也称为线性时间非比较类排序。</font>

<font style="color:rgb(44, 62, 80);">我们通过一张图片来看看这两种分类方式分别包括哪些排序方法。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781114614-d8eae2e6-4d68-4571-9bfa-2c99a9c71e8e.png)

<font style="color:rgb(44, 62, 80);">非比较类的排序在实际情况中用的比较少</font>

**<font style="color:rgb(44, 62, 80);">1. 冒泡排序</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">冒泡排序是最基础的排序，一般在最开始学习数据结构的时候就会接触它。冒泡排序是一次比较两个元素，如果顺序是错误的就把它们交换过来。走访数列的工作会重复地进行，直到不需要再交换，也就是说该数列已经排序完成。请看下面的代码。</font>

```plain
var a = [1, 3, 6, 3, 23, 76, 1, 34, 222, 6, 456, 221];
function bubbleSort(array) {
  const len = array.length
  if (len < 2) return array
  for (let i = 0; i < len; i++) {
    for (let j = 0; j < i; j++) {
      if (array[j] > array[i]) {
        const temp = array[j]
        array[j] = array[i]
        array[i] = temp
      }
    }
  }
  return array
}
bubbleSort(a);  // [1, 1, 3, 3, 6, 6, 23, 34, 76, 221, 222, 456]
```

<font style="color:rgb(44, 62, 80);">从上面这段代码可以看出，最后返回的是排好序的结果。因为冒泡排序实在太基础和简单，这里就不过多赘述了。下面我们来看看快速排序法</font>

**<font style="color:rgb(44, 62, 80);">2. 快速排序</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">快速排序的基本思想是通过一趟排序，将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可以分别对这两部分记录继续进行排序，以达到整个序列有序。</font>

```plain
var a = [1, 3, 6, 3, 23, 76, 1, 34, 222, 6, 456, 221];
function quickSort(array) {
  var quick = function(arr) {
    if (arr.length <= 1) return arr
    const len = arr.length
    const index = Math.floor(len >> 1)
    const pivot = arr.splice(index, 1)[0]
    const left = []
    const right = []
    for (let i = 0; i < len; i++) {
      if (arr[i] > pivot) {
        right.push(arr[i])
      } else if (arr[i] <= pivot) {
        left.push(arr[i])
      }
    }
    return quick(left).concat([pivot], quick(right))
  }
  const result = quick(array)
  return result
}
quickSort(a);//  [1, 1, 3, 3, 6, 6, 23, 34, 76, 221, 222, 456]
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上面的代码在控制台执行之后，也可以得到预期的结果。最主要的思路是从数列中挑出一个元素，称为 “基准”（pivot）；然后重新排序数列，所有元素比基准值小的摆放在基准前面、比基准值大的摆在基准的后面；在这个区分搞定之后，该基准就处于数列的中间位置；然后把小于基准值元素的子数列（left）和大于基准值元素的子数列（right）递归地调用 quick 方法排序完成，这就是快排的思路。</font>

**<font style="color:rgb(44, 62, 80);">3. 插入排序</font>**

<font style="color:rgb(44, 62, 80);">插入排序算法描述的是一种简单直观的排序算法。它的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入，从而达到排序的效果</font><font style="color:rgb(44, 62, 80);">。来看一下代码</font>

```plain
var a = [1, 3, 6, 3, 23, 76, 1, 34, 222, 6, 456, 221];
function insertSort(array) {
  const len = array.length
  let current
  let prev
  for (let i = 1; i < len; i++) {
    current = array[i]
    prev = i - 1
    while (prev >= 0 && array[prev] > current) {
      array[prev + 1] = array[prev]
      prev--
    }
    array[prev + 1] = current
  }
  return array
}
insertSort(a); // [1, 1, 3, 3, 6, 6, 23, 34, 76, 221, 222, 456]
```

<font style="color:rgb(44, 62, 80);">从执行的结果中可以发现，通过插入排序这种方式实现了排序效果。插入排序的思路是基于数组本身进行调整的，首先循环遍历从 i 等于 1 开始，拿到当前的 current 的值，去和前面的值比较，如果前面的大于当前的值，就把前面的值和当前的那个值进行交换，通过这样不断循环达到了排序的目的</font>

**<font style="color:rgb(44, 62, 80);">4. 选择排序</font>**

<font style="color:rgb(44, 62, 80);">选择排序是一种简单直观的排序算法。它的工作原理是，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">首先将最小的元素存放在序列的起始位置，再从剩余未排序元素中继续寻找最小元素，然后放到已排序的序列后面……以此类推，直到所有元素均排序完毕</font><font style="color:rgb(44, 62, 80);">。请看下面的代码。</font>

```plain
var a = [1, 3, 6, 3, 23, 76, 1, 34, 222, 6, 456, 221];
function selectSort(array) {
  const len = array.length
  let temp
  let minIndex
  for (let i = 0; i < len - 1; i++) {
    minIndex = i
    for (let j = i + 1; j < len; j++) {
      if (array[j] <= array[minIndex]) {
        minIndex = j
      }
    }
    temp = array[i]
    array[i] = array[minIndex]
    array[minIndex] = temp
  }
  return array
}
selectSort(a); // [1, 1, 3, 3, 6, 6, 23, 34, 76, 221, 222, 456]
```

<font style="color:rgb(44, 62, 80);">这样，通过选择排序的方法同样也可以实现数组的排序，从上面的代码中可以看出该排序是表现最稳定的排序算法之一，因为无论什么数据进去都是 O(n 平方) 的时间复杂度，所以用到它的时候，数据规模越小越好</font>

**<font style="color:rgb(44, 62, 80);">5. 堆排序</font>**

<font style="color:rgb(44, 62, 80);">堆排序是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质，即子结点的键值或索引总是小于（或者大于）它的父节点。堆的底层实际上就是一棵完全二叉树，可以用数组实现。</font>

<font style="color:rgb(44, 62, 80);">根节点最大的堆叫作大根堆，根节点最小的堆叫作小根堆，你可以根据从大到小排序或者从小到大来排序，分别建立对应的堆就可以。请看下面的代码</font>

```javascript
var a = [1, 3, 6, 3, 23, 76, 1, 34, 222, 6, 456, 221];
function heap_sort(arr) {
  var len = arr.length
  var k = 0
  function swap(i, j) {
    var temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
  }
  function max_heapify(start, end) {
    var dad = start
    var son = dad * 2 + 1
    if (son >= end) return
    if (son + 1 < end && arr[son] < arr[son + 1]) {
      son++
    }
    if (arr[dad] <= arr[son]) {
      swap(dad, son)
      max_heapify(son, end)
    }
  }
  for (var i = Math.floor(len / 2) - 1; i >= 0; i--) {
    max_heapify(i, len)
  }

  for (var j = len - 1; j > k; j--) {
    swap(0, j)
    max_heapify(0, j)
  }

  return arr
}
heap_sort(a); // [1, 1, 3, 3, 6, 6, 23, 34, 76, 221, 222, 456]
```

<font style="color:rgb(44, 62, 80);">从代码来看，堆排序相比上面几种排序整体上会复杂一些，不太容易理解。不过你应该知道两点：</font>

+ <font style="color:rgb(44, 62, 80);">一是堆排序最核心的点就在于排序前先建堆；</font>
+ <font style="color:rgb(44, 62, 80);">二是由于堆其实就是完全二叉树，如果父节点的序号为 n，那么叶子节点的序号就分别是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">2n</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">2n+1</font><font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">你理解了这两点，再看代码就比较好理解了。堆排序最后有两个循环：第一个是处理父节点的顺序；第二个循环则是根据父节点和叶子节点的大小对比，进行堆的调整。通过这两轮循环的调整，最后堆排序完成。</font>

**<font style="color:rgb(44, 62, 80);">6. 归并排序</font>**

<font style="color:rgb(44, 62, 80);">归并排序是建立在归并操作上的一种有效的排序算法，该算法是采用分治法的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为二路归并。我们先看一下代码。</font>

```javascript
var a = [1, 3, 6, 3, 23, 76, 1, 34, 222, 6, 456, 221];
function mergeSort(array) {
  const merge = (right, left) => {
    const result = []
    let il = 0
    let ir = 0
    while (il < left.length && ir < right.length) {
      if (left[il] < right[ir]) {
        result.push(left[il++])
      } else {
        result.push(right[ir++])
      }
    }
    while (il < left.length) {
      result.push(left[il++])
    }
    while (ir < right.length) {
      result.push(right[ir++])
    }
    return result
  }
  const mergeSort = array => {
    if (array.length === 1) { return array }
    const mid = Math.floor(array.length / 2)
    const left = array.slice(0, mid)
    const right = array.slice(mid, array.length)
    return merge(mergeSort(left), mergeSort(right))
  }
  return mergeSort(array)
}
mergeSort(a); // [1, 1, 3, 3, 6, 6, 23, 34, 76, 221, 222, 456]
```

<font style="color:rgb(44, 62, 80);">从上面这段代码中可以看到，通过归并排序可以得到想要的结果。上面提到了分治的思路，你可以从 mergeSort 方法中看到，通过 mid 可以把该数组分成左右两个数组，分别对这两个进行递归调用排序方法，最后将两个数组按照顺序归并起来。</font>

<font style="color:rgb(44, 62, 80);">归并排序是一种稳定的排序方法，和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好得多，因为始终都是 O(nlogn) 的时间复杂度。而代价是需要额外的内存空间。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781114577-c5fc04d8-5750-4e56-9fbc-b958c702193e.png)

<font style="color:rgb(44, 62, 80);">其中你可以看到排序相关的时间复杂度和空间复杂度以及稳定性的情况，如果遇到需要自己实现排序的时候，可以根据它们的空间和时间复杂度综合考量，选择最适合的排序方法</font>

