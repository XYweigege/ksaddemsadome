### <font style="color:rgb(102, 102, 102);">在说具体内容之前，我们必须了解几个概念，就是：</font>**<font style="color:rgb(102, 102, 102);">Retina屏、物理像素、设备独立像素、设备像素比</font>**
<font style="color:rgb(36, 41, 46);">在CSS中我们一般使用px作为单位，需要注意的是，CSS样式里面的px和物理</font>[<font style="color:rgb(36, 41, 46);">像素</font>](https://so.csdn.net/so/search?q=%E5%83%8F%E7%B4%A0&spm=1001.2101.3001.7020)<font style="color:rgb(36, 41, 46);">并不是相等的。CSS中的像素只是一个抽象的单位，在不同的设备或不同的环境中，CSS中的1px所代表的物理像素是不同的。在PC端，CSS的1px一般对应着电脑屏幕的1个物理像素，但在移动端，CSS的1px等于几个物理像素是和屏幕像素密度有关的。</font>

**<font style="color:rgb(36, 41, 46);">1.Retina屏</font>**

<font style="color:rgb(36, 41, 46);">所谓“Retina”是一种显示标准，是</font><font style="color:rgb(243, 59, 69);">把更多的像素点压缩至一块屏幕里</font><font style="color:rgb(36, 41, 46);">，从而达到更高的分辨率并提高屏幕显示的细腻程度。由摩托罗拉公司研发。最初该技术是用于Moto Aura上。这种分辨率在正常观看距离下足以使人肉眼无法分辨其中的单独像素。也被称为视网膜显示屏。</font>

<font style="color:rgb(36, 41, 46);">Retina屏一般在苹果公司的产品上用的比较多，诸如MacBook、iPad、iPhone等</font>

<font style="color:rgb(36, 41, 46);">以MacBook Pro with Retina Display为例，工作时显卡渲染出2880x1800个像素，其中每四个像素一组，输出原来屏幕的一个像素显示的大小区域内的图像。这样一来，用户所看到的图标与文字的大小与原来的1440x900分辨率显示屏相同，但精细度是原来的4倍，但对于特殊元素，如视频与图像，则以一个图片像素对应一个屏幕像素的方式显示。故不会产生</font>[<font style="color:rgb(36, 41, 46);">Windows</font>](https://baike.baidu.com/item/Windows)<font style="color:rgb(36, 41, 46);">中分辨率提升使屏幕文字与图像变小，造成阅读困难的问题。这样在设计软件时只需将所有的UI元素的精细度都提高到原来的4倍就可以既保持了观看舒适度，又提高了显示效果。关于</font>[<font style="color:rgb(36, 41, 46);">iOS设备</font>](https://baike.baidu.com/item/iOS%E8%AE%BE%E5%A4%87)<font style="color:rgb(36, 41, 46);">，也由四个像素代替原来一个像素，通过下图对比就可以较明显地观察到这种关系</font>

![]()

**<font style="color:rgb(36, 41, 46);">2.物理像素(physical pixel)</font>**

<font style="color:rgb(36, 41, 46);">物理像素又被称为</font>**<font style="color:rgb(36, 41, 46);">设备像素、设备物理像素</font>**<font style="color:rgb(36, 41, 46);">，它是显示器（电脑、手机屏幕）最小的物理显示单位，每个物理像素由颜色值和亮度值组成。所谓的一倍屏、二倍屏(Retina)、三倍屏，指的是设备以多少物理像素来显示一个CSS像素，也就是说，多倍屏以更多更精细的物理像素点来显示一个CSS像素点，在普通屏幕下1个CSS像素对应1个物理像素，而在二倍</font>[<font style="color:rgb(36, 41, 46);">Retina</font>](https://baike.baidu.com/item/Retina/4616695?fr=aladdin)<font style="color:rgb(36, 41, 46);">屏幕下，1个CSS像素对应的却是4个物理像素（参照上面Retina屏的原理下文田字示意图理解）。</font>

**<font style="color:rgb(36, 41, 46);">3</font>**<font style="color:rgb(36, 41, 46);">.</font>**<font style="color:rgb(36, 41, 46);">设备独立像素(device-independent pixel)</font>**

<font style="color:rgb(36, 41, 46);">设备独立像素是我们写CSS时所用的像素，它是一个抽像的单位，主要使用在浏览器上，用来精确度量Web页面上的内容。</font>

**<font style="color:rgb(36, 41, 46);">4.设备像素比(device pixel ratio)</font>**

<font style="color:rgb(36, 41, 46);">设备像素比简称为</font><font style="color:rgb(243, 59, 69);">dpr</font><font style="color:rgb(36, 41, 46);">，</font>**<font style="color:rgb(36, 41, 46);">物理像素与设备独立像素的比例</font>**<font style="color:rgb(36, 41, 46);">。</font>

<font style="color:rgb(36, 41, 46);">当这个比率为1:1时，使用1个设备像素显示1个css像素。当这个比率为2:1=2时，使用4(2*2)个设备像素显示1个css像素。当这个比率为3:1=3，使用9(3*3)个设备像素显示1个css像素。</font>

<font style="color:rgb(36, 41, 46);">这里要注意，dpr=2,并不是物理像素是设备独立像素的2倍，而是用4个物理像素去表示1个设备逻辑像素</font>

**<font style="color:rgb(36, 41, 46);">应该说1个设备独立像素是1个物理像素的4倍！！！！！，因为你4个网物理像素才代表我1个设备独立像素</font>**

**<font style="color:rgb(36, 41, 46);">dpr只代表一个数字比例，不是倍数关系。</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730191613116-5914ea84-3aee-4f58-9aa6-6adb88476ac2.png)

<font style="color:rgb(36, 41, 46);">CSS的1px等于几个物理像素，除了和屏幕像素密度dpr有关，还和用户缩放有关系。例如，当用户把页面放大一倍，那么CSS中1px所代表的物理像素也会增加一倍；反之把页面缩小一倍，CSS中1px所代表的物理像素也会减少一倍。关于这点，在文章后面的1px细线问题部分还会讲到。</font>

---

### **<font style="color:rgb(102, 102, 102);">viewport</font>**
<font style="color:rgb(36, 41, 46);">viewport就是设备上用来显示网页的那一块区域，但viewport又不局限于浏览器可视区域的大小，它可能比浏览器的可视区域要大，也可能比浏览器的可视区域要小。</font>

<font style="color:rgb(36, 41, 46);">在默认情况下，一般来讲，移动设备上的viewport都是要大于浏览器可视区域的，这是因为考虑到移动设备的分辨率相对于桌面电脑来说都比较小，所以为了能在移动设备上正常显示那些传统的为桌面浏览器设计的网站，移动设备上的浏览器都会把自己默认的viewport设为980px或1024px（也可能是其它值，这个是由设备自己决定的），但带来的后果就是浏览器会出现横向滚动条，因为浏览器可视区域的宽度是比这个默认的viewport的宽度要小的。</font>

<font style="color:rgb(36, 41, 46);">这里要认清三种视口</font>

+ <font style="color:rgb(36, 41, 46);"> visual viewport 可见视口，指屏幕宽度</font>
+ <font style="color:rgb(36, 41, 46);"> layout viewport 布局视口，指DOM宽度</font>
+ <font style="color:rgb(36, 41, 46);"> ideal viewport 理想适口，使布局视口就是可见视口即为理想视口</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730191613003-e457702b-342d-48dc-979d-ad01b07dfa7f.png)

<font style="color:rgb(36, 41, 46);">获取屏幕宽度(visual viewport)的尺寸：</font>

```javascript
window. innerWidth/Height
```

<font style="color:rgb(36, 41, 46);">获取DOM宽度(layout viewport)的尺寸：</font>

```javascript
document. documentElement. clientWidth/Height
```

<font style="color:rgb(36, 41, 46);">设置理想视口ideal viewport：</font>

```javascript
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

<font style="color:rgb(36, 41, 46);">该meta标签的作用是让layout viewport的宽度等于visual viewport的宽度，同时不允许用户手动缩放，从而达到理想视口。</font>

```css

```

1. <font style="color:rgb(36, 41, 46);"> meta</font><font style="color:rgb(171, 86, 86);">[name=</font><font style="color:rgb(136, 0, 0);">"viewport"</font><font style="color:rgb(171, 86, 86);">]</font><font style="color:rgb(36, 41, 46);">里各参数的含义为：</font>
2. **<font style="color:rgb(36, 41, 46);">width</font>**<font style="color:rgb(36, 41, 46);">: 设置layout viewport 的宽度，为一个正整数，或字符串”width-device”。</font>
3. <font style="color:rgb(36, 41, 46);"> initial-scale: 设置页面的初始缩放值，为一个数字，可以带小数。</font>
4. <font style="color:rgb(36, 41, 46);"> minimum-scale: 允许用户的最小缩放值，为一个数字，可以带小数。</font>
5. <font style="color:rgb(36, 41, 46);"> maximum-scale: 允许用户的最大缩放值，为一个数字，可以带小数。</font>
6. <font style="color:rgb(36, 41, 46);"> height: 设置layout viewport 的高度，这个属性对我们并不重要，很少使用。</font>
7. <font style="color:rgb(36, 41, 46);"> user-scalable: 是否允许用户进行缩放，值为“no”或“yes”。</font>

### <font style="color:rgb(102, 102, 102);">1 rem适配</font>
<font style="color:rgb(36, 41, 46);">哇卡卡卡，终于说到移动端屏幕适配方案了，可能大家看的想骂娘了，不过别急啊，上面的东西弄不清楚，下面是适配也听不懂啊</font>

<font style="color:rgb(36, 41, 46);">rem是相对于根元素的字体大小的单位，也就是html的font-size大小，浏览器默认的字体大小是16px，所以默认的1rem=16px，我们可以根据设备宽度动态设置根元素的font-size，使得以rem为单位的元素在不同终端上以相对一致的视觉效果呈现。</font>

<font style="color:rgb(36, 41, 46);">事实上</font><font style="color:rgb(36, 41, 46);"> </font>[<font style="color:rgb(36, 41, 46);">rem</font>](http://pan.baidu.com/s/1bpFOweN)<font style="color:rgb(36, 41, 46);">是把屏幕等分成 指定的份数，以20份为例，每份为</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">1rem</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">，</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">1rem</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">对应的大小就是</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">rem基准值</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">，</font>[<font style="color:rgb(36, 41, 46);">rem</font>](http://pan.baidu.com/s/1bpFOweN)<font style="color:rgb(36, 41, 46);">做的就是把</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">rem基准值</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">给</font>`<font style="color:rgb(36, 41, 46);background-color:rgb(249, 250, 250);"><html></font>`<font style="color:rgb(36, 41, 46);">的</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">font-size</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">，所以如果设备的</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">物理像素</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">宽为</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">640px</font>**<font style="color:rgb(36, 41, 46);"> ，分成20份，那么</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">1rem=640px/20=32px</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">,</font><font style="color:rgb(36, 41, 46);"> </font>`<font style="color:rgb(36, 41, 46);background-color:rgb(249, 250, 250);"><html></font>`<font style="color:rgb(36, 41, 46);">的</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">font-size为32px</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">。</font>

```javascript
1. //这段代码放在head标签里面
2. (function () {
3. var html = document.documentElement;
4. function onWindowResize() {
5. html.style.fontSize = html.getBoundingClientRect().width / 20 + 'px';
6. }
7. window.addEventListener('resize', onWindowResize);
8. onWindowResize();
9. })();
```

<font style="color:rgb(36, 41, 46);">当然，你也可以分成30份，40份，60份等等，这个看自己的喜好了</font>

<font style="color:rgb(36, 41, 46);">在我们实际切图的时候，对于视觉稿上的元素尺寸换算，只需要元素的</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">原始的px值(即你量的大小)</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">除以</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">rem基准值</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">即可。例如设计稿的大小为640px，</font><font style="color:rgb(36, 41, 46);"> </font>**<font style="color:rgb(36, 41, 46);">rem基准值 = 640px/20 = 32px</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">，有个元素的大小你量出来是</font><font style="color:rgb(36, 41, 46);"> </font>_<font style="color:rgb(36, 41, 46);">140px</font>__<font style="color:rgb(36, 41, 46);">286px</font>_<font style="color:rgb(36, 41, 46);">* ，那么换算过来就是：</font>



1. <font style="color:rgb(136, 0, 0);">140px</font><font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">=</font><font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(136, 0, 0);">140</font><font style="color:rgb(36, 41, 46);">/</font><font style="color:rgb(136, 0, 0);">32</font><font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">=</font><font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(136, 0, 0);">4.375rem</font>
2. <font style="color:rgb(136, 0, 0);">286px</font><font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">=</font><font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(136, 0, 0);">286</font><font style="color:rgb(36, 41, 46);">/</font><font style="color:rgb(136, 0, 0);">32</font><font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">=</font><font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(136, 0, 0);">8.9375rem</font>



<font style="color:rgb(36, 41, 46);">这样我们就可以用rem来代替像素px了， 而且在所有的移动端都是自适应的</font>

<font style="color:rgb(36, 41, 46);">这个方法目前是比较好的适配方法，只是rem在计算时很麻烦，有很多小数，这个时候大家可以试一下用less.js解决rem的小数问题</font>

---

### <font style="color:rgb(102, 102, 102);">2 rem+vw适配</font>
<font style="color:rgb(36, 41, 46);">什么是vw和vh?</font>

+ <font style="color:rgb(36, 41, 46);">vw : 1vw 等于视口宽度的1%</font>
+ <font style="color:rgb(36, 41, 46);">vh : 1vh 等于视口高度的1%</font>
+ <font style="color:rgb(36, 41, 46);">vmin : 选取 vw 和 vh 中最小的那个</font>
+ <font style="color:rgb(36, 41, 46);">vmax : 选取 vw 和 vh 中最大的那个</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730191612989-a16c5d0b-d74b-4c7b-8471-36375dead1aa.png)

<font style="color:rgb(36, 41, 46);">用视口单位度量，视口宽度为100vw，高度为100vh（左侧为竖屏情况，右侧为横屏情况）</font>

<font style="color:rgb(36, 41, 46);">例如，在桌面端浏览器视口尺寸为650px，那么 1vw = 650 * 1% = 6.5px（这是理论推算的出，如果浏览器不支持0.5px，那么实际渲染结果可能是7px）</font>

<font style="color:rgb(243, 59, 69);">注意：这里的1%是指占视口的1%，而不是我们定义的div的1%</font>

<font style="color:rgb(36, 41, 46);">如何利用rem+vw进行屏幕适配呢？我们以设计稿为750px为基准</font>

<font style="color:rgb(36, 41, 46);">第一步：设置meta标签</font>

```html
<meta name="viewport" content="width=device-width, initial-scale=2.0, maximum-scale=2.0, minimum-scale=2.0, user-scalable=no">
```

<font style="color:rgb(36, 41, 46);">因为iPhone6以及大多数的dpr为2,为了第二步的方便进行换算</font>

<font style="color:rgb(36, 41, 46);">第二步：设置html的font-size大小</font>

```css
html{font-size:13.33333333vw}
```

<font style="color:rgb(243, 59, 69);">为什么font-size=13.33333333vw?</font>

<font style="color:rgb(36, 41, 46);">下面分析下原理吧, 上面我们说了</font>**<font style="color:rgb(36, 41, 46);">vw表示1%的屏幕宽度</font>**<font style="color:rgb(36, 41, 46);">,而我们的设计稿通常是750px的,</font>**<font style="color:rgb(36, 41, 46);">屏幕一共是100vw</font>**<font style="color:rgb(36, 41, 46);">,对应750px,那么1</font>**<font style="color:rgb(36, 41, 46);">px就是0.1333333vw,。</font>**

<font style="color:rgb(36, 41, 46);">同时我们知道rem,rem是相对html元素的字体大小，为了方便计算,我们取html的font-size=100px,通过上面的计算结果</font>**<font style="color:rgb(36, 41, 46);">1px是0.13333333vw,那么100px就是13.333333vw了</font>**

**<font style="color:rgb(36, 41, 46);">所以，我们让1rem=100px=13.333333vw</font>**

<font style="color:rgb(36, 41, 46);">那么在项目上就很好地可以进行使用了</font>

<font style="color:rgb(36, 41, 46);">当我们通过ps测量一个div的大小为 width:200px,height:137px时，我们就可以这样写，ps量出来的像素直接除以100，计算小数很方便</font>

1. **<font style="color:rgb(36, 41, 46);">div</font>**<font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(36, 41, 46);">{</font>
2. **<font style="color:rgb(36, 41, 46);">width</font>**<font style="color:rgb(36, 41, 46);">:</font><font style="color:rgb(36, 41, 46);"> </font><font style="color:rgb(136, 0, 0);">2rem</font><font style="color:rgb(36, 41, 46);">;</font>
3. **<font style="color:rgb(36, 41, 46);">height</font>**<font style="color:rgb(36, 41, 46);">:</font><font style="color:rgb(136, 0, 0);">1.37rem</font><font style="color:rgb(36, 41, 46);">;</font>
4. <font style="color:rgb(36, 41, 46);">}</font>



<font style="color:rgb(36, 41, 46);">是不是相对于前面的rem适配来说，不用去计算小数了呢？</font>

<font style="color:rgb(36, 41, 46);">但是注意，这是针对于dpr=2的适配，至于其他dpr适配，要根据设计师的设计稿规定</font>

<font style="color:rgb(36, 41, 46);">1）如果你的设计图是640px或者750px，那么dpr的取值就是</font>**<font style="color:rgb(36, 41, 46);">2,</font>**<font style="color:rgb(36, 41, 46);">meta如下设置</font>

```html
<meta name="viewport" content="width=device-width, initial-scale=2.0, maximum-scale=2.0, minimum-scale=2.0, user-scalable=no">
```

<font style="color:rgb(36, 41, 46);"></font>

<font style="color:rgb(36, 41, 46);">3）如果你的设计图是1080px,那么css像素就是1080px,那么dpr的取值就是</font>**<font style="color:rgb(36, 41, 46);">3,</font>**<font style="color:rgb(36, 41, 46);">这个时候，meta要如下设置</font>

```html
<meta name="viewport" content="width=device-width, initial-scale=3.0, maximum-scale=3.0, minimum-scale=3.0, user-scalable=no">
```

<font style="color:rgb(36, 41, 46);">然后rem的计算方法和dpr=2时一样。</font>

<font style="color:rgb(36, 41, 46);">rem+vw适配解决了小数问题，但是兼容性不太好，所以看大家怎么选择了</font>

