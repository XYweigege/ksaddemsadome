## 安装
```shell
npm i axios
```

## 封装
`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">utils</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">目录下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">request</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件夹，并新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">index.ts</font>`<font style="color:rgb(51, 51, 51);">、</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">request.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">和</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">status.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件。</font>

### <font style="color:rgb(51, 51, 51);">1.</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">status.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件主要是封装状态码</font>
```javascript
export const ErrMessage = (status: number | string): string => {
  let message: string = ''
  switch (status) {
    case 400:
      message = '请求错误！请您稍后重试'
      break
    case 401:
      message = '未授权！请您重新登录'
      break
    case 403:
      message = '当前账号无访问权限！'
      break
    case 404:
      message = '访问的资源不存在！请您稍后重试'
      break
    case 405:
      message = '请求方式错误！请您稍后重试'
      break
    case 408:
      message = '请求超时！请您稍后重试'
      break
    case 500:
      message = '服务异常！请您稍后重试'
      break
    case 501:
      message = '不支持此请求！请您稍后重试'
      break
    case 502:
      message = '网关错误！请您稍后重试'
      break
    case 503:
      message = '服务不可用！请您稍后重试'
      break
    case 504:
      message = '网关超时！请您稍后重试'
      break
    default:
      message = '请求失败！请您稍后重试'
  }
  return message
}
```

<font style="color:rgb(51, 51, 51);">此时，eslint会报</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">switch</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">前面的空格错误，需要修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.eslintrc.cjs</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">indent</font>`<font style="color:rgb(51, 51, 51);">，修改后，错误消失。</font>

```javascript
rules: {
  // Switch语句 https://zh-hans.eslint.org/docs/latest/rules/indent#switchcase
  indent: ['error', 2, { SwitchCase: 1 }]
}
```

### <font style="color:rgb(51, 51, 51);">2.</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">request.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">主要是封装</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">axios</font>`
```javascript
/**
 * 封装axios
 * axios 实例的类型为 AxiosInstance，请求需要传入的参数类型为 AxiosRequestConfig，响应的数据类型为 AxiosResponse，InternalAxiosRequestConfig 继承于 AxiosRequestConfig
 */
import axios, { AxiosInstance, AxiosRequestConfig, InternalAxiosRequestConfig, AxiosResponse } from 'axios'
import { ErrMessage } from './status'

// 自定义请求返回数据的类型
interface Data<T> {
  data: T
    code: string
success: boolean
}

// 扩展 InternalAxiosRequestConfig，让每个请求都可以控制是否要loading
interface RequestInternalAxiosRequestConfig extends InternalAxiosRequestConfig {
  showLoading?: boolean
    }

// 拦截器
interface InterceptorHooks {
  requestInterceptor?: (config: RequestInternalAxiosRequestConfig) => RequestInternalAxiosRequestConfig
    requestInterceptorCatch?: (error: any) => any
    responseInterceptor?: (response: AxiosResponse) => AxiosResponse
  responseInterceptorCatch?: (error: any) => any
    }
// 扩展 AxiosRequestConfig，showLoading 给实例默认增加loading，interceptorHooks 拦截
interface RequestConfig extends AxiosRequestConfig {
  showLoading?: boolean
    interceptorHooks?: InterceptorHooks
    }

class Request {
  config: RequestConfig
  instance: AxiosInstance
  loading?: boolean // 用loading指代加载动画状态

  constructor(options: RequestConfig) {
    this.config = options
    this.instance = axios.create(options)
    this.setupInterceptor()
  }

  // 类型参数的作用，T决定AxiosResponse实例中data的类型
  request<T = any>(config: RequestConfig): Promise<T> {
    return new Promise((resolve, reject) => {
      this.instance
        .request<any, Data<T>>(config)
        .then((res) => {
          resolve(res.data)
        })
        .catch((err) => {
          reject(err)
        })
    })
  }

  // 封装常用方法
  get<T = any>(url: string, params?: object, _object = {}): Promise<T> {
    return this.request({ url, params, ..._object, method: 'GET' })
  }

  post<T = any>(url: string, params?: object, _object = {}): Promise<T> {
    return this.request({ url, params, ..._object, method: 'POST' })
  }

  delete<T = any>(url: string, params?: object, _object = {}): Promise<T> {
    return this.request({ url, params, ..._object, method: 'DELETE' })
  }

  patch<T = any>(url: string, params?: object, _object = {}): Promise<T> {
    return this.request({ url, params, ..._object, method: 'PATCH' })
  }

  put<T = any>(url: string, params?: object, _object = {}): Promise<T> {
    return this.request({ url, params, ..._object, method: 'PUT' })
  }

  // 自定义拦截器 https://axios-http.com/zh/docs/interceptors
  setupInterceptor(): void {
    /**
     * 通用拦截
     */
    this.instance.interceptors.request.use((config: RequestInternalAxiosRequestConfig) => {
      if (config.showLoading) {
        // 加载loading动画
        this.loading = true
      }
      return config
    })
    // 响应后关闭loading
    this.instance.interceptors.response.use(
      (res) => {
        if (this.loading) this.loading = false
        return res
      },
      (err) => {
        const { response, message } = err
        if (this.loading) this.loading = false
          // 根据不同状态码，返回不同信息
        const messageStr = response ? ErrMessage(response.status) : message || '请求失败，请重试'
        window.alert(messageStr)
        return Promise.reject(err)
      }
    )
    /**
     * 使用通用实例里的拦截，两个拦截都会生效，返回值以后一个执行的为准
     */
    // 请求拦截
    this.instance.interceptors.request.use(
      this.config?.interceptorHooks?.requestInterceptor,
      this.config?.interceptorHooks?.requestInterceptorCatch
    )
    // 响应拦截
    this.instance.interceptors.response.use(
      this.config?.interceptorHooks?.responseInterceptor,
      this.config?.interceptorHooks?.responseInterceptorCatch
    )
  }
}
 
export default Request
```

