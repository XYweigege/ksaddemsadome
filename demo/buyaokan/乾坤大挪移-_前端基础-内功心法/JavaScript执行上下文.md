### 前言
在编程这个行业中总是能听到这个词“执行上下文”。那么什么叫“执行上下文”呢？

本篇文章主要是介绍<font style="background-color:rgb(249, 242, 244);">javascript</font>中的执行上下文, 看完之后你可以了解到:

+ 执行上下文的类型
+ 执行上下文特点
+ 执行栈
+ 执行上下文的生命周期

### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#%E6%A6%82%E5%BF%B5)概念
首先我们来介绍什么是“执行上下文”.

举个例子，生活中，相同的话在不同的场合说可能会有不同的意思，而这个说话的场合就是我们说话的语境。

同样对应在编程中， 对程序语言进行“解读”的时候，也必须在特定的语境中，这个语境就是<font style="background-color:rgb(249, 242, 244);">javascript</font>中的执行上下文。

一句话概括：

<font style="background-color:rgb(255, 249, 249);">执行上下文就是</font><font style="background-color:rgb(249, 242, 244);">javascript</font><font style="background-color:rgb(255, 249, 249);">代码被解析和执行时所在环境的抽象概念。</font>

### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E7%9A%84%E7%B1%BB%E5%9E%8B)执行上下文的类型
在<font style="background-color:rgb(249, 242, 244);">js</font>中，执行上下文分为以下三种：

+ **全局执行上下文**：只有一个，也就是浏览器对象(即<font style="background-color:rgb(249, 242, 244);">window</font>对象)，<font style="background-color:rgb(249, 242, 244);">this</font>指向的就是这个全局对象。
+ **函数执行上下文**：有无数个，只有在函数**被调用**时才会被**创建**，每次调用函数都会创建一个新的执行上下文。
+ **Eval函数执行上下文**：<font style="background-color:rgb(249, 242, 244);">js</font>的<font style="background-color:rgb(249, 242, 244);">eval</font>函数执行其内部的代码会创建属于自己的执行上下文, 很少用而且不建议使用。

### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E7%9A%84%E7%89%B9%E7%82%B9)执行上下文的特点
1. 单线程，只在主线程上运行；
2. 同步执行，从上向下按顺序执行；
3. 全局上下文只有一个，也就是<font style="background-color:rgb(249, 242, 244);">window</font>对象；
4. 函数执行上下文没有限制；
5. 函数每调用一次就会产生一个新的执行上下文环境。

### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#js%E5%A6%82%E4%BD%95%E7%AE%A1%E7%90%86%E5%A4%9A%E4%B8%AA%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87)JS如何管理多个执行上下文
通过上面介绍，我们知道了<font style="background-color:rgb(249, 242, 244);">js</font>代码在运行时可能会产生无数个执行上下文，那么它是如何管理这些执行上下文的呢？

同时由于<font style="background-color:rgb(249, 242, 244);">js</font>是单线程的，所以不能同时干两件事，必须一个个去执行，那么这么多的执行上下文是按什么顺序执行的呢？

### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#%E6%89%A7%E8%A1%8C%E6%A0%88)执行栈
接下来就对上面的问题做出解答，管理多个执行上下文靠的就是**执行栈**，也被叫做**调用栈**。

**特点**：后进先出（LIFO）的结构。

**作用**：存储在代码执行期间的所有执行上下文。

（<font style="background-color:rgb(249, 242, 244);">LIFO</font>: <font style="background-color:rgb(249, 242, 244);">last-in, first-out</font>，类似于向乒乓球桶中放球，最先放入的球最后取出）

<font style="background-color:rgb(249, 242, 244);">js</font>在首次执行的时候，会创建一个**全局执行上下文**并推入栈中。

每当有函数被调用时，引擎都会为该函数创建一个新的**函数执行上下文**然后推入栈中。

当栈顶的函数执行完毕之后，该函数对应的**执行上下文**就会从执行栈中<font style="background-color:rgb(249, 242, 244);">pop</font>出，然后上下文控制权移到下一个执行上下文。

比如下面的一个例子🌰：

```javascript
var a = 1; // 1. 全局上下文环境
function bar (x) {
    console.log('bar')
    var b = 2;
    fn(x + b); // 3. fn上下文环境
}
function fn (c) {
    console.log(c);
}
bar(3); // 2. bar上下文环境
```

如下图：

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959604636-71723716-9841-4ef2-8963-821a235ddef8.png)

### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)执行上下文的生命周期
执行上下文的生命周期也非常容易理解, 分为三个阶段:

1. 创建阶段
2. 执行阶段
3. 销毁阶段

### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#%E5%88%9B%E5%BB%BA%E9%98%B6%E6%AE%B5)创建阶段
在**创建阶段**, 主要有是有这么几件事:

1. 确定**this**的值, 也就是**绑定this** (**This Binding**);
2. **词法环境(LexicalEnvironment)**组件被创建;
3. **变量环境(VariableEnvironment)**组件被创建.

_一张图方便你理解_ 🤔

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1718959604692-def6ddc1-6586-43c4-a6dd-bfd4a73d50c7.jpeg)

有一些教材中也喜欢用伪代码来实现:

```javascript
ExecutionContext = {  
  ThisBinding = <this value>,     // 确定this 
  LexicalEnvironment = { ... },   // 词法环境
  VariableEnvironment = { ... },  // 变量环境
}
```

#### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#this-binding)This Binding
通过上面的介绍我们知道实际开发主要用到两种执行上下文为**全局**和**函数**, 那么绑定<font style="background-color:rgb(249, 242, 244);">this</font>在这两种上下文中也不同.

