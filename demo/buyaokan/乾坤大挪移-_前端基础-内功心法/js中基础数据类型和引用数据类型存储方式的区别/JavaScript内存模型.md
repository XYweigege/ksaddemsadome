<font style="color:rgb(51, 51, 51);">对我们程序员来说，声明变量、进行初始化和赋值几乎是每天都在做的一件事情。不过，这些操作本质上做了什么事情呢？JavaScript 是如何在内部对这些进行处理的？更重要的是，了解 JavaScript 的底层细节对我们程序员有什么好处？</font>

<font style="color:rgb(51, 51, 51);">本文的大纲如下：</font>

1. <font style="color:rgb(51, 51, 51);">JS 基本类型的变量声明和赋值</font>
2. <font style="color:rgb(51, 51, 51);">JS 的内存模型：调用栈和堆</font>
3. <font style="color:rgb(51, 51, 51);">JS 引用类型的变量声明和赋值</font>
4. <font style="color:rgb(51, 51, 51);">Let vs const</font>

---

## <font style="color:rgb(0, 0, 0);">JS 基本类型的变量声明和赋值</font>
<font style="color:rgb(51, 51, 51);">我们先从一个简单的例子讲起：声明一个名为 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">muNumber</font><font style="color:rgb(51, 51, 51);"> 的变量，并初始化赋值为 23。</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
let myNumber = 23
```

<font style="color:rgb(51, 51, 51);">当执行这一行代码的时候，JS 将会 ……</font>

1. <font style="color:rgb(51, 51, 51);">为变量创建一个唯一的标识符（ </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myNumber</font><font style="color:rgb(51, 51, 51);"> ）Create a unique identifier for your variable (“myNumber”).</font>
2. <font style="color:rgb(51, 51, 51);">在栈内存中分配一块空间（将在运行时完成分配）</font>
3. <font style="color:rgb(51, 51, 51);">将值 23 保存在这个分配出去的空间中</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412268478-7100884d-ed25-40e8-832d-a06f4aa3c955.jpeg)

<br/>color1
<font style="color:rgb(51, 51, 51);">我们习惯的说法是“</font><font style="color:rgb(10, 191, 91);">myNumber</font><font style="color:rgb(51, 51, 51);"> 等于23”，但更严谨的说法应该是，</font><font style="color:rgb(10, 191, 91);">myNumber</font><font style="color:rgb(51, 51, 51);"> 等于保存着值 23 的那个内存空间的地址。这两者的区别很关键，需要搞清楚。</font>

<br/>

<font style="color:rgb(51, 51, 51);">如果我们创建一个新变量 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">newVar</font><font style="color:rgb(51, 51, 51);"> 并将 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myNumber</font><font style="color:rgb(51, 51, 51);"> 赋值给它 ……</font>

 

```javascript
let newVar = myNumber
```

<font style="color:rgb(51, 51, 51);">…… 由于 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myNumber</font><font style="color:rgb(51, 51, 51);"> 实际上等于内存地址 “0012CCGWH80”，因此这一操作会使得 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">newVar</font><font style="color:rgb(51, 51, 51);"> 也等于 “0012CCGWH80”，也就是等于保存着值 23 的那个内存地址。最终，我们可能会习惯说“</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">newVar</font><font style="color:rgb(51, 51, 51);"> 现在等于 23 了”。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412268717-548c237e-21b7-406b-a8e4-4cc6e65968b0.jpeg)

<font style="color:rgb(51, 51, 51);">那么，如果我这样做会发生什么呢？</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
myNumber = myNumber + 1
```

<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myNumber</font><font style="color:rgb(51, 51, 51);"> 自然会“等于” 24，不过 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">newVar</font><font style="color:rgb(51, 51, 51);"> 和 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myNumber</font><font style="color:rgb(51, 51, 51);"> 指向的可是同一块内存空间啊，</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">newVar</font><font style="color:rgb(51, 51, 51);"> 是否也会“等于” 24 呢？</font>

<font style="color:rgb(51, 51, 51);">并不会。在 JS 中，基本数据类型是</font>**<font style="color:rgb(51, 51, 51);">不可改变的</font>**<font style="color:rgb(51, 51, 51);">，在 “myNumber + 1” 被解析为 “24” 的时候，JS 实际上将会在内存中重新分配一块新的空间用于存放 24 这个值，而 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myNumber</font><font style="color:rgb(51, 51, 51);"> 将会转而指向这个新的内存空间的地址。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412268590-16359ea9-e55f-4e26-9876-a4edaf3d0f51.jpeg)

