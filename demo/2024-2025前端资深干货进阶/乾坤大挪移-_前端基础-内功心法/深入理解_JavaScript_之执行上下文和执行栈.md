<font style="color:rgb(31, 35, 40);">首先先来了解几个专业概念</font>

+ <font style="color:rgb(31, 35, 40);">EC：函数执行环境（或执行上下文），Execution Context</font>
+ <font style="color:rgb(31, 35, 40);">ECS：执行环境栈，Execution Context Stack</font>
+ <font style="color:rgb(31, 35, 40);">VO：变量对象，Variable Object</font>
+ <font style="color:rgb(31, 35, 40);">AO：活动对象，Active Object</font>
+ <font style="color:rgb(31, 35, 40);">scope chain：作用域链</font>
+ <font style="color:rgb(31, 35, 40);">this</font>

## <font style="color:rgb(31, 35, 40);">什么是执行上下文</font>
<font style="color:rgb(31, 35, 40);">每次当控制器转到 ECMAScript 可执行代码的时候，它都是在执行上下文中运行，即是指当前执行环境中的变量、函数声明，参数，作用域链，this 等信息。</font>

### <font style="color:rgb(31, 35, 40);">代码示例</font>
```javascript
const ExecutionContextObj = {
  VO: window, // 变量对象
  ScopeChain: {}, // 作用域链
  this: window,
}
```

## <font style="color:rgb(31, 35, 40);">执行上下文的类型</font>
<font style="color:rgb(31, 35, 40);">JavaScript 中有三种执行上下文类型。</font>

1. **<font style="color:rgb(31, 35, 40);">全局执行上下文</font>**<font style="color:rgb(31, 35, 40);">—— 这是默认上下文，浏览器中的全局对象就是 window 对象，任何不在函数内部的代码都在全局上下文中，this 指向这个全局对象。</font>
2. **<font style="color:rgb(31, 35, 40);">函数执行上下文</font>**<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">—— 当</font>**<font style="color:rgb(31, 35, 40);">函数被调用时创建</font>**<font style="color:rgb(31, 35, 40);">,会为该函数创建一个新的执行上下文，可以有任意个。</font>
3. **<font style="color:rgb(31, 35, 40);">Eval 函数执行上下文</font>**<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">—— 执行 eval 函数内部的代码也有属于它的上下文，由于开发中是尽量避免或不用 eval 函数，故此不作讨论。</font>

## <font style="color:rgb(31, 35, 40);">执行栈</font>
<font style="color:rgb(31, 35, 40);">执行栈，也叫调用栈，被用来存储代码运行时创建的所有执行上下文。</font>

栈：一种数据结构，遵循后进先出的原则

<font style="color:rgb(31, 35, 40);">当 JavaScript 引擎第一次遇到脚本时，它会创建一个全局的执行上下文并且压入当前执行栈。每当引擎遇到一个函数调用，它会为该函数创建一个新的执行上下文并压入栈的顶部。</font>

<font style="color:rgb(31, 35, 40);">引擎会执行那些执行上下文位于栈顶的函数。当该函数执行结束时，执行上下文从栈中弹出，控制流程到达当前栈中的下一个上下文。</font>

```javascript
function fn1() {
  console.log('fn1被调用了 -- 创建了fn1的函数执行上下文，压入栈')
  fn2()
  console.log('fn2执行完成，fn2的执行上下文会从栈中弹出')
}

function fn2() {
  console.log('fn2被调用了 -- 创建了fn2的函数执行上下文，压入栈')
}

fn1()
console.log('fn1执行完成，fn2的执行上下文会从栈中弹出')
```

<font style="color:rgb(31, 35, 40);">运行结果：</font>

```javascript
fn1被调用了 -- 创建了fn1的函数执行上下文，压入栈
fn2被调用了 -- 创建了fn2的函数执行上下文，压入栈
fn2执行完成，fn2的执行上下文会从栈中弹出
fn1执行完成，fn2的执行上下文会从栈中弹出
```

