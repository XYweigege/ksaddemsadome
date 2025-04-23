#### 前端大屏可视化项目适配方案，和具体的视频的原理实现

怎么实现一个大屏  
[代码地址： ](https://gitee.com/sohucw/big-screen-vue3)(Vue版本)  
实现原理的视频：[https://www.douyin.com/user/self?modal_id=7350631615254564122](https://www.douyin.com/user/self?modal_id=7350631615254564122)  

1.**缩放适配原理:**   

  scale方式：通过scale属性，根据屏幕大小，对图表进行等比缩放  

 优缺点:

​     优点：代码少，适配简单，当一次处理后，不需要再为每个图表进行单独适配

​     缺点：因为是根据ui等比缩放，当大屏幕跟ui比例不一致时候，会出现周边留白情况

​		 当缩放比例大时候，字体和图片会有一点失真。

​                 当缩放比例过大时候，事件热区会出现偏移。



1. ​        1280*720 老式电脑屏幕  
2. ​        1366*768 普通液晶
3. ​        1080P  : 1920*1080 高清液晶 
4. ​         2K: 2560   *1440   
5. ​        4K: 3840*2160
6. ​        8K:  7680*4320

 监听dom变化，     修改样式 transform: translate scale(数字)   缩放

   缩放比例公式：

   ```js
   const getScale=(w=1920. h=960)=>{
       //根据屏幕变化适配的比例
       const scale =window.innerWidth / window.innerHeight < w/h?
             			window.innerWidth/ w:   
                       window.innerHeight /h;
       return scale;
   }  
   
   
   ```

设计稿   1920  960。

屏幕 1440 900  那么显示不完全。1440/900   < 1920/960 1.6<2

根据上述公式，需要缩放的比例，是window.innerWidth/ w= 1440/1920=0.75  元素dom需要缩小0.75

1920*0.75 = 1440

960*0.75 720



**2:vm vh方式**   实现原理：根据设计稿的尺寸，将px按比例转化为vw vh，

优缺点

​	优点可以动态计算图表的宽高，字体，灵活度较高。当屏幕比例和ui不一样的时候，不会出现两边留白

​       缺点需要编写公共函数为每个图表都单独做字体，  间距，位移的适配，比较麻烦。













视频地址：[https://www.douyin.com/user/self?modal_id=7338374759513689370](https://www.douyin.com/user/self?modal_id=7338374759513689370)



大屏另外一直适配技术方案（- vw vh方案：）

[代码地址：](https://gitee.com/sohucw/big-screen-adapter)  
视频地址：[https://www.douyin.com/user/self?modal_id=7356946850072562971&showTab=post](https://www.douyin.com/user/self?modal_id=7356946850072562971&showTab=post)



大屏项目：前端一套代码，同时适配pc端和移动端

[代码库地址：](https://gitee.com/sohucw/screen-pc-mobile)

视频地址：https://www.douyin.com/user/self?modal_id=7359133992861420827

基于postcss和vw





​        

基于 React、zustand、DataV、ECharts 框架的 " **数据大屏项目** "。支持数据动态刷新渲染、屏幕适配、数据请求模拟、局部样式、图表自由替换/复用等功能。 使用了函数式编程

[代码地址：](https://github.com/sohucw/react18-bigscreen)（React版本）

项目环境：react^18.2.0、vite^5.2.0、npm^9.8.1、node^18.18, zustand^4.5.2。









### 难点可以这样写：
#### echarts在vue或者react中使用存在的问题
<br/>color1
+ 每个图表需要从头到尾写一遍完整的option配置，十分冗余
+ 在同一个项目中，各类图表设计十分相似，甚至是相同，没必要一直做重复工作
+ 窗口缩放时的适应问题
+ 在项目中如何按需引入echarts图表 减少打包体积的大小

<br/>

####   
解决方案
####   
项目中作为难点和亮点怎么和面试官聊和写到简历里面
<br/>color1

+ 统一封装了图表组件，统一管理图表配置，做到了业务数据和echarts的 option样式配置数据分离
+ 统一解决了窗口缩放时的图表的适应问题, 解决了窗口缩放引起的echarts图形变形的问题
+ 按需引入echarts图表 减少打包体积的大小，（全量引入按需引入 打包后的体积减少了三分之一）
+ 解决了大屏适配(在 2k、4k， 8k 屏幕下)的问题并封装成了组件在团队各个项目中使用，提升了项目研发效率

<br/>



> 大屏适配参考（2种方案都和面试官深聊一下，作为高级的难点和亮点，前提你要知道具体怎么实现的）可以看我之前发的内容，都给出了代码和demo还有视频讲解，
>



一种方案

+ 使用了scale方案  
通过 scale 属性，根据屏幕大小，对图表进行整体的等比缩放  
其实是有一定的问题的：  
**<font style="color:#DF2A3F;">1.因为是根据 ui 稿等比缩放，当大屏跟 ui 稿的比例不一样时，会出现周边留白情况   
</font>****<font style="color:#DF2A3F;">2.当缩放比例过大时候，字体会有一点点模糊，就一点点 </font>**

**<font style="color:#DF2A3F;">      3.当缩放比例过大时候，事件热区会偏移。</font>**

```html
<div className="screen-wrapper">
  <div className="screen" id="screen">
  </div>
</div>
<script>
  export default {
    mounted() {
      // 初始化自适应  ----在刚显示的时候就开始适配一次
      handleScreenAuto();
      // 绑定自适应函数   ---防止浏览器栏变化后不再适配
      window.onresize = () => handleScreenAuto();
    },
    deleted() {
      window.onresize = null;
    },
    methods: {
      // 数据大屏自适应函数
      handleScreenAuto() {
        const designDraftWidth = 1920; //设计稿的宽度
        const designDraftHeight = 1080; //设计稿的高度
        // 根据屏幕的变化适配的比例
        const scale =
          document.documentElement.clientWidth /
          document.documentElement.clientHeight <
          designDraftWidth / designDraftHeight
          ? document.documentElement.clientWidth / designDraftWidth
          : document.documentElement.clientHeight / designDraftHeight;
        // 缩放比例
        document.querySelector(
          '#screen',
        ).style.transform = `scale(${scale}) translate(-50%, -50%)`;
      },
    },
  };
</script>

/*
除了设计稿的宽高是根据您自己的设计稿决定以外，其他复制粘贴就完事
*/  
.screen-root {
  height: 100%;
  width: 100%;
  .screen {
    display: inline-block;
    width: 1920px;  //设计稿的宽度
    height: 1080px;  //设计稿的高度
    transform-origin: 0 0;
    position: absolute;
    left: 50%;
    top: 50%;
  }
}

```

采用了另外的一直方式

+ vw vh方案：  
实现方式：  
按照设计稿的尺寸，将px按比例计算转为vw和vh  
优点：
+ 1.可以动态计算图表的宽高，字体等，灵活性较高 
+ 2.当屏幕比例跟 ui 稿不一致时，不会出现两边留白情况  
缺点：每个图表都需要单独做字体、间距、位移的适配，比较麻烦



按需引入代码

```javascript
import * as echarts from 'echarts/core'
import customedTheme from './theme/customed.json'

// 引入柱状图图表，图表后缀都为 Chart
import { BarChart, LineChart, PieChart } from 'echarts/charts'
// 引入提示框，标题，直角坐标系，数据集，内置数据转换器组件，组件后缀都为 Component
import { TitleComponent, TooltipComponent, LegendComponent, GridComponent, GraphicComponent, DataZoomComponent } from 'echarts/components'
// 标签自动布局、全局过渡动画等特性
import { LabelLayout, UniversalTransition } from 'echarts/features'
// 引入 Canvas 渲染器，注意引入 CanvasRenderer 或者 SVGRenderer 是必须的一步
import { CanvasRenderer } from 'echarts/renderers'

echarts.use([
  TitleComponent,
  TooltipComponent,
  LegendComponent,
  GridComponent,
  GraphicComponent,
  DataZoomComponent,
  BarChart,
  LineChart,
  PieChart,
  LabelLayout,
  UniversalTransition,
  CanvasRenderer
])

echarts.registerTheme('customed', customedTheme)
export default echarts
```

 

**<font style="color:#0e0e0e;">1. Canvas 渲染</font>**<font style="color:#0e0e0e;"></font>

<font style="color:#0e0e0e;">	</font>**<font style="color:#0e0e0e;">优点</font>**<font style="color:#0e0e0e;">：</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">性能优越</font>**<font style="color:#0e0e0e;">：Canvas 渲染是基于位图的，绘制速度较快，适合处理大量数据，尤其是当图表中元素数量非常多时。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">动画流畅</font>**<font style="color:#0e0e0e;">：Canvas 能够很好地支持高性能的动态渲染，适合展示实时数据更新和复杂动画效果。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">内存占用低</font>**<font style="color:#0e0e0e;">：相对于 SVG，Canvas 对内存的使用更高效，尤其是当图表中元素数量庞大时。</font>

<font style="color:#0e0e0e;">	</font>**<font style="color:#0e0e0e;">缺点</font>**<font style="color:#0e0e0e;">：</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">不可编辑性</font>**<font style="color:#0e0e0e;">：Canvas 渲染的图形是一个位图，一旦绘制完成，无法像 SVG 那样直接修改元素（例如动态添加、删除图形）。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">SEO 和可访问性差</font>**<font style="color:#0e0e0e;">：Canvas 内容不容易被搜索引擎抓取和解析，也不适合需要无障碍访问的场景。</font>

<font style="color:#0e0e0e;">	</font>**<font style="color:#0e0e0e;">适用场景</font>**<font style="color:#0e0e0e;">：</font>

<font style="color:#0e0e0e;">	•大量数据（如散点图、热力图、实时数据流图等）。</font>

<font style="color:#0e0e0e;">	•高性能需求场景（如实时动态更新、动画效果等）。</font>

<font style="color:#0e0e0e;">	•需要绘制复杂图形、覆盖面积较大的图表。</font>

**<font style="color:#0e0e0e;">2. SVG 渲染</font>**

<font style="color:#0e0e0e;">	</font>**<font style="color:#0e0e0e;">优点</font>**<font style="color:#0e0e0e;">：</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">高质量的图形</font>**<font style="color:#0e0e0e;">：SVG 是矢量图形，具有无限缩放的特性，不会失真，适合用于图表的精准显示。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">易于交互和修改</font>**<font style="color:#0e0e0e;">：SVG 可以通过 DOM 操作直接修改图形元素，支持动态交互和元素的编辑、删除等操作。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">良好的可访问性</font>**<font style="color:#0e0e0e;">：SVG 图形可以通过屏幕阅读器等工具进行访问，适用于需要无障碍支持的场景。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">SEO 支持</font>**<font style="color:#0e0e0e;">：SVG 是文本文件，能够被搜索引擎解析，适用于需要被索引的图表内容。</font>

<font style="color:#0e0e0e;">	</font>**<font style="color:#0e0e0e;">缺点</font>**<font style="color:#0e0e0e;">：</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">性能较差</font>**<font style="color:#0e0e0e;">：SVG 是基于 XML 的，对于图形较复杂或数据量庞大的图表，性能不如 Canvas。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">内存占用较高</font>**<font style="color:#0e0e0e;">：每个图形元素都是 DOM 节点，元素数量增多时会占用更多内存，渲染速度也可能变慢。</font>

<font style="color:#0e0e0e;">	</font>**<font style="color:#0e0e0e;">适用场景</font>**<font style="color:#0e0e0e;">：</font>

<font style="color:#0e0e0e;">	•少量数据和静态图表（如饼图、条形图等）。</font>

<font style="color:#0e0e0e;">	•需要交互、动画和实时更新的场景。</font>

<font style="color:#0e0e0e;">	•需要较高质量图形的场景（如响应式设计、需要高精度显示的图表）。  
</font>

**<font style="color:#0e0e0e;">总结</font>**

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">Canvas</font>**<font style="color:#0e0e0e;"> 适合</font>**<font style="color:#0e0e0e;">数据量大</font>**<font style="color:#0e0e0e;">、</font>**<font style="color:#0e0e0e;">需要高性能</font>**<font style="color:#0e0e0e;">和复杂动画的图表。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">SVG</font>**<font style="color:#0e0e0e;"> 适合</font>**<font style="color:#0e0e0e;">图形较简单</font>**<font style="color:#0e0e0e;">、</font>**<font style="color:#0e0e0e;">需要交互</font>**<font style="color:#0e0e0e;">和</font>**<font style="color:#0e0e0e;">注重质量</font>**<font style="color:#0e0e0e;">的图表，特别是数据量较小、精确度要求高的场景。</font>











## 大屏获取数据更新。

基于sse，接受服务端向客户端的数据，前端 new EventSource("http://")。  

基于websocket

基于定时轮询







