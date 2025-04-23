```bash
pnpm i pinia
```

安装好之后，先在src目录下新建pinia文件夹

```bash
├─ 📁src
│  ├─ 📁pinia
│  │  ├─ 📁modules
│  │  │  ├─ 📄app.js
│  │  │  ├─ 📄tag.js
│  │  │  └─ 📄user.js
│  │  └─ 📄index.js

```

```javascript

import { getAppIsCollapse, setAppIsCollapse } from '@/utils/webStorage'

const useAppStore = defineStore('app', {
  state: () => {
    return {
      isCollapse: getAppIsCollapse()
    }
  },
  actions: {
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
      setAppIsCollapse('isCollapse', this.isCollapse)
    }
  }
})

export default useAppStore

```

```javascript

import { createPinia } from 'pinia'
import useAppStore from './modules/app'
import useUserStore from './modules/user'
import useTagStore from './modules/tag'

const pinia = createPinia()

export { useAppStore, useUserStore, useTagStore }
export default pinia

```





```javascript
import { createApp } from 'vue'

import pinia from './pinia' // 状态管理器
import router from './router' // 路由管理器
import plugins from './plugins' // 插件管理器

import '@/style/index.scss' // 全局样式入口

import App from './App.vue' // App入口

import './permission' // 路由权限校验

createApp(App).use(pinia).use(router).use(plugins).mount('#app')

```



```javascript
pnpm i vue-router

```

```javascript
├─ 📁src
│  ├─ 📁router
│  │  ├─ 📄index.js
│  │  └─ 📄router.config.js

```

```javascript

import { createRouter, createWebHistory } from 'vue-router'

const routes = [
  { // 使用layout布局（头部，侧边导航，tag导航标签）的内页都放这里
    path: '/',
    name: 'layout',
    redirect: '/login',
    component: () => import('@/layout/Index.vue'),
    children: [
      { // 有了欢迎光临，动态路由再也不怕没有首页了
        path: '/welcome',
        name: 'welcome',
        component: () => import('@/views/Welcome/Index.vue')
      },
      { // 又要keep-alive，又要刷新，只能靠这个重定向到空白页面，然后router.replace刷新了
        path: '/redirect/:pathMatch(.*)*',
        name: 'redirect',
        component: () => import('@/views/Redirect/Index.vue')
      },
      { // 404页面有了，403, 500有没必要？
        path: '/404',
        name: 'notfound',
        component: () => import('@/views/Error/NotFound.vue')
      }
      // { // 为什么写了要注释掉。写是因为404需要动态匹配并重定向才行。注释掉是因为动态路由的原因，把这个路由用router.addRoute的方式引入进来了。
      //   path: '/:pathMatch(.*)',
      //   name: '404',
      //   redirect: '/404',
      //   component: () => import('@/views/Error/NotFound.vue')
      // }
    ]
  },
  { // 非内页，不需要layout布局的放外面，如登录，大屏等
    path: '/login',
    name: 'login',
    component: () => import('@/views/Login/Index.vue')
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router

```

#### 权限校验
这里应该还有一个permission.js的文件来校验路由权限。 不同项目、不同团队间的权限校验方式应该是不一样的。这里只需要参考一下。

```javascript
// src/permission.js
import router from '@/router'
import { getToken } from '@/utils/webStorage'
import { useUserStore } from '@/pinia'

// 白名单
const whiteList = ['/login']
// 是否已加载路由
let routerIsLoaded = false
// 是否加载本地路由
const isLocal = import.meta.env.VITE_API_ISLOCAL === 'true'

// 加载路由
export const loadRoute = async () => {
  const userStore = useUserStore()
  await userStore.getUserInfo(isLocal)
  routerIsLoaded = true
}

// 路由导航
router.beforeEach(async (to, from, next) => {

  // 设置页面标题
  if (to.meta.title) {
    document.title = to.meta.title
  }

  // 跳转到登录页时，重置加载路由
  if (to.path === '/login') {
    routerIsLoaded = false
  }

  // 白名单内直接跳转直接进入
  if (whiteList.includes(to.path)) {
    next()
    return
  }

  // 判断是否登录
  if (getToken() || isLocal) {
    // 判断路由是否加载
    if (routerIsLoaded) {
      // 路由已加载时，匹配到路由 ? 则直接进入 :跳转到404
      if (to.matched.length) {
        next()
      } else {
        next('/404')
      }
    } else {
      // 路由未加载时则等待路由加载完成后，进行下一步
      await loadRoute()
      next({ ...to, replace: true })
    }
  } else {
    // 未登录跳转到登录页
    next({ path: '/login' })
  }
})


```

