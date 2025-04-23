### <font style="color:rgb(44, 62, 80);">变量提升</font>
<font style="color:rgb(44, 62, 80);">在使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">javascript</font><font style="color:rgb(44, 62, 80);">编写代码的时候, 我们知道, 声明一个变量用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var</font><font style="color:rgb(44, 62, 80);">, 定义一个函数用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function</font><font style="color:rgb(44, 62, 80);">.那你知道程序在运行它的时候, 都经历了什么吗?</font>

#### [](https://www.123fe.net/blog-docs/javaScript2/20-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88%E5%92%8C%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1.html#%E5%8F%98%E9%87%8F%E5%A3%B0%E6%98%8E%E6%8F%90%E5%8D%87)<font style="color:rgb(44, 62, 80);">变量声明提升</font>
<font style="color:rgb(44, 62, 80);">首先是用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var</font><font style="color:rgb(44, 62, 80);">定义一个变量的时候, 例如:</font>

```javascript
var a = 10;
```

<font style="color:rgb(44, 62, 80);">大部分的编程语言都是先声明变量再使用, 但是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">javascript</font><font style="color:rgb(44, 62, 80);">有所不同, 上面的代码, 实际相当于这样执行:</font>

```javascript
var a;
a = 10;
```

<font style="color:rgb(44, 62, 80);">因此有了下面这段代码的执行结果:</font>

```javascript
console.log(a); // 声明,先给一个默认值undefined;
var a = 10; // 赋值,对变量a赋值了10
console.log(a); // 10
```

<font style="color:rgb(44, 62, 80);">上面的代码</font><font style="color:rgb(44, 62, 80);">👆</font><font style="color:rgb(44, 62, 80);">在第一行中并不会报错</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Uncaught ReferenceError: a is not defined</font><font style="color:rgb(44, 62, 80);">, 是因为</font>**<font style="color:rgb(238, 63, 77);">声明提升</font>**<font style="color:rgb(44, 62, 80);">, 给了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(44, 62, 80);">一个默认值.</font>

<font style="color:rgb(44, 62, 80);">这就是最简单的</font>**<font style="color:rgb(238, 63, 77);">变量声明提升</font>**<font style="color:rgb(44, 62, 80);">.</font>

#### [](https://www.123fe.net/blog-docs/javaScript2/20-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88%E5%92%8C%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1.html#%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E%E6%8F%90%E5%8D%87)<font style="color:rgb(44, 62, 80);">函数声明提升</font>
<font style="color:rgb(44, 62, 80);">定义函数也有两种方法:</font>

+ <font style="color:rgb(44, 62, 80);">函数声明:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function foo () {}</font><font style="color:rgb(44, 62, 80);">;</font>
+ <font style="color:rgb(44, 62, 80);">函数表达式:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var foo = function () {}</font><font style="color:rgb(44, 62, 80);">.</font>

<font style="color:rgb(44, 62, 80);">第二种</font>**<font style="color:rgb(238, 63, 77);">函数表达式</font>**<font style="color:rgb(44, 62, 80);">的声明方式更像是给一个变量</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">foo</font><font style="color:rgb(44, 62, 80);">赋值一个匿名函数.</font>

<font style="color:rgb(44, 62, 80);">那这两种在函数声明的时候有什么区别吗?</font>

**<font style="color:rgb(238, 63, 77);">案例一</font>****<font style="color:rgb(238, 63, 77);">🌰</font>**<font style="color:rgb(44, 62, 80);">:</font>

```javascript
console.log(f1) // function f1(){}
function f1() {} // 函数声明
console.log(f2) // undefined
var f2 = function() {} // 函数表达式
```

<font style="color:rgb(44, 62, 80);">可以看到, 使用</font>**<font style="color:rgb(238, 63, 77);">函数声明</font>**<font style="color:rgb(44, 62, 80);">的函数会将</font>**<font style="color:rgb(238, 63, 77);">整个函数</font>**<font style="color:rgb(44, 62, 80);">都提升到作用域(后面会介绍到)的最顶部, 因此打印出来的是整个函数;</font>

<font style="color:rgb(44, 62, 80);">而使用</font>**<font style="color:rgb(238, 63, 77);">函数表达式</font>**<font style="color:rgb(44, 62, 80);">声明则类似于</font>**<font style="color:rgb(238, 63, 77);">变量声明提升</font>**<font style="color:rgb(44, 62, 80);">, 将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var f2</font><font style="color:rgb(44, 62, 80);">提升到了顶部并赋值</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">.</font>

---

<font style="color:rgb(44, 62, 80);">我们将案例一的代码添加一点东西:</font>

