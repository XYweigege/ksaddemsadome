### <font style="color:rgb(44, 62, 80);">盒模型</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">有两种，</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">盒子模型、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">W3C</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">盒子模型；</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">盒模型： 内容(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)、填充(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)、边界(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)、 边框(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">)；</font>
+ <font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">区 别：</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">部分把</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">计算了进去;</font>

**<font style="color:rgb(44, 62, 80);">标准盒子模型的模型图</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783165749-04d3a849-aebd-4d63-a0f4-792e2428737f.png)

<font style="color:rgb(44, 62, 80);">从上图可以看到：</font>

+ <font style="color:rgb(44, 62, 80);">盒子总宽度 =</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">;</font>
+ <font style="color:rgb(44, 62, 80);">盒子总高度 =</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font>

<font style="color:rgb(44, 62, 80);">也就是，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width/height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">只是内容高度，不包含</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值</font>

**<font style="color:rgb(44, 62, 80);">IE 怪异盒子模型</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783166562-eb17f03a-f6fd-4b56-b109-c64c9fc40bfe.png)

<font style="color:rgb(44, 62, 80);">从上图可以看到：</font>

+ <font style="color:rgb(44, 62, 80);">盒子总宽度 =</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">;</font>
+ <font style="color:rgb(44, 62, 80);">盒子总高度 =</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">+</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">;</font>

<font style="color:rgb(44, 62, 80);">也就是，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width/height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">包含了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">值</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">页面渲染时，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">dom</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">元素所采用的 布局模型。可通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">进行设置</font>

**<font style="color:rgb(44, 62, 80);">通过 box-sizing 来改变元素的盒模型</font>**

<font style="color:rgb(44, 62, 80);">CSS 中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性定义了引擎应该如何计算一个元素的总宽度和总高度</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing: content-box;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">默认的标准(W3C)盒模型元素效果，元素的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width/height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">不包含</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">，与标准盒子模型表现一致</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing: border-box;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">触发怪异(IE)盒模型元素的效果，元素的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width/height</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">包含</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">，与怪异盒子模型表现一致</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing: inherit;</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">继承父元素</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性的值</font>

**<font style="color:rgb(44, 62, 80);">小结</font>**