### <font style="color:rgb(51, 51, 51);">3.</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">index.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">主要是创建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">Request</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">实例</font>
```javascript
/**
 * 创建实例，可以多个，当你需要请求多个不同域名的接口时
 */
import Request from './request'
import { getToken } from '@/utils/auth'

const defRequest = new Request({
  // 这里用 Easy Mock 模拟了真实接口
  baseURL: 'https://mock.mengxuegu.com/mock/65421527a6dde808a695e96d/official/',
  timeout: 5000,
  showLoading: true,
  interceptorHooks: {
    requestInterceptor: (config) => {
      const token = getToken()
      if (token) {
        config.headers.Authorization = token
      }
      return config
    },
    requestInterceptorCatch: (err) => {
      return err
    },
    responseInterceptor: (res) => {
      return res.data
    },
    responseInterceptorCatch: (err) => {
      return Promise.reject(err)
    }
  }
})

// 创建其他示例，然后导出
// const otherRequest = new Request({...})

export { defRequest }
```

## 使用
`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">src</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">目录下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">api</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件夹，并新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">login.ts</font>`

### <font style="color:rgb(51, 51, 51);">1.</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">login.ts</font>`
```javascript
import { defRequest } from '../utils/request'

export const loginApi = (params: any) => {
  // 设置 showLoading，timeout 会覆盖index.ts里的默认值
  return defRequest.post<any>('/login', params, { showLoading: false, timeout: 1000 })
}
```

### <font style="color:rgb(51, 51, 51);">2. 修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">login.vue</font>`
```html
<script setup lang="ts">
import { ref } from 'vue'
import { storeToRefs } from 'pinia'
import { useUserStore } from '@store/user'
import { loginApi } from '@/api/login'
 
defineOptions({
  name: 'V-login'
})
 
const userStore = useUserStore()
const { userInfo, token } = storeToRefs(userStore)
let userName = ref(userInfo.value.name)
let userToken = ref(token)
 
const updateUserName = () => {
  userStore.setUserInfo({
    name: userName.value
  })
}
const updateUserToken = () => {
  userStore.setToken(userToken.value)
}
 
const login = () => {
  loginApi({
    name: userName.value
  })
    .then((res) => {
      userName.value = res.name
      userToken.value = res.token
      updateUserToken()
    })
    .catch((err) => {
      console.log(err)
    })
}
</script>
 
<template>
  <div>login page</div>
  name:
  <input type="text" v-model="userName" @input="updateUserName" />
  <br />
  token:
  <input type="text" v-model="userToken" />
  <hr />
  <button @click="login">login</button>
</template>
 
<style scoped></style>
```

