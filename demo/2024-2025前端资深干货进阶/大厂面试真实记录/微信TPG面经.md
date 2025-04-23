<br/>color1
<font style="color:rgb(36, 41, 47);">从面试经历来看，确实很有深度，八股文考的比较少，更多是深挖简历里做过的事情，以及相关技术点背后的原理、拓展，甚至涉及到了数学。对于想挑战大厂的同学，这篇面经非常有代表性，可以看看大厂的面试风格和难度是怎样的。</font>

<font style="color:rgb(36, 41, 47);">正如一直强调的，对于每一轮面试之后的复盘，也是非常重要的，很多时候下一轮面试都会涉及到上一轮中回答不好的问题，希望同学们引起重视。</font>

<br/>



### 一面
##### <font style="color:rgba(0, 0, 0, 0.85);">加载单图片、多图片</font>
<font style="color:rgb(36, 41, 47);">这个问题可以拆解成两个部分：</font>

+ <font style="color:rgb(36, 41, 47);">包装</font><font style="color:rgb(36, 41, 47);"> </font><font style="color:rgb(239, 112, 96);">Promise</font><font style="color:rgb(36, 41, 47);">：对于单图片直接使用</font><font style="color:rgb(36, 41, 47);"> </font><font style="color:rgb(239, 112, 96);">promise.then</font><font style="color:rgb(36, 41, 47);"> </font><font style="color:rgb(36, 41, 47);">就可以，多图片需要参考</font><font style="color:rgb(36, 41, 47);"> </font><font style="color:rgb(239, 112, 96);">promise.all</font><font style="color:rgb(36, 41, 47);"> </font><font style="color:rgb(36, 41, 47);">的实现。</font>
+ <font style="color:rgb(36, 41, 47);">图片加载：这一步没有写出来，面试官跟我说这一步很多人都想不到，但其实非常简单。</font>

<font style="color:rgb(36, 41, 47);">有多简单？</font><font style="color:rgb(239, 112, 96);">new Image(src)</font><font style="color:rgb(36, 41, 47);"> 就可以了</font>

<font style="color:rgb(36, 41, 47);">紧接着就基于这两个功能进行了扩展探究：</font>

+ <font style="color:rgb(36, 41, 47);">加载单图片作为开放能力提供给开发者，需要注意什么问题？</font>
    - <font style="color:rgb(36, 41, 47);">兜底操作，对 src 进行解析，保证图片正确</font>
    - <font style="color:rgb(36, 41, 47);">缓存</font>
    - <font style="color:rgb(36, 41, 47);">透传相关属性，实现定制化</font>
+ <font style="color:rgb(36, 41, 47);">多图片呢？</font>
    - <font style="color:rgb(36, 41, 47);">兜底操作，某一图片失败时提供信息</font>
    - <font style="color:rgb(36, 41, 47);">某一图片加载完后，优先展示</font>
    - <font style="color:rgb(36, 41, 47);">并发任务控制</font>
+ <font style="color:rgb(36, 41, 47);">如果一个庞大的架构体系改变一个小功能，如何保证整体逻辑不会崩？</font>
    - <font style="color:rgb(36, 41, 47);">确认逻辑的大概改动范围，进行单例测试</font>

##### <font style="color:rgba(0, 0, 0, 0.85);">LRU 算法</font>
<font style="color:rgb(36, 41, 47);">LRU 即为</font>**<font style="color:rgb(36, 41, 47);">最近最少使用缓存</font>**

<font style="color:rgb(36, 41, 47);">由于我是用 map 实现的，面试官认为我是卡了一个 map 的 bug，让我想想有没有别的方式，实现 LRU 的核心是属性有序，所以我说可以用链表实现，由于用链表实现太废时了，就没让我写</font>

<font style="color:rgb(36, 41, 47);">追问：map 为什么是有序，为什么查找的时间时间复杂度可以到 O(1)？  
</font><font style="color:rgb(36, 41, 47);">答：给我干碎了，真没了解过，具体的原因我会在后面总结一篇文章出来。</font>