<font style="color:rgb(31, 35, 40);">上述代码的执行上下文栈：</font><font style="color:rgb(31, 35, 40);"> </font>![](https://camo.githubusercontent.com/009d35d0fcc8bb8c54c727520c4f5afee5b288587cf44003b535a144482abf82/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031392f31312f32372f313665616437633034316134623431353f773d3132373626683d33313626663d706e6726733d3330373136)

<font style="color:rgb(31, 35, 40);">当上述代码在浏览器加载时，JavaScript 引擎创建了一个全局执行上下文并把它压入栈中，当函数 fn1()被调用时，JavaScript 为该函数创建了一个函数执行上下文，并把它压入当前执行栈的顶部。</font>

<font style="color:rgb(31, 35, 40);">当 fn1()函数内部调用 fn2()函数时，JavaScript 引擎同样创建了 fn2()的函数执行上下文并压入栈的顶部。然后执行了 fn2()函数后，fn2()函数会从当前栈（后进先出结构）弹出，并且按程序执行顺序继续执行 fn1()函数，即此刻处于 fn1 的函数执行上下文。</font>

<font style="color:rgb(31, 35, 40);">当 fn1()函数执行完毕，它的执行上下文从栈弹出，控制流程到达全局执行上下文。一旦所有代码执行完毕，JavaScript 引擎从当前栈中移除全局执行上下文。</font>

## <font style="color:rgb(31, 35, 40);">执行上下文的创建</font>
<font style="color:rgb(31, 35, 40);">已经知道 JavaScript 怎样管理执行上下文了，现在来了解 JavaScript 引擎是怎么创建执行上下文的。</font>

<font style="color:rgb(31, 35, 40);">创建执行上下文有两个阶段：</font>

1. <font style="color:rgb(31, 35, 40);">创建阶段</font>
2. <font style="color:rgb(31, 35, 40);">执行阶段。</font>

<font style="color:rgb(31, 35, 40);">在 JavaScript 代码执行前，执行上下文将经历创建阶段。在创建阶段会发生三件事：</font>

1. <font style="color:rgb(31, 35, 40);">this 绑定</font>
2. <font style="color:rgb(31, 35, 40);">创建(LexicalEnvironment)词法环境组件</font>
3. <font style="color:rgb(31, 35, 40);">创建(VariableEnvironment)变量环境组件</font>

<font style="color:rgb(31, 35, 40);">执行上下文在概念可表示为：</font>

```javascript
ExecutionContext = {
  ThisBinding = <this value>,
  LexicalEnvironment = { ... },
  VariableEnvironment = { ... },
}
```

## <font style="color:rgb(31, 35, 40);">1. 创建阶段</font>
### <font style="color:rgb(31, 35, 40);">this 绑定</font>
<font style="color:rgb(31, 35, 40);">在全局执行上下文中，this 的值指向全局对象。(在浏览器中，this 引用 window 对象)</font>

<font style="color:rgb(31, 35, 40);">在函数执行上下文中，this 的指向取决于函数是如何被调用的,在本篇暂不对 this 指向做详细讨论。</font>

```javascript
let obj = {
  fn: function () {
    console.log(this)
  },
}
let win = obj.fn

obj.fn() //this指向obj
win() // this指向window
```

### <font style="color:rgb(31, 35, 40);">词法环境（Lexical Environment）</font>
<font style="color:rgb(31, 35, 40);">ES6 官方文档把词法环境定义为：</font>

词法环境是用来定义 基于词法嵌套结构的 ECMAScript 代码内的标识符与变量值和函数值之间的关联关系 的一种规范类型。一个词法环境由环境记录（Environment Record）和一个可能为 null 的对外部词法环境的引用（outer）组成。一般来说，词法环境都与特定的 ECMAScript 代码语法结构相关联，例如函数、代码块、TryCatch 中的 Catch 从句，并且每次执行这类代码时都会创建新的词法环境。

<font style="color:rgb(31, 35, 40);">可以理解为词法环境是一种包含标识符(变量/函数的名称)和变量(函数/原始值/数组对象等)映射的数据结构</font>

#### <font style="color:rgb(31, 35, 40);">词法环境有两个组成部分</font>
1. <font style="color:rgb(31, 35, 40);">声明式环境记录器：存储变量和函数声明的实际位置</font>
2. <font style="color:rgb(31, 35, 40);">对象环境记录器：可以访问其外部词法环境(作用域)</font>

#### <font style="color:rgb(31, 35, 40);">词法环境有两种类型</font>
1. <font style="color:rgb(31, 35, 40);">全局环境：是一个没有外部环境的词法环境，其外部环境引用为 null。拥有一个全局对象（window 对象）及其关联的方法和属性（例如数组方法）以及任何用户自定义的全局变量，this 的值指向这个全局对象。</font>
2. <font style="color:rgb(31, 35, 40);">函数环境：用户在函数中定义的变量被存储在环境记录中，包含了 arguments 对象。对外部环境的引用可以是全局环境，也可以是包含内部函数的外部函数环境。</font>

### <font style="color:rgb(31, 35, 40);">变量环境 (VariableEnvironment)</font>
<font style="color:rgb(31, 35, 40);">变量环境也是一个词法环境，因此它具有上面定义的词法环境的所有属性。</font>

<font style="color:rgb(31, 35, 40);">在 ES6 中，词法环境组件和变量环境组件之间的一个区别是前者用于存储函数声明和变量 let 和 const 绑定，而后者仅用于存储变量 var 绑定。</font>

## <font style="color:rgb(31, 35, 40);">2.执行阶段</font>
<font style="color:rgb(31, 35, 40);">在此阶段，完成对所有这些变量的分配，最后执行代码。（在执行阶段，如果 JavaScript 引擎不能在源码中声明的实际位置找到 let 变量的值，它会被赋值为 undefined）</font>



### 二、原理梳理  
1，整体流程概览
<font style="color:rgb(77, 77, 77);">JS解释引擎是边解析边执行的。JS解释引擎在载入一段脚本（进入任何一段<script>标签范围（包括通过src引入的外表脚本））的时候，会执行这个流程：（多个script标签之间的加载执行顺序问题本文暂时不讨论）</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1722323993802-5c1216b8-a962-486c-a047-c2f4b0b1dc21.png)

