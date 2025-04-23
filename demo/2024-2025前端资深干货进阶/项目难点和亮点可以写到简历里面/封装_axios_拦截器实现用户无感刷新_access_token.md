## <font style="color:rgb(31, 35, 40);">背景</font>
<font style="color:rgb(31, 35, 40);">最近做项目的时候，涉及到一个单点登录，即是项目的登录页面，用的是公司共用的一个登录页面，在该页面统一处理逻辑。最终实现用户只需登录一次，就可以以登录状态访问公司旗下的所有网站。</font>

> 单点登录( Single Sign On ，简称 SSO），是目前比较流行的企业业务整合的解决方案之一，用于多个应用系统间，用户只需要登录一次就可以访问所有相互信任的应用系统。
>

<font style="color:rgb(31, 35, 40);">其中本文讲的是在登录后如何管理</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">和</font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);">，主要就是封装 axios 拦截器，从前端角度去分析</font>

## <font style="color:rgb(31, 35, 40);">需求</font>
+ <font style="color:rgb(31, 35, 40);">前置场景</font>
1. <font style="color:rgb(31, 35, 40);">进入该项目某个页面</font>`<font style="color:rgb(31, 35, 40);">http://xxxx.project.com/profile</font>`<font style="color:rgb(31, 35, 40);">需要登录，未登录就跳转至 SSO 登录平台，此时的登录网址 url 为</font>`<font style="color:rgb(31, 35, 40);">http://xxxxx.com/login?app_id=project_name_id&redirect_url=http://xxxx.project.com/profile</font>`<font style="color:rgb(31, 35, 40);">，其中</font>`<font style="color:rgb(31, 35, 40);">app_id</font>`<font style="color:rgb(31, 35, 40);">是后台那边约定定义好的，</font>`<font style="color:rgb(31, 35, 40);">redirect_url</font>`<font style="color:rgb(31, 35, 40);">是成功授权后指定的回调地址。</font>
2. <font style="color:rgb(31, 35, 40);">输入账号密码且正确后，就会重定向回刚开始进入的页面，并在地址栏带一个参数</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">?code=XXXXX</font>`<font style="color:rgb(31, 35, 40);">，即是</font>`<font style="color:rgb(31, 35, 40);">http://xxxx.project.com/profile?code=XXXXXX</font>`<font style="color:rgb(31, 35, 40);">，code 的值是使用一次后即无效，且 10 分钟内过期</font>
3. <font style="color:rgb(31, 35, 40);">立马获取这个 code 值再去请求一个 api</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">/access_token/authenticate</font>`<font style="color:rgb(31, 35, 40);">，携带参数</font>`<font style="color:rgb(31, 35, 40);">{ verify_code: code }</font>`<font style="color:rgb(31, 35, 40);">，并且该 api 已经自带</font>`<font style="color:rgb(31, 35, 40);">app_id</font>`<font style="color:rgb(31, 35, 40);">和</font>`<font style="color:rgb(31, 35, 40);">app_secret</font>`<font style="color:rgb(31, 35, 40);">两个固定值参数，通过它去请求授权的 api，请求成功后得到返回值</font>`<font style="color:rgb(31, 35, 40);">{ access_token: "xxxxxxx", refresh_token: "xxxxxxxx", expires_in: xxxxxxxx }</font>`<font style="color:rgb(31, 35, 40);">，存下</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">和</font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);">到 cookie 中（localStorage 也可以），此时用户就算登录成功了。</font>
4. `<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">为标准 JWT 格式，是授权令牌，可以理解就是验证用户身份的，是应用在调用 api 访问和修改用户数据必须传入的参数（放在请求头 headers 里），2 小时后过期。也就是说，做完前三步后，你可以调用需要用户登录才能使用的 api；但是假如你什么都不操作，静静过去两个小时后，再去请求这些 api，就会报</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">过期，调用失败。</font>
5. <font style="color:rgb(31, 35, 40);">那么总不能 2 小时后就让用户退出登录吧，解决方法就是两小时后拿着过期的</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">和</font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);">（</font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);">过期时间一般长一些，比如一个月或更长）去请求</font>`<font style="color:rgb(31, 35, 40);">/refresh</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">api，返回结果为</font>`<font style="color:rgb(31, 35, 40);">{ access_token: "xxxxx", expires_in: xxxxx }</font>`<font style="color:rgb(31, 35, 40);">，换取新的</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">，新的</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">过期时间也是 2 小时，并重新存到 cookie，循环往复继续保持登录调用用户 api 了。</font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);">在限定过期时间内（比如一周或一个月等），下次就可以继续换取新的</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">，但过了限定时间，就算真正意义过期了，也就要重新输入账号密码来登录了。</font>