<font style="color:rgb(43, 43, 43);">路由加载通过pinia里的user模块的getUserInfo接口做统一处理。</font>

```javascript
import { getUserInfo as getUserInfoStorage, setUserInfo as setUserInfoStorage } from '@/utils/webStorage'
import { getUserInfo as apiGetUserInfo } from '@/api/user'

import router from '@/router'
import routes from '@/router/router.config'
import { flattenTree } from '@/utils/dealData'

const useUserStore = defineStore('user', {
  state: () => ({
    userInfo: getUserInfoStorage(),
    buttons: [],
    routes: []
  }),
  actions: {
    getUserInfo(isLocal) {
      return new Promise((resolve, reject) => {
        if (isLocal) {
          // 本地模式下，不使用接口，直接记载本地路由
          this.userInfo = {}
          this.buttons = []
          this.routes = [...routes]
          flattenTree(routes).forEach(item => {
            router.addRoute('layout', item)
          })
          router.addRoute({
            path: '/:pathMatch(.*)',
            name: '404',
            redirect: '/404',
            component: () => import('@/views/Error/NotFound.vue')
          })
          resolve()
        } else {
          // 非本地模式下，通过接口获取路由权限
          getInfo().then(res => {
            this.userInfo = res.data.userInfo
            this.buttons = res.data.buttons
            this.routes = res.data.routes
            flattenTree(routes).forEach(item => {
              router.addRoute('layout', item)
            })
            router.addRoute({
              path: '/:pathMatch(.*)',
              name: '404',
              redirect: '/404',
              component: () => import('@/views/Error/NotFound.vue')
            })
            resolve()
          })
        }
      })
    },
    logout() {
      this.userInfo = {}
      this.buttons = []
      this.routes = []
      sessionStorage.clear()
      router.push('/login')
    }
  }
})

export default useUserStore

```



### axios
```bash
pnpm i axios

```

```bash
├─ 📁src
│  ├─ 📁api
│  │  └─ 📄user.js
│  ├─ 📁utils
│  │  ├─ 📄request.js

```

```javascript

import axios from 'axios'

import { getToken } from '@/utils/webStorage'
import qs from 'qs'

const service = axios.create({
  baseURL: '/api',
  withCredentials: false, // 是否携带cookie
  timeout: 3000 // 超时响应
})

// 字节流下载flag
let isBlob = false

// axios拦截器 - 请求头拦截
service.interceptors.request.use(
  config => {
    // 根据请求头判断是否为字节流
    isBlob = config.responseType === 'blob'

    // 处理头部信息：添加token
    config.headers.Authorization = getToken()

    // 请求为表单时
    if (config.contentType === 'form') {
      config.headers['Content-Type'] = 'application/x-www-form-urlencoded'
    }

    // get请求包含数组时,参数进行qs序列化
    if (config.method === 'get') {
      config.paramsSerializer = params => {
        return qs.stringify(params, { arrayFormat: 'repeat' })
      }
    }

    // 请求参数为空时默认给个空对象，undefined和null有一些后端会说报错。
    config.data = config.data || {}
    config.params = config.params || {}
    return config
  },
  error => {
    return Promise.reject(error)
  }
)

// axios拦截器 - 响应拦截
service.interceptors.response.use(
  response => {
    // 如果为字节流，直接抛出字节流，不走code验证
    if (isBlob) {
      return response
    }

    const res = response.data

    // 后台返回非200处理
    if (res.code !== 200) {
      ElMessage({
        message: res.msg || 'Error',
        type: 'error',
        duration: 5 * 1000
      })

      // 401: token失效
      // 这里需要跟后端大佬约定好，一般来说存在几个状态: 
      // 1.登录时间过长导致的本地token过期
      // 2.异地同账号登录导致的本地token失效（单点登录）
      // 3.账号封禁或者其他状态本地token校验失败
      if (res.code === 401) {
        ElMessageBox.confirm('登录失效，请重新登录', '提示', {
          confirmButtonText: '重新登录',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          const router = useRouter()
          router.push('/login') // token的问题都这里处理，并跳转到登录页
        })
      }
      return Promise.reject(new Error(res.msg || 'Error'))
    } else {
      return res
    }
  },
  error => {
    // 异常抛出错误并弹个消息
    ElMessage({
      message: error,
      type: 'error',
      duration: 5 * 1000
    })
    return Promise.reject(error)
  }
)

export default service

```

