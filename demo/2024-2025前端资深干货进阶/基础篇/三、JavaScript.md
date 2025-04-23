### <font style="color:rgb(44, 62, 80);">1 闭包</font>
+ <font style="color:rgb(44, 62, 80);">闭包就是能够读取其他函数内部变量的函数</font>
+ <font style="color:rgb(44, 62, 80);">闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量,利用闭包可以突破作用链域</font>
+ **<font style="color:rgb(44, 62, 80);">闭包的特性：</font>**
    - <font style="color:rgb(44, 62, 80);">函数内再嵌套函数</font>
    - <font style="color:rgb(44, 62, 80);">内部函数可以引用外层的参数和变量</font>
    - <font style="color:rgb(44, 62, 80);">参数和变量不会被垃圾回收机制回收</font>

**<font style="color:rgb(44, 62, 80);">说说你对闭包的理解</font>**

+ <font style="color:rgb(44, 62, 80);">使用闭包主要是为了设计私有的方法和变量。闭包的优点是可以避免全局变量的污染，缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。在js中，函数即闭包，只有函数才会产生作用域的概念</font>
+ <font style="color:rgb(44, 62, 80);">闭包 的最大用处有两个，一个是可以读取函数内部的变量，另一个就是让这些变量始终保持在内存中</font>
+ <font style="color:rgb(44, 62, 80);">闭包的另一个用处，是封装对象的私有属性和私有方法</font>
+ **<font style="color:rgb(44, 62, 80);">好处</font>**<font style="color:rgb(44, 62, 80);">：能够实现封装和缓存等；</font>
+ **<font style="color:rgb(44, 62, 80);">坏处</font>**<font style="color:rgb(44, 62, 80);">：就是消耗内存、不正当使用会造成内存溢出的问题</font>

**<font style="color:rgb(44, 62, 80);">使用闭包的注意点</font>**

+ <font style="color:rgb(44, 62, 80);">由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露</font>
+ <font style="color:rgb(44, 62, 80);">解决方法是，在退出函数之前，将不使用的局部变量全部删除</font>

**<font style="color:rgb(44, 62, 80);">举出闭包实际场景运用的例子</font>**

1. <font style="color:rgb(44, 62, 80);">比如常见的防抖节流</font>

```javascript
// 防抖
function debounce(fn, delay = 300) {
  let timer; //闭包引用的外界变量
  return function () {
    const args = arguments;
    if (timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}
```

1. <font style="color:rgb(44, 62, 80);">使用闭包可以在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中模拟块级作用域</font>

```javascript
function outputNumbers(count) {
  (function () {
    for (var i = 0; i < count; i++) {
      alert(i);
    }
  })();
  alert(i); //导致一个错误！
}
```

1. <font style="color:rgb(44, 62, 80);">闭包可以用于在对象中创建私有变量</font>

```javascript
var aaa = (function () {
  var a = 1;
  function bbb() {
    a++;
    console.log(a);
  }
  function ccc() {
    a++;
    console.log(a);
  }
  return {
    b: bbb, //json结构
    c: ccc,
  };
})();
console.log(aaa.a); //undefined
aaa.b(); //2
aaa.c(); //3
```

### <font style="color:rgb(44, 62, 80);">2 说说你对作用域链的理解</font>
+ <font style="color:rgb(44, 62, 80);">作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font><font style="color:rgb(44, 62, 80);">对象即被终止，作用域链向下访问变量是不被允许的</font>
+ <font style="color:rgb(44, 62, 80);">简单的说，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">作用域就是变量与函数的可访问范围</font><font style="color:rgb(44, 62, 80);">，即作用域控制着变量与函数的可见性和生命周期</font>

### <font style="color:rgb(44, 62, 80);">3 JavaScript原型，原型链 ? 有什么特点？</font>
+ <font style="color:rgb(44, 62, 80);">每个对象都会在其内部初始化一个属性，就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);">，当我们访问一个对象的属性时</font>
+ <font style="color:rgb(44, 62, 80);">如果这个对象内部不存在这个属性，那么他就会去</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);">里找这个属性，这个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);">又会有自己的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);">，于是就这样一直找下去，也就是我们平时所说的原型链的概念。按照标准，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">__proto__</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是不对外公开的，也就是说是个私有属性</font>
+ <font style="color:rgb(44, 62, 80);">关系：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">instance.constructor.prototype == instance.__proto__</font>

```javascript
// eg.
var a = {}

a.constructor.prototype == a.__proto__
```

+ <font style="color:rgb(44, 62, 80);">特点：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变</font>
+ <font style="color:rgb(44, 62, 80);">当我们需要一个属性的时，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Javascript</font><font style="color:rgb(44, 62, 80);">引擎会先看当前对象中是否有这个属性， 如果没有的</font>
+ <font style="color:rgb(44, 62, 80);">就会查找他的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Prototype</font><font style="color:rgb(44, 62, 80);">对象是否有这个属性，如此递推下去，一直检索到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内建对象</font>
+ **<font style="color:rgb(44, 62, 80);">原型：</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">的所有对象中都包含了一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[__proto__]</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内部属性，这个属性所对应的就是该对象的原型</font>
    - <font style="color:rgb(44, 62, 80);">JavaScript的函数对象，除了原型</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[__proto__]</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之外，还预置了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性</font>
    - <font style="color:rgb(44, 62, 80);">当函数对象作为构造函数创建实例时，该 prototype 属性值将被作为实例对象的原型</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[__proto__]</font><font style="color:rgb(44, 62, 80);">。</font>
+ **<font style="color:rgb(44, 62, 80);">原型链：</font>**
    - <font style="color:rgb(44, 62, 80);">当一个对象调用的属性/方法自身不存在时，就会去自己</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[__proto__]</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">关联的前辈</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象上去找</font>
    - <font style="color:rgb(44, 62, 80);">如果没找到，就会去该</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">原型</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[__proto__]</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">关联的前辈</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">去找。依次类推，直到找到属性/方法或</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为止。从而形成了所谓的“原型链”</font>
+ **<font style="color:rgb(44, 62, 80);">原型特点：</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">对象是通过引用来传递的，当修改原型时，与之相关的对象也会继承这一改变</font>

### <font style="color:rgb(44, 62, 80);">4 请解释什么是事件代理</font>
+ <font style="color:rgb(44, 62, 80);">事件代理（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Event Delegation</font><font style="color:rgb(44, 62, 80);">），又称之为事件委托。是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中常用绑定事件的常用技巧。顾名思义，“事件代理”即是把原本需要绑定的事件委托给父元素，让父元素担当事件监听的职务。事件代理的原理是DOM元素的事件冒泡。使用事件代理的好处是可以提高性能</font>
+ <font style="color:rgb(44, 62, 80);">可以大量节省内存占用，减少事件注册，比如在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">table</font><font style="color:rgb(44, 62, 80);">上代理所有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">td</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">click</font><font style="color:rgb(44, 62, 80);">事件就非常棒</font>
+ <font style="color:rgb(44, 62, 80);">可以实现当新增子对象时无需再次对其绑定</font>

### <font style="color:rgb(44, 62, 80);">5 Javascript如何实现继承？</font>
+ <font style="color:rgb(44, 62, 80);">构造继承</font>
+ <font style="color:rgb(44, 62, 80);">原型继承</font>
+ <font style="color:rgb(44, 62, 80);">实例继承</font>
+ <font style="color:rgb(44, 62, 80);">拷贝继承</font>
+ <font style="color:rgb(44, 62, 80);">原型</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);">机制或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call</font><font style="color:rgb(44, 62, 80);">方法去实现较简单，建议使用构造函数与原型混合方式</font>

```javascript
function Parent(){
	this.name = 'wang';
}

function Child(){
        this.age = 28;
}
    
Child.prototype = new Parent();//继承了Parent，通过原型

var demo = new Child();
alert(demo.age);
alert(demo.name);//得到被继承的属性
```

### <font style="color:rgb(44, 62, 80);">6 谈谈This对象的理解</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">总是指向函数的直接调用者（而非间接调用者）</font>
+ <font style="color:rgb(44, 62, 80);">如果有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);">关键字，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">指向</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);">出来的那个对象</font>
+ <font style="color:rgb(44, 62, 80);">在事件中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">指向触发这个事件的对象，特殊的是，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">attachEvent</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">总是指向全局对象</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Window</font>

### <font style="color:rgb(44, 62, 80);">7 事件模型</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">W3C</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中定义事件的发生经历三个阶段：捕获阶段（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">capturing</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）、目标阶段（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">targetin</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）、冒泡阶段（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bubbling</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）</font>

+ <font style="color:rgb(44, 62, 80);">冒泡型事件：当你使用事件冒泡时，子级元素先触发，父级元素后触发</font>
+ <font style="color:rgb(44, 62, 80);">捕获型事件：当你使用事件捕获时，父级元素先触发，子级元素后触发</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">事件流：同时支持两种事件模型：捕获型事件和冒泡型事件</font>
+ <font style="color:rgb(44, 62, 80);">阻止冒泡：在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">W3c</font><font style="color:rgb(44, 62, 80);">中，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stopPropagation()</font><font style="color:rgb(44, 62, 80);">方法；在IE下设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cancelBubble = true</font>
+ <font style="color:rgb(44, 62, 80);">阻止捕获：阻止事件的默认行为，例如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">click - <a></font><font style="color:rgb(44, 62, 80);">后的跳转。在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">W3c</font><font style="color:rgb(44, 62, 80);">中，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">preventDefault()</font><font style="color:rgb(44, 62, 80);">方法，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">下设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.event.returnValue = false</font>

### <font style="color:rgb(44, 62, 80);">8 new操作符具体干了什么呢?</font>
+ <font style="color:rgb(44, 62, 80);">创建一个空对象，并且</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">变量引用该对象，同时还继承了该函数的原型</font>
+ <font style="color:rgb(44, 62, 80);">属性和方法被加入到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">引用的对象中</font>
+ <font style="color:rgb(44, 62, 80);">新创建的对象由</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">所引用，并且最后隐式的返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font>

### <font style="color:rgb(44, 62, 80);">9 Ajax原理</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ajax</font><font style="color:rgb(44, 62, 80);">的原理简单来说是在用户和服务器之间加了—个中间层(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AJAX</font><font style="color:rgb(44, 62, 80);">引擎)，通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XmlHttpRequest</font><font style="color:rgb(44, 62, 80);">对象来向服务器发异步请求，从服务器获得数据，然后用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">javascrip</font><font style="color:rgb(44, 62, 80);">t来操作</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">而更新页面。使用户操作与服务器响应异步化。这其中最关键的一步就是从服务器获得请求数据</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ajax</font><font style="color:rgb(44, 62, 80);">的过程只涉及</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XMLHttpRequest</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XMLHttpRequest</font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ajax</font><font style="color:rgb(44, 62, 80);">的核心机制</font>

```javascript
// 手写简易ajax
/** 1. 创建连接 **/
var xhr = null;
xhr = new XMLHttpRequest()
/** 2. 连接服务器 **/
xhr.open('get', url, true)
/** 3. 发送请求 **/
xhr.send(null);
/** 4. 接受请求 **/
xhr.onreadystatechange = function(){
  if(xhr.readyState == 4){
    if(xhr.status == 200){
      success(xhr.responseText);
    } else { 
      /** false **/
      fail && fail(xhr.status);
    }
  }
}
```

```javascript
// promise封装
function ajax(url) {
  const p = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', url, true)
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (xhr.status === 200) {
          resolve(
            JSON.parse(xhr.responseText)
          )
        } else if (xhr.status === 404 || xhr.status === 500) {
          reject(new Error('404 not found'))
        }
      }
    }
    xhr.send(null)
  })
  return p
}
```

```plain
// 测试
const url = '/data/test.json'
ajax(url)
  .then(res => console.log(res))
  .catch(err => console.error(err))
```

**<font style="color:rgb(44, 62, 80);">ajax 有那些优缺点?</font>**

+ <font style="color:rgb(44, 62, 80);">优点：</font>
    - <font style="color:rgb(44, 62, 80);">通过异步模式，提升了用户体验.</font>
    - <font style="color:rgb(44, 62, 80);">优化了浏览器和服务器之间的传输，减少不必要的数据往返，减少了带宽占用.</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ajax</font><font style="color:rgb(44, 62, 80);">在客户端运行，承担了一部分本来由服务器承担的工作，减少了大用户量下的服务器负载。</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ajax</font><font style="color:rgb(44, 62, 80);">可以实现动态不刷新（局部刷新）</font>
+ <font style="color:rgb(44, 62, 80);">缺点：</font>
    - <font style="color:rgb(44, 62, 80);">安全问题</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AJAX</font><font style="color:rgb(44, 62, 80);">暴露了与服务器交互的细节。</font>
    - <font style="color:rgb(44, 62, 80);">对搜索引擎的支持比较弱。</font>
    - <font style="color:rgb(44, 62, 80);">不容易调试。</font>

### <font style="color:rgb(44, 62, 80);">10 如何解决跨域问题?</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">首先了解下浏览器的同源策略 同源策略</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">/SOP（Same origin policy）</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XSS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSRF</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">等攻击。所谓同源是指"</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">协议+域名+端口</font>**<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">"三者相同，即便两个不同的域名指向同一个ip地址，也非同源</font>

**<font style="color:rgb(44, 62, 80);">那么怎样解决跨域问题的呢？</font>**

+ **<font style="color:rgb(44, 62, 80);">通过jsonp跨域</font>**

```javascript
var script = document.createElement('script');
script.type = 'text/javascript';

// 传参并指定回调执行函数为onBack
script.src = 'http://www.....:8080/login?user=admin&callback=onBack';
document.head.appendChild(script);

// 回调执行函数
function onBack(res) {
    alert(JSON.stringify(res));
}
```

+ **<font style="color:rgb(44, 62, 80);">document.domain + iframe跨域</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">此方案仅限主域相同，子域不同的跨域应用场景</font>