<font style="color:rgb(31, 35, 40);">公司网站登录过期时间都只有两小时（token 过期时间），但又想让一个月内经常活跃的用户不再次登录，于是才有这样需求，避免了用户再次输入账号密码登录。</font>

<font style="color:rgb(31, 35, 40);">为什么要专门用一个</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">去更新</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">呢？首先</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">会关联一定的用户权限，如果用户授权更改了，这个</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">也是需要被刷新以关联新的权限的，如果没有</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);">，也可以刷新</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">，但每次刷新都要用户输入登录用户名与密码，多麻烦。有了</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">refresh_ token</font>`<font style="color:rgb(31, 35, 40);">，可以减少这个麻烦，客户端直接用</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);"> </font><font style="color:rgb(31, 35, 40);">去更新</font><font style="color:rgb(31, 35, 40);"> </font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">，无需用户进行额外的操作。</font>

<font style="color:rgb(31, 35, 40);">说了这么多，或许有人会吐槽，一个登录用</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">就行了还要加个</font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);">搞得这么麻烦，或者有的公司</font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);">是后台包办的并不需要前端处理。但是，前置场景在那了，需求都是基于该场景下的。</font>

+ <font style="color:rgb(31, 35, 40);">需求</font>
1. <font style="color:rgb(31, 35, 40);">当</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">过期的时候，要用</font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);">去请求获取新的</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">，前端需要做到用户无感知的刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">。比如用户发起一个请求时，如果判断</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">已经过期，那么就先要去调用刷新 token 接口拿到新的</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">，再重新发起用户请求。</font>
2. <font style="color:rgb(31, 35, 40);">如果同时发起多个用户请求，第一个用户请求去调用刷新 token 接口，当接口还没返回时，其余的用户请求也依旧发起了刷新 token 接口请求，就会导致多个请求，这些请求如何处理，就是我们本文的内容了。</font>

## <font style="color:rgb(31, 35, 40);">思路</font>
### <font style="color:rgb(31, 35, 40);">方案一</font>
<font style="color:rgb(31, 35, 40);">写在请求拦截器里，在请求前，先利用最初请求返回的字段</font>`<font style="color:rgb(31, 35, 40);">expires_in</font>`<font style="color:rgb(31, 35, 40);">字段来判断</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">是否已经过期，若已过期，则将请求挂起，先刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">后再继续请求。</font>

+ <font style="color:rgb(31, 35, 40);">优点： 能节省 http 请求</font>
+ <font style="color:rgb(31, 35, 40);">缺点： 因为使用了本地时间判断，若本地时间被篡改，有校验失败的风险</font>

### <font style="color:rgb(31, 35, 40);">方案二</font>
<font style="color:rgb(31, 35, 40);">写在响应拦截器里，拦截返回后的数据。先发起用户请求，如果接口返回</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">过期，先刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">，再进行一次重试。</font>

+ <font style="color:rgb(31, 35, 40);">优点：无需判断时间</font>
+ <font style="color:rgb(31, 35, 40);">缺点： 会消耗多一次 http 请求</font>

