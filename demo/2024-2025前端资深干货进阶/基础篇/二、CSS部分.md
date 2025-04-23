### <font style="color:rgb(44, 62, 80);">1 css sprite是什么,有什么优缺点</font>
+ <font style="color:rgb(44, 62, 80);">概念：将多个小图片拼接到一个图片中。通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-position</font><font style="color:rgb(44, 62, 80);">和元素尺寸调节需要显示的背景图案。</font>
+ <font style="color:rgb(44, 62, 80);">优点：</font>
    - <font style="color:rgb(44, 62, 80);">减少</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">请求数，极大地提高页面加载速度</font>
    - <font style="color:rgb(44, 62, 80);">增加图片信息重复度，提高压缩比，减少图片大小</font>
    - <font style="color:rgb(44, 62, 80);">更换风格方便，只需在一张或几张图片上修改颜色或样式即可实现</font>
+ <font style="color:rgb(44, 62, 80);">缺点：</font>
    - <font style="color:rgb(44, 62, 80);">图片合并麻烦</font>
    - <font style="color:rgb(44, 62, 80);">维护麻烦，修改一个图片可能需要从新布局整个图片，样式</font>

### [](https://www.123fe.net/docs/base.html#_2-display-none-%E4%B8%8Evisibility-hidden-%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">2</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display: none;</font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility: hidden;</font><font style="color:rgb(44, 62, 80);">的区别</font>
+ <font style="color:rgb(44, 62, 80);">联系：它们都能让元素不可见</font>
+ <font style="color:rgb(44, 62, 80);">区别：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:none</font><font style="color:rgb(44, 62, 80);">;会让元素完全从渲染树中消失，渲染的时候不占据任何空间；</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility: hidden</font><font style="color:rgb(44, 62, 80);">;不会让元素从渲染树消失，渲染师元素继续占据空间，只是内容不可见</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display: none</font><font style="color:rgb(44, 62, 80);">;是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">；visibility: hidden;</font><font style="color:rgb(44, 62, 80);">是继承属性，子孙节点消失由于继承了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hidden</font><font style="color:rgb(44, 62, 80);">，通过设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility: visible;</font><font style="color:rgb(44, 62, 80);">可以让子孙节点显式</font>
    - <font style="color:rgb(44, 62, 80);">修改常规流中元素的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">通常会造成文档重排。修改</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility</font><font style="color:rgb(44, 62, 80);">属性只会造成本元素的重绘。</font>
    - <font style="color:rgb(44, 62, 80);">读屏器不会读取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display: none</font><font style="color:rgb(44, 62, 80);">;元素内容；会读取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility: hidden;</font><font style="color:rgb(44, 62, 80);">元素内容</font>

### [](https://www.123fe.net/docs/base.html#_3-link%E4%B8%8E-import%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font><font style="color:rgb(44, 62, 80);">的区别</font>
1. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML</font><font style="color:rgb(44, 62, 80);">方式，</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font><font style="color:rgb(44, 62, 80);">是CSS方式</font>
2. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);">最大限度支持并行下载，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font><font style="color:rgb(44, 62, 80);">过多嵌套导致串行下载，出现</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FOUC</font><font style="color:rgb(44, 62, 80);">(文档样式短暂失效)</font>
3. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);">可以通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rel="alternate stylesheet"</font><font style="color:rgb(44, 62, 80);">指定候选样式</font>
4. <font style="color:rgb(44, 62, 80);">浏览器对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);">支持早于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font><font style="color:rgb(44, 62, 80);">，可以使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font><font style="color:rgb(44, 62, 80);">对老浏览器隐藏样式</font>
5. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font><font style="color:rgb(44, 62, 80);">必须在样式规则之前，可以在css文件中引用其他文件</font>
6. <font style="color:rgb(44, 62, 80);">总体来说：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);">优于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font>

### [](https://www.123fe.net/docs/base.html#_4-%E4%BB%80%E4%B9%88%E6%98%AFfouc-%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D)<font style="color:rgb(44, 62, 80);">4 什么是FOUC?如何避免</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Flash Of Unstyled Content</font><font style="color:rgb(44, 62, 80);">：用户定义样式表加载之前浏览器使用默认样式显示文档，用户样式加载渲染之后再从新显示文档，造成页面闪烁。</font>
+ **<font style="color:rgb(44, 62, 80);">解决方法</font>**<font style="color:rgb(44, 62, 80);">：把样式表放到文档的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><head></font>

### [](https://www.123fe.net/docs/base.html#_5-%E5%A6%82%E4%BD%95%E5%88%9B%E5%BB%BA%E5%9D%97%E7%BA%A7%E6%A0%BC%E5%BC%8F%E5%8C%96%E4%B8%8A%E4%B8%8B%E6%96%87-block-formatting-context-bfc%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8)<font style="color:rgb(44, 62, 80);">5 如何创建块级格式化上下文(block formatting context),BFC有什么用</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">BFC(Block Formatting Context)，块级格式化上下文，是一个独立的渲染区域，让处于 BFC 内部的元素与外部的元素相互隔离，使内外元素的定位不会相互影响</font>

**<font style="color:rgb(44, 62, 80);">触发条件 (以下任意一条)</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);">的值不为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">none</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">overflow</font><font style="color:rgb(44, 62, 80);">的值不为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visible</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">的值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">table-cell</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tabble-caption</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-block</font><font style="color:rgb(44, 62, 80);">之一</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position</font><font style="color:rgb(44, 62, 80);">的值不为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">static</font><font style="color:rgb(44, 62, 80);">或则</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">releative</font><font style="color:rgb(44, 62, 80);">中的任何一个</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">下,</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Layout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">,可通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">zoom:1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">触发</font>

**<font style="color:rgb(44, 62, 80);">.BFC布局与普通文档流布局区别 普通文档流布局:</font>**

+ <font style="color:rgb(44, 62, 80);">浮动的元素是不会被父级计算高度</font>
+ <font style="color:rgb(44, 62, 80);">非浮动元素会覆盖浮动元素的位置</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">会传递给父级元素</font>
+ <font style="color:rgb(44, 62, 80);">两个相邻元素上下的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">会重叠</font>

**<font style="color:rgb(44, 62, 80);">BFC布局规则:</font>**

+ <font style="color:rgb(44, 62, 80);">浮动的元素会被父级计算高度(父级元素触发了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(44, 62, 80);">非浮动元素不会覆盖浮动元素的位置(非浮动元素触发了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">不会传递给父级(父级触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(44, 62, 80);">属于同一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">的两个相邻元素上下</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">会重叠</font>

**<font style="color:rgb(44, 62, 80);">开发中的应用</font>**

+ <font style="color:rgb(44, 62, 80);">阻止</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">重叠</font>
+ <font style="color:rgb(44, 62, 80);">可以包含浮动元素 —— 清除内部浮动(清除浮动的原理是两个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">都位于同一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">区域之中)</font>
+ <font style="color:rgb(44, 62, 80);">自适应两栏布局</font>
+ <font style="color:rgb(44, 62, 80);">可以阻止元素被浮动元素覆盖</font>

### [](https://www.123fe.net/docs/base.html#_6-display%E3%80%81float%E3%80%81position%E7%9A%84%E5%85%B3%E7%B3%BB)<font style="color:rgb(44, 62, 80);">6 display、float、position的关系</font>
+ <font style="color:rgb(44, 62, 80);">如果</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">取值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">none</font><font style="color:rgb(44, 62, 80);">，那么</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);">都不起作用，这种情况下元素不产生框</font>
+ <font style="color:rgb(44, 62, 80);">否则，如果</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position</font><font style="color:rgb(44, 62, 80);">取值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">absolute</font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fixed</font><font style="color:rgb(44, 62, 80);">，框就是绝对定位的，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);">的计算值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">none</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">根据下面的表格进行调整。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718780294739-600d0d90-bc37-4041-8597-74aeaaa1ca0b.png)

+ <font style="color:rgb(44, 62, 80);">否则，如果</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);">不是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">none</font><font style="color:rgb(44, 62, 80);">，框是浮动的，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">根据下表进行调整</font>
+ <font style="color:rgb(44, 62, 80);">否则，如果元素是根元素，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">根据下表进行调整</font>
+ <font style="color:rgb(44, 62, 80);">其他情况下</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">的值为指定值</font>
+ <font style="color:rgb(44, 62, 80);">总结起来：</font>**<font style="color:rgb(44, 62, 80);">绝对定位、浮动、根元素都需要调整</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font>**

### [](https://www.123fe.net/docs/base.html#_7-%E6%B8%85%E9%99%A4%E6%B5%AE%E5%8A%A8%E7%9A%84%E5%87%A0%E7%A7%8D%E6%96%B9%E5%BC%8F-%E5%90%84%E8%87%AA%E7%9A%84%E4%BC%98%E7%BC%BA%E7%82%B9)<font style="color:rgb(44, 62, 80);">7 清除浮动的几种方式，各自的优缺点</font>
+ <font style="color:rgb(44, 62, 80);">父级</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">定义</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height</font>
+ <font style="color:rgb(44, 62, 80);">结尾处加空</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">标签</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clear:both</font>
+ <font style="color:rgb(44, 62, 80);">父级</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">定义伪类</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:after</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">zoom</font>
+ <font style="color:rgb(44, 62, 80);">父级</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">定义</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">overflow:hidden</font>
+ <font style="color:rgb(44, 62, 80);">父级</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">也浮动，需要定义宽度</font>
+ <font style="color:rgb(44, 62, 80);">结尾处加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">br</font><font style="color:rgb(44, 62, 80);">标签</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clear:both</font>
+ <font style="color:rgb(44, 62, 80);">比较好的是第3种方式，好多网站都这么用</font>

### [](https://www.123fe.net/docs/base.html#_8-%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%88%9D%E5%A7%8B%E5%8C%96css%E6%A0%B7%E5%BC%8F)<font style="color:rgb(44, 62, 80);">8 为什么要初始化CSS样式?</font>
+ <font style="color:rgb(44, 62, 80);">因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">初始化往往会出现浏览器之间的页面显示差异。</font>
+ <font style="color:rgb(44, 62, 80);">当然，初始化样式会对</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">SEO</font><font style="color:rgb(44, 62, 80);">有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化</font>

### [](https://www.123fe.net/docs/base.html#_9-css3%E6%9C%89%E5%93%AA%E4%BA%9B%E6%96%B0%E7%89%B9%E6%80%A7)<font style="color:rgb(44, 62, 80);">9 css3有哪些新特性</font>
+ <font style="color:rgb(44, 62, 80);">新增选择器</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p:nth-child(n){color: rgba(255, 0, 0, 0.75)}</font>
+ <font style="color:rgb(44, 62, 80);">弹性盒模型</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display: flex;</font>
+ <font style="color:rgb(44, 62, 80);">多列布局</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">column-count: 5;</font>
+ <font style="color:rgb(44, 62, 80);">媒体查询</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@media (max-width: 480px) {.box: {column-count: 1;}}</font>
+ <font style="color:rgb(44, 62, 80);">个性化字体</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@font-face{font-family: BorderWeb; src:url(BORDERW0.eot);}</font>
+ <font style="color:rgb(44, 62, 80);">颜色透明度</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">color: rgba(255, 0, 0, 0.75);</font>
+ <font style="color:rgb(44, 62, 80);">圆角</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-radius: 5px;</font>
+ <font style="color:rgb(44, 62, 80);">渐变</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background:linear-gradient(red, green, blue);</font>
+ <font style="color:rgb(44, 62, 80);">阴影</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-shadow:3px 3px 3px rgba(0, 64, 128, 0.3);</font>
+ <font style="color:rgb(44, 62, 80);">倒影</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-reflect: below 2px;</font>
+ <font style="color:rgb(44, 62, 80);">文字装饰</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-stroke-color: red;</font>
+ <font style="color:rgb(44, 62, 80);">文字溢出</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-overflow:ellipsis;</font>
+ <font style="color:rgb(44, 62, 80);">背景效果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-size: 100px 100px;</font>
+ <font style="color:rgb(44, 62, 80);">边框效果</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-image:url(bt_blue.png) 0 10;</font>
+ <font style="color:rgb(44, 62, 80);">转换</font>
    - <font style="color:rgb(44, 62, 80);">旋转</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: rotate(20deg);</font>
    - <font style="color:rgb(44, 62, 80);">倾斜</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: skew(150deg, -10deg);</font>
    - <font style="color:rgb(44, 62, 80);">位移</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: translate(20px, 20px);</font>
    - <font style="color:rgb(44, 62, 80);">缩放</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: scale(.5);</font>