+ <font style="color:rgb(44, 62, 80);">盒子模型构成：内容(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font><font style="color:rgb(44, 62, 80);">)、内填充(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);">)、 边框(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">)、外边距(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">)</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE8</font><font style="color:rgb(44, 62, 80);">及其以下版本浏览器，未声明</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">DOCTYPE</font><font style="color:rgb(44, 62, 80);">，内容宽高会包含内填充和边框，称为怪异盒模型(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">盒模型)</font>
+ <font style="color:rgb(44, 62, 80);">标准(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">W3C</font><font style="color:rgb(44, 62, 80);">)盒模型：元素宽度 =</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width + padding + border + margin</font>
+ <font style="color:rgb(44, 62, 80);">怪异(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(44, 62, 80);">)盒模型：元素宽度 =</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">width + margin</font>
+ <font style="color:rgb(44, 62, 80);">标准浏览器通过设置 css3 的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-sizing: border-box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性，触发“怪异模式”解析计算宽高</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#bfc)<font style="color:rgb(44, 62, 80);">BFC</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">块级格式化上下文，是一个独立的渲染区域，让处于</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">内部的元素与外部的元素相互隔离，使内外元素的定位不会相互影响。</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">IE</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">下为</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Layout</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，可通过</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">zoom:1</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">触发</font>

**<font style="color:rgb(44, 62, 80);">触发条件:</font>**

+ <font style="color:rgb(44, 62, 80);">根元素，即HTML元素</font>
+ <font style="color:rgb(44, 62, 80);">绝对定位元素</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position: absolute/fixed</font>
+ <font style="color:rgb(44, 62, 80);">行内块元素</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">的值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-block</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">table</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-flex</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">grid</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-grid</font>
+ <font style="color:rgb(44, 62, 80);">浮动元素：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);">值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">right</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">overflow值</font><font style="color:rgb(44, 62, 80);">不为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visible</font><font style="color:rgb(44, 62, 80);">，为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scroll</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hidden</font>

**<font style="color:rgb(44, 62, 80);">规则:</font>**

1. <font style="color:rgb(44, 62, 80);">属于同一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的两个相邻</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">垂直排列</font>
2. <font style="color:rgb(44, 62, 80);">属于同一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的两个相邻</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会发生重叠</font>
3. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中子元素的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin box</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的左边， 与包含块 (BFC)</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border box</font><font style="color:rgb(44, 62, 80);">的左边相接触 (子元素</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">absolute</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">除外)</font>
4. <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的区域不会与</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的元素区域重叠</font>
5. <font style="color:rgb(44, 62, 80);">计算</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的高度时，浮动子元素也参与计算</font>
6. <font style="color:rgb(44, 62, 80);">文字层不会被浮动层覆盖，环绕于周围</font>

**<font style="color:rgb(44, 62, 80);">应用:</font>**

+ <font style="color:rgb(44, 62, 80);">利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">2</font><font style="color:rgb(44, 62, 80);">：阻止</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">重叠</font>
+ <font style="color:rgb(44, 62, 80);">利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">4</font><font style="color:rgb(44, 62, 80);">：自适应两栏布局</font>
+ <font style="color:rgb(44, 62, 80);">利用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">5</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，可以避免高度塌陷</font>
+ <font style="color:rgb(44, 62, 80);">可以包含浮动元素 —— 清除内部浮动(清除浮动的原理是两个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">div</font><font style="color:rgb(44, 62, 80);">都位于同一个</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">区域之中)</font>

**<font style="color:rgb(44, 62, 80);">示例</font>**

**<font style="color:rgb(44, 62, 80);">1. 防止margin重叠（塌陷）</font>**

```html
<style>
    p {
      color: f55;
      background: fcc;
      width: 200px;
      line-height: 100px;
      text-align:center;
      margin: 100px;
    }
</style>
<body>
  <p>Haha</p >
  <p>Hehe</p >
</body>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783165900-38f928e1-0c67-4d2a-bed4-9a60c0320087.png)

+ <font style="color:rgb(44, 62, 80);">两个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p</font><font style="color:rgb(44, 62, 80);">元素之间的距离为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100px</font><font style="color:rgb(44, 62, 80);">，发生了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">重叠（塌陷），以最大的为准，如果第一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">P</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">80</font><font style="color:rgb(44, 62, 80);">的话，两个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">P</font><font style="color:rgb(44, 62, 80);">之间的距离还是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100</font><font style="color:rgb(44, 62, 80);">，以最大的为准。</font>
+ <font style="color:rgb(44, 62, 80);">同一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">的俩个相邻的盒子的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">会发生重叠</font>
+ <font style="color:rgb(44, 62, 80);">可以在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p</font><font style="color:rgb(44, 62, 80);">外面包裹一层容器，并触发这个容器生成一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">，那么两个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p</font><font style="color:rgb(44, 62, 80);">就不属于同一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">，则不会出现</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">重叠</font>

```html
<style>
    .wrap {
        overflow: hidden;// 新的BFC
    }
    p {
        color: f55;
        background: fcc;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 100px;
    }
</style>
<body>
    <p>Haha</p >
    <div class="wrap">
        <p>Hehe</p >
    </div>
</body>
```

<font style="color:rgb(44, 62, 80);">这时候，边距则不会重叠：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783166477-8b8ed9fd-2381-4999-bbb3-5a11de619b58.png)

**<font style="color:rgb(44, 62, 80);">2. 清除内部浮动</font>**

```html
<style>
    .par {
        border: 5px solid fcc;
        width: 300px;
    }
 
    .child {
        border: 5px solid f66;
        width:100px;
        height: 100px;
        float: left;
    }
</style>
<body>
    <div class="par">
      <div class="child"></div>
      <div class="child"></div>
    </div>
</body>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783166213-5aad7c4c-2ac3-4301-b15c-2e59cd5c758e.png)

<font style="color:rgb(44, 62, 80);">而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">在计算高度时，浮动元素也会参与，所以我们可以触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.par</font><font style="color:rgb(44, 62, 80);">元素生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">，则内部浮动元素计算高度时候也会计算</font>

```css
.par {
    overflow: hidden;
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783166810-780527f7-5382-406c-a3b1-8381722f23d9.png)

**<font style="color:rgb(44, 62, 80);">3. 自适应多栏布局</font>**

<font style="color:rgb(44, 62, 80);">这里举个两栏的布局</font>

```html
<style>
    body {
        width: 300px;
        position: relative;
    }
 
    .aside {
        width: 100px;
        height: 150px;
        float: left;
        background: f66;
    }
 
    .main {
        height: 200px;
        background: fcc;
    }
</style>
<body>
    <div class="aside"></div>
    <div class="main"></div>
</body>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783167146-4c053b4d-9301-43b4-8944-40b204348e15.png)

+ <font style="color:rgb(44, 62, 80);">每个元素的左外边距与包含块的左边界相接触</font>
+ <font style="color:rgb(44, 62, 80);">因此，虽然</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.aslide</font><font style="color:rgb(44, 62, 80);">为浮动元素，但是main的左边依然会与包含块的左边相接触，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">的区域不会与浮动盒子重叠</font>
+ <font style="color:rgb(44, 62, 80);">所以我们可以通过触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">main</font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">，以此适应两栏布局</font>

```css
.main {
  overflow: hidden;
}
```

<font style="color:rgb(44, 62, 80);">这时候，新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">不会与浮动的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.aside</font><font style="color:rgb(44, 62, 80);">元素重叠。因此会根据包含块的宽度，和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.aside</font><font style="color:rgb(44, 62, 80);">的宽度，自动变窄</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783166528-646530eb-3ecf-4863-a6e2-b7e29543815d.png)

### [](https://www.123fe.net/docs/base/high-frequency.html#%E9%80%89%E6%8B%A9%E5%99%A8%E6%9D%83%E9%87%8D%E8%AE%A1%E7%AE%97%E6%96%B9%E5%BC%8F)<font style="color:rgb(44, 62, 80);">选择器权重计算方式</font>
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

### [](https://www.123fe.net/docs/base/high-frequency.html#%E6%B8%85%E9%99%A4%E6%B5%AE%E5%8A%A8)<font style="color:rgb(44, 62, 80);">清除浮动</font>
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

### [](https://www.123fe.net/docs/base/high-frequency.html#%E5%9E%82%E7%9B%B4%E5%B1%85%E4%B8%AD%E7%9A%84%E6%96%B9%E6%A1%88)<font style="color:rgb(44, 62, 80);">垂直居中的方案</font>
1. **<font style="color:rgb(44, 62, 80);">利用绝对定位+transform</font>**<font style="color:rgb(44, 62, 80);">，设置</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left: 50%</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">top: 50%</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">现将子元素左上角移到父元素中心位置，然后再通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">translate</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">来调整子元素的中心点到父元素的中心。该方法可以</font>**<font style="color:rgb(44, 62, 80);">不定宽高</font>**

```css
.father {
  position: relative;
}
.son {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
```

1. **<font style="color:rgb(44, 62, 80);">利用绝对定位+margin:auto</font>**<font style="color:rgb(44, 62, 80);">，子元素所有方向都为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，将</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">设置为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，由于宽高固定，对应方向实现平分，该方法必须</font>**<font style="color:rgb(44, 62, 80);">盒子有宽高</font>**

```css
.father {
  position: relative;
}
.son {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0px;
  margin: auto;
  height: 100px;
  width: 100px;
}
```

1. **<font style="color:rgb(44, 62, 80);">利用绝对定位+margin:负值</font>**<font style="color:rgb(44, 62, 80);">，设置</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left: 50%</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">top: 50%</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">现将子元素左上角移到父元素中心位置，然后再通过</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin-left</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin-top</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以子元素自己的一半宽高进行负值赋值。该方法必须定宽高</font>

```css
.father {
  position: relative;
}
.son {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 200px;
  height: 200px;
  margin-left: -100px;
  margin-top: -100px;
}
```

1. **<font style="color:rgb(44, 62, 80);">利用 flex</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，最经典最方便的一种了，不用解释，</font>**<font style="color:rgb(44, 62, 80);">定不定宽高无所谓</font>**

```html
<style>
    .father {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 200px;
        background: skyblue;
    }
    .son {
        width: 100px;
        height: 100px;
        background: red;
    }
</style>
<div class="father">
    <div class="son"></div>
</div>
```

1. **<font style="color:rgb(44, 62, 80);">grid网格布局</font>**

```html
<style>
.father {
  display: grid;
  align-items:center;
  justify-content: center;
  width: 200px;
  height: 200px;
  background: skyblue;

}
.son {
  width: 10px;
  height: 10px;
  border: 1px solid red
}
</style>
<div class="father">
  <div class="son"></div>
</div>
```

1. **<font style="color:rgb(44, 62, 80);">table布局</font>**

<font style="color:rgb(44, 62, 80);">设置父元素为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:table-cell</font><font style="color:rgb(44, 62, 80);">，子元素设置</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display: inline-block</font><font style="color:rgb(44, 62, 80);">。利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vertical</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-align</font><font style="color:rgb(44, 62, 80);">可以让所有的行内块级元素水平垂直居中</font>

```html
<style>
    .father {
        display: table-cell;
        width: 200px;
        height: 200px;
        background: skyblue;
        vertical-align: middle;
        text-align: center;
    }
    .son {
        display: inline-block;
        width: 100px;
        height: 100px;
        background: red;
    }
</style>
<div class="father">
    <div class="son"></div>
</div>
```

**<font style="color:rgb(44, 62, 80);">小结</font>**

<font style="color:rgb(44, 62, 80);">不知道元素宽高大小仍能实现水平垂直居中的方法有：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">利用定位+margin:auto</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">利用定位+transform</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex</font><font style="color:rgb(44, 62, 80);">布局</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">grid</font><font style="color:rgb(44, 62, 80);">布局</font>

**<font style="color:rgb(44, 62, 80);">根据元素标签的性质，可以分为：</font>**

+ <font style="color:rgb(44, 62, 80);">内联元素居中布局</font>
+ <font style="color:rgb(44, 62, 80);">块级元素居中布局</font>

**<font style="color:rgb(44, 62, 80);">内联元素居中布局</font>**

+ <font style="color:rgb(44, 62, 80);">水平居中</font>
    - <font style="color:rgb(44, 62, 80);">行内元素可设置：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-align: center</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex</font><font style="color:rgb(44, 62, 80);">布局设置父元素：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display: flex; justify-content: center</font>
+ <font style="color:rgb(44, 62, 80);">垂直居中</font>
    - <font style="color:rgb(44, 62, 80);">单行文本父元素确认高度：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">height === line-height</font>
    - <font style="color:rgb(44, 62, 80);">多行文本父元素确认高度：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display: table-cell; vertical-align: middle</font>

**<font style="color:rgb(44, 62, 80);">块级元素居中布局</font>**

+ <font style="color:rgb(44, 62, 80);">水平居中</font>
    - <font style="color:rgb(44, 62, 80);">定宽:</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin: 0 auto</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">绝对定位+left:50%+margin:负自身一半</font>
+ <font style="color:rgb(44, 62, 80);">垂直居中</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position: absolute</font><font style="color:rgb(44, 62, 80);">设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">top</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin-left</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin-top</font><font style="color:rgb(44, 62, 80);">(定高)</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display: table-cell</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: translate(x, y)</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex</font><font style="color:rgb(44, 62, 80);">(不定高，不定宽)</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">grid</font><font style="color:rgb(44, 62, 80);">(不定高，不定宽)，兼容性相对比较差</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#css3%E7%9A%84%E6%96%B0%E7%89%B9%E6%80%A7)<font style="color:rgb(44, 62, 80);">CSS3的新特性</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783167415-f690b705-5170-4888-a910-63817db8f6f3.png)

**<font style="color:rgb(44, 62, 80);">1. 是什么</font>**

<font style="color:rgb(44, 62, 80);">css，即层叠样式表（Cascading Style Sheets）的简称，是一种标记语言，由浏览器解释执行用来使页面变得更美观</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css3</font><font style="color:rgb(44, 62, 80);">是css的最新标准，是向后兼容的，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS1/2</font><font style="color:rgb(44, 62, 80);">的特性在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">里都是可以使用的</font>

<font style="color:rgb(44, 62, 80);">而</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">也增加了很多新特性，为开发带来了更佳的开发体验</font>

**<font style="color:rgb(44, 62, 80);">2. 选择器</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css3</font><font style="color:rgb(44, 62, 80);">中新增了一些选择器，主要为如下图所示：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718783167260-d09f2979-8ec6-4cc5-83e8-33815b34dd49.png)