<font style="color:rgb(31, 35, 40);">在此我选择的是方案二。</font>

## <font style="color:rgb(31, 35, 40);">实现</font>
<font style="color:rgb(31, 35, 40);">这里使用 axios，其中做的是请求后拦截，所以用到的是 axios 的响应拦截器</font>`<font style="color:rgb(31, 35, 40);">axios.interceptors.response.use()</font>`<font style="color:rgb(31, 35, 40);">方法</font>

### <font style="color:rgb(31, 35, 40);">方法介绍</font>
+ <font style="color:rgb(31, 35, 40);">@utils/auth.js</font>

```javascript
import Cookies from 'js-cookie'

const TOKEN_KEY = 'access_token'
const REGRESH_TOKEN_KEY = 'refresh_token'

export const getToken = () => Cookies.get(TOKEN_KEY)

export const setToken = (token, params = {}) => {
  Cookies.set(TOKEN_KEY, token, params)
}

export const setRefreshToken = token => {
  Cookies.set(REGRESH_TOKEN_KEY, token)
}
```

+ <font style="color:rgb(31, 35, 40);">request.js</font>

```javascript
import axios from 'axios'
import { getToken, setToken, getRefreshToken } from '@utils/auth'

// 刷新 access_token 的接口
const refreshToken = () => {
  return instance.post('/auth/refresh', { refresh_token: getRefreshToken() }, true)
}

// 创建 axios 实例
const instance = axios.create({
  baseURL: process.env.GATSBY_API_URL,
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
  },
})

instance.interceptors.response.use(
  response => {
    return response
  },
  error => {
    if (!error.response) {
      return Promise.reject(error)
    }
    // token 过期或无效，返回 401 状态码，在此处理逻辑
    return Promise.reject(error)
  }
)

// 给请求头添加 access_token
const setHeaderToken = isNeedToken => {
  const accessToken = isNeedToken ? getToken() : null
  if (isNeedToken) {
    // api 请求需要携带 access_token
    if (!accessToken) {
      console.log('不存在 access_token 则跳转回登录页')
    }
    instance.defaults.headers.common.Authorization = `Bearer ${accessToken}`
  }
}

// 有些 api 并不需要用户授权使用，则不携带 access_token；默认不携带，需要传则设置第三个参数为 true
export const get = (url, params = {}, isNeedToken = false) => {
  setHeaderToken(isNeedToken)
  return instance({
    method: 'get',
    url,
    params,
  })
}

export const post = (url, params = {}, isNeedToken = false) => {
  setHeaderToken(isNeedToken)
  return instance({
    method: 'post',
    url,
    data: params,
  })
}
```

<font style="color:rgb(31, 35, 40);">接下来改造 request.js 中 axios 的响应拦截器</font>

```javascript
instance.interceptors.response.use(
  response => {
    return response
  },
  error => {
    if (!error.response) {
      return Promise.reject(error)
    }
    if (error.response.status === 401) {
      const { config } = error
      return refreshToken()
        .then(res => {
          const { access_token } = res.data
          setToken(access_token)
          config.headers.Authorization = `Bearer ${access_token}`
          return instance(config)
        })
        .catch(err => {
          console.log('抱歉，您的登录状态已失效，请重新登录！')
          return Promise.reject(err)
        })
    }
    return Promise.reject(error)
  }
)
```

<font style="color:rgb(31, 35, 40);">约定返回 401 状态码表示</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">过期或者无效，如果用户发起一个请求后返回结果是</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">过期，则请求刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">的接口。请求成功则进入</font>`<font style="color:rgb(31, 35, 40);">then</font>`<font style="color:rgb(31, 35, 40);">里面，重置配置，并刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">并重新发起原来的请求。</font>

