## 销毁局部变量和全局变量
首先局部变量和全局变量的清除有什么不同呢?

**1. 局部变量的销毁**

对于局部变量, 由于它们是存在于函数中的, 那么当这个函数执行完了之后, 它里面的变量会被<font style="background-color:rgb(249, 242, 244);">GC</font>(垃圾收集)掉吗🤔️?

很多教材中说的是:

<font style="background-color:rgb(255, 249, 249);">垃圾收集器很容易做出判断并回收.</font>

确实, 这里还真不是全部被清理掉, 还是得看情况.

比如闭包中的变量并不会随着函数的执行完毕而被清除掉，反而会一直保留着，除非这个闭包被清除-也就是闭包中涉及的变量再也没有被别的函数引用到.

**2. 全局变量的销毁**

全局变量所存在的作用域太过广泛了, 什么时候需要自动释放内存空间就很难判断. 具体要不要回收还是得看后面的**垃圾回收机制**, 所以才说要避免使用全局变量.

---

## [](https://www.123fe.net/blog-docs/javaScript2/17-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E6%9C%BA%E5%88%B6.html#v8%E5%BC%95%E6%93%8E%E7%9A%84%E5%86%85%E5%AD%98%E6%9C%BA%E5%88%B6)V8引擎的内存机制
首先大家要知道一点, 我们常说的引擎, 它在使用的时候其实是会使用系统的内存的.

对于像<font style="background-color:rgb(249, 242, 244);">Java/Go</font>这样的后端语言, 在使用内存的时候是没有什么限制的.

但是对于我们<font style="background-color:rgb(249, 242, 244);">V8</font>引擎来说(应该都知道<font style="background-color:rgb(249, 242, 244);">V8</font>引擎是一种<font style="background-color:rgb(249, 242, 244);">JS</font>引擎的实现), 它只能使用系统的一部分内存.

查看了一下资料:

+ 64位系统下能使用约<font style="background-color:rgb(249, 242, 244);">1.4GB</font>;
+ 32位系统下能使用约<font style="background-color:rgb(249, 242, 244);">0.7GB</font>.

在我们前端看来好像已经很多了, 够用了, 但是别忘了<font style="background-color:rgb(249, 242, 244);">node.js</font>这位“后端大哥”.

想想要是它遇到了一个很大的文件, 比如<font style="background-color:rgb(249, 242, 244);">2G</font>的文件, 那么它就无法将其全部读入内存且进行其他的操作.

再来想想我们<font style="background-color:rgb(249, 242, 244);">JS</font>中的存储, 分为栈存储和堆存储.

1. 对于栈内存, 当<font style="background-color:rgb(249, 242, 244);">ESP</font>指针(你只需要知道它是栈指针)下移，也就是上下文切换之后，栈顶的空间会自动被回收.
2. 而对象的存储是通过堆来进行分配的, 当在构建一个对象且进行赋值操作的时候, <font style="background-color:rgb(249, 242, 244);">JS</font>会将相应的内存分配到堆上. 所以每创建一个对象之后, 堆就会大一点.

那么前面我们也说了, <font style="background-color:rgb(249, 242, 244);">V8</font>引擎只能使用系统的一部分内存, 你的堆可能会不停的增大, 直到大小达到了<font style="background-color:rgb(249, 242, 244);">V8</font>引擎的内存上限为止.

可是<font style="background-color:rgb(249, 242, 244);">V8</font>引擎为什么要给它设置一个内存的上限呢? 如果没有上限或者上限很大, 那么不是能够干更多的事啦🤔️?

其实这个还真不怪<font style="background-color:rgb(249, 242, 244);">V8</font>, 主要原因是两个大家都经常听到的词:

+ <font style="background-color:rgb(249, 242, 244);">JS</font>的单线程执行机制
+ <font style="background-color:rgb(249, 242, 244);">JS</font>垃圾回收机制的限制

为什么说这两个是限制内存上限的原因呢🤔️?

在<font style="background-color:rgb(249, 242, 244);">JS</font>中, 由于它是单线程运行的, 也就是一次只能做一件事, 那么意味着一旦进入了垃圾回收阶段, 其它的运行逻辑都得暂停了, 得等它过了这个阶段才继续执行.

