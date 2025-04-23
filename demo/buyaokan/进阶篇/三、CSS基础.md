### <font style="color:rgb(44, 62, 80);">1 盒模型</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">content（元素内容） + padding（内边距） + border（边框） + margin（外边距）</font>

<font style="color:rgb(44, 62, 80);">延伸：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content-box</font><font style="color:rgb(44, 62, 80);">：默认值，总宽度 =</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-box</font><font style="color:rgb(44, 62, 80);">：盒子宽度包含</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">总宽度 = margin + width</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inherit</font><font style="color:rgb(44, 62, 80);">：从父元素继承</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性</font>

### [](https://www.123fe.net/docs/base/improve.html#_2-bfc)<font style="color:rgb(44, 62, 80);">2 BFC</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">块级格式化上下文，是一个独立的渲染区域，让处于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">内部的元素与外部的元素相互隔离，使内外元素的定位不会相互影响。</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">IE下为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Layout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，可通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">zoom:1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">触发</font>

**<font style="color:rgb(44, 62, 80);">触发条件:</font>**

+ <font style="color:rgb(44, 62, 80);">根元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position: absolute/fixed</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display: inline-block / table</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ovevflow !== visible</font>

**<font style="color:rgb(44, 62, 80);">规则:</font>**

+ <font style="color:rgb(44, 62, 80);">属于同一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的两个相邻</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">垂直排列</font>
+ <font style="color:rgb(44, 62, 80);">属于同一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的两个相邻</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会发生重叠</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中子元素的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的左边， 与包含块 (BFC)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border box</font><font style="color:rgb(44, 62, 80);">的左边相接触 (子元素</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">absolute</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">除外)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的区域不会与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的元素区域重叠</font>
+ <font style="color:rgb(44, 62, 80);">计算</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的高度时，浮动子元素也参与计算</font>
+ <font style="color:rgb(44, 62, 80);">文字层不会被浮动层覆盖，环绕于周围</font>

**<font style="color:rgb(44, 62, 80);">应用:</font>**

+ <font style="color:rgb(44, 62, 80);">阻止</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">重叠</font>
+ <font style="color:rgb(44, 62, 80);">可以包含浮动元素 —— 清除内部浮动(清除浮动的原理是两个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">都位于同一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">区域之中)</font>
+ <font style="color:rgb(44, 62, 80);">自适应两栏布局</font>
+ <font style="color:rgb(44, 62, 80);">可以阻止元素被浮动元素覆盖</font>

### [](https://www.123fe.net/docs/base/improve.html#_3-%E5%B1%82%E5%8F%A0%E4%B8%8A%E4%B8%8B%E6%96%87)<font style="color:rgb(44, 62, 80);">3 层叠上下文</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">元素提升为一个比较特殊的图层，在三维空间中 (z轴) 高出普通元素一等。</font>

**<font style="color:rgb(44, 62, 80);">触发条件</font>**