<font style="color:rgb(44, 62, 80);">1.）父窗口：(http://www.domain.com/a.html)</font>

```html
<iframe id="iframe" src="http://child.domain.com/b.html"></iframe>
<script>
    document.domain = 'domain.com';
    var user = 'admin';
</script>
```

<font style="color:rgb(44, 62, 80);">2.）子窗口：(http://child.domain.com/b.html)</font>

```javascript
document.domain = 'domain.com';
// 获取父窗口中变量
alert('get js data from parent ---> ' + window.parent.user);
```

+ **<font style="color:rgb(44, 62, 80);">nginx代理跨域</font>**
+ **<font style="color:rgb(44, 62, 80);">nodejs中间件代理跨域</font>**
+ **<font style="color:rgb(44, 62, 80);">后端在头部信息里面设置安全域名</font>**

### [](https://www.123fe.net/docs/base.html#_11-%E6%A8%A1%E5%9D%97%E5%8C%96%E5%BC%80%E5%8F%91%E6%80%8E%E4%B9%88%E5%81%9A)<font style="color:rgb(44, 62, 80);">11 模块化开发怎么做？</font>
+ <font style="color:rgb(44, 62, 80);">立即执行函数,不暴露私有成员</font>

```javascript
var module1 = (function(){
　　　　var _count = 0;
　　　　var m1 = function(){
　　　　　　//...
　　　　};
　　　　var m2 = function(){
　　　　　　//...
　　　　};
　　　　return {
　　　　　　m1 : m1,
　　　　　　m2 : m2
　　　　};
})();
```

### <font style="color:rgb(44, 62, 80);">12 异步加载JS的方式有哪些？</font>
+ <font style="color:rgb(44, 62, 80);">设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><script></font><font style="color:rgb(44, 62, 80);">属性</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">async="async"</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">（一旦脚本可用，则会异步执行）</font>
+ <font style="color:rgb(44, 62, 80);">动态创建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script DOM</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document.createElement('script');</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XmlHttpRequest</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">脚本注入</font>
+ <font style="color:rgb(44, 62, 80);">异步加载库</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LABjs</font>
+ <font style="color:rgb(44, 62, 80);">模块加载器</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Sea.js</font>

### <font style="color:rgb(44, 62, 80);">13 那些操作会造成内存泄漏？</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">JavaScript 内存泄露指对象在不需要使用它时仍然存在，导致占用的内存不能使用或回收</font>

+ <font style="color:rgb(44, 62, 80);">未使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">声明的全局变量</font>
+ <font style="color:rgb(44, 62, 80);">闭包函数(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Closures</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(44, 62, 80);">循环引用(两个对象相互引用)</font>
+ <font style="color:rgb(44, 62, 80);">控制台日志(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">console.log</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(44, 62, 80);">移除存在绑定事件的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">元素(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的第一个参数使用字符串而非函数的话，会引发内存泄漏</font>
+ <font style="color:rgb(44, 62, 80);">垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收</font>

### <font style="color:rgb(44, 62, 80);">14 XML和JSON的区别？</font>
+ <font style="color:rgb(44, 62, 80);">数据体积方面</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">相对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">于XML</font><font style="color:rgb(44, 62, 80);">来讲，数据的体积小，传递的速度更快些。</font>
+ <font style="color:rgb(44, 62, 80);">数据交互方面</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">的交互更加方便，更容易解析处理，更好的数据交互</font>
+ <font style="color:rgb(44, 62, 80);">数据描述方面</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">对数据的描述性比</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XML</font><font style="color:rgb(44, 62, 80);">较差</font>
+ <font style="color:rgb(44, 62, 80);">传输速度方面</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">的速度要远远快于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XML</font>

### <font style="color:rgb(44, 62, 80);">15 谈谈你对webpack的看法</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WebPack</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是一个模块打包工具，你可以使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WebPack</font><font style="color:rgb(44, 62, 80);">管理你的模块依赖，并编绎输出模块们所需的静态文件。它能够很好地管理、打包</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Web</font><font style="color:rgb(44, 62, 80);">开发中所用到的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Javascript</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">webpack</font><font style="color:rgb(44, 62, 80);">有对应的模块加载器。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">webpack</font><font style="color:rgb(44, 62, 80);">模块打包器会分析模块间的依赖关系，最后 生成了优化且合并后的静态资源</font>

### <font style="color:rgb(44, 62, 80);">16 说说你对AMD和Commonjs的理解</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CommonJS</font><font style="color:rgb(44, 62, 80);">是服务器端模块的规范，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node.js</font><font style="color:rgb(44, 62, 80);">采用了这个规范。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CommonJS</font><font style="color:rgb(44, 62, 80);">规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AMD</font><font style="color:rgb(44, 62, 80);">规范则是非同步加载模块，允许指定回调函数</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AMD</font><font style="color:rgb(44, 62, 80);">推荐的风格通过返回一个对象做为模块对象，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CommonJS</font><font style="color:rgb(44, 62, 80);">的风格通过对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">module.exports</font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">exports</font><font style="color:rgb(44, 62, 80);">的属性赋值来达到暴露模块对象的目的</font>

### <font style="color:rgb(44, 62, 80);">17 常见web安全及防护原理</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sql</font><font style="color:rgb(44, 62, 80);">注入原理</font>
    - <font style="color:rgb(44, 62, 80);">就是通过把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SQL</font><font style="color:rgb(44, 62, 80);">命令插入到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Web</font><font style="color:rgb(44, 62, 80);">表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令</font>
+ <font style="color:rgb(44, 62, 80);">总的来说有以下几点</font>
    - <font style="color:rgb(44, 62, 80);">永远不要信任用户的输入，要对用户的输入进行校验，可以通过正则表达式，或限制长度，对单引号和双</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">"-"</font><font style="color:rgb(44, 62, 80);">进行转换等</font>
    - <font style="color:rgb(44, 62, 80);">永远不要使用动态拼装SQL，可以使用参数化的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SQL</font><font style="color:rgb(44, 62, 80);">或者直接使用存储过程进行数据查询存取</font>
    - <font style="color:rgb(44, 62, 80);">永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接</font>
    - <font style="color:rgb(44, 62, 80);">不要把机密信息明文存放，请加密或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hash</font><font style="color:rgb(44, 62, 80);">掉密码和敏感的信息</font>

**<font style="color:rgb(44, 62, 80);">XSS原理及防范</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Xss(cross-site scripting)</font><font style="color:rgb(44, 62, 80);">攻击指的是攻击者往</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Web</font><font style="color:rgb(44, 62, 80);">页面里插入恶意</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);">标签或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">javascript</font><font style="color:rgb(44, 62, 80);">代码。比如：攻击者在论坛中放一个看似安全的链接，骗取用户点击后，窃取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">中的用户私密信息；或者攻击者在论坛中加一个恶意表单，当用户提交表单的时候，却把信息传送到攻击者的服务器中，而不是用户原本以为的信任站点</font>

**<font style="color:rgb(44, 62, 80);">XSS防范方法</font>**

+ <font style="color:rgb(44, 62, 80);">首先代码里对用户输入的地方和变量都需要仔细检查长度和对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">”<”,”>”,”;”,”’”</font><font style="color:rgb(44, 62, 80);">等字符做过滤；其次任何内容写到页面之前都必须加以encode，避免不小心把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html tag</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">弄出来。这一个层面做好，至少可以堵住超过一半的XSS 攻击</font>

**<font style="color:rgb(44, 62, 80);">XSS与CSRF有什么区别吗？</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XSS</font><font style="color:rgb(44, 62, 80);">是获取信息，不需要提前知道其他用户页面的代码和数据包。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSRF</font><font style="color:rgb(44, 62, 80);">是代替用户完成指定的动作，需要知道其他用户页面的代码和数据包。要完成一次</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSRF</font><font style="color:rgb(44, 62, 80);">攻击，受害者必须依次完成两个步骤</font>
+ <font style="color:rgb(44, 62, 80);">登录受信任网站</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">A</font><font style="color:rgb(44, 62, 80);">，并在本地生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cookie</font>
+ <font style="color:rgb(44, 62, 80);">在不登出</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">A</font><font style="color:rgb(44, 62, 80);">的情况下，访问危险网站</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">B</font>

**<font style="color:rgb(44, 62, 80);">CSRF的防御</font>**

+ <font style="color:rgb(44, 62, 80);">服务端的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSRF</font><font style="color:rgb(44, 62, 80);">方式方法很多样，但总的思想都是一致的，就是在客户端页面增加伪随机数</font>
+ <font style="color:rgb(44, 62, 80);">通过验证码的方法</font>

### <font style="color:rgb(44, 62, 80);">18 用过哪些设计模式？</font>
+ <font style="color:rgb(44, 62, 80);">工厂模式：</font>
    - <font style="color:rgb(44, 62, 80);">工厂模式解决了重复实例化的问题，但还有一个问题,那就是识别问题，因为根本无法</font>
    - <font style="color:rgb(44, 62, 80);">主要好处就是可以消除对象间的耦合，通过使用工程方法而不是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);">关键字</font>
+ <font style="color:rgb(44, 62, 80);">构造函数模式</font>
    - <font style="color:rgb(44, 62, 80);">使用构造函数的方法，即解决了重复实例化的问题，又解决了对象识别的问题，该模式与工厂模式的不同之处在于</font>
    - <font style="color:rgb(44, 62, 80);">直接将属性和方法赋值给</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">对象;</font>

### <font style="color:rgb(44, 62, 80);">19 为什么要有同源限制？</font>
+ <font style="color:rgb(44, 62, 80);">同源策略指的是：协议，域名，端口相同，同源策略是一种安全协议</font>
+ <font style="color:rgb(44, 62, 80);">举例说明：比如一个黑客程序，他利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Iframe</font><font style="color:rgb(44, 62, 80);">把真正的银行登录页面嵌到他的页面上，当你使用真实的用户名，密码登录时，他的页面就可以通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Javascript</font><font style="color:rgb(44, 62, 80);">读取到你的表单中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">input</font><font style="color:rgb(44, 62, 80);">中的内容，这样用户名，密码就轻松到手了。</font>

### <font style="color:rgb(44, 62, 80);">20 offsetWidth/offsetHeight,clientWidth/clientHeight与scrollWidth/scrollHeight的区别</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">offsetWidth/offsetHeight</font><font style="color:rgb(44, 62, 80);">返回值包含</font>**<font style="color:rgb(44, 62, 80);">content + padding + border</font>**<font style="color:rgb(44, 62, 80);">，效果与e.getBoundingClientRect()相同</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clientWidth/clientHeight</font><font style="color:rgb(44, 62, 80);">返回值只包含</font>**<font style="color:rgb(44, 62, 80);">content + padding</font>**<font style="color:rgb(44, 62, 80);">，如果有滚动条，也</font>**<font style="color:rgb(44, 62, 80);">不包含滚动条</font>**
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scrollWidth/scrollHeight</font><font style="color:rgb(44, 62, 80);">返回值包含</font>**<font style="color:rgb(44, 62, 80);">content + padding + 溢出内容的尺寸</font>**

### <font style="color:rgb(44, 62, 80);">21 javascript有哪些方法定义对象</font>
+ <font style="color:rgb(44, 62, 80);">对象字面量：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var obj = {};</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">原型是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.prototype</font>
+ <font style="color:rgb(44, 62, 80);">构造函数：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var obj = new Object();</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.create()</font><font style="color:rgb(44, 62, 80);">:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var obj = Object.create(Object.prototype);</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.create(null)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">没有原型</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.create({...})</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可指定原型</font>

### <font style="color:rgb(44, 62, 80);">22 常见兼容性问题？</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">png24</font><font style="color:rgb(44, 62, 80);">位的图片在iE6浏览器上出现背景，解决方案是做成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PNG8</font>
+ <font style="color:rgb(44, 62, 80);">浏览器默认的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);">不同。解决方案是加一个全局的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">*{margin:0;padding:0;}</font><font style="color:rgb(44, 62, 80);">来统一,，但是全局效率很低，一般是如下这样解决：</font>

```css
body,ul,li,ol,dl,dt,dd,form,input,h1,h2,h3,h4,h5,h6,p{
margin:0;
padding:0;
}
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">下,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event</font><font style="color:rgb(44, 62, 80);">对象有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">x</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">y</font><font style="color:rgb(44, 62, 80);">属性,但是没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pageX</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pageY</font><font style="color:rgb(44, 62, 80);">属性</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Firefox</font><font style="color:rgb(44, 62, 80);">下,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event</font><font style="color:rgb(44, 62, 80);">对象有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pageX</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pageY</font><font style="color:rgb(44, 62, 80);">属性,但是没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">x,y</font><font style="color:rgb(44, 62, 80);">属性.</font>

### [](https://www.123fe.net/docs/base.html#_23-%E8%AF%B4%E8%AF%B4%E4%BD%A0%E5%AF%B9promise%E7%9A%84%E4%BA%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">23 说说你对promise的了解</font>
+ <font style="color:rgb(44, 62, 80);">依照</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise/A+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的定义，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">有四种状态：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pending:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">初始状态, 非</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fulfilled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rejected.</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fulfilled:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">成功的操作.</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rejected:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">失败的操作.</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">settled: Promise</font><font style="color:rgb(44, 62, 80);">已被</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fulfilled</font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rejected</font><font style="color:rgb(44, 62, 80);">，且不是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pending</font>
+ <font style="color:rgb(44, 62, 80);">另外，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fulfilled</font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rejected</font><font style="color:rgb(44, 62, 80);">一起合称</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">settled</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象用来进行延迟(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">deferred</font><font style="color:rgb(44, 62, 80);">) 和异步(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">asynchronous</font><font style="color:rgb(44, 62, 80);">) 计算</font>

**<font style="color:rgb(44, 62, 80);">Promise 的构造函数</font>**

+ <font style="color:rgb(44, 62, 80);">构造一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">，最基本的用法如下：</font>

```javascript
var promise = new Promise(function(resolve, reject) {

        if (...) {  // succeed

            resolve(result);

        } else {   // fails

            reject(Error(errMessage));

        }
    });
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实例拥有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">then</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法（具有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">then</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法的对象，通常被称为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">thenable</font><font style="color:rgb(44, 62, 80);">）。它的使用方法如下：</font>

```plain
promise.then(onFulfilled, onRejected)
```

+ <font style="color:rgb(44, 62, 80);">接收两个函数作为参数，一个在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fulfilled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的时候被调用，一个在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rejected</font><font style="color:rgb(44, 62, 80);">的时候被调用，接收参数就是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">future</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onFulfilled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对应</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">resolve</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onRejected</font><font style="color:rgb(44, 62, 80);">对应</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reject</font>

### [](https://www.123fe.net/docs/base.html#_24-%E4%BD%A0%E8%A7%89%E5%BE%97jquery%E6%BA%90%E7%A0%81%E6%9C%89%E5%93%AA%E4%BA%9B%E5%86%99%E7%9A%84%E5%A5%BD%E7%9A%84%E5%9C%B0%E6%96%B9)<font style="color:rgb(44, 62, 80);">24 你觉得jQuery源码有哪些写的好的地方</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jquery</font><font style="color:rgb(44, 62, 80);">源码封装在一个匿名函数的自执行环境中，有助于防止变量的全局污染，然后通过传入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font><font style="color:rgb(44, 62, 80);">对象参数，可以使</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font><font style="color:rgb(44, 62, 80);">对象作为局部变量使用，好处是当</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jquery</font><font style="color:rgb(44, 62, 80);">中访问</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font><font style="color:rgb(44, 62, 80);">对象的时候，就不用将作用域链退回到顶层作用域了，从而可以更快的访问window对象。同样，传入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">参数，可以缩短查找</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">时的作用域链</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jquery</font><font style="color:rgb(44, 62, 80);">将一些原型属性和方法封装在了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jquery.prototype</font><font style="color:rgb(44, 62, 80);">中，为了缩短名称，又赋值给了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jquery.fn</font><font style="color:rgb(44, 62, 80);">，这是很形象的写法</font>
+ <font style="color:rgb(44, 62, 80);">有一些数组或对象的方法经常能使用到，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jQuery</font><font style="color:rgb(44, 62, 80);">将其保存为局部变量以提高访问速度</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jquery</font><font style="color:rgb(44, 62, 80);">实现的链式调用可以节约代码，所返回的都是同一个对象，可以提高代码效率</font>

### [](https://www.123fe.net/docs/base.html#_25-vue%E3%80%81react%E3%80%81angular)<font style="color:rgb(44, 62, 80);">25 vue、react、angular</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue.js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一个用于创建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">web</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">交互界面的库，是一个精简的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MVVM</font><font style="color:rgb(44, 62, 80);">。它通过双向数据绑定把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">View</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">层和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Model</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">层连接了起来。实际的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">封装和输出格式都被抽象为了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Directives</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Filters</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AngularJS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是一个比较完善的前端</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MVVM</font><font style="color:rgb(44, 62, 80);">框架，包含模板，数据双向绑定，路由，模块化，服务，依赖注入等所有功能，模板功能强大丰富，自带了丰富的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Angular</font><font style="color:rgb(44, 62, 80);">指令</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">react</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">React</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">仅仅是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">VIEW</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">层是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">facebook</font><font style="color:rgb(44, 62, 80);">公司。推出的一个用于构建</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI</font><font style="color:rgb(44, 62, 80);">的一个库，能够实现服务器端的渲染。用了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">virtual dom</font><font style="color:rgb(44, 62, 80);">，所以性能很好。</font>

### [](https://www.123fe.net/docs/base.html#_26-node%E7%9A%84%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF)<font style="color:rgb(44, 62, 80);">26 Node的应用场景</font>
+ <font style="color:rgb(44, 62, 80);">特点：</font>
    - <font style="color:rgb(44, 62, 80);">1、它是一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Javascript</font><font style="color:rgb(44, 62, 80);">运行环境</font>
    - <font style="color:rgb(44, 62, 80);">2、依赖于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Chrome V8</font><font style="color:rgb(44, 62, 80);">引擎进行代码解释</font>
    - <font style="color:rgb(44, 62, 80);">3、事件驱动</font>
    - <font style="color:rgb(44, 62, 80);">4、非阻塞</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">I/O</font>
    - <font style="color:rgb(44, 62, 80);">5、单进程，单线程</font>
+ <font style="color:rgb(44, 62, 80);">优点：</font>
    - <font style="color:rgb(44, 62, 80);">高并发（最重要的优点）</font>
+ <font style="color:rgb(44, 62, 80);">缺点：</font>
    - <font style="color:rgb(44, 62, 80);">1、只支持单核</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CPU</font><font style="color:rgb(44, 62, 80);">，不能充分利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CPU</font>
    - <font style="color:rgb(44, 62, 80);">2、可靠性低，一旦代码某个环节崩溃，整个系统都崩溃</font>

### [](https://www.123fe.net/docs/base.html#_27-%E8%B0%88%E8%B0%88%E4%BD%A0%E5%AF%B9amd%E3%80%81cmd%E7%9A%84%E7%90%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">27 谈谈你对AMD、CMD的理解</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CommonJS</font><font style="color:rgb(44, 62, 80);">是服务器端模块的规范，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node.js</font><font style="color:rgb(44, 62, 80);">采用了这个规范。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CommonJS</font><font style="color:rgb(44, 62, 80);">规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AMD</font><font style="color:rgb(44, 62, 80);">规范则是非同步加载模块，允许指定回调函数</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AMD</font><font style="color:rgb(44, 62, 80);">推荐的风格通过返回一个对象做为模块对象，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CommonJS</font><font style="color:rgb(44, 62, 80);">的风格通过对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">module.exports</font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">exports</font><font style="color:rgb(44, 62, 80);">的属性赋值来达到暴露模块对象的目的</font>

**<font style="color:rgb(44, 62, 80);">es6模块 CommonJS、AMD、CMD</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CommonJS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的规范中，每个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">文件就是一个独立的模块上下文（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">module context</font><font style="color:rgb(44, 62, 80);">），在这个上下文中默认创建的属性都是私有的。也就是说，在一个文件定义的变量（还包括函数和类），都是私有的，对其他文件是不可见的。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CommonJS</font><font style="color:rgb(44, 62, 80);">是同步加载模块,在浏览器中会出现堵塞情况，所以不适用</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AMD</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">异步，需要定义回调</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">define</font><font style="color:rgb(44, 62, 80);">方式</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">es6</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一个模块就是一个独立的文件，该文件内部的所有变量，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">export</font><font style="color:rgb(44, 62, 80);">关键字输出该变量</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">es6</font><font style="color:rgb(44, 62, 80);">还可以导出类、方法，自动适用严格模式</font>

### [](https://www.123fe.net/docs/base.html#_28-%E9%82%A3%E4%BA%9B%E6%93%8D%E4%BD%9C%E4%BC%9A%E9%80%A0%E6%88%90%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F)<font style="color:rgb(44, 62, 80);">28 那些操作会造成内存泄漏</font>
+ <font style="color:rgb(44, 62, 80);">内存泄漏指任何对象在您不再拥有或需要它之后仍然存在</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的第一个参数使用字符串而非函数的话，会引发内存泄漏</font>
+ <font style="color:rgb(44, 62, 80);">闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）</font>

### [](https://www.123fe.net/docs/base.html#_29-web%E5%BC%80%E5%8F%91%E4%B8%AD%E4%BC%9A%E8%AF%9D%E8%B7%9F%E8%B8%AA%E7%9A%84%E6%96%B9%E6%B3%95%E6%9C%89%E5%93%AA%E4%BA%9B)<font style="color:rgb(44, 62, 80);">29 web开发中会话跟踪的方法有哪些</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">session</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">url</font><font style="color:rgb(44, 62, 80);">重写</font>
+ <font style="color:rgb(44, 62, 80);">隐藏</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">input</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ip</font><font style="color:rgb(44, 62, 80);">地址</font>

### [](https://www.123fe.net/docs/base.html#_30-js%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E5%92%8C%E5%BC%95%E7%94%A8%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)<font style="color:rgb(44, 62, 80);">30 JS的基本数据类型和引用数据类型</font>
+ <font style="color:rgb(44, 62, 80);">基本数据类型：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">boolean</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">number</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">string</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">symbol</font>
+ <font style="color:rgb(44, 62, 80);">引用数据类型：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">array</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font>

### [](https://www.123fe.net/docs/base.html#_31-%E4%BB%8B%E7%BB%8Djs%E6%9C%89%E5%93%AA%E4%BA%9B%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1)<font style="color:rgb(44, 62, 80);">31 介绍js有哪些内置对象</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中所有对象的父对象</font>
+ <font style="color:rgb(44, 62, 80);">数据封装类对象：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Boolean</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Number</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">String</font>
+ <font style="color:rgb(44, 62, 80);">其他对象：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Function</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Arguments</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Math</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Date</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">RegExp</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Error</font>

### [](https://www.123fe.net/docs/base.html#_32-%E8%AF%B4%E5%87%A0%E6%9D%A1%E5%86%99javascript%E7%9A%84%E5%9F%BA%E6%9C%AC%E8%A7%84%E8%8C%83)<font style="color:rgb(44, 62, 80);">32 说几条写JavaScript的基本规范</font>
+ <font style="color:rgb(44, 62, 80);">不要在同一行声明多个变量</font>
+ <font style="color:rgb(44, 62, 80);">请使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">===/!==</font><font style="color:rgb(44, 62, 80);">来比较</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true/false</font><font style="color:rgb(44, 62, 80);">或者数值</font>
+ <font style="color:rgb(44, 62, 80);">使用对象字面量替代</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new Array</font><font style="color:rgb(44, 62, 80);">这种形式</font>
+ <font style="color:rgb(44, 62, 80);">不要使用全局函数</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Switch</font><font style="color:rgb(44, 62, 80);">语句必须带有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">default</font><font style="color:rgb(44, 62, 80);">分支</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If</font><font style="color:rgb(44, 62, 80);">语句必须使用大括号</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for-in</font><font style="color:rgb(44, 62, 80);">循环中的变量 应该使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var</font><font style="color:rgb(44, 62, 80);">关键字明确限定作用域，从而避免作用域污</font>

### [](https://www.123fe.net/docs/base.html#_33-javascript%E6%9C%89%E5%87%A0%E7%A7%8D%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%80%BC)<font style="color:rgb(44, 62, 80);">33 JavaScript有几种类型的值</font>
+ <font style="color:rgb(44, 62, 80);">栈：原始数据类型（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Undefined</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Null</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Boolean</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Number</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">String</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">堆：引用数据类型（对象、数组和函数）</font>
+ <font style="color:rgb(44, 62, 80);">两种类型的区别是：存储位置不同；</font>
+ <font style="color:rgb(44, 62, 80);">原始数据类型直接存储在栈(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stack</font><font style="color:rgb(44, 62, 80);">)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；</font>
+ <font style="color:rgb(44, 62, 80);">引用数据类型存储在堆(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">heap</font><font style="color:rgb(44, 62, 80);">)中的对象,占据空间大、大小不固定,如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其</font>
+ <font style="color:rgb(44, 62, 80);">在栈中的地址，取得地址后从堆中获得实体</font>

### [](https://www.123fe.net/docs/base.html#_34-javascript%E5%88%9B%E5%BB%BA%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%87%A0%E7%A7%8D%E6%96%B9%E5%BC%8F)<font style="color:rgb(44, 62, 80);">34 javascript创建对象的几种方式</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">javascript</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">创建对象简单的说,无非就是使用内置对象或各种自定义对象，当然还可以用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">；但写法有很多种，也能混合使用</font>

+ <font style="color:rgb(44, 62, 80);">对象字面量的方式</font>

```javascript
person={firstname:"Mark",lastname:"Yun",age:25,eyecolor:"black"};
```

+ <font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);">来模拟无参的构造函数</font>

```javascript
function Person(){}
	var person=new Person();//定义一个function，如果使用new"实例化",该function可以看作是一个Class
        person.name="Mark";
        person.age="25";
        person.work=function(){
        alert(person.name+" hello...");
}
person.work();
```

+ <font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);">来模拟参构造函数来实现（用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">关键字定义构造的上下文属性）</font>

```javascript
function Pet(name,age,hobby){
       this.name=name;//this作用域：当前对象
       this.age=age;
       this.hobby=hobby;
       this.eat=function(){
           alert("我叫"+this.name+",我喜欢"+this.hobby+",是个程序员");
       }
}
var maidou =new Pet("麦兜",25,"coding");//实例化、创建对象
maidou.eat();//调用eat方法
```

+ <font style="color:rgb(44, 62, 80);">用工厂方式来创建（内置对象）</font>

```javascript
var wcDog =new Object();
     wcDog.name="旺财";
     wcDog.age=3;
     wcDog.work=function(){
       alert("我是"+wcDog.name+",汪汪汪......");
     }
     wcDog.work();
```

+ <font style="color:rgb(44, 62, 80);">用原型方式来创建</font>

```javascript
function Dog(){}
Dog.prototype.name="旺财";
Dog.prototype.eat=function(){
	alert(this.name+"是个吃货");
}
var wangcai =new Dog();
wangcai.eat();
```

+ <font style="color:rgb(44, 62, 80);">用混合方式来创建</font>

```javascript
function Car(name,price){
	this.name=name;
	this.price=price;
}
Car.prototype.sell=function(){
	alert("我是"+this.name+"，我现在卖"+this.price+"万元");
}
var camry =new Car("凯美瑞",27);
camry.sell();
```

### [](https://www.123fe.net/docs/base.html#_35-eval%E6%98%AF%E5%81%9A%E4%BB%80%E4%B9%88%E7%9A%84)<font style="color:rgb(44, 62, 80);">35 eval是做什么的</font>
+ <font style="color:rgb(44, 62, 80);">它的功能是把对应的字符串解析成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">代码并运行</font>
+ <font style="color:rgb(44, 62, 80);">应该避免使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eval</font><font style="color:rgb(44, 62, 80);">，不安全，非常耗性能（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">2</font><font style="color:rgb(44, 62, 80);">次，一次解析成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);">语句，一次执行）</font>
+ <font style="color:rgb(44, 62, 80);">由</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">字符串转换为JSON对象的时候可以用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eval，var obj =eval('('+ str +')')</font>

### [](https://www.123fe.net/docs/base.html#_36-null-undefined-%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">36 null，undefined 的区别</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">表示不存在这个值。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">:是一个表示"无"的原始值或者说表示"缺少值"，就是此处应该有一个值，但是还没有定义。当尝试读取时会返回</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font>
+ <font style="color:rgb(44, 62, 80);">例如变量被声明了，但没有赋值时，就等于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">表示一个对象被定义了，值为“空值”</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">: 是一个对象(空对象, 没有任何属性和方法)</font>
+ <font style="color:rgb(44, 62, 80);">例如作为函数的参数，表示该函数的参数不是对象；</font>
+ <font style="color:rgb(44, 62, 80);">在验证</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);">时，一定要使用 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">===</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">==</font><font style="color:rgb(44, 62, 80);">无法分别</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font>

### [](https://www.123fe.net/docs/base.html#_37-1-2-3-map-parseint-%E7%AD%94%E6%A1%88%E6%98%AF%E5%A4%9A%E5%B0%91)<font style="color:rgb(44, 62, 80);">37 ["1", "2", "3"].map(parseInt) 答案是多少</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[1, NaN, NaN]</font><font style="color:rgb(44, 62, 80);">因为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">parseInt</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">需要两个参数</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">(val, radix)</font><font style="color:rgb(44, 62, 80);">，其中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">radix</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">表示解析时用的基数。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">map</font><font style="color:rgb(44, 62, 80);">传了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">3</font><font style="color:rgb(44, 62, 80);">个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">(element, index, array)</font><font style="color:rgb(44, 62, 80);">，对应的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">radix</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不合法导致解析失败。</font>

### [](https://www.123fe.net/docs/base.html#_38-javascript-%E4%BB%A3%E7%A0%81%E4%B8%AD%E7%9A%84-use-strict-%E6%98%AF%E4%BB%80%E4%B9%88%E6%84%8F%E6%80%9D)<font style="color:rgb(44, 62, 80);">38 javascript 代码中的"use strict";是什么意思</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">use strict</font><font style="color:rgb(44, 62, 80);">是一种</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ECMAscript 5</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">添加的（严格）运行模式,这种模式使得 Javascript 在更严格的条件下运行,使</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">编码更加规范化的模式,消除</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Javascript</font><font style="color:rgb(44, 62, 80);">语法的一些不合理、不严谨之处，减少一些怪异行为</font>

### [](https://www.123fe.net/docs/base.html#_39-json-%E7%9A%84%E4%BA%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">39 JSON 的了解</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON(JavaScript Object Notation)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是一种轻量级的数据交换格式</font>
+ <font style="color:rgb(44, 62, 80);">它是基于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">的一个子集。数据格式简单, 易于读写, 占用带宽小</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">字符串转换为JSON对象:</font>