但是好巧不巧的是, 垃圾回收是一件非常耗时的事情, 以 <font style="background-color:rgb(249, 242, 244);">1.5GB</font> 的垃圾回收堆内存为例，<font style="background-color:rgb(249, 242, 244);">V8</font> 做一次小的垃圾回收需要50ms 以上，做一次非增量式的垃圾回收甚至要 1s 以上.

所以若是垃圾回收时常过久的话, <font style="background-color:rgb(249, 242, 244);">JS</font>代码会一直没有响应, 造成了应用卡顿, 其中的坏处我就不用多说了吧.

就这样, <font style="background-color:rgb(249, 242, 244);">V8</font>干脆给它限制了堆内存大小, 这样就算你到顶了也不会说太卡, 而且其实大部分情况也不会说有操作几个<font style="background-color:rgb(249, 242, 244);">G</font>的情况, 因此这也是<font style="background-color:rgb(249, 242, 244);">V8</font>的一种权衡.

这个限制是不可修改的吗🤔️?

并不是的, 你可以通过执行以下命令来修改它:

```plain
// 这是调整老生代这部分的内存，单位是MB。后面会详细介绍新生代和老生代内存
node --max-old-space-size=2048 xxx.js 

// 这是调整新生代这部分的内存，单位是 KB。
node --max-new-space-size=2048 xxx.js
```

之前我在用<font style="background-color:rgb(249, 242, 244);">Angular</font>打包项目的时候就遇到过频繁报内存溢出：<font style="background-color:rgb(249, 242, 244);">FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed - process out of memory</font>。并且打包速度相当慢，估计项目过大了.

解决办法就是:

定义在项目根目录<font style="background-color:rgb(249, 242, 244);">node_modeles</font>文件夹下的<font style="background-color:rgb(249, 242, 244);">.bin</font>目录里面，<font style="background-color:rgb(249, 242, 244);">.bin</font>目录下我们能找到一个叫<font style="background-color:rgb(249, 242, 244);">ng</font>的文件，在该文件的首行写上<font style="background-color:rgb(249, 242, 244);">!/usr/bin/env node --max_old_space_size=4096</font>，这样也就可以解除<font style="background-color:rgb(249, 242, 244);">v8</font>对<font style="background-color:rgb(249, 242, 244);">node</font>的内存使用限制了。

当然如果你是用<font style="background-color:rgb(249, 242, 244);">vue</font>和<font style="background-color:rgb(249, 242, 244);">react</font>开发的话就没这么麻烦了, 具体可以看这篇文章:

[《nodejs 前端项目编译时内存溢出问题的原因及解决方案》](https://blog.csdn.net/qq_35624642/article/details/81084331)

---

## [](https://www.123fe.net/blog-docs/javaScript2/17-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E6%9C%BA%E5%88%B6.html#%E5%A0%86%E5%86%85%E5%AD%98%E7%9A%84%E5%88%86%E4%BB%A3%E7%AE%A1%E7%90%86)堆内存的分代管理
<font style="background-color:rgb(249, 242, 244);">V8</font>引擎对堆内存中的<font style="background-color:rgb(249, 242, 244);">JS</font>对象进行了分代管理, 也就是分为 **新生代** 和 **老生代**.

首先让我们来了解以下几个知识点:

+ **新生代** 就是临时分配的内存，存活时间短, 如临时变量、字符串等;
+ **老生代** 是常驻内存，存活的时间长, 如主控制器、服务器对象等;
+ <font style="background-color:rgb(249, 242, 244);">V8</font>的堆内存, 就是两个内存之和.

就像下面的这张图一样:

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959356222-5b1e749f-500c-4915-ba20-993dcf905f70.png)

### [](https://www.123fe.net/blog-docs/javaScript2/17-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E6%9C%BA%E5%88%B6.html#%E6%96%B0%E7%94%9F%E4%BB%A3%E5%86%85%E5%AD%98%E7%9A%84%E5%9B%9E%E6%94%B6)新生代内存的回收
其实也像图里画的一样, 新生代的默认内存限制很小:

+ 64位系统下为<font style="background-color:rgb(249, 242, 244);">32MB</font>;
+ 32位系统下为<font style="background-color:rgb(249, 242, 244);">16MB</font>.

确实是够小的啦, 主要原因是新生代中的变量存活时间短，来了马上就走，不容易产生太大的内存负担，因此可以将它设的足够小.