####   
JS解释引擎是边解析边执行的。JS解释引擎在载入一段脚本（进入任何一段）
2，创建执行上下文  
2.1，创建执行上下文概览  
当浏览器首次载入脚本，它将默认进入全局执行上下文。如果在全局代码中调用一个函数，解释引擎执行流将进入被调用的函数，并创建一个新的执行上下文，并将新创建的上下文压入执行栈的顶部。如果你调用当前函数内部的其他函数，相同的事情会再次上演。代码的执行流程进入内部函数，创建一个新的执行上下文并把它压入执行栈的顶部。浏览器总会执行位于栈顶的执行上下文，一旦当前上下文函数执行结束，它将被从栈顶弹出，并将上下文控制权交给当前的栈。这样，堆栈中的上下文就会被依次执行并且弹出堆栈，直到回到全局的上下文。

参考下图：

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1722324092705-b5bcc87e-0190-4a3d-9d02-1083ccd5aa95.png)





执行上下文栈示例：

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1722324190363-ee7dbe9f-3514-4645-9823-268a64242af5.png)

2.2，创建执行上下文示例：MDN上的一个例子

```javascript

function foo(i) {
    if (i < 0) return;
    console.log('begin:' + i);
    foo(i - 1);
    console.log('end:' + i);
}
foo(2);
// 输出:
// begin:2
// begin:1
// begin:0
// end:0
// end:1
// end:2

```

其执行上下文的入栈、出栈流程示意图：

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1722324214230-0e24a879-6d43-4788-8261-f5ddcf05a647.png)

3，创建全局执行上下文，预编译和执行全局代码  
这个环节相对简单点，先直接上一个流程图。细节部分参考函数的预编译和执行。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1722324252730-571533a0-8e29-48da-b192-135900296ba6.png)

4，创建函数执行上下文，预编译和执行函数代码  
整体流程：解析代码，创建函数执行上下文，压入执行上下文栈，然后逐行执行。函数执行完成之后，将该函数的Execution Context出栈。