```javascript
var obj =eval('('+ str +')');
var obj = str.parseJSON();
var obj = JSON.parse(str);
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">对象转换为JSON字符串：</font>

```plain
var last=obj.toJSONString();
var last=JSON.stringify(obj);
```

### [](https://www.123fe.net/docs/base.html#_40-js%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD%E7%9A%84%E6%96%B9%E5%BC%8F%E6%9C%89%E5%93%AA%E4%BA%9B)<font style="color:rgb(44, 62, 80);">40 js延迟加载的方式有哪些</font>
+ <font style="color:rgb(44, 62, 80);">设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><script></font><font style="color:rgb(44, 62, 80);">属性</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defer="defer"</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">（脚本将在页面完成解析时执行）</font>
+ <font style="color:rgb(44, 62, 80);">动态创建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script DOM</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document.createElement('script');</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XmlHttpRequest</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">脚本注入</font>
+ <font style="color:rgb(44, 62, 80);">延迟加载工具</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LazyLoad</font>

### [](https://www.123fe.net/docs/base.html#_41-%E5%90%8C%E6%AD%A5%E5%92%8C%E5%BC%82%E6%AD%A5%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">41 同步和异步的区别</font>
+ <font style="color:rgb(44, 62, 80);">同步：浏览器访问服务器请求，用户看得到页面刷新，重新发请求,等请求完，页面刷新，新内容出现，用户看到新内容,进行下一步操作</font>
+ <font style="color:rgb(44, 62, 80);">异步：浏览器访问服务器请求，用户正常操作，浏览器后端进行请求。等请求完，页面不刷新，新内容也会出现，用户看到新内容</font>

### [](https://www.123fe.net/docs/base.html#_42-%E6%B8%90%E8%BF%9B%E5%A2%9E%E5%BC%BA%E5%92%8C%E4%BC%98%E9%9B%85%E9%99%8D%E7%BA%A7)<font style="color:rgb(44, 62, 80);">42 渐进增强和优雅降级</font>
+ <font style="color:rgb(44, 62, 80);">渐进增强 ：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。</font>
+ <font style="color:rgb(44, 62, 80);">优雅降级 ：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容</font>

### [](https://www.123fe.net/docs/base.html#_43-defer%E5%92%8Casync)<font style="color:rgb(44, 62, 80);">43 defer和async</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defer</font><font style="color:rgb(44, 62, 80);">并行加载</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);">文件，会按照页面上</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);">标签的顺序执行</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">async</font><font style="color:rgb(44, 62, 80);">并行加载</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);">文件，下载完成立即执行，不会按照页面上</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);">标签的顺序执行</font>

### [](https://www.123fe.net/docs/base.html#_44-%E8%AF%B4%E8%AF%B4%E4%B8%A5%E6%A0%BC%E6%A8%A1%E5%BC%8F%E7%9A%84%E9%99%90%E5%88%B6)<font style="color:rgb(44, 62, 80);">44 说说严格模式的限制</font>
+ <font style="color:rgb(44, 62, 80);">变量必须声明后再使用</font>
+ <font style="color:rgb(44, 62, 80);">函数的参数不能有同名属性，否则报错</font>
+ <font style="color:rgb(44, 62, 80);">不能使用with语句</font>
+ <font style="color:rgb(44, 62, 80);">不能对只读属性赋值，否则报错</font>
+ <font style="color:rgb(44, 62, 80);">不能使用前缀0表示八进制数，否则报错</font>
+ <font style="color:rgb(44, 62, 80);">不能删除不可删除的属性，否则报错</font>
+ <font style="color:rgb(44, 62, 80);">不能删除变量</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">delete prop</font><font style="color:rgb(44, 62, 80);">，会报错，只能删除属性</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">delete global[prop]</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eval</font><font style="color:rgb(44, 62, 80);">不会在它的外层作用域引入变量</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eval</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);">不能被重新赋值</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);">不会自动反映函数参数的变化</font>
+ <font style="color:rgb(44, 62, 80);">不能使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments.callee</font>
+ <font style="color:rgb(44, 62, 80);">不能使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments.caller</font>
+ <font style="color:rgb(44, 62, 80);">禁止</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">指向全局对象</font>
+ <font style="color:rgb(44, 62, 80);">不能使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fn.caller</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fn.arguments</font><font style="color:rgb(44, 62, 80);">获取函数调用的堆栈</font>
+ <font style="color:rgb(44, 62, 80);">增加了保留字（比如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">protected</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">static</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">interface</font><font style="color:rgb(44, 62, 80);">）</font>

### [](https://www.123fe.net/docs/base.html#_45-attribute%E5%92%8Cproperty%E7%9A%84%E5%8C%BA%E5%88%AB%E6%98%AF%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">45 attribute和property的区别是什么</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">attribute</font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">元素在文档中作为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);">标签拥有的属性；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">property</font><font style="color:rgb(44, 62, 80);">就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">元素在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);">中作为对象拥有的属性。</font>
+ <font style="color:rgb(44, 62, 80);">对于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);">的标准属性来说，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">attribute</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">property</font><font style="color:rgb(44, 62, 80);">是同步的，是会自动更新的</font>
+ <font style="color:rgb(44, 62, 80);">但是对于自定义的属性来说，他们是不同步的</font>

### [](https://www.123fe.net/docs/base.html#_46-%E8%B0%88%E8%B0%88%E4%BD%A0%E5%AF%B9es6%E7%9A%84%E7%90%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">46 谈谈你对ES6的理解</font>
+ <font style="color:rgb(44, 62, 80);">新增模板字符串（为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">提供了简单的字符串插值功能）</font>
+ <font style="color:rgb(44, 62, 80);">箭头函数</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for-of</font><font style="color:rgb(44, 62, 80);">（用来遍历数据—例如数组中的值。）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);">对象可被不定参数和默认参数完美代替。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(44, 62, 80);">将p</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">romise</font><font style="color:rgb(44, 62, 80);">对象纳入规范，提供了原生的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise</font><font style="color:rgb(44, 62, 80);">对象。</font>
+ <font style="color:rgb(44, 62, 80);">增加了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">const</font><font style="color:rgb(44, 62, 80);">命令，用来声明变量。</font>
+ <font style="color:rgb(44, 62, 80);">增加了块级作用域。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(44, 62, 80);">命令实际上就增加了块级作用域。</font>
+ <font style="color:rgb(44, 62, 80);">还有就是引入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">module</font><font style="color:rgb(44, 62, 80);">模块的概念</font>

### [](https://www.123fe.net/docs/base.html#_47-ecmascript6-%E6%80%8E%E4%B9%88%E5%86%99class%E4%B9%88)<font style="color:rgb(44, 62, 80);">47 ECMAScript6 怎么写class么</font>
+ <font style="color:rgb(44, 62, 80);">这个语法糖可以让有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">OOP</font><font style="color:rgb(44, 62, 80);">基础的人更快上手</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);">，至少是一个官方的实现了</font>
+ <font style="color:rgb(44, 62, 80);">但对熟悉</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);">的人来说，这个东西没啥大影响；一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.creat()</font><font style="color:rgb(44, 62, 80);">搞定继承，比</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(44, 62, 80);">简洁清晰的多</font>

### [](https://www.123fe.net/docs/base.html#_48-%E4%BB%80%E4%B9%88%E6%98%AF%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%BC%96%E7%A8%8B%E5%8F%8A%E9%9D%A2%E5%90%91%E8%BF%87%E7%A8%8B%E7%BC%96%E7%A8%8B-%E5%AE%83%E4%BB%AC%E7%9A%84%E5%BC%82%E5%90%8C%E5%92%8C%E4%BC%98%E7%BC%BA%E7%82%B9)<font style="color:rgb(44, 62, 80);">48 什么是面向对象编程及面向过程编程，它们的异同和优缺点</font>
+ <font style="color:rgb(44, 62, 80);">面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了</font>
+ <font style="color:rgb(44, 62, 80);">面向对象是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描叙某个事物在整个解决问题的步骤中的行为</font>
+ <font style="color:rgb(44, 62, 80);">面向对象是以功能来划分问题，而不是步骤</font>

### [](https://www.123fe.net/docs/base.html#_49-%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%BC%96%E7%A8%8B%E6%80%9D%E6%83%B3)<font style="color:rgb(44, 62, 80);">49 面向对象编程思想</font>
+ <font style="color:rgb(44, 62, 80);">基本思想是使用对象，类，继承，封装等基本概念来进行程序设计</font>
+ <font style="color:rgb(44, 62, 80);">优点</font>
    - <font style="color:rgb(44, 62, 80);">易维护</font>
        * <font style="color:rgb(44, 62, 80);">采用面向对象思想设计的结构，可读性高，由于继承的存在，即使改变需求，那么维护也只是在局部模块，所以维护起来是非常方便和较低成本的</font>
    - <font style="color:rgb(44, 62, 80);">易扩展</font>
    - <font style="color:rgb(44, 62, 80);">开发工作的重用性、继承性高，降低重复工作量。</font>
    - <font style="color:rgb(44, 62, 80);">缩短了开发周期</font>

### [](https://www.123fe.net/docs/base.html#_50-%E5%AF%B9web%E6%A0%87%E5%87%86%E3%80%81%E5%8F%AF%E7%94%A8%E6%80%A7%E3%80%81%E5%8F%AF%E8%AE%BF%E9%97%AE%E6%80%A7%E7%9A%84%E7%90%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">50 对web标准、可用性、可访问性的理解</font>
+ <font style="color:rgb(44, 62, 80);">可用性（Usability）：产品是否容易上手，用户能否完成任务，效率如何，以及这过程中用户的主观感受可好，是从用户的角度来看产品的质量。可用性好意味着产品质量高，是企业的核心竞争力</font>
+ <font style="color:rgb(44, 62, 80);">可访问性（Accessibility）：Web内容对于残障用户的可阅读和可理解性</font>
+ <font style="color:rgb(44, 62, 80);">可维护性（Maintainability）：一般包含两个层次，一是当系统出现问题时，快速定位并解决问题的成本，成本低则可维护性好。二是代码是否容易被人理解，是否容易修改和增强功能。</font>

### [](https://www.123fe.net/docs/base.html#_51-%E5%A6%82%E4%BD%95%E9%80%9A%E8%BF%87js%E5%88%A4%E6%96%AD%E4%B8%80%E4%B8%AA%E6%95%B0%E7%BB%84)<font style="color:rgb(44, 62, 80);">51 如何通过JS判断一个数组</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">instanceof</font><font style="color:rgb(44, 62, 80);">方法</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">instanceof</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">运算符是用来测试一个对象是否在其原型链原型构造函数的属性</font>

```javascript
var arr = [];
arr instanceof Array; // true
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">constructor</font><font style="color:rgb(44, 62, 80);">方法</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">constructor</font><font style="color:rgb(44, 62, 80);">属性返回对创建此对象的数组函数的引用，就是返回对象相对应的构造函数</font>

```javascript
var arr = [];
arr.constructor == Array; //true
```

+ <font style="color:rgb(44, 62, 80);">最简单的方法</font>
    - <font style="color:rgb(44, 62, 80);">这种写法，是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">jQuery</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">正在使用的</font>

```javascript
Object.prototype.toString.call(value) == '[object Array]'
// 利用这个方法，可以写一个返回数据类型的方法
var isType = function (obj) {
     return Object.prototype.toString.call(obj).slice(8,-1);
}
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES5</font><font style="color:rgb(44, 62, 80);">新增方法</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isArray()</font>

```javascript
var a = new Array(123);
var b = new Date();
console.log(Array.isArray(a)); //true
console.log(Array.isArray(b)); //false
```

### [](https://www.123fe.net/docs/base.html#_52-%E8%B0%88%E4%B8%80%E8%B0%88let%E4%B8%8Evar%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">52 谈一谈let与var的区别</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(44, 62, 80);">命令不存在变量提升，如果在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(44, 62, 80);">前使用，会导致报错</font>
+ <font style="color:rgb(44, 62, 80);">如果块区中存在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">const</font><font style="color:rgb(44, 62, 80);">命令，就会形成封闭作用域</font>
+ <font style="color:rgb(44, 62, 80);">不允许重复声明，因此，不能在函数内部重新声明参数</font>

### [](https://www.123fe.net/docs/base.html#_53-map%E4%B8%8Eforeach%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">53 map与forEach的区别</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forEach</font><font style="color:rgb(44, 62, 80);">方法，是最基本的方法，就是遍历与循环，默认有3个传参：分别是遍历的数组内容</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">item</font><font style="color:rgb(44, 62, 80);">、数组索引</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">index</font><font style="color:rgb(44, 62, 80);">、和当前遍历数组</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">map</font><font style="color:rgb(44, 62, 80);">方法，基本用法与</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forEach</font><font style="color:rgb(44, 62, 80);">一致，但是不同的，它会返回一个新的数组，所以在callback需要有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return</font><font style="color:rgb(44, 62, 80);">值，如果没有，会返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font>

### [](https://www.123fe.net/docs/base.html#_54-%E8%B0%88%E4%B8%80%E8%B0%88%E4%BD%A0%E7%90%86%E8%A7%A3%E7%9A%84%E5%87%BD%E6%95%B0%E5%BC%8F%E7%BC%96%E7%A8%8B)<font style="color:rgb(44, 62, 80);">54 谈一谈你理解的函数式编程</font>
+ <font style="color:rgb(44, 62, 80);">简单说，"函数式编程"是一种"编程范式"（programming paradigm），也就是如何编写程序的方法论</font>
+ <font style="color:rgb(44, 62, 80);">它具有以下特性：闭包和高阶函数、惰性计算、递归、函数是"第一等公民"、只用"表达式"</font>

### [](https://www.123fe.net/docs/base.html#_55-%E8%B0%88%E4%B8%80%E8%B0%88%E7%AE%AD%E5%A4%B4%E5%87%BD%E6%95%B0%E4%B8%8E%E6%99%AE%E9%80%9A%E5%87%BD%E6%95%B0%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">55 谈一谈箭头函数与普通函数的区别？</font>
+ <font style="color:rgb(44, 62, 80);">函数体内的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">对象，就是定义时所在的对象，而不是使用时所在的对象</font>
+ <font style="color:rgb(44, 62, 80);">不可以当作构造函数，也就是说，不可以使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);">命令，否则会抛出一个错误</font>
+ <font style="color:rgb(44, 62, 80);">不可以使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);">对象，该对象在函数体内不存在。如果要用，可以用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Rest</font><font style="color:rgb(44, 62, 80);">参数代替</font>
+ <font style="color:rgb(44, 62, 80);">不可以使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">yield</font><font style="color:rgb(44, 62, 80);">命令，因此箭头函数不能用作</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Generator</font><font style="color:rgb(44, 62, 80);">函数</font>

### [](https://www.123fe.net/docs/base.html#_56-%E8%B0%88%E4%B8%80%E8%B0%88%E5%87%BD%E6%95%B0%E4%B8%ADthis%E7%9A%84%E6%8C%87%E5%90%91)<font style="color:rgb(44, 62, 80);">56 谈一谈函数中this的指向</font>
+ <font style="color:rgb(44, 62, 80);">this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象</font>
+ <font style="color:rgb(44, 62, 80);">《javascript语言精髓》中大概概括了4种调用方式：</font>
+ <font style="color:rgb(44, 62, 80);">方法调用模式</font>
+ <font style="color:rgb(44, 62, 80);">函数调用模式</font>
+ <font style="color:rgb(44, 62, 80);">构造器调用模式</font>

```javascript
graph LR
A-->B
```

+ <font style="color:rgb(44, 62, 80);">apply/call调用模式</font>

### [](https://www.123fe.net/docs/base.html#_57-%E5%BC%82%E6%AD%A5%E7%BC%96%E7%A8%8B%E7%9A%84%E5%AE%9E%E7%8E%B0%E6%96%B9%E5%BC%8F)<font style="color:rgb(44, 62, 80);">57 异步编程的实现方式</font>
+ <font style="color:rgb(44, 62, 80);">回调函数</font>
    - <font style="color:rgb(44, 62, 80);">优点：简单、容易理解</font>
    - <font style="color:rgb(44, 62, 80);">缺点：不利于维护，代码耦合高</font>
+ <font style="color:rgb(44, 62, 80);">事件监听(采用时间驱动模式，取决于某个事件是否发生)：</font>
    - <font style="color:rgb(44, 62, 80);">优点：容易理解，可以绑定多个事件，每个事件可以指定多个回调函数</font>
    - <font style="color:rgb(44, 62, 80);">缺点：事件驱动型，流程不够清晰</font>
+ <font style="color:rgb(44, 62, 80);">发布/订阅(观察者模式)</font>
    - <font style="color:rgb(44, 62, 80);">类似于事件监听，但是可以通过‘消息中心’，了解现在有多少发布者，多少订阅者</font>
+ <font style="color:rgb(44, 62, 80);">Promise对象</font>
    - <font style="color:rgb(44, 62, 80);">优点：可以利用then方法，进行链式写法；可以书写错误时的回调函数；</font>
    - <font style="color:rgb(44, 62, 80);">缺点：编写和理解，相对比较难</font>
+ <font style="color:rgb(44, 62, 80);">Generator函数</font>
    - <font style="color:rgb(44, 62, 80);">优点：函数体内外的数据交换、错误处理机制</font>
    - <font style="color:rgb(44, 62, 80);">缺点：流程管理不方便</font>
+ <font style="color:rgb(44, 62, 80);">async函数</font>
    - <font style="color:rgb(44, 62, 80);">优点：内置执行器、更好的语义、更广的适用性、返回的是Promise、结构清晰。</font>
    - <font style="color:rgb(44, 62, 80);">缺点：错误处理机制</font>

### [](https://www.123fe.net/docs/base.html#_58-%E5%AF%B9%E5%8E%9F%E7%94%9Fjavascript%E4%BA%86%E8%A7%A3%E7%A8%8B%E5%BA%A6)<font style="color:rgb(44, 62, 80);">58 对原生Javascript了解程度</font>
+ <font style="color:rgb(44, 62, 80);">数据类型、运算、对象、Function、继承、闭包、作用域、原型链、事件、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">RegExp</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ajax</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BOM</font><font style="color:rgb(44, 62, 80);">、内存泄漏、跨域、异步装载、模板引擎、前端</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MVC</font><font style="color:rgb(44, 62, 80);">、路由、模块化、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Canvas</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ECMAScript</font>

### [](https://www.123fe.net/docs/base.html#_59-js%E5%8A%A8%E7%94%BB%E4%B8%8Ecss%E5%8A%A8%E7%94%BB%E5%8C%BA%E5%88%AB%E5%8F%8A%E7%9B%B8%E5%BA%94%E5%AE%9E%E7%8E%B0)<font style="color:rgb(44, 62, 80);">59 Js动画与CSS动画区别及相应实现</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);">的动画的优点</font>
    - <font style="color:rgb(44, 62, 80);">在性能上会稍微好一些，浏览器会对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);">的动画做一些优化</font>
    - <font style="color:rgb(44, 62, 80);">代码相对简单</font>
+ <font style="color:rgb(44, 62, 80);">缺点</font>
    - <font style="color:rgb(44, 62, 80);">在动画控制上不够灵活</font>
    - <font style="color:rgb(44, 62, 80);">兼容性不好</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">的动画正好弥补了这两个缺点，控制能力很强，可以单帧的控制、变换，同时写得好完全可以兼容</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE6</font><font style="color:rgb(44, 62, 80);">，并且功能强大。对于一些复杂控制的动画，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">javascript</font><font style="color:rgb(44, 62, 80);">会比较靠谱。而在实现一些小的交互动效的时候，就多考虑考虑</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">吧</font>

### [](https://www.123fe.net/docs/base.html#_60-js-%E6%95%B0%E7%BB%84%E5%92%8C%E5%AF%B9%E8%B1%A1%E7%9A%84%E9%81%8D%E5%8E%86%E6%96%B9%E5%BC%8F-%E4%BB%A5%E5%8F%8A%E5%87%A0%E7%A7%8D%E6%96%B9%E5%BC%8F%E7%9A%84%E6%AF%94%E8%BE%83)<font style="color:rgb(44, 62, 80);">60 JS 数组和对象的遍历方式，以及几种方式的比较</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">通常我们会用循环的方式来遍历数组。但是循环是 导致js 性能问题的原因之一。一般我们会采用下几种方式来进行数组的遍历</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for in</font><font style="color:rgb(44, 62, 80);">循环</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for</font><font style="color:rgb(44, 62, 80);">循环</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forEach</font>
    - <font style="color:rgb(44, 62, 80);">这里的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forEach</font><font style="color:rgb(44, 62, 80);">回调中两个参数分别为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">index</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forEach</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">无法遍历对象</font>
    - <font style="color:rgb(44, 62, 80);">IE不支持该方法；</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Firefox</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">chrome</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">支持</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forEach</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">无法使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">break</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">continue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">跳出循环，且使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是跳过本次循环</font>
+ <font style="color:rgb(44, 62, 80);">这两种方法应该非常常见且使用很频繁。但实际上，这两种方法都存在性能问题</font>
+ <font style="color:rgb(44, 62, 80);">在方式一中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for-in</font><font style="color:rgb(44, 62, 80);">需要分析出</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">array</font><font style="color:rgb(44, 62, 80);">的每个属性，这个操作性能开销很大。用在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">已知的数组上是非常不划算的。所以尽量不要用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for-in</font><font style="color:rgb(44, 62, 80);">，除非你不清楚要处理哪些属性，例如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">对象这样的情况</font>
+ <font style="color:rgb(44, 62, 80);">在方式2中，循环每进行一次，就要检查一下数组长度。读取属性（数组长度）要比读局部变量慢，尤其是当</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">array</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里存放的都是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素，因为每次读取都会扫描一遍页面上的选择器相关元素，速度会大大降低</font>

### [](https://www.123fe.net/docs/base.html#_61-gulp%E6%98%AF%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">61 gulp是什么</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">gulp</font><font style="color:rgb(44, 62, 80);">是前端开发过程中一种基于流的代码构建工具，是自动化项目的构建利器；它不仅能对网站资源进行优化，而且在开发过程中很多重复的任务能够使用正确的工具自动完成</font>
+ <font style="color:rgb(44, 62, 80);">Gulp的核心概念：流</font>
+ <font style="color:rgb(44, 62, 80);">流，简单来说就是建立在面向对象基础上的一种抽象的处理数据的工具。在流中，定义了一些处理数据的基本操作，如读取数据，写入数据等，程序员是对流进行所有操作的，而不用关心流的另一头数据的真正流向</font>
+ <font style="color:rgb(44, 62, 80);">gulp正是通过流和代码优于配置的策略来尽量简化任务编写的工作</font>
+ <font style="color:rgb(44, 62, 80);">Gulp的特点：</font>
    - **<font style="color:rgb(44, 62, 80);">易于使用</font>**<font style="color:rgb(44, 62, 80);">：通过代码优于配置的策略，gulp 让简单的任务简单，复杂的任务可管理</font>
    - **<font style="color:rgb(44, 62, 80);">构建快速</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">利用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node.js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">流的威力，你可以快速构建项目并减少频繁的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IO</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">操作</font>
    - **<font style="color:rgb(44, 62, 80);">易于学习</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">通过最少的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">API</font><font style="color:rgb(44, 62, 80);">，掌握</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">gulp</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">毫不费力，构建工作尽在掌握：如同一系列流管道</font>

### [](https://www.123fe.net/docs/base.html#_62-%E8%AF%B4%E4%B8%80%E4%B8%8Bvue%E7%9A%84%E5%8F%8C%E5%90%91%E7%BB%91%E5%AE%9A%E6%95%B0%E6%8D%AE%E7%9A%84%E5%8E%9F%E7%90%86)<font style="color:rgb(44, 62, 80);">62 说一下Vue的双向绑定数据的原理</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue.js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">则是采用数据劫持结合发布者-订阅者模式的方式，通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.defineProperty()</font><font style="color:rgb(44, 62, 80);">来劫持各个属性的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setter</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getter</font><font style="color:rgb(44, 62, 80);">，在数据变动时发布消息给订阅者，触发相应的监听回调</font>

### [](https://www.123fe.net/docs/base.html#_63-%E4%BA%8B%E4%BB%B6%E7%9A%84%E5%90%84%E4%B8%AA%E9%98%B6%E6%AE%B5)<font style="color:rgb(44, 62, 80);">63 事件的各个阶段</font>
+ <font style="color:rgb(44, 62, 80);">默认是冒泡</font>
+ <font style="color:rgb(44, 62, 80);">1：捕获阶段 ---> 2：目标阶段 ---> 3：冒泡阶段</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">---></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">target</font><font style="color:rgb(44, 62, 80);">目标 ----></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document</font>
+ <font style="color:rgb(44, 62, 80);">由此，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">addEventListener</font><font style="color:rgb(44, 62, 80);">的第三个参数设置为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">的区别已经非常清晰了</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">表示该元素在事件的“捕获阶段”（由外往内传递时）响应事件</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">表示该元素在事件的“冒泡阶段”（由内向外传递时）响应事件</font>

### [](https://www.123fe.net/docs/base.html#_64-let-var-const)<font style="color:rgb(44, 62, 80);">64 let var const</font>
**<font style="color:rgb(44, 62, 80);">let</font>**

+ <font style="color:rgb(44, 62, 80);">允许你声明一个作用域被限制在块级中的变量、语句或者表达式</font>
+ <font style="color:rgb(44, 62, 80);">let绑定不受变量提升的约束，这意味着let声明不会被提升到当前</font>
+ <font style="color:rgb(44, 62, 80);">该变量处于从块开始到初始化处理的“暂存死区”</font>

**<font style="color:rgb(44, 62, 80);">var</font>**

+ <font style="color:rgb(44, 62, 80);">声明变量的作用域限制在其声明位置的上下文中，而非声明变量总是全局的</font>
+ <font style="color:rgb(44, 62, 80);">由于变量声明（以及其他声明）总是在任意代码执行之前处理的，所以在代码中的任意位置声明变量总是等效于在代码开头声明</font>

**<font style="color:rgb(44, 62, 80);">const</font>**

+ <font style="color:rgb(44, 62, 80);">声明创建一个值的只读引用 (即指针)</font>
+ <font style="color:rgb(44, 62, 80);">基本数据当值发生改变时，那么其对应的指针也将发生改变，故造成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">const</font><font style="color:rgb(44, 62, 80);">申明基本数据类型时</font>
+ <font style="color:rgb(44, 62, 80);">再将其值改变时，将会造成报错， 例如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">const a = 3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a = 5</font><font style="color:rgb(44, 62, 80);">时 将会报错</font>
+ <font style="color:rgb(44, 62, 80);">但是如果是复合类型时，如果只改变复合类型的其中某个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Value</font><font style="color:rgb(44, 62, 80);">项时， 将还是正常使用</font>

### [](https://www.123fe.net/docs/base.html#_65-%E5%BF%AB%E9%80%9F%E7%9A%84%E8%AE%A9%E4%B8%80%E4%B8%AA%E6%95%B0%E7%BB%84%E4%B9%B1%E5%BA%8F)<font style="color:rgb(44, 62, 80);">65 快速的让一个数组乱序</font>
```javascript
var arr = [1,2,3,4,5,6,7,8,9,10];
arr.sort(function(){
    return Math.random() - 0.5;
})
console.log(arr);
```

### [](https://www.123fe.net/docs/base.html#_66-%E5%A6%82%E4%BD%95%E6%B8%B2%E6%9F%93%E5%87%A0%E4%B8%87%E6%9D%A1%E6%95%B0%E6%8D%AE%E5%B9%B6%E4%B8%8D%E5%8D%A1%E4%BD%8F%E7%95%8C%E9%9D%A2)<font style="color:rgb(44, 62, 80);">66 如何渲染几万条数据并不卡住界面</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这道题考察了如何在不卡住页面的情况下渲染数据，也就是说不能一次性将几万条都渲染出来，而应该一次渲染部分</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，那么就可以通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来每</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">16 ms</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">刷新一次</font>

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <ul>控件</ul>
  <script>
    setTimeout(() => {
      // 插入十万条数据
      const total = 100000
      // 一次插入 20 条，如果觉得性能不好就减少
      const once = 20
      // 渲染数据总共需要几次
      const loopCount = total / once
      let countOfRender = 0
      let ul = document.querySelector("ul");
      function add() {
        // 优化性能，插入不会造成回流
        const fragment = document.createDocumentFragment();
        for (let i = 0; i < once; i++) {
          const li = document.createElement("li");
          li.innerText = Math.floor(Math.random() * total);
          fragment.appendChild(li);
        }
        ul.appendChild(fragment);
        countOfRender += 1;
        loop();
      }
      function loop() {
        if (countOfRender < loopCount) {
          window.requestAnimationFrame(add);
        }
      }
      loop();
    }, 0);
  </script>
</body>
</html>
```

### [](https://www.123fe.net/docs/base.html#_67-%E5%B8%8C%E6%9C%9B%E8%8E%B7%E5%8F%96%E5%88%B0%E9%A1%B5%E9%9D%A2%E4%B8%AD%E6%89%80%E6%9C%89%E7%9A%84checkbox%E6%80%8E%E4%B9%88%E5%81%9A)<font style="color:rgb(44, 62, 80);">67 希望获取到页面中所有的checkbox怎么做？</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不使用第三方框架</font>