#### [](https://www.123fe.net/blog-docs/javaScript2/17-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E6%9C%BA%E5%88%B6.html#%E6%96%B0%E7%94%9F%E4%BB%A3%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84)新生代内存结构
新生代内存会被分为两个部分:

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959356243-7b6451b6-04a8-405d-918b-46400df95c80.png)

一块叫做<font style="background-color:rgb(249, 242, 244);">From</font>, 另一块叫做<font style="background-color:rgb(249, 242, 244);">To</font>. (别的教材中是这么命名的, 后来我去找寻原因, 发现大概是因为在[V8的源码-内存管理](https://github.com/tsy77/blog/issues/13)中有<font style="background-color:rgb(249, 242, 244);">from_space_</font>和<font style="background-color:rgb(249, 242, 244);">to_space_</font>这两个东西吧)

+ <font style="background-color:rgb(249, 242, 244);">From</font>表示正在使用的内存;
+ <font style="background-color:rgb(249, 242, 244);">To</font>表示目前闲置的内存.

#### [](https://www.123fe.net/blog-docs/javaScript2/17-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E6%9C%BA%E5%88%B6.html#scavenge%E7%AE%97%E6%B3%95)Scavenge算法
上面已经介绍了**新生代内存的结构**, 下面来说说它具体是如何进行垃圾回收的.

当进行垃圾回收的时候, 会经过以下几个步骤:

1. <font style="background-color:rgb(249, 242, 244);">V8</font>将<font style="background-color:rgb(249, 242, 244);">From</font>部分的对象全部检查一遍;
2. 检查出若是 **存活对象** 则复制到<font style="background-color:rgb(249, 242, 244);">To</font>内存中, 若不是则直接回收;
3. 复制到<font style="background-color:rgb(249, 242, 244);">To</font>内存中是按照顺序从头放置的;
4. 当<font style="background-color:rgb(249, 242, 244);">From</font>中所有的存活对象全部复制完毕之后, <font style="background-color:rgb(249, 242, 244);">From</font>和<font style="background-color:rgb(249, 242, 244);">To</font>就会 **对调** , 也就是<font style="background-color:rgb(249, 242, 244);">From</font>被闲置, <font style="background-color:rgb(249, 242, 244);">To</font>在使用;
5. 如此循环.

_一张图方便你理解__🤔__:_

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959356192-e88a2cd9-b875-42ec-9298-000e5679496c.png)

不就是个清理垃圾的动作吗? 为什么<font style="background-color:rgb(249, 242, 244);">V8</font>要整的这么复杂啊, 又是遍历又是复制的.

而且为什么还要在<font style="background-color:rgb(249, 242, 244);">To</font>内存中按照顺序从头放置呢🤔️?

其实, 它这样做是有一定好处的, 首先让我们来看看下面这张图:

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959356204-e95b0ba4-ec59-456c-a031-7b7d018985a9.png)

在上图中, 黄色的部分是**待分配的内存**, 而蓝色的小方块就是**存活对象**.

看起来存活对象非常的散乱, 使得空间变得零零散散, 并且堆内存又是连续分配的, 若是碰到稍微大点的对象的话都没有办法进行空间分配了.

<font style="background-color:rgb(255, 249, 249);">堆包含一个链表来维护已用和空闲的内存块。在堆上新分配（用 new 或者 malloc）内存是从空闲的内存块中找到一些满足要求的合适块。所以可能让人觉得只要有很多不连续的零散的小区域，只要总数达到申请的内存块，就可以分配。</font>

<font style="background-color:rgb(255, 249, 249);">但事实上是不行的，这又让人觉得是不是零散的内存块不能连接成一个大的空间，而必须要一整块连续的内存空间才能申请成功.</font>

(原文链接：https://blog.csdn.net/jin13277480598/article/details/54409543)

而这种零散的空间也有一个名字, 叫做 **内存碎片**.

因此将其按照顺序从头放置也是为了解决 **内存碎片** 的问题, 在一顿复制之后, <font style="background-color:rgb(249, 242, 244);">To</font>内存会被排列的整整齐齐的:

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959358746-4e9e0759-95e2-407d-90be-8f4ac834908f.png)

整顿之后就大大方便了后续连续空间的分配.

上面👆说的这种新生代垃圾回收算法也被叫做 **<font style="background-color:rgb(249, 242, 244);">Scavenge</font>****算法** (<font style="background-color:rgb(249, 242, 244);">scavenge</font>的本意就是回收).

所以这个<font style="background-color:rgb(249, 242, 244);">Scavenge</font>算法不仅仅是将非存活对象给回收了, 还需要对内存空间做整顿.

