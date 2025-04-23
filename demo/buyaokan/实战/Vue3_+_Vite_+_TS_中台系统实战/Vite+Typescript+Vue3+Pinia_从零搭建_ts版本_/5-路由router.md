## 1. 安装
```shell
npm i vue-router@4
```

## 2. 集成
### <font style="color:rgb(51, 51, 51);">1. 新建两页面进行示例</font>
<font style="color:rgb(51, 51, 51);">在</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">src/view</font>`<font style="color:rgb(51, 51, 51);">下新建</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">home.vue</font>`<font style="color:rgb(51, 51, 51);">和</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">login.vue</font>`<font style="color:rgb(51, 51, 51);">，内容如下：</font>

```html
<script setup lang="ts">
defineOptions({
  name: 'V-home'
})
</script>
 
<template>
  <div>home page</div>
</template>
 
<style scoped></style>
```

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">login.vue</font>`<font style="color:rgb(51, 51, 51);">里修改下对应name即可</font>

### <font style="color:rgb(51, 51, 51);">2.</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">src</font>`<font style="color:rgb(51, 51, 51);">下新建</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">router</font>`<font style="color:rgb(51, 51, 51);">文件夹</font>
`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">index.ts</font>`<font style="color:rgb(51, 51, 51);">作为路由入口，</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">static.ts</font>`<font style="color:rgb(51, 51, 51);">作为静态路由，</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">modules</font>`<font style="color:rgb(51, 51, 51);">内还可以放入其他类型路由，整体目录结构如下：</font>

```markdown
src
|   
+---router
|   |   index.ts
|   +---modules
|       |   static.ts
```

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">static.ts</font>`<font style="color:rgb(51, 51, 51);">内容如下：</font>

```javascript
const routes = [
  {
    path: '/',
    component: () => import('@/views/home.vue')
  },
  {
    path: '/login',
    component: () => import('@/views/login.vue') //路由懒加载
  }
]

export default routes
```

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">index.ts</font>`<font style="color:rgb(51, 51, 51);">内容如下：</font>

```javascript
import { Router, createRouter, createWebHistory } from 'vue-router'

/** 自动导入 src/router/modules 下的静态路由
 * import.meta.glob使用说明：https://cn.vitejs.dev/guide/features#glob-import
 */
const modules: Record<string, any> = import.meta.glob(['./modules/**/*.ts'], {
  eager: true
})

/** 初始路由 **/
const routes: any[] = []

Object.keys(modules).forEach((key) => {
  const module = modules[key].default
  if (Array.isArray(module)) {
    for (const item of module) {
      routes.push(item)
    }
  } else {
    routes.push(module)
  }
})

/**
 * 创建路由实例
 * createRouter选项有：https://router.vuejs.org/zh/api/interfaces/RouterOptions.html
 * hash模式使用createWebHashHistory(): https://router.vuejs.org/zh/api/#Functions-createWebHashHistory
 */
export const router: Router = createRouter({
  history: createWebHistory(),
  routes,
  strict: true,
  scrollBehavior(_to, from, savedPosition) {
    return new Promise((resolve) => {
      if (savedPosition) {
        return savedPosition
      } else {
        if (from.meta.saveSrollTop) {
          const top: number = document.documentElement.scrollTop || document.body.scrollTop
          resolve({ left: 0, top })
        }
      }
    })
  }
})

/**
 * 路由守卫
 * https://router.vuejs.org/zh/guide/advanced/navigation-guards.html
 */
router.beforeEach((to, _from, next) => {
  // isAuthenticated 代表你的鉴权
  const isAuthenticated = true
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})

export default router
```

### <font style="color:rgb(51, 51, 51);">3. 修改</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">App.vue</font>`
<font style="color:rgb(51, 51, 51);">承载路由，并增加导航</font>

```html
<script setup lang="ts"></script>
 
<template>
  <router-link to="/"> 去首页 </router-link> 丨 <router-link to="/login"> 去登录 </router-link>
  <router-view />
</template>
 
<style scoped></style>
```

### <font style="color:rgb(51, 51, 51);">4. 修改</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">main.ts</font>`
<font style="color:rgb(51, 51, 51);">使用路由</font>

```javascript
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'

import router from '@/router/index'

createApp(App).use(router).mount('#app')
```

## 3. 预览
![](https://cdn.nlark.com/yuque/0/2024/gif/207857/1730706381215-535e0dd0-35c9-4679-97d2-1bb71db5e8fb.gif)<font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(51, 51, 51);">其他用法，包括传参、重定向、动态路由、过渡动效等，请参考</font>[**<font style="color:#117CEE;">官方文档</font>**](https://router.vuejs.org/zh/)**<font style="color:#117CEE;">。</font>**