+ <font style="color:rgb(44, 62, 80);">平滑过渡</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition: all .3s ease-in .1s;</font>
+ <font style="color:rgb(44, 62, 80);">动画</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@keyframes anim-1 {50% {border-radius: 50%;}} animation: anim-1 1s;</font>

**<font style="color:rgb(44, 62, 80);">CSS3新增伪类有那些？</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p:first-of-type</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择属于其父元素的首个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);">元素的每个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p:last-of-type</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择属于其父元素的最后</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素的每个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p:only-of-type</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择属于其父元素唯一的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);">元素的每个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p:only-child</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择属于其父元素的唯一子元素的每个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p:nth-child(2)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择属于其父元素的第二个子元素的每个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><p></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:after</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在元素之前添加内容,也可以用来做清除浮动。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:before</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在元素之后添加内容。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:enabled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">已启用的表单元素。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:disabled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">已禁用的表单元素。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:checked</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">单选框或复选框被选中。</font>

### [](https://www.123fe.net/docs/base.html#_10-display%E6%9C%89%E5%93%AA%E4%BA%9B%E5%80%BC-%E8%AF%B4%E6%98%8E%E4%BB%96%E4%BB%AC%E7%9A%84%E4%BD%9C%E7%94%A8)<font style="color:rgb(44, 62, 80);">10 display有哪些值？说明他们的作用</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">block</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">转换成块状元素。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">转换成行内元素。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">none</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">设置元素不可见。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-block</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">象行内元素一样显示，但其内容象块类型元素一样显示。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">list-item</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">象块类型元素一样显示，并添加样式列表标记。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">table</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">此元素会作为块级表格来显示</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inherit</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定应该从父元素继承</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值</font>

### [](https://www.123fe.net/docs/base.html#_11-%E4%BB%8B%E7%BB%8D%E4%B8%80%E4%B8%8B%E6%A0%87%E5%87%86%E7%9A%84css%E7%9A%84%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B-%E4%BD%8E%E7%89%88%E6%9C%ACie%E7%9A%84%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B%E6%9C%89%E4%BB%80%E4%B9%88%E4%B8%8D%E5%90%8C%E7%9A%84)<font style="color:rgb(44, 62, 80);">11 介绍一下标准的CSS的盒子模型？低版本IE的盒子模型有什么不同的？</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">有两种，</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">盒子模型、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">W3C</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">盒子模型；</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">盒模型： 内容(content)、填充(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)、边界(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)、 边框(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)；</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">区 别：</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的c</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ontent</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">部分把</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">计算了进去;</font>
+ <font style="color:rgb(44, 62, 80);">盒子模型构成：内容(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font><font style="color:rgb(44, 62, 80);">)、内填充(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);">)、 边框(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">)、外边距(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE8</font><font style="color:rgb(44, 62, 80);">及其以下版本浏览器，未声明</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOCTYPE</font><font style="color:rgb(44, 62, 80);">，内容宽高会包含内填充和边框，称为怪异盒模型(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">盒模型)</font>
+ <font style="color:rgb(44, 62, 80);">标准(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">W3C</font><font style="color:rgb(44, 62, 80);">)盒模型：元素宽度 =</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width + padding + border + margin</font>
+ <font style="color:rgb(44, 62, 80);">怪异(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">)盒模型：元素宽度 =</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width + margin</font>
+ <font style="color:rgb(44, 62, 80);">标准浏览器通过设置 css3 的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing: border-box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，触发“怪异模式”解析计算宽高</font>

**<font style="color:rgb(44, 62, 80);">box-sizing 常用的属性有哪些？分别有什么作用</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing: content-box;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">默认的标准(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">W3C</font><font style="color:rgb(44, 62, 80);">)盒模型元素效果</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing: border-box;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">触发怪异(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">)盒模型元素的效果</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing: inherit;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">继承父元素</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值</font>

### [](https://www.123fe.net/docs/base.html#_12-css%E4%BC%98%E5%85%88%E7%BA%A7%E7%AE%97%E6%B3%95%E5%A6%82%E4%BD%95%E8%AE%A1%E7%AE%97)<font style="color:rgb(44, 62, 80);">12 CSS优先级算法如何计算？</font>
+ <font style="color:rgb(44, 62, 80);">优先级就近原则，同权重情况下样式定义最近者为准</font>
+ <font style="color:rgb(44, 62, 80);">载入样式以最后载入的定位为准</font>
+ <font style="color:rgb(44, 62, 80);">优先级为:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">!important > id > class > tag</font><font style="color:rgb(44, 62, 80);">;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">!important</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">比 内联优先级高</font>

### [](https://www.123fe.net/docs/base.html#_13-%E5%AF%B9bfc%E8%A7%84%E8%8C%83%E7%9A%84%E7%90%86%E8%A7%A3)<font style="color:rgb(44, 62, 80);">13 对BFC规范的理解？</font>
+ <font style="color:rgb(44, 62, 80);">一个页面是由很多个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">组成的,元素的类型和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性,决定了这个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的类型</font>
+ <font style="color:rgb(44, 62, 80);">不同类型的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);">,会参与不同的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Formatting Context</font><font style="color:rgb(44, 62, 80);">（决定如何渲染文档的容器）,因此</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);">内的元素会以不同的方式渲染,也就是说BFC内部的元素和外部的元素不会互相影响</font>

### [](https://www.123fe.net/docs/base.html#_14-%E8%B0%88%E8%B0%88%E6%B5%AE%E5%8A%A8%E5%92%8C%E6%B8%85%E9%99%A4%E6%B5%AE%E5%8A%A8)<font style="color:rgb(44, 62, 80);">14 谈谈浮动和清除浮动</font>
+ <font style="color:rgb(44, 62, 80);">浮动的框可以向左或向右移动，直到他的外边缘碰到包含框或另一个浮动框的边框为止。由于浮动框不在文档的普通流中，所以文档的普通流的块框表现得就像浮动框不存在一样。浮动的块框会漂浮在文档普通流的块框上</font>

### [](https://www.123fe.net/docs/base.html#_15-position%E7%9A%84%E5%80%BC-relative%E5%92%8Cabsolute%E5%AE%9A%E4%BD%8D%E5%8E%9F%E7%82%B9%E6%98%AF)<font style="color:rgb(44, 62, 80);">15 position的值， relative和absolute定位原点是</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">absolute</font><font style="color:rgb(44, 62, 80);">：生成绝对定位的元素，相对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">static</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">定位以外的第一个父元素进行定位</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fixed</font><font style="color:rgb(44, 62, 80);">：生成绝对定位的元素，相对于浏览器窗口进行定位</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">relative</font><font style="color:rgb(44, 62, 80);">：生成相对定位的元素，相对于其正常位置进行定位</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">static</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">默认值。没有定位，元素出现在正常的流中</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inherit</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定从父元素继承</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值</font>

### [](https://www.123fe.net/docs/base.html#_16-display-inline-block-%E4%BB%80%E4%B9%88%E6%97%B6%E5%80%99%E4%B8%8D%E4%BC%9A%E6%98%BE%E7%A4%BA%E9%97%B4%E9%9A%99-%E6%90%BA%E7%A8%8B)<font style="color:rgb(44, 62, 80);">16 display:inline-block 什么时候不会显示间隙？(携程)</font>
+ <font style="color:rgb(44, 62, 80);">移除空格</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">负值</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-size:0</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">letter-spacing</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">word-spacing</font>

### [](https://www.123fe.net/docs/base.html#_17-png-gif-jpg%E7%9A%84%E5%8C%BA%E5%88%AB%E5%8F%8A%E5%A6%82%E4%BD%95%E9%80%89)<font style="color:rgb(44, 62, 80);">17 PNG\GIF\JPG的区别及如何选</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GIF</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">8</font><font style="color:rgb(44, 62, 80);">位像素，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">256</font><font style="color:rgb(44, 62, 80);">色</font>
    - <font style="color:rgb(44, 62, 80);">无损压缩</font>
    - <font style="color:rgb(44, 62, 80);">支持简单动画</font>
    - <font style="color:rgb(44, 62, 80);">支持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">boolean</font><font style="color:rgb(44, 62, 80);">透明</font>
    - <font style="color:rgb(44, 62, 80);">适合简单动画</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">JPEG</font>
    - <font style="color:rgb(44, 62, 80);">颜色限于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">256</font>
    - <font style="color:rgb(44, 62, 80);">有损压缩</font>
    - <font style="color:rgb(44, 62, 80);">可控制压缩质量</font>
    - <font style="color:rgb(44, 62, 80);">不支持透明</font>
    - <font style="color:rgb(44, 62, 80);">适合照片</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PNG</font>
    - <font style="color:rgb(44, 62, 80);">有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PNG8</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">truecolor PNG</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PNG8</font><font style="color:rgb(44, 62, 80);">类似</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">GIF</font><font style="color:rgb(44, 62, 80);">颜色上限为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">256</font><font style="color:rgb(44, 62, 80);">，文件小，支持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">alpha</font><font style="color:rgb(44, 62, 80);">透明度，无动画</font>
    - <font style="color:rgb(44, 62, 80);">适合图标、背景、按钮</font>

### [](https://www.123fe.net/docs/base.html#_18-%E8%A1%8C%E5%86%85%E5%85%83%E7%B4%A0float-left%E5%90%8E%E6%98%AF%E5%90%A6%E5%8F%98%E4%B8%BA%E5%9D%97%E7%BA%A7%E5%85%83%E7%B4%A0)<font style="color:rgb(44, 62, 80);">18 行内元素float:left后是否变为块级元素？</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">行内元素设置成浮动之后变得更加像是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-block</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（行内块级元素，设置成这个属性的元素会同时拥有行内和块级的特性，最明显的不同是它的默认宽度不是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100%</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">），这时候给行内元素设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding-top</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding-bottom</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">都是有效果的</font>

### [](https://www.123fe.net/docs/base.html#_19-%E5%9C%A8%E7%BD%91%E9%A1%B5%E4%B8%AD%E7%9A%84%E5%BA%94%E8%AF%A5%E4%BD%BF%E7%94%A8%E5%A5%87%E6%95%B0%E8%BF%98%E6%98%AF%E5%81%B6%E6%95%B0%E7%9A%84%E5%AD%97%E4%BD%93-%E4%B8%BA%E4%BB%80%E4%B9%88%E5%91%A2)<font style="color:rgb(44, 62, 80);">19 在网页中的应该使用奇数还是偶数的字体？为什么呢？</font>
+ <font style="color:rgb(44, 62, 80);">偶数字号相对更容易和 web 设计的其他部分构成比例关系</font>

### [](https://www.123fe.net/docs/base.html#_20-before-%E5%92%8C-after%E4%B8%AD%E5%8F%8C%E5%86%92%E5%8F%B7%E5%92%8C%E5%8D%95%E5%86%92%E5%8F%B7-%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB-%E8%A7%A3%E9%87%8A%E4%B8%80%E4%B8%8B%E8%BF%992%E4%B8%AA%E4%BC%AA%E5%85%83%E7%B4%A0%E7%9A%84%E4%BD%9C%E7%94%A8)<font style="color:rgb(44, 62, 80);">20 ::before 和 :after中双冒号和单冒号 有什么区别？解释一下这2个伪元素的作用</font>
+ <font style="color:rgb(44, 62, 80);">单冒号(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:</font><font style="color:rgb(44, 62, 80);">)用于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);">伪类，双冒号(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">::</font><font style="color:rgb(44, 62, 80);">)用于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);">伪元素</font>
+ <font style="color:rgb(44, 62, 80);">用于区分伪类和伪元素</font>