**<font style="color:rgb(44, 62, 80);">3. 新样式</font>**

+ **<font style="color:rgb(44, 62, 80);">边框</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css3</font><font style="color:rgb(44, 62, 80);">新增了三个边框属性，分别是：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-radius</font><font style="color:rgb(44, 62, 80);">：创建圆角边框</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-shadow</font><font style="color:rgb(44, 62, 80);">：为元素添加阴影</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-image</font><font style="color:rgb(44, 62, 80);">：使用图片来绘制边框</font>
+ **<font style="color:rgb(44, 62, 80);">box-shadow</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">设置元素阴影，设置属性如下（其中水平阴影和垂直阴影是必须设置的）</font>
    - <font style="color:rgb(44, 62, 80);">水平阴影</font>
    - <font style="color:rgb(44, 62, 80);">垂直阴影</font>
    - <font style="color:rgb(44, 62, 80);">模糊距离(虚实)</font>
    - <font style="color:rgb(44, 62, 80);">阴影尺寸(影子大小)</font>
    - <font style="color:rgb(44, 62, 80);">阴影颜色</font>
    - <font style="color:rgb(44, 62, 80);">内/外阴影</font>
+ **<font style="color:rgb(44, 62, 80);">背景</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">新增了几个关于背景的属性，分别是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-clip</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-origin</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-size</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-break</font>
    - **<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-clip</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">用于确定背景画区，有以下几种可能的属性：通常情况，背景都是覆盖整个元素的，利用这个属性可以设定背景颜色或图片的覆盖范围</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-clip: border-box</font><font style="color:rgb(44, 62, 80);">; 背景从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">开始显示</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-clip: padding-box</font><font style="color:rgb(44, 62, 80);">; 背景从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);">开始显示</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-clip: content-box</font><font style="color:rgb(44, 62, 80);">; 背景显</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font><font style="color:rgb(44, 62, 80);">区域开始显示</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-clip: no-clip</font><font style="color:rgb(44, 62, 80);">; 默认属性，等同于b</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">order-box</font>
    - **<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-origin</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">当我们设置背景图片时，图片是会以左上角对齐，但是是以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">的左上角对齐还是以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);">的左上角或者</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font><font style="color:rgb(44, 62, 80);">的左上角对齐?</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-origin</font><font style="color:rgb(44, 62, 80);">正是用来设置这个的</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-origin: border-box</font><font style="color:rgb(44, 62, 80);">; 从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border</font><font style="color:rgb(44, 62, 80);">开始计算</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-position</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-origin: padding-box</font><font style="color:rgb(44, 62, 80);">; 从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);">开始计算</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-position</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-origin: content-box</font><font style="color:rgb(44, 62, 80);">; 从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">content</font><font style="color:rgb(44, 62, 80);">开始计算</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-position</font>
        * <font style="color:rgb(44, 62, 80);">默认情况是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding-box</font><font style="color:rgb(44, 62, 80);">，即以</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">padding</font><font style="color:rgb(44, 62, 80);">的左上角为原点</font>
    - **<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-size</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">常用来调整背景图片的大小，主要用于设定图片本身。有以下可能的属性：</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-size: contain</font><font style="color:rgb(44, 62, 80);">; 缩小图片以适合元素（维持像素长宽比）</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-size: cover</font><font style="color:rgb(44, 62, 80);">; 扩展元素以填补元素（维持像素长宽比）</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-size: 100px 100px</font><font style="color:rgb(44, 62, 80);">; 缩小图片至指定的大小</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-size: 50% 100%</font><font style="color:rgb(44, 62, 80);">; 缩小图片至指定的大小，百分比是相对包 含元素的尺寸</font>
    - **<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-break</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">元素可以被分成几个独立的盒子（如使内联元素</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">span</font><font style="color:rgb(44, 62, 80);">跨越多行），</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-break</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">属性用来控制背景怎样在这些不同的盒子中显示</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-break: continuous</font><font style="color:rgb(44, 62, 80);">; 默认值。忽略盒之间的距离（也就是像元素没有分成多个盒子，依然是一个整体一样）</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-break: bounding-box</font><font style="color:rgb(44, 62, 80);">; 把盒之间的距离计算在内；</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-break: each-box</font><font style="color:rgb(44, 62, 80);">; 为每个盒子单独重绘背景</font>
+ **<font style="color:rgb(44, 62, 80);">文字</font>**
    - **<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">word-wrap: normal|break-word</font>**
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">normal</font><font style="color:rgb(44, 62, 80);">：使用浏览器默认的换行</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">break-all</font><font style="color:rgb(44, 62, 80);">：允许在单词内换行</font>
    - **<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-overflow</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">设置或检索当当前行超过指定容器的边界时如何显示，属性有两个值选择</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">clip</font><font style="color:rgb(44, 62, 80);">：修剪文本</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ellipsis</font><font style="color:rgb(44, 62, 80);">：显示省略符号来代表被修剪的文本</font>
    - **<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-shadow</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可向文本应用阴影。能够规定水平阴影、垂直阴影、模糊距离，以及阴影的颜色</font>
    - **<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-decoration</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">CSS3里面开始支持对文字的更深层次的渲染，具体有三个属性可供设置：</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-fill-color</font><font style="color:rgb(44, 62, 80);">: 设置文字内部填充颜色</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-stroke-color</font><font style="color:rgb(44, 62, 80);">: 设置文字边界填充颜色</font>
        * <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">text-stroke-width</font><font style="color:rgb(44, 62, 80);">: 设置文字边界宽度</font>
+ **<font style="color:rgb(44, 62, 80);">颜色</font>**
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css3</font><font style="color:rgb(44, 62, 80);">新增了新的颜色表示方式</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rgba</font><font style="color:rgb(44, 62, 80);">与</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hsla</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rgba</font><font style="color:rgb(44, 62, 80);">分为两部分，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rgb</font><font style="color:rgb(44, 62, 80);">为颜色值，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(44, 62, 80);">为透明度</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hala</font><font style="color:rgb(44, 62, 80);">分为四部分，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h</font><font style="color:rgb(44, 62, 80);">为色相，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">s</font><font style="color:rgb(44, 62, 80);">为饱和度，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">l</font><font style="color:rgb(44, 62, 80);">为亮度，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">a</font><font style="color:rgb(44, 62, 80);">为透明度</font>