**<font style="color:rgb(238, 63, 77);">案例二</font>****<font style="color:rgb(238, 63, 77);">🌰</font>**<font style="color:rgb(44, 62, 80);">:</font>

```javascript
console.log(f1) // function f1(){...}
f1(); // 1
function f1() { // 函数声明
	console.log('1')
}
console.log(f2) // undefined
f2(); // 报错: Uncaught TypeError: f2 is not a function
var f2 = function() { // 函数表达式
	console.log('2')
}
```

<font style="color:rgb(44, 62, 80);">虽然</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">f1()</font><font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">function f1 () {...}</font><font style="color:rgb(44, 62, 80);">之前,但是却可以正常执行;</font>

<font style="color:rgb(44, 62, 80);">而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">f2()</font><font style="color:rgb(44, 62, 80);">却会报错, 原因在案例一中也介绍了是因为在调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">f2()</font><font style="color:rgb(44, 62, 80);">时,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">f2</font><font style="color:rgb(44, 62, 80);">还只是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undifined</font><font style="color:rgb(44, 62, 80);">并没有被赋值为一个函数, 因此会报错.</font>

#### [](https://www.123fe.net/blog-docs/javaScript2/20-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88%E5%92%8C%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1.html#%E5%A3%B0%E6%98%8E%E4%BC%98%E5%85%88%E7%BA%A7-%E5%87%BD%E6%95%B0%E5%A4%A7%E4%BA%8E%E5%8F%98%E9%87%8F)<font style="color:rgb(44, 62, 80);">声明优先级: 函数大于变量</font>
<font style="color:rgb(44, 62, 80);">通过上面的介绍我们已经知道了两种声明提升, 但是当遇到函数和变量同名且都会被提升的情况时,</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(238, 63, 77);">函数声明</font>**<font style="color:rgb(44, 62, 80);">的优先级是要</font>**<font style="color:rgb(238, 63, 77);">大于变量声明</font>**<font style="color:rgb(44, 62, 80);">的.</font>

+ <font style="color:rgb(44, 62, 80);">变量声明会被函数声明覆盖</font>
+ <font style="color:rgb(44, 62, 80);">可以重新赋值</font>

**<font style="color:rgb(238, 63, 77);">案例一</font>****<font style="color:rgb(238, 63, 77);">🌰</font>**<font style="color:rgb(44, 62, 80);">:</font>

```javascript
console.log(f1); // f f1() {...}
var f1 = "10";
function f1() {
	console.log('我是函数')
}
// 或者将 var f1 = "10"; 放到后面
```

<font style="color:rgb(44, 62, 80);">案例一说明了变量声明会被函数声明所覆盖.</font>

**<font style="color:rgb(238, 63, 77);">案例二</font>****<font style="color:rgb(238, 63, 77);">🌰</font>**<font style="color:rgb(44, 62, 80);">:</font>

```javascript
console.log(f1); // f f1() { console.log('我是新的函数') }
var f1 = "10";

function f1() {
		console.log('我是函数')
}

function f1() {
		console.log('我是新的函数')
}
```

<font style="color:rgb(44, 62, 80);">案例二说明了前面声明的函数会被后面声明的同名函数给覆盖.</font>

<font style="color:rgb(44, 62, 80);">如果你搞懂了, 来做个小练习?</font>

**<font style="color:rgb(238, 63, 77);">练习</font>****<font style="color:rgb(238, 63, 77);">✍️</font>**

```javascript
function test(arg) {
		console.log(arg);
		var arg = 10;
		function arg() {
				console.log('函数')
		}
		console.log(arg)
}
test('LinDaiDai');
```

**<font style="color:rgb(238, 63, 77);">答案</font>****<font style="color:rgb(238, 63, 77);">📖</font>**

```javascript
function test(arg) {
		console.log(arg); // f arg() { console.log('函数') }
		var arg = 10;
		function arg() {
				console.log('函数')
		}
		console.log(arg); // 10
}
test('LinDaiDai');
```

1. <font style="color:rgb(44, 62, 80);">函数里的形参</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arg</font><font style="color:rgb(44, 62, 80);">被后面</font>**<font style="color:rgb(238, 63, 77);">函数声明</font>**<font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arg</font><font style="color:rgb(44, 62, 80);">给覆盖了, 所以第一个打印出的是函数;</font>
2. <font style="color:rgb(44, 62, 80);">当执行到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var arg = 10</font><font style="color:rgb(44, 62, 80);">的时候,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arg</font><font style="color:rgb(44, 62, 80);">又被赋值了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">10</font><font style="color:rgb(44, 62, 80);">, 所以第二个打印出</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">10</font><font style="color:rgb(44, 62, 80);">.</font>