### [](https://www.123fe.net/docs/base.html#_21-%E5%A6%82%E6%9E%9C%E9%9C%80%E8%A6%81%E6%89%8B%E5%8A%A8%E5%86%99%E5%8A%A8%E7%94%BB-%E4%BD%A0%E8%AE%A4%E4%B8%BA%E6%9C%80%E5%B0%8F%E6%97%B6%E9%97%B4%E9%97%B4%E9%9A%94%E6%98%AF%E5%A4%9A%E4%B9%85-%E4%B8%BA%E4%BB%80%E4%B9%88-%E9%98%BF%E9%87%8C)<font style="color:rgb(44, 62, 80);">21 如果需要手动写动画，你认为最小时间间隔是多久，为什么？（阿里）</font>
+ <font style="color:rgb(44, 62, 80);">多数显示器默认频率是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">60Hz</font><font style="color:rgb(44, 62, 80);">，即</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">秒刷新</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">60</font><font style="color:rgb(44, 62, 80);">次，所以理论上最小间隔为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1/60*1000ms ＝ 16.7ms</font>

### [](https://www.123fe.net/docs/base.html#_22-css%E5%90%88%E5%B9%B6%E6%96%B9%E6%B3%95)<font style="color:rgb(44, 62, 80);">22 CSS合并方法</font>
+ <font style="color:rgb(44, 62, 80);">避免使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font><font style="color:rgb(44, 62, 80);">引入多个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">文件，可以使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">工具将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">合并为一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">文件，例如使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Sass\Compass</font><font style="color:rgb(44, 62, 80);">等</font>

### [](https://www.123fe.net/docs/base.html#_23-css%E4%B8%8D%E5%90%8C%E9%80%89%E6%8B%A9%E5%99%A8%E7%9A%84%E6%9D%83%E9%87%8D-css%E5%B1%82%E5%8F%A0%E7%9A%84%E8%A7%84%E5%88%99)<font style="color:rgb(44, 62, 80);">23 CSS不同选择器的权重(CSS层叠的规则)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">！important</font><font style="color:rgb(44, 62, 80);">规则最重要，大于其它规则</font>
+ <font style="color:rgb(44, 62, 80);">行内样式规则，加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1000</font>
+ <font style="color:rgb(44, 62, 80);">对于选择器中给定的各个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ID</font><font style="color:rgb(44, 62, 80);">属性值，加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100</font>
+ <font style="color:rgb(44, 62, 80);">对于选择器中给定的各个类属性、属性选择器或者伪类选择器，加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">10</font>
+ <font style="color:rgb(44, 62, 80);">对于选择其中给定的各个元素标签选择器，加1</font>
+ <font style="color:rgb(44, 62, 80);">如果权值一样，则按照样式规则的先后顺序来应用，顺序靠后的覆盖靠前的规则</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">以下是权重的规则：标签的权重为1，class的权重为10，id的权重为100，以下/// 例子是演示各种定义的权重值：</font>

```css
/*权重为1*/
div{
}
/*权重为10*/
.class1{
}
/*权重为100*/
id1{
}
/*权重为100+1=101*/
id1 div{
}
/*权重为10+1=11*/
.class1 div{
}
/*权重为10+10+1=21*/
.class1 .class2 div{
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">如果权重相同，则最后定义的样式会起作用，但是应该避免这种情况出现</font>

### [](https://www.123fe.net/docs/base.html#_24-%E5%88%97%E5%87%BA%E4%BD%A0%E6%89%80%E7%9F%A5%E9%81%93%E5%8F%AF%E4%BB%A5%E6%94%B9%E5%8F%98%E9%A1%B5%E9%9D%A2%E5%B8%83%E5%B1%80%E7%9A%84%E5%B1%9E%E6%80%A7)<font style="color:rgb(44, 62, 80);">24 列出你所知道可以改变页面布局的属性</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">top</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">right</font><font style="color:rgb(44, 62, 80);">、`</font>

### [](https://www.123fe.net/docs/base.html#_25-css%E5%9C%A8%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E6%96%B9%E9%9D%A2%E7%9A%84%E5%AE%9E%E8%B7%B5)<font style="color:rgb(44, 62, 80);">25 CSS在性能优化方面的实践</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">压缩与合并、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Gzip</font><font style="color:rgb(44, 62, 80);">压缩</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">文件放在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">head</font><font style="color:rgb(44, 62, 80);">里、不要用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font>
+ <font style="color:rgb(44, 62, 80);">尽量用缩写、避免用滤镜、合理使用选择器</font>

### [](https://www.123fe.net/docs/base.html#_26-css3%E5%8A%A8%E7%94%BB-%E7%AE%80%E5%8D%95%E5%8A%A8%E7%94%BB%E7%9A%84%E5%AE%9E%E7%8E%B0-%E5%A6%82%E6%97%8B%E8%BD%AC%E7%AD%89)<font style="color:rgb(44, 62, 80);">26 CSS3动画（简单动画的实现，如旋转等）</font>
+ <font style="color:rgb(44, 62, 80);">依靠</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);">中提出的三个属性：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(44, 62, 80);">：定义了元素在变化过程中是怎么样的，包含</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-property</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-duration</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-timing-function</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-delay</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(44, 62, 80);">：定义元素的变化结果，包含</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rotate</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scale</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">skew</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">translate</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(44, 62, 80);">：动画定义了动作的每一帧（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@keyframes</font><font style="color:rgb(44, 62, 80);">）有什么效果，包括</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-name</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-duration</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-timing-function</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-delay</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-iteration-count</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-direction</font>

### [](https://www.123fe.net/docs/base.html#_27-base64%E7%9A%84%E5%8E%9F%E7%90%86%E5%8F%8A%E4%BC%98%E7%BC%BA%E7%82%B9)<font style="color:rgb(44, 62, 80);">27 base64的原理及优缺点</font>
+ <font style="color:rgb(44, 62, 80);">优点可以加密，减少了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTTP</font><font style="color:rgb(44, 62, 80);">请求</font>
+ <font style="color:rgb(44, 62, 80);">缺点是需要消耗</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CPU</font><font style="color:rgb(44, 62, 80);">进行编解码</font>

### [](https://www.123fe.net/docs/base.html#_28-%E5%87%A0%E7%A7%8D%E5%B8%B8%E8%A7%81%E7%9A%84css%E5%B8%83%E5%B1%80)<font style="color:rgb(44, 62, 80);">28 几种常见的CSS布局</font>
#### [](https://www.123fe.net/docs/base.html#%E6%B5%81%E4%BD%93%E5%B8%83%E5%B1%80)<font style="color:rgb(44, 62, 80);">流体布局</font>
```css
.left {
		float: left;
		width: 100px;
		height: 200px;
		background: red;
	}
	.right {
		float: right;
		width: 200px;
		height: 200px;
		background: blue;
	}
	.main {
		margin-left: 120px;
		margin-right: 220px;
		height: 200px;
		background: green;
	}
```

```html
<div class="container">
    <div class="left"></div>
    <div class="right"></div>
    <div class="main"></div>
</div>
```

#### [](https://www.123fe.net/docs/base.html#%E5%9C%A3%E6%9D%AF%E5%B8%83%E5%B1%80)<font style="color:rgb(44, 62, 80);">圣杯布局</font>
+ <font style="color:rgb(44, 62, 80);">要求：三列布局；中间主体内容前置，且宽度自适应；两边内容定宽</font>
    - <font style="color:rgb(44, 62, 80);">好处：重要的内容放在文档流前面可以优先渲染</font>
    - <font style="color:rgb(44, 62, 80);">原理：利用相对定位、浮动、负边距布局，而不添加额外标签</font>

```css
.container {
      padding-left: 150px;
      padding-right: 190px;
  }
  .main {
      float: left;
      width: 100%;
  }
  .left {
      float: left;
      width: 190px;
      margin-left: -100%;
      position: relative;
      left: -150px;
  }
  .right {
      float: left;
      width: 190px;
      margin-left: -190px;
      position: relative;
      right: -190px;
  }
```

```html
<div class="container">
	<div class="main"></div>
	<div class="left"></div>
	<div class="right"></div>
</div>
```

#### [](https://www.123fe.net/docs/base.html#%E5%8F%8C%E9%A3%9E%E7%BF%BC%E5%B8%83%E5%B1%80)<font style="color:rgb(44, 62, 80);">双飞翼布局</font>
+ <font style="color:rgb(44, 62, 80);">双飞翼布局：对圣杯布局（使用相对定位，对以后布局有局限性）的改进，消除相对定位布局</font>
+ <font style="color:rgb(44, 62, 80);">原理：主体元素上设置左右边距，预留两翼位置。左右两栏使用浮动和负边距归位，消除相对定位。</font>

```css
.container {
    /*padding-left:150px;*/
    /*padding-right:190px;*/
}
.main-wrap {
    width: 100%;
    float: left;
}
.main {
    margin-left: 150px;
    margin-right: 190px;
}
.left {
    float: left;
    width: 150px;
    margin-left: -100%;
    /*position: relative;*/
    /*left:-150px;*/
}
.right {
    float: left;
    width: 190px;
    margin-left: -190px;
    /*position:relative;*/
    /*right:-190px;*/
}
```

```html
<div class="content">
    <div class="main"></div>
</div>
<div class="left"></div>
<div class="right"></div>
```

### [](https://www.123fe.net/docs/base.html#_29-stylus-sass-less%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">29 stylus/sass/less区别</font>
+ <font style="color:rgb(44, 62, 80);">均具有“变量”、“混合”、“嵌套”、“继承”、“颜色混合”五大基本特性</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Scss</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LESS</font><font style="color:rgb(44, 62, 80);">语法较为严谨，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LESS</font><font style="color:rgb(44, 62, 80);">要求一定要使用大括号“{}”，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Scss</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Stylus</font><font style="color:rgb(44, 62, 80);">可以通过缩进表示层次与嵌套关系</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Scss</font><font style="color:rgb(44, 62, 80);">无全局变量的概念，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LESS</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Stylus</font><font style="color:rgb(44, 62, 80);">有类似于其它语言的作用域概念</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Sass</font><font style="color:rgb(44, 62, 80);">是基于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ruby</font><font style="color:rgb(44, 62, 80);">语言的，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LESS</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Stylus</font><font style="color:rgb(44, 62, 80);">可以基于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NodeJS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">NPM</font><font style="color:rgb(44, 62, 80);">下载相应库后进行编译；</font>

### [](https://www.123fe.net/docs/base.html#_30-postcss%E7%9A%84%E4%BD%9C%E7%94%A8)<font style="color:rgb(44, 62, 80);">30 postcss的作用</font>
+ <font style="color:rgb(44, 62, 80);">可以直观的理解为：它就是一个平台。为什么说它是一个平台呢？因为我们直接用它，感觉不能干什么事情，但是如果让一些插件在它上面跑，那么将会很强大</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PostCSS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">提供了一个解析器，它能够将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">解析成抽象语法树</font>
+ <font style="color:rgb(44, 62, 80);">通过在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">PostCSS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个平台上，我们能够开发一些插件，来处理我们的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">，比如热门的：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">autoprefixer</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">postcss</font><font style="color:rgb(44, 62, 80);">可以对sass处理过后的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">再处理 最常见的就是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">autoprefixer</font>

### [](https://www.123fe.net/docs/base.html#_31-css%E6%A0%B7%E5%BC%8F-%E9%80%89%E6%8B%A9%E5%99%A8-%E7%9A%84%E4%BC%98%E5%85%88%E7%BA%A7)<font style="color:rgb(44, 62, 80);">31 css样式（选择器）的优先级</font>
+ <font style="color:rgb(44, 62, 80);">计算权重确定</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">!important</font>
+ <font style="color:rgb(44, 62, 80);">内联样式</font>
+ <font style="color:rgb(44, 62, 80);">后写的优先级高</font>

### [](https://www.123fe.net/docs/base.html#_32-%E8%87%AA%E5%AE%9A%E4%B9%89%E5%AD%97%E4%BD%93%E7%9A%84%E4%BD%BF%E7%94%A8%E5%9C%BA%E6%99%AF)<font style="color:rgb(44, 62, 80);">32 自定义字体的使用场景</font>
+ <font style="color:rgb(44, 62, 80);">宣传/品牌/</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">banner</font><font style="color:rgb(44, 62, 80);">等固定文案</font>
+ <font style="color:rgb(44, 62, 80);">字体图标</font>

### [](https://www.123fe.net/docs/base.html#_33-%E5%A6%82%E4%BD%95%E7%BE%8E%E5%8C%96checkbox)<font style="color:rgb(44, 62, 80);">33 如何美化CheckBox</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><label></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">for</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">id</font>
+ <font style="color:rgb(44, 62, 80);">隐藏原生的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><input></font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:checked + <label></font>