<font style="color:rgb(43, 43, 43);">这里使用了</font>`<font style="color:rgb(38, 198, 218);">qs</font>`<font style="color:rgb(43, 43, 43);">做url参数序列化处理。</font>

```bash
pnpm i qs
```

#### 接口示例
```javascript

import request from '@/utils/request'

// 登录
export function login(data) {
  return request({
    url: '/auth/login',
    method: 'post',
    data // 参数在body用data
  })
}

// 获取用户信息
export function getUserInfo(params) {
  return request({
    url: '/auth/getUserInfo',
    method: 'get',
    params // 参数在url上用params
  })
}

// 导出表格
export function exportTable(params) {
  return request({
    url: '/xxx/xxxx',
    method: 'get',
    responseType: 'blob', // 自己封装的下载
    params
  })
}

```

<font style="color:rgb(43, 43, 43);">使用示例</font>

```javascript
import { login } from '@/api/user

const baseInfo = reactive({
  form: {
    username: '',
    password: ''
  },
  rules: {},
  loading: false
})

login(baseInfo.form).then(res => {
   console.log(res)
})


```

#### mockjs
这里如果后端接口长期跟不上，建议是使用mockjs去模拟数据。是否使用完全根据自己团队的现状，除非是演示型demo， 

由于是vite，这里使用了vite-plugin-mock，可以查看[mock官方文档](http://mockjs.com/examples.html)和[vite-plugin-mock](https://github.com/vbenjs/vite-plugin-mock/blob/main/README.zh_CN.md)官方文档，其中mock官方文档包含模拟数据该如何编写，vite-plugin-mock官方文档包含了如何安装搭建mock，这里就不做赘述了。

 

#### 按需引入
```javascript

// 引入 echarts 核心模块，核心模块提供了 echarts 使用必须要的接口。
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

// 注册必须的组件
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



#### 自定义主题
<font style="color:rgb(43, 43, 43);">注意上一个按需引用文件，我使用了自定义主题，这样就不需要对每一个图标都去设置公用的属性。自定义主题是由官网提供的能力。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728438707225-fc55362f-07d6-46e8-a5c6-25a586dd84f1.png)



<font style="color:rgb(43, 43, 43);">这里需要注意的是三个按钮，下载主题是主题文件，直接导入。剩下的导入配置和导出配置是把你的配置项到出来，下次如果在此基础上修改，导出配置之后再次导入就会回滚到你上次用的配置项。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728438969922-b1cfcacc-aa2a-48a5-9b1a-dbe4388e5a7d.png)

<font style="color:rgb(43, 43, 43);">官网提供的能力一般情况下是够用了，但是针对特殊的情况，可能还不够。这个时候可以打开主题文件，进行修改</font>

<font style="color:rgb(43, 43, 43);"></font>

#### 全局hooks
<font style="color:rgb(43, 43, 43);">这里还处理过网站主题变化时样式修改调用的问题，tab标签切换下样式问题等。hooks真的好用。</font>

```javascript

import echarts from '@/echarts'
import { debounce } from 'lodash-es'

const useEchartHooks = id => {
  let chart = null
  let options = null

  const renderChart = val => {
    return new Promise((resolve, reject) => {
      options = val
      if (!chart) {
        chart = echarts.init(document.getElementById(id), 'customed')
        resolve(chart)

        // nextTick防止样式未组装完成就开始渲染页面
        nextTick(() => {
          chart.setOption(options, true)
        })
      }

      // nextTick防止样式未组装完成就开始渲染页面
      nextTick(() => {
        chart.setOption(options)
      })
    })
  }

  const state = reactive({
    resize: null
  })

  const sidebarResize = e => {
    if (e.propertyName === 'width') {
      state.resize()
    }
  }

  const initListener = () => {
    state.resize = debounce(() => {
      resize()
    }, 100)
    window.addEventListener('resize', state.resize)

    // 监听侧边菜单栏-宽度
    state.sidebarEle = document.querySelector('.g-sider')
    state.sidebarEle && state.sidebarEle.addEventListener('transitionend', sidebarResize)
  }
  const destroyListener = () => {
    window.removeEventListener('resize', state.resize)
    state.resize = null

    state.sidebarEle && state.sidebarEle.removeEventListener('transitionend', sidebarResize)
  }

  const resize = () => {
    chart && chart.resize()
  }
  onMounted(() => {
    initListener()
  })

  onActivated(() => {
    if (!state.resize) {
      initListener()
    }
  })

  onBeforeUnmount(() => {
    destroyListener()
    chart && chart.dispose()
    chart = null
  })

  onDeactivated(() => {
    destroyListener()
  })
  return { renderChart }
}

export default useEchartHooks

```

<font style="color:rgb(43, 43, 43);">使用示例</font>

```vue
<template>
  <div id="trendLine" style="width: 100%; height: 200px"></div>
</template>

<script setup>
  import { getTrendData } from '@/api/dasheboard'
  import useEchartHooks from '@/hooks/echarts'

  const echart = reactive({
    data: [
      { name: '当前', type: 'bar', data: [] },
      { name: '环比', type: 'line', data: [] },
      { name: '同比', type: 'line', data: [] }
    ]
  })

  const onSearch = () => {
    getTrendData(search.form).then(res => {
      echart.data = res.data
      initChart()
    })
  }

  const { renderChart } = useEchartHooks('earningLine')
  const initChart = () => {
    const options = {
      tooltip: {
        trigger: 'axis'
      },
      xAxis: {
        type: 'category',
        data: echart.data.map(item => item.name)
      },
      yAxis: {
        type: 'value'
      },
      series: echart.data.map(item => {
        return {
          name: item.name,
          type: item.type,
          data: item.data
        }
      })
    }
    renderChart(options)
  }
</script>

```

### 工具类
工具类可以使用开源的lodash-es和vueuse这两个，其他不推荐。小型项目其实可以自己封装，取决于自己。



```javascript

export function getToken() {
  return sessionStorage.getItem('token') ?? ''
}
export function setToken(token) {
  sessionStorage.setItem('token', token)
}
export function getUserInfo() {
  const data = sessionStorage.getItem('userInfo') ?? '{}'
  return JSON.parse(data)
}
export function setUserInfo(data) {
  const dataStr = JSON.stringify(data || {})
  sessionStorage.setItem('userInfo', dataStr)
}

// 侧边栏开关
export function setSiderCollapse(collapse) {
  sessionStorage.setItem('siderCollapse', collapse)
}
export function getSiderCollapse() {
  const collapse = sessionStorage.getItem('siderCollapse')
  return collapse === 'true'
}

export function clearStore() {
  sessionStorage.clear()
}

```

```javascript
/**
 * 将树结构数据扁平化处理。
 * @param {Array} tree 原始树
 * @param {string} childrenKey 子节点的键名
 * @returns {Array} 返回扁平化的数组
 */
export function flattenTree(tree, childrenKey = 'children') {
  const result = []

  // 遍历树结构的递归函数
  function traverse(arr) {
    arr.forEach(item => {
      const { [childrenKey]: children, ...data } = item // 解构分离
      result.push(data)
      if (children && children.length) {
        traverse(children)
      }
    })
  }

  traverse(tree)

  return result
}

/**
 * 清洗tree，提取有效字段
 * @param {Array} tree 原始树
 * @param {Function} callback 节点数据处理回调
 * @param {String} childrenKey 子节点原始key值
 * @param {String} newChildrenKey 子节点新key值
 * @returns {Array} 清洗后的树
 */
export function cleanTree(tree, callback, childrenKey = 'children', newChildrenKey = 'children') {
  const traverse = tree => {
    const result = []
    tree.forEach(node => {
      const { [childrenKey]: children, ...item } = node
      const newNode = callback(item)
      if (children && children.length) {
        newNode[newChildrenKey] = traverse(children)
      }
      result.push(newNode)
    })
    return result
  }

  return traverse(tree)
}


```

#### 文件下载
```javascript

/**
 * 导出文件-字节流
 * @param {Object} res 字节流
 * @param {string} fileName 文件名
 */
export function exportFile(res, fileName) {
  const file = res.data
  const name = decodeURI(res.headers.filename)
  // 兼容IE浏览器
  if (!!window.ActiveXObject || 'ActiveXObject' in window) {
    const blob = new Blob([file])
    window.navigator.msSaveOrOpenBlob(blob, fileName)
  } else {
    const objectUrl = window.URL.createObjectURL(new Blob([file]))
    const link = document.createElement('a')
    link.download = fileName || name
    link.href = objectUrl
    link.click()
    setTimeout(() => {
      document.body.removeChild(link)
    }, 3000)
  }
}
/**
 * 下载文件-链接下载
 * @param {string} url 下载地址
 * @param {string} fileName 文件名
 */
export function downFile(url, fileName) {
  const link = document.createElement('a')
  link.style.display = 'none'

  link.href = url
  link.download = fileName

  document.body.appendChild(link)
  link.click()
  setTimeout(() => {
    document.body.removeChild(link)
  }, 3000)
}


```