<font style="color:rgb(51, 51, 51);">点击</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">login</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">按钮，即可看到请求。  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730706656213-07fb4393-6bcc-40ae-8e02-c3f2fa4caaa8.png)

## 说明
<font style="color:rgb(51, 51, 51);">对于</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">axios</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">的封装和使用，这里要说明几点：</font>

### <font style="color:rgb(51, 51, 51);">1. 为什么要使用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">InternalAxiosRequestConfig</font>`
<font style="color:rgb(51, 51, 51);">axios 源码有修改，拦截器传入和返回的参数不再是</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">AxiosRequestConfig</font>`<font style="color:rgb(51, 51, 51);">，而是这个新类型</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">InternalAxiosRequestConfig</font>`<font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(51, 51, 51);">想要具体了解，可以查看这篇博文</font><font style="color:rgb(51, 51, 51);"> </font>[<font style="color:rgb(51, 51, 51);">https://blog.csdn.net/huangfengnt/article/details/131490913</font>](https://blog.csdn.net/huangfengnt/article/details/131490913)

### <font style="color:rgb(51, 51, 51);">2.</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">Request</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">config</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">参数</font>
<font style="color:rgb(51, 51, 51);">constructor 里的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">this.config</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">会接受所有实例参数，所以通用实例拦截里使用的是</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">this.config?.xxx</font>`<font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(51, 51, 51);">通用拦截里使用的是</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">config.showLoading</font>`<font style="color:rgb(51, 51, 51);">，而不是</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">this.config.showLoading</font>`<font style="color:rgb(51, 51, 51);">，是为了我们在实际的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">api/login.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里可以再传入</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">showLoading</font>`<font style="color:rgb(51, 51, 51);">，以满足我们单个请求的要求。而通过</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">this.config</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里获取的配置是</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">request/index.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里传入的配置。在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">config.showLoading</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">之前我们可以打印下这两个</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">config</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">，</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">console.log(this.config, config)</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">结果如下：  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730706656094-74d6e542-5f6e-469f-b395-7d8d9acd7c94.png)

<font style="color:rgb(51, 51, 51);">如果在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">login.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里不传入</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">showLoading</font>`<font style="color:rgb(51, 51, 51);">，那么</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">config.showLoading</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">会去拿通用实例</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">request/index.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">showLoading</font>`<font style="color:rgb(51, 51, 51);">。  
</font><font style="color:rgb(51, 51, 51);">** 当然如果不需要全局加载动画，整个</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">loading</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">也都可以去掉 **</font>

### <font style="color:rgb(51, 51, 51);">3. 总结下</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">request/index.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">和</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">api/login.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里的参数有什么不同</font>
`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">request/index.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里可以建多个实例，一般以</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">baseURL</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">来判断是否要多个，它的参数是当前url下的通用参数，拦截规则也是；  
</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">api/login.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">是具体的请求，它的大部分参数是url和请求传参。同一个</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">baseURL</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">下有的请求有特殊的要求，那你就可以去加一些参数。  
</font><font style="color:rgb(51, 51, 51);">总的来说，</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">request/index.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">是对</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">baseURL</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">一样的请求的封装，</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">request/request.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">是对所有请求的封装</font>

### <font style="color:rgb(51, 51, 51);">4. 优化</font>
+ <font style="color:rgb(51, 51, 51);">因为 Easy Mock 的接口支持跨域，所以没有配到代理里去，如果是正常开发接口，还需要修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">vite.config.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">proxy</font>`<font style="color:rgb(51, 51, 51);">。不过我们之前的教程里已有代理配置说明，这里便不再赘述</font>
+ `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">baseURL</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">还可以放在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">env</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">变量里，以便区分开发环境和生产环境</font>
+ <font style="color:rgb(51, 51, 51);">** 删除 </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">loading</font>`<font style="color:rgb(51, 51, 51);">，这里只是为了提供一种思路</font><font style="color:rgb(51, 51, 51);">😂</font><font style="color:rgb(51, 51, 51);"> **</font>