+ 全局执行上下文中, <font style="background-color:rgb(249, 242, 244);">this</font>指的就是全局对象, 浏览器环境指向<font style="background-color:rgb(249, 242, 244);">window</font>对象, <font style="background-color:rgb(249, 242, 244);">nodejs</font>中指向这个文件的<font style="background-color:rgb(249, 242, 244);">module</font>对象.
+ 函数执行上下文较为复杂, <font style="background-color:rgb(249, 242, 244);">this</font>的值取决于函数的调用方式. 具体有: 默认绑定、隐式绑定、显式绑定、<font style="background-color:rgb(249, 242, 244);">new</font>绑定、箭头函数.

#### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#%E8%AF%8D%E6%B3%95%E7%8E%AF%E5%A2%83)词法环境
如上图, **词法环境**是由两个部分**组成**的:

1. **环境记录**: 存储变量和函数声明的实际位置;
2. **对外部环境的引用**: 用于访问其外部词法环境.

同样的, **词法环境**也主要有两种类型:

1. **全局环境**: 拥有一个全局对象(window对象)及其关联的所有属性和方法(比如数组的方法<font style="background-color:rgb(249, 242, 244);">splice、concat</font>等), 同时也包含了用户自定义的全局变量. 但是**全局环境**中没有外部环境的引用, 也就是外部环境引用为<font style="background-color:rgb(249, 242, 244);">null</font>.
2. **函数环境**: 用户在函数中自定义的变量和函数存储在**环境记录**中, 包含了<font style="background-color:rgb(249, 242, 244);">arguments</font>对象. 而对外部环境的引用可以是**全局环境**， 也可以是另一个**函数环境**(比如一个函数中包含了另一个函数).

继续用伪代码来实现:

```javascript
GlobalExectionContext = { // 全局执行上下文
	LexicalEnvironment: {   // 词法环境
		EnvironmentRecord: {   // 环境记录
			Type: "Object"       // 全局环境
      // 标识符绑定在这里
		},
    outer: <null>          // 外部环境引用
	}
}
FunctionExectionContext = { // 函数执行上下文
	LexicalEnvironment: {   // 词法环境
		EnvironmentRecord: {   // 环境记录
			Type: "Object",       // 函数环境
			// 标识符绑定在这里
		},
    outer: <Global or FunctionEnvironment> // 外部环境引用
	}
}
```

#### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#%E5%8F%98%E9%87%8F%E7%8E%AF%E5%A2%83)变量环境
**变量环境**其实也是一个词法环境, 因此它具有上面定义的词法环境的所有属性.

在 ES6 中，**词法** 环境和 **变量** 环境的区别在于前者用于存储**函数声明和变量（ <font style="background-color:rgb(249, 242, 244);">let</font> 和 <font style="background-color:rgb(249, 242, 244);">const</font> ）**绑定，而后者仅用于存储**变量（ <font style="background-color:rgb(249, 242, 244);">var</font> ）**绑定。

**案例****🌰**:

```javascript
var a;
var	b = 1;
let c = 2;
const d = 3;
function fn (e, f) {
	var g = 4;
	return e + f + g;
}
a = fn(10, 20);
```

执行上下文如下:

```javascript
GlobalExectionContext = { // 全局执行上下文

	ThisBinding: <Global Object>,
	
	LexicalEnvironment: {   // 词法环境
		EnvironmentRecord: {   // 环境记录
			Type: "Object",       // 全局环境
			c: < uninitialized >,
  		d: < uninitialized >,
			fn: < func >
		},
		outer: <null>            // 外部环境引用
	},
  
  VariableEnvironment: {   // 变量环境
    EnvironmentRecord: {   // 环境记录
      Type: "Object",
      a: < uninitialized >,
      b: < uninitialized >
    },
    outer: <null>  
  }
}
FunctionExectionContext = { // 函数执行上下文
  
  ThisBinding: <Global Object>, // this绑定window, 因为调用fn的是window对象
  
	LexicalEnvironment: {   // 词法环境
		EnvironmentRecord: {   // 环境记录
			Type: "Object",       // 函数环境
			Arguments: { 0: 10, 1: 20, length: 2 }
		},
    outer: < GlobalLexicalEnvironment > // 全局环境的引用
	},
  
  VariableEnvironment: {   // 变量环境
    EnvironmentRecord: {   // 环境记录
      Type: "Object",
      g: < uninitialized >
    },
    outer: < GlobalLexicalEnvironment > // 全局环境的引用
  }
}
```

因此我们可以知道**变量提升**的原因是:

<font style="background-color:rgb(255, 249, 249);">在创建阶段，函数声明存储在环境中，而变量会被设置为</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">undefined</font><font style="background-color:rgb(255, 249, 249);">（在</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">var</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">的情况下）或保持未初始化（在</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">let</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">和</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">const</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">的情况下）。所以这就是为什么可以在声明之前访问</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">var</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">定义的变量（尽管是</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">undefined</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">），但如果在声明之前访问</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">let</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">和</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">const</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">定义的变量就会提示引用错误的原因。这就是所谓的变量提升。</font>

### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#%E6%89%A7%E8%A1%8C%E9%98%B6%E6%AE%B5)执行阶段
执行阶段主要做三件事情:

1. 变量赋值
2. 函数引用
3. 执行其他的代码

**注****⚠️**

<font style="background-color:rgb(255, 249, 249);">如果 Javascript 引擎在源代码中声明的实际位置找不到</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">let</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">变量的值，那么将为其分配</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(249, 242, 244);">undefined</font><font style="background-color:rgb(255, 249, 249);"> </font><font style="background-color:rgb(255, 249, 249);">值。</font>

### [](https://www.123fe.net/blog-docs/javaScript2/19-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87.html#%E9%94%80%E6%AF%81%E9%98%B6%E6%AE%B5)销毁阶段
执行完毕出栈，等待回收被销毁