### [](https://www.123fe.net/blog-docs/javaScript2/20-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88%E5%92%8C%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1.html#%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88%E7%9A%84%E5%8F%98%E5%8C%96)<font style="color:rgb(44, 62, 80);">执行上下文栈的变化</font>
<font style="color:rgb(44, 62, 80);">先来看看下面两段代码, 在执行结果上是一样的, 那么它们在执行的过程中有什么不同吗?</font>

```javascript
var scope = "global";
function checkScope () {
	var scope = "local";
	function fn () {
		return scope;
	}
	return fn();
}
checkScope();
```

```javascript
var scope = "global"
function checkScope () {
	var scope = "local"
	function fn () {
		return scope
	}
	return fn;
}
checkScope()();
```

<font style="color:rgb(44, 62, 80);">答案是</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(238, 63, 77);">执行上下文栈的变化</font>**<font style="color:rgb(44, 62, 80);">不一样。</font>

<font style="color:rgb(44, 62, 80);">在第一段代码中, 栈的变化是这样的:</font>

```javascript
ECStack.push(<checkscope> functionContext);
ECStack.push(<f> functionContext);
ECStack.pop();
ECStack.pop();
```

<font style="color:rgb(44, 62, 80);">可以看到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fn</font><font style="color:rgb(44, 62, 80);">后被推入栈中, 但是先执行了, 所以先被推出栈;</font>

---

<font style="color:rgb(44, 62, 80);">而在第二段中, 栈的变化为:</font>

```javascript
ECStack.push(<checkscope> functionContext);
ECStack.pop();
ECStack.push(<f> functionContext);
ECStack.pop();
```

<font style="color:rgb(44, 62, 80);">由于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">checkscope</font><font style="color:rgb(44, 62, 80);">是先推入栈中且先执行的, 所以在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fn</font><font style="color:rgb(44, 62, 80);">被执行前就被推出了.</font>

### [](https://www.123fe.net/blog-docs/javaScript2/20-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88%E5%92%8C%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1.html#vo-ao)<font style="color:rgb(44, 62, 80);">VO/AO</font>
<font style="color:rgb(44, 62, 80);">接下来要介绍两个概念:</font>