```plain
var domList = document.getElementsByTagName(‘input’)
 var checkBoxList = [];
 var len = domList.length;　　//缓存到局部变量
 while (len--) {　　//使用while的效率会比for循环更高
 　　if (domList[len].type == ‘checkbox’) {
     　　checkBoxList.push(domList[len]);
 　　}
 }
```

### [](https://www.123fe.net/docs/base.html#_68-%E6%80%8E%E6%A0%B7%E6%B7%BB%E5%8A%A0%E3%80%81%E7%A7%BB%E9%99%A4%E3%80%81%E7%A7%BB%E5%8A%A8%E3%80%81%E5%A4%8D%E5%88%B6%E3%80%81%E5%88%9B%E5%BB%BA%E5%92%8C%E6%9F%A5%E6%89%BE%E8%8A%82%E7%82%B9)<font style="color:rgb(44, 62, 80);">68 怎样添加、移除、移动、复制、创建和查找节点</font>
**<font style="color:rgb(44, 62, 80);">创建新节点</font>**

```plain
createDocumentFragment()    //创建一个DOM片段
createElement()   //创建一个具体的元素
createTextNode()   //创建一个文本节点
```

**<font style="color:rgb(44, 62, 80);">添加、移除、替换、插入</font>**

```plain
appendChild()      //添加
removeChild()      //移除
replaceChild()      //替换
insertBefore()      //插入
```

**<font style="color:rgb(44, 62, 80);">查找</font>**

```plain
getElementsByTagName()    //通过标签名称
getElementsByName()     //通过元素的Name属性的值
getElementById()        //通过元素Id，唯一性
```

### [](https://www.123fe.net/docs/base.html#_69-%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)<font style="color:rgb(44, 62, 80);">69 正则表达式</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">正则表达式构造函数</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var reg=new RegExp(“xxx”)</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">与正则表达字面量</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var reg=//</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">有什么不同？匹配邮箱的正则表达式？</font>

+ <font style="color:rgb(44, 62, 80);">当使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">RegExp()</font><font style="color:rgb(44, 62, 80);">构造函数的时候，不仅需要转义引号（即</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">\</font><font style="color:rgb(44, 62, 80);">”表示”），并且还需要双反斜杠（即</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">\\</font><font style="color:rgb(44, 62, 80);">表示一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">\</font><font style="color:rgb(44, 62, 80);">）。使用正则表达字面量的效率更高</font>

<font style="color:rgb(44, 62, 80);">邮箱的正则匹配：</font>

```plain
var regMail = /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+((.[a-zA-Z0-9_-]{2,3}){1,2})$/;
```

### [](https://www.123fe.net/docs/base.html#_70-javascript%E4%B8%ADcallee%E5%92%8Ccaller%E7%9A%84%E4%BD%9C%E7%94%A8)<font style="color:rgb(44, 62, 80);">70 Javascript中callee和caller的作用？</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">caller</font><font style="color:rgb(44, 62, 80);">是返回一个对函数的引用，该函数调用了当前函数；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callee</font><font style="color:rgb(44, 62, 80);">是返回正在被执行的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);">函数，也就是所指定的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);">对象的正文</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">那么问题来了？如果一对兔子每月生一对兔子；一对新生兔，从第二个月起就开始生兔子；假定每对兔子都是一雌一雄，试问一对兔子，第n个月能繁殖成多少对兔子？（使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callee</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">完成）</font>

```plain
var result=[];
  function fn(n){  //典型的斐波那契数列
     if(n==1){
          return 1;
     }else if(n==2){
             return 1;
     }else{
          if(result[n]){
                  return result[n];
         }else{
                 //argument.callee()表示fn()
                 result[n]=arguments.callee(n-1)+arguments.callee(n-2);
                 return result[n];
         }
    }
 }
```

### [](https://www.123fe.net/docs/base.html#_71-window-onload%E5%92%8C-document-ready)<font style="color:rgb(44, 62, 80);">71 window.onload和$(document).ready</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">原生</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onload</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">与</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Jquery</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$(document).ready(function(){})</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">有什么不同？如何用原生JS实现Jq的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ready</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法？</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.onload()</font><font style="color:rgb(44, 62, 80);">方法是必须等到页面内包括图片的所有元素加载完毕后才能执行。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$(document).ready()</font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">结构绘制完毕后就执行，不必等到加载完毕</font>

```plain
function ready(fn){
      if(document.addEventListener) {        //标准浏览器
          document.addEventListener('DOMContentLoaded', function() {
              //注销事件, 避免反复触发
              document.removeEventListener('DOMContentLoaded',arguments.callee, false);
              fn();            //执行函数
          }, false);
      }else if(document.attachEvent) {        //IE
          document.attachEvent('onreadystatechange', function() {
             if(document.readyState == 'complete') {
                 document.detachEvent('onreadystatechange', arguments.callee);
                 fn();        //函数执行
             }
         });
     }
 };
```

### [](https://www.123fe.net/docs/base.html#_72-addeventlistener-%E5%92%8Cattachevent-%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">72 addEventListener()和attachEvent()的区别</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">addEventListener()</font><font style="color:rgb(44, 62, 80);">是符合W3C规范的标准方法;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">attachEvent()</font><font style="color:rgb(44, 62, 80);">是IE低版本的非标准方法</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">addEventListener()</font><font style="color:rgb(44, 62, 80);">支持事件冒泡和事件捕获; - 而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">attachEvent()</font><font style="color:rgb(44, 62, 80);">只支持事件冒泡</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">addEventListener()</font><font style="color:rgb(44, 62, 80);">的第一个参数中,事件类型不需要添加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">on</font><font style="color:rgb(44, 62, 80);">;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">attachEvent()</font><font style="color:rgb(44, 62, 80);">需要添加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">'on'</font>
+ <font style="color:rgb(44, 62, 80);">如果为同一个元素绑定多个事件,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">addEventListener()</font><font style="color:rgb(44, 62, 80);">会按照事件绑定的顺序依次执行,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">attachEvent()</font><font style="color:rgb(44, 62, 80);">会按照事件绑定的顺序倒序执行</font>

### [](https://www.123fe.net/docs/base.html#_73-%E8%8E%B7%E5%8F%96%E9%A1%B5%E9%9D%A2%E6%89%80%E6%9C%89%E7%9A%84checkbox)<font style="color:rgb(44, 62, 80);">73 获取页面所有的checkbox</font>
```plain
var resultArr= [];
var input = document.querySelectorAll('input');
for( var i = 0; i < input.length; i++ ) {
    if( input[i].type == 'checkbox' ) {
        resultArr.push( input[i] );
    }
}
//resultArr即中获取到了页面中的所有checkbox
```

### [](https://www.123fe.net/docs/base.html#_74-%E6%95%B0%E7%BB%84%E5%8E%BB%E9%87%8D%E6%96%B9%E6%B3%95%E6%80%BB%E7%BB%93)<font style="color:rgb(44, 62, 80);">74 数组去重方法总结</font>
**<font style="color:rgb(44, 62, 80);">方法一、利用ES6 Set去重（ES6中最常用）</font>**

```plain
function unique (arr) {
  return Array.from(new Set(arr))
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
 //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {}, {}]
```

**<font style="color:rgb(44, 62, 80);">方法二、利用for嵌套for，然后splice去重（ES5中最常用）</font>**

```plain
function unique(arr){            
        for(var i=0; i<arr.length; i++){
            for(var j=i+1; j<arr.length; j++){
                if(arr[i]==arr[j]){         //第一个等同于第二个，splice方法删除第二个
                    arr.splice(j,1);
                    j--;
                }
            }
        }
	return arr;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
    //[1, "true", 15, false, undefined, NaN, NaN, "NaN", "a", {…}, {…}]     //NaN和{}没有去重，两个null直接消失了
```

+ <font style="color:rgb(44, 62, 80);">双层循环，外层循环元素，内层循环时比较值。值相同时，则删去这个值。</font>
+ <font style="color:rgb(44, 62, 80);">想快速学习更多常用的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(44, 62, 80);">语法</font>

**<font style="color:rgb(44, 62, 80);">方法三、利用indexOf去重</font>**

```plain
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var array = [];
    for (var i = 0; i < arr.length; i++) {
        if (array .indexOf(arr[i]) === -1) {
            array .push(arr[i])
        }
    }
    return array;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
   // [1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]  //NaN、{}没有去重
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">新建一个空的结果数组，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">循环原数组，判断结果数组是否存在当前元素，如果有相同的值则跳过，不相同则</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">push</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">进数组</font>

**<font style="color:rgb(44, 62, 80);">方法四、利用sort()</font>**

```plain
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return;
    }
    arr = arr.sort()
    var arrry= [arr[0]];
    for (var i = 1; i < arr.length; i++) {
        if (arr[i] !== arr[i-1]) {
            arrry.push(arr[i]);
        }
    }
    return arrry;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
// [0, 1, 15, "NaN", NaN, NaN, {…}, {…}, "a", false, null, true, "true", undefined]      //NaN、{}没有去重
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sort()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">排序方法，然后根据排序后的结果进行遍历及相邻元素比对</font>

**<font style="color:rgb(44, 62, 80);">方法五、利用对象的属性不能相同的特点进行去重</font>**

```plain
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var arrry= [];
     var  obj = {};
    for (var i = 0; i < arr.length; i++) {
        if (!obj[arr[i]]) {
            arrry.push(arr[i])
            obj[arr[i]] = 1
        } else {
            obj[arr[i]]++
        }
    }
    return arrry;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
//[1, "true", 15, false, undefined, null, NaN, 0, "a", {…}]    //两个true直接去掉了，NaN和{}去重
```

**<font style="color:rgb(44, 62, 80);">方法六、利用includes</font>**

```plain
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var array =[];
    for(var i = 0; i < arr.length; i++) {
            if( !array.includes( arr[i]) ) {//includes 检测数组是否有某个值
                    array.push(arr[i]);
              }
    }
    return array
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
    //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]     //{}没有去重
```

**<font style="color:rgb(44, 62, 80);">方法七、利用hasOwnProperty</font>**

