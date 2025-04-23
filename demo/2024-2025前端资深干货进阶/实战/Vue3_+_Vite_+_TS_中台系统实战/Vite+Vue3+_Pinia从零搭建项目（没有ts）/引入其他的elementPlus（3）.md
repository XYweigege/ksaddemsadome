<font style="color:rgb(43, 43, 43);">首先接入UI库，这里是介绍的是按需自动导入，这里可以根据官网的</font>[按需自动导入](https://element-plus.org/zh-CN/guide/quickstart.html#%E8%87%AA%E5%8A%A8%E5%AF%BC%E5%85%A5-%E6%8E%A8%E8%8D%90)

<font style="color:rgb(43, 43, 43);">同时加上</font>[自定义主题](https://element-plus.org/zh-CN/guide/theming.html#%E6%9B%B4%E6%8D%A2%E4%B8%BB%E9%A2%98%E8%89%B2)<font style="color:rgb(43, 43, 43);">，这里不仅仅可改变主题色，也可以改变组件其他样式</font>

<font style="color:rgb(43, 43, 43);">综上，再加上代理，修改vite.config.js配置如下：</font>

```javascript
import { defineConfig } from 'vite'

import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'

// 引入path： 别名
import { resolve } from 'path'

export default defineConfig({
  plugins: [ // 插件引用（标准vite插件放这里引用）
    vue(), // vue插件
    vueJsx(), // jsx拓展插件
    AutoImport({ // 自动引入，不需要手动去写import
      // 这里可以不需要写import调用大部分vue/vue-router/pinia方法（记住是大部分）
      imports: ['vue', 'vue-router', 'pinia'],
      // element需要通过resolvers引用
      resolvers: [ElementPlusResolver()],
      // 会自动生成eslint规则，防止eslint报错，后续selint配置的时候会讲到
      eslintrc: {
        enabled: true
      }
    }),
    Components({ // 按需引入，避免没使用的组件也打包
      resolvers: [ElementPlusResolver({ importStyle: 'sass' })]
    })
  ],
  resolve: {
    // 别名配置
    alias: [
      {
        find: '@',
        replacement: resolve(__dirname, 'src')
      }
    ]
  },
  css: {
    preprocessorOptions: {
      scss: {
        additionalData: '@use "@/style/globalvar.scss" as *;'
      }
    }
  },
  server: {
    host: '0.0.0.0', // 服务监听地址，设置该值表示监听所有
    cors: true, // 允许跨源
    port: 9227, // 本地服务端口号
    proxy: { // 代理
      '^/api': { // api地址匹配的字符串，可以使用正则，此处表示以/api为开头的接口地址
        target: 'http://xxx.com/', // 指向，表示上述需要匹配的地址都指向这个域名，注意要用/结尾
        rewrite: (path) => path.replace(/^\/api/, ''), // 如果匹配字符不需要了，可以使用重写去掉
        changeOrigin: true // 是否修改请求头的origin，让服务器认为这个请求来自本域名
      }
    }
  }
})

```

<br/>color1
上面配置项中，npm run dev 根目录会自动生成一个.eslintrc-auto-import.json文件。里面包含eslint自动引入的代码，这个文件的路径需要添加到eslint配置文件中去。

<br/>

```javascript
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true
  },
  // 新增./.eslintrc-auto-import.json
  extends: ['standard', 'plugin:vue/vue3-essential', 'prettier', './.eslintrc-auto-import.json'],
  overrides: [
    {
      env: {
        node: true
      },
      files: ['.eslintrc.{js,cjs}'],
      parserOptions: {
        sourceType: 'script'
      }
    }
  ],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module'
  },
  plugins: ['vue', 'prettier'],
  rules: {
    'vue/multi-word-component-names': 'off',
    'vue/no-useless-template-attributes': 'off',
    'prettier/prettier': 'error'
  }
}

```

```json
{
  "globals": {
    "Component": true,
    "ComponentPublicInstance": true,
    "ComputedRef": true,
    "EffectScope": true,
    "ExtractDefaultPropTypes": true,
    "ExtractPropTypes": true,
    "ExtractPublicPropTypes": true,
    "InjectionKey": true,
    "PropType": true,
    "Ref": true,
    "VNode": true,
    "WritableComputedRef": true,
    "acceptHMRUpdate": true,
    "computed": true,
    "createApp": true,
    "createPinia": true,
    "customRef": true,
    "defineAsyncComponent": true,
    "defineComponent": true,
    "defineStore": true,
    "effectScope": true,
    "getActivePinia": true,
    "getCurrentInstance": true,
    "getCurrentScope": true,
    "h": true,
    "inject": true,
    "isProxy": true,
    "isReactive": true,
    "isReadonly": true,
    "isRef": true,
    "mapActions": true,
    "mapGetters": true,
    "mapState": true,
    "mapStores": true,
    "mapWritableState": true,
    "markRaw": true,
    "nextTick": true,
    "onActivated": true,
    "onBeforeMount": true,
    "onBeforeRouteLeave": true,
    "onBeforeRouteUpdate": true,
    "onBeforeUnmount": true,
    "onBeforeUpdate": true,
    "onDeactivated": true,
    "onErrorCaptured": true,
    "onMounted": true,
    "onRenderTracked": true,
    "onRenderTriggered": true,
    "onScopeDispose": true,
    "onServerPrefetch": true,
    "onUnmounted": true,
    "onUpdated": true,
    "provide": true,
    "reactive": true,
    "readonly": true,
    "ref": true,
    "resolveComponent": true,
    "setActivePinia": true,
    "setMapStoreSuffix": true,
    "shallowReactive": true,
    "shallowReadonly": true,
    "shallowRef": true,
    "storeToRefs": true,
    "toRaw": true,
    "toRef": true,
    "toRefs": true,
    "toValue": true,
    "triggerRef": true,
    "unref": true,
    "useAttrs": true,
    "useCssModule": true,
    "useCssVars": true,
    "useLink": true,
    "useRoute": true,
    "useRouter": true,
    "useSlots": true,
    "watch": true,
    "watchEffect": true,
    "watchPostEffect": true,
    "watchSyncEffect": true
  }
}

```

<font style="color:rgb(43, 43, 43);">这样就不用每次都去import了。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728437426218-54afb329-598b-4c35-9cb2-cd7c3623659e.png)

<font style="color:rgb(43, 43, 43);">生成之后一直有人问，自动引用elementPlus但是ElMessage还是会报错怎么办。</font>

<font style="color:rgb(43, 43, 43);"></font>

1. <font style="color:rgb(89, 89, 89);">在</font>`<font style="color:rgb(38, 198, 218);">.eslintrc-auto-import.json</font>`<font style="color:rgb(89, 89, 89);">手动添加</font>

```json

{
  "globals": {
     // ...
    "ElMessage": true,
    "ElMessageBox": true
  }
}

```

2. <font style="color:rgb(89, 89, 89);">先在代码区块内import，等到终端窗口出现如下提示，查看</font>`<font style="color:rgb(38, 198, 218);">.eslintrc-auto-import.json</font>`<font style="color:rgb(89, 89, 89);">会自动添加</font>`<font style="color:rgb(38, 198, 218);">ElMessageBox</font>`<font style="color:rgb(89, 89, 89);">和</font>`<font style="color:rgb(38, 198, 218);">ElMessage</font>`<font style="color:rgb(89, 89, 89);">的配置项。这时再删掉import语句即可。</font>



![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728437821641-279427a5-cdf9-4157-aba6-50f4e6b01cc7.png)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728437832281-7bd5f66e-d9d7-4e53-ba47-8b0441620766.png)

