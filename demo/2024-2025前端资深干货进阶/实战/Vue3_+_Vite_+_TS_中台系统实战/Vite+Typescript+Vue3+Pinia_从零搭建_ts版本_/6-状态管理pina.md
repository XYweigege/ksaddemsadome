## 特点
+ <font style="color:rgb(51, 51, 51);">更简洁直接的 API，提供组合式风格的 API</font>
+ <font style="color:rgb(51, 51, 51);">支持模块热更新和服务端渲染</font>
+ <font style="color:rgb(51, 51, 51);">对TS支持更为友好</font>

## 安装
```shell
npm i pinia
```

## 使用
### <font style="color:rgb(51, 51, 51);">1. 创建实例</font>
<font style="color:rgb(51, 51, 51);">src目录下新建store文件夹，并新建index.ts文件</font>

```javascript
import { createPinia } from 'pinia'

const store = createPinia()

export default store
```

### <font style="color:rgb(51, 51, 51);">2. 使用实例</font>
<font style="color:rgb(51, 51, 51);">在main.ts里引入并使用</font>

```javascript
import { createApp } from 'vue'
import pinia from '@/store'
import './style.css'
import App from './App.vue'

import router from '@/router/index'

createApp(App).use(router).use(pinia).mount('#app')
```

### <font style="color:rgb(51, 51, 51);">3. 创建store</font>
<font style="color:rgb(51, 51, 51);">types文件夹下创建store.ts类型声明</font>

```javascript
export interface UserState {
  token: string
  userInfo: { name?: string; phone?: string }
}
```

<font style="color:rgb(51, 51, 51);">store目录下新建modules文件夹，并新建user.ts文件</font>

```javascript
import { defineStore } from 'pinia'
import { UserState } from 'types/store'

// 第一个参数是id，唯一
export const useUserStore = defineStore('user', {
  state: () => {
    return {
      token: 'EFA68205747CB561BB7C0F85D5689856',
      userInfo: { name: 'weizwz', phone: '18392016879' }
    }
  },
  getters: {
    namePic: (state) => state.userInfo.name.substring(0, 1)
  },
  actions: {
    setToken(token: string) {
      this.token = token
    },
    setUserInfo(userInfo: UserState['userInfo']) {
      this.userInfo = { ...this.userInfo, ...userInfo }
    }
  }
})
```

### <font style="color:rgb(51, 51, 51);">4. 使用store</font>
<font style="color:rgb(51, 51, 51);">修改view文件夹下的home.vue来获取store</font>

```html
<script setup lang="ts">
import { computed } from 'vue'
import { storeToRefs } from 'pinia'
import { useUserStore } from '@store/user'
 
defineOptions({
  name: 'V-home'
})
 
const userStore = useUserStore()
// 获取state使用computed或者使用storeToRefs，直接使用不具备响应式（拿到的永远是初次的值）
const username = computed(() => userStore.userInfo.name)
// 获取getter使用storeToRefs，或者直接使用，在模板里 userStore.namePic
const { namePic, token } = storeToRefs(userStore)
</script>
 
<template>
  <div>Hello: {{ namePic }}, your name is {{ username }}, your token is {{ token }}</div>
</template>
 
<style scoped></style>
```

<font style="color:rgb(51, 51, 51);">修改view文件夹下的login.vue来设置store</font>

```html
<script setup lang="ts">
import { ref } from 'vue'
import { storeToRefs } from 'pinia'
import { useUserStore } from '@store/user'
 
defineOptions({
  name: 'V-login'
})
 
const userStore = useUserStore()
const { userInfo, token } = storeToRefs(userStore)
const userName = ref(userInfo.value.name)
const userToken = ref(token)
 
const updateUserName = () => {
  userStore.setUserInfo({
    name: userName.value
  })
}
const updateUserToken = () => {
  userStore.setToken(userToken.value)
}
</script>
 
<template>
  <div>login page</div>
  name:
  <input type="text" v-model="userName" @input="updateUserName" />
  <br />
  token:
  <input type="text" v-model="userToken" @input="updateUserToken" />
</template>
 
<style scoped></style>
```

**<font style="color:rgb(51, 51, 51);">如果</font>**`**<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">@store/user</font>**`**<font style="color:rgb(51, 51, 51);">不能识别，请在</font>**`**<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">tsconfig.json</font>**`**<font style="color:rgb(51, 51, 51);">中的</font>**`**<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">paths</font>**`**<font style="color:rgb(51, 51, 51);">里加入此路径</font>**

```json
"paths": {
  "@store/*": ["src/store/modules/*"],
},
```

## 持久化
<font style="color:rgb(51, 51, 51);">由于刷新界面会导致store重置，所以一般通过将store存储到cookie或者storage里使其持久化。cookie方面的插件，我这里推荐使用</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">js-cookie</font>`

### <font style="color:rgb(51, 51, 51);">1. 安装</font>
```shell
npm i js-cookie -S
npm i @types/js-cookie -S
```

### <font style="color:rgb(51, 51, 51);">2.封装</font>
<font style="color:rgb(51, 51, 51);">在src下新建utils文件夹，并新建auth.ts</font>

```javascript
import Cookies from 'js-cookie'

export const TokenKey = 'weiz-token'

type ExpiresData = Date | number
export interface TokenInfo {
  token: string
  expires: ExpiresData
}

export function getToken() {
  return Cookies.get(TokenKey)
}

export function setToken(data: TokenInfo) {
  const { token, expires } = data
  return expires ? Cookies.set(TokenKey, token, { expires: expires }) : Cookies.set(TokenKey, token)
}

export function removeToken() {
  return Cookies.remove(TokenKey)
}
```

### <font style="color:rgb(51, 51, 51);">3. 使用</font>
<font style="color:rgb(51, 51, 51);">修改store下的user.ts</font>

```javascript
import { getToken, setToken } from '@/utils/auth'
// 省略
state: () => {
  return {
    token: getToken() || 'EFA68205747',
    userInfo: { name: 'weizwz', phone: '18392016879' }
  }
}
// 省略
setToken(token: string) {
  this.token = token
  setToken({
    token,
    expires: 30
  })
}
```

## 效果展示
![](https://cdn.nlark.com/yuque/0/2024/gif/207857/1730706460981-7e824fb4-9d06-471e-98e3-47e24f9ce492.gif)

