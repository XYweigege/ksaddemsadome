访问地址：[https://pdf-demo-phi.vercel.app/](https://pdf-demo-phi.vercel.app/)

代码地址： [pdf-demo.zip](https://www.yuque.com/attachments/yuque/0/2024/zip/207857/1723454032883-68c0ac5f-42f5-4c17-998c-235e9df05af1.zip)

`html2canvas` + `jspdf`方案是前端实现页面打印的一种常用方案，但是在实践过程中，遇到的最大问题就是**分页截断**的问题：当页面元素超过一页A4纸的时候，连续的页面就会因为分页而导致内容被截断，进而影响了pdf的可读性。

由于网上关于分页截断的解决思路比较少，所以特意将此次的解决方案记录下来。



## **使用 JSPDF 和 html2canvas 创建简单的 PDF文件**
首先，我们开始使用 JSPDF 和 html2canvas 生成一个简单的 PDF文件。

### 创建一个 JSPDF 实例
创建一个 JSPDF 实例，设置页面的大小、方向和其他参数。[参考官网](https://artskydj.github.io/jsPDF/docs/index.html)可以写一个很简单的实例

```jsx
var doc = new jsPDF({
  orientation: 'landscape',
  unit: 'in',
  format: [4, 2]
}

                    doc.text('Hello world!', 1, 1)
doc.save('two-by-four.pdf')
```

生成一个pdf文件，并且在文件中写入一定内容，其实`JSPDF`这个库就能做到。

但是很多业务场景下，我们的目标内容会更复杂，而且还要考虑样式，所以最好的方式是引入`html2canvas`这个库，将页面元素转换成`base64`数据，然后贴在`pdf`中(使用[addImage方法](https://artskydj.github.io/jsPDF/docs/module-addImage.html)），这样就能保证页面的内容。

引入了`html2canvas`库后，我们更多关注是**利用现成组件库、框架或者原生html和css实现更复杂的页面内容**。

### 引入 [html2canvas](https://html2canvas.hertzen.com/)
使用 `html2canvas` 捕捉 HTML 内容或特定的 HTML 元素，并将其转换为 Canvas。其中，`html2canvas` 函数的主要用法是：

```jsx
html2canvas(element, options);
```

<br/>color1
+ `**element**`**：** 要渲染为 canvas 的 HTML 元素。这可以是一个 DOM 元素，也可以是一个选择器字符串，表示需要渲染的元素。
+ `**options**`**（可选）：** 一个包含配置选项的对象，用于定制 `**html2canvas**` 的行为。

以下是一些常见的配置选项：

+ `**allowTaint**`**（默认值: **`**false**`**）：** 是否允许加载跨域的图片，默认为 `**false**`。如果设为 `**true**`，`**html2canvas**` 将尝试加载跨域的图片，但在某些情况下可能会受到浏览器的限制。
+ `**backgroundColor**`**（默认值: **`**#ffffff**`**）：** canvas 的背景颜色。
+ `**useCORS**`**（默认值: **`**false**`**）：** 是否使用 CORS（Cross-Origin Resource Sharing）来加载图片。如果设置为 `**true**`，则 `**html2canvas**` 将尝试使用 CORS 来加载图片。
+ `**logging**`**（默认值: **`**false**`**）：** 是否输出日志信息到控制台。
+ `**width**`** 和 **`**height**`**：** canvas 的宽度和高度。如果未指定，则默认为目标元素的宽度和高度。
+ `**scale**`**（默认值: **`**window.devicePixelRatio**`**）：** 缩放因子，决定 canvas 的分辨率。

<br/>

下面是一个简单的demo，可以看到`html2canvas`能够将`dom`元素转化为一张`base64`图片，将鼠标选中元素，可以感受到图片和文字的不同。

```plain
<div id="capture" style="padding: 10px; background: #f5da55">
    <h4 style="color: #000; ">Hello world!</h4>
</div>
```

```plain
html2canvas(document.querySelector("#capture")).then(canvas => {
    document.body.appendChild(canvas)
});
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723453719567-c7d095cf-6344-4d70-9cdd-04fcdd689ac5.png)

### 将html2canvas转化的图片放到pdf中
这一步我们需要使用JSPDF 的`addImage`方法，其语法如下：

```javascript
addImage(imageData, format, x, y, width, height, alias, compression)
```

<br/>color1
+ `imageData` - 要添加的图像数据。可以是图像的 URL、图像的 base64 编码字符串或图像的二进制数据
+ `format` - 图像的格式。可以是 "JPEG"、"PNG" 或 "TIFF"。
+ `x` - 图像在 PDF 文档中的 x 坐标。
+ `y` - 图像在 PDF 文档中的 y 坐标。
+ `width` - 图像的宽度。
+ `height` - 图像的高度。
+ `alias` - 图像的别名。此别名可用于在 PDF 文档中引用图像。
+ `compression` - 图像的压缩级别。可以是 "`NONE`"、"`FAST`" 或 "`SLOW`"。

<br/>

下面是一串示例代码：

```jsx
import jsPDF from 'jspdf';

export default function addImageUsage() {
  const doc = new jsPDF();
  const imageData = 【替换成base64数据流】;
  doc.addImage(imageData, 'png', 0, 0, 10, 10);
  doc.addImage(imageData, 'png', 100, 100, 10, 10);
  doc.addImage(imageData, 'png', 200, 200, 10, 10);

  drawNet(doc);

  doc.save('test.pdf');
}

const drawNet = (doc) => {
  const gap = 10;
  const start = [0, 0];
  const end = [595.28, 841.89];

  // 所有横线
  for (let i = start[0]; i < end[0]; i = i + gap) {
    doc.line(i, 0, i, end[0]);
  }
  // 所有纵线
  for (let j = start[1]; j < end[1]; j = j + gap) {
    doc.line(0, j, end[1], j);
  }
};
```

此示例将在 PDF 文档（默认是A4纸大小，宽高为[595.28, 841.89]像素）的 `(10, 10)` 、`(100, 100)` 、`(200, 200)` 坐标处，添加一张png 图像。图像的宽度和高度将分别为 10 和 10 像素，为了了解pdf中的坐标系统，此示例还在pdf文档中生成了间距为10px的网格系统。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723453787296-6acad590-04af-4450-9a71-f06cee2c628d.png)

### **JSPDF 和 html2canvas结合起来用**
了解了上面的三个关键点，接下来我们将这三个步骤串联起来，实现一个基本的`html→pdf`的方案。大致步骤如下：

1. 写一个基本`html`页面
2. 创建`jspdf`实例
3. 获取页面的dom节点，使用`html2canvas`将其转化为`base64`数据流
4. 将`base64`数据流装载到`jspdf`提供的`addImage`方法中
5. 保存`pdf`

基于这5个步骤，可以实现基本的页面打印。

```jsx
import html2canvas from 'html2canvas';
import jsPDF, { RGBAData } from 'jspdf';

// 将元素转化为canvas元素
// 通过 放大 提高清晰度
// width为内容宽度
async function toCanvas(element: HTMLElement) {
  if (!element) return { width: 0, height: 0 };

  // canvas元素
  const canvas = await html2canvas(element, {
    scale: window.devicePixelRatio * 2, // 增加清晰度
    useCORS: true // 允许跨域
  });

  // 获取canvas转化后的宽高
  const { width: canvasWidth, height: canvasHeight } = canvas;

  // 转化成图片Data
  const canvasData = canvas.toDataURL('image/jpeg', 1.0);

  return { width: canvasWidth, height: canvasHeight, data: canvasData };
}

/**
 * 生成pdf(A4多页pdf截断问题， 包括页眉、页脚 和 上下左右留空的护理)
 */
export async function generatePDF({
  /** pdf内容的dom元素 */
  element,

  /** pdf文件名 */
  filename
}) {
  if (!(element instanceof HTMLElement)) {
    return;
  }

  const pdf = new jsPDF();

  // 一页的高度， 转换宽度为一页元素的宽度
  const {
    width: imageWidth,
    height: imageHeight,
    data
  } = await toCanvas(element);

  // 添加图片
  function addImage(
    _x: number,
    _y: number,
    pdfInstance: jsPDF,
    base_data:
    | string
    | HTMLImageElement
    | HTMLCanvasElement
    | Uint8Array
    | RGBAData,
    _width: number,
    _height: number
  ) {
    pdfInstance.addImage(base_data, 'JPEG', _x, _y, _width, _height);
  }

  addImage(0, 0, pdf, data!, imageWidth, imageHeight);

  return pdf.save(filename);
}
```

## 多页：比例缩放+循环移位
通常，在我们的实践中，会发现2个问题：

+ 生成的pdf内容与实际的页面元素比例不一致
+ 页面内容超出一页pdf的高度，但是生成的pdf只有一页，没有展示全部的页面信息

这两个问题的解决方案是**等比例缩放**+**循环移位**：

+ **等比例缩放**

通过比例缩放，实现页面内容等比例展示在pdf文档中

令页面元素的宽高为`x`, `y`（转化成canvas图片的宽高），pdf文档的宽高为`w`, `h`。因为高度可以通过加页延伸，所以可以按照宽度进行缩放，缩放后的图片高度可以通过下列公式计算

y_scaled=(w/x)\*yy\_{scaled} = (w / x) \* yy_scaled=(w/x)<font style="color:#cc0000;">\*</font>y

+ **循环移位**

如果页面的高度超出了pdf文档的高度，即`y > h`，使用`addPage`方法添加一页即可。但是在新的一页中，我们的图片内容的高度需要调整。

假设`y = 2 * h`，这意味我们需要两页才能完整得展示页面内容。在一页pdf中，图片在起始位置插入即可，即

```jsx
PDF.addImage(pageData, 'JPEG', 0, 0, x, y)// 注意x,y 是缩放后的大小
```

在第二页pdf中，图片的纵向位置需要调整一页pdf的高度，即

```jsx
PDF.addImage(pageData, 'JPEG', 0, -h, x, y)// 注意x,y 是缩放后的大小
```

通过循环计算剩余高度，然后不停调整纵向位置移动base64的图片位置，可以解决多页的问题。

## **分页截断的挑战**
尽管 `JSPDF` 和 `html2canvas` 是功能强大的工具，但是他们也有很多槽点，比如得手动分页，手动处理分页截断的问题。等你实践到这一步，就开始面临分页截断的问题，类似的问题也有网友在[Github上提出](https://github.com/parallax/jsPDF/issues/1517)，但是底下依然没有很好的解决思路。

<br/>color1
处理分页截断的原理就是在使用`addImage`之前，将html进行分页，通过维护一个高度位置数据，来记录每次循环迭代`addImage`的位置。

<br/>

从高到低遍历维护一个分页数组`pages`，该数组记录每一页的起始位置，如：`pages[0]` 对应 第一页起始位置，`pages[1]` 对应第二页起始位置

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723453871623-90e1be94-f946-4ec5-bd82-483b754c40ad.png)

接下来我们重点讨论如何将页面进行切割，然后生成`pages`这个数组。

假设页面的高度是`1500`，pdf宽高是`[500, 900]`，如果不用处理分页截断的问题，我们可以想到第一页（`0-900`）是用来承载页面从高度为0到900的信息；

第二页（`900-1800`）是用来承载页面从高度900到1500的，所以`pages`数组为[0, 900]。

如果要处理分页截断呢，这时候就需要计算页面元素的距离pdf文档起始位置的高度`h1`，以及该元素的内部高度`h2`，通过这两个高度来判断这个元素要不要放在下一页，防止截断，示意图如下：

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723453882662-f59d7406-6067-4314-ad5f-941f52caae39.png)

如果`h1 + h2 > 页面高度`， 这时候说明这个元素不处理的就会被分页截断，所以应该要把这个元素放到第二页去渲染，这就意味着`pages`记录的数据要变化，示意图如下，可以看到`pages[1]`我们往上调整了，比第二页pdf的起始位置更高。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723453891755-23c5f982-396d-4985-8387-1316df9be3c1.png)

说明渲染第二页pdf的时候，要从`h1`开始渲染，`pages`数组为`[0, h1]`，解释为第一页pdf渲染页面高度区域为`0-900`, 第二页pdf渲染html高度区域为`h1-1500`。注意到第一页渲染的时候到尾部的时候，**会有部分内容和第二页头部内容重合。**因为`h1`到`900`这部分的内容肯定会渲染，这部分内容一直都是页面元素，我们改变`pages[1]`的值的原因只是创建一个副本，让页面看起来内容没有被截断。

为了解决这个问题（为了美观），我们用填充一块白色区域遮掉它！此处使用`jspdf`的`rect`和`setFillColor`方法，把重合的区域遮白处理。

```javascript
pdf.setFillColor(255, 255, 255);
pdf.rect(x, y, Math.ceil(_width), Math.ceil(_height), 'F');
```

### 如何获得h1和h2
上面我们谈到了`h1`和`h2`，其中`h1`是元素盒子的上边距到打印区域的高度（比例缩放后的高度），`h2`是元素盒子的内部高度。

计算`h1`: [getBoundingClientRect方法](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect)

```jsx
const rect = contentElement.getBoundingClientRect() || {};
const topDistance = rect.top;
return topDistance;
```



![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723453923347-fde508f4-bc96-4c0d-8ce4-ace0a86addc5.png)

计算`h2`： [offsetHeight方法](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/offsetHeight)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723453974671-0b07fb9c-6f68-4a65-a273-80b3361f078d.png)

值得注意的是，因为打印区域的html元素不一定是从窗口顶部开始，所以为了计算实际的`h1`(元素到打印区域的顶部距离），可以采用这样的方法：

+ 用`getBoundingClientRect`方法计算元素到窗口顶部的距离
+ 循环打印之前将`pages`信息针对第一个元素进行一个高度校准。

```jsx
/ 对pages进行一个值的修正，因为pages生成是根据根元素来的，根元素并不是我们实际要打印的元素，而是element，
// 所以要把它修正，让其值是以真实的打印元素顶部节点为准
const newPages = pages.map((item) => item - pages[0]);
```

##  
 

 