<font style="color:rgb(51, 51, 51);">再看一个类型的例子：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
let myString = 'abc'  
myString = myString + 'd'
```

<font style="color:rgb(51, 51, 51);">JS 初学者可能会认为，无论字符串 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">abc</font><font style="color:rgb(51, 51, 51);"> 存放在内存的哪个地方，这个操作都会将字符 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">d</font><font style="color:rgb(51, 51, 51);"> 拼接在字符串后面。这种想法是错误的。别忘了，在 JS 中字符串也是基本类型。当 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">abc</font><font style="color:rgb(51, 51, 51);"> 与 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">d</font><font style="color:rgb(51, 51, 51);"> 拼接的时候，在内存中会重新分配一块新的空间用于存放 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">abcd</font><font style="color:rgb(51, 51, 51);"> 这个字符串，而 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myString</font><font style="color:rgb(51, 51, 51);"> 将会转而指向这个新的内存空间的地址（同时，</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">abc</font><font style="color:rgb(51, 51, 51);"> 依然位于原先的内存空间中）。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412268601-5d263731-7bda-4510-991c-d20077b725c0.jpeg)

<font style="color:rgb(51, 51, 51);">接下来我们看一下基本类型的内存分配发生在哪里。</font>

---

## <font style="color:rgb(0, 0, 0);">JS 的内存模型：调用栈和堆</font>
<font style="color:rgb(51, 51, 51);">简单理解，可以认为 JS 的内存模型包含两个不同的区域，一个是调用栈，一个是堆。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412268776-8dddaa37-c130-469e-a405-80d00081bc76.jpeg)

<font style="color:rgb(51, 51, 51);">除了函数调用之外，调用栈同时也用于存放基本类型的数据。以上一小节的代码为例，在声明变量后，调用栈可以粗略表示如下图：</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412269060-500e5d1e-3bae-4f10-b2a0-0bfeadd718ee.jpeg)

<font style="color:rgb(51, 51, 51);">在上面这张图中，我对内存地址进行了抽象，以显示每个变量的值，但请记住，（正如之前所说的）变量始终指向某一块保存着某个值的内存空间。这是理解 let vs const 这一小节的关键。</font>

<font style="color:rgb(51, 51, 51);">再来看一下堆。</font>

<font style="color:rgb(51, 51, 51);">堆是引用类型变量存放的地方。堆相对于栈的一个关键区别就在于，堆可以存放动态增长的无序数据 —— 尤其是数组和对象。</font>

---

## <font style="color:rgb(0, 0, 0);">JS 引用类型的变量声明和赋值</font>
<font style="color:rgb(51, 51, 51);">在变量声明与赋值这方面，引用类型变量与基本类型变量的行为表现有很大的差异。</font>

<font style="color:rgb(51, 51, 51);">我们同样从一个简单的例子讲起。下面声明一个名为 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font><font style="color:rgb(51, 51, 51);"> 的变量并初始化为一个空数组：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
let myArray = \[\]
```

<font style="color:rgb(51, 51, 51);">当你声明一个变量 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font><font style="color:rgb(51, 51, 51);"> 并通过引用类型数据（比如 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">[]</font><font style="color:rgb(51, 51, 51);">）为它赋值的时候，在内存中的操作是这样的：</font>

1. <font style="color:rgb(51, 51, 51);">为变量创建一个唯一的标识符（</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font><font style="color:rgb(51, 51, 51);">）</font>
2. <font style="color:rgb(51, 51, 51);">在堆内存中分配一块空间（将在运行时完成分配）</font>
3. <font style="color:rgb(51, 51, 51);">这个空间存放着此前所赋的值（空数组 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">[]</font><font style="color:rgb(51, 51, 51);">）</font>
4. <font style="color:rgb(51, 51, 51);">在栈内存中分配一块空间</font>
5. <font style="color:rgb(51, 51, 51);">这个空间存放着指向被分配的堆空间的地址</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412269095-10ebc881-0b73-4250-a453-a21352806674.jpeg)

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412269666-866085bf-3f77-4f6f-8ce9-775568a20498.jpeg)