### [](https://www.123fe.net/docs/base.html#_34-%E4%BC%AA%E7%B1%BB%E5%92%8C%E4%BC%AA%E5%85%83%E7%B4%A0%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">34 伪类和伪元素的区别</font>
+ <font style="color:rgb(44, 62, 80);">伪类表状态</font>
+ <font style="color:rgb(44, 62, 80);">伪元素是真的有元素</font>
+ <font style="color:rgb(44, 62, 80);">前者单冒号，后者双冒号</font>

### [](https://www.123fe.net/docs/base.html#_35-base64%E7%9A%84%E4%BD%BF%E7%94%A8)<font style="color:rgb(44, 62, 80);">35</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">base64</font><font style="color:rgb(44, 62, 80);">的使用</font>
+ <font style="color:rgb(44, 62, 80);">用于减少</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">请求</font>
+ <font style="color:rgb(44, 62, 80);">适用于小图片</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">base64</font><font style="color:rgb(44, 62, 80);">的体积约为原图的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">3/4</font>

### [](https://www.123fe.net/docs/base.html#_36-%E8%87%AA%E9%80%82%E5%BA%94%E5%B8%83%E5%B1%80)<font style="color:rgb(44, 62, 80);">36 自适应布局</font>
<font style="color:rgb(44, 62, 80);">思路：</font>

+ <font style="color:rgb(44, 62, 80);">左侧浮动或者绝对定位，然后右侧</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">撑开</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><div></font><font style="color:rgb(44, 62, 80);">包含，然后靠负</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">形成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bfc</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex</font>

### [](https://www.123fe.net/docs/base.html#_37-%E8%AF%B7%E7%94%A8css%E5%86%99%E4%B8%80%E4%B8%AA%E7%AE%80%E5%8D%95%E7%9A%84%E5%B9%BB%E7%81%AF%E7%89%87%E6%95%88%E6%9E%9C%E9%A1%B5%E9%9D%A2)<font style="color:rgb(44, 62, 80);">37 请用CSS写一个简单的幻灯片效果页面</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">知道是要用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">动画实现一个简单的幻灯片效果</font>

```css
/**css**/
.ani{
  width:480px;
  height:320px;
  margin:50px auto;
  overflow: hidden;
  box-shadow:0 0 5px rgba(0,0,0,1);
  background-size: cover;
  background-position: center;
  -webkit-animation-name: "loops";
  -webkit-animation-duration: 20s;
  -webkit-animation-iteration-count: infinite;
}
@-webkit-keyframes "loops" {
    0% {
        background:url(http://d.hiphotos.baidu.com/image/w%3D400/sign=c01e6adca964034f0fcdc3069fc27980/e824b899a9014c08e5e38ca4087b02087af4f4d3.jpg) no-repeat;             
    }
    25% {
        background:url(http://b.hiphotos.baidu.com/image/w%3D400/sign=edee1572e9f81a4c2632edc9e72b6029/30adcbef76094b364d72bceba1cc7cd98c109dd0.jpg) no-repeat;
    }
    50% {
        background:url(http://b.hiphotos.baidu.com/image/w%3D400/sign=937dace2552c11dfded1be2353266255/d8f9d72a6059252d258e7605369b033b5bb5b912.jpg) no-repeat;
    }
    75% {
        background:url(http://g.hiphotos.baidu.com/image/w%3D400/sign=7d37500b8544ebf86d71653fe9f9d736/0df431adcbef76095d61f0972cdda3cc7cd99e4b.jpg) no-repeat;
    }
    100% {
        background:url(http://c.hiphotos.baidu.com/image/w%3D400/sign=cfb239ceb0fb43161a1f7b7a10a54642/3b87e950352ac65ce2e73f76f9f2b21192138ad1.jpg) no-repeat;
    }
}
```

### [](https://www.123fe.net/docs/base.html#_38-%E4%BB%80%E4%B9%88%E6%98%AF%E5%A4%96%E8%BE%B9%E8%B7%9D%E9%87%8D%E5%8F%A0-%E9%87%8D%E5%8F%A0%E7%9A%84%E7%BB%93%E6%9E%9C%E6%98%AF%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">38 什么是外边距重叠？重叠的结果是什么？</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">外边距重叠就是margin-collapse</font>

+ <font style="color:rgb(44, 62, 80);">在CSS当中，相邻的两个盒子（可能是兄弟关系也可能是祖先关系）的外边距可以结合成一个单独的外边距。这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距。</font>

**<font style="color:rgb(44, 62, 80);">折叠结果遵循下列计算规则</font>**<font style="color:rgb(44, 62, 80);">：</font>

+ <font style="color:rgb(44, 62, 80);">两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。</font>
+ <font style="color:rgb(44, 62, 80);">两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。</font>
+ <font style="color:rgb(44, 62, 80);">两个外边距一正一负时，折叠结果是两者的相加的和。</font>

### [](https://www.123fe.net/docs/base.html#_39-rgba-%E5%92%8Copacity%E7%9A%84%E9%80%8F%E6%98%8E%E6%95%88%E6%9E%9C%E6%9C%89%E4%BB%80%E4%B9%88%E4%B8%8D%E5%90%8C)<font style="color:rgb(44, 62, 80);">39 rgba()和opacity的透明效果有什么不同？</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rgba()</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">opacity</font><font style="color:rgb(44, 62, 80);">都能实现透明效果，但最大的不同是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">opacity</font><font style="color:rgb(44, 62, 80);">作用于元素，以及元素内的所有内容的透明度，</font>
+ <font style="color:rgb(44, 62, 80);">而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rgba()</font><font style="color:rgb(44, 62, 80);">只作用于元素的颜色或其背景色。（设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rgba</font><font style="color:rgb(44, 62, 80);">透明的元素的子元素不会继承透明效果！）</font>

### [](https://www.123fe.net/docs/base.html#_40-css%E4%B8%AD%E5%8F%AF%E4%BB%A5%E8%AE%A9%E6%96%87%E5%AD%97%E5%9C%A8%E5%9E%82%E7%9B%B4%E5%92%8C%E6%B0%B4%E5%B9%B3%E6%96%B9%E5%90%91%E4%B8%8A%E9%87%8D%E5%8F%A0%E7%9A%84%E4%B8%A4%E4%B8%AA%E5%B1%9E%E6%80%A7%E6%98%AF%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">40 css中可以让文字在垂直和水平方向上重叠的两个属性是什么？</font>
+ <font style="color:rgb(44, 62, 80);">垂直方向：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">line-height</font>
+ <font style="color:rgb(44, 62, 80);">水平方向：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">letter-spacing</font>

### [](https://www.123fe.net/docs/base.html#_41-%E5%A6%82%E4%BD%95%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD%E4%B8%80%E4%B8%AA%E6%B5%AE%E5%8A%A8%E5%85%83%E7%B4%A0)<font style="color:rgb(44, 62, 80);">41 如何垂直居中一个浮动元素？</font>
```css
/**方法一：已知元素的高宽**/

div1{
  background-color:6699FF;
  width:200px;
  height:200px;
  position: absolute;        //父元素需要相对定位
  top: 50%;
  left: 50%;
  margin-top:-100px ;   //二分之一的height，width
  margin-left: -100px;
}

/**方法二:**/

div1{
  width: 200px;
  height: 200px;
  background-color: 6699FF;
  margin:auto;
  position: absolute;        //父元素需要相对定位
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
}
```

**<font style="color:rgb(44, 62, 80);">如何垂直居中一个</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><img></font>****<font style="color:rgb(44, 62, 80);">?（用更简便的方法。）</font>**

```css
container     /**<img>的容器设置如下**/
{
    display:table-cell;
    text-align:center;
    vertical-align:middle;
}
```

### [](https://www.123fe.net/docs/base.html#_42-px%E5%92%8Cem%E7%9A%84%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">42 px和em的区别</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);">都是长度单位，区别是，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);">的值是固定的，指定是多少就是多少，计算比较容易。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);">得值不是固定的，并且</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);">会继承父级元素的字体大小。</font>
+ <font style="color:rgb(44, 62, 80);">浏览器的默认字体高都是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">16px</font><font style="color:rgb(44, 62, 80);">。所以未经调整的浏览器都符合:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1em=16px</font><font style="color:rgb(44, 62, 80);">。那么</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">12px=0.75em</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">10px=0.625em</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">px 相对于显示器屏幕分辨率，无法用浏览器字体放大功能</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">em 值并不是固定的，会继承父级的字体大小： em = 像素值 / 父级font-size</font>

### [](https://www.123fe.net/docs/base.html#_43-sass%E3%80%81less%E6%98%AF%E4%BB%80%E4%B9%88-%E5%A4%A7%E5%AE%B6%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%BD%BF%E7%94%A8%E4%BB%96%E4%BB%AC)<font style="color:rgb(44, 62, 80);">43 Sass、LESS是什么？大家为什么要使用他们？</font>
+ <font style="color:rgb(44, 62, 80);">他们是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">预处理器。他是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">上的一种抽象层。他们是一种特殊的语法/语言编译成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">例如Less是一种动态样式语言. 将CSS赋予了动态语言的特性，如变量，继承，运算， 函数.</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">LESS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">既可以在客户端上运行 (支持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE 6+</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Webkit</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Firefox</font><font style="color:rgb(44, 62, 80);">)，也可一在服务端运行 (借助</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Node.js</font><font style="color:rgb(44, 62, 80);">)</font>

**<font style="color:rgb(44, 62, 80);">为什么要使用它们？</font>**

+ <font style="color:rgb(44, 62, 80);">结构清晰，便于扩展。</font>
+ <font style="color:rgb(44, 62, 80);">可以方便地屏蔽浏览器私有语法差异。这个不用多说，封装对- 浏览器语法差异的重复处理，减少无意义的机械劳动。</font>
+ <font style="color:rgb(44, 62, 80);">可以轻松实现多重继承。</font>
+ <font style="color:rgb(44, 62, 80);">完全兼容 CSS 代码，可以方便地应用到老项目中。LESS 只- 是在 CSS 语法上做了扩展，所以老的 CSS 代码也可以与 LESS 代码一同编译</font>

### [](https://www.123fe.net/docs/base.html#_44-%E7%9F%A5%E9%81%93css%E6%9C%89%E4%B8%AAcontent%E5%B1%9E%E6%80%A7%E5%90%97-%E6%9C%89%E4%BB%80%E4%B9%88%E4%BD%9C%E7%94%A8-%E6%9C%89%E4%BB%80%E4%B9%88%E5%BA%94%E7%94%A8)<font style="color:rgb(44, 62, 80);">44 知道css有个content属性吗？有什么作用？有什么应用？</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">css的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性专门应用在</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">before/after</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">伪元素上，用于来插入生成内容。最常见的应用是利用伪类清除浮动。</font>

```css
/**一种常见利用伪类清除浮动的代码**/
.clearfix:after {
    content:".";       //这里利用到了content属性
    display:block;
    height:0;
    visibility:hidden;
    clear:both; 
 }
.clearfix {
    *zoom:1;
}
```

### [](https://www.123fe.net/docs/base.html#_45-%E6%B0%B4%E5%B9%B3%E5%B1%85%E4%B8%AD%E7%9A%84%E6%96%B9%E6%B3%95)<font style="color:rgb(44, 62, 80);">45 水平居中的方法</font>
+ <font style="color:rgb(44, 62, 80);">元素为行内元素，设置父元素</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-align:center</font>
+ <font style="color:rgb(44, 62, 80);">如果元素宽度固定，可以设置左右</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font><font style="color:rgb(44, 62, 80);">;</font>
+ <font style="color:rgb(44, 62, 80);">绝对定位和移动:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">absolute + transform</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex-box</font><font style="color:rgb(44, 62, 80);">布局，指定</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">justify-content</font><font style="color:rgb(44, 62, 80);">属性为center</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">设置为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tabel-ceil</font>

### [](https://www.123fe.net/docs/base.html#_46-%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD%E7%9A%84%E6%96%B9%E6%B3%95)<font style="color:rgb(44, 62, 80);">46 垂直居中的方法</font>
+ <font style="color:rgb(44, 62, 80);">将显示方式设置为表格，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:table-cell</font><font style="color:rgb(44, 62, 80);">,同时设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vertial-align：middle</font>
+ <font style="color:rgb(44, 62, 80);">使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex</font><font style="color:rgb(44, 62, 80);">布局，设置为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">align-item：center</font>
+ <font style="color:rgb(44, 62, 80);">绝对定位中设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bottom:0,top:0</font><font style="color:rgb(44, 62, 80);">,并设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin:auto</font>
+ <font style="color:rgb(44, 62, 80);">绝对定位中固定高度时设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">top:50%，margin-top</font><font style="color:rgb(44, 62, 80);">值为高度一半的负值</font>
+ <font style="color:rgb(44, 62, 80);">文本垂直居中设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">line-height</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height</font><font style="color:rgb(44, 62, 80);">值</font>
+ <font style="color:rgb(44, 62, 80);">如果是单行文本, line-height 设置成和 height 值</font>