<font style="color:rgb(31, 35, 40);">但如果</font>`<font style="color:rgb(31, 35, 40);">refresh_token</font>`<font style="color:rgb(31, 35, 40);">也过期了，则请求也是返回 401。此时调试会发现函数进不到</font>`<font style="color:rgb(31, 35, 40);">refreshToken()</font>`<font style="color:rgb(31, 35, 40);">的</font>`<font style="color:rgb(31, 35, 40);">catch</font>`<font style="color:rgb(31, 35, 40);">里面，那是因为</font>`<font style="color:rgb(31, 35, 40);">refreshToken()</font>`<font style="color:rgb(31, 35, 40);">方法内部是也是用了同个</font>`<font style="color:rgb(31, 35, 40);">instance</font>`<font style="color:rgb(31, 35, 40);">实例，重复响应拦截器 401 的处理逻辑，但该函数本身就是刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">，故需要把该接口排除掉，即：</font>

```javascript
if (error.response.status === 401 && !error.config.url.includes('/auth/refresh')) {
}
```

<font style="color:rgb(31, 35, 40);">上述代码就已经实现了无感刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">了，当</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">没过期，正常返回；过期时，则 axios 内部进行了一次刷新 token 的操作，再重新发起原来的请求。</font>

## <font style="color:rgb(31, 35, 40);">优化</font>
### <font style="color:rgb(31, 35, 40);">防止多次刷新 token</font>
<font style="color:rgb(31, 35, 40);">如果 token 是过期的，那请求刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">的接口返回也是有一定时间间隔，如果此时还有其他请求发过来，就会再执行一次刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">的接口，就会导致多次刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">。因此，我们需要做一个判断，定义一个标记判断当前是否处于刷新</font>`<font style="color:rgb(31, 35, 40);">access_token</font>`<font style="color:rgb(31, 35, 40);">的状态，如果处在刷新状态则不再允许其他请求调用该接口。</font>

```javascript
let isRefreshing = false // 标记是否正在刷新 token
instance.interceptors.response.use(
  response => {
    return response
  },
  error => {
    if (!error.response) {
      return Promise.reject(error)
    }
    if (error.response.status === 401 && !error.config.url.includes('/auth/refresh')) {
      const { config } = error
      if (!isRefreshing) {
        isRefreshing = true
        return refreshToken()
          .then(res => {
            const { access_token } = res.data
            setToken(access_token)
            config.headers.Authorization = `Bearer ${access_token}`
            return instance(config)
          })
          .catch(err => {
            console.log('抱歉，您的登录状态已失效，请重新登录！')
            return Promise.reject(err)
          })
          .finally(() => {
            isRefreshing = false
          })
      }
    }
    return Promise.reject(error)
  }
)
```

### <font style="color:rgb(31, 35, 40);">同时发起多个请求的处理</font>
<font style="color:rgb(31, 35, 40);">上面做法还不够，因为如果同时发起多个请求，在 token 过期的情况，第一个请求进入刷新 token 方法，则其他请求进去没有做任何逻辑处理，单纯返回失败，最终只执行了第一个请求，这显然不合理。</font>

<font style="color:rgb(31, 35, 40);">比如同时发起三个请求，第一个请求进入刷新 token 的流程，第二个和第三个请求需要存起来，等到 token 更新后再重新发起请求。</font>

<font style="color:rgb(31, 35, 40);">在此，我们定义一个数组</font>`<font style="color:rgb(31, 35, 40);">requests</font>`<font style="color:rgb(31, 35, 40);">，用来保存处于等待的请求，之后返回一个</font>`<font style="color:rgb(31, 35, 40);">Promise</font>`<font style="color:rgb(31, 35, 40);">，只要不调用</font>`<font style="color:rgb(31, 35, 40);">resolve</font>`<font style="color:rgb(31, 35, 40);">方法，该请求就会处于等待状态，则可以知道其实数组存的是函数；等到 token 更新完毕，则通过数组循环执行函数，即逐个执行 resolve 重发请求。</font>