<br/>color1
1、查找调用函数的代码。  
2、执行代码之前，先进入创建上下文阶段：  
    - 初始化作用域[[Scope]]，（拷贝传入的父执行上下文的Scope），数据结构应该是数组或者链表。  
    - 创建活动对象，创建完成之后，将活动对象推入作用域链的最前端：  
        - 创建arguments对象，检查上下文，初始化参数名称和值并创建引用的复制。  
        - 扫描上下文的函数声明（而非函数表达式）：  
            - 为发现的每一个函数，在变量对象上创建一个属性——确切的说是函数的名字——其有一个指向函数在内存中的引用。  
            - 如果函数的名字已经存在，引用指针将被重写。函数声明比变量优先级要高，并且定义过程不会被变量覆盖，除非是赋值  
    - 扫描上下文的变量声明：  
        - 为发现的每个变量声明，在变量对象上创建一个属性——就是变量的名字，并且将变量的值初始化为undefined  
        - 如果变量的名字已经在变量对象里存在，将不会进行任何操作并继续扫描。  
    - 求出上下文内部this的值。

3、激活/代码执行阶段：

+ 在当前上下文上运行/解释函数代码，并随着代码一行行执行指派变量的值。

<br/>

 流程图参考：

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1722324320967-703bb408-0423-465b-bf87-2145d41043bb.png)

5，示例分析  
5.1，VO/AO创建分析

```javascript

var a = "outer";
function foo(i){
    console.log(a);
    console.log(b);
    console.log(c);
    var a = 'hello'
    var b = function(){}
    function c(){}
    console.log(------------);
    console.log(a);
    console.log(b);
    console.log(c);
}
foo(22)
```

  
上述全局代码的EC创建阶段是这样的 

```javascript
// 模拟的伪代码
// 全局EC
GlobalECObj = {
    [[Scope]] : [VO],
    VO : {
        foo : fnFoo,
        a : "outer"
    },
    this : {}
}

```

当我们调用foo(22)时，创建阶段是下面这样的

```javascript
// 伪代码，函数EC
ECObj = {
    [[Scope]] : [
        {AO},
        {GlobalVO}
    ],
    AO: {
        arguments: {
                0: 22,
                length: 1
        },
        i: 22,
        c: pointer to function c()
        a: undefined,
        b: undefined
    },
    this: { ... }
}

```

正如我们看到的，在上下文创建阶段，VO的初始化过程如下（该过程是有先后顺序的：函数的形参>>函数声明>>变量声明）：

函数的形参和arguments（当进入函数执行上下文时） —— 活动对象的一个属性，其属性名就是形参的名字，其值就是实参的值；对于没有传递的参数，其值为undefined

函数声明（FunctionDeclaration, FD） —— 活动对象的一个属性，其属性名和值都是函数对象创建出来的；如果活动对象已经包含了相同名字的属性，则替换它的值；（含义之一是如果函数的形参已经包含相同的名字的形参，则替换它的值）。

变量声明（var，VariableDeclaration） —— 活动对象的一个属性，其属性名即为变量名，其值为undefined;如果变量名和已经声明的函数名或者函数的参数名相同，则不会影响已经存在的属性。

对于函数的形参没有什么可说的，主要看一下函数的声明以及变量的声明两个部分。

5.2、如何理解函数声明过程中如果变量对象已经包含了相同名字的属性，则替换它的值这句话？  
看如下这段代码：

```javascript
function foo1(a){
    console.log(a); // 'function a(){}'
    function a(){} 
}
foo1(20)

```

我们知道AO创建过程中，函数形参的时机是先于函数的声明的，结果是函数体内部声明的function a(){}覆盖了函数形参a的声明，因此最后输出a是一个function 

详细步骤见：

