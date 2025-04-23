### <font style="color:rgb(44, 62, 80);">BFC 是什么？</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">(</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Block Formatting Contexts</font><font style="color:rgb(44, 62, 80);">) 即块级格式化上下文，从样式上看，它与普通的容器没有什么区别，但是从功能上，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">具有普通容器没有的一些特性，例如可以包含浮动元素，使到它可以包含浮动元素，从而防止出现高度塌陷的问题</font>

---

### [#](https://www.123fe.net/blog-docs/css/-CSS%E4%B8%AD%E7%9A%84BFC.html#%E5%A6%82%E4%BD%95%E8%A7%A6%E5%8F%91-bfc)<font style="color:rgb(44, 62, 80);">如何触发 BFC</font>
+ <font style="color:rgb(44, 62, 80);">触发 BFC 的条件</font>
    - <font style="color:rgb(44, 62, 80);">浮动元素，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">除</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">none</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以外的值</font>
    - <font style="color:rgb(44, 62, 80);">绝对定位元素，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position</font><font style="color:rgb(44, 62, 80);">（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">absolute</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fixed</font><font style="color:rgb(44, 62, 80);">）</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">为以下其中之一的值</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">inline-blocks</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">table-cells</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">table-captions</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">overflow</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">除了</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">visible</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">以外的值（</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">hidden</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">auto</font><font style="color:rgb(44, 62, 80);">，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">scroll</font><font style="color:rgb(44, 62, 80);">）</font>
+ <font style="color:rgb(44, 62, 80);">在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">叫做</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Flow Root</font><font style="color:rgb(44, 62, 80);">，并增加了一些触发条件：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">display</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">table-caption</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">position</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fixed</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">值，其实</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">fixed</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">absolute</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的一个子类，因此在</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS2.1</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中使用这个值也会触发</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，只是在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">CSS3</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">中更加明确了这一点</font>

---

### [#](https://www.123fe.net/blog-docs/css/-CSS%E4%B8%AD%E7%9A%84BFC.html#bfc%E5%B8%83%E5%B1%80%E8%A7%84%E5%88%99)<font style="color:rgb(44, 62, 80);">BFC布局规则</font>
+ <font style="color:rgb(44, 62, 80);">内部的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);">会在垂直方向，一个接一个地放置。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);">垂直方向的距离由</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">决定。属于同一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">的两个相邻</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);">的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">会发生重叠</font>
+ <font style="color:rgb(44, 62, 80);">每个元素的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin box</font><font style="color:rgb(44, 62, 80);">的左边， 与包含块</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border box</font><font style="color:rgb(44, 62, 80);">的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如 此。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">的区域不会与</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float box</font><font style="color:rgb(44, 62, 80);">重叠。</font>
+ <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。</font>
+ <font style="color:rgb(44, 62, 80);">计算</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">的高度时，浮动元素也参与计算</font>

---

### [#](https://www.123fe.net/blog-docs/css/-CSS%E4%B8%AD%E7%9A%84BFC.html#bfc%E7%9A%84%E4%BD%9C%E7%94%A8%E5%8F%8A%E5%8E%9F%E7%90%86)<font style="color:rgb(44, 62, 80);">BFC的作用及原理</font>
+ **<font style="color:rgb(238, 63, 77);">自适应两栏布局</font>**

```css
body {
        width: 300px;
        position: relative;
    }
    .aside {
        width: 100px;
        height: 150px;
        float: left;
        background: #f66;
    }
    .main {
        height: 200px;
        background: #fcc;
    }
```

```html
<body>
        <div class="aside"></div>
        <div class="main"></div>
 </body>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959720201-a58af1af-df79-45bf-b754-1dcc3638ad77.png)

+ <font style="color:rgb(44, 62, 80);">根据BFC布局规则第3条：</font>
    - <font style="color:rgb(44, 62, 80);">每个元素的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin box</font><font style="color:rgb(44, 62, 80);">的左边， 与包含块</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">border box</font><font style="color:rgb(44, 62, 80);">的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。</font>
+ <font style="color:rgb(44, 62, 80);">因此，虽然存在浮动的元素</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">aslide</font><font style="color:rgb(44, 62, 80);">，但</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">main</font><font style="color:rgb(44, 62, 80);">的左边依然会与包含块的左边相接触</font>
+ <font style="color:rgb(44, 62, 80);">根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">布局规则第四条：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">的区域不会与</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">float box</font><font style="color:rgb(44, 62, 80);">重叠</font>
+ <font style="color:rgb(44, 62, 80);">我们可以通过通过触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">main</font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">， 来实现自适应两栏布局</font>

```css
.main {
    overflow: hidden;
}
```

+ <font style="color:rgb(44, 62, 80);">当触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">main</font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">后，这个新的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">不会与浮动的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">aside</font><font style="color:rgb(44, 62, 80);">重叠。因此会根据包含块的宽度，和</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">aside</font><font style="color:rgb(44, 62, 80);">的宽度，自动变窄。效果如下：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959720212-f05bb67c-3cfc-4657-9f3b-8beeee7b302d.png)

+ **<font style="color:rgb(238, 63, 77);">清除内部浮动</font>**

```css
.par {
    border: 5px solid #fcc;
    width: 300px;
 }