```javascript
let isRefreshing = false // 标记是否正在刷新 token
let requests = [] // 存储待重发请求的数组

instance.interceptors.response.use(
  response => {
    return response
  },
  error => {
    if (!error.response) {
      return Promise.reject(error)
    }
    if (error.response.status === 401 && !error.config.url.includes('/auth/refresh')) {
      const { config } = error
      if (!isRefreshing) {
        isRefreshing = true
        return refreshToken()
          .then(res => {
            const { access_token } = res.data
            setToken(access_token)
            config.headers.Authorization = `Bearer ${access_token}`
            // token 刷新后将数组的方法重新执行
            requests.forEach(cb => cb(access_token))
            requests = [] // 重新请求完清空
            return instance(config)
          })
          .catch(err => {
            console.log('抱歉，您的登录状态已失效，请重新登录！')
            return Promise.reject(err)
          })
          .finally(() => {
            isRefreshing = false
          })
      } else {
        // 返回未执行 resolve 的 Promise
        return new Promise(resolve => {
          // 用函数形式将 resolve 存入，等待刷新后再执行
          requests.push(token => {
            config.headers.Authorization = `Bearer ${token}`
            resolve(instance(config))
          })
        })
      }
    }
    return Promise.reject(error)
  }
)
```

<font style="color:rgb(31, 35, 40);">最终 request.js 代码</font>

```javascript
import axios from 'axios'
import { getToken, setToken, getRefreshToken } from '@utils/auth'

// 刷新 access_token 的接口
const refreshToken = () => {
  return instance.post('/auth/refresh', { refresh_token: getRefreshToken() }, true)
}

// 创建 axios 实例
const instance = axios.create({
  baseURL: process.env.GATSBY_API_URL,
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
  },
})

let isRefreshing = false // 标记是否正在刷新 token
let requests = [] // 存储待重发请求的数组

instance.interceptors.response.use(
  response => {
    return response
  },
  error => {
    if (!error.response) {
      return Promise.reject(error)
    }
    if (error.response.status === 401 && !error.config.url.includes('/auth/refresh')) {
      const { config } = error
      if (!isRefreshing) {
        isRefreshing = true
        return refreshToken()
          .then(res => {
            const { access_token } = res.data
            setToken(access_token)
            config.headers.Authorization = `Bearer ${access_token}`
            // token 刷新后将数组的方法重新执行
            requests.forEach(cb => cb(access_token))
            requests = [] // 重新请求完清空
            return instance(config)
          })
          .catch(err => {
            console.log('抱歉，您的登录状态已失效，请重新登录！')
            return Promise.reject(err)
          })
          .finally(() => {
            isRefreshing = false
          })
      } else {
        // 返回未执行 resolve 的 Promise
        return new Promise(resolve => {
          // 用函数形式将 resolve 存入，等待刷新后再执行
          requests.push(token => {
            config.headers.Authorization = `Bearer ${token}`
            resolve(instance(config))
          })
        })
      }
    }
    return Promise.reject(error)
  }
)

// 给请求头添加 access_token
const setHeaderToken = isNeedToken => {
  const accessToken = isNeedToken ? getToken() : null
  if (isNeedToken) {
    // api 请求需要携带 access_token
    if (!accessToken) {
      console.log('不存在 access_token 则跳转回登录页')
    }
    instance.defaults.headers.common.Authorization = `Bearer ${accessToken}`
  }
}

// 有些 api 并不需要用户授权使用，则无需携带 access_token；默认不携带，需要传则设置第三个参数为 true
export const get = (url, params = {}, isNeedToken = false) => {
  setHeaderToken(isNeedToken)
  return instance({
    method: 'get',
    url,
    params,
  })
}

export const post = (url, params = {}, isNeedToken = false) => {
  setHeaderToken(isNeedToken)
  return instance({
    method: 'post',
    url,
    data: params,
  })
}
```