```plain
function unique(arr) {
    var obj = {};
    return arr.filter(function(item, index, arr){
        return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = true)
    })
}
    var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
        console.log(unique(arr))
//[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}]   //所有的都去重了
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hasOwnProperty</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">判断是否存在对象属性</font>

**<font style="color:rgb(44, 62, 80);">方法八、利用filter</font>**

```plain
function unique(arr) {
  return arr.filter(function(item, index, arr) {
    //当前元素，在原始数组中的第一个索引==当前索引值，否则返回当前元素
    return arr.indexOf(item, 0) === index;
  });
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
//[1, "true", true, 15, false, undefined, null, "NaN", 0, "a", {…}, {…}]
```

**<font style="color:rgb(44, 62, 80);">方法九、利用递归去重</font>**

```plain
function unique(arr) {
    var array= arr;
    var len = array.length;

	array.sort(function(a,b){   //排序后更加方便去重
		return a - b;
	})

	function loop(index){
        if(index >= 1){
            if(array[index] === array[index-1]){
            array.splice(index,1);
            }
            loop(index - 1);    //递归loop，然后数组去重
        }
	}
	loop(len-1);
	return array;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
//[1, "a", "true", true, 15, false, 1, {…}, null, NaN, NaN, "NaN", 0, "a", {…}, undefined]
```

**<font style="color:rgb(44, 62, 80);">方法十、利用Map数据结构去重</font>**

```plain
function arrayNonRepeatfy(arr) {
	let map = new Map();
		let array = new Array();  // 数组用于返回结果
		for (let i = 0; i < arr.length; i++) {
			if(map .has(arr[i])) {  // 如果有该key值
			map .set(arr[i], true);
		} else {
			map .set(arr[i], false);   // 如果没有该key值
			array .push(arr[i]);
		}
	}
	return array ;
}
 var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
    console.log(unique(arr))
//[1, "a", "true", true, 15, false, 1, {…}, null, NaN, NaN, "NaN", 0, "a", {…}, undefined]
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">创建一个空</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Map</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">数据结构，遍历需要去重的数组，把数组的每一个元素作为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">存到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Map</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中。由于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Map</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中不会出现相同的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">key</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">值，所以最终得到的就是去重后的结果</font>

**<font style="color:rgb(44, 62, 80);">方法十一、利用reduce+includes</font>**

```plain
function unique(arr){
    return arr.reduce((prev,cur) => prev.includes(cur) ? prev : [...prev,cur],[]);
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr));
// [1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
```

**<font style="color:rgb(44, 62, 80);">方法十二、[...new Set(arr)]</font>**

```plain
[...new Set(arr)]
//代码就是这么少----（其实，严格来说并不算是一种，相对于第一种方法来说只是简化了代码）
```

### [](https://www.123fe.net/docs/base.html#_75-%E8%AE%BE%E8%AE%A1%E9%A2%98-%E6%83%B3%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AA%E5%AF%B9%E9%A1%B5%E9%9D%A2%E6%9F%90%E4%B8%AA%E8%8A%82%E7%82%B9%E7%9A%84%E6%8B%96%E6%9B%B3-%E5%A6%82%E4%BD%95%E5%81%9A-%E4%BD%BF%E7%94%A8%E5%8E%9F%E7%94%9Fjs)<font style="color:rgb(44, 62, 80);">75 （设计题）想实现一个对页面某个节点的拖曳？如何做？（使用原生JS）</font>
+ <font style="color:rgb(44, 62, 80);">给需要拖拽的节点绑定</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mousedown</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mousemove</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mouseup</font><font style="color:rgb(44, 62, 80);">事件</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mousedown</font><font style="color:rgb(44, 62, 80);">事件触发后，开始拖拽</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mousemove</font><font style="color:rgb(44, 62, 80);">时，需要通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event.clientX</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clientY</font><font style="color:rgb(44, 62, 80);">获取拖拽位置，并实时更新位置</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mouseup</font><font style="color:rgb(44, 62, 80);">时，拖拽结束</font>
+ <font style="color:rgb(44, 62, 80);">需要注意浏览器边界的情况</font>

### [](https://www.123fe.net/docs/base.html#_76-javascript%E5%85%A8%E5%B1%80%E5%87%BD%E6%95%B0%E5%92%8C%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F)<font style="color:rgb(44, 62, 80);">76 Javascript全局函数和全局变量</font>
**<font style="color:rgb(44, 62, 80);">全局变量</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Infinity</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">代表正的无穷大的数值。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NaN</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指示某个值是不是数字值。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指示未定义的值。</font>

**<font style="color:rgb(44, 62, 80);">全局函数</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">decodeURI()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">解码某个编码的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">URI</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">decodeURIComponent()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">解码一个编码的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">URI</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">encodeURI()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">把字符串编码为 URI。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">encodeURIComponent()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">把字符串编码为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">URI</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组件。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">escape()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对字符串进行编码。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eval()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">计算</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">字符串，并把它作为脚本代码来执行。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isFinite()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">检查某个值是否为有穷大的数。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">isNaN()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">检查某个值是否是数字。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Number()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">把对象的值转换为数字。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">parseFloat()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">解析一个字符串并返回一个浮点数。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">parseInt()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">解析一个字符串并返回一个整数。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">String()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">把对象的值转换为字符串。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unescape()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对由</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">escape()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">编码的字符串进行解码</font>

### [](https://www.123fe.net/docs/base.html#_77-%E4%BD%BF%E7%94%A8js%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AA%E6%8C%81%E7%BB%AD%E7%9A%84%E5%8A%A8%E7%94%BB%E6%95%88%E6%9E%9C)<font style="color:rgb(44, 62, 80);">77 使用js实现一个持续的动画效果</font>
**<font style="color:rgb(44, 62, 80);">定时器思路</font>**

```plain
var e = document.getElementById('e')
var flag = true;
var left = 0;
setInterval(() => {
    left == 0 ? flag = true : left == 100 ? flag = false : ''
    flag ? e.style.left = ` ${left++}px` : e.style.left = ` ${left--}px`
}, 1000 / 60)
```

**<font style="color:rgb(44, 62, 80);">requestAnimationFrame</font>**

```plain
//兼容性处理
window.requestAnimFrame = (function(){
    return window.requestAnimationFrame       ||
           window.webkitRequestAnimationFrame ||
           window.mozRequestAnimationFrame    ||
           function(callback){
                window.setTimeout(callback, 1000 / 60);
           };
})();

var e = document.getElementById("e");
var flag = true;
var left = 0;

function render() {
    left == 0 ? flag = true : left == 100 ? flag = false : '';
    flag ? e.style.left = ` ${left++}px` :
        e.style.left = ` ${left--}px`;
}

(function animloop() {
    render();
    requestAnimFrame(animloop);
})();
```

**<font style="color:rgb(44, 62, 80);">使用css实现一个持续的动画效果</font>**

```css
animation:mymove 5s infinite;

@keyframes mymove {
    from {top:0px;}
    to {top:200px;}
}
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-name</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定需要绑定到选择器的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">keyframe</font><font style="color:rgb(44, 62, 80);">名称。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-duration</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定完成动画所花费的时间，以秒或毫秒计。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-timing-function</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定动画的速度曲线。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-delay</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定在动画开始之前的延迟。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-iteration-count</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定动画应该播放的次数。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-direction</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定是否应该轮流反向播放动画</font>

### [](https://www.123fe.net/docs/base.html#_78-%E5%B0%81%E8%A3%85%E4%B8%80%E4%B8%AA%E5%87%BD%E6%95%B0-%E5%8F%82%E6%95%B0%E6%98%AF%E5%AE%9A%E6%97%B6%E5%99%A8%E7%9A%84%E6%97%B6%E9%97%B4-then%E6%89%A7%E8%A1%8C%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0)<font style="color:rgb(44, 62, 80);">78 封装一个函数，参数是定时器的时间，.then执行回调函数</font>
```plain
function sleep (time) {
    return new Promise((resolve) => setTimeout(resolve, time));
}
```

### [](https://www.123fe.net/docs/base.html#_79-%E6%80%8E%E4%B9%88%E5%88%A4%E6%96%AD%E4%B8%A4%E4%B8%AA%E5%AF%B9%E8%B1%A1%E7%9B%B8%E7%AD%89)<font style="color:rgb(44, 62, 80);">79 怎么判断两个对象相等？</font>
```plain
obj={
    a:1,
    b:2
}
obj2={
    a:1,
    b:2
}
obj3={
    a:1,
    b:'2'
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以转换为字符串来判断</font>

```plain
JSON.stringify(obj)==JSON.stringify(obj2);//true
JSON.stringify(obj)==JSON.stringify(obj3);//false
```

### [](https://www.123fe.net/docs/base.html#_80-%E9%A1%B9%E7%9B%AE%E5%81%9A%E8%BF%87%E5%93%AA%E4%BA%9B%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96)<font style="color:rgb(44, 62, 80);">80 项目做过哪些性能优化？</font>
+ <font style="color:rgb(44, 62, 80);">减少</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">请求数</font>
+ <font style="color:rgb(44, 62, 80);">减少</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DNS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">查询</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CDN</font>
+ <font style="color:rgb(44, 62, 80);">避免重定向</font>
+ <font style="color:rgb(44, 62, 80);">图片懒加载</font>
+ <font style="color:rgb(44, 62, 80);">减少</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素数量</font>
+ <font style="color:rgb(44, 62, 80);">减少</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">操作</font>
+ <font style="color:rgb(44, 62, 80);">使用外部</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font>
+ <font style="color:rgb(44, 62, 80);">压缩</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">、字体、图片等</font>
+ <font style="color:rgb(44, 62, 80);">优化</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS Sprite</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">iconfont</font>
+ <font style="color:rgb(44, 62, 80);">字体裁剪</font>
+ <font style="color:rgb(44, 62, 80);">多域名分发划分内容到不同域名</font>
+ <font style="color:rgb(44, 62, 80);">尽量减少</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">iframe</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">使用</font>
+ <font style="color:rgb(44, 62, 80);">避免图片</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">src</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为空</font>
+ <font style="color:rgb(44, 62, 80);">把样式表放在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中</font>
+ <font style="color:rgb(44, 62, 80);">把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">放在页面底部</font>

### [](https://www.123fe.net/docs/base.html#_81-%E6%B5%8F%E8%A7%88%E5%99%A8%E7%BC%93%E5%AD%98)<font style="color:rgb(44, 62, 80);">81 浏览器缓存</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">浏览器缓存分为强缓存和协商缓存。当客户端请求某个资源时，获取缓存的流程如下</font>

+ <font style="color:rgb(44, 62, 80);">先根据这个资源的一些</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">http header</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">判断它是否命中强缓存，如果命中，则直接从本地获取缓存资源，不会发请求到服务器；</font>
+ <font style="color:rgb(44, 62, 80);">当强缓存没有命中时，客户端会发送请求到服务器，服务器通过另一些</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">request header</font><font style="color:rgb(44, 62, 80);">验证这个资源是否命中协商缓存，称为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">http</font><font style="color:rgb(44, 62, 80);">再验证，如果命中，服务器将请求返回，但不返回资源，而是告诉客户端直接从缓存中获取，客户端收到返回后就会从缓存中获取资源；</font>
+ <font style="color:rgb(44, 62, 80);">强缓存和协商缓存共同之处在于，如果命中缓存，服务器都不会返回资源； 区别是，强缓存不对发送请求到服务器，但协商缓存会。</font>
+ <font style="color:rgb(44, 62, 80);">当协商缓存也没命中时，服务器就会将资源发送回客户端。</font>
+ <font style="color:rgb(44, 62, 80);">当</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ctrl+f5</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">强制刷新网页时，直接从服务器加载，跳过强缓存和协商缓存；</font>
+ <font style="color:rgb(44, 62, 80);">当</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">f5</font><font style="color:rgb(44, 62, 80);">刷新网页时，跳过强缓存，但是会检查协商缓存；</font>

**<font style="color:rgb(44, 62, 80);">强缓存</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Expires</font><font style="color:rgb(44, 62, 80);">（该字段是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">http1.0</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">时的规范，值为一个绝对时间的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GMT</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">格式的时间字符串，代表缓存资源的过期时间）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache-Control:max-age</font><font style="color:rgb(44, 62, 80);">（该字段是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">http1.1</font><font style="color:rgb(44, 62, 80);">的规范，强缓存利用其</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">max-age</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值来判断缓存资源的最大生命周期，它的值单位为秒）</font>

**<font style="color:rgb(44, 62, 80);">协商缓存</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-Modified</font><font style="color:rgb(44, 62, 80);">（值为资源最后更新时间，随服务器response返回）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-Modified-Since</font><font style="color:rgb(44, 62, 80);">（通过比较两个时间来判断资源在两次请求期间是否有过修改，如果没有修改，则命中协商缓存）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ETag</font><font style="color:rgb(44, 62, 80);">（表示资源内容的唯一标识，随服务器</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">response</font><font style="color:rgb(44, 62, 80);">返回）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-None-Match</font><font style="color:rgb(44, 62, 80);">（服务器通过比较请求头部的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-None-Match</font><font style="color:rgb(44, 62, 80);">与当前资源的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ETag</font><font style="color:rgb(44, 62, 80);">是否一致来判断资源是否在两次请求之间有过修改，如果没有修改，则命中协商缓存）</font>

### [](https://www.123fe.net/docs/base.html#_82-websocket)<font style="color:rgb(44, 62, 80);">82 WebSocket</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">由于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">http</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">存在一个明显的弊端（消息只能有客户端推送到服务器端，而服务器端不能主动推送到客户端），导致如果服务器如果有连续的变化，这时只能使用轮询，而轮询效率过低，并不适合。于是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">WebSocket</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">被发明出来</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">相比与</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">http</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">具有以下有点</font>

+ <font style="color:rgb(44, 62, 80);">支持双向通信，实时性更强；</font>
+ <font style="color:rgb(44, 62, 80);">可以发送文本，也可以二进制文件；</font>
+ <font style="color:rgb(44, 62, 80);">协议标识符是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ws</font><font style="color:rgb(44, 62, 80);">，加密后是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">wss</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">；</font>
+ <font style="color:rgb(44, 62, 80);">较少的控制开销。连接创建后，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ws</font><font style="color:rgb(44, 62, 80);">客户端、服务端进行数据交换时，协议控制的数据包头部较小。在不包含头部的情况下，服务端到客户端的包头只有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">2~10</font><font style="color:rgb(44, 62, 80);">字节（取决于数据包长度），客户端到服务端的的话，需要加上额外的4字节的掩码。而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">协议每次通信都需要携带完整的头部；</font>
+ <font style="color:rgb(44, 62, 80);">支持扩展。ws协议定义了扩展，用户可以扩展协议，或者实现自定义的子协议。（比如支持自定义压缩算法等）</font>
+ <font style="color:rgb(44, 62, 80);">无跨域问题。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">实现比较简单，服务端库如</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">socket.io</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ws</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，可以很好的帮助我们入门。而客户端也只需要参照</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">api</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">实现即可</font>

### [](https://www.123fe.net/docs/base.html#_83-%E5%B0%BD%E5%8F%AF%E8%83%BD%E5%A4%9A%E7%9A%84%E8%AF%B4%E5%87%BA%E4%BD%A0%E5%AF%B9-electron-%E7%9A%84%E7%90%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">83 尽可能多的说出你对 Electron 的理解</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">最最重要的一点，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">electron</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">实际上是一个套了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Chrome</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">nodeJS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">程序</font>

**<font style="color:rgb(44, 62, 80);">所以应该是从两个方面说开来</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Chrome</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">（无各种兼容性问题）；</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NodeJS</font><font style="color:rgb(44, 62, 80);">（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NodeJS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">能做的它也能做）</font>

### [](https://www.123fe.net/docs/base.html#_84-%E6%B7%B1%E6%B5%85%E6%8B%B7%E8%B4%9D)<font style="color:rgb(44, 62, 80);">84 深浅拷贝</font>
**<font style="color:rgb(44, 62, 80);">浅拷贝</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.assign</font>
+ <font style="color:rgb(44, 62, 80);">或者展开运算符</font>

**<font style="color:rgb(44, 62, 80);">深拷贝</font>**

+ <font style="color:rgb(44, 62, 80);">可以通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON.parse(JSON.stringify(object))</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来解决</font>

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

**<font style="color:rgb(44, 62, 80);">该方法也是有局限性的</font>**

+ <font style="color:rgb(44, 62, 80);">会忽略</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font>
+ <font style="color:rgb(44, 62, 80);">不能序列化函数</font>
+ <font style="color:rgb(44, 62, 80);">不能解决循环引用的对象</font>

### [](https://www.123fe.net/docs/base.html#_85-%E9%98%B2%E6%8A%96-%E8%8A%82%E6%B5%81)<font style="color:rgb(44, 62, 80);">85 防抖/节流</font>
**<font style="color:rgb(44, 62, 80);">防抖</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在滚动事件中需要做个复杂计算或者实现一个按钮的防二次点击操作。可以通过函数防抖动来实现</font>

```plain
// 使用 underscore 的源码来解释防抖动

/**
 * underscore 防抖函数，返回函数连续调用时，空闲时间必须大于或等于 wait，func 才会执行
 *
 * @param  {function} func        回调函数
 * @param  {number}   wait        表示时间窗口的间隔
 * @param  {boolean}  immediate   设置为ture时，是否立即调用函数
 * @return {function}             返回客户调用函数
 */
_.debounce = function(func, wait, immediate) {
    var timeout, args, context, timestamp, result;

    var later = function() {
      // 现在和上一次时间戳比较
      var last = _.now() - timestamp;
      // 如果当前间隔时间少于设定时间且大于0就重新设置定时器
      if (last < wait && last >= 0) {
        timeout = setTimeout(later, wait - last);
      } else {
        // 否则的话就是时间到了执行回调函数
        timeout = null;
        if (!immediate) {
          result = func.apply(context, args);
          if (!timeout) context = args = null;
        }
      }
    };

    return function() {
      context = this;
      args = arguments;
      // 获得时间戳
      timestamp = _.now();
      // 如果定时器不存在且立即执行函数
      var callNow = immediate && !timeout;
      // 如果定时器不存在就创建一个
      if (!timeout) timeout = setTimeout(later, wait);
      if (callNow) {
        // 如果需要立即执行函数的话 通过 apply 执行
        result = func.apply(context, args);
        context = args = null;
      }

      return result;
    };
  };
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">整体函数实现</font>

<font style="color:rgb(44, 62, 80);">对于按钮防点击来说的实现</font>

+ <font style="color:rgb(44, 62, 80);">开始一个定时器，只要我定时器还在，不管你怎么点击都不会执行回调函数。一旦定时器结束并设置为 null，就可以再次点击了</font>
+ <font style="color:rgb(44, 62, 80);">对于延时执行函数来说的实现：每次调用防抖动函数都会判断本次调用和之前的时间间隔，如果小于需要的时间间隔，就会重新创建一个定时器，并且定时器的延时为设定时间减去之前的时间间隔。一旦时间到了，就会执行相应的回调函数</font>

**<font style="color:rgb(44, 62, 80);">节流</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">防抖动和节流本质是不一样的。防抖动是将多次执行变为最后一次执行，节流是将多次执行变成每隔一段时间执行</font>

```javascript
/**
 * underscore 节流函数，返回函数连续调用时，func 执行频率限定为 次 / wait
 *
 * @param  {function}   func      回调函数
 * @param  {number}     wait      表示时间窗口的间隔
 * @param  {object}     options   如果想忽略开始函数的的调用，传入{leading: false}。
 *                                如果想忽略结尾函数的调用，传入{trailing: false}
 *                                两者不能共存，否则函数不能执行
 * @return {function}             返回客户调用函数   
 */
_.throttle = function(func, wait, options) {
    var context, args, result;
    var timeout = null;
    // 之前的时间戳
    var previous = 0;
    // 如果 options 没传则设为空对象
    if (!options) options = {};
    // 定时器回调函数
    var later = function() {
      // 如果设置了 leading，就将 previous 设为 0
      // 用于下面函数的第一个 if 判断
      previous = options.leading === false ? 0 : _.now();
      // 置空一是为了防止内存泄漏，二是为了下面的定时器判断
      timeout = null;
      result = func.apply(context, args);
      if (!timeout) context = args = null;
    };
    return function() {
      // 获得当前时间戳
      var now = _.now();
      // 首次进入前者肯定为 true
	  // 如果需要第一次不执行函数
	  // 就将上次时间戳设为当前的
      // 这样在接下来计算 remaining 的值时会大于0
      if (!previous && options.leading === false) previous = now;
      // 计算剩余时间
      var remaining = wait - (now - previous);
      context = this;
      args = arguments;
      // 如果当前调用已经大于上次调用时间 + wait
      // 或者用户手动调了时间
 	  // 如果设置了 trailing，只会进入这个条件
	  // 如果没有设置 leading，那么第一次会进入这个条件
	  // 还有一点，你可能会觉得开启了定时器那么应该不会进入这个 if 条件了
	  // 其实还是会进入的，因为定时器的延时
	  // 并不是准确的时间，很可能你设置了2秒
	  // 但是他需要2.2秒才触发，这时候就会进入这个条件
      if (remaining <= 0 || remaining > wait) {
        // 如果存在定时器就清理掉否则会调用二次回调
        if (timeout) {
          clearTimeout(timeout);
          timeout = null;
        }
        previous = now;
        result = func.apply(context, args);
        if (!timeout) context = args = null;
      } else if (!timeout && options.trailing !== false) {
        // 判断是否设置了定时器和 trailing
	    // 没有的话就开启一个定时器
        // 并且不能不能同时设置 leading 和 trailing
        timeout = setTimeout(later, remaining);
      }
      return result;
    };
  };
```

### [](https://www.123fe.net/docs/base.html#_86-%E8%B0%88%E8%B0%88%E5%8F%98%E9%87%8F%E6%8F%90%E5%8D%87)<font style="color:rgb(44, 62, 80);">86 谈谈变量提升？</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当执行 JS 代码时，会生成执行环境，只要代码不是写在函数中的，就是在全局执行环境中，函数中的代码会产生函数执行环境，只此两种执行环境</font>

+ <font style="color:rgb(44, 62, 80);">接下来让我们看一个老生常谈的例子，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var</font>

```plain
b() // call b
console.log(a) // undefined

var a = 'Hello world'

function b() {
    console.log('call b')
}
```

**<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">变量提升</font>**

<font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">这是因为函数和变量提升的原因。通常提升的解释是说将声明的代码移动到了顶部，这其实没有什么错误，便于大家理解。但是更准确的解释应该是：在生成执行环境时，会有两个阶段。第一个阶段是创建的阶段，JS 解释器会找出需要提升的变量和函数，并且给他们提前在内存中开辟好空间，函数的话会将整个函数存入内存中，变量只声明并且赋值为</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);background-color:rgb(243, 245, 247);">，所以在第二个阶段，也就是代码执行阶段，我们可以直接提前使用</font>

<font style="color:rgb(44, 62, 80);">在提升的过程中，相同的函数会覆盖上一个函数，并且函数优先于变量提升</font>

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

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">复制代码</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会产生很多错误，所以在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中引入了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不能在声明前使用，但是这并不是常说的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不会提升，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">let</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">提升了，在第一阶段内存也已经为他开辟好了空间，但是因为这个声明的特性导致了并不能在声明前使用</font>

### [](https://www.123fe.net/docs/base.html#_87-%E4%BB%80%E4%B9%88%E6%98%AF%E5%8D%95%E7%BA%BF%E7%A8%8B-%E5%92%8C%E5%BC%82%E6%AD%A5%E7%9A%84%E5%85%B3%E7%B3%BB)<font style="color:rgb(44, 62, 80);">87 什么是单线程，和异步的关系</font>
+ <font style="color:rgb(44, 62, 80);">单线程 - 只有一个线程，只能做一件事</font>
+ <font style="color:rgb(44, 62, 80);">原因 - 避免</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">渲染的冲突</font>
    - <font style="color:rgb(44, 62, 80);">浏览器需要渲染</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以修改</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">结构</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">执行的时候，浏览器</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">渲染会暂停</font>
    - <font style="color:rgb(44, 62, 80);">两段 JS 也不能同时执行（都修改</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就冲突了）</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">webworker</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">支持多线程，但是不能访问</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font>
+ <font style="color:rgb(44, 62, 80);">解决方案 - 异步</font>

### [](https://www.123fe.net/docs/base.html#_88-%E6%98%AF%E5%90%A6%E7%94%A8%E8%BF%87-jquery-%E7%9A%84-deferred)<font style="color:rgb(44, 62, 80);">88 是否用过 jQuery 的 Deferred</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780379874-ba551c87-423a-427f-88b5-0ae773a74f95.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780380306-4500ef82-b708-4c57-9c02-e775f95de089.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780378397-7b2ed416-9060-49d7-81ff-d1f545af3e5d.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780379266-b7a04020-2b71-484b-942c-6e8f67c43816.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780379729-1f5e965a-ce95-4c97-a437-0359de6ccd5c.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780385036-a8ffeb39-5b6c-4f97-a997-a4f8443de855.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780388893-4552519e-dac0-49a2-a41b-b60652a936f5.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780388947-4785b0b7-d867-421d-86d2-82a1532e2d9d.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780390368-51ecf72f-5816-426e-982f-82d72554933b.png)<font style="color:rgb(44, 62, 80);"> </font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780391339-c5d32996-6414-451c-9aa5-71c71cf30087.png)

### [](https://www.123fe.net/docs/base.html#_89-%E5%89%8D%E7%AB%AF%E9%9D%A2%E8%AF%95%E4%B9%8Bhybrid)<font style="color:rgb(44, 62, 80);">89 前端面试之hybrid</font>
### [](https://www.123fe.net/docs/base.html#_90-%E5%89%8D%E7%AB%AF%E9%9D%A2%E8%AF%95%E4%B9%8B%E7%BB%84%E4%BB%B6%E5%8C%96)<font style="color:rgb(44, 62, 80);">90 前端面试之组件化</font>
### [](https://www.123fe.net/docs/base.html#_91-%E5%89%8D%E7%AB%AF%E9%9D%A2%E8%AF%95%E4%B9%8Bmvvm%E6%B5%85%E6%9E%90)<font style="color:rgb(44, 62, 80);">91 前端面试之MVVM浅析</font>
### [](https://www.123fe.net/docs/base.html#_92-%E5%AE%9E%E7%8E%B0%E6%95%88%E6%9E%9C-%E7%82%B9%E5%87%BB%E5%AE%B9%E5%99%A8%E5%86%85%E7%9A%84%E5%9B%BE%E6%A0%87-%E5%9B%BE%E6%A0%87%E8%BE%B9%E6%A1%86%E5%8F%98%E6%88%90border-1px-solid-red-%E7%82%B9%E5%87%BB%E7%A9%BA%E7%99%BD%E5%A4%84%E9%87%8D%E7%BD%AE)<font style="color:rgb(44, 62, 80);">92 实现效果，点击容器内的图标，图标边框变成border 1px solid red，点击空白处重置</font>
```plain
const box = document.getElementById('box');
function isIcon(target) {
  return target.className.includes('icon');
}

box.onClick = function(e) {
  e.stopPropagation();
  const target = e.target;
  if (isIcon(target)) {
    target.style.border = '1px solid red';
  }
}
const doc = document;
doc.onclick = function(e) {
  const children = box.children;
  for(let i; i < children.length; i++) {
    if (isIcon(children[i])) {
      children[i].style.border = 'none';
    }
  }
}
```

### [](https://www.123fe.net/docs/base.html#_93-%E8%AF%B7%E7%AE%80%E5%8D%95%E5%AE%9E%E7%8E%B0%E5%8F%8C%E5%90%91%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9Amvvm)<font style="color:rgb(44, 62, 80);">93 请简单实现双向数据绑定</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mvvm</font>
```html
<input id="input"/>
```

```plain
const data = {};
const input = document.getElementById('input');
Object.defineProperty(data, 'text', {
  set(value) {
    input.value = value;
    this.value = value;
  }
});
input.onChange = function(e) {
  data.text = e.target.value;
}
```

### [](https://www.123fe.net/docs/base.html#_94-%E5%AE%9E%E7%8E%B0storage-%E4%BD%BF%E5%BE%97%E8%AF%A5%E5%AF%B9%E8%B1%A1%E4%B8%BA%E5%8D%95%E4%BE%8B-%E5%B9%B6%E5%AF%B9localstorage%E8%BF%9B%E8%A1%8C%E5%B0%81%E8%A3%85%E8%AE%BE%E7%BD%AE%E5%80%BCsetitem-key-value-%E5%92%8Cgetitem-key)<font style="color:rgb(44, 62, 80);">94 实现Storage，使得该对象为单例，并对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">localStorage</font><font style="color:rgb(44, 62, 80);">进行封装设置值setItem(key,value)和getItem(key)</font>
```plain
var instance = null;
class Storage {
  static getInstance() {
    if (!instance) {
      instance = new Storage();
    }
    return instance;
  }
  setItem = (key, value) => localStorage.setItem(key, value),
  getItem = key => localStorage.getItem(key)
}
```

### [](https://www.123fe.net/docs/base.html#_95-%E8%AF%B4%E8%AF%B4event-loop)<font style="color:rgb(44, 62, 80);">95 说说</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event loop</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">首先，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是单线程的，主要的任务是处理用户的交互，而用户的交互无非就是响应</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的增删改，使用事件队列的形式，一次事件循环只处理一个事件响应，使得脚本执行相对连续，所以有了事件队列，用来储存待执行的事件，那么事件队列的事件从哪里被</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">push</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">进来的呢。那就是另外一个线程叫事件触发线程做的事情了，他的作用主要是在定时触发器线程、异步</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">请求线程满足特定条件下的回调函数</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">push</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">到事件队列中，等待</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">引擎空闲的时候去执行，当然js引擎执行过程中有优先级之分，首先js引擎在一次事件循环中，会先执行js线程的主任务，然后会去查找是否有微任务</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">microtask（promise）</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，如果有那就优先执行微任务，如果没有，在去查找宏任务</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">macrotask（setTimeout、setInterval）</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">进行执行</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">众所周知</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是门非阻塞单线程语言，因为在最初</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就是为了和浏览器交互而诞生的。如果</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是门多线程的语言话，我们在多个线程中处理</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就可能会发生问题（一个线程中新加节点，另一个线程中删除节点）</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在执行的过程中会产生执行环境，这些执行环境会被顺序的加入到执行栈中。如果遇到异步的代码，会被挂起并加入到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Task</font><font style="color:rgb(44, 62, 80);">（有多种</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">task</font><font style="color:rgb(44, 62, 80);">） 队列中。一旦执行栈为空，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Event</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Loop</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就会从</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Task</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">队列中拿出需要执行的代码并放入执行栈中执行，所以本质上来说</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中的异步还是同步行为</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780393118-5056d55d-70df-40f2-8a11-93ac1722b7fc.png)

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

**<font style="color:rgb(44, 62, 80);">宏任务</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setImmediate</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">I/O</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UI rendering</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">宏任务中包括了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，浏览器会先执行一个宏任务，接下来有异步代码的话就先执行微任务</font>

**<font style="color:rgb(44, 62, 80);">所以正确的一次 Event loop 顺序是这样的</font>**

+ <font style="color:rgb(44, 62, 80);">执行同步代码，这属于宏任务</font>
+ <font style="color:rgb(44, 62, 80);">执行栈为空，查询是否有微任务需要执行</font>
+ <font style="color:rgb(44, 62, 80);">执行所有微任务</font>
+ <font style="color:rgb(44, 62, 80);">必要的话渲染 UI</font>
+ <font style="color:rgb(44, 62, 80);">然后开始下一轮</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Event loop</font><font style="color:rgb(44, 62, 80);">，执行宏任务中的异步代码</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">通过上述的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Event loop</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">顺序可知，如果宏任务中的异步代码有大量的计算并且需要操作</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的话，为了更快的响应界面响应，我们可以把操作</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">放入微任务中</font>

```plain
setTimeout(function () {
  console.log("1");
}, 0);
async function async1() {
  console.log("2");
  const data = await async2();
  console.log("3");
  return data;
}
async function async2() {
  return new Promise((resolve) => {
    console.log("4");
    resolve("async2的结果");
  }).then((data) => {
    console.log("5");
    return data;
  });
}
async1().then((data) => {
  console.log("6");
  console.log(data);
});
new Promise(function (resolve) {
  console.log("7");
  //   resolve()
}).then(function () {
  console.log("8");
});
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">输出结果：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">247536</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">async2</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的结果</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font>

### [](https://www.123fe.net/docs/base.html#_96-%E8%AF%B4%E8%AF%B4%E4%BA%8B%E4%BB%B6%E6%B5%81)<font style="color:rgb(44, 62, 80);">96 说说事件流</font>
**<font style="color:rgb(44, 62, 80);">事件流分为两种，捕获事件流和冒泡事件流</font>**

+ <font style="color:rgb(44, 62, 80);">捕获事件流从根节点开始执行，一直往子节点查找执行，直到查找执行到目标节点</font>
+ <font style="color:rgb(44, 62, 80);">冒泡事件流从目标节点开始执行，一直往父节点冒泡查找执行，直到查到到根节点</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">事件流分为三个阶段，一个是捕获节点，一个是处于目标节点阶段，一个是冒泡阶段</font>

### [](https://www.123fe.net/docs/base.html#_97-javascript-%E5%AF%B9%E8%B1%A1%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E7%9A%84%E7%90%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">97 JavaScript 对象生命周期的理解</font>
+ <font style="color:rgb(44, 62, 80);">当创建一个对象时，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会自动为该对象分配适当的内存</font>
+ <font style="color:rgb(44, 62, 80);">垃圾回收器定期扫描对象，并计算引用了该对象的其他对象的数量</font>
+ <font style="color:rgb(44, 62, 80);">如果被引用数量为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">，或惟一引用是循环的，那么该对象的内存即可回收</font>

### [](https://www.123fe.net/docs/base.html#_98-%E6%88%91%E7%8E%B0%E5%9C%A8%E6%9C%89%E4%B8%80%E4%B8%AAcanvas-%E4%B8%8A%E9%9D%A2%E9%9A%8F%E6%9C%BA%E5%B8%83%E7%9D%80%E4%B8%80%E4%BA%9B%E9%BB%91%E5%9D%97-%E8%AF%B7%E5%AE%9E%E7%8E%B0%E6%96%B9%E6%B3%95-%E8%AE%A1%E7%AE%97canvas%E4%B8%8A%E6%9C%89%E5%A4%9A%E5%B0%91%E4%B8%AA%E9%BB%91%E5%9D%97)<font style="color:rgb(44, 62, 80);">98 我现在有一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">canvas</font><font style="color:rgb(44, 62, 80);">，上面随机布着一些黑块，请实现方法，计算canvas上有多少个黑块</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">https://www.jianshu.com/p/f54d265f7aa4</font>

### [](https://www.123fe.net/docs/base.html#_99-%E8%AF%B7%E6%89%8B%E5%86%99%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AApromise)<font style="color:rgb(44, 62, 80);">99 请手写实现一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">promise</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">https://segmentfault.com/a/1190000013396601</font>

### [](https://www.123fe.net/docs/base.html#_100-%E8%AF%B4%E8%AF%B4%E4%BB%8E%E8%BE%93%E5%85%A5url%E5%88%B0%E7%9C%8B%E5%88%B0%E9%A1%B5%E9%9D%A2%E5%8F%91%E7%94%9F%E7%9A%84%E5%85%A8%E8%BF%87%E7%A8%8B-%E8%B6%8A%E8%AF%A6%E7%BB%86%E8%B6%8A%E5%A5%BD)<font style="color:rgb(44, 62, 80);">100 说说从输入URL到看到页面发生的全过程，越详细越好</font>
+ <font style="color:rgb(44, 62, 80);">首先浏览器主进程接管，开了一个下载线程。</font>
+ <font style="color:rgb(44, 62, 80);">然后进行HTTP请求（DNS查询、IP寻址等等），中间会有三次捂手，等待响应，开始下载响应报文。</font>
+ <font style="color:rgb(44, 62, 80);">将下载完的内容转交给Renderer进程管理。</font>
+ <font style="color:rgb(44, 62, 80);">Renderer进程开始解析css rule tree和dom tree，这两个过程是并行的，所以一般我会把link标签放在页面顶部。</font>
+ <font style="color:rgb(44, 62, 80);">解析绘制过程中，当浏览器遇到link标签或者script、img等标签，浏览器会去下载这些内容，遇到时候缓存的使用缓存，不适用缓存的重新下载资源。</font>
+ <font style="color:rgb(44, 62, 80);">css rule tree和dom tree生成完了之后，开始合成render tree，这个时候浏览器会进行layout，开始计算每一个节点的位置，然后进行绘制。</font>
+ <font style="color:rgb(44, 62, 80);">绘制结束后，关闭TCP连接，过程有四次挥手</font>

### [](https://www.123fe.net/docs/base.html#_101-%E6%8F%8F%E8%BF%B0%E4%B8%80%E4%B8%8Bthis)<font style="color:rgb(44, 62, 80);">101 描述一下</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，函数执行的上下文，可以通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">改变</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的指向。对于匿名函数或者直接调用的函数来说，this指向全局上下文（浏览器为window，NodeJS为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">global</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">），剩下的函数调用，那就是谁调用它，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就指向谁。当然还有es6的箭头函数，箭头函数的指向取决于该箭头函数声明的位置，在哪里声明，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就指向哪里</font>

### [](https://www.123fe.net/docs/base.html#_102-%E8%AF%B4%E4%B8%80%E4%B8%8B%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84%E7%BC%93%E5%AD%98%E6%9C%BA%E5%88%B6)<font style="color:rgb(44, 62, 80);">102 说一下浏览器的缓存机制</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">浏览器缓存机制有两种，一种为强缓存，一种为协商缓存</font>

+ <font style="color:rgb(44, 62, 80);">对于强缓存，浏览器在第一次请求的时候，会直接下载资源，然后缓存在本地，第二次请求的时候，直接使用缓存。</font>
+ <font style="color:rgb(44, 62, 80);">对于协商缓存，第一次请求缓存且保存缓存标识与时间，重复请求向服务器发送缓存标识和最后缓存时间，服务端进行校验，如果失效则使用缓存</font>

**<font style="color:rgb(44, 62, 80);">协商缓存相关设置</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Exprires</font><font style="color:rgb(44, 62, 80);">：服务端的响应头，第一次请求的时候，告诉客户端，该资源什么时候会过期。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Exprires</font><font style="color:rgb(44, 62, 80);">的缺陷是必须保证服务端时间和客户端时间严格同步。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Cache-control：max-age</font><font style="color:rgb(44, 62, 80);">：表示该资源多少时间后过期，解决了客户端和服务端时间必须同步的问题，</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-None-Match/ETag</font><font style="color:rgb(44, 62, 80);">：缓存标识，对比缓存时使用它来标识一个缓存，第一次请求的时候，服务端会返回该标识给客户端，客户端在第二次请求的时候会带上该标识与服务端进行对比并返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-None-Match</font><font style="color:rgb(44, 62, 80);">标识是否表示匹配。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-modified/If-Modified-Since</font><font style="color:rgb(44, 62, 80);">：第一次请求的时候服务端返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Last-modified</font><font style="color:rgb(44, 62, 80);">表明请求的资源上次的修改时间，第二次请求的时候客户端带上请求头</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">If-Modified-Since</font><font style="color:rgb(44, 62, 80);">，表示资源上次的修改时间，服务端拿到这两个字段进行对比</font>

### [](https://www.123fe.net/docs/base.html#_103-%E7%8E%B0%E5%9C%A8%E8%A6%81%E4%BD%A0%E5%AE%8C%E6%88%90%E4%B8%80%E4%B8%AAdialog%E7%BB%84%E4%BB%B6-%E8%AF%B4%E8%AF%B4%E4%BD%A0%E8%AE%BE%E8%AE%A1%E7%9A%84%E6%80%9D%E8%B7%AF-%E5%AE%83%E5%BA%94%E8%AF%A5%E6%9C%89%E4%BB%80%E4%B9%88%E5%8A%9F%E8%83%BD)<font style="color:rgb(44, 62, 80);">103 现在要你完成一个Dialog组件，说说你设计的思路？它应该有什么功能？</font>
+ <font style="color:rgb(44, 62, 80);">该组件需要提供</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hook</font><font style="color:rgb(44, 62, 80);">指定渲染位置，默认渲染在body下面。</font>
+ <font style="color:rgb(44, 62, 80);">然后改组件可以指定外层样式，如宽度等</font>
+ <font style="color:rgb(44, 62, 80);">组件外层还需要一层</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mask</font><font style="color:rgb(44, 62, 80);">来遮住底层内容，点击</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mask</font><font style="color:rgb(44, 62, 80);">可以执行传进来的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onCancel</font><font style="color:rgb(44, 62, 80);">函数关闭</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dialog</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">另外组件是可控的，需要外层传入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visible</font><font style="color:rgb(44, 62, 80);">表示是否可见。</font>
+ <font style="color:rgb(44, 62, 80);">然后</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Dialog</font><font style="color:rgb(44, 62, 80);">可能需要自定义头head和底部</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">footer</font><font style="color:rgb(44, 62, 80);">，默认有头部和底部，底部有一个确认按钮和取消按钮，确认按钮会执行外部传进来的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onOk</font><font style="color:rgb(44, 62, 80);">事件，然后取消按钮会执行外部传进来的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onCancel</font><font style="color:rgb(44, 62, 80);">事件。</font>
+ <font style="color:rgb(44, 62, 80);">当组件的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visible</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">true</font><font style="color:rgb(44, 62, 80);">时候，设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">body</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">overflow</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hidden</font><font style="color:rgb(44, 62, 80);">，隐藏</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">body</font><font style="color:rgb(44, 62, 80);">的滚动条，反之显示滚动条。</font>
+ <font style="color:rgb(44, 62, 80);">组件高度可能大于页面高度，组件内部需要滚动条。</font>
+ <font style="color:rgb(44, 62, 80);">只有组件的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visible</font><font style="color:rgb(44, 62, 80);">有变化且为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ture</font><font style="color:rgb(44, 62, 80);">时候，才重渲染组件内的所有内容</font>

### [](https://www.123fe.net/docs/base.html#_104-caller%E5%92%8Ccallee%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">104</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">caller</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callee</font><font style="color:rgb(44, 62, 80);">的区别</font>
**<font style="color:rgb(44, 62, 80);">callee</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">caller</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">返回一个函数的引用，这个函数调用了当前的函数。</font>

**<font style="color:rgb(44, 62, 80);">使用这个属性要注意</font>**

+ <font style="color:rgb(44, 62, 80);">这个属性只有当函数在执行时才有用</font>
+ <font style="color:rgb(44, 62, 80);">如果在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">javascript</font><font style="color:rgb(44, 62, 80);">程序中，函数是由顶层调用的，则返回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">null</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">functionName.caller: functionName</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">是当前正在执行的函数。</font>

```plain
function a() {
  console.log(a.caller)
}
```

**<font style="color:rgb(44, 62, 80);">callee</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">callee</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">放回正在执行的函数本身的引用，它是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的一个属性</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">使用callee时要注意:</font>

+ <font style="color:rgb(44, 62, 80);">这个属性只有在函数执行时才有效</font>
+ <font style="color:rgb(44, 62, 80);">它有一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">length</font><font style="color:rgb(44, 62, 80);">属性，可以用来获得形参的个数，因此可以用来比较形参和实参个数是否一致，即比较</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments.length</font><font style="color:rgb(44, 62, 80);">是否等于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments.callee.length</font>
+ <font style="color:rgb(44, 62, 80);">它可以用来递归匿名函数。</font>

```plain
function a() {
  console.log(arguments.callee)
}
```

### [](https://www.123fe.net/docs/base.html#_105-ajax%E3%80%81axios%E3%80%81fetch%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">105 ajax、axios、fetch区别</font>
+ <font style="color:rgb(44, 62, 80);">ajax 是一种技术统称</font>
+ <font style="color:rgb(44, 62, 80);">fetch 一个原生API</font>
+ <font style="color:rgb(44, 62, 80);">axios 一个第三方库</font>

**<font style="color:rgb(44, 62, 80);">jQuery ajax</font>**

```plain
$.ajax({
   type: 'POST',
   url: url,
   data: data,
   dataType: dataType,
   success: function () {},
   error: function () {}
});
```

<font style="color:rgb(44, 62, 80);">优缺点：</font>

+ <font style="color:rgb(44, 62, 80);">本身是针对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MVC</font><font style="color:rgb(44, 62, 80);">的编程,不符合现在前端</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">MVVM</font><font style="color:rgb(44, 62, 80);">的浪潮</font>
+ <font style="color:rgb(44, 62, 80);">基于原生的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XHR</font><font style="color:rgb(44, 62, 80);">开发，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XHR</font><font style="color:rgb(44, 62, 80);">本身的架构不清晰，已经有了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fetch</font><font style="color:rgb(44, 62, 80);">的替代方案</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JQuery</font><font style="color:rgb(44, 62, 80);">整个项目太大，单纯使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ajax</font><font style="color:rgb(44, 62, 80);">却要引入整个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JQuery</font><font style="color:rgb(44, 62, 80);">非常的不合理（采取个性化打包的方案又不能享受CDN服务）</font>

**<font style="color:rgb(44, 62, 80);">axios</font>**

```plain
axios({
    method: 'post',
    url: '/user/12345',
    data: {
        firstName: 'Fred',
        lastName: 'Flintstone'
    }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});
```

<font style="color:rgb(44, 62, 80);">优缺点：</font>

+ <font style="color:rgb(44, 62, 80);">从浏览器中创建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XMLHttpRequest</font>
+ <font style="color:rgb(44, 62, 80);">从</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">node.js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">发出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">http</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">请求</font>
+ <font style="color:rgb(44, 62, 80);">支持</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise API</font>
+ <font style="color:rgb(44, 62, 80);">拦截请求和响应</font>
+ <font style="color:rgb(44, 62, 80);">转换请求和响应数据</font>
+ <font style="color:rgb(44, 62, 80);">取消请求</font>
+ <font style="color:rgb(44, 62, 80);">自动转换</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">数据</font>
+ <font style="color:rgb(44, 62, 80);">客户端支持防止</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSRF/XSRF</font>

**<font style="color:rgb(44, 62, 80);">fetch</font>**

```plain
try {
  let response = await fetch(url);
  let data = response.json();
  console.log(data);
} catch(e) {
  console.log("Oops, error", e);

}
```

<font style="color:rgb(44, 62, 80);">优缺点：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fetcht</font><font style="color:rgb(44, 62, 80);">只对网络请求报错，对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">400</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">500</font><font style="color:rgb(44, 62, 80);">都当做成功的请求，需要封装去处理</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fetch</font><font style="color:rgb(44, 62, 80);">默认不会带</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cookie</font><font style="color:rgb(44, 62, 80);">，需要添加配置项</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fetch</font><font style="color:rgb(44, 62, 80);">不支持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">abort</font><font style="color:rgb(44, 62, 80);">，不支持超时控制，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);">及</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promise.reject</font><font style="color:rgb(44, 62, 80);">的实现的超时控制并不能阻止请求过程继续在后台运行，造成了量的浪费</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fetch</font><font style="color:rgb(44, 62, 80);">没有办法原生监测请求的进度，而XHR可以</font>

### [](https://www.123fe.net/docs/base.html#_106-javascript%E7%9A%84%E7%BB%84%E6%88%90)<font style="color:rgb(44, 62, 80);">106 JavaScript的组成</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">由以下三部分组成：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ECMAScript（核心）：</font><font style="color:rgb(44, 62, 80);">JavaScript` 语言基础</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">（文档对象模型）：规定了访问</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XML</font><font style="color:rgb(44, 62, 80);">的接口</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BOM</font><font style="color:rgb(44, 62, 80);">（浏览器对象模型）：提供了浏览器窗口之间进行交互的对象和方法</font>

### [](https://www.123fe.net/docs/base.html#_107-%E6%A3%80%E6%B5%8B%E6%B5%8F%E8%A7%88%E5%99%A8%E7%89%88%E6%9C%AC%E7%89%88%E6%9C%AC%E6%9C%89%E5%93%AA%E4%BA%9B%E6%96%B9%E5%BC%8F)<font style="color:rgb(44, 62, 80);">107 检测浏览器版本版本有哪些方式？</font>
+ <font style="color:rgb(44, 62, 80);">根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">navigator.userAgent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">UA.toLowerCase().indexOf('chrome')</font>
+ <font style="color:rgb(44, 62, 80);">根据</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象的成员</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">'ActiveXObject' in window</font>

### [](https://www.123fe.net/docs/base.html#_108-%E4%BB%8B%E7%BB%8Djs%E6%9C%89%E5%93%AA%E4%BA%9B%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1)<font style="color:rgb(44, 62, 80);">108 介绍JS有哪些内置对象</font>
+ <font style="color:rgb(44, 62, 80);">数据封装类对象：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Boolean</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Number</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">String</font>
+ <font style="color:rgb(44, 62, 80);">其他对象：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Function</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Arguments</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Math</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Date</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">RegExp</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Error</font>
+ <font style="color:rgb(44, 62, 80);">ES6新增对象：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Map</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Set</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Promises</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Proxy</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Reflect</font>

### [](https://www.123fe.net/docs/base.html#_109-%E8%AF%B4%E5%87%A0%E6%9D%A1%E5%86%99javascript%E7%9A%84%E5%9F%BA%E6%9C%AC%E8%A7%84%E8%8C%83)<font style="color:rgb(44, 62, 80);">109 说几条写JavaScript的基本规范</font>
+ <font style="color:rgb(44, 62, 80);">代码缩进，建议使用“四个空格”缩进</font>
+ <font style="color:rgb(44, 62, 80);">代码段使用花括号</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{}</font><font style="color:rgb(44, 62, 80);">包裹</font>
+ <font style="color:rgb(44, 62, 80);">语句结束使用分号;</font>
+ <font style="color:rgb(44, 62, 80);">变量和函数在使用前进行声明</font>
+ <font style="color:rgb(44, 62, 80);">以大写字母开头命名构造函数，全大写命名常量</font>
+ <font style="color:rgb(44, 62, 80);">规范定义</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON</font><font style="color:rgb(44, 62, 80);">对象，补全双引号</font>
+ <font style="color:rgb(44, 62, 80);">用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{}</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[]</font><font style="color:rgb(44, 62, 80);">声明对象和数组</font>

### [](https://www.123fe.net/docs/base.html#_110-%E5%A6%82%E4%BD%95%E7%BC%96%E5%86%99%E9%AB%98%E6%80%A7%E8%83%BD%E7%9A%84javascript)<font style="color:rgb(44, 62, 80);">110 如何编写高性能的JavaScript</font>
+ <font style="color:rgb(44, 62, 80);">遵循严格模式：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">"use strict";</font>
+ <font style="color:rgb(44, 62, 80);">将js脚本放在页面底部，加快渲染页面</font>
+ <font style="color:rgb(44, 62, 80);">将js脚本将脚本成组打包，减少请求</font>
+ <font style="color:rgb(44, 62, 80);">使用非阻塞方式下载js脚本</font>
+ <font style="color:rgb(44, 62, 80);">尽量使用局部变量来保存全局变量</font>
+ <font style="color:rgb(44, 62, 80);">尽量减少使用闭包</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">对象属性方法时，省略</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font>
+ <font style="color:rgb(44, 62, 80);">尽量减少对象成员嵌套</font>
+ <font style="color:rgb(44, 62, 80);">缓存</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点的访问</font>
+ <font style="color:rgb(44, 62, 80);">通过避免使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eval()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Function()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">构造器</font>
+ <font style="color:rgb(44, 62, 80);">给</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setInterval()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">传递函数而不是字符串作为参数</font>
+ <font style="color:rgb(44, 62, 80);">尽量使用直接量创建对象和数组</font>
+ <font style="color:rgb(44, 62, 80);">最小化重绘(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">repaint</font><font style="color:rgb(44, 62, 80);">)和回流(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reflow</font><font style="color:rgb(44, 62, 80);">)</font>

### [](https://www.123fe.net/docs/base.html#_111-%E6%8F%8F%E8%BF%B0%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84%E6%B8%B2%E6%9F%93%E8%BF%87%E7%A8%8B-dom%E6%A0%91%E5%92%8C%E6%B8%B2%E6%9F%93%E6%A0%91%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">111 描述浏览器的渲染过程，DOM树和渲染树的区别</font>
+ <font style="color:rgb(44, 62, 80);">浏览器的渲染过程：</font>
    - <font style="color:rgb(44, 62, 80);">解析</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);">构建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">(DOM树)，并行请求</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css/image/js</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">文件下载完成，开始构建</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSSOM</font><font style="color:rgb(44, 62, 80);">(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">树)</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSSOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">构建结束后，和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一起生成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Render Tree</font><font style="color:rgb(44, 62, 80);">(渲染树)</font>
    - <font style="color:rgb(44, 62, 80);">布局(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Layout</font><font style="color:rgb(44, 62, 80);">)：计算出每个节点在屏幕中的位置</font>
    - <font style="color:rgb(44, 62, 80);">显示(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Painting</font><font style="color:rgb(44, 62, 80);">)：通过显卡把页面画到屏幕上</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">树 和 渲染树 的区别：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">树与</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);">标签一一对应，包括</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">head</font><font style="color:rgb(44, 62, 80);">和隐藏元素</font>
    - <font style="color:rgb(44, 62, 80);">渲染树不包括</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">head</font><font style="color:rgb(44, 62, 80);">和隐藏元素，大段文本的每一个行都是独立节点，每一个节点都有对应的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">属性</font>

### [](https://www.123fe.net/docs/base.html#_112-script-%E7%9A%84%E4%BD%8D%E7%BD%AE%E6%98%AF%E5%90%A6%E4%BC%9A%E5%BD%B1%E5%93%8D%E9%A6%96%E5%B1%8F%E6%98%BE%E7%A4%BA%E6%97%B6%E9%97%B4)<font style="color:rgb(44, 62, 80);">112 script 的位置是否会影响首屏显示时间</font>
+ <font style="color:rgb(44, 62, 80);">在解析</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">过程中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">js</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">文件的下载是并行的，不需要</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">处理到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">节点。因此，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);">的位置不影响首屏显示的开始时间。</font>
+ <font style="color:rgb(44, 62, 80);">浏览器解析</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是自上而下的线性过程，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);">作为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的一部分同样遵循这个原则</font>
+ <font style="color:rgb(44, 62, 80);">因此，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会延迟</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DomContentLoad</font><font style="color:rgb(44, 62, 80);">，只显示其上部分首屏内容，从而影响首屏显示的完成时间</font>