**<font style="color:rgb(44, 62, 80);">4. transition 过渡</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(44, 62, 80);">属性可以被指定为一个或多个CSS属性的过渡效果，多个属性之间用逗号进行分隔，必须规定两项内容：</font>

+ <font style="color:rgb(44, 62, 80);">过度效果</font>
+ <font style="color:rgb(44, 62, 80);">持续时间</font>

```plain
transition： CSS属性，花费时间，效果曲线(默认ease)，延迟时间(默认0)
```

<font style="color:rgb(44, 62, 80);">上面为简写模式，也可以分开写各个属性</font>

```css
transition-property: width; 
transition-duration: 1s;
transition-timing-function: linear;
transition-delay: 2s;
```

**<font style="color:rgb(44, 62, 80);">5. transform 转换</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(44, 62, 80);">属性允许你旋转，缩放，倾斜或平移给定元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform-origin</font><font style="color:rgb(44, 62, 80);">：转换元素的位置（围绕那个点进行转换），默认值为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">(x,y,z):(50%,50%,0)</font>

<font style="color:rgb(44, 62, 80);">使用方式：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: translate(120px, 50%)</font><font style="color:rgb(44, 62, 80);">：位移</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: scale(2, 0.5)</font><font style="color:rgb(44, 62, 80);">：缩放</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: rotate(0.5turn)</font><font style="color:rgb(44, 62, 80);">：旋转</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: skew(30deg, 20deg)</font><font style="color:rgb(44, 62, 80);">：倾斜</font>

**<font style="color:rgb(44, 62, 80);">6. animation 动画</font>**

<font style="color:rgb(44, 62, 80);">动画这个平常用的也很多，主要是做一个预设的动画。和一些页面交互的动画效果，结果和过渡应该一样，让页面不会那么生硬</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(44, 62, 80);">也有很多的属性</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-name</font><font style="color:rgb(44, 62, 80);">：动画名称</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-duration</font><font style="color:rgb(44, 62, 80);">：动画持续时间</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-timing-function</font><font style="color:rgb(44, 62, 80);">：动画时间函数</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-delay</font><font style="color:rgb(44, 62, 80);">：动画延迟时间</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-iteration-count</font><font style="color:rgb(44, 62, 80);">：动画执行次数，可以设置为一个整数，也可以设置为infinite，意思是无限循环</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-direction</font><font style="color:rgb(44, 62, 80);">：动画执行方向</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-paly-state</font><font style="color:rgb(44, 62, 80);">：动画播放状态</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-fill-mode</font><font style="color:rgb(44, 62, 80);">：动画填充模式</font>

**<font style="color:rgb(44, 62, 80);">7. 渐变</font>**

<font style="color:rgb(44, 62, 80);">颜色渐变是指在两个颜色之间平稳的过渡，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css3</font><font style="color:rgb(44, 62, 80);">渐变包括</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">linear-gradient</font><font style="color:rgb(44, 62, 80);">：线性渐变</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">background-image: linear-gradient(direction, color-stop1, color-stop2, ...)</font><font style="color:rgb(44, 62, 80);">;</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">radial-gradient</font><font style="color:rgb(44, 62, 80);">：径向渐变</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">linear-gradient(0deg, red, green)</font>

**<font style="color:rgb(44, 62, 80);">8. 其他</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Flex</font><font style="color:rgb(44, 62, 80);">弹性布局</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Grid</font><font style="color:rgb(44, 62, 80);">栅格布局</font>
+ <font style="color:rgb(44, 62, 80);">媒体查询</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@media screen and (max-width: 960px) {}</font><font style="color:rgb(44, 62, 80);">还有打印</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">print</font>

**<font style="color:rgb(44, 62, 80);">transition和animation的区别</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Animation</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">大部分属性是相同的，他们都是随时间改变元素的属性值，他们的主要区别是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">需要触发一个事件才能改变属性，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不需要触发任何事件的情况下才会随时间改变属性值，并且</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">为2帧，从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">from .... to</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">可以一帧一帧的</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#css%E5%8A%A8%E7%94%BB%E5%92%8C%E8%BF%87%E6%B8%A1)<font style="color:rgb(44, 62, 80);">CSS动画和过渡</font>
<font style="color:rgb(44, 62, 80);">常见的动画效果有很多，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">平移</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">旋转</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">缩放</font><font style="color:rgb(44, 62, 80);">等等，复杂动画则是多个简单动画的组合</font>

**<font style="color:rgb(44, 62, 80);">css实现动画的方式，有如下几种：</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实现渐变动画</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">转变动画</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">实现自定义动画</font>

**<font style="color:rgb(44, 62, 80);">1. transition 实现渐变动画</font>**

**<font style="color:rgb(44, 62, 80);">transition的属性如下：</font>**

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-property:填写需要变化的css属性</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-duration:完成过渡效果需要的时间单位(s或者ms)默认是 0</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-timing-function:完成效果的速度曲线</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition-delay: （规定过渡效果何时开始。默认是</font><font style="color:rgb(44, 62, 80);">0</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">）</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">一般情况下，我们都是写一起的，比如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition： width 2s ease 1s</font>

<font style="color:rgb(44, 62, 80);">其中</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">timing-function</font><font style="color:rgb(44, 62, 80);">的值有如下：</font>

| <font style="color:rgb(44, 62, 80);">值</font> | <font style="color:rgb(44, 62, 80);">描述</font> |
| --- | --- |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">linear</font> | <font style="color:rgb(44, 62, 80);">匀速（等于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cubic-bezier(0,0,1,1)</font><font style="color:rgb(44, 62, 80);">）</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ease</font> | <font style="color:rgb(44, 62, 80);">从慢到快再到慢（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cubic-bezier(0.25,0.1,0.25,1)</font><font style="color:rgb(44, 62, 80);">）</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ease-in</font> | <font style="color:rgb(44, 62, 80);">慢慢变快（等于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cubic-bezier(0.42,0,1,1)</font><font style="color:rgb(44, 62, 80);">）</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ease-out</font> | <font style="color:rgb(44, 62, 80);">慢慢变慢（等于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cubic-bezier(0,0,0.58,1)</font><font style="color:rgb(44, 62, 80);">）</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ease-in-out</font> | <font style="color:rgb(44, 62, 80);">先变快再到慢（等于 cubic-bezier(0.42,0,0.58,1)`），渐显渐隐效果</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cubic-bezier(*n*,*n*,*n*,*n*)</font> | <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cubic-bezier</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">函数中定义自己的值。可能的值是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">至</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之间的数值</font> |


<font style="color:rgb(44, 62, 80);">注意：并不是所有的属性都能使用过渡的，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:none<->display:block</font>

<font style="color:rgb(44, 62, 80);">举个例子，实现鼠标移动上去发生变化动画效果</font>

```html
<style>
  .base {
    width: 100px;
    height: 100px;
    display: inline-block;
    background-color: 0EA9FF;
    border-width: 5px;
    border-style: solid;
    border-color: 5daf34;
    transition-property: width, height, background-color, border-width;
    transition-duration: 2s;
    transition-timing-function: ease-in;
    transition-delay: 500ms;
  }

  /*简写*/
  /*transition: all 2s ease-in 500ms;*/
  .base:hover {
    width: 200px;
    height: 200px;
    background-color: 5daf34;
    border-width: 10px;
    border-color: 3a8ee6;
  }
</style>
<div class="base"></div>
```

**<font style="color:rgb(44, 62, 80);">2. transform 转变动画</font>**