+ **<font style="color:rgb(238, 63, 77);">VO(变量对象)</font>**<font style="color:rgb(44, 62, 80);">, 也就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">variable object</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font>**<font style="color:rgb(238, 63, 77);">创建执行上下文</font>**<font style="color:rgb(44, 62, 80);">时与之关联的会有一个变量对象，该上下文中的所有变量和函数全都保存在这个对象中。</font>
+ **<font style="color:rgb(238, 63, 77);">AO(活动对象)</font>**<font style="color:rgb(44, 62, 80);">, 也就是``activation object`,</font>**<font style="color:rgb(238, 63, 77);">进入到一个执行上下文</font>**<font style="color:rgb(44, 62, 80);">时，此执行上下文中的变量和函数都可以被访问到，可以理解为被激活了。</font>

<font style="color:rgb(44, 62, 80);">活动对象和变量对象的区别在于:</font>

+ <font style="color:rgb(44, 62, 80);">变量对象（</font>**<font style="color:rgb(238, 63, 77);">VO</font>**<font style="color:rgb(44, 62, 80);">）是规范上或者是JS引擎上实现的，并不能在JS环境中直接访问。</font>
+ <font style="color:rgb(44, 62, 80);">当进入到一个执行上下文后，这个变量对象才会被</font>**<font style="color:rgb(238, 63, 77);">激活</font>**<font style="color:rgb(44, 62, 80);">，所以叫活动对象（</font>**<font style="color:rgb(238, 63, 77);">AO</font>**<font style="color:rgb(44, 62, 80);">），这时候活动对象上的各种属性才能被访问。</font>

<font style="color:rgb(44, 62, 80);">上面似乎说的比较难理解</font><font style="color:rgb(44, 62, 80);">😢</font><font style="color:rgb(44, 62, 80);">, 没关系, 我们慢慢来看.</font>

### [](https://www.123fe.net/blog-docs/javaScript2/20-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88%E5%92%8C%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1.html#%E6%89%A7%E8%A1%8C%E8%BF%87%E7%A8%8B)<font style="color:rgb(44, 62, 80);">执行过程</font>
<font style="color:rgb(44, 62, 80);">首先来看看一个**执行上下文(EC)**被创建和执行的过程:</font>

1. <font style="color:rgb(44, 62, 80);">创建阶段:</font>
+ <font style="color:rgb(44, 62, 80);">创建变量、参数、函数</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);">对象;</font>
+ <font style="color:rgb(44, 62, 80);">建立作用域链;</font>
+ <font style="color:rgb(44, 62, 80);">确定</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">this</font><font style="color:rgb(44, 62, 80);">的值.</font>
1. <font style="color:rgb(44, 62, 80);">执行阶段:</font>

<font style="color:rgb(44, 62, 80);">变量赋值, 函数引用, 执行代码.</font>

#### [](https://www.123fe.net/blog-docs/javaScript2/20-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88%E5%92%8C%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1.html#%E8%BF%9B%E5%85%A5%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87)<font style="color:rgb(44, 62, 80);">进入执行上下文</font>
<font style="color:rgb(44, 62, 80);">在创建阶段, 也就是还没有执行代码之前</font>

<font style="color:rgb(44, 62, 80);">此时的变量对象包括(如下顺序初始化):</font>

1. <font style="color:rgb(44, 62, 80);">函数的所有形参(仅在函数上下文): 没有实参, 属性值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">;</font>
2. <font style="color:rgb(44, 62, 80);">函数声明：如果变量对象已经存在相同名称的属性，则完全</font>**<font style="color:rgb(238, 63, 77);">替换</font>**<font style="color:rgb(44, 62, 80);">这个属性;</font>
3. <font style="color:rgb(44, 62, 80);">变量声明：如果变量名称跟已经声明的形参或函数相同，则变量声明</font>**<font style="color:rgb(238, 63, 77);">不会干扰</font>**<font style="color:rgb(44, 62, 80);">已经存在的这类属性</font>

<font style="color:rgb(44, 62, 80);">一起来看下面的例子</font><font style="color:rgb(44, 62, 80);">🌰</font><font style="color:rgb(44, 62, 80);">:</font>

```javascript
function fn (a) {
	var b = 2;
	function c () {};
	var d = function {};
	b = 20
}
fn(1)
```

<font style="color:rgb(44, 62, 80);">对于上面的例子, 此时的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AO</font><font style="color:rgb(44, 62, 80);">是:</font>

```javascript
AO = {
	arguments: {
		0: 1,
		length: 1
	},
	a: 1,
	b: undefined,
	c: reference to function c() {},
	d: undefined
}
```

<font style="color:rgb(44, 62, 80);">可以看到, 形参</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">arguments</font><font style="color:rgb(44, 62, 80);">此时已经有赋值了, 但是变量还是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">undefined</font><font style="color:rgb(44, 62, 80);">.</font>

#### [](https://www.123fe.net/blog-docs/javaScript2/20-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88%E5%92%8C%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1.html#%E4%BB%A3%E7%A0%81%E6%89%A7%E8%A1%8C)<font style="color:rgb(44, 62, 80);">代码执行</font>
<font style="color:rgb(44, 62, 80);">到了代码执行时, 会修改变量对象的值, 执行完后</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">AO</font><font style="color:rgb(44, 62, 80);">如下:</font>

```javascript
AO = {
	arguments: {
		0: 1,
		length: 1
	},
	a: 1,
	b: 20,
	c: reference to function c() {},
	d: reference to function d() {}
}
```

<font style="color:rgb(44, 62, 80);">在此阶段, 前面的变量对象中的值就会被赋值了, 此时变量对象处于激活状态.</font>

### [](https://www.123fe.net/blog-docs/javaScript2/20-JavaScript%E8%BF%9B%E9%98%B6_%E6%89%A7%E8%A1%8C%E4%B8%8A%E4%B8%8B%E6%96%87%E6%A0%88%E5%92%8C%E5%8F%98%E9%87%8F%E5%AF%B9%E8%B1%A1.html#%E6%80%BB%E7%BB%93)<font style="color:rgb(44, 62, 80);">总结</font>
+ <font style="color:rgb(44, 62, 80);">全局上下文的变量对象初始化是全局对象, 而函数上下文的变量对象初始化只有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Arguments</font><font style="color:rgb(44, 62, 80);">对象;</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">EC</font><font style="color:rgb(44, 62, 80);">创建阶段分为创建阶段和代码执行阶段;</font>
+ <font style="color:rgb(44, 62, 80);">在进入执行上下文时会给变量对象</font>**<font style="color:rgb(238, 63, 77);">添加形参、函数声明、变量声明</font>**<font style="color:rgb(44, 62, 80);">等初始的属性值;</font>
+ <font style="color:rgb(44, 62, 80);">在代码执行阶段，会再次修改变量对象的属性值.</font>