<font style="color:rgb(51, 51, 51);">我们可以对 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font><font style="color:rgb(51, 51, 51);"> 进行各种数组操作：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
myArray.push("first")  
myArray.push("second")  
myArray.push("third")  
myArray.push("fourth")  
myArray.pop()
```

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412269552-5a060b3c-71c5-4245-80f8-dc86703db34d.jpeg)

---

## <font style="color:rgb(0, 0, 0);">Let vs const</font>
<font style="color:rgb(51, 51, 51);">通常来讲，我们应该尽可能多地使用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);">，并且只在确定变量会</font>**<font style="color:rgb(51, 51, 51);">改变</font>**<font style="color:rgb(51, 51, 51);">之后才使用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">let</font><font style="color:rgb(51, 51, 51);">。</font>

<font style="color:rgb(51, 51, 51);">重点来了，注意这里的</font>**<font style="color:rgb(51, 51, 51);">改变</font>**<font style="color:rgb(51, 51, 51);">究竟指的是什么意思。</font>

<font style="color:rgb(51, 51, 51);">很多人会错误地认为，这里的“改变”指的是值的改变，并且可能试图用类似下面的代码进行解释：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
let sum = 0  
sum = 1 + 2 + 3 + 4 + 5

let numbers = []  
numbers.push(1)  
numbers.push(2)  
numbers.push(3)  
numbers.push(4)  
numbers.push(5)
```

<font style="color:rgb(51, 51, 51);">是的，用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">let</font><font style="color:rgb(51, 51, 51);"> 声明 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">sum</font><font style="color:rgb(51, 51, 51);"> 变量是正确的，毕竟 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">sum</font><font style="color:rgb(51, 51, 51);"> 变量的值确实会改变；不过，用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">let</font><font style="color:rgb(51, 51, 51);"> 声明 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">numbers</font><font style="color:rgb(51, 51, 51);"> 是错误的。而错误的根源在于，这些人认为往数组中添加元素是在改变它的值。</font>

**<font style="color:rgb(51, 51, 51);">所谓的“改变”，实际上指的是内存地址的改变</font>**<font style="color:rgb(51, 51, 51);">。</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">let</font><font style="color:rgb(51, 51, 51);"> 声明的变量允许我们修改内存地址，而 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 则不允许。</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
const importantID = 489  
importantID = 100 // TypeError: Assignment to constant variable
```

<font style="color:rgb(51, 51, 51);">我们研究一下这里为什么会报错。</font>

<font style="color:rgb(51, 51, 51);">当声明 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">importantID</font><font style="color:rgb(51, 51, 51);"> 变量之后，某一块内存空间被分配出去，用于存放 489 这个值。牢记我们之前所说的，变量 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">importantID</font><font style="color:rgb(51, 51, 51);"> 从来只等于某一个内存地址。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412269619-0a868e4b-b706-450d-99e2-34ef35f35eb2.jpeg)

<font style="color:rgb(51, 51, 51);">当把 100 赋值给 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">importantID</font><font style="color:rgb(51, 51, 51);"> 的时候，由于 100 是基本类型的值，内存中会分配一块新的空间用于存放 100。之后，JS 试图将这块新空间的地址赋值给 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">importantID</font><font style="color:rgb(51, 51, 51);">，此时就会报错。这其实正是我们期望的结果，因为我们根本就不想对这个非常重要的 ID 进行改动 …….</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412269937-6047a16f-0efb-43d3-84e7-209eab3da862.jpeg)

<font style="color:rgb(51, 51, 51);">这样就说得通了，用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">let</font><font style="color:rgb(51, 51, 51);"> 声明数组是错误的（不合适的），应该用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 才行。这对初学者来说确实比较困惑，毕竟这完全不符合直觉啊！初学者会认为，既然是数组肯定需要有所改动，而 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 声明的常量明明是不可改动的啊，那为何还要用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> ？不过，你必须得记住：所谓的“改变”指的是内存地址的改变。我们再来深入理解一下，为什么在这里使用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 完全没问题，并且绝对是更好的选择。</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
const myArray = []
```

<font style="color:rgb(51, 51, 51);">在声明 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font><font style="color:rgb(51, 51, 51);"> 之后，调用栈会分配一块内存空间，它所存放的值是指向堆中某个被分配内存空间的地址。而堆中的这个空间才是实际上存放空数组的地方。看下面的图理解一下：</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412269917-17cad9af-1b41-4843-88dc-265aab2d0ec9.jpeg)

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412270196-a4239820-9dea-4230-8bc7-f6e66db6ed29.jpeg)

<font style="color:rgb(51, 51, 51);">如果我们进行这些操作：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
myArray.push(1)  
myArray.push(2)  
myArray.push(3)  
myArray.push(4)  
myArray.push(5)
```

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412270116-8ccf3322-ded6-4bed-b630-804aa2c1b6fd.jpeg)

<font style="color:rgb(51, 51, 51);">这将会往堆中的数组添加元素。不过，</font>**<font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font>****<font style="color:rgb(51, 51, 51);"> 的内存地址可是至始至终都没改变的</font>**<font style="color:rgb(51, 51, 51);">。这也就解释了为什么 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font><font style="color:rgb(51, 51, 51);"> 是用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 声明的，但是对它（数组）的修改却不会报错。因为，</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font><font style="color:rgb(51, 51, 51);"> 始终等于内存地址 “0458AFCZX91”，该地址指向的空间存放着另一个内存地址 “22VVCX011”，而这第二个地址指向的空间则真正存放着堆中的数组。</font>

<font style="color:rgb(51, 51, 51);">如果我们这么做，则会报错：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
myArray = 3
```