<font style="color:rgb(44, 62, 80);">包含四个常用的功能：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">translate(x,y)</font><font style="color:rgb(44, 62, 80);">：位移</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scale</font><font style="color:rgb(44, 62, 80);">：缩放</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rotate</font><font style="color:rgb(44, 62, 80);">：旋转</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">skew</font><font style="color:rgb(44, 62, 80);">：倾斜</font>

<font style="color:rgb(44, 62, 80);">一般配合</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition</font><font style="color:rgb(44, 62, 80);">过度使用</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">注意的是，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">不支持</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline元</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">素，使用前把它变成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">block</font>

<font style="color:rgb(44, 62, 80);">举个例子</font>

```html
<style>
.base {
  width: 100px;
  height: 100px;
  display: inline-block;
  background-color: 0EA9FF;
  border-width: 5px;
  border-style: solid;
  border-color: 5daf34;
  transition-property: width, height, background-color, border-width;
  transition-duration: 2s;
  transition-timing-function: ease-in;
  transition-delay: 500ms;
}
.base2 {
  transform: none;
  transition-property: transform;
  transition-delay: 5ms;
}
.base2:hover {
  transform: scale(0.8, 1.5) rotate(35deg) skew(5deg) translate(15px, 25px);
}
</style>
<div class="base base2"></div>
```

<font style="color:rgb(44, 62, 80);">可以看到盒子发生了旋转，倾斜，平移，放大</font>

**<font style="color:rgb(44, 62, 80);">3. animation 实现自定义动画</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">一个关键帧动画，最少包含两部分，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">属性及属性值（动画的名称和运行方式运行时间等）</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@keyframes</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（规定动画的具体实现过程）</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(44, 62, 80);">是由</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">8</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">个属性的简写，分别如下：</font>

| <font style="color:rgb(44, 62, 80);">属性</font> | <font style="color:rgb(44, 62, 80);">描述</font> | <font style="color:rgb(44, 62, 80);">属性值</font> |
| --- | --- | --- |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-duration</font> | <font style="color:rgb(44, 62, 80);">指定动画完成一个周期所需要时间，单位秒（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">s</font><font style="color:rgb(44, 62, 80);">）或毫秒（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ms</font><font style="color:rgb(44, 62, 80);">），默认是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font> | |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-timing-function</font> | <font style="color:rgb(44, 62, 80);">指定动画计时函数，即动画的速度曲线，默认是 "</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ease</font><font style="color:rgb(44, 62, 80);">"</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">linear</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ease</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ease-in</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ease-out</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ease-in-out</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-delay</font> | <font style="color:rgb(44, 62, 80);">指定动画延迟时间，即动画何时开始，默认是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font> | |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-iteration-count</font> | <font style="color:rgb(44, 62, 80);">指定动画播放的次数，默认是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">。但我们一般用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">infinite</font><font style="color:rgb(44, 62, 80);">，一直播放</font> | |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-direction</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">指定动画播放的方向</font> | <font style="color:rgb(44, 62, 80);">默认是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">normal</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">normal</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reverse</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">alternate</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">alternate-reverse</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-fill-mode</font> | <font style="color:rgb(44, 62, 80);">指定动画填充模式。默认是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">none</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">forwards</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">backwards</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">both</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-play-state</font> | <font style="color:rgb(44, 62, 80);">指定动画播放状态，正在运行或暂停。默认是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">running</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">running</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pauser</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation-name</font> | <font style="color:rgb(44, 62, 80);">指定</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@keyframes</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">动画的名称</font> | |


<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">动画只需要定义一些关键的帧，而其余的帧，浏览器会根据计时函数插值计算出来，</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@keyframes</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">定义关键帧，可以是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">from->to</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">（等同于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0%</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100%</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">），也可以是从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0%->100%</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">之间任意个的分层设置</font>

<font style="color:rgb(44, 62, 80);">因此，如果我们想要让元素旋转一圈，只需要定义开始和结束两帧即可：</font>

```css
@keyframes rotate{
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
```

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">from</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">表示最开始的那一帧，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">to</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);"> </font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">表示结束时的那一帧</font>

**<font style="color:rgb(44, 62, 80);">也可以使用百分比刻画生命周期</font>**

```css
@keyframes rotate{
  0%{
    transform: rotate(0deg);
  }
  50%{
    transform: rotate(180deg);
  }
  100%{
    transform: rotate(360deg);
  }
}
```

<font style="color:rgb(44, 62, 80);">定义好了关键帧后，下来就可以直接用它了：</font>

```css
animation: rotate 2s;
```

**<font style="color:rgb(44, 62, 80);">总结</font>**

| <font style="color:rgb(44, 62, 80);">属性</font> | <font style="color:rgb(44, 62, 80);">含义</font> |
| --- | --- |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transition（过度）</font> | <font style="color:rgb(44, 62, 80);">用于设置元素的样式过度，和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation</font><font style="color:rgb(44, 62, 80);">有着类似的效果，但细节上有很大的不同</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform（变形）</font> | <font style="color:rgb(44, 62, 80);">用于元素进行旋转、缩放、移动或倾斜，和设置样式的动画并没有什么关系，就相当于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">color</font><font style="color:rgb(44, 62, 80);">一样用来设置元素的“外表”</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">translate（移动）</font> | <font style="color:rgb(44, 62, 80);">只是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(44, 62, 80);">的一个属性值，即移动</font> |
| <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">animation（动画）</font> | <font style="color:rgb(44, 62, 80);">用于设置动画属性，他是一个简写的属性，包含</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">6</font><font style="color:rgb(44, 62, 80);">个属性</font> |


**<font style="color:rgb(44, 62, 80);">4. 用css3动画使一个图片旋转</font>**

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

### [](https://www.123fe.net/docs/base/high-frequency.html#%E6%9C%89%E5%93%AA%E4%BA%9B%E6%96%B9%E5%BC%8F-css-%E5%8F%AF%E4%BB%A5%E9%9A%90%E8%97%8F%E9%A1%B5%E9%9D%A2%E5%85%83%E7%B4%A0)<font style="color:rgb(44, 62, 80);">有哪些方式（CSS）可以隐藏页面元素</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">opacity:0</font><font style="color:rgb(44, 62, 80);">：本质上是将元素的透明度将为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">，就看起来隐藏了，但是依然占据空间且可以交互</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:none</font><font style="color:rgb(44, 62, 80);">: 这个是彻底隐藏了元素，元素从文档流中消失，既不占据空间也不交互，也不影响布局</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility:hidden</font><font style="color:rgb(44, 62, 80);">: 与上一个方法类似的效果，占据空间，但是不可以交互了</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">overflow:hidden</font><font style="color:rgb(44, 62, 80);">: 这个只隐藏元素溢出的部分，但是占据空间且不可交互</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">z-index:-9999</font><font style="color:rgb(44, 62, 80);">: 原理是将层级放到底部，这样就被覆盖了，看起来隐藏了</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform:scale(0,0)</font><font style="color:rgb(44, 62, 80);">: 平面变换，将元素缩放为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);">，但是依然占据空间，但不可交互</font>

**<font style="color:rgb(44, 62, 80);">display: none 与 visibility: hidden 的区别</font>**