### [](https://www.123fe.net/docs/base.html#_113-%E8%A7%A3%E9%87%8Ajavascript%E4%B8%AD%E7%9A%84%E4%BD%9C%E7%94%A8%E5%9F%9F%E4%B8%8E%E5%8F%98%E9%87%8F%E5%A3%B0%E6%98%8E%E6%8F%90%E5%8D%87)<font style="color:rgb(44, 62, 80);">113 解释JavaScript中的作用域与变量声明提升</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">作用域：</font>
    - <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Java</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">C</font><font style="color:rgb(44, 62, 80);">等语言中，作用域为for语句、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">if</font><font style="color:rgb(44, 62, 80);">语句或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{}</font><font style="color:rgb(44, 62, 80);">内的一块区域，称为作用域；</font>
    - <font style="color:rgb(44, 62, 80);">而在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中，作用域为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function(){}</font><font style="color:rgb(44, 62, 80);">内的区域，称为函数作用域。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">变量声明提升：</font>
    - <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">中，函数声明与变量声明经常被</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript</font><font style="color:rgb(44, 62, 80);">引擎隐式地提升到当前作用域的顶部。</font>
    - <font style="color:rgb(44, 62, 80);">声明语句中的赋值部分并不会被提升，只有名称被提升</font>
    - <font style="color:rgb(44, 62, 80);">函数声明的优先级高于变量，如果变量名跟函数名相同且未赋值，则函数声明会覆盖变量声明</font>
    - <font style="color:rgb(44, 62, 80);">如果函数有多个同名参数，那么最后一个参数（即使没有定义）会覆盖前面的同名参数</font>

### [](https://www.123fe.net/docs/base.html#_114-javascript%E6%9C%89%E5%87%A0%E7%A7%8D%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%80%BC-%E4%BD%A0%E8%83%BD%E7%94%BB%E4%B8%80%E4%B8%8B%E4%BB%96%E4%BB%AC%E7%9A%84%E5%86%85%E5%AD%98%E5%9B%BE%E5%90%97)<font style="color:rgb(44, 62, 80);">114 JavaScript有几种类型的值？，你能画一下他们的内存图吗</font>
+ <font style="color:rgb(44, 62, 80);">原始数据类型（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Undefined</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Null</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Boolean</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Number</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">String</font><font style="color:rgb(44, 62, 80);">）-- 栈</font>
+ <font style="color:rgb(44, 62, 80);">引用数据类型（对象、数组和函数）-- 堆</font>
+ <font style="color:rgb(44, 62, 80);">两种类型的区别是：存储位置不同：</font>
+ <font style="color:rgb(44, 62, 80);">原始数据类型是直接存储在栈(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stack</font><font style="color:rgb(44, 62, 80);">)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据；</font>
+ <font style="color:rgb(44, 62, 80);">引用数据类型存储在堆(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">heap</font><font style="color:rgb(44, 62, 80);">)中的对象，占据空间大、大小不固定，如果存储在栈中，将会影响程序运行的性能；</font>
+ <font style="color:rgb(44, 62, 80);">引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。</font>
+ <font style="color:rgb(44, 62, 80);">当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。</font>

### [](https://www.123fe.net/docs/base.html#_115-javascript%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AA%E7%B1%BB-%E6%80%8E%E4%B9%88%E5%AE%9E%E4%BE%8B%E5%8C%96%E8%BF%99%E4%B8%AA%E7%B1%BB)<font style="color:rgb(44, 62, 80);">115 JavaScript如何实现一个类，怎么实例化这个类</font>
+ <font style="color:rgb(44, 62, 80);">构造函数法（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);">） -- 用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">关键字 生成实例对象</font>
    - <font style="color:rgb(44, 62, 80);">缺点：用到了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);">，编写复杂，可读性差</font>

```plain
function Mobile(name, price){
  this.name = name;
  this.price = price;
}
Mobile.prototype.sell = function(){
  alert(this.name + "，售价 $" + this.price);
}
var iPhone7 = new Mobile("iPhone7", 1000);
iPhone7.sell();
```

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.create</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">法 -- 用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.create()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">生成实例对象</font>
+ <font style="color:rgb(44, 62, 80);">缺点：不能实现私有属性和私有方法，实例对象之间也不能共享数据</font>

```plain
var Person = {
     firstname: "Mark",
     lastname: "Yun",
     age: 25,
     introduce: function(){
         alert('I am ' + Person.firstname + ' ' + Person.lastname);
     }
 };

 var person = Object.create(Person);
 person.introduce();

 // Object.create 要求 IE9+，低版本浏览器可以自行部署：
 if (!Object.create) {
　   Object.create = function (o) {
　　　 function F() {}
　　　 F.prototype = o;
　　　 return new F();
　　};
　}
```

+ <font style="color:rgb(44, 62, 80);">极简主义法（消除</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(44, 62, 80);">） -- 调用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">createNew()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">得到实例对象</font>
    - <font style="color:rgb(44, 62, 80);">优点：容易理解，结构清晰优雅，符合传统的"面向对象编程"的构造</font>

```plain
var Cat = {
   age: 3, // 共享数据 -- 定义在类对象内，createNew() 外
   createNew: function () {
     var cat = {};
     // var cat = Animal.createNew(); // 继承 Animal 类
     cat.name = "小咪";
     var sound = "喵喵喵"; // 私有属性--定义在 createNew() 内，输出对象外
     cat.makeSound = function () {
       alert(sound);  // 暴露私有属性
     };
     cat.changeAge = function(num){
       Cat.age = num; // 修改共享数据
     };
     return cat; // 输出对象
   }
 };

 var cat = Cat.createNew();
 cat.makeSound();
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">语法糖</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">class</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">-- 用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">关键字 生成实例对象</font>

```plain
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

var point = new Point(2, 3);
```

### [](https://www.123fe.net/docs/base.html#_116-javascript%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E7%BB%A7%E6%89%BF)<font style="color:rgb(44, 62, 80);">116 Javascript如何实现继承</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">构造函数绑定：使用</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">或</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法，将父对象的构造函数绑定在子对象上</font>

```plain
function Cat(name,color){
 　Animal.apply(this, arguments);
 　this.name = name;
 　this.color = color;
}
```

+ <font style="color:rgb(44, 62, 80);">实例继承：将子对象的 prototype 指向父对象的一个实例</font>

```javascript
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">拷贝继承：如果把父对象的所有属性和方法，拷贝进子对象</font>