<font style="color:rgb(51, 51, 51);">因为 3 是基本类型的值，这么做会在内存中分配一块新的空间用于存放 3，同时会修改 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font><font style="color:rgb(51, 51, 51);"> 的值，使其等于这块新空间的地址。而由于 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font><font style="color:rgb(51, 51, 51);"> 是用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 声明的，这样修改就必然会报错。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412270397-62a2a5d7-2ee4-4049-a9f3-44f553313d0c.jpeg)

<font style="color:rgb(51, 51, 51);">下面这样做同样会报错：</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
myArray = ['a']
```

<font style="color:rgb(51, 51, 51);">由于 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">[‘a’]</font><font style="color:rgb(51, 51, 51);"> 是一个新的引用类型的数组，因此在栈中会分配一块新的空间来存放堆中的某个空间地址，堆中这块空间则用于存放</font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">[‘a’]</font><font style="color:rgb(51, 51, 51);"> 。之后我们试图把新的内存地址赋值给 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">myArray</font><font style="color:rgb(51, 51, 51);">，这样显然也是会报错的。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1719412270522-94b5cc9c-12da-4a58-ba63-cf2e5d6e9076.jpeg)

<font style="color:rgb(51, 51, 51);">对于用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 声明的对象，它和数组的表现也是一样的。因为对象也是引用类型的数据，可以添加键，更新值，诸如此类。</font>

**<font style="color:rgb(0, 0, 0);">代码语言：</font>**<font style="color:rgb(0, 0, 0);">javascript</font>

**<font style="color:rgb(0, 0, 0);">复制</font>**

```javascript
const myObj = {}  
myObj['newKey'] = 'someValue' // this will not throw an error
```

## **<font style="color:rgb(0, 0, 0);">知道这些有什么用？</font>**
<font style="color:rgb(0, 82, 217);">GitHub</font><font style="color:rgb(51, 51, 51);"> 和 Stack Overflow 年度开发者调查报告) 的相关数据显示，JavaScript 是排名第一的语言。精通这门语言并成为一名“JS 大师”可能是我们梦寐以求的。在任何一门像样的 JS 课程或者一本书中，都会倡导我们多使用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 和 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">let</font><font style="color:rgb(51, 51, 51);">，少使用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">var</font><font style="color:rgb(51, 51, 51);">，但他们基本上都没有解释这其中的缘由。很多初学者会疑惑为什么有些用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 声明的变量在“修改”的时候确实会报错，而有些变量却不会。我能够理解，正是这种反直觉的体验让他们更喜欢随处都使用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">let</font><font style="color:rgb(51, 51, 51);">，毕竟谁也不想踩坑嘛。</font>

<font style="color:rgb(51, 51, 51);">不过，这并不是我们推荐的方式。Google 作为一家拥有顶尖程序员的公司，它的 JavaScript 风格指南中就有这么一段话：用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 或者 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">let</font><font style="color:rgb(51, 51, 51);"> 声明所有的局部变量。除非一个变量有重新赋值的需要，否则默认使用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 进行声明。绝不允许使用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">var</font><font style="color:rgb(51, 51, 51);">关键字 (来源)。</font>

<font style="color:rgb(51, 51, 51);">虽然他们没有指出个中缘由，不过我认为有下面这些理由：</font>

1. <font style="color:rgb(51, 51, 51);">预先避免将来可能产生的 bug</font>
2. <font style="color:rgb(51, 51, 51);">用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 声明的变量在声明的时候就必须进行初始化，这会引导开发者关注这些变量在作用域中的表现，最终有助于促进更好的</font><font style="color:rgb(0, 82, 217);">内存管理</font><font style="color:rgb(51, 51, 51);">与性能表现。</font>
3. <font style="color:rgb(51, 51, 51);">带来更好的可读性，任何接管代码的人都能知道，哪些变量是不可修改的（就 JS 而言），哪些变量是可以重新赋值的。</font>

<font style="color:rgb(51, 51, 51);">希望本文能够帮助你理解使用 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">const</font><font style="color:rgb(51, 51, 51);"> 或者 </font><font style="color:rgb(10, 191, 91);background-color:rgb(243, 245, 249);">let</font><font style="color:rgb(51, 51, 51);"> 声明变量的个中缘由以及应用场景。</font>