就像是我们平常打扫房间, 不仅仅是将不要的垃圾清理掉, 还顺便把房间内的东西给放整齐了😊.

### [](https://www.123fe.net/blog-docs/javaScript2/17-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E6%9C%BA%E5%88%B6.html#%E8%80%81%E7%94%9F%E4%BB%A3%E5%86%85%E5%AD%98%E7%9A%84%E5%9B%9E%E6%94%B6)老生代内存的回收
如果**新生代中的变量**经过多次回收之后依然存在的话, 它就会发生“**晋升**”, 被放入**老生代内存中**.

产生晋升的情况:

+ 已经经历过一次<font style="background-color:rgb(249, 242, 244);">Scavenge</font>回收;
+ <font style="background-color:rgb(249, 242, 244);">To(闲置内存)</font>空间的内存不足<font style="background-color:rgb(249, 242, 244);">75%</font>.

通过上面👆的介绍我们已经知道, 老生代内存的空间会比新生代的大了很多, 而且老生代累计的变量空间一般都是很大的.

因此老生代的垃圾回收就不能用<font style="background-color:rgb(249, 242, 244);">Scavenge</font>算法了, 一是会浪费一半的空间, 二对庞大的内存空间进行复制本身就是个“很重的体力活”.

#### [](https://www.123fe.net/blog-docs/javaScript2/17-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E6%9C%BA%E5%88%B6.html#%E6%A0%87%E8%AE%B0%E6%B8%85%E9%99%A4)标记清除
所以对于老生代的垃圾回收干脆粗暴点吧, 采用**标记清除**的方式进行回收.

**标记清除**主要是经过以下几个过程:

1. 遍历堆中的所有对象, 给它们做上标记;
2. 之后对于代码环境中<font style="background-color:rgb(249, 242, 244);">使用的变量</font>和<font style="background-color:rgb(249, 242, 244);">被强引用</font>的变量<font style="background-color:rgb(249, 242, 244);">取消标记</font>(被标记的都是垃圾);
3. 把<font style="background-color:rgb(249, 242, 244);">依然被标记的变量</font>当成垃圾给清除掉, 进行空间的回收;

当然, 和新生代一样, 在清理了之后, 还要整理内存碎片, 当然它的整理办法就是在清理阶段结束后把存活对象全部往一端靠拢.

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959360423-21c234ca-cda3-4efe-bb37-56855d2b111a.png)

所以总的来说, 对于老生代内存的回收主要就是经过:

+ <font style="background-color:rgb(249, 242, 244);">标记清除阶段</font>, 留下存活对象;
+ <font style="background-color:rgb(249, 242, 244);">整理阶段</font>, 把存活对象往一边靠拢.

因此, 对于现在的主流浏览器来说, 只要切断对象与根部的关系, 就可以将对象进行回收.

### [](https://www.123fe.net/blog-docs/javaScript2/17-JavaScript%E8%BF%9B%E9%98%B6_%E5%86%85%E5%AD%98%E6%9C%BA%E5%88%B6.html#%E5%B9%B6%E5%8F%91%E6%A0%87%E8%AE%B0)并发标记
在上面我们已经介绍过了<font style="background-color:rgb(249, 242, 244);">V8</font>在进行垃圾回收的时候, 不可避免地会阻塞业务逻辑的执行, 特别如果是老生代垃圾回收的任务比较繁重的时候, 会很耗时严重影响应用的性能.

为优化解决此问题, <font style="background-color:rgb(249, 242, 244);">V8</font>官方在<font style="background-color:rgb(249, 242, 244);">2018</font>年推出了名为**增量标记**的技术.

总的来说该技术的作用就是<font style="background-color:rgb(249, 242, 244);">将原本一口气完成的标记任务分为了很多小的部分去完成, 每完成一个小任务就停一会, 让js逻辑执行一会, 然后再继续执行下面的部分</font>.

<font style="background-color:rgb(255, 249, 249);">在 GC 扫描和标记活动对象时，它允许 JavaScript 应用程序继续运行</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959360445-b53867df-6266-4b8b-a6b6-fc6dd56c5784.png)

<font style="background-color:rgb(255, 249, 249);">在通过增量标记后, 垃圾回收过程对JS应用的阻塞时间减少到原来了1 / 6, 可以说这优化相当大了啊.</font>

