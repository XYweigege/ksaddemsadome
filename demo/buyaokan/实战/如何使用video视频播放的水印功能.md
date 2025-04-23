[代码地址：](https://gitee.com/sohucw/video-watermark)

项目需求: 给视频加上水印（主要是防录屏,最后的实现效果是这样）

### 难点
**<font style="color:#0e0e0e;">实现方式</font>**<font style="color:#0e0e0e;">：</font>

<font style="color:#0e0e0e;">通过 CSS、Canvas 或 SVG 绘制水印，将其以 </font><font style="color:#0e0e0e;">absolute</font><font style="color:#0e0e0e;"> 或 </font><font style="color:#0e0e0e;">fixed</font><font style="color:#0e0e0e;"> 的方式覆盖在页面上。可以动态调整位置、透明度和大小。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">难点</font>**<font style="color:#0e0e0e;">：</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">防篡改</font>**<font style="color:#0e0e0e;">：需要防止用户通过 F12 删除或修改水印</font>

<font style="color:#0e0e0e;"> </font>**<font style="color:#DF2A3F;">	结合 MutationObserver 监听 DOM 变化，当水印被移除时立刻重新添加</font>**

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">响应式布局</font>**<font style="color:#0e0e0e;">：水印需要随着页面尺寸变化自适应。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">性能优化</font>**<font style="color:#0e0e0e;">：保证在复杂布局页面中添加水印不影响渲染性能。  
</font>

**<font style="color:#0e0e0e;">2. 视频播放器加水印</font>**

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">实现方式</font>**<font style="color:#0e0e0e;">：</font>

<font style="color:#0e0e0e;">	•在视频流中叠加水印。可以通过使用 Canvas 绘制视频帧并在其上叠加水印。</font>

<font style="color:#0e0e0e;">	•在 HLS 或 DASH 流媒体中，在服务端直接加水印。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">难点</font>**<font style="color:#0e0e0e;">：</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">实时性</font>**<font style="color:#0e0e0e;">：水印需要随着视频播放实时渲染，确保同步性。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">性能要求</font>**<font style="color:#0e0e0e;">：特别是在高分辨率视频中，水印的渲染可能对性能产生较大影响。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">兼容性</font>**<font style="color:#0e0e0e;">：需要兼容不同的播放器（如原生 video 标签或第三方播放器库）。</font>

<font style="color:#0e0e0e;">	•</font>**<font style="color:#0e0e0e;">安全性</font>**<font style="color:#0e0e0e;">：确保水印无法轻松被去除，比如通过屏幕录制等方式窃取无水印视频。</font>

### 效果：
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731393246363-c7808b0d-e2f2-45cb-8cbb-6e8c5b94c8fd.png)





### 具体水印的代码：
```javascript
import Vue from 'vue';

Vue.directive('watermark', {
  bind: function (el, binding) {
    // 水印文字，父元素，画布宽度，画布高度，字体，文字颜色，画布横坐标
    function addWaterMarker(str, parentNode, width, height, font, textColor, fillTextX = '10') {
      // 检查父元素是否包含子元素
      const elementContains = (parent, child) => parent !== child && parent.contains(child);
      const flag = elementContains(parentNode, document.querySelector('canvas'));
      // 防止重复创建
      if (!flag) {
        const can = document.createElement('canvas');
        parentNode.appendChild(can);
        can.width = width || 200;
        can.height = height || 140;
        can.style.display = 'none';
        const cans = can.getContext('2d');
        cans.rotate((-20 * Math.PI) / 180);
        cans.font = font || '13px Microsoft Yahei';
        cans.fillStyle = textColor || '#dddddd';
        cans.textAlign = 'left';
        cans.textBaseline = 'Middle';
        cans.fillText(str, fillTextX, can.height);
        // 设置背景图（整个项目中都添加水印建议使用此方法）
        // parentNode.style.backgroundImage = "url(" + can.toDataURL("image/png") + ")";

        // 创建div 定位覆盖（某个元素，如图片添加水印建议使用此方法）
        const div = document.createElement('div');
        div.id = str;
        div.style.pointerEvents = 'none';
        div.style.top = '0';
        div.style.left = '0';
        div.style.position = 'absolute';
        div.style.zIndex = '100000';
        div.style.width = '100%';
        div.style.height = '100%';
        div.style.background = 'url(' + can.toDataURL('image/png') + ')';
        parentNode.appendChild(div);
      }
    }
    if (binding.value.text) {
      addWaterMarker(binding.value.text, el, binding.value.width, binding.value.height, binding.value.font, binding.value.textColor, binding.value.fillTextX);
    }
  }
});

```

##### <font style="color:rgb(79, 79, 79);">main.js 引入warterMark.js （或者在想要添加的某个页面引入）</font>
```javascript
import Vue from 'vue';
import App from './App.vue';
import router from './router';
import store from './store';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import 'normalize.css/normalize.css';
import './assets/css/reset.css';
import './router/router-config'; // 路由守卫，做动态路由的地方
import '@/utils/warterMark.js';
Vue.use(ElementUI);
Vue.config.productionTip = false;

new Vue({
  router,
  store,
  render: (h) => h(App)
}).$mount('#app');

```

##### <font style="color:rgb(79, 79, 79);">页面结构（注意水印一定要跟video同级，不要直接加到video上面去，会没有用）</font>
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731393481533-42eb2029-1f28-4a37-87e8-2081cf770787.png)

### 遇到的问题
<font style="color:rgb(79, 79, 79);">加了水印后，但是会出现一个问题，视频在点击全屏时，水印会消失</font>

代码的处理逻辑：

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731393618979-84d2878e-5bc1-4498-9bfe-090767cc187d.png)

<br/>color1
监听视频全屏时，拿到视频标签元素 和 水印标签元素，然后把 水印标签元素append到视频标签元素里面去就可以显示水印了。（因为我这里用的是腾讯云sdk，大家可以根据自己用的视频插件来写，基本逻辑就在这里了）

原因就是在视频全屏播放时，会把其他的元素都隐藏掉（css默认）。所以很多做视频开发的开发者，他们的全屏其实不是真正的全屏，而是套了一个壳，把外面的壳放大了。

<br/>





### 其他
<font style="color:rgb(77, 77, 77);">如果demo运行起来视频不可用，可能是云点播的license过期了，大家可以自己去申请一个填到项目里。</font>  
<font style="color:rgb(77, 77, 77);">云点播地址：</font>[https://cloud.tencent.com/document/product/266/58772](https://cloud.tencent.com/document/product/266/58772)



![](https://cdn.nlark.com/yuque/0/2024/png/207857/1731394427222-44992cec-bac8-4e08-9149-a4b13a3c43a9.png)

 

