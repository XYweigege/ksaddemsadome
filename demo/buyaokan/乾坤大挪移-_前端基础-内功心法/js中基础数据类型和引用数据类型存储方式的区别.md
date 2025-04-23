#### **<font style="color:rgb(0, 0, 0);">基本类型和引用类型</font>**
<font style="color:rgb(51, 51, 51);">JavaScript 数据类型目前是有 8 种，在大的方向可以分为两种，一种是基本类型，另外一种是引用类型。</font>

1. **<font style="color:rgb(51, 51, 51);">基本类型</font>**<font style="color:rgb(51, 51, 51);"> 基本类型也称为原始数据类型，基本数据类型有 7 种，number、string、boolean、null、undefined，symbol(ES6)，bigint(ES10)</font>
2. **<font style="color:rgb(51, 51, 51);">引用类型</font>**<font style="color:rgb(51, 51, 51);"> 引用类型统称为 object 类型，细分的话有：Object 类型、Array 类型、Date 类型、RegExp 类型、Function 类型 等。</font>

#### **<font style="color:rgb(0, 0, 0);">变量的内存分配</font>**
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1719281627285-27a8b5f0-4313-4dfb-aef5-1b35377969a6.png)




<font style="color:rgb(77, 77, 77);">简单类型又叫做基本数据类型或者值类型，复杂类型又叫做引用类型</font>

<font style="color:#DF2A3F;">基础数据类型：在存储时变量中存储的是值本身  
</font><font style="color:#DF2A3F;">引用数据类型：存储时变量中存储的是地址</font>

<br/>




<font style="color:rgb(77, 77, 77);">内存中有堆和栈的区别：</font>  
<font style="color:rgb(77, 77, 77);">堆：存储复杂类型(对象)，一般由程序员分配释放，若程序员不释放，由</font>**<font style="color:#DF2A3F;">垃圾回收</font>**<font style="color:rgb(77, 77, 77);">机制回收；</font>  
<font style="color:rgb(77, 77, 77);">栈：由操作系统自动分配释放存放函数的参数值、局部变量的值等。其操作方式类似于</font>**<font style="color:#DF2A3F;">数据结构</font>**<font style="color:rgb(77, 77, 77);">中</font>  
<font style="color:rgb(77, 77, 77);">的栈。</font>

<br/>




<font style="color:rgb(77, 77, 77);">堆和栈的特点：</font>  
<font style="color:rgb(77, 77, 77);">栈：空间小，读写速度快；</font>  
<font style="color:rgb(77, 77, 77);">堆：空间大，读写速度慢；</font>

<br/>



1. **<font style="color:rgb(51, 51, 51);">基本类型</font>**<font style="color:rgb(51, 51, 51);"> 基本数据类型变量保存在栈（stack）中,它们的值直接存储在变量访问的位置。这是因为这些原始类型占据的空间是固定的，所以可将它们存储在较小的内存区域 – 栈中。这样存储便于迅速查寻变量的值。</font>
2. **<font style="color:rgb(51, 51, 51);">引用类型</font>**<font style="color:rgb(51, 51, 51);"> javascript 的引用数据类型是同时保存在栈内存和堆内存中的对象。与其它语言的不同是，你不可以直接访问堆内存空间中的位置和操作堆内存空间。只能操作对象在栈内存中的引用地址。准确地说，引用类型的存储需要内存的栈区和堆区（堆区是指内存里的堆内存）共同完成，栈区内存保存变量标识符和指向堆内存中该对象的指针，也可以说是该对象在堆内存的地址。由于引用值的大小会改变，所以不能把它放在栈中，否则会降低变量查寻的速度。</font>



<font style="color:rgb(77, 77, 77);">基础数据类型的值占据的空间都不大，因此是直接存储在栈中，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1719281321425-fbdf585c-37ac-411f-bd33-e101546440e7.png)


<font style="color:rgb(77, 77, 77);">当把变量a赋值给变量b时，会直接把变量a的值复制一份，然后赋值给变量b，这时改变变量a或者变量b的值，不会改变另外一个变量的值。</font>

<br/>



<font style="color:rgb(77, 77, 77);">引用数据类型的值有可能会占用很大的内存空间，因此会把值存储在堆中，如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1719281363416-11c68341-19f5-48d6-9035-d6281f0ba165.png)


<font style="color:rgb(77, 77, 77);">引用数据类型的变量在栈中存放的是内存地址，如果把变量a赋值给变量b，是把变量a的内存地址复制一份赋值给变量b。改变变量a的值是改变存放在堆里面的数据，而不是内存地址，内存地址是没有变的。</font>

<font style="color:rgb(77, 77, 77);">因此这时改变变量a或者变量b的值就会让另一个变量的值改变。</font>

<br/>