```javascript
// 步骤1：根据形参创建arguments，填充形参，用实参赋值给对应的形参。没有实参的赋值为undefined
AO_Step1: {
    arguments: {
            0: 20,
            length: 1
    },
    a: 20,
},
// 步骤2：扫描函数声明，此时发现名称为a的函数声明，将其添加到活动对象上，替换掉已经存在的相同名称的属性a，也就是替换你掉形参a的值，替换为函数引用。

AO_Step2: {
    arguments: {
            0: 20,
            length: 1
    },
    a: 指向 function a(){} ,
},

// 步骤3：扫描变量声明，未发现有变量声明。
// 因此，执行阶段，在函数的第一行，输出的是'function a(){}'
```

5.3、如何理解变量声明过程中如果变量名和已经声明的函数名或者函数的参数名相同，则不会影响已经存在的属性这句话？

```javascript


//情景一：与参数名相同
function foo2(a){
    console.log(a) // 20
    var a = 10
    console.log(a) // 10
}
foo2(20) 
//情景二：变量与函数名相同
function foo21(){
    console.log(a) // function a(){}
    var a = 10
    function a(){}
    console.log(a) // 10
}
foo21() 
//情景三：参数、函数名、变量名相同。哈哈，真实项目中，谁这样写得拉出去突突突突半小时。
function foo21(a){
    console.log(a) // function a(){}
    var a = 10
    function a(){}
    console.log(a) // 10
}
foo21("fff"); 

```

5.4、再体会函数声明比变量优先级要高，并且定义过程不会被变量覆盖，除非是赋值

```javascript
function foo3(a){
    console.log(a)  // body line 1   // function a(){}
    var a = 10  // body line 2
    function a(){} // body line 3
    console.log(a)  // body line 4   // 输出 10
}
foo3(22, 500)
```

具体步骤详解：

```javascript
// 步骤详解，以下是伪代码
// 步骤1.1，创建arguments，添加形参到VO，将实参赋值给对应的形参
foo3_AO_step1_1 = {
    arguments: {
        0: 22,
        1: 500,
        length: 2
    },
    a: 22,
}
// 步骤1.2，扫描函数声明，添加到VO，若有同名属性，替换掉它的值。发现函数a的声明，替换掉形参的值。这也是为啥函数是一等公民，可以替换其他的
foo3_AO_step1_2 = {
    arguments: {
        0: 22,
        1: 500,
        length: 2
    },
    a: FD, // 指向 function a(){}
}
// 步骤1.3，扫描变量声明，添加到VO，若有同名属性，不做处理，因此这一步还是这样
foo3_AO_step1_3 = {
    arguments: {
        0: 22,
        1: 500,
        length: 2
    },
    a: FD, // 指向 function a(){}
}
// 步骤2开始逐行执行
// 步骤2.1 body line 1， 此时输出的a，也就是AO中的a，是一个函数引用
// 步骤2.2 body line 2，这里有一个赋值语句，因此会替换掉AO中a的值，此时AO中a的值变为10
foo3_AO_step2_1 = {
    arguments: {
        0: 22,
        1: 500,
        length: 2
    },
    a: 10
}
// 步骤2.3 body line 3，这里仅是声明，扫描阶段已经过了，不会添加到AO
// 步骤2.4 body line 4，此时AO中a为10，因此输出10
```

5.5，一个思考题，下面这个代码输出什么？解释一下原因和具体JS引擎的执行步骤

```javascript

function foo32(a){
    var a 
    function a(){}
    console.log(a)
}
foo32(20) 
```

  
三、总结：  
1、EC分为两个阶段，创建执行上下文(有的也叫预编译)和执行代码。  
2、每个EC可以抽象为一个对象，这个对象具有三个属性，分别为：作用域链Scope，VO|AO（AO，VO只能有一个）以及this。  
3、函数EC中的AO在进入函数EC时，确定了arguments对象的属性；在执行函数EC时，其它变量属性具体化。  
4、VO（函数中是AO）创建过程中添加对应属性是有先后顺序的：参数声明 > 函数声明 > 变量声明。  
    4.1，添加函数声明时，其属性名和值都是函数对象创建出来的；如果活动对象已经包含了相同名字的属性，则替换它的值。函数的一等公民特性。  
    4.2，添加变量声明时，其属性名即为变量名，其值为undefined；如果变量名和已经声明的函数名或者函数的参数名相同，则不会影响已经存在的属性。

 

##  