```css
.vertical {
    height: 100px;
    line-height: 100px;
  }
```

+ <font style="color:rgb(44, 62, 80);">已知高度的块级子元素，采用绝对定位和负边距</font>

```css
.container {
  position: relative;
}
.vertical {
  height: 300px;  /*子元素高度*/
  position: absolute;
  top:50%;  /*父元素高度50%*/
  margin-top: -150px; /*自身高度一半*/
}
```

+ <font style="color:rgb(44, 62, 80);">未知高度的块级父子元素居中，模拟表格布局</font>
+ <font style="color:rgb(44, 62, 80);">缺点：IE67不兼容，父级 overflow：hidden 失效</font>

```css
.container {
    display: table;
  }
  .content {
    display: table-cell;
    vertical-align: middle;
  }
```

+ <font style="color:rgb(44, 62, 80);">新增 inline-block 兄弟元素，设置 vertical-align</font>
    - <font style="color:rgb(44, 62, 80);">缺点：需要增加额外标签，IE67不兼容</font>

```css
.container {
  height: 100%;/*定义父级高度，作为参考*/
}
.extra .vertical{
  display: inline-block;  /*行内块显示*/
  vertical-align: middle; /*垂直居中*/
}
.extra {
  height: 100%; /*设置新增元素高度为100%*/
}
```

+ <font style="color:rgb(44, 62, 80);">绝对定位配合 CSS3 位移</font>

```css
.vertical {
  position: absolute;
  top:50%;  /*父元素高度50%*/
  transform:translateY(-50%, -50%);
}
```

+ <font style="color:rgb(44, 62, 80);">CSS3弹性盒模型</font>

```css
.container {
  display:flex;
  justify-content: center; /*子元素水平居中*/
  align-items: center; /*子元素垂直居中*/
}
```

### [](https://www.123fe.net/docs/base.html#_47-%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8css%E5%AE%9E%E7%8E%B0%E7%A1%AC%E4%BB%B6%E5%8A%A0%E9%80%9F)<font style="color:rgb(44, 62, 80);">47 如何使用CSS实现硬件加速？</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">硬件加速是指通过创建独立的复合图层，让GPU来渲染这个图层，从而提高性能，</font>

+ <font style="color:rgb(44, 62, 80);">一般触发硬件加速的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">属性有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">opacity</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">filter</font><font style="color:rgb(44, 62, 80);">，为了避免2D动画在 开始和结束的时候的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">repaint</font><font style="color:rgb(44, 62, 80);">操作，一般使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">tranform:translateZ(0)</font>

### [](https://www.123fe.net/docs/base.html#_48-%E9%87%8D%E7%BB%98%E5%92%8C%E5%9B%9E%E6%B5%81-%E9%87%8D%E6%8E%92-%E6%98%AF%E4%BB%80%E4%B9%88-%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D)<font style="color:rgb(44, 62, 80);">48 重绘和回流（重排）是什么，如何避免？</font>
+ <font style="color:rgb(44, 62, 80);">重绘：当渲染树中的元素外观（如：颜色）发生改变，不影响布局时，产生重绘</font>
+ <font style="color:rgb(44, 62, 80);">回流：当渲染树中的元素的布局（如：尺寸、位置、隐藏/状态状态）发生改变时，产生重绘回流</font>
+ <font style="color:rgb(44, 62, 80);">注意：JS获取Layout属性值（如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">offsetLeft</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scrollTop</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getComputedStyle</font><font style="color:rgb(44, 62, 80);">等）也会引起回流。因为浏览器需要通过回流计算最新值</font>
+ <font style="color:rgb(44, 62, 80);">回流必将引起重绘，而重绘不一定会引起回流</font>

**<font style="color:rgb(44, 62, 80);">如何最小化重绘(repaint)和回流(reflow)</font>**<font style="color:rgb(44, 62, 80);">：</font>

+ <font style="color:rgb(44, 62, 80);">需要要对元素进行复杂的操作时，可以先隐藏(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:"none"</font><font style="color:rgb(44, 62, 80);">)，操作完成后再显示</font>
+ <font style="color:rgb(44, 62, 80);">需要创建多个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOM</font><font style="color:rgb(44, 62, 80);">节点时，使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DocumentFragment</font><font style="color:rgb(44, 62, 80);">创建完后一次性的加入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document</font>
+ <font style="color:rgb(44, 62, 80);">缓存</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Layout</font><font style="color:rgb(44, 62, 80);">属性值，如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">var left = elem.offsetLeft;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这样，多次使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">只产生一次回流</font>
+ <font style="color:rgb(44, 62, 80);">尽量避免用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">table</font><font style="color:rgb(44, 62, 80);">布局（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">table</font><font style="color:rgb(44, 62, 80);">元素一旦触发回流就会导致table里所有的其它元素回流）</font>
+ <font style="color:rgb(44, 62, 80);">避免使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">表达式(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">expression</font><font style="color:rgb(44, 62, 80);">)，因为每次调用都会重新计算值（包括加载页面）</font>
+ <font style="color:rgb(44, 62, 80);">尽量使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性简写，如：用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">代替</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-width</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-style</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-color</font>
+ <font style="color:rgb(44, 62, 80);">批量修改元素样式：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">elem.className</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">elem.style.cssText</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">代替</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">elem.style.xxx</font>

### [](https://www.123fe.net/docs/base.html#_49-%E8%AF%B4%E4%B8%80%E8%AF%B4css3%E7%9A%84animation)<font style="color:rgb(44, 62, 80);">49 说一说css3的animation</font>
+ <font style="color:rgb(44, 62, 80);">css3的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(44, 62, 80);">是css3新增的动画属性，这个css3动画的每一帧是通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@keyframes</font><font style="color:rgb(44, 62, 80);">来声明的，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">keyframes</font><font style="color:rgb(44, 62, 80);">声明了动画的名称，通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">from</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">to</font><font style="color:rgb(44, 62, 80);">或者是百分比来定义</font>
+ <font style="color:rgb(44, 62, 80);">每一帧动画元素的状态，通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-name</font><font style="color:rgb(44, 62, 80);">来引用这个动画，同时css3动画也可以定义动画运行的时长、动画开始时间、动画播放方向、动画循环次数、动画播放的方式，</font>
+ <font style="color:rgb(44, 62, 80);">这些相关的动画子属性有：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-name</font><font style="color:rgb(44, 62, 80);">定义动画名、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-duration</font><font style="color:rgb(44, 62, 80);">定义动画播放的时长、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-delay</font><font style="color:rgb(44, 62, 80);">定义动画延迟播放的时间、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-direction</font><font style="color:rgb(44, 62, 80);">定义 动画的播放方向、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-iteration-count</font><font style="color:rgb(44, 62, 80);">定义播放次数、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-fill-mode</font><font style="color:rgb(44, 62, 80);">定义动画播放之后的状态、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-play-state</font><font style="color:rgb(44, 62, 80);">定义播放状态，如暂停运行等、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-timing-function</font>
+ <font style="color:rgb(44, 62, 80);">定义播放的方式，如恒速播放、减速播放等。</font>

### [](https://www.123fe.net/docs/base.html#_50-%E5%B7%A6%E8%BE%B9%E5%AE%BD%E5%BA%A6%E5%9B%BA%E5%AE%9A-%E5%8F%B3%E8%BE%B9%E8%87%AA%E9%80%82%E5%BA%94)<font style="color:rgb(44, 62, 80);">50 左边宽度固定，右边自适应</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">左侧固定宽度，右侧自适应宽度的两列布局实现</font>

<font style="color:rgb(44, 62, 80);">html结构</font>

```html
<div class="outer">
    <div class="left">固定宽度</div>
    <div class="right">自适应宽度</div>
</div>
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在外层</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（类名为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">outer</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">）的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">中，有两个子</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，类名分别为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">right</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，其中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为固定宽度，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">right</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为自适应宽度</font>

**<font style="color:rgb(44, 62, 80);">方法1：左侧div设置成浮动：float: left，右侧div宽度会自动适应</font>**

```css
.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
    float: left;
}
.right {
    height: 200px;
    background-color: blue;
}
```

**<font style="color:rgb(44, 62, 80);">方法2：对右侧:div进行绝对定位，然后再设置right=0，即可以实现宽度自适应</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">绝对定位元素的第一个高级特性就是其具有自动伸缩的功能，当我们将</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">设置为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的时候（或者不设置，默认为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">），绝对定位元素会根据其</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">right</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">自动伸缩其大小</font>

```css
.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
    position: relative;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
}
.right {
    height: 200px;
    background-color: blue;
    position: absolute;
    left: 200px;
    top:0;          
    right: 0;
}
```

**<font style="color:rgb(44, 62, 80);">方法3：将左侧</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font>****<font style="color:rgb(44, 62, 80);">进行绝对定位，然后右侧</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font>****<font style="color:rgb(44, 62, 80);">设置</font>****<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin-left: 200px</font>**

```css
.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
    position: relative;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
    position: absolute;
}
.right {
    height: 200px;
    background-color: blue;
    margin-left: 200px;
}
```

**<font style="color:rgb(44, 62, 80);">方法4：使用flex布局</font>**

```css
.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
    display: flex;
    flex-direction: row;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
}
.right {
    height: 200px;
    background-color: blue;
    flex: 1;
}
```

### [](https://www.123fe.net/docs/base.html#_51-%E4%B8%A4%E7%A7%8D%E4%BB%A5%E4%B8%8A%E6%96%B9%E5%BC%8F%E5%AE%9E%E7%8E%B0%E5%B7%B2%E7%9F%A5%E6%88%96%E8%80%85%E6%9C%AA%E7%9F%A5%E5%AE%BD%E5%BA%A6%E7%9A%84%E5%9E%82%E7%9B%B4%E6%B0%B4%E5%B9%B3%E5%B1%85%E4%B8%AD)<font style="color:rgb(44, 62, 80);">51 两种以上方式实现已知或者未知宽度的垂直水平居中</font>
```css
/** 1 **/
.wraper {
  position: relative;
  .box {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 100px;
    height: 100px;
    margin: -50px 0 0 -50px;
  }
}