+ <font style="color:rgb(44, 62, 80);">修改常规流中元素的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);">通常会造成文档重排。修改</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility</font><font style="color:rgb(44, 62, 80);">属性只会造成本元素的重绘</font>
+ <font style="color:rgb(44, 62, 80);">读屏器不会读取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:none</font><font style="color:rgb(44, 62, 80);">;元素内容；会读取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility:hidden;</font><font style="color:rgb(44, 62, 80);">元素内容</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:none</font><font style="color:rgb(44, 62, 80);">;会让元素完全从渲染树中消失，渲染的时候不占据任何空间；</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility:hidden</font><font style="color:rgb(44, 62, 80);">;不会让元素从渲染树消失，渲染师元素继续占据空间，只是内容不可见</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display:none</font><font style="color:rgb(44, 62, 80);">;是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示；</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility:hidden;</font><font style="color:rgb(44, 62, 80);">是继承属性，子孙节点消失由于继承了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hidden</font><font style="color:rgb(44, 62, 80);">，通过设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visibility:visible;</font><font style="color:rgb(44, 62, 80);">可以让子孙节点显式</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#%E8%AF%B4%E8%AF%B4em-px-rem-vh-vw%E5%8C%BA%E5%88%AB)<font style="color:rgb(44, 62, 80);">说说em/px/rem/vh/vw区别</font>
+ <font style="color:rgb(44, 62, 80);">传统的项目开发中，我们只会用到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">%</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);">这几个单位，它可以适用于大部分的项目开发，且拥有比较良好的兼容性</font>
+ <font style="color:rgb(44, 62, 80);">从</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);">开始，浏览器对计量单位的支持又提升到了另外一个境界，新增了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rem</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vh</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vw</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vm</font><font style="color:rgb(44, 62, 80);">等一些新的计量单位</font>
+ <font style="color:rgb(44, 62, 80);">利用这些新的单位开发出比较良好的响应式页面，适应多种不同分辨率的终端，包括移动设备等</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">单位中，可以分为长度单位、绝对单位，如下表所指示</font>

| **<font style="color:rgb(44, 62, 80);">CSS单位</font>** | |
| --- | --- |
| <font style="color:rgb(44, 62, 80);">相对长度单位</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ex</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ch</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rem</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vw</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vh</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vmin</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vmax</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">%</font> |
| <font style="color:rgb(44, 62, 80);">绝对长度单位</font> | <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">cm</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">mm</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">in</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pt</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pc</font> |


<font style="color:rgb(44, 62, 80);">这里我们主要讲述</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rem</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vh</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vw</font>

**<font style="color:rgb(44, 62, 80);">px</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);">，表示像素，所谓像素就是呈现在我们显示器上的一个个小点，每个像素点都是大小等同的，所以像素为计量单位被分在了绝对长度单位中</font>

<font style="color:rgb(44, 62, 80);">有些人会把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);">认为是相对长度，原因在于在移动端中存在设备像素比，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);">实际显示的大小是不确定的</font>

<font style="color:rgb(44, 62, 80);">这里之所以认为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);">为绝对单位，在于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);">的大小和元素的其他属性无关</font>

**<font style="color:rgb(44, 62, 80);">em</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);">是相对长度单位。相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1em = 16px</font><font style="color:rgb(44, 62, 80);">）</font>

<font style="color:rgb(44, 62, 80);">为了简化</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-size</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的换算，我们需要在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">body</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">选择器中声明</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-size</font><font style="color:rgb(44, 62, 80);">=</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">62.5%</font><font style="color:rgb(44, 62, 80);">，这就使 em 值变为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">16px*62.5% = 10px</font>

<font style="color:rgb(44, 62, 80);">这样</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">12px = 1.2em</font><font style="color:rgb(44, 62, 80);">,</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">10px = 1em</font><font style="color:rgb(44, 62, 80);">, 也就是说只需要将你的原来的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">px</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">数值除以 10，然后换上</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);">作为单位就行了</font>

<font style="color:rgb(44, 62, 80);">特点：</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的值并不是固定的</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">会继承父级元素的字体大小</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">em</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是相对长度单位。相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸</font>
+ <font style="color:rgb(44, 62, 80);">任意浏览器的默认字体高都是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">16px</font>

<font style="color:rgb(44, 62, 80);">举个例子</font>

```html
<div class="big">
  我是14px=1.4rem<div class="small">我是12px=1.2rem</div>
</div>
```

<font style="color:rgb(44, 62, 80);">样式为</font>

```css
<style>
    html {font-size: 10px;  } /*  公式16px*62.5%=10px  */  
    .big{font-size: 1.4rem}
    .small{font-size: 1.2rem}
</style>
```

<font style="color:rgb(44, 62, 80);">这时候</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.big</font><font style="color:rgb(44, 62, 80);">元素的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-size</font><font style="color:rgb(44, 62, 80);">为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">14px</font><font style="color:rgb(44, 62, 80);">，而</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.small</font><font style="color:rgb(44, 62, 80);">元素的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-size</font><font style="color:rgb(44, 62, 80);">为12px</font>

**<font style="color:rgb(44, 62, 80);">rem(常用)</font>**

+ <font style="color:rgb(44, 62, 80);">根据屏幕的分辨率动态设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);">的文字大小，达到等比缩放的功能</font>
+ <font style="color:rgb(44, 62, 80);">保证</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);">最终算出来的字体大小，不能小于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">12px</font>
+ <font style="color:rgb(44, 62, 80);">在不同的移动端显示不同的元素比例效果</font>
+ <font style="color:rgb(44, 62, 80);">如果</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-size:20px</font><font style="color:rgb(44, 62, 80);">的时候，那么此时的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1rem = 20px</font>
+ <font style="color:rgb(44, 62, 80);">把设计图的宽度分成多少分之一，根据实际情况</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rem</font><font style="color:rgb(44, 62, 80);">做盒子的宽度，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">viewport</font><font style="color:rgb(44, 62, 80);">缩放</font>

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">head</font><font style="color:rgb(44, 62, 80);">加入常见的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">meta</font><font style="color:rgb(44, 62, 80);">属性</font>

```html
<meta name="format-detection" content="telephone=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<!--这个是关键-->
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0，minimum-scale=1.0">
```

<font style="color:rgb(44, 62, 80);">把这段代码加入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">head</font><font style="color:rgb(44, 62, 80);">中的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">script</font><font style="color:rgb(44, 62, 80);">预先加载</font>

```javascript
// rem适配用这段代码动态计算html的font-size大小
(function(win) {
  var docEl = win.document.documentElement;
  var timer = '';

  function changeRem() {
    var width = docEl.getBoundingClientRect().width;
    if (width > 750) { // 750是设计稿大小
      width = 750;
    }
    var fontS = width / 10; // 把设备宽度十等分 1rem=10px
    docEl.style.fontSize = fontS + "px";
  }
  win.addEventListener("resize", function() {
    clearTimeout(timer);
    timer = setTimeout(changeRem, 30);
  }, false);
  win.addEventListener("pageshow", function(e) {
    if (e.persisted) { //清除缓存
      clearTimeout(timer);
      timer = setTimeout(changeRem, 30);
    }
  }, false);
  changeRem();
})(window)
```

