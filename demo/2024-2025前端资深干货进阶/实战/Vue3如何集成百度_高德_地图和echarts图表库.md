[项目代码地址：](https://github.com/sohucw/vue3-baidumap-echarts.git)

访问地址：[https://sohucw.github.io/vue3-baidumap-echarts/#/](https://sohucw.github.io/vue3-baidumap-echarts/#/)

### <font style="color:rgb(51, 51, 51);">1. 注册为百度地图开发者</font>
**<font style="color:rgb(51, 51, 51);">第1步和第2步是为了在echarts里使用百度地图，如果你不想使用，或者使用高德地图，可忽略</font>**<font style="color:rgb(51, 51, 51);">  
</font>[<font style="color:rgb(51, 51, 51);">官网地址</font>](https://lbsyun.baidu.com/)<font style="color:rgb(51, 51, 51);">，然后在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">应用管理</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-></font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">我的应用</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里，</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">创建应用</font>`<font style="color:rgb(51, 51, 51);">，创建好后复制</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">AK</font>`<font style="color:rgb(51, 51, 51);">  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730970865630-097165a5-10a1-4c38-b79d-b17e4e4777db.png)

### <font style="color:rgb(51, 51, 51);">2. 在根目录的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">index.html</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里引入百度地图</font>
```html
<head>
  <meta charset="UTF-8" />
  <link rel="icon" type="image/svg+xml" href="/vite.svg" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>xxx</title>
  <script type="text/javascript" src="https://api.map.baidu.com/api?v=3.0&ak=你复制好的AK"></script>
</head>
```

<br/>color1
<font style="color:rgb(51, 51, 51);">在 head 里引入，是为了提前加载进来</font>

<br/>

## 封装
### <font style="color:rgb(51, 51, 51);">1. 增加ts对百度地图的支持</font>
<font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.eslintrc.cjs</font>`<font style="color:rgb(51, 51, 51);">，加入对百度地图的支持</font>

```javascript
module.exports = {
  // 其他省略
  globals: {
    BMap: true
  }
}
```

### <font style="color:rgb(51, 51, 51);">2. 全局注册 echarts</font>
<font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">main.ts</font>`

```javascript
// 引入 echarts
import * as echarts from 'echarts'
import themeJSON from '@/assets/weizwz.json'
echarts.registerTheme('weizwz', themeJSON)

const app = createApp(App)
// 全局挂载 echarts
app.config.globalProperties.$echarts = echarts
```

### <font style="color:rgb(51, 51, 51);">3. 封装 echarts</font>
`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">src/components</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">chart</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件夹，并在其下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">index.vue</font>`<font style="color:rgb(51, 51, 51);">，封装如下</font>

```vue
<script setup lang="ts">
  import { onMounted, getCurrentInstance, defineExpose, ref } from 'vue'

  defineOptions({
    name: 'WChart'
  })
  // defineExpose 让父组件可调用此方法
  defineExpose({
    setData
  })

  // 组件传参
  const props = defineProps({
    width: {
      type: String, //参数类型
      default: '100%', //默认值
      required: false //是否必须传递
    },
    height: {
      type: String,
      default: '10rem',
      required: true
    },
    option: {
      type: Object,
      default: () => {
        return {}
      },
      required: true
    },
    // 初始化之前的工作，比如加载百度地图相关数据
    initBefore: {
      type: Function,
      required: false
    },
    // 初始化之后的工作，比如添加百度地址控件
    initAfter: {
      type: Function,
      required: false
    }
  })

  let chart: { setOption: (arg0: Record<string, any>) => void; resize: () => void }
  const wchart = ref(null)

  //声明周期函数，自动执行初始化
  onMounted(() => {
    init()
    // 监控窗口大小，自动适应界面
    window.addEventListener('resize', resize, false)
  })

  //初始化函数
  function init() {
    // 基于准备好的dom，初始化echarts实例
    const dom = wchart.value
    // 通过 internalInstance.appContext.config.globalProperties 获取全局属性或方法
    let internalInstance = getCurrentInstance()
    let echarts = internalInstance?.appContext.config.globalProperties.$echarts

    chart = echarts.init(dom, 'weizwz')
    // 渲染图表
    if (props.initBefore) {
      props.initBefore(chart).then((data: Record<string, any>) => {
        setData(data)
        if (props.initAfter) props.initAfter(chart)
      })
    } else {
      chart.setOption(props.option)
      if (props.initAfter) props.initAfter(chart)
    }
  }

  function resize() {
    chart.resize()
  }
  // 父组件可调用，设置动态数据
  function setData(option: Record<string, any>) {
    chart.setOption(option)
  }
</script>

<template>
  <div ref="wchart" :style="`width: ${props.width} ; height: ${props.height}`" />
</template>

<style lang="scss" scoped></style>
```

## 使用
### <font style="color:rgb(51, 51, 51);">1. 使用 echarts 普通图表</font>
<font style="color:rgb(51, 51, 51);">示例：使用玫瑰环形图</font>

```vue
<script setup lang="ts">
  import WChart from '@comp/chart/index.vue'

  defineOptions({
    name: 'ChartLoop'
  })
  // 正常 echarts 参数
  const option = {
    grid: {
      top: '20',
      left: '10',
      right: '10',
      bottom: '20',
      containLabel: true
    },
    series: [
      {
        name: '人口统计',
        type: 'pie',
        radius: [50, 120],
        center: ['50%', '50%'],
        roseType: 'area',
        itemStyle: {
          borderRadius: 8
        },
        label: {
          formatter: '{b}\n{c} 万人'
        },
        data: [
          { value: 2189.31, name: '北京' },
          { value: 1299.59, name: '西安' },
          { value: 1004.79, name: '长沙' }
        ]
      }
    ]
  }
</script>

<template>
  <WChart width="100%" height="300px" :option="option" />
</template>

<style lang="scss" scoped></style>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730970913244-fdb5d8da-589a-460f-a11d-7a32a1ad13d2.png)

### <font style="color:rgb(51, 51, 51);">2. 结合百度地图</font>
<font style="color:rgb(51, 51, 51);">示例：西安热力图</font>

```vue
<script setup lang="ts">
  import { reactive } from 'vue'
  import WChart from '@comp/chart/index.vue'
  // 注意需要引入 bmap，即 echarts 对百度地图的支持扩展
  import 'echarts/extension/bmap/bmap'
  // 热力数据，内容如：{ features: [ { geometry: { coordinates: [ [ [x, y] ] ] } } ]}
  // 为什么这么复杂，因为是我从阿里地理数据下载的，地址 https://datav.aliyun.com/portal/school/atlas/area_selector
  import xianJson from '@/assets/xian.json'

  defineOptions({
    name: 'ChartMap'
  })

  const option = {
    animation: false,
    backgroundColor: 'transparent',
    bmap: {
      // 地图中心点
      center: [108.93957150268, 34.21690396762],
      zoom: 12,
      roam: true
    },
    visualMap: {
      show: false,
      top: 'top',
      min: 0,
      max: 5,
      seriesIndex: 0,
      calculable: true,
      inRange: {
        color: ['blue', 'blue', 'green', 'yellow', 'red']
      }
    },
    series: [
      {
        type: 'heatmap',
        coordinateSystem: 'bmap',
        data: reactive([] as any[]),
        pointSize: 5,
        blurSize: 6
      }
    ]
  }

  const initBefore = () => {
    return new Promise((resolve) => {
      // 处理数据
      const arr = []
      for (const item of xianJson.features) {
        const positions = item.geometry.coordinates[0][0]
        for (const temp of positions) {
          const position = temp.concat(Math.random() * 1000 + 200)
          arr.push(position)
        }
      }
      option.series[0].data = arr
      resolve(option)
    })
  }

  const initAfter = (chart: {
    getModel: () => {
      (): any
      new (): any
      getComponent: { (arg0: string): { (): any; new (): any; getBMap: { (): any; new (): any } }; new (): any }
    }
  }) => {
    // 添加百度地图插件
    var bmap = chart.getModel().getComponent('bmap').getBMap()
    // 百度地图样式，需要自己去创建
    bmap.setMapStyleV2({
      styleId: 'bc05830a75e51be40a38ffc9220613bb'
    })
    // bmap.addControl(new BMap.MapTypeControl())
  }
</script>

<template>
  <WChart width="100%" height="500px" :option="option" :initBefore="initBefore" :initAfter="initAfter" />
</template>

<style lang="scss" scoped></style>
```

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730970913235-f27a4cf7-da8d-4475-afa7-db07d93b726e.png)