/** 2 **/
.wraper {
  position: relative;
  .box {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}

/** 3 **/
.wraper {
  .box {
    display: flex;
    justify-content:center;
    align-items: center;
    height: 100px;
  }
}

/** 4 **/
.wraper {
  display: table;
  .box {
    display: table-cell;
    vertical-align: middle;
  }
}
```

### [](https://www.123fe.net/docs/base.html#_52-%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E5%B0%8F%E4%BA%8E12px%E7%9A%84%E5%AD%97%E4%BD%93%E6%95%88%E6%9E%9C)<font style="color:rgb(44, 62, 80);">52 如何实现小于12px的字体效果</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform:scale()</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">这个属性只可以缩放可以定义宽高的元素，而行内元素是没有宽高的，我们可以加上一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:inline-block</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">;</font>

```css
transform: scale(0.7);
transform-origin: left top;
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">当我们在使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: scale</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">缩小元素的时候，顺便把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform-origin</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">改成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left top</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，才能保证左上角不动，因为左上角一动就导致定位就不准了（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">top</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">等都不准了）</font>

### [](https://www.123fe.net/docs/base.html#_53-css-hack%E5%8E%9F%E7%90%86%E5%8F%8A%E5%B8%B8%E7%94%A8hack)<font style="color:rgb(44, 62, 80);">53 css hack原理及常用hack</font>
+ <font style="color:rgb(44, 62, 80);">原理：利用不同浏览器对CSS的支持和解析结果不一样编写针对特定浏览器样式。</font>
+ <font style="color:rgb(44, 62, 80);">常见的hack有</font>
    - <font style="color:rgb(44, 62, 80);">属性hack</font>
    - <font style="color:rgb(44, 62, 80);">选择器hack</font>
    - <font style="color:rgb(44, 62, 80);">IE条件注释</font>

### [](https://www.123fe.net/docs/base.html#_54-css%E6%9C%89%E5%93%AA%E4%BA%9B%E7%BB%A7%E6%89%BF%E5%B1%9E%E6%80%A7)<font style="color:rgb(44, 62, 80);">54 CSS有哪些继承属性</font>
+ <font style="color:rgb(44, 62, 80);">关于文字排版的属性如：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">word-break</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">letter-spacing</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-align</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-rendering</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">word-spacing</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">white-space</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-indent</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-transform</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-shadow</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">line-height</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">color</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cursor</font>

### [](https://www.123fe.net/docs/base.html#_55-%E5%A4%96%E8%BE%B9%E8%B7%9D%E6%8A%98%E5%8F%A0-collapsing-margins)<font style="color:rgb(44, 62, 80);">55 外边距折叠(collapsing margins)</font>
+ <font style="color:rgb(44, 62, 80);">毗邻的两个或多个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会合并成一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">，叫做外边距折叠。规则如下：</font>
    - <font style="color:rgb(44, 62, 80);">两个或多个毗邻的普通流中的块元素垂直方向上的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">会折叠</font>
    - <font style="color:rgb(44, 62, 80);">浮动元素或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-block</font><font style="color:rgb(44, 62, 80);">元素或绝对定位元素的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">不会和垂直方向上的其他元素的margin折叠</font>
    - <font style="color:rgb(44, 62, 80);">创建了块级格式化上下文的元素，不会和它的子元素发生margin折叠</font>
    - <font style="color:rgb(44, 62, 80);">元素自身的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin-bottom</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin-top</font><font style="color:rgb(44, 62, 80);">相邻时也会折</font>

### [](https://www.123fe.net/docs/base.html#_56-css%E9%80%89%E6%8B%A9%E7%AC%A6%E6%9C%89%E5%93%AA%E4%BA%9B-%E5%93%AA%E4%BA%9B%E5%B1%9E%E6%80%A7%E5%8F%AF%E4%BB%A5%E7%BB%A7%E6%89%BF)<font style="color:rgb(44, 62, 80);">56 CSS选择符有哪些？哪些属性可以继承</font>
+ <font style="color:rgb(44, 62, 80);">id选择器（</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"> myid</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">类选择器（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.myclassname</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">标签选择器（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h1</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">相邻选择器（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h1 + p</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">子选择器（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ul > li</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">后代选择器（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">li a</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">通配符选择器（</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">*</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">属性选择器（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a[rel = "external"]</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">伪类选择器（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a:hover, li:nth-child</font><font style="color:rgb(44, 62, 80);">）</font>

**<font style="color:rgb(44, 62, 80);">CSS哪些属性可以继承？哪些属性不可以继承</font>**

+ <font style="color:rgb(44, 62, 80);">可继承的样式：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-size font-family color, UL LI DL DD DT</font>
+ <font style="color:rgb(44, 62, 80);">不可继承的样式：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border padding margin width height</font>

### [](https://www.123fe.net/docs/base.html#_57-css3%E6%96%B0%E5%A2%9E%E4%BC%AA%E7%B1%BB%E6%9C%89%E9%82%A3%E4%BA%9B)<font style="color:rgb(44, 62, 80);">57 CSS3新增伪类有那些</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:root</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择文档的根元素，等同于 html 元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:empty</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择没有子元素的元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:target</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选取当前活动的目标元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:not(selector)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择除</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">selector</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素意外的元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:enabled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择可用的表单元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:disabled</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择禁用的表单元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:checked</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择被选中的表单元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:after</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在元素内部最前添加内容</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:before</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在元素内部最后添加内容</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:nth-child(n)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">匹配父元素下指定子元素，在所有子元素中排序第n</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:nth-last-child(n)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">匹配父元素下指定子元素，在所有子元素中排序第n，从后向前数</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:nth-child(odd)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:nth-child(even)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:nth-child(3n+1)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:first-child</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:last-child</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:only-child</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:nth-of-type(n)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">匹配父元素下指定子元素，在同类子元素中排序第n</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:nth-last-of-type(n)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">匹配父元素下指定子元素，在同类子元素中排序第n，从后向前数</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:nth-of-type(odd)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:nth-of-type(even)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:nth-of-type(3n+1)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:first-of-type</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:last-of-type</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:only-of-type</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">::selection</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择被用户选取的元素部分</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:first-line</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择元素中的第一行</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:first-letter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择元素中的第一个字符</font>

### [](https://www.123fe.net/docs/base.html#_58-%E5%A6%82%E4%BD%95%E5%B1%85%E4%B8%ADdiv-%E5%A6%82%E4%BD%95%E5%B1%85%E4%B8%AD%E4%B8%80%E4%B8%AA%E6%B5%AE%E5%8A%A8%E5%85%83%E7%B4%A0-%E5%A6%82%E4%BD%95%E8%AE%A9%E7%BB%9D%E5%AF%B9%E5%AE%9A%E4%BD%8D%E7%9A%84div%E5%B1%85%E4%B8%AD)<font style="color:rgb(44, 62, 80);">58 如何居中div？如何居中一个浮动元素？如何让绝对定位的div居中</font>
+ <font style="color:rgb(44, 62, 80);">给</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">设置一个宽度，然后添加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin:0 auto</font><font style="color:rgb(44, 62, 80);">属性</font>

```css
div{
  width:200px;
  margin:0 auto;
```

+ <font style="color:rgb(44, 62, 80);">居中一个浮动元素</font>

```css
/* 确定容器的宽高 宽500 高 300 的层
设置层的外边距 */

.div {
  width:500px ; height:300px;//高度可以不设
  margin: -150px 0 0 -250px;
  position:relative;         //相对定位
  background-color:pink;     //方便看效果
  left:50%;
  top:50%;
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">让绝对定位的div居中</font>

```css
position: absolute;
width: 1200px;
background: none;
margin: 0 auto;
top: 0;
left: 0;
bottom: 0;
right: 0;
```

### [](https://www.123fe.net/docs/base.html#_59-%E7%94%A8%E7%BA%AFcss%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E4%B8%89%E8%A7%92%E5%BD%A2%E7%9A%84%E5%8E%9F%E7%90%86%E6%98%AF%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">59 用纯CSS创建一个三角形的原理是什么</font>
```css
/* 把上、右、左、三条边隐藏掉（颜色设为 transparent） */
demo {
  width: 0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}
```

### [](https://www.123fe.net/docs/base.html#_60-%E4%B8%80%E4%B8%AA%E6%BB%A1%E5%B1%8F-%E5%93%81-%E5%AD%97%E5%B8%83%E5%B1%80-%E5%A6%82%E4%BD%95%E8%AE%BE%E8%AE%A1)<font style="color:rgb(44, 62, 80);">60 一个满屏 品 字布局 如何设计?</font>
+ <font style="color:rgb(44, 62, 80);">简单的方式：</font>
    - <font style="color:rgb(44, 62, 80);">上面的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">宽</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100%</font><font style="color:rgb(44, 62, 80);">，</font>
    - <font style="color:rgb(44, 62, 80);">下面的两个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">分别宽</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">50%</font><font style="color:rgb(44, 62, 80);">，</font>
    - <font style="color:rgb(44, 62, 80);">然后用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline</font><font style="color:rgb(44, 62, 80);">使其不换行即可</font>

### [](https://www.123fe.net/docs/base.html#_61-li%E4%B8%8Eli%E4%B9%8B%E9%97%B4%E6%9C%89%E7%9C%8B%E4%B8%8D%E8%A7%81%E7%9A%84%E7%A9%BA%E7%99%BD%E9%97%B4%E9%9A%94%E6%98%AF%E4%BB%80%E4%B9%88%E5%8E%9F%E5%9B%A0%E5%BC%95%E8%B5%B7%E7%9A%84-%E6%9C%89%E4%BB%80%E4%B9%88%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95)<font style="color:rgb(44, 62, 80);">61 li与li之间有看不见的空白间隔是什么原因引起的？有什么解决办法</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">行框的排列会受到中间空白（回车\空格）等的影响，因为空格也属于字符,这些空白也会被应用样式，占据空间，所以会有间隔，把字符大小设为0，就没有空格了</font>

### [](https://www.123fe.net/docs/base.html#_62-%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%88%9D%E5%A7%8B%E5%8C%96css%E6%A0%B7%E5%BC%8F)<font style="color:rgb(44, 62, 80);">62 为什么要初始化CSS样式</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异</font>

### [](https://www.123fe.net/docs/base.html#_63-%E8%AF%B7%E5%88%97%E4%B8%BE%E5%87%A0%E7%A7%8D%E9%9A%90%E8%97%8F%E5%85%83%E7%B4%A0%E7%9A%84%E6%96%B9%E6%B3%95)<font style="color:rgb(44, 62, 80);">63 请列举几种隐藏元素的方法</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility: hidden;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">这个属性只是简单的隐藏某个元素，但是元素占用的空间任然存在</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">opacity: 0;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);">属性，设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">可以使一个元素完全透明</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position: absolute;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">设置一个很大的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">负值定位，使元素定位在可见区域之外</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display: none;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素会变得不可见，并且不会再占用文档的空间。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: scale(0);</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将一个元素设置为缩放无限小，元素将不可见，元素原来所在的位置将被保留</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><div hidden="hidden"></font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTML5</font><font style="color:rgb(44, 62, 80);">属性,效果和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:none;</font><font style="color:rgb(44, 62, 80);">相同，但这个属性用于记录一个元素的状态</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height: 0;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">将元素高度设为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，并消除边框</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">filter: blur(0);</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);">属性，将一个元素的模糊度设置为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">，从而使这个元素“消失”在页面中</font>

### [](https://www.123fe.net/docs/base.html#_64-rgba-%E5%92%8C-opacity-%E7%9A%84%E9%80%8F%E6%98%8E%E6%95%88%E6%9E%9C%E6%9C%89%E4%BB%80%E4%B9%88%E4%B8%8D%E5%90%8C)<font style="color:rgb(44, 62, 80);">64 rgba() 和 opacity 的透明效果有什么不同</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">opacity</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">作用于元素以及元素内的所有内容（包括文字）的透明度</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rgba()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">只作用于元素自身的颜色或其背景色，子元素不会继承透明效果</font>

### [](https://www.123fe.net/docs/base.html#_65-css-%E5%B1%9E%E6%80%A7-content-%E6%9C%89%E4%BB%80%E4%B9%88%E4%BD%9C%E7%94%A8)<font style="color:rgb(44, 62, 80);">65 css 属性 content 有什么作用</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性专门应用在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">before/after</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">伪元素上，用于插入额外内容或样式</font>

### [](https://www.123fe.net/docs/base.html#_66-%E8%AF%B7%E8%A7%A3%E9%87%8A%E4%B8%80%E4%B8%8B-css3-%E7%9A%84-flexbox-%E5%BC%B9%E6%80%A7%E7%9B%92%E5%B8%83%E5%B1%80%E6%A8%A1%E5%9E%8B-%E4%BB%A5%E5%8F%8A%E9%80%82%E7%94%A8%E5%9C%BA%E6%99%AF)<font style="color:rgb(44, 62, 80);">66 请解释一下 CSS3 的 Flexbox（弹性盒布局模型）以及适用场景</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Flexbox</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">用于不同尺寸屏幕中创建可自动扩展和收缩布局</font>

### [](https://www.123fe.net/docs/base.html#_67-%E7%BB%8F%E5%B8%B8%E9%81%87%E5%88%B0%E7%9A%84%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84js%E5%85%BC%E5%AE%B9%E6%80%A7%E6%9C%89%E5%93%AA%E4%BA%9B-%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%E6%98%AF%E4%BB%80%E4%B9%88)<font style="color:rgb(44, 62, 80);">67 经常遇到的浏览器的JS兼容性有哪些？解决方法是什么</font>
+ <font style="color:rgb(44, 62, 80);">当前样式：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">getComputedStyle(el, null) VS el.currentStyle</font>
+ <font style="color:rgb(44, 62, 80);">事件对象：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">e VS window.event</font>
+ <font style="color:rgb(44, 62, 80);">鼠标坐标：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">e.pageX, e.pageY VS window.event.x, window.event.y</font>
+ <font style="color:rgb(44, 62, 80);">按键码：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">e.which VS event.keyCode</font>
+ <font style="color:rgb(44, 62, 80);">文本节点：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">el.textContent VS el.innerText</font>

### [](https://www.123fe.net/docs/base.html#_68-%E8%AF%B7%E5%86%99%E5%87%BA%E5%A4%9A%E7%A7%8D%E7%AD%89%E9%AB%98%E5%B8%83%E5%B1%80)<font style="color:rgb(44, 62, 80);">68 请写出多种等高布局</font>
+ <font style="color:rgb(44, 62, 80);">在列的父元素上使用这个背景图进行Y轴的铺放，从而实现一种等高列的假像</font>
+ <font style="color:rgb(44, 62, 80);">模仿表格布局等高列效果：兼容性不好，在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ie6-7</font><font style="color:rgb(44, 62, 80);">无法正常运行</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css3 flexbox</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">布局：</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.container{display: flex; align-items: stretch;}</font>

### [](https://www.123fe.net/docs/base.html#_69-%E6%B5%AE%E5%8A%A8%E5%85%83%E7%B4%A0%E5%BC%95%E8%B5%B7%E7%9A%84%E9%97%AE%E9%A2%98)<font style="color:rgb(44, 62, 80);">69 浮动元素引起的问题</font>
+ <font style="color:rgb(44, 62, 80);">父元素的高度无法被撑开，影响与父元素同级的元素</font>
+ <font style="color:rgb(44, 62, 80);">与浮动元素同级的非浮动元素会跟随其后</font>

### [](https://www.123fe.net/docs/base.html#_70-css%E4%BC%98%E5%8C%96%E3%80%81%E6%8F%90%E9%AB%98%E6%80%A7%E8%83%BD%E7%9A%84%E6%96%B9%E6%B3%95%E6%9C%89%E5%93%AA%E4%BA%9B)<font style="color:rgb(44, 62, 80);">70 CSS优化、提高性能的方法有哪些</font>
+ <font style="color:rgb(44, 62, 80);">多个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">合并，尽量减少</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">HTTP</font><font style="color:rgb(44, 62, 80);">请求</font>
+ <font style="color:rgb(44, 62, 80);">将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">文件放在页面最上面</font>
+ <font style="color:rgb(44, 62, 80);">移除空的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">规则</font>
+ <font style="color:rgb(44, 62, 80);">避免使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);">表达式</font>
+ <font style="color:rgb(44, 62, 80);">选择器优化嵌套，尽量避免层级过深</font>
+ <font style="color:rgb(44, 62, 80);">充分利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">继承属性，减少代码量</font>
+ <font style="color:rgb(44, 62, 80);">抽象提取公共样式，减少代码量</font>
+ <font style="color:rgb(44, 62, 80);">属性值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">时，不加单位</font>
+ <font style="color:rgb(44, 62, 80);">属性值为小于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">的小数时，省略小数点前面的0</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">雪碧图</font>

### [](https://www.123fe.net/docs/base.html#_71-%E6%B5%8F%E8%A7%88%E5%99%A8%E6%98%AF%E6%80%8E%E6%A0%B7%E8%A7%A3%E6%9E%90css%E9%80%89%E6%8B%A9%E5%99%A8%E7%9A%84)<font style="color:rgb(44, 62, 80);">71 浏览器是怎样解析CSS选择器的</font>
+ <font style="color:rgb(44, 62, 80);">浏览器解析 CSS 选择器的方式是从右到左</font>

### [](https://www.123fe.net/docs/base.html#_72-%E5%9C%A8%E7%BD%91%E9%A1%B5%E4%B8%AD%E7%9A%84%E5%BA%94%E8%AF%A5%E4%BD%BF%E7%94%A8%E5%A5%87%E6%95%B0%E8%BF%98%E6%98%AF%E5%81%B6%E6%95%B0%E7%9A%84%E5%AD%97%E4%BD%93)<font style="color:rgb(44, 62, 80);">72 在网页中的应该使用奇数还是偶数的字体</font>
+ <font style="color:rgb(44, 62, 80);">在网页中的应该使用“偶数”字体：</font>
    - <font style="color:rgb(44, 62, 80);">偶数字号相对更容易和 web 设计的其他部分构成比例关系</font>
    - <font style="color:rgb(44, 62, 80);">使用奇数号字体时文本段落无法对齐</font>
    - <font style="color:rgb(44, 62, 80);">宋体的中文网页排布中使用最多的就是 12 和 14</font>

### [](https://www.123fe.net/docs/base.html#_73-margin%E5%92%8Cpadding%E5%88%86%E5%88%AB%E9%80%82%E5%90%88%E4%BB%80%E4%B9%88%E5%9C%BA%E6%99%AF%E4%BD%BF%E7%94%A8)<font style="color:rgb(44, 62, 80);">73 margin和padding分别适合什么场景使用</font>
+ <font style="color:rgb(44, 62, 80);">需要在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">外侧添加空白，且空白处不需要背景（色）时，使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font>
+ <font style="color:rgb(44, 62, 80);">需要在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">内测添加空白，且空白处需要背景（色）时，使用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font>

### [](https://www.123fe.net/docs/base.html#_74-%E6%8A%BD%E7%A6%BB%E6%A0%B7%E5%BC%8F%E6%A8%A1%E5%9D%97%E6%80%8E%E4%B9%88%E5%86%99-%E8%AF%B4%E5%87%BA%E6%80%9D%E8%B7%AF)<font style="color:rgb(44, 62, 80);">74 抽离样式模块怎么写，说出思路</font>
+ <font style="color:rgb(44, 62, 80);">CSS可以拆分成2部分：公共CSS 和 业务CSS：</font>
    - <font style="color:rgb(44, 62, 80);">网站的配色，字体，交互提取出为公共CSS。这部分CSS命名不应涉及具体的业务</font>
    - <font style="color:rgb(44, 62, 80);">对于业务CSS，需要有统一的命名，使用公用的前缀。可以参考面向对象的CSS</font>

### [](https://www.123fe.net/docs/base.html#_75-%E5%85%83%E7%B4%A0%E7%AB%96%E5%90%91%E7%9A%84%E7%99%BE%E5%88%86%E6%AF%94%E8%AE%BE%E5%AE%9A%E6%98%AF%E7%9B%B8%E5%AF%B9%E4%BA%8E%E5%AE%B9%E5%99%A8%E7%9A%84%E9%AB%98%E5%BA%A6%E5%90%97)<font style="color:rgb(44, 62, 80);">75 元素竖向的百分比设定是相对于容器的高度吗</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">元素竖向的百分比设定是相对于容器的宽度，而不是高度</font>

### [](https://www.123fe.net/docs/base.html#_76-%E5%85%A8%E5%B1%8F%E6%BB%9A%E5%8A%A8%E7%9A%84%E5%8E%9F%E7%90%86%E6%98%AF%E4%BB%80%E4%B9%88-%E7%94%A8%E5%88%B0%E4%BA%86css%E7%9A%84%E9%82%A3%E4%BA%9B%E5%B1%9E%E6%80%A7)<font style="color:rgb(44, 62, 80);">76 全屏滚动的原理是什么？ 用到了CSS的那些属性</font>
+ <font style="color:rgb(44, 62, 80);">原理类似图片轮播原理，超出隐藏部分，滚动时显示</font>
+ <font style="color:rgb(44, 62, 80);">可能用到的CSS属性：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">overflow:hidden; transform:translate(100%, 100%); display:none;</font>

### [](https://www.123fe.net/docs/base.html#_77-%E4%BB%80%E4%B9%88%E6%98%AF%E5%93%8D%E5%BA%94%E5%BC%8F%E8%AE%BE%E8%AE%A1-%E5%93%8D%E5%BA%94%E5%BC%8F%E8%AE%BE%E8%AE%A1%E7%9A%84%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86%E6%98%AF%E4%BB%80%E4%B9%88-%E5%A6%82%E4%BD%95%E5%85%BC%E5%AE%B9%E4%BD%8E%E7%89%88%E6%9C%AC%E7%9A%84ie)<font style="color:rgb(44, 62, 80);">77 什么是响应式设计？响应式设计的基本原理是什么？如何兼容低版本的IE</font>
+ <font style="color:rgb(44, 62, 80);">响应式设计就是网站能够兼容多个终端，而不是为每个终端做一个特定的版本</font>
+ <font style="color:rgb(44, 62, 80);">基本原理是利用CSS3媒体查询，为不同尺寸的设备适配不同样式</font>
+ <font style="color:rgb(44, 62, 80);">对于低版本的IE，可采用JS获取屏幕宽度，然后通过resize方法来实现兼容：</font>

```plain
$(window).resize(function () {
  screenRespond();
});
screenRespond();
function screenRespond(){
var screenWidth = $(window).width();
if(screenWidth <= 1800){
  $("body").attr("class", "w1800");
}
if(screenWidth <= 1400){
  $("body").attr("class", "w1400");
}
if(screenWidth > 1800){
  $("body").attr("class", "");
}
}
```

### [](https://www.123fe.net/docs/base.html#_78-%E4%BB%80%E4%B9%88%E6%98%AF%E8%A7%86%E5%B7%AE%E6%BB%9A%E5%8A%A8%E6%95%88%E6%9E%9C-%E5%A6%82%E4%BD%95%E7%BB%99%E6%AF%8F%E9%A1%B5%E5%81%9A%E4%B8%8D%E5%90%8C%E7%9A%84%E5%8A%A8%E7%94%BB)<font style="color:rgb(44, 62, 80);">78 什么是视差滚动效果，如何给每页做不同的动画</font>
+ <font style="color:rgb(44, 62, 80);">视差滚动是指多层背景以不同的速度移动，形成立体的运动效果，具有非常出色的视觉体验</font>
+ <font style="color:rgb(44, 62, 80);">一般把网页解剖为：背景层、内容层和悬浮层。当滚动鼠标滚轮时，各图层以不同速度移动，形成视差的</font>
+ <font style="color:rgb(44, 62, 80);">实现原理</font>
    - <font style="color:rgb(44, 62, 80);">以 “页面滚动条” 作为 “视差动画进度条”</font>
    - <font style="color:rgb(44, 62, 80);">以 “滚轮刻度” 当作 “动画帧度” 去播放动画的</font>
    - <font style="color:rgb(44, 62, 80);">监听 mousewheel 事件，事件被触发即播放动画，实现“翻页”效果</font>

### [](https://www.123fe.net/docs/base.html#_79-a%E6%A0%87%E7%AD%BE%E4%B8%8A%E5%9B%9B%E4%B8%AA%E4%BC%AA%E7%B1%BB%E7%9A%84%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F%E6%98%AF%E6%80%8E%E4%B9%88%E6%A0%B7%E7%9A%84)<font style="color:rgb(44, 62, 80);">79 a标签上四个伪类的执行顺序是怎么样的</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link > visited > hover > active</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">L-V-H-A</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">love hate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">用喜欢和讨厌两个词来方便记忆</font>

### [](https://www.123fe.net/docs/base.html#_80-%E4%BC%AA%E5%85%83%E7%B4%A0%E5%92%8C%E4%BC%AA%E7%B1%BB%E7%9A%84%E5%8C%BA%E5%88%AB%E5%92%8C%E4%BD%9C%E7%94%A8)<font style="color:rgb(44, 62, 80);">80 伪元素和伪类的区别和作用</font>
+ <font style="color:rgb(44, 62, 80);">伪元素 -- 在内容元素的前后插入额外的元素或样式，但是这些元素实际上并不在文档中生成。</font>
+ <font style="color:rgb(44, 62, 80);">它们只在外部显示可见，但不会在文档的源代码中找到它们，因此，称为“伪”元素。例如：</font>

```css
p::before {content:"第一章：";}
p::after {content:"Hot!";}
p::first-line {background:red;}
p::first-letter {font-size:30px;}
```

+ <font style="color:rgb(44, 62, 80);">伪类 -- 将特殊的效果添加到特定选择器上。它是已有元素上添加类别的，不会产生新的元素。例如：</font>

```css
a:hover {color: FF00FF}
p:first-child {color: red}
```

### [](https://www.123fe.net/docs/base.html#_81-before-%E5%92%8C-after-%E4%B8%AD%E5%8F%8C%E5%86%92%E5%8F%B7%E5%92%8C%E5%8D%95%E5%86%92%E5%8F%B7%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">81 ::before 和 :after 中双冒号和单冒号有什么区别</font>
+ <font style="color:rgb(44, 62, 80);">在 CSS 中伪类一直用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">表示，如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:hover</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:active</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">等</font>
+ <font style="color:rgb(44, 62, 80);">伪元素在CSS1中已存在，当时语法是用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">表示，如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:before</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:after</font>
+ <font style="color:rgb(44, 62, 80);">后来在CSS3中修订，伪元素用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">::</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">表示，如</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">::before</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">::after</font><font style="color:rgb(44, 62, 80);">，以此区分伪元素和伪类</font>
+ <font style="color:rgb(44, 62, 80);">由于低版本IE对双冒号不兼容，开发者为了兼容性各浏览器，继续使使用 :after 这种老语法表示伪元素</font>
+ <font style="color:rgb(44, 62, 80);">综上所述：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">::before</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中写伪元素的新语法；</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">:after</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中存在的、兼容IE的老语法</font>

### [](https://www.123fe.net/docs/base.html#_82-%E5%A6%82%E4%BD%95%E4%BF%AE%E6%94%B9chrome%E8%AE%B0%E4%BD%8F%E5%AF%86%E7%A0%81%E5%90%8E%E8%87%AA%E5%8A%A8%E5%A1%AB%E5%85%85%E8%A1%A8%E5%8D%95%E7%9A%84%E9%BB%84%E8%89%B2%E8%83%8C%E6%99%AF)<font style="color:rgb(44, 62, 80);">82 如何修改Chrome记住密码后自动填充表单的黄色背景</font>
+ <font style="color:rgb(44, 62, 80);">产生原因：由于Chrome默认会给自动填充的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">input</font><font style="color:rgb(44, 62, 80);">表单加上</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">input:-webkit-autofill</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">私有属性造成的</font>
+ <font style="color:rgb(44, 62, 80);">解决方案1：在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">form</font><font style="color:rgb(44, 62, 80);">标签上直接关闭了表单的自动填充：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">autocomplete="off"</font>
+ <font style="color:rgb(44, 62, 80);">解决方案2：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">input:-webkit-autofill { background-color: transparent; }</font>

**<font style="color:rgb(44, 62, 80);">input [type=search] 搜索框右侧小图标如何美化？</font>**

```css
input[type="search"]::-webkit-search-cancel-button{
  -webkit-appearance: none;
  height: 15px;
  width: 15px;
  border-radius: 8px;
  background:url("images/searchicon.png") no-repeat 0 0;
  background-size: 15px 15px;
}
```

### [](https://www.123fe.net/docs/base.html#_83-%E7%BD%91%E7%AB%99%E5%9B%BE%E7%89%87%E6%96%87%E4%BB%B6-%E5%A6%82%E4%BD%95%E7%82%B9%E5%87%BB%E4%B8%8B%E8%BD%BD-%E8%80%8C%E9%9D%9E%E7%82%B9%E5%87%BB%E9%A2%84%E8%A7%88)<font style="color:rgb(44, 62, 80);">83 网站图片文件，如何点击下载？而非点击预览</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><a href="logo.jpg" download>下载</a></font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><a href="logo.jpg" download="网站LOGO" >下载</a></font>

### [](https://www.123fe.net/docs/base.html#_63-ios-safari-%E5%A6%82%E4%BD%95%E9%98%BB%E6%AD%A2-%E6%A9%A1%E7%9A%AE%E7%AD%8B%E6%95%88%E6%9E%9C)<font style="color:rgb(44, 62, 80);">63 iOS safari 如何阻止“橡皮筋效果”</font>
```plain
$(document).ready(function(){
  var stopScrolling = function(event) {
    event.preventDefault();
  }
  document.addEventListener('touchstart', stopScrolling, false);
  document.addEventListener('touchmove', stopScrolling, false);
});
```

### [](https://www.123fe.net/docs/base.html#_84-%E4%BD%A0%E5%AF%B9-line-height-%E6%98%AF%E5%A6%82%E4%BD%95%E7%90%86%E8%A7%A3%E7%9A%84)<font style="color:rgb(44, 62, 80);">84 你对 line-height 是如何理解的</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">line-height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指一行字的高度，包含了字间距，实际上是下一行基线到上一行基线距离</font>
+ <font style="color:rgb(44, 62, 80);">如果一个标签没有定义</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，那么其最终表现的高度是由</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">line-height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">决定的</font>
+ <font style="color:rgb(44, 62, 80);">一个容器没有设置高度，那么撑开容器高度的是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">line-height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">而不是容器内的文字内容</font>
+ <font style="color:rgb(44, 62, 80);">把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">line-height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值设置为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">一样大小的值可以实现单行文字的垂直居中</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">line-height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">都能撑开一个高度，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会触发</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">haslayout</font><font style="color:rgb(44, 62, 80);">，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">line-height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不会</font>

### [](https://www.123fe.net/docs/base.html#_85-line-height-%E4%B8%89%E7%A7%8D%E8%B5%8B%E5%80%BC%E6%96%B9%E5%BC%8F%E6%9C%89%E4%BD%95%E5%8C%BA%E5%88%AB-%E5%B8%A6%E5%8D%95%E4%BD%8D%E3%80%81%E7%BA%AF%E6%95%B0%E5%AD%97%E3%80%81%E7%99%BE%E5%88%86%E6%AF%94)<font style="color:rgb(44, 62, 80);">85 line-height 三种赋值方式有何区别？（带单位、纯数字、百分比）</font>
+ <font style="color:rgb(44, 62, 80);">带单位：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是固定值，而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会参考父元素</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-size</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值计算自身的行高</font>
+ <font style="color:rgb(44, 62, 80);">纯数字：会把比例传递给后代。例如，父级行高为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1.5</font><font style="color:rgb(44, 62, 80);">，子元素字体为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">18px</font><font style="color:rgb(44, 62, 80);">，则子元素行高为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1.5 * 18 = 27px</font>
+ <font style="color:rgb(44, 62, 80);">百分比：将计算后的值传递给后代</font>

### [](https://www.123fe.net/docs/base.html#_86-%E8%AE%BE%E7%BD%AE%E5%85%83%E7%B4%A0%E6%B5%AE%E5%8A%A8%E5%90%8E-%E8%AF%A5%E5%85%83%E7%B4%A0%E7%9A%84-display-%E5%80%BC%E4%BC%9A%E5%A6%82%E4%BD%95%E5%8F%98%E5%8C%96)<font style="color:rgb(44, 62, 80);">86 设置元素浮动后，该元素的 display 值会如何变化</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">设置元素浮动后，该元素的</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">值自动变成</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">block</font>

### [](https://www.123fe.net/docs/base.html#_87-%E8%AE%A9%E9%A1%B5%E9%9D%A2%E9%87%8C%E7%9A%84%E5%AD%97%E4%BD%93%E5%8F%98%E6%B8%85%E6%99%B0-%E5%8F%98%E7%BB%86%E7%94%A8css%E6%80%8E%E4%B9%88%E5%81%9A-ios%E6%89%8B%E6%9C%BA%E6%B5%8F%E8%A7%88%E5%99%A8%E5%AD%97%E4%BD%93%E9%BD%BF%E8%BD%AE%E8%AE%BE%E7%BD%AE)<font style="color:rgb(44, 62, 80);">87 让页面里的字体变清晰，变细用CSS怎么做？（IOS手机浏览器字体齿轮设置）</font>
```css
-webkit-font-smoothing: antialiased;
```

### [](https://www.123fe.net/docs/base.html#_88-font-style-%E5%B1%9E%E6%80%A7-oblique-%E6%98%AF%E4%BB%80%E4%B9%88%E6%84%8F%E6%80%9D)<font style="color:rgb(44, 62, 80);">88 font-style 属性 oblique 是什么意思</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-style: oblique;</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">使没有</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">italic</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性的文字实现倾斜</font>

### [](https://www.123fe.net/docs/base.html#_89-display-inline-block-%E4%BB%80%E4%B9%88%E6%97%B6%E5%80%99%E4%BC%9A%E6%98%BE%E7%A4%BA%E9%97%B4%E9%9A%99)<font style="color:rgb(44, 62, 80);">89 display:inline-block 什么时候会显示间隙</font>
+ <font style="color:rgb(44, 62, 80);">相邻的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-block</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素之间有换行或空格分隔的情况下会产生间距</font>
+ <font style="color:rgb(44, 62, 80);">非</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-block</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">水平元素设置为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-block</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">也会有水平间距</font>
+ <font style="color:rgb(44, 62, 80);">可以借助</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vertical-align:top;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">消除垂直间隙</font>
+ <font style="color:rgb(44, 62, 80);">可以在父级加</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-size：0;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在子元素里设置需要的字体大小，消除垂直间隙</font>
+ <font style="color:rgb(44, 62, 80);">把</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">li</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">标签写到同一行可以消除垂直间隙，但代码可读性差</font>

### [](https://www.123fe.net/docs/base.html#_90-%E4%B8%80%E4%B8%AA%E9%AB%98%E5%BA%A6%E8%87%AA%E9%80%82%E5%BA%94%E7%9A%84div-%E9%87%8C%E9%9D%A2%E6%9C%89%E4%B8%A4%E4%B8%AAdiv-%E4%B8%80%E4%B8%AA%E9%AB%98%E5%BA%A6100px-%E5%B8%8C%E6%9C%9B%E5%8F%A6%E4%B8%80%E4%B8%AA%E5%A1%AB%E6%BB%A1%E5%89%A9%E4%B8%8B%E7%9A%84%E9%AB%98%E5%BA%A6)<font style="color:rgb(44, 62, 80);">90 一个高度自适应的div，里面有两个div，一个高度100px，希望另一个填满剩下的高度</font>
+ <font style="color:rgb(44, 62, 80);">方案1：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.sub { height: calc(100%-100px); }</font>
+ <font style="color:rgb(44, 62, 80);">方案2：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.container { position:relative; }</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.sub { position: absolute; top: 100px; bottom: 0; }</font>
+ <font style="color:rgb(44, 62, 80);">方案3：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.container { display:flex; flex-direction:column; }</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.sub { flex:1; }</font>

### [](https://www.123fe.net/docs/base.html#_91-css-%E7%9A%84%E6%B8%B2%E6%9F%93%E5%B1%82%E5%90%88%E6%88%90%E6%98%AF%E4%BB%80%E4%B9%88-%E6%B5%8F%E8%A7%88%E5%99%A8%E5%A6%82%E4%BD%95%E5%88%9B%E5%BB%BA%E6%96%B0%E7%9A%84%E6%B8%B2%E6%9F%93%E5%B1%82)<font style="color:rgb(44, 62, 80);">91 css 的渲染层合成是什么 浏览器如何创建新的渲染层</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">在 DOM 树中每个节点都会对应一个渲染对象（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">RenderObject），当它们的渲染对象处于相同的坐标空间（z 轴空间）时，就会形成一个 RenderLayers，也就是渲染层。渲染层将保证页面元素以正确的顺序堆叠，这时候就会出现层合成（</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">composite`），从而正确处理透明元素和重叠元素的显示。对于有位置重叠的元素的页面，这个过程尤其重要，因为一旦图层的合并顺序出错，将会导致元素显示异常</font>

**<font style="color:rgb(44, 62, 80);">浏览器如何创建新的渲染层</font>**

+ <font style="color:rgb(44, 62, 80);">根元素</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">document</font>
+ <font style="color:rgb(44, 62, 80);">有明确的定位属性（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">relative</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fixed</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">sticky</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">absolute</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">opacity < 1</font>
+ <font style="color:rgb(44, 62, 80);">有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS fliter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性</font>
+ <font style="color:rgb(44, 62, 80);">有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS mask</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性</font>
+ <font style="color:rgb(44, 62, 80);">有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS mix-blend-mode</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性且值不为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">normal</font>
+ <font style="color:rgb(44, 62, 80);">有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS transform</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性且值不为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">none</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">backface-visibility</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hidden</font>
+ <font style="color:rgb(44, 62, 80);">有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS reflection</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性</font>
+ <font style="color:rgb(44, 62, 80);">有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS column-count</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性且值不为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS column-width</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性且值不为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font>
+ <font style="color:rgb(44, 62, 80);">当前有对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">opacity</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fliter</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">backdrop-filter</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">应用动画</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">overflow</font><font style="color:rgb(44, 62, 80);"> 不为 </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visible</font>