+ <font style="color:rgb(44, 62, 80);">或者使用淘宝提供的库</font><font style="color:rgb(44, 62, 80);"> </font>[https://github.com/amfe/lib-flexible(opens new window)](https://github.com/amfe/lib-flexible)

**<font style="color:rgb(44, 62, 80);">vh、vw</font>**

<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vw</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，就是根据窗口的宽度，分成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100</font><font style="color:rgb(44, 62, 80);">等份，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100vw</font><font style="color:rgb(44, 62, 80);">就表示满宽，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">50vw</font><font style="color:rgb(44, 62, 80);">就表示一半宽。（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vw</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">始终是针对窗口的宽），同理，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vh</font><font style="color:rgb(44, 62, 80);">则为窗口的高度</font>

<font style="color:rgb(44, 62, 80);">这里的窗口分成几种情况：</font>

+ <font style="color:rgb(44, 62, 80);">在桌面端，指的是浏览器的可视区域</font>
+ <font style="color:rgb(44, 62, 80);">移动端指的就是布局视口</font>

<font style="color:rgb(44, 62, 80);">像</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vw</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vh</font><font style="color:rgb(44, 62, 80);">，比较容易混淆的一个单位是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">%</font><font style="color:rgb(44, 62, 80);">，不过百分比宽泛的讲是相对于父元素：</font>

+ <font style="color:rgb(44, 62, 80);">对于普通定位元素就是我们理解的父元素</font>
+ <font style="color:rgb(44, 62, 80);">对于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position: absolute;</font><font style="color:rgb(44, 62, 80);">的元素是相对于已定位的父元素</font>
+ <font style="color:rgb(44, 62, 80);">对于</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position: fixed;</font><font style="color:rgb(44, 62, 80);">的元素是相对于</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ViewPort</font><font style="color:rgb(44, 62, 80);">（可视窗口）</font>

**<font style="color:rgb(44, 62, 80);">总结</font>**

+ **<font style="color:rgb(44, 62, 80);">px</font>**<font style="color:rgb(44, 62, 80);">：绝对单位，页面按精确像素展示</font>
+ **<font style="color:rgb(44, 62, 80);">%</font>**<font style="color:rgb(44, 62, 80);">：相对于父元素的宽度比例</font>
+ **<font style="color:rgb(44, 62, 80);">em</font>**<font style="color:rgb(44, 62, 80);">：相对单位，基准点为父节点字体的大小，如果自身定义了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">font-size</font><font style="color:rgb(44, 62, 80);">按自身来计算（浏览器默认字体是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">16px</font><font style="color:rgb(44, 62, 80);">），整个页面内</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1em</font><font style="color:rgb(44, 62, 80);">不是一个固定的值</font>
+ **<font style="color:rgb(44, 62, 80);">rem</font>**<font style="color:rgb(44, 62, 80);">：相对单位，可理解为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">root em</font><font style="color:rgb(44, 62, 80);">, 相对根节点</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);">的字体大小来计算</font>
+ **<font style="color:rgb(44, 62, 80);">vh、vw</font>**<font style="color:rgb(44, 62, 80);">：主要用于页面视口大小布局，在页面布局上更加方便简单</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vw</font><font style="color:rgb(44, 62, 80);">：屏幕宽度的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1%</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vh</font><font style="color:rgb(44, 62, 80);">：屏幕高度的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1%</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vmin</font><font style="color:rgb(44, 62, 80);">：取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vw</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vh</font><font style="color:rgb(44, 62, 80);">中较小的那个（如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">10vh=100px 10vw=200px</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">则</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vmin=10vh=100px</font><font style="color:rgb(44, 62, 80);">）</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vmax</font><font style="color:rgb(44, 62, 80);">：取</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vw</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vh</font><font style="color:rgb(44, 62, 80);">中较大的那个（如：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">10vh=100px 10vw=200px</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">则</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">vmax=10vw=200px</font><font style="color:rgb(44, 62, 80);">）</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#flex%E5%B8%83%E5%B1%80)<font style="color:rgb(44, 62, 80);">flex布局</font>
<font style="color:rgb(44, 62, 80);">很多时候我们会用到</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex: 1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，它具体包含了以下的意思</font>

+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex-grow: 1</font><font style="color:rgb(44, 62, 80);"> ：该属性默认为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font><font style="color:rgb(44, 62, 80);"> ，如果存在剩余空间，元素也不放大。设置为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">1</font><font style="color:rgb(44, 62, 80);">  代表会放大。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex-shrink: 1</font><font style="color:rgb(44, 62, 80);"> ：该属性默认为 `1 ，如果空间不足，元素缩小。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex-basis: 0%</font><font style="color:rgb(44, 62, 80);"> ：该属性定义在分配多余空间之前，元素占据的主轴空间。浏览器就是根据这个属性来计算是否有多余空间的。默认值为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font><font style="color:rgb(44, 62, 80);"> ，即项目本身大小。设置为</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0%</font><font style="color:rgb(44, 62, 80);">  之后，因为有</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex-grow</font><font style="color:rgb(44, 62, 80);">  和</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex-shrink</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的设置会自动放大或缩小。在做两栏布局时，如果右边的自适应元素</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">flex-basis</font><font style="color:rgb(44, 62, 80);">  设为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font><font style="color:rgb(44, 62, 80);">  的话，其本身大小将会是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">0</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#%E5%A6%82%E6%9E%9C%E8%A6%81%E5%81%9A%E4%BC%98%E5%8C%96-css%E6%8F%90%E9%AB%98%E6%80%A7%E8%83%BD%E7%9A%84%E6%96%B9%E6%B3%95%E6%9C%89%E5%93%AA%E4%BA%9B)<font style="color:rgb(44, 62, 80);">如果要做优化，CSS提高性能的方法有哪些？</font>
<font style="color:rgb(44, 62, 80);">实现方式有很多种，主要有如下：</font>

+ **<font style="color:rgb(44, 62, 80);">内联首屏关键CSS</font>**
    - <font style="color:rgb(44, 62, 80);">在打开一个页面，页面首要内容出现在屏幕的时间影响着用户的体验，而通过内联</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">关键代码能够使浏览器在下载完html后就能立刻渲染</font>
    - <font style="color:rgb(44, 62, 80);">而如果外部引用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">代码，在解析</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">html</font><font style="color:rgb(44, 62, 80);">结构过程中遇到外部</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">文件，才会开始下载</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">代码，再渲染</font>
    - <font style="color:rgb(44, 62, 80);">所以，CSS内联使用使渲染时间提前</font>
    - <font style="color:rgb(44, 62, 80);">注意：但是较大的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">代码并不合适内联（初始拥塞窗口、没有缓存），而其余代码则采取外部引用方式</font>
+ **<font style="color:rgb(44, 62, 80);">异步加载CSS</font>**
    - <font style="color:rgb(44, 62, 80);">在CSS文件请求、下载、解析完成之前，CSS会阻塞渲染，浏览器将不会渲染任何已处理的内容</font>
    - <font style="color:rgb(44, 62, 80);">前面加载内联代码后，后面的外部引用css则没必要阻塞浏览器渲染。这时候就可以采取异步加载的方案，主要有如下：</font>
        * <font style="color:rgb(44, 62, 80);">使用javascript将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);">标签插到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">head</font><font style="color:rgb(44, 62, 80);">标签最后</font>

```javascript
// 创建link标签
const myCSS = document.createElement( "link" );
myCSS.rel = "stylesheet";
myCSS.href = "mystyles.css";
// 插入到header的最后位置
document.head.insertBefore( myCSS, document.head.childNodes[ document.head.childNodes.length - 1 ].nextSibling )
```

        * <font style="color:rgb(44, 62, 80);">设置</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);">标签</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">media</font><font style="color:rgb(44, 62, 80);">属性为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">noexis</font><font style="color:rgb(44, 62, 80);">，浏览器会认为当前样式表不适用当前类型，会在不阻塞页面渲染的情况下再进行下载。加载完成后，将media的值设为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">screen</font><font style="color:rgb(44, 62, 80);">或</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">all</font><font style="color:rgb(44, 62, 80);">，从而让浏览器开始解析CSS</font>

```html
<link rel="stylesheet" href="mystyles.css" media="noexist" onload="this.media='all'">
```

        * <font style="color:rgb(44, 62, 80);">通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rel</font><font style="color:rgb(44, 62, 80);">属性将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);">元素标记为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">alternate</font><font style="color:rgb(44, 62, 80);">可选样式表，也能实现浏览器异步加载。同样别忘了加载完成之后，将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rel</font><font style="color:rgb(44, 62, 80);">设回</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">stylesheet</font>