+ <font style="color:rgb(44, 62, 80);">根层叠上下文(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css3</font><font style="color:rgb(44, 62, 80);">属性</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">opacity</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">filter</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">will-change</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">webkit-overflow-scrolling</font>

**<font style="color:rgb(44, 62, 80);">层叠等级：层叠上下文在z轴上的排序</font>**

+ <font style="color:rgb(44, 62, 80);">在同一层叠上下文中，层叠等级才有意义</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">z-index</font><font style="color:rgb(44, 62, 80);">的优先级最高</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718781425142-538358a9-e177-477c-a5b6-36249f3adaac.png)

### [](https://www.123fe.net/docs/base/improve.html#_4-%E5%B7%A6%E5%8F%B3%E5%B1%85%E4%B8%AD%E6%96%B9%E6%A1%88)<font style="color:rgb(44, 62, 80);">4 左右居中方案</font>
+ <font style="color:rgb(44, 62, 80);">行内元素:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-align: center</font>
+ <font style="color:rgb(44, 62, 80);">定宽块状元素: 左右</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font>
+ <font style="color:rgb(44, 62, 80);">不定宽块状元素:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">table</font><font style="color:rgb(44, 62, 80);">布局，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position + transform</font>

```css
/* 方案1 */
.wrap {
  text-align: center
}
.center {
  display: inline;
  /* or */
  /* display: inline-block; */
}
/* 方案2 */
.center {
  width: 100px;
  margin: 0 auto;
}
/* 方案2 */
.wrap {
  position: relative;
}
.center {
  position: absulote;
  left: 50%;
  transform: translateX(-50%);
}
```

### [](https://www.123fe.net/docs/base/improve.html#_5-%E4%B8%8A%E4%B8%8B%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD%E6%96%B9%E6%A1%88)<font style="color:rgb(44, 62, 80);">5 上下垂直居中方案</font>
+ <font style="color:rgb(44, 62, 80);">定高：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position + margin</font><font style="color:rgb(44, 62, 80);">(负值)</font>
+ <font style="color:rgb(44, 62, 80);">不定高：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IFC + vertical-align:middle</font>

```css
/* 定高方案1 */
.center {
  height: 100px;
  margin: 50px 0;   
}
/* 定高方案2 */
.center {
  height: 100px;
  position: absolute;
  top: 50%;
  margin-top: -25px;
}
/* 不定高方案1 */
.center {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
/* 不定高方案2 */
.wrap {
  display: flex;
  align-items: center;
}
.center {
  width: 100%;
}
/* 不定高方案3 */
/* 设置 inline-block 则会在外层产生 IFC，高度设为 100% 撑开 wrap 的高度 */
.wrap::before {
  content: '';
  height: 100%;
  display: inline-block;
  vertical-align: middle;
}
.wrap {
  text-align: center;
}
.center {
  display: inline-block;  
  vertical-align: middle;
}
```

### [](https://www.123fe.net/docs/base/improve.html#_6-%E9%80%89%E6%8B%A9%E5%99%A8%E6%9D%83%E9%87%8D%E8%AE%A1%E7%AE%97%E6%96%B9%E5%BC%8F)<font style="color:rgb(44, 62, 80);">6 选择器权重计算方式</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">!important > 内联样式 = 外联样式 > ID选择器 > 类选择器 = 伪类选择器 = 属性选择器 > 元素选择器 = 伪元素选择器 > 通配选择器 = 后代选择器 = 兄弟选择器</font>

1. <font style="color:rgb(44, 62, 80);">属性后面加</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">!import</font><font style="color:rgb(44, 62, 80);">会覆盖页面内任何位置定义的元素样式</font>
2. <font style="color:rgb(44, 62, 80);">作为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">style</font><font style="color:rgb(44, 62, 80);">属性写在元素内的样式</font>
3. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">id</font><font style="color:rgb(44, 62, 80);">选择器</font>
4. <font style="color:rgb(44, 62, 80);">类选择器</font>
5. <font style="color:rgb(44, 62, 80);">标签选择器</font>
6. <font style="color:rgb(44, 62, 80);">通配符选择器（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">*</font><font style="color:rgb(44, 62, 80);">）</font>
7. <font style="color:rgb(44, 62, 80);">浏览器自定义或继承</font>

**<font style="color:rgb(44, 62, 80);">同一级别：后写的会覆盖先写的</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">css选择器的解析原则：选择器定位DOM元素是从右往左的方向，这样可以尽早的过滤掉一些不必要的样式规则和元素</font>

### [](https://www.123fe.net/docs/base/improve.html#_7-%E6%B8%85%E9%99%A4%E6%B5%AE%E5%8A%A8)<font style="color:rgb(44, 62, 80);">7 清除浮动</font>
1. <font style="color:rgb(44, 62, 80);">在浮动元素后面添加</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clear:both</font><font style="color:rgb(44, 62, 80);">的空</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素</font>

```html
<div class="container">
    <div class="left"></div>
    <div class="right"></div>
    <div style="clear:both"></div>
</div>
```

1. <font style="color:rgb(44, 62, 80);">给父元素添加</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">overflow:hidden</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">或者</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">样式，触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font>

```html
<div class="container">
    <div class="left"></div>
    <div class="right"></div>
</div>
```

```css
.container{
    width: 300px;
    background-color: aaa;
    overflow:hidden;
    zoom:1;   /*IE6*/
}
```

1. <font style="color:rgb(44, 62, 80);">使用伪元素，也是在元素末尾添加一个点并带有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clear: both</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的元素实现的。</font>

```html
<div class="container clearfix">
    <div class="left"></div>
    <div class="right"></div>
</div>
```

```css
.clearfix{
    zoom: 1; /*IE6*/
}
.clearfix:after{
    content: ".";
    height: 0;
    clear: both;
    display: block;
    visibility: hidden;
}
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">推荐使用第三种方法，不会在页面新增div，文档结构更加清晰</font>

### [](https://www.123fe.net/docs/base/improve.html#_8-%E5%B7%A6%E8%BE%B9%E5%AE%9A%E5%AE%BD-%E5%8F%B3%E8%BE%B9%E8%87%AA%E9%80%82%E5%BA%94%E6%96%B9%E6%A1%88)<font style="color:rgb(44, 62, 80);">8 左边定宽，右边自适应方案</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">float + margin，float + calc</font>

```css
/* 方案1 */ 
.left {
  width: 120px;
  float: left;
}
.right {
  margin-left: 120px;
}
/* 方案2 */ 
.left {
  width: 120px;
  float: left;
}
.right {
  width: calc(100% - 120px);
  float: left;
}
```

### [](https://www.123fe.net/docs/base/improve.html#_9-%E5%B7%A6%E5%8F%B3%E4%B8%A4%E8%BE%B9%E5%AE%9A%E5%AE%BD-%E4%B8%AD%E9%97%B4%E8%87%AA%E9%80%82%E5%BA%94)<font style="color:rgb(44, 62, 80);">9 左右两边定宽，中间自适应</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">float，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float + calc</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">, 圣杯布局（设置BFC，margin负值法），flex</font>

```css
.wrap {
  width: 100%;
  height: 200px;
}
.wrap > div {
  height: 100%;
}
/* 方案1 */
.left {
  width: 120px;
  float: left;
}
.right {
  float: right;
  width: 120px;
}
.center {
  margin: 0 120px; 
}
/* 方案2 */
.left {
  width: 120px;
  float: left;
}
.right {
  float: right;
  width: 120px;
}
.center {
  width: calc(100% - 240px);
  margin-left: 120px;
}
/* 方案3 */
.wrap {
  display: flex;
}
.left {
  width: 120px;
}
.right {
  width: 120px;
}
.center {
  flex: 1;
}
```

### [](https://www.123fe.net/docs/base/improve.html#_10-css%E5%8A%A8%E7%94%BB%E5%92%8C%E8%BF%87%E6%B8%A1)<font style="color:rgb(44, 62, 80);">10 CSS动画和过渡</font>
**<font style="color:rgb(44, 62, 80);">animation / keyframes</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-name</font><font style="color:rgb(44, 62, 80);">: 动画名称，对应</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@keyframes</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-duration</font><font style="color:rgb(44, 62, 80);">: 间隔</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-timing-function</font><font style="color:rgb(44, 62, 80);">: 曲线</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-delay</font><font style="color:rgb(44, 62, 80);">: 延迟</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-iteration-count</font><font style="color:rgb(44, 62, 80);">: 次数</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">infinite</font><font style="color:rgb(44, 62, 80);">: 循环动画</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-direction</font><font style="color:rgb(44, 62, 80);">: 方向</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">alternate</font><font style="color:rgb(44, 62, 80);">: 反向播放</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-fill-mode</font><font style="color:rgb(44, 62, 80);">: 静止模式</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwards</font><font style="color:rgb(44, 62, 80);">: 停止时，保留最后一帧</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">backwards</font><font style="color:rgb(44, 62, 80);">: 停止时，回到第一帧</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">both</font><font style="color:rgb(44, 62, 80);">: 同时运用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwards / backwards</font>
+ <font style="color:rgb(44, 62, 80);">常用钩子:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animationend</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">动画属性: 尽量使用动画属性进行动画，能拥有较好的性能表现</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">translate</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scale</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rotate</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">skew</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">opacity</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">color</font>

**<font style="color:rgb(44, 62, 80);">transform</font>**

+ <font style="color:rgb(44, 62, 80);">位移属性</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">translate( x , y )</font>
+ <font style="color:rgb(44, 62, 80);">旋转属性</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rotate()</font>
+ <font style="color:rgb(44, 62, 80);">缩放属性</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scale()</font>
+ <font style="color:rgb(44, 62, 80);">倾斜属性</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">skew()</font>

**<font style="color:rgb(44, 62, 80);">transition</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-property</font><font style="color:rgb(44, 62, 80);">（过渡的属性的名称）。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-duration</font><font style="color:rgb(44, 62, 80);">（定义过渡效果花费的时间,默认是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">）。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-timing-function:linear(匀速) ease</font><font style="color:rgb(44, 62, 80);">(慢速开始，然后变快，然后慢速结束)（规定过渡效果的时间曲线，最常用的是这两个）。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-delay</font><font style="color:rgb(44, 62, 80);">（规定过渡效果何时开始。默认是 0）</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">般情况下，我们都是写一起的，比如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition： width 2s ease 1s</font>

**<font style="color:rgb(44, 62, 80);">关键帧动画animation</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">一个关键帧动画，最少包含两部分，animation 属性及属性值（动画的名称和运行方式运行时间等）。@keyframes（规定动画的具体实现过程）</font>

**<font style="color:rgb(44, 62, 80);">animation 属性可以拆分为</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-name</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定@keyframes 动画的名称。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-duration</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定动画完成一个周期所花费的秒或毫秒。默认是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-timing-function</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定动画的速度曲线。默认是 “ease”，常用的还有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">linear</font><font style="color:rgb(44, 62, 80);">，同</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transtion</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-delay</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定动画何时开始。默认是 0。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-iteration-count</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">规定动画被播放的次数。默认是 1，但我们一般用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">infinite</font><font style="color:rgb(44, 62, 80);">，一直播放</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@keyframes</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的使用方法，可以是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">from->to</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（等同于0%和100%），也可以是从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0%->100%</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之间任意个的分层设置。我们通过下面一个稍微复杂点的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">demo</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">来看一下，基本上用到了上面说到的大部分知识</font>

```css
eg:
   @keyframes mymove
  {
      from {top:0px;}
      to {top:200px;}
  }
 
/* 等同于： */
 
@keyframes mymove
{
 0%   {top:0px;}
 25%  {top:200px;}
 50%  {top:100px;}
 75%  {top:200px;}
 100% {top:0px;}
}
```

**<font style="color:rgb(44, 62, 80);">用css3动画使一个图片旋转</font>**

```css
loader {

    display: block;

    position: relative;

    -webkit-animation: spin 2s linear infinite;

    animation: spin 2s linear infinite;

}

@-webkit-keyframes spin {

    0%   {

        -webkit-transform: rotate(0deg);

        -ms-transform: rotate(0deg);

        transform: rotate(0deg);

    }

    100% {

        -webkit-transform: rotate(360deg);

        -ms-transform: rotate(360deg);

        transform: rotate(360deg);

    }

}

@keyframes spin {

    0%   {

        -webkit-transform: rotate(0deg);

        -ms-transform: rotate(0deg);

        transform: rotate(0deg);

    }

    100% {

        -webkit-transform: rotate(360deg);

        -ms-transform: rotate(360deg);

        transform: rotate(360deg);

    }

}
```

### [](https://www.123fe.net/docs/base/improve.html#_11-css3%E7%9A%84%E6%96%B0%E7%89%B9%E6%80%A7)<font style="color:rgb(44, 62, 80);">11 CSS3的新特性</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(44, 62, 80);">：过渡</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(44, 62, 80);">: 旋转、缩放、移动或倾斜</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(44, 62, 80);">: 动画</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">gradient</font><font style="color:rgb(44, 62, 80);">: 渐变</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-shadow</font><font style="color:rgb(44, 62, 80);">: 阴影</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-radius</font><font style="color:rgb(44, 62, 80);">: 圆角</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">word-break</font><font style="color:rgb(44, 62, 80);">:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">normal|break-all|keep-all</font><font style="color:rgb(44, 62, 80);">; 文字换行(默认规则|单词也可以换行|只在半角空格或连字符换行)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-overflow</font><font style="color:rgb(44, 62, 80);">: 文字超出部分处理</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-shadow</font><font style="color:rgb(44, 62, 80);">: 水平阴影，垂直阴影，模糊的距离，以及阴影的颜色。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing</font><font style="color:rgb(44, 62, 80);">:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content-box|border-box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">盒模型</font>
+ <font style="color:rgb(44, 62, 80);">媒体查询</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@media screen and (max-width: 960px) {}</font><font style="color:rgb(44, 62, 80);">还有打印</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">print</font>

### [](https://www.123fe.net/docs/base/improve.html#_12-%E5%88%97%E4%B8%BE%E5%87%A0%E4%B8%AAcss%E4%B8%AD%E5%8F%AF%E7%BB%A7%E6%89%BF%E5%92%8C%E4%B8%8D%E5%8F%AF%E7%BB%A7%E6%89%BF%E7%9A%84%E5%85%83%E7%B4%A0)<font style="color:rgb(44, 62, 80);">12 列举几个css中可继承和不可继承的元素</font>
+ <font style="color:rgb(44, 62, 80);">不可继承的：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display、margin、border、padding、background、height、min-height、max-height、width、min-width、max-width、overflow、position、left、right、top、bottom、z-index、float、clear、table-layout、vertical-align</font>
+ <font style="color:rgb(44, 62, 80);">所有元素可继承：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cursor</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">内联元素可继承：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">letter-spacing、word-spacing、white-space、line-height、color、font、font-family、font-size、font-style、font-variant、font-weight、text-decoration、text-transform、direction</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">终端块状元素可继承：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-indent和text-align</font><font style="color:rgb(44, 62, 80);">。</font>
+ <font style="color:rgb(44, 62, 80);">列表元素可继承：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">list-style、list-style-type、list-style-position、list-style-imag</font><font style="color:rgb(44, 62, 80);">e`。</font>

**<font style="color:rgb(44, 62, 80);">transition和animation的区别</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Animation</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">大部分属性是相同的，他们都是随时间改变元素的属性值，他们的主要区别是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">需要触发一个事件才能改变属性，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不需要触发任何事件的情况下才会随时间改变属性值，并且</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为2帧，从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">from .... to</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以一帧一帧的</font>