.child {
    border: 5px solid #f66;
    width: 100px;
    height: 100px;
    float: left;
}
```

```html
<body>
    <div class="par">
        <div class="child"></div>
        <div class="child"></div>
    </div>
</body>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959720247-2be9ff03-7ce2-4f44-813b-648b38527eed.png)

+ <font style="color:rgb(44, 62, 80);">根据</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">布局规则第六条：</font>
    - <font style="color:rgb(44, 62, 80);">计算</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">的高度时，浮动元素也参与计算</font>
+ <font style="color:rgb(44, 62, 80);">为达到清除内部浮动，我们可以触发</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">par</font><font style="color:rgb(44, 62, 80);">生成</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">，那么</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">par</font><font style="color:rgb(44, 62, 80);">在计算高度时，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">par</font><font style="color:rgb(44, 62, 80);">内部的浮动元素</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">child</font><font style="color:rgb(44, 62, 80);">也会参与计算</font>

```css
.par {
    overflow: hidden;
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959719727-c310202c-356f-4f3b-86a0-4ae4f8a7b69c.png)

+ **<font style="color:rgb(238, 63, 77);">防止垂直 margin 重叠</font>**

```css
p {
        color: #f55;
        background: #fcc;
        width: 200px;
        line-height: 100px;
        text-align: center;
        margin: 100px;
    }
```

```html
<p>Haha</p>
    <p>Hehe</p>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959720081-b37b6218-4cbf-46d2-b304-b3e17e4f6919.png)

+ <font style="color:rgb(44, 62, 80);">两个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p</font><font style="color:rgb(44, 62, 80);">之间的距离为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">100px</font><font style="color:rgb(44, 62, 80);">，发送了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">重叠</font>
+ <font style="color:rgb(44, 62, 80);">根据BFC布局规则第二条：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);">垂直方向的距离由</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">决定。属于同一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">的两个相邻Box的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">会发生重叠</font>
+ <font style="color:rgb(44, 62, 80);">我们可以在</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">p</font><font style="color:rgb(44, 62, 80);">外面包裹一层容器，并触发该容器生成一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">。那么两个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">P</font><font style="color:rgb(44, 62, 80);">便不属于同一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">，就不会发生</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">重叠了</font>

```css
.wrap {
        overflow: hidden;
    }
    p {
        color: #f55;
        background: #fcc;
        width: 200px;
        line-height: 100px;
        text-align: center;
        margin: 100px;
    }
```

```html
<p>Haha</p>
    <div class="wrap">
        <p>Hehe</p>
    </div>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1718959720188-c4e1321f-45fe-4f7d-949f-348409ea4b4f.png)

---

### [#](https://www.123fe.net/blog-docs/css/-CSS%E4%B8%AD%E7%9A%84BFC.html#%E6%80%BB%E7%BB%93)<font style="color:rgb(44, 62, 80);">总结</font>
+ <font style="color:rgb(44, 62, 80);">其实以上的几个例子都体现了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">布局规则第五条</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此</font>
+ <font style="color:rgb(44, 62, 80);">因为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">内部的元素和外部的元素绝对不会互相影响，因此， 当</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">外部存在浮动时，它不应该影响</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">内部</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Box</font><font style="color:rgb(44, 62, 80);">的布局，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">会通过变窄，而不与浮动有重叠。同样的，当</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">内部有浮动时，为了不影响外部元素的布局，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">BFC</font><font style="color:rgb(44, 62, 80);">计算高度时会包括浮动的高度。避免</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">margin</font><font style="color:rgb(44, 62, 80);">重叠也是这样的一个道理</font>