```html
<link rel="alternate stylesheet" href="mystyles.css" onload="this.rel='stylesheet'">
```

+ **<font style="color:rgb(44, 62, 80);">资源压缩</font>**
    - <font style="color:rgb(44, 62, 80);">利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">webpack</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">gulp/grunt</font><font style="color:rgb(44, 62, 80);">、</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rollup</font><font style="color:rgb(44, 62, 80);">等模块化工具，将</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css</font><font style="color:rgb(44, 62, 80);">代码进行压缩，使文件变小，大大降低了浏览器的加载时间</font>
+ **<font style="color:rgb(44, 62, 80);">合理使用选择器</font>**
    - <font style="color:rgb(44, 62, 80);">css匹配的规则是从右往左开始匹配，例如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">markdown .content h3</font><font style="color:rgb(44, 62, 80);">匹配规则如下：</font>
        * <font style="color:rgb(44, 62, 80);">先找到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">h3</font><font style="color:rgb(44, 62, 80);">标签元素</font>
        * <font style="color:rgb(44, 62, 80);">然后去除祖先不是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">.content</font><font style="color:rgb(44, 62, 80);">的元素</font>
        * <font style="color:rgb(44, 62, 80);">最后去除祖先不是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">markdown</font><font style="color:rgb(44, 62, 80);">的元素</font>
    - <font style="color:rgb(44, 62, 80);">如果嵌套的层级更多，页面中的元素更多，那么匹配所要花费的时间代价自然更高</font>
    - <font style="color:rgb(44, 62, 80);">所以我们在编写选择器的时候，可以遵循以下规则：</font>
        * <font style="color:rgb(44, 62, 80);">不要嵌套使用过多复杂选择器，最好不要三层以上</font>
        * <font style="color:rgb(44, 62, 80);">使用id选择器就没必要再进行嵌套</font>
        * <font style="color:rgb(44, 62, 80);">通配符和属性选择器效率最低，避免使用</font>
+ **<font style="color:rgb(44, 62, 80);">减少使用昂贵的属性</font>**
    - <font style="color:rgb(44, 62, 80);">在页面发生重绘的时候，昂贵属性如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">box-shadow/border-radius/filter/透明度/:nth-child</font><font style="color:rgb(44, 62, 80);">等，会降低浏览器的渲染性能</font>
+ **<font style="color:rgb(44, 62, 80);">不要使用@import</font>**
    - <font style="color:rgb(44, 62, 80);">css样式文件有两种引入方式，一种是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">link</font><font style="color:rgb(44, 62, 80);">元素，另一种是</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font><font style="color:rgb(44, 62, 80);">会影响浏览器的并行下载，使得页面在加载时增加额外的延迟，增添了额外的往返耗时</font>
    - <font style="color:rgb(44, 62, 80);">而且多个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import</font><font style="color:rgb(44, 62, 80);">可能会导致下载顺序紊乱</font>
    - <font style="color:rgb(44, 62, 80);">比如一个css文件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">index.css</font><font style="color:rgb(44, 62, 80);">包含了以下内容：</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@import url("reset.css")</font>
    - <font style="color:rgb(44, 62, 80);">那么浏览器就必须先把</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">index.css</font><font style="color:rgb(44, 62, 80);">下载、解析和执行后，才下载、解析和执行第二个文件</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">reset.css</font>
+ **<font style="color:rgb(44, 62, 80);">其他</font>**
    - <font style="color:rgb(44, 62, 80);">减少重排操作，以及减少不必要的重绘</font>
    - <font style="color:rgb(44, 62, 80);">了解哪些属性可以继承而来，避免对这些属性重复编写</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">css Sprite</font><font style="color:rgb(44, 62, 80);">，合成所有</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">icon</font><font style="color:rgb(44, 62, 80);">图片，用宽高加上b</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ackgroud-position</font><font style="color:rgb(44, 62, 80);">的背景图方式显现出我们要的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">icon</font><font style="color:rgb(44, 62, 80);">图，减少了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">http</font><font style="color:rgb(44, 62, 80);">请求</font>
    - <font style="color:rgb(44, 62, 80);">把小的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">icon</font><font style="color:rgb(44, 62, 80);">图片转成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">base64</font><font style="color:rgb(44, 62, 80);">编码</font>
    - <font style="color:rgb(44, 62, 80);">CSS3动画或者过渡尽量使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform</font><font style="color:rgb(44, 62, 80);">和o</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">pacity</font><font style="color:rgb(44, 62, 80);">来实现动画，不要使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">left</font><font style="color:rgb(44, 62, 80);">和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">top</font><font style="color:rgb(44, 62, 80);">属性</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#%E7%94%BB%E4%B8%80%E6%9D%A1-0-5px-%E7%9A%84%E7%BA%BF)<font style="color:rgb(44, 62, 80);">画一条 0.5px 的线</font>
+ <font style="color:rgb(44, 62, 80);">采用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">meta viewport</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的方式</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);"><meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no" /></font>
+ <font style="color:rgb(44, 62, 80);">采用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border-image</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的方式</font>
+ <font style="color:rgb(44, 62, 80);">采用</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">transform: scale()</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的方式</font>

### [](https://www.123fe.net/docs/base/high-frequency.html#%E5%A6%82%E4%BD%95%E7%94%BB%E4%B8%80%E4%B8%AA%E4%B8%89%E8%A7%92%E5%BD%A2)<font style="color:rgb(44, 62, 80);">如何画一个三角形</font>
<font style="color:rgb(44, 62, 80);">三角形原理:边框的均分原理</font>

```css
div {
  width:0px;
  height:0px;
  border-top:10px solid red; 
  border-right:10px solid transparent; 
  border-bottom:10px solid transparent; 
  border-left:10px solid transparent;
}
```

### [](https://www.123fe.net/docs/base/high-frequency.html#%E4%B8%A4%E6%A0%8F%E5%B8%83%E5%B1%80-%E5%B7%A6%E8%BE%B9%E5%AE%9A%E5%AE%BD-%E5%8F%B3%E8%BE%B9%E8%87%AA%E9%80%82%E5%BA%94%E6%96%B9%E6%A1%88)<font style="color:rgb(44, 62, 80);">两栏布局：左边定宽，右边自适应方案</font>
```html
<div class="box">
  <div class="box-left"></div>
  <div class="box-right"></div>
</div>
```

**<font style="color:rgb(44, 62, 80);">利用float + margin实现</font>**

```css
.box {
 height: 200px;
}

.box > div {
  height: 100%;
}

.box-left {
  width: 200px;
  float: left;
  background-color: blue;
}

.box-right {
  margin-left: 200px;
  background-color: red;
}
```

**<font style="color:rgb(44, 62, 80);">利用calc计算宽度</font>**

```css
.box {
 height: 200px;
}

.box > div {
  height: 100%;
}

.box-left {
  width: 200px;
  float: left;
  background-color: blue;
}

.box-right {
  width: calc(100% - 200px);
  float: right;
  background-color: red;
}
```

**<font style="color:rgb(44, 62, 80);">利用float + overflow实现</font>**

```css
.box {
 height: 200px;
}

.box > div {
  height: 100%;
}

.box-left {
  width: 200px;
  float: left;
  background-color: blue;
}

.box-right {
  overflow: hidden;
  background-color: red;
}
```

**<font style="color:rgb(44, 62, 80);">利用flex实现</font>**

```css
.box {
  height: 200px;
  display: flex;
}

.box > div {
  height: 100%;
}

.box-left {
  width: 200px;
  background-color: blue;
}

.box-right {
  flex: 1; // 设置flex-grow属性为1，默认为0
  overflow: hidden;
  background-color: red;
}
```