##### <font style="color:rgb(36, 41, 47);">双链表升序合并 「设立哨兵连接指针即可」</font>
##### <font style="color:rgba(0, 0, 0, 0.85);">飞书妙记做的是什么？业务实现？为什么我能在实习就做工程化？</font>
<font style="color:rgb(36, 41, 47);">至于我为什么能在实习就做工程化？一切都因为妙记本身的前端业务太复杂了，一开始不好上手，所以 mentor 想我从工程化入手，结果就一发不可收拾地往 owner 整个团队工程化的方向发展了。</font>

<font style="color:rgb(36, 41, 47);"></font>

##### <font style="color:rgba(0, 0, 0, 0.85);">babel-runtime 怎么实现的，具体细节，原理？</font>
<font style="color:rgb(36, 41, 47);">涉及到性能优化部分， </font>[babel-runtime 如何缩小打包体积](https://www.yuque.com/sohucw/hzbvu7/bgwh9m2le7m8oana)

<font style="color:rgb(36, 41, 47);">基于这个问题，我的面试官往往喜欢会发散出去问我首屏性能优化、构建速度优化的一系列问题，往往都是加分点！</font>

<font style="color:rgb(36, 41, 47);"></font>

##### <font style="color:rgb(36, 41, 47);">fp、ftp 这些数据怎么记录的</font>


##### <font style="color:rgba(0, 0, 0, 0.85);">如何提高本地构建速度的？</font>
+ <font style="color:rgb(36, 41, 47);">基于插件、loader 的特性和环境变量，进行合理编排。</font>

<font style="color:rgb(36, 41, 47);">其实这方面的速度，还涉及到 </font><font style="color:rgb(239, 112, 96);">并行任务控制</font><font style="color:rgb(36, 41, 47);">、</font><font style="color:rgb(239, 112, 96);">缓存</font><font style="color:rgb(36, 41, 47);">，和速度相关的这两个点基本跑不到</font>

<font style="color:rgb(36, 41, 47);"></font>

##### <font style="color:rgba(0, 0, 0, 0.85);">包依赖管理怎么做的</font>
[说说包管理工具的发展以及 pnpm 依赖治理的最佳实践](https://www.yuque.com/sohucw/hzbvu7/po3xgfo83vv02tfg)

##### <font style="color:rgba(0, 0, 0, 0.85);">monorepo 源码引用 sdk 的 alias 内容怎么做的</font>


### 二面
##### <font style="color:rgba(0, 0, 0, 0.85);">介绍一个项目 开源，架构设计？解决痛点？</font>
<font style="color:rgb(36, 41, 47);">后面的面试基本都会围绕开源或创业展开问题，对于开源，当然是最近在做的一个脚手架项目啦，感兴趣的可以参考：</font><font style="color:rgb(36, 41, 47);">🎉🎉🎉</font><font style="color:rgb(36, 41, 47);"> 重构三个月，我们设计出了一个轻量且非常灵活的前端脚手架架构</font>

<font style="color:rgb(36, 41, 47);">对于开源，他们就是会比较在意这个项目的背景是什么，解决了什么痛点，具体的技术实现反而是次要的，如果介绍过程可以让面试官对这个产品心动，那么就会非常加分。</font>

##### <font style="color:rgba(0, 0, 0, 0.85);">上个面试官问你 map 的查找为什么 O1，回去有了解吗？</font>
<font style="color:rgb(36, 41, 47);">好问！回去后狠狠学习了一下，基本内容全都说出来了，但这个面试官又开始了深挖……</font>

+ <font style="color:rgb(36, 41, 47);">hashMap 实现一个数组加链表的结构，数组大小怎么设置？固定还是用户设置还是动态变化？什么情况触发扩容？</font>
+ <font style="color:rgb(36, 41, 47);">map 最坏查找情况是怎样的？红黑树实现 hashMap 的话缺点在哪里？</font>
+ <font style="color:rgb(36, 41, 47);">map 过大时，扩容怎么做，新创立空间的话很卡，怎么优化？</font>

##### <font style="color:rgba(0, 0, 0, 0.85);">写题：升序数组 [2,3,4,5],插入一个数字，返回应该插入的位置</font>
+ <font style="color:rgb(36, 41, 47);">怎么优化（二分查找），考虑二分算 midIndex 时超过整数最大上限怎么处理</font>
+ <font style="color:rgb(36, 41, 47);">如果让你写测试数据会写什么（重复元素），如果重复，插入哪里更合适（最后面的，开销最小）</font>
+ <font style="color:rgb(36, 41, 47);">你开源项目怎么做的测试？ 「好问，一个一个测！」</font>

##### <font style="color:rgb(36, 41, 47);">async/await 降到 es5 做了什么转化，给了一段代码让我写出转化的结构。  
</font><font style="color:rgb(36, 41, 47);">font-size 的 px 是基于什么而定的（屏幕像素），是决定了字体的长宽还是什么？（决定我寄了  
</font><font style="color:rgb(36, 41, 47);">http 1.x、2、3 的区别，UDP、TCP 的区别？  
</font><font style="color:rgb(36, 41, 47);">js 怎么发生的内存泄露  
</font><font style="color:rgb(36, 41, 47);">聊聊安全，问了 xss、csrf、sql 注入的实现原理？场景？</font>




##### 三面
##### <font style="color:rgb(36, 41, 47);">1.自我介绍  
</font><font style="color:rgb(36, 41, 47);">2.讲一下创业？规模？形式和方向？我做的什么东西？用户量？  
</font><font style="color:rgb(36, 41, 47);">3.创业的项目中遇到什么有挑战的事情（技术方面）？在图片处理方面的流程设计，做过什么性能优化吗？  
</font><font style="color:rgb(36, 41, 47);">4.飞书做的工作介绍</font>
+ <font style="color:rgb(36, 41, 47);">构建速度怎么优化的？</font>
+ <font style="color:rgb(36, 41, 47);">webpack 插件怎么做的？</font>

##### <font style="color:rgba(0, 0, 0, 0.85);">设计一个混淆压缩怎么做？Tree-shaking 怎么去除未引用代码？</font>
###### <font style="color:rgb(36, 41, 47);">要设计一个混淆压缩方案，通常需要结合多种技术来保证代码的安全性和性能，下面是我提出的方案：</font>
1. **<font style="color:rgb(36, 41, 47);">代码压缩</font>**<font style="color:rgb(36, 41, 47);">：通过删除空格、注释、不必要的字符等来减小代码体积，提高加载速度。</font>
2. **<font style="color:rgb(36, 41, 47);">变量重命名</font>**<font style="color:rgb(36, 41, 47);">：将变量名、函数名等重命名为更短、无意义的名称，使代码难以理解，增加逆向工程的难度。</font>
3. **<font style="color:rgb(36, 41, 47);">代码混淆</font>**<font style="color:rgb(36, 41, 47);">：使用各种技术对代码进行混淆，如将代码转换为一系列的计算表达式、条件语句等，使代码难以阅读和理解。</font>
4. **<font style="color:rgb(36, 41, 47);">Tree-shaking</font>**<font style="color:rgb(36, 41, 47);">：在打包过程中去除未被使用的代码，以减少最终构建出的文件大小。</font>

<font style="color:rgb(36, 41, 47);">接着面试官追问了 Tree-shaking 的细节，首先肯定要是实现了 ESM 的模块化，接着会有如下操作：</font>

1. **<font style="color:rgb(36, 41, 47);">静态分析</font>**<font style="color:rgb(36, 41, 47);">：在代码编译阶段，对整个应用程序进行静态分析，确定模块之间的依赖关系。</font>
2. **<font style="color:rgb(36, 41, 47);">标记未被引用的代码</font>**<font style="color:rgb(36, 41, 47);">：通过静态分析，标记出哪些代码被其他模块所引用，而哪些代码没有被引用。</font>
3. **<font style="color:rgb(36, 41, 47);">剔除未被引用的代码</font>**<font style="color:rgb(36, 41, 47);">：标记完成后，将那些未被引用的代码从最终的构建输出中“摇晃”出去，不包含在生成的文件中。</font>
4. **<font style="color:rgb(36, 41, 47);">优化输出</font>**<font style="color:rgb(36, 41, 47);">：去除未被引用的代码后，对最终的输出文件进行优化，确保删除了所有未使用的部分。</font>

##### <font style="color:rgba(0, 0, 0, 0.85);">AST 怎么比较两端混淆的代码有抄袭情况？AST 里面的函数怎么转换去对比？</font>
1. **<font style="color:rgb(36, 41, 47);">提取关键信息</font>**<font style="color:rgb(36, 41, 47);">：从生成的 AST 中提取关键信息，例如函数、类、变量声明等，用于判断两段代码的相似性。</font>
2. **<font style="color:rgb(36, 41, 47);">指纹技术</font>**<font style="color:rgb(36, 41, 47);">：对代码进行哈希或其他指纹化处理，然后比较生成的指纹，以判断两段代码之间的相似性。</font>

##### <font style="color:rgb(36, 41, 47);">Webpack 增量打包怎么做到的？「方案还挺多的，基本就是缓存以及相关的增量编译的插件、工具」</font>
##### <font style="color:rgb(36, 41, 47);">再介绍一下飞书中的一些贡献点，pnpm + monorepo 包依赖治理</font>
<font style="color:rgb(36, 41, 47);"></font>

<font style="color:rgb(36, 41, 47);"></font>

##### <font style="color:rgba(0, 0, 0, 0.85);">做过游戏相关的事情吗？俄罗斯方块的物理碰撞用哪个物理引擎实现的？</font>
<font style="color:rgb(36, 41, 47);"></font>

<font style="color:rgb(36, 41, 47);">这个部门是游戏 + 前端方向的，主要提供微信小游戏开发者一个开发平台，有实现一些如 unity 转前端的基建，结果面试官就来问我游戏了？然而是真的不懂呀，但这个应该无伤大雅。</font>

##### <font style="color:rgb(36, 41, 47);">跨栈这边，flutter 这些了解过没「没做过，但聊了聊对跨栈发展趋势的认可」</font>


##### <font style="color:rgba(0, 0, 0, 0.85);">你怎么保证短时间内把跨栈能力提升上来？你当时工程化啥也不会咋做的工程化？</font>
###### <font style="color:rgb(36, 41, 47);">当时是这么说的：既然在飞书可以从小白快速上手工程化，在这边我相信依然可以。  
</font><font style="color:rgb(36, 41, 47);">这时候面试官又好奇为什么会做工程化，这对实习生来说确实是一个比较稀罕的事情，几场面试下来经常被问到这个。</font>
<font style="color:rgb(36, 41, 47);">12.问我这边工资多少？我问了微信，差不了太多。让我可以和 hr 沟通。  
</font><font style="color:rgb(36, 41, 47);">13.反问：</font>

+ <font style="color:rgb(36, 41, 47);">游戏 + 前端 的发展方向怎么看待的</font>
+ <font style="color:rgb(36, 41, 47);">小游戏发展现状和未来趋势</font>
+ <font style="color:rgb(36, 41, 47);">实习生培养模式</font>
+ <font style="color:rgb(36, 41, 47);">有什么可以提升的地方：一些算法之类的底层还需要花时间再深入一些</font>

##### <font style="color:rgb(36, 41, 47);">  
</font><font style="color:rgb(36, 41, 47);"> </font>
##### <font style="color:rgb(36, 41, 47);">  
</font><font style="color:rgb(36, 41, 47);"> </font>