```plain
function extend(Child, Parent) {
　　　var p = Parent.prototype;
　　　var c = Child.prototype;
　　　for (var i in p) {
　　　   c[i] = p[i];
　　　}
　　　c.uber = p;
　 }
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">原型继承：将子对象的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">指向父对象的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">prototype</font>

```plain
function extend(Child, Parent) {
    var F = function(){};
  　F.prototype = Parent.prototype;
  　Child.prototype = new F();
  　Child.prototype.constructor = Child;
  　Child.uber = Parent.prototype;
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ES6</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">语法糖</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">extends：class ColorPoint extends Point {}</font>

```plain
class ColorPoint extends Point {
    constructor(x, y, color) {
      super(x, y); // 调用父类的constructor(x, y)
      this.color = color;
    }
    toString() {
      return this.color + ' ' + super.toString(); // 调用父类的toString()
    }
}
```

### [](https://www.123fe.net/docs/base.html#_117-javascript%E4%BD%9C%E7%94%A8%E9%93%BE%E5%9F%9F)<font style="color:rgb(44, 62, 80);">117 Javascript作用链域</font>
+ <font style="color:rgb(44, 62, 80);">全局函数无法查看局部函数的内部细节，但局部函数可以查看其上层的函数细节，直至全局细节</font>
+ <font style="color:rgb(44, 62, 80);">如果当前作用域没有找到属性或方法，会向上层作用域查找，直至全局函数，这种形式就是作用域链</font>

### [](https://www.123fe.net/docs/base.html#_118-%E4%BB%8B%E7%BB%8D-dom-%E7%9A%84%E5%8F%91%E5%B1%95)<font style="color:rgb(44, 62, 80);">118 介绍 DOM 的发展</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">：文档对象模型（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Document Object Model</font><font style="color:rgb(44, 62, 80);">），定义了访问HTML和XML文档的标准，与编程语言及平台无关</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM0</font><font style="color:rgb(44, 62, 80);">：提供了查询和操作Web文档的内容API。未形成标准，实现混乱。如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document.forms['login']</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM1</font><font style="color:rgb(44, 62, 80);">：W3C提出标准化的DOM，简化了对文档中任意部分的访问和操作。如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JavaScript中的Document</font><font style="color:rgb(44, 62, 80);">对象</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM2</font><font style="color:rgb(44, 62, 80);">：原来DOM基础上扩充了鼠标事件等细分模块，增加了对CSS的支持。如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getComputedStyle(elem, pseudo)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM3</font><font style="color:rgb(44, 62, 80);">：增加了XPath模块和加载与保存（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Load and Save</font><font style="color:rgb(44, 62, 80);">）模块。如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">XPathEvaluator</font>

### [](https://www.123fe.net/docs/base.html#_119-%E4%BB%8B%E7%BB%8Ddom0-dom2-dom3%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86%E6%96%B9%E5%BC%8F%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">119 介绍DOM0，DOM2，DOM3事件处理方式区别</font>
+ <font style="color:rgb(44, 62, 80);">DOM0级事件处理方式：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">btn.onclick = func;</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">btn.onclick = null;</font>
+ <font style="color:rgb(44, 62, 80);">DOM2级事件处理方式：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">btn.addEventListener('click', func, false);</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">btn.removeEventListener('click', func, false);</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">btn.attachEvent("onclick", func);</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">btn.detachEvent("onclick", func);</font>
+ <font style="color:rgb(44, 62, 80);">DOM3级事件处理方式：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eventUtil.addListener(input, "textInput", func);</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eventUtil</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是自定义对象，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">textInput</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是DOM3级事件</font>

**<font style="color:rgb(44, 62, 80);">事件的三个阶段</font>**

+ <font style="color:rgb(44, 62, 80);">捕获、目标、冒泡</font>

### [](https://www.123fe.net/docs/base.html#_120-%E4%BB%8B%E7%BB%8D%E4%BA%8B%E4%BB%B6-%E6%8D%95%E8%8E%B7-%E5%92%8C-%E5%86%92%E6%B3%A1-%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F%E5%92%8C%E4%BA%8B%E4%BB%B6%E7%9A%84%E6%89%A7%E8%A1%8C%E6%AC%A1%E6%95%B0)<font style="color:rgb(44, 62, 80);">120 介绍事件“捕获”和“冒泡”执行顺序和事件的执行次数</font>
+ <font style="color:rgb(44, 62, 80);">按照W3C标准的事件：首是进入捕获阶段，直到达到目标元素，再进入冒泡阶段</font>
+ <font style="color:rgb(44, 62, 80);">事件执行次数（DOM2-addEventListener）：元素上绑定事件的个数</font>
    - <font style="color:rgb(44, 62, 80);">注意1：前提是事件被确实触发</font>
    - <font style="color:rgb(44, 62, 80);">注意2：事件绑定几次就算几个事件，即使类型和功能完全一样也不会“覆盖”</font>
+ <font style="color:rgb(44, 62, 80);">事件执行顺序：判断的关键是否目标元素</font>
    - <font style="color:rgb(44, 62, 80);">非目标元素：根据W3C的标准执行：捕获->目标元素->冒泡（不依据事件绑定顺序）</font>
    - <font style="color:rgb(44, 62, 80);">目标元素：依据事件绑定顺序：先绑定的事件先执行（不依据捕获冒泡标准）</font>
    - <font style="color:rgb(44, 62, 80);">最终顺序：父元素捕获->目标元素事件1->目标元素事件2->子元素捕获->子元素冒泡->父元素冒泡</font>
    - <font style="color:rgb(44, 62, 80);">注意：子元素事件执行前提 事件确实“落”到子元素布局区域上，而不是简单的具有嵌套关系</font>

**<font style="color:rgb(44, 62, 80);">在一个DOM上同时绑定两个点击事件：一个用捕获，一个用冒泡。事件会执行几次，先执行冒泡还是捕获？</font>**

+ <font style="color:rgb(44, 62, 80);">该DOM上的事件如果被触发，会执行两次（执行次数等于绑定次数）</font>
+ <font style="color:rgb(44, 62, 80);">如果该DOM是目标元素，则按事件绑定顺序执行，不区分冒泡/捕获</font>
+ <font style="color:rgb(44, 62, 80);">如果该DOM是处于事件流中的非目标元素，则先执行捕获，后执行冒泡</font>

**<font style="color:rgb(44, 62, 80);">事件的代理/委托</font>**

+ <font style="color:rgb(44, 62, 80);">事件委托是指将事件绑定目标元素的到父元素上，利用冒泡机制触发该事件</font>
    - <font style="color:rgb(44, 62, 80);">优点：</font>
        * <font style="color:rgb(44, 62, 80);">可以减少事件注册，节省大量内存占用</font>
        * <font style="color:rgb(44, 62, 80);">可以将事件应用于动态添加的子元素上</font>
    - <font style="color:rgb(44, 62, 80);">缺点： 使用不当会造成事件在不应该触发时触发</font>
    - <font style="color:rgb(44, 62, 80);">示例：</font>

```plain
ulEl.addEventListener('click', function(e){
    var target = event.target || event.srcElement;
    if(!!target && target.nodeName.toUpperCase() === "LI"){
        console.log(target.innerHTML);
    }
}, false);
```

**<font style="color:rgb(44, 62, 80);">W3C事件的 target 与 currentTarget 的区别？</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">target</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">只会出现在事件流的目标阶段</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">currentTarget</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可能出现在事件流的任何阶段</font>
+ <font style="color:rgb(44, 62, 80);">当事件流处在目标阶段时，二者的指向相同</font>
+ <font style="color:rgb(44, 62, 80);">当事件流处于捕获或冒泡阶段时：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">currentTarget</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指向当前事件活动的对象(一般为父级)</font>

**<font style="color:rgb(44, 62, 80);">如何派发事件(dispatchEvent)？（如何进行事件广播？）</font>**

+ <font style="color:rgb(44, 62, 80);">W3C: 使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dispatchEvent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法</font>
+ <font style="color:rgb(44, 62, 80);">IE: 使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fireEvent</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">方法</font>

```plain
var fireEvent = function(element, event){
    if (document.createEventObject){
        var mockEvent = document.createEventObject();
        return element.fireEvent('on' + event, mockEvent)
    }else{
        var mockEvent = document.createEvent('HTMLEvents');
        mockEvent.initEvent(event, true, true);
        return !element.dispatchEvent(mockEvent);
    }
}
```

### [](https://www.123fe.net/docs/base.html#_121-%E4%BB%80%E4%B9%88%E6%98%AF%E5%87%BD%E6%95%B0%E8%8A%82%E6%B5%81-%E4%BB%8B%E7%BB%8D%E4%B8%80%E4%B8%8B%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF%E5%92%8C%E5%8E%9F%E7%90%86)<font style="color:rgb(44, 62, 80);">121 什么是函数节流？介绍一下应用场景和原理？</font>
+ <font style="color:rgb(44, 62, 80);">函数节流(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">throttle</font><font style="color:rgb(44, 62, 80);">)是指阻止一个函数在很短时间间隔内连续调用。 只有当上一次函数执行后达到规定的时间间隔，才能进行下一次调用。 但要保证一个累计最小调用间隔（否则拖拽类的节流都将无连续效果）</font>
+ <font style="color:rgb(44, 62, 80);">函数节流用于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onresize</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">onscroll</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等短时间内会多次触发的事件</font>
+ <font style="color:rgb(44, 62, 80);">函数节流的原理：使用定时器做时间节流。 当触发一个事件时，先用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">让这个事件延迟一小段时间再执行。 如果在这个时间间隔内又触发了事件，就</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clearTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">原来的定时器， 再</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setTimeout</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一个新的定时器重复以上流程。</font>

**<font style="color:rgb(44, 62, 80);">函数节流简单实现：</font>**

```plain
function throttle(method, context) {
     clearTimeout(methor.tId);
     method.tId = setTimeout(function(){
         method.call(context);
     }， 100); // 两次调用至少间隔 100ms
}
// 调用
window.onresize = function(){
  throttle(myFunc, window);
}
```

### [](https://www.123fe.net/docs/base.html#_122-%E5%8C%BA%E5%88%86%E4%BB%80%E4%B9%88%E6%98%AF-%E5%AE%A2%E6%88%B7%E5%8C%BA%E5%9D%90%E6%A0%87-%E3%80%81-%E9%A1%B5%E9%9D%A2%E5%9D%90%E6%A0%87-%E3%80%81-%E5%B1%8F%E5%B9%95%E5%9D%90%E6%A0%87)<font style="color:rgb(44, 62, 80);">122 区分什么是“客户区坐标”、“页面坐标”、“屏幕坐标”</font>
+ <font style="color:rgb(44, 62, 80);">客户区坐标：鼠标指针在可视区中的水平坐标(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clientX</font><font style="color:rgb(44, 62, 80);">)和垂直坐标(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clientY</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(44, 62, 80);">页面坐标：鼠标指针在页面布局中的水平坐标(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pageX</font><font style="color:rgb(44, 62, 80);">)和垂直坐标(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pageY</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(44, 62, 80);">屏幕坐标：设备物理屏幕的水平坐标(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">screenX</font><font style="color:rgb(44, 62, 80);">)和垂直坐标(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">screenY</font><font style="color:rgb(44, 62, 80);">)</font>

**<font style="color:rgb(44, 62, 80);">如何获得一个DOM元素的绝对位置？</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">elem.offsetLef</font><font style="color:rgb(44, 62, 80);">t：返回元素相对于其定位父级左侧的距离</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">elem.offsetTop</font><font style="color:rgb(44, 62, 80);">：返回元素相对于其定位父级顶部的距离</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">elem.getBoundingClientRect()</font><font style="color:rgb(44, 62, 80);">：返回一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOMRect</font><font style="color:rgb(44, 62, 80);">对象，包含一组描述边框的只读属性，单位像素</font>

### [](https://www.123fe.net/docs/base.html#_123-%E8%A7%A3%E9%87%8A%E4%B8%80%E4%B8%8B%E8%BF%99%E6%AE%B5%E4%BB%A3%E7%A0%81%E7%9A%84%E6%84%8F%E6%80%9D)<font style="color:rgb(44, 62, 80);">123 解释一下这段代码的意思</font>
```plain
[].forEach.call($$("*"), function(el){
      el.style.outline = "1px solid " + (~~(Math.random()*(1<<24))).toString(16);
  })
```

+ <font style="color:rgb(44, 62, 80);">解释：获取页面所有的元素，遍历这些元素，为它们添加1像素随机颜色的轮廓(outline)</font>
    1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">$$(sel)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">// $$函数被许多现代浏览器命令行支持，等价于 document.querySelectorAll(sel)</font>
    2. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[].forEach.call(NodeLists)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">// 使用 call 函数将数组遍历函数 forEach 应到节点元素列表</font>
    3. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">el.style.outline = "1px solid 333"</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">// 样式 outline 位于盒模型之外，不影响元素布局位置</font>
    4. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">(1<<24)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">// parseInt("ffffff", 16) == 16777215 == 2^24 - 1 // 1<<24 == 2^24 == 16777216</font>
    5. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Math.random()*(1<<24)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">// 表示一个位于 0 到 16777216 之间的随机浮点数</font>
    6. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">~~Math.random()*(1<<24)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">//</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">~~</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">作用相当于 parseInt 取整</font>
    7. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">(~~(Math.random()*(1<<24))).toString(16)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">// 转换为一个十六进制-</font>

### [](https://www.123fe.net/docs/base.html#_124-javascript%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E6%96%B9%E6%B3%95)<font style="color:rgb(44, 62, 80);">124 Javascript垃圾回收方法</font>
+ <font style="color:rgb(44, 62, 80);">标记清除（mark and sweep）</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这是JavaScript最常见的垃圾回收方式，当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变量离开环境的时候（函数执行结束）将其标记为“离开环境”</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了</font>

**<font style="color:rgb(44, 62, 80);">引用计数(reference counting)</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在低版本IE中经常会出现内存泄露，很多时候就是因为其采用引用计数方式进行垃圾回收。引用计数的策略是跟踪记录每个值被使用的次数，当声明了一个 变量并将一个引用类型赋值给该变量的时候这个值的引用次数就加1，如果该变量的值变成了另外一个，则这个值得引用次数减1，当这个值的引用次数变为0的时 候，说明没有变量在使用，这个值没法被访问了，因此可以将其占用的空间回收，这样垃圾回收器会在运行的时候清理掉引用次数为0的值占用的空间</font>

### [](https://www.123fe.net/docs/base.html#_125-%E8%AF%B7%E8%A7%A3%E9%87%8A%E4%B8%80%E4%B8%8B-javascript-%E7%9A%84%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5)<font style="color:rgb(44, 62, 80);">125 请解释一下 JavaScript 的同源策略</font>
+ <font style="color:rgb(44, 62, 80);">概念:同源策略是客户端脚本（尤其是Javascript）的重要的安全度量标准。它最早出自Netscape Navigator2.0，其目的是防止某个文档或脚本从多个不同源装载。这里的同源策略指的是：协议，域名，端口相同，同源策略是一种安全协议</font>
+ <font style="color:rgb(44, 62, 80);">指一段脚本只能读取来自同一来源的窗口和文档的属性</font>

**<font style="color:rgb(44, 62, 80);">为什么要有同源限制？</font>**

+ <font style="color:rgb(44, 62, 80);">我们举例说明：比如一个黑客程序，他利用Iframe把真正的银行登录页面嵌到他的页面上，当你使用真实的用户名，密码登录时，他的页面就可以通过Javascript读取到你的表单中input中的内容，这样用户名，密码就轻松到手了。</font>
+ <font style="color:rgb(44, 62, 80);">缺点</font>
    - <font style="color:rgb(44, 62, 80);">现在网站的JS都会进行压缩，一些文件用了严格模式，而另一些没有。这时这些本来是严格模式的文件，被 merge后，这个串就到了文件的中间，不仅没有指示严格模式，反而在压缩后浪费了字节</font>

### [](https://www.123fe.net/docs/base.html#_126-%E5%A6%82%E4%BD%95%E5%88%A0%E9%99%A4%E4%B8%80%E4%B8%AAcookie)<font style="color:rgb(44, 62, 80);">126 如何删除一个cookie</font>
+ <font style="color:rgb(44, 62, 80);">将时间设为当前时间往前一点</font>

```plain
var date = new Date();

date.setDate(date.getDate() - 1);//真正的删除
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">setDate()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法用于设置一个月的某一天</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">expires</font><font style="color:rgb(44, 62, 80);">的设置</font>

```plain
document.cookie = 'user='+ encodeURIComponent('name')  + ';expires = ' + new Date(0)
```

### [](https://www.123fe.net/docs/base.html#_127-%E9%A1%B5%E9%9D%A2%E7%BC%96%E7%A0%81%E5%92%8C%E8%A2%AB%E8%AF%B7%E6%B1%82%E7%9A%84%E8%B5%84%E6%BA%90%E7%BC%96%E7%A0%81%E5%A6%82%E6%9E%9C%E4%B8%8D%E4%B8%80%E8%87%B4%E5%A6%82%E4%BD%95%E5%A4%84%E7%90%86)<font style="color:rgb(44, 62, 80);">127 页面编码和被请求的资源编码如果不一致如何处理</font>
+ <font style="color:rgb(44, 62, 80);">后端响应头设置</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">charset</font>
+ <font style="color:rgb(44, 62, 80);">前端页面</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><meta></font><font style="color:rgb(44, 62, 80);">设置</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">charset</font>

### [](https://www.123fe.net/docs/base.html#_128-%E6%8A%8A-script-%E6%94%BE%E5%9C%A8-body-%E4%B9%8B%E5%89%8D%E5%92%8C%E4%B9%8B%E5%90%8E%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB-%E6%B5%8F%E8%A7%88%E5%99%A8%E4%BC%9A%E5%A6%82%E4%BD%95%E8%A7%A3%E6%9E%90%E5%AE%83%E4%BB%AC)<font style="color:rgb(44, 62, 80);">128 把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><script></font><font style="color:rgb(44, 62, 80);">放在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"></body></font><font style="color:rgb(44, 62, 80);">之前和之后有什么区别？浏览器会如何解析它们？</font>
+ <font style="color:rgb(44, 62, 80);">按照HTML标准，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"></body></font><font style="color:rgb(44, 62, 80);">结束后出现</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><script></font><font style="color:rgb(44, 62, 80);">或任何元素的开始标签，都是解析错误</font>
+ <font style="color:rgb(44, 62, 80);">虽然不符合HTML标准，但浏览器会自动容错，使实际效果与写在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"></body></font><font style="color:rgb(44, 62, 80);">之前没有区别</font>
+ <font style="color:rgb(44, 62, 80);">浏览器的容错机制会忽略</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><script></font><font style="color:rgb(44, 62, 80);">之前的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"></body></font><font style="color:rgb(44, 62, 80);">，视作</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><script></font><font style="color:rgb(44, 62, 80);">仍在 body 体内。省略</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"></body></font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"></html></font><font style="color:rgb(44, 62, 80);">闭合标签符合HTML标准，服务器可以利用这一标准尽可能少输出内容</font>

### [](https://www.123fe.net/docs/base.html#_129-javascript-%E4%B8%AD-%E8%B0%83%E7%94%A8%E5%87%BD%E6%95%B0%E6%9C%89%E5%93%AA%E5%87%A0%E7%A7%8D%E6%96%B9%E5%BC%8F)<font style="color:rgb(44, 62, 80);">129 JavaScript 中，调用函数有哪几种方式</font>
+ <font style="color:rgb(44, 62, 80);">方法调用模式</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Foo.foo(arg1, arg2);</font>
+ <font style="color:rgb(44, 62, 80);">函数调用模式</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">foo(arg1, arg2);</font>
+ <font style="color:rgb(44, 62, 80);">构造器调用模式</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">(new Foo())(arg1, arg2);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call/applay</font><font style="color:rgb(44, 62, 80);">调用模式</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Foo.foo.call(that, arg1, arg2);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(44, 62, 80);">调用模式</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Foo.foo.bind(that)(arg1, arg2)();</font>

### [](https://www.123fe.net/docs/base.html#_130-%E7%AE%80%E5%8D%95%E5%AE%9E%E7%8E%B0-function-bind-%E5%87%BD%E6%95%B0)<font style="color:rgb(44, 62, 80);">130 简单实现 Function.bind 函数</font>
```plain
if (!Function.prototype.bind) {
    Function.prototype.bind = function(that) {
      var func = this, args = arguments;
      return function() {
        return func.apply(that, Array.prototype.slice.call(args, 1));
      }
    }
  }
  // 只支持 bind 阶段的默认参数：
  func.bind(that, arg1, arg2)();

  // 不支持以下调用阶段传入的参数：
  func.bind(that)(arg1, arg2);
```

### [](https://www.123fe.net/docs/base.html#_131-%E5%88%97%E4%B8%BE%E4%B8%80%E4%B8%8Bjavascript%E6%95%B0%E7%BB%84%E5%92%8C%E5%AF%B9%E8%B1%A1%E6%9C%89%E5%93%AA%E4%BA%9B%E5%8E%9F%E7%94%9F%E6%96%B9%E6%B3%95)<font style="color:rgb(44, 62, 80);">131 列举一下JavaScript数组和对象有哪些原生方法？</font>
**<font style="color:rgb(44, 62, 80);">数组：</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.concat(arr1, arr2, arrn);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.join(",");</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.sort(func);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.pop();</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.push(e1, e2, en);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.shift();</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">unshift(e1, e2, en);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.reverse();</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.slice(start, end);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.splice(index, count, e1, e2, en);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.indexOf(el);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.includes(el);</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">// ES6</font>

**<font style="color:rgb(44, 62, 80);">对象：</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object.hasOwnProperty(prop);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object.propertyIsEnumerable(prop);</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object.valueOf();</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object.toString();</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">object.toLocaleString();</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Class.prototype.isPropertyOf(object);</font>

### [](https://www.123fe.net/docs/base.html#_132-array-splice-%E4%B8%8E-array-splice-%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">132 Array.splice() 与 Array.splice() 的区别？</font>
**<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">slice</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">“读取”数组指定的元素，不会对原数组进行修改</font>

+ <font style="color:rgb(44, 62, 80);">语法：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.slice(start, end)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">start</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指定选取开始位置（含）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">end</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指定选取结束位置（不含）</font>

**<font style="color:rgb(44, 62, 80);">splice</font>**

+ <font style="color:rgb(44, 62, 80);">“操作”数组指定的元素，会修改原数组，返回被删除的元素</font>
+ <font style="color:rgb(44, 62, 80);">语法：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr.splice(index, count, [insert Elements])</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">index</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是操作的起始位置</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">count = 0</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">插入元素，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">count > 0</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">删除元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">[insert Elements]</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">向数组新插入的元素</font>

### [](https://www.123fe.net/docs/base.html#_133-mvvm)<font style="color:rgb(44, 62, 80);">133 MVVM</font>
**<font style="color:rgb(44, 62, 80);">MVVM 由以下三个内容组成</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">View</font><font style="color:rgb(44, 62, 80);">：界面</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Model</font><font style="color:rgb(44, 62, 80);">：数据模型</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ViewModel</font><font style="color:rgb(44, 62, 80);">：作为桥梁负责沟通</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">View</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Model</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在 JQuery 时期，如果需要刷新 UI 时，需要先取到对应的 DOM 再更新 UI，这样数据和业务的逻辑就和页面有强耦合</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在 MVVM 中，UI 是通过数据驱动的，数据一旦改变就会相应的刷新对应的 UI，UI 如果改变，也会改变对应的数据。这种方式就可以在业务处理中只关心数据的流转，而无需直接和页面打交道。ViewModel 只关心数据和业务的处理，不关心 View 如何处理数据，在这种情况下，View 和 Model 都可以独立出来，任何一方改变了也不一定需要改变另一方，并且可以将一些可复用的逻辑放在一个 ViewModel 中，让多个 View 复用这个 ViewModel</font>
+ <font style="color:rgb(44, 62, 80);">在 MVVM 中，最核心的也就是数据双向绑定，例如 Angluar 的脏数据检测，Vue 中的数据劫持</font>

**<font style="color:rgb(44, 62, 80);">脏数据检测</font>**

+ <font style="color:rgb(44, 62, 80);">当触发了指定事件后会进入脏数据检测，这时会调用 $digest 循环遍历所有的数据观察者，判断当前值是否和先前的值有区别，如果检测到变化的话，会调用 $watch 函数，然后再次调用 $digest 循环直到发现没有变化。循环至少为二次 ，至多为十次</font>
+ <font style="color:rgb(44, 62, 80);">脏数据检测虽然存在低效的问题，但是不关心数据是通过什么方式改变的，都可以完成任务，但是这在 Vue 中的双向绑定是存在问题的。并且脏数据检测可以实现批量检测出更新的值，再去统一更新 UI，大大减少了操作 DOM 的次数</font>

**<font style="color:rgb(44, 62, 80);">数据劫持</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">内部使用了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Obeject.defineProperty()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来实现双向绑定，通过这个函数可以监听到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">set</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">get</font><font style="color:rgb(44, 62, 80);">的事件</font>

```javascript
var data = { name: 'yck' }
observe(data)
let name = data.name // -> get value
data.name = 'yyy' // -> change value

function observe(obj) {
  // 判断类型
  if (!obj || typeof obj !== 'object') {
    return
  }
  Object.keys(data).forEach(key => {
    defineReactive(data, key, data[key])
  })
}

function defineReactive(obj, key, val) {
  // 递归子属性
  observe(val)
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function reactiveGetter() {
      console.log('get value')
      return val
    },
    set: function reactiveSetter(newVal) {
      console.log('change value')
      val = newVal
    }
  })
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以上代码简单的实现了如何监听数据的 set 和 get 的事件，但是仅仅如此是不够的，还需要在适当的时候给属性添加发布订阅</font>

```html
<div>
    {{name}}
</div>
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在解析如上模板代码时，遇到</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">{name}</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">就会给属性</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">name</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">添加发布订阅</font>

```javascript
// 通过 Dep 解耦
class Dep {
  constructor() {
    this.subs = []
  }
  addSub(sub) {
    // sub 是 Watcher 实例
    this.subs.push(sub)
  }
  notify() {
    this.subs.forEach(sub => {
      sub.update()
    })
  }
}
// 全局属性，通过该属性配置 Watcher
Dep.target = null

function update(value) {
  document.querySelector('div').innerText = value
}

class Watcher {
  constructor(obj, key, cb) {
    // 将 Dep.target 指向自己
    // 然后触发属性的 getter 添加监听
    // 最后将 Dep.target 置空
    Dep.target = this
    this.cb = cb
    this.obj = obj
    this.key = key
    this.value = obj[key]
    Dep.target = null
  }
  update() {
    // 获得新值
    this.value = this.obj[this.key]
    // 调用 update 方法更新 Dom
    this.cb(this.value)
  }
}
var data = { name: 'yck' }
observe(data)
// 模拟解析到 `{{name}}` 触发的操作
new Watcher(data, 'name', update)
// update Dom innerText
data.name = 'yyy'
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">接下来,对 defineReactive 函数进行改造</font>

```javascript
function defineReactive(obj, key, val) {
  // 递归子属性
  observe(val)
  let dp = new Dep()
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function reactiveGetter() {
      console.log('get value')
      // 将 Watcher 添加到订阅
      if (Dep.target) {
        dp.addSub(Dep.target)
      }
      return val
    },
    set: function reactiveSetter(newVal) {
      console.log('change value')
      val = newVal
      // 执行 watcher 的 update 方法
      dp.notify()
    }
  })
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以上实现了一个简易的双向绑定，核心思路就是手动触发一次属性的 getter 来实现发布订阅的添加</font>

**<font style="color:rgb(44, 62, 80);">Proxy 与 Obeject.defineProperty 对比</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Obeject.defineProperty</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">虽然已经能够实现双向绑定了，但是他还是有缺陷的。</font>
    - <font style="color:rgb(44, 62, 80);">只能对属性进行数据劫持，所以需要深度遍历整个对象</font>
    - <font style="color:rgb(44, 62, 80);">对于数组不能监听到数据的变化</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">虽然</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vue</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中确实能检测到数组数据的变化，但是其实是使用了</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hack</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的办法，并且也是有缺陷的</font>

### [](https://www.123fe.net/docs/base.html#_134-web%E5%BA%94%E7%94%A8%E4%BB%8E%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%BB%E5%8A%A8%E6%8E%A8%E9%80%81data%E5%88%B0%E5%AE%A2%E6%88%B7%E7%AB%AF%E6%9C%89%E9%82%A3%E4%BA%9B%E6%96%B9%E5%BC%8F)<font style="color:rgb(44, 62, 80);">134 WEB应用从服务器主动推送Data到客户端有那些方式</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AJAX</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">轮询</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html5</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">服务器推送事件</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">(new EventSource(SERVER_URL)).addEventListener("message", func);</font>
+ <font style="color:rgb(44, 62, 80);">html5 Websocket</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">(new WebSocket(SERVER_URL)).addEventListener("message", func);</font>

### [](https://www.123fe.net/docs/base.html#_135-%E7%BB%A7%E6%89%BF)<font style="color:rgb(44, 62, 80);">135 继承</font>
+ **<font style="color:rgb(44, 62, 80);">原型链继承</font>**<font style="color:rgb(44, 62, 80);">，将父类的实例作为子类的原型，他的特点是实例是子类的实例也是父类的实例，父类新增的原型方法/属性，子类都能够访问，并且原型链继承简单易于实现，缺点是来自原型对象的所有属性被所有实例共享，无法实现多继承，无法向父类构造函数传参。</font>
+ **<font style="color:rgb(44, 62, 80);">构造继承</font>**<font style="color:rgb(44, 62, 80);">，使用父类的构造函数来增强子类实例，即复制父类的实例属性给子类，构造继承可以向父类传递参数，可以实现多继承，通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call</font><font style="color:rgb(44, 62, 80);">多个父类对象。但是构造继承只能继承父类的实例属性和方法，不能继承原型属性和方法，无法实现函数服用，每个子类都有父类实例函数的副本，影响性能</font>
+ **<font style="color:rgb(44, 62, 80);">实例继承</font>**<font style="color:rgb(44, 62, 80);">，为父类实例添加新特性，作为子类实例返回，实例继承的特点是不限制调用方法，不管是new 子类（）还是子类（）返回的对象具有相同的效果，缺点是实例是父类的实例，不是子类的实例，不支持多继承</font>
+ **<font style="color:rgb(44, 62, 80);">拷贝继承</font>**<font style="color:rgb(44, 62, 80);">：特点：支持多继承，缺点：效率较低，内存占用高（因为要拷贝父类的属性）无法获取父类不可枚举的方法（不可枚举方法，不能使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for in</font><font style="color:rgb(44, 62, 80);">访问到）</font>
+ **<font style="color:rgb(44, 62, 80);">组合继承</font>**<font style="color:rgb(44, 62, 80);">：通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用</font>
+ **<font style="color:rgb(44, 62, 80);">寄生组合继承</font>**<font style="color:rgb(44, 62, 80);">：通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点</font>

### [](https://www.123fe.net/docs/base.html#_136-this%E6%8C%87%E5%90%91)<font style="color:rgb(44, 62, 80);">136 this指向</font>
**<font style="color:rgb(44, 62, 80);">1. this 指向有哪几种</font>**

+ <font style="color:rgb(44, 62, 80);">默认绑定：全局环境中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">默认绑定到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font>
+ <font style="color:rgb(44, 62, 80);">隐式绑定：一般地，被直接对象所包含的函数调用时，也称为方法调用，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">隐式绑定到该直接对象</font>
+ <font style="color:rgb(44, 62, 80);">隐式丢失：隐式丢失是指被隐式绑定的函数丢失绑定对象，从而默认绑定到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font><font style="color:rgb(44, 62, 80);">。显式绑定：通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call()</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply()</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind()</font><font style="color:rgb(44, 62, 80);">方法把对象绑定到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">上，叫做显式绑定</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);">绑定：如果函数或者方法调用之前带有关键字</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);">，它就构成构造函数调用。对于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">绑定来说，称为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);">绑定</font>
    - <font style="color:rgb(44, 62, 80);">构造函数通常不使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return</font><font style="color:rgb(44, 62, 80);">关键字，它们通常初始化新对象，当构造函数的函数体执行完毕时，它会显式返回。在这种情况下，构造函数调用表达式的计算结果就是这个新对象的值</font>
    - <font style="color:rgb(44, 62, 80);">如果构造函数使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return</font><font style="color:rgb(44, 62, 80);">语句但没有指定返回值，或者返回一个原始值，那么这时将忽略返回值，同时使用这个新对象作为调用结果</font>
    - <font style="color:rgb(44, 62, 80);">如果构造函数显式地使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">return</font><font style="color:rgb(44, 62, 80);">语句返回一个对象，那么调用表达式的值就是这个对象</font>

**<font style="color:rgb(44, 62, 80);">2. 改变函数内部 this 指针的指向函数（bind，apply，call的区别）</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">apply</font><font style="color:rgb(44, 62, 80);">：调用一个对象的一个方法，用另一个对象替换当前对象。例如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">B.apply(A, arguments)</font><font style="color:rgb(44, 62, 80);">;即A对象应用B对象的方法</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call</font><font style="color:rgb(44, 62, 80);">：调用一个对象的一个方法，用另一个对象替换当前对象。例如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">B.call(A, args1,args2)</font><font style="color:rgb(44, 62, 80);">;即A对象调用B对象的方法</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bind</font><font style="color:rgb(44, 62, 80);">除了返回是函数以外，它的参数和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">call</font><font style="color:rgb(44, 62, 80);">一样</font>

**<font style="color:rgb(44, 62, 80);">3. 箭头函数</font>**

+ <font style="color:rgb(44, 62, 80);">箭头函数没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">，所以需要通过查找作用域链来确定</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">的值，这就意味着如果箭头函数被非箭头函数包含，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">绑定的就是最近一层非箭头函数的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">，</font>
+ <font style="color:rgb(44, 62, 80);">箭头函数没有自己的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);">对象，但是可以访问外围函数的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);">对象</font>
+ <font style="color:rgb(44, 62, 80);">不能通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new</font><font style="color:rgb(44, 62, 80);">关键字调用，同样也没有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">new.target</font><font style="color:rgb(44, 62, 80);">值和原型</font>

### [](https://www.123fe.net/docs/base.html#_137-%E5%88%A4%E6%96%AD%E6%98%AF%E5%90%A6%E6%98%AF%E6%95%B0%E7%BB%84)<font style="color:rgb(44, 62, 80);">137 判断是否是数组</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Array.isArray(arr</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.prototype.toString.call(arr) === '[Object Array]'</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arr instanceof Array</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">array.constructor === Array</font>

### [](https://www.123fe.net/docs/base.html#_138-%E5%8A%A0%E8%BD%BD)<font style="color:rgb(44, 62, 80);">138 加载</font>
**<font style="color:rgb(44, 62, 80);">1. 异步加载js的方法</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defer</font><font style="color:rgb(44, 62, 80);">：只支持IE如果您的脚本不会改变文档的内容，可将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defer</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性加入到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><script></font><font style="color:rgb(44, 62, 80);">标签中，以便加快处理文档的速度。因为浏览器知道它将能够安全地读取文档的剩余部分而不用执行脚本，它将推迟对脚本的解释，直到文档已经显示给用户为止</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">async</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML5</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，仅适用于外部脚本；并且如果在IE中，同时存在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defer</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">async</font><font style="color:rgb(44, 62, 80);">，那么</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">defer</font><font style="color:rgb(44, 62, 80);">的优先级比较高；脚本将在页面完成时执行</font>

**<font style="color:rgb(44, 62, 80);">2. 图片的懒加载和预加载</font>**

+ <font style="color:rgb(44, 62, 80);">预加载：提前加载图片，当用户需要查看时可直接从本地缓存中渲染。</font>
+ <font style="color:rgb(44, 62, 80);">懒加载：懒加载的主要目的是作为服务器前端的优化，减少请求数或延迟请求数</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">两种技术的本质：两者的行为是相反的，一个是提前加载，一个是迟缓甚至不加载。懒加载对服务器前端有一定的缓解压力作用，预加载则会增加服务器前端压力。</font>

### [](https://www.123fe.net/docs/base.html#_139-%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6)<font style="color:rgb(44, 62, 80);">139 垃圾回收</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">找出那些不再继续使用的变 量，然后释放其占用的内存。为此，垃圾收集器会按照固定的时间间隔(或代码执行中预定的收集时间)， 周期性地执行这一操作。</font>

**<font style="color:rgb(44, 62, 80);">标记清除</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">先所有都加上标记，再把环境中引用到的变量去除标记。剩下的就是没用的了</font>

**<font style="color:rgb(44, 62, 80);">引用计数</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">跟踪记录每 个值被引用的次数。清除引用次数为0的变量 </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">⚠️</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">会有循环引用问题 。循环引用如果大量存在就会导致内存泄露。</font>

### [](https://www.123fe.net/docs/base.html#_140-%E6%9C%89%E5%9B%9B%E4%B8%AA%E6%93%8D%E4%BD%9C%E4%BC%9A%E5%BF%BD%E7%95%A5enumerable%E4%B8%BAfalse%E7%9A%84%E5%B1%9E%E6%80%A7)<font style="color:rgb(44, 62, 80);">140 有四个操作会忽略enumerable为false的属性</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for...in</font><font style="color:rgb(44, 62, 80);">循环：只遍历对象自身的和继承的可枚举的属性。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.keys()</font><font style="color:rgb(44, 62, 80);">：返回对象自身的所有可枚举的属性的键名。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JSON.stringify()</font><font style="color:rgb(44, 62, 80);">：只串行化对象自身的可枚举的属性。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.assign()</font><font style="color:rgb(44, 62, 80);">： 忽略</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">enumerable</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">false</font><font style="color:rgb(44, 62, 80);">的属性，只拷贝对象自身的可枚举的属性。</font>

### [](https://www.123fe.net/docs/base.html#_141-%E5%B1%9E%E6%80%A7%E7%9A%84%E9%81%8D%E5%8E%86)<font style="color:rgb(44, 62, 80);">141 属性的遍历</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">ES6 一共有 5 种方法可以遍历对象的属性。</font>

**<font style="color:rgb(44, 62, 80);">（1）for...in</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for...in</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。</font>

**<font style="color:rgb(44, 62, 80);">（2）Object.keys(obj)</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.keys</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。</font>

**<font style="color:rgb(44, 62, 80);">（3）Object.getOwnPropertyNames(obj)</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.getOwnPropertyNames</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。</font>

**<font style="color:rgb(44, 62, 80);">（4）Object.getOwnPropertySymbols(obj)</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object.getOwnPropertySymbols</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">返回一个数组，包含对象自身的所有 Symbol 属性的键名。</font>

**<font style="color:rgb(44, 62, 80);">（5）Reflect.ownKeys(obj)</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Reflect.ownKeys</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。</font>

+ <font style="color:rgb(44, 62, 80);">首先遍历所有数值键，按照数值升序排列。</font>
+ <font style="color:rgb(44, 62, 80);">其次遍历所有字符串键，按照加入时间升序排列。</font>
+ <font style="color:rgb(44, 62, 80);">最后遍历所有 Symbol 键，按照加入时间升序排列。</font>

```plain
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">上面代码中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Reflect.ownKeys</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">方法返回一个数组，包含了参数对象的所有属性。这个数组的属性次序是这样的，首先是数值属性2和10，其次是字符串属性b和a，最后是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Symbol</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性。</font>

### [](https://www.123fe.net/docs/base.html#_142-%E4%B8%BA%E4%BB%80%E4%B9%88%E9%80%9A%E5%B8%B8%E5%9C%A8%E5%8F%91%E9%80%81%E6%95%B0%E6%8D%AE%E5%9F%8B%E7%82%B9%E8%AF%B7%E6%B1%82%E7%9A%84%E6%97%B6%E5%80%99%E4%BD%BF%E7%94%A8%E7%9A%84%E6%98%AF-1x1-%E5%83%8F%E7%B4%A0%E7%9A%84%E9%80%8F%E6%98%8E-gif-%E5%9B%BE%E7%89%87)<font style="color:rgb(44, 62, 80);">142 为什么通常在发送数据埋点请求的时候使用的是 1x1 像素的透明 gif 图片</font>
+ <font style="color:rgb(44, 62, 80);">能够完成整个 HTTP 请求+响应（尽管不需要响应内容）</font>
+ <font style="color:rgb(44, 62, 80);">触发 GET 请求之后不需要获取和处理数据、服务器也不需要发送数据</font>
+ <font style="color:rgb(44, 62, 80);">跨域友好</font>
+ <font style="color:rgb(44, 62, 80);">执行过程无阻塞</font>
+ <font style="color:rgb(44, 62, 80);">相比 XMLHttpRequest 对象发送 GET 请求，性能上更好</font>
+ <font style="color:rgb(44, 62, 80);">GIF的最低合法体积最小（最小的BMP文件需要74个字节，PNG需要67个字节，而合法的GIF，只需要43个字节）</font>

### [](https://www.123fe.net/docs/base.html#_143-%E5%9C%A8%E8%BE%93%E5%85%A5%E6%A1%86%E4%B8%AD%E5%A6%82%E4%BD%95%E5%88%A4%E6%96%AD%E8%BE%93%E5%85%A5%E7%9A%84%E6%98%AF%E4%B8%80%E4%B8%AA%E6%AD%A3%E7%A1%AE%E7%9A%84%E7%BD%91%E5%9D%80)<font style="color:rgb(44, 62, 80);">143 在输入框中如何判断输入的是一个正确的网址</font>
```plain
function isUrl(url) {
       try {
           new URL(url);
           return true;
       }catch(err){
     return false;
}}
```

### [](https://www.123fe.net/docs/base.html#_144-%E5%B8%B8%E7%94%A8%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E6%9C%89%E5%93%AA%E4%BA%9B%E5%B9%B6%E4%B8%BE%E4%BE%8B%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF)<font style="color:rgb(44, 62, 80);">144 常用设计模式有哪些并举例使用场景</font>
+ **<font style="color:rgb(44, 62, 80);">工厂模式</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">- 传入参数即可创建实例</font>
    - <font style="color:rgb(44, 62, 80);">虚拟 DOM 根据参数的不同返回基础标签的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vnode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和组件</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Vnode</font>
+ **<font style="color:rgb(44, 62, 80);">单例模式</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">- 整个程序有且仅有一个实例</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vuex</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue-router</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的插件注册方法</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">install</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">判断如果系统存在实例就直接返回掉</font>
+ **<font style="color:rgb(44, 62, 80);">发布-订阅模式</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vue</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">事件机制)</font>
+ **<font style="color:rgb(44, 62, 80);">观察者模式</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">(响应式数据原理)</font>
+ **<font style="color:rgb(44, 62, 80);">装饰模式</font>**<font style="color:rgb(44, 62, 80);">: (</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@</font><font style="color:rgb(44, 62, 80);">装饰器的用法)</font>
+ **<font style="color:rgb(44, 62, 80);">策略模式</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">策略模式指对象有某个行为,但是在不同的场景中,该行为有不同的实现方案-比如选项的合并策略</font>

### [](https://www.123fe.net/docs/base.html#_145-%E5%8E%9F%E5%9E%8B%E9%93%BE%E5%88%A4%E6%96%AD)<font style="color:rgb(44, 62, 80);">145 原型链判断</font>
<font style="color:rgb(44, 62, 80);">请写出下面的答案</font>

```plain
Object.prototype.__proto__;
Function.prototype.__proto__;
Object.__proto__;
Object instanceof Function;
Function instanceof Object;
Function.prototype === Function.__proto__;
```

<font style="color:rgb(44, 62, 80);">答案</font>

```plain
Object.prototype.__proto__; //null
Function.prototype.__proto__; //Object.prototype
Object.__proto__; //Function.prototype
Object instanceof Function; //true
Function instanceof Object; //true
Function.prototype === Function.__proto__; //true
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这道题目深入考察了原型链相关知识点 尤其是</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Function</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Object</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的之间的关系</font>

### [](https://www.123fe.net/docs/base.html#_146-raf-%E5%92%8C-ric-%E6%98%AF%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">146 RAF 和 RIC 是什么</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestAnimationFrame</font><font style="color:rgb(44, 62, 80);">： 告诉浏览器在下次重绘之前执行传入的回调函数(通常是操纵 dom，更新动画的函数)；由于是每帧执行一次，那结果就是每秒的执行次数与浏览器屏幕刷新次数一样，通常是每秒</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">60</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">次。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">requestIdleCallback</font><font style="color:rgb(44, 62, 80);">：: 会在浏览器空闲时间执行回调，也就是允许开发人员在主事件循环中执行低优先级任务，而不影响一些延迟关键事件。如果有多个回调，会按照先进先出原则执行，但是当传入了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">timeout</font><font style="color:rgb(44, 62, 80);">，为了避免超时，有可能会打乱这个顺序</font>

### [](https://www.123fe.net/docs/base.html#_147-%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1)<font style="color:rgb(44, 62, 80);">147 负载均衡</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">多台服务器共同协作，不让其中某一台或几台超额工作，发挥服务器的最大作用</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">http</font><font style="color:rgb(44, 62, 80);">重定向负载均衡：调度者根据策略选择服务器以302响应请求，缺点只有第一次有效果，后续操作维持在该服务器 dns负载均衡：解析域名时，访问多个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ip</font><font style="color:rgb(44, 62, 80);">服务器中的一个（可监控性较弱）</font>
+ <font style="color:rgb(44, 62, 80);">反向代理负载均衡：访问统一的服务器，由服务器进行调度访问实际的某个服务器，对统一的服务器要求大，性能受到 服务器群的数量</font>

### <font style="color:rgb(44, 62, 80);">148 CDN</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">内容分发网络，基本思路是尽可能避开互联网上有可能影响数据传输速度和稳定性的瓶颈和环节，使内容传输的更快、更稳定。</font>

### [](https://www.123fe.net/docs/base.html#_149-%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F)<font style="color:rgb(44, 62, 80);">149 内存泄漏</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">定义：程序中己动态分配的堆内存由于某种原因程序未释放或无法释放引发的各种问题。</font>

**<font style="color:rgb(44, 62, 80);">js中可能出现的内存泄漏情况</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">结果：变慢，崩溃，延迟大等，原因：</font>

+ <font style="color:rgb(44, 62, 80);">全局变量</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(44, 62, 80);">清空时，还存在引用</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ie</font><font style="color:rgb(44, 62, 80);">中使用闭包</font>
+ <font style="color:rgb(44, 62, 80);">定时器未清除</font>
+ <font style="color:rgb(44, 62, 80);">子元素存在引起的内存泄露</font>

**<font style="color:rgb(44, 62, 80);">避免策略</font>**

+ <font style="color:rgb(44, 62, 80);">减少不必要的全局变量，或者生命周期较长的对象，及时对无用的数据进行垃圾回收；</font>
+ <font style="color:rgb(44, 62, 80);">注意程序逻辑，避免“死循环”之类的 ；</font>
+ <font style="color:rgb(44, 62, 80);">避免创建过多的对象 原则：不用了的东西要及时归还。</font>
+ <font style="color:rgb(44, 62, 80);">减少层级过多的引用</font>

### <font style="color:rgb(44, 62, 80);">150 js自定义事件</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">三要素：</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document.createEvent()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">event.initEvent()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">element.dispatchEvent()</font>

```plain
// (en:自定义事件名称，fn:事件处理函数，addEvent:为DOM元素添加自定义事件，triggerEvent:触发自定义事件)
window.onload = function(){
    var demo = document.getElementById("demo");
    demo.addEvent("test",function(){console.log("handler1")});
    demo.addEvent("test",function(){console.log("handler2")});
    demo.onclick = function(){
        this.triggerEvent("test");
    }
}
Element.prototype.addEvent = function(en,fn){
    this.pools = this.pools || {};
    if(en in this.pools){
        this.pools[en].push(fn);
    }else{
        this.pools[en] = [];
        this.pools[en].push(fn);
    }
}
Element.prototype.triggerEvent  = function(en){
    if(en in this.pools){
        var fns = this.pools[en];
        for(var i=0,il=fns.length;i<il;i++){
            fns[i]();
        }
    }else{
        return;
    }
}
```

### <font style="color:rgb(44, 62, 80);">151 前后端路由差别</font>
+ <font style="color:rgb(44, 62, 80);">后端每次路由请求都是重新访问服务器</font>
+ <font style="color:rgb(44, 62, 80);">前端路由实际上只是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JS</font><font style="color:rgb(44, 62, 80);">根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">URL</font><font style="color:rgb(44, 62, 80);">来操作</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">元素，根据每个页面需要的去服务端请求数据，返回数据后和模板进行组合</font>

### <font style="color:rgb(44, 62, 80);">152 前端性能定位以及优化指标</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">前端性能优化 已经是老生常谈的一项技术了 很多人说起性能优化方案的时候头头是道 但是真正的对于性能分析定位和性能指标这块却一知半解 所以这道题虽然和性能相关 但是考察点在于平常项目如何进行性能定位和分析</font>

+ <font style="color:rgb(44, 62, 80);">我们可以从 前端性能监控-埋点以及</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window.performance</font><font style="color:rgb(44, 62, 80);">相关的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">api</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">去回答</font>
+ <font style="color:rgb(44, 62, 80);">也可以从性能分析工具</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Performance</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Lighthouse</font>
+ <font style="color:rgb(44, 62, 80);">还可以从性能指标</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LCP</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FCP</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FID</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CLS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等去着手</font>

### <font style="color:rgb(44, 62, 80);">153 offsetHeight-scrollHeight-clientHeight有什么区别</font>
**<font style="color:rgb(44, 62, 80);">计算规则</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">offsetHeight offsetWidth</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clientHeight clientWidth</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scrollHeight scrollWidth</font><font style="color:rgb(44, 62, 80);">：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">实际内容尺寸</font>

```html
<style>
  * {
    padding: 0;
    margin: 0;
  }

  body {
    background-color: f1f1f1;
  }

  container {
    width: 300px;
    height: 200px;
    padding: 20px;
    margin: 30px;
    border: 5px solid ccc;
    box-sizing: border-box;
    overflow: auto;
    background-color: fff;
  }
  content {
    width: 600px;
    height: 500px;
    background-color: f1f1f1;
    display: inline-block;
  }
</style>
<p>offsetHeight</p>

<div id="container">
  <div id="content">
    <p>offsetHeight scrollHeight clientHeight 区别</p>
  </div>
</div>

<script>
    const container = document.getElementById('container')
    
    console.log('offsetHeight', container.offsetHeight)
    console.log('offsetWidth', container.offsetWidth)
    console.log('clientWidth', container.clientWidth)
    console.log('clientHeight', container.clientHeight)
    console.log('scrollWidth', container.scrollWidth)
    console.log('scrollHeight', container.scrollHeight)

    // scrollTop scrollLeft 需滚动之后获取
</script>
```

### <font style="color:rgb(44, 62, 80);">154 JS严格模式有什么特点</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">'use strict'</font><font style="color:rgb(44, 62, 80);">开启严格模式</font>
+ <font style="color:rgb(44, 62, 80);">全局变量必须先声明</font>
+ <font style="color:rgb(44, 62, 80);">禁止使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">with</font>
+ <font style="color:rgb(44, 62, 80);">创建</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eval</font><font style="color:rgb(44, 62, 80);">作用域</font>
+ <font style="color:rgb(44, 62, 80);">禁止</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">指向</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">window</font>
+ <font style="color:rgb(44, 62, 80);">函数参数不能重名</font>

