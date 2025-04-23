## 开发前准备
<font style="color:rgb(51, 51, 51);">利用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-app</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">开发，有两种方法：</font>

1. <font style="color:rgb(51, 51, 51);">通过</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">HBuilderX</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">创建（需安装</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">HBuilderX</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">编辑器）</font>
2. <font style="color:rgb(51, 51, 51);">通过命令行创建（需安装</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">NodeJS</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">环境），推荐使用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">vscode</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">编辑器</font>

<font style="color:rgb(51, 51, 51);">这里我们使用第2种方法，这两种方法官方都有详细介绍</font><font style="color:rgb(51, 51, 51);"> </font>[<font style="color:rgb(51, 51, 51);">点击查看官方文档</font>](https://uniapp.dcloud.net.cn/quickstart-hx.html)

### `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">vscode</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">安装插件</font>
1. <font style="color:rgb(51, 51, 51);">安装 Vue3 插件，</font>[<font style="color:rgb(51, 51, 51);">点击查看官方文档</font>](https://cn.vuejs.org/guide/typescript/overview.html#ide-support)
+ <font style="color:rgb(51, 51, 51);">安装</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">Vue Language Features (Volar)</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">：Vue3 语法提示插件</font>
+ <font style="color:rgb(51, 51, 51);">安装</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">TypeScript Vue Plugin (Volar)</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">：Vue3+TS 插件</font>
+ **<font style="color:rgb(51, 51, 51);">工作区禁用</font>**<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">Vue2 的 Vetur 插件(Vue3 插件和 Vue2 冲突)</font>
+ **<font style="color:rgb(51, 51, 51);">工作区禁用</font>**<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">@builtin typescript 插件（禁用后开启 Vue3 的 TS 托管模式）</font>
1. <font style="color:rgb(51, 51, 51);">安装 uni-app 开发插件</font>
+ `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-create-view</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">：快速创建 uni-app 页面</font>
+ `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-helper</font>`<font style="color:rgb(51, 51, 51);">（插件套装，安装一个后会有多个插件） ：代码提示</font>
+ `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uniapp小程序扩展</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">：鼠标悬停查文档</font>

### `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-create-view</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">插件使用</font>
`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-create-view</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">安装后，需要修改配置，进入</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">文件</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-></font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">首选项</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">-></font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">设置</font>`<font style="color:rgb(51, 51, 51);">，按以下选项修改即可  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730949495940-ec897efb-e970-4eae-878f-864f92c69060.png)

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-create-view</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">使用方法：  
</font><font style="color:rgb(51, 51, 51);">在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">src/pages</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">下右键，选择</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">新建uni-app页面</font>`<font style="color:rgb(51, 51, 51);">，弹出输入框，输入</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">文件夹名称 页面名称</font>`<font style="color:rgb(51, 51, 51);">，然后回车  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730949495916-229a68e0-4ddf-4363-a882-679915335930.png)

<font style="color:rgb(51, 51, 51);">生成如下目录文件  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730949495979-da4eb782-2d87-4a8b-a448-205049418839.png)

<font style="color:rgb(51, 51, 51);">并且在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">src/pages.json</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">目录下，已将新界面配置进去  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730949495949-6071ea98-05e2-4579-9ca6-6fd111564ba7.png)

### `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">vscode</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">项目配置</font>
**<font style="color:rgb(51, 51, 51);">项目生成后，在项目的根目录进行</font>**<font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(51, 51, 51);">新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.vscode</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件夹，并创建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">settings.json</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件：</font>

```json
{
  // 在保存时格式化文件
  "editor.formatOnSave": true,
  // 文件格式化配置
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  // 配置语言的文件关联
  "files.associations": {
    "pages.json": "jsonc", // pages.json 可以写注释
    "manifest.json": "jsonc" // manifest.json 可以写注释
  }
}
```

<font style="color:rgb(51, 51, 51);">同样，在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.vscode</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件夹内，创建 vue3 模板文件，命名为</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">vue3-uniapp.code-snippets</font>`<font style="color:rgb(51, 51, 51);">：</font>

```json
{
  "vue3+uniapp快速生成模板": {
    "scope": "vue",
    "prefix": "Vue3",
    "body": [
      "<script setup lang=\"ts\">",
      "$2",
      "</script>\n",
      "<template>",
      "\t<view class=\"\">test</view>",
      "</template>\n",
      "<style lang=\"scss\"></style>",
      "$2"
    ],
    "description": "vue3+uniapp快速生成模板"
  }
}
```

<font style="color:rgb(51, 51, 51);">然后，在空白vue文件中，输入vue3，选择此模板，即可快速生成代码  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730949495936-59ae94d8-76e1-42a6-ae54-cb2dfe0d86f5.png)

## 项目初始化
### <font style="color:rgb(51, 51, 51);">项目创建</font>
<font style="color:rgb(51, 51, 51);">拉取 </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-app</font>`<font style="color:rgb(51, 51, 51);"> 官方项目基础架构代码 </font>[<font style="color:#117CEE;">https://uniapp.dcloud.net.cn/quickstart-cli.html</font>](https://uniapp.dcloud.net.cn/quickstart-cli.html)

```bash
npx degit dcloudio/uni-preset-vue#vite-ts uni-vue3-ts-shop
 
cd uni-vue3-ts-shop
```

<font style="color:rgb(51, 51, 51);">或者直接直接克隆国内 gitee 地址，然后修改项目名称，并进入项目根目标</font>

```bash
git clone -b vite-ts https://gitee.com/dcloud/uni-preset-vue.git
```

### <font style="color:rgb(51, 51, 51);">安装ts扩展</font>
<font style="color:rgb(51, 51, 51);">主要是为了增加</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-app</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">、</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">微信小程序</font>`<font style="color:rgb(51, 51, 51);">、</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">nodejs</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">对ts的支持</font>

```bash
npm i -D @uni-helper/uni-app-types miniprogram-api-typings @types/node
```

<font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">tsconfig.json</font>`

```json
{
  "compilerOptions": {
    "ignoreDeprecations": "5.0",
    "allowJs": true,
    },
    "types": ["@dcloudio/types", "miniprogram-api-typings", "@uni-helper/uni-app-types"]
  },
  "vueCompilerOptions": {
    // experimentalRuntimeMode 已废弃，现调整为 nativeTags，请升级 Volar 插件至最新版本
    "nativeTags": ["block", "component", "template", "slot"]
  }
}
```

## 配置环境变量
[<font style="color:rgb(51, 51, 51);">点我查看官方文档</font>](https://uniapp.dcloud.net.cn/tutorial/migration-to-vue3.html#%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)

### <font style="color:rgb(51, 51, 51);">新增env文件</font>
<font style="color:rgb(51, 51, 51);">根目录下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.env</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件</font>

```javascript
VITE_HTTP = https://mock.mengxuegu.com/mock/6598258423a3c638568501db/uniapp_template
```

### <font style="color:rgb(51, 51, 51);">使用</font>
<font style="color:rgb(51, 51, 51);">获取环境变量</font>

```plain
process.env.NODE_ENV          // 应用运行的模式，比如vite.config.ts里
import.meta.env.VITE_HTTP     // src下的vue文件或其他ts文件里
```

### <font style="color:rgb(51, 51, 51);">开启</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">sourcemap</font>`
[<font style="color:rgb(51, 51, 51);">点我查看官方文档</font>](https://uniapp.dcloud.net.cn/tutorial/migration-to-vue3.html#%E9%9C%80%E4%B8%BB%E5%8A%A8%E5%BC%80%E5%90%AF-sourcemap)<font style="color:rgb(51, 51, 51);">  
</font><font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">vite.config.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件：</font>

```javascript
export default defineConfig({
  build: {
    // 开发阶段启用源码映射：https://uniapp.dcloud.net.cn/tutorial/migration-to-vue3.html#需主动开启-sourcemap
    sourcemap: process.env.NODE_ENV === 'development',
  },
  plugins: [uni()],
})
```

## 统一代码规范
### <font style="color:rgb(51, 51, 51);">安装</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">prettier</font>`
```shell
pnpm i -D prettier
```

<font style="color:rgb(51, 51, 51);">根目录下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.prettierrc.json</font>`

```json
{
  "singleQuote": true,
  "semi": false,
  "printWidth": 120,
  "trailingComma": "all",
  "endOfLine": "auto"
}
```

### <font style="color:rgb(51, 51, 51);">安装</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">eslint</font>`
```shell
pnpm i -D eslint eslint-plugin-vue @rushstack/eslint-patch @vue/eslint-config-typescript  @vue/eslint-config-prettier
```

<font style="color:rgb(51, 51, 51);">根目录下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.eslintrc.js</font>`

```javascript
/* eslint-env node */
require('@rushstack/eslint-patch/modern-module-resolution')

module.exports = {
  root: true,
  extends: [
    'plugin:vue/vue3-essential',
    'eslint:recommended',
    '@vue/eslint-config-typescript',
    '@vue/eslint-config-prettier',
  ],
  // 小程序全局变量
  globals: {
    uni: true,
    wx: true,
    WechatMiniprogram: true,
    getCurrentPages: true,
    getApp: true,
    UniApp: true,
    UniHelper: true,
    App: true,
    Page: true,
    Component: true,
    AnyObject: true,
  },
  parserOptions: {
    ecmaVersion: 'latest',
  },
  rules: {
    'prettier/prettier': [
      'warn',
      {
        singleQuote: true,
        semi: false,
        printWidth: 120,
        trailingComma: 'all',
        endOfLine: 'auto',
      },
    ],
    'vue/multi-word-component-names': ['off'],
    'vue/no-setup-props-destructure': ['off'],
    'vue/no-deprecated-html-element-is': ['off'],
    '@typescript-eslint/no-unused-vars': ['off'],
  },
}
```

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">package.json</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">中新增命令</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">lint</font>`

```json
{
  "scripts": {
    "lint": "eslint . --ext .vue,.js,.jsx,.cjs,.mjs,.ts,.tsx,.cts,.mts --fix --ignore-path .gitignore"
  }
}
```

<font style="color:rgb(51, 51, 51);">然后运行</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">npm run lint</font>`<font style="color:rgb(51, 51, 51);">，将项目内的文件格式化为</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">eslint</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">规定的格式（这个命令可随时运行，以便有新页面/插件加入时，解决代码风格的问题）</font>

## 规范git提交
`**<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">非必需，适合多人开发</font>**`

### <font style="color:rgb(51, 51, 51);">安装</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">husky</font>`
<font style="color:rgb(51, 51, 51);">安装并初始化</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">husky</font>`

```bash
npx husky-init
```

<font style="color:rgb(51, 51, 51);">如果是首次安装，会有以下提示，输入</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">y</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">回车即可</font>

```bash
Need to install the following packages:
husky-init@8.0.0
Ok to proceed? (y)
```

<font style="color:rgb(51, 51, 51);">安装完成后，会多出</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.husky</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件夹和</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">pre-commit</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件</font>

### <font style="color:rgb(51, 51, 51);">安装</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">lint-staged</font>`
```bash
pnpm i -D lint-staged
```

<font style="color:rgb(51, 51, 51);">安装完成后配置</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">package.json</font>`

```json
{
  "script": {
    // ... 省略 ...
    "lint-staged": "lint-staged"
  },
  "lint-staged": {
    "*.{vue,ts,js}": ["eslint --fix"]
  }
}
```

<font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">pre-commit</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件</font>

```diff
- pnpm test
+ pnpm run lint-staged
```

### <font style="color:rgb(51, 51, 51);">提交规范</font>
<font style="color:rgb(51, 51, 51);">至此，已完成</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">husky</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">+</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">lint-staged</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">的配置。之后，每次提交代码，在提交信息前都要加入以下提交类型之一，譬如：</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">feat: 首页新增轮播图</font>`

| <font style="color:rgb(51, 51, 51);">提交字段</font> | <font style="color:rgb(51, 51, 51);">说明</font> |
| --- | --- |
| <font style="color:rgb(51, 51, 51);">feat:</font> | <font style="color:rgb(51, 51, 51);">增加新功能</font> |
| <font style="color:rgb(51, 51, 51);">fix:</font> | <font style="color:rgb(51, 51, 51);">修复问题/BUG</font> |
| <font style="color:rgb(51, 51, 51);">style:</font> | <font style="color:rgb(51, 51, 51);">代码风格相关无影响运行结果的</font> |
| <font style="color:rgb(51, 51, 51);">perf:</font> | <font style="color:rgb(51, 51, 51);">优化/性能提升</font> |
| <font style="color:rgb(51, 51, 51);">refactor:</font> | <font style="color:rgb(51, 51, 51);">重构</font> |
| <font style="color:rgb(51, 51, 51);">revert:</font> | <font style="color:rgb(51, 51, 51);">撤销修改</font> |
| <font style="color:rgb(51, 51, 51);">test:</font> | <font style="color:rgb(51, 51, 51);">测试相关</font> |
| <font style="color:rgb(51, 51, 51);">docs:</font> | <font style="color:rgb(51, 51, 51);">文档/注释</font> |
| <font style="color:rgb(51, 51, 51);">chore:</font> | <font style="color:rgb(51, 51, 51);">依赖更新/脚手架配置修改等</font> |
| <font style="color:rgb(51, 51, 51);">workflow:</font> | <font style="color:rgb(51, 51, 51);">工作流改进</font> |
| <font style="color:rgb(51, 51, 51);">ci:</font> | <font style="color:rgb(51, 51, 51);">持续集成</font> |
| <font style="color:rgb(51, 51, 51);">types:</font> | <font style="color:rgb(51, 51, 51);">类型定义文件更改</font> |
| <font style="color:rgb(51, 51, 51);">wip:</font> | <font style="color:rgb(51, 51, 51);">开发中</font> |


## 安装 `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-ui</font>` 组件库
`**<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">非必需，也可使用其他组件库</font>**`

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-ui</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">是DCloud提供的一个跨端ui库，它是基于vue组件的、flex布局的、无dom的跨全端ui框架。</font>[<font style="color:rgb(51, 51, 51);">查看官方文档</font>](https://uniapp.dcloud.net.cn/component/uniui/quickstart.html)

### <font style="color:rgb(51, 51, 51);">安装</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-ui</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">及相关插件</font>
`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">sass sass-loader</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">是</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-ui</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">的依赖库，</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">@uni-helper/uni-ui-types</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">是类型声明文件</font>

```bash
pnpm i  -D sass sass-loader
pnpm i @dcloudio/uni-ui
pnpm i -D @uni-helper/uni-ui-types
```

### <font style="color:rgb(51, 51, 51);">修改配置</font>
<font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">tsconfig.json</font>`<font style="color:rgb(51, 51, 51);">，配置类型声明文件</font>

```json
{
  "compilerOptions": {
    "types": ["@dcloudio/types", "miniprogram-api-typings", "@uni-helper/uni-app-types", "@uni-helper/uni-ui-types"]
  }
}
```

<font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">src/pages.json</font>`<font style="color:rgb(51, 51, 51);">，配置自动导入组件</font>

```json
{
  "easycom": {
    "autoscan": true,
    "custom": {
      // uni-ui 规则如下配置  
      "^uni-(.*)": "@dcloudio/uni-ui/lib/uni-$1/uni-$1.vue" 
    }
  },
  "pages": [
    // ……
  ]
}
```

## 安装配置 `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">pina</font>`
### <font style="color:rgb(51, 51, 51);">安装</font>
`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">pinia-plugin-persistedstate</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">是持久化</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">pina</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">插件</font>

```bash
pnpm i pinia pinia-plugin-persistedstate
```

### <font style="color:rgb(51, 51, 51);">使用</font>
<font style="color:rgb(51, 51, 51);">在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">src</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">下新增以下目录和文件</font>

```markdown
src
├─stores
│  ├─modules
│  │ └─user.ts
|  └─index.ts
```

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">user.ts</font>`

```javascript
import { defineStore } from 'pinia'
import { ref } from 'vue'

// 定义 Store
export const useMemberStore = defineStore(
  'user',
  () => {
    // 用户信息
    const userInfo = ref<any>()

      // 保存用户信息，登录时使用
      const setUserInfo = (val: any) => {
      userInfo.value = val
    }

    // 清理用户信息，退出时使用
    const clearUserInfo = () => {
      userInfo.value = undefined
    }

    return {
      userInfo,
      setUserInfo,
      clearUserInfo,
    }
  },
  // TODO: 持久化
  {
    persist: true,
  },
)
```

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">index.ts</font>`

```javascript
import { createPinia } from 'pinia'
import persist from 'pinia-plugin-persistedstate'

// 创建 pinia 实例
const pinia = createPinia()
// 使用持久化存储插件
pinia.use(persist)

// 默认导出，给 main.ts 使用
export default pinia

// 模块统一导出
export * from './modules/user'
```

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">main.ts</font>`

```javascript
import { createSSRApp } from 'vue'
import pinia from './stores'

import App from './App.vue'
export function createApp() {
  const app = createSSRApp(App)

  app.use(pinia)
  return {
    app,
  }
}
```

### <font style="color:rgb(51, 51, 51);">持久化</font>
<font style="color:rgb(51, 51, 51);">插件默认使用</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">localStorage</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">实现持久化，小程序端不兼容，需要替换持久化 API。  
</font><font style="color:rgb(51, 51, 51);">修改</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">stores/modules/user.ts</font>`

```javascript
export const useUserStore = defineStore(
  'member',
  () => {
    //…省略
  },
  {
    // 配置持久化
    persist: {
      // 调整为兼容多端的API
      storage: {
        setItem(key, value) {
          uni.setStorageSync(key, value) 
        },
        getItem(key) {
          return uni.getStorageSync(key) 
        },
      },
    },
  },
)
```

## 封装请求
### `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uniapp</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">拦截器</font>
`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni.addInterceptor</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">的使用参考</font><font style="color:rgb(51, 51, 51);"> </font>[<font style="color:rgb(51, 51, 51);">官方文档</font>](https://uniapp.dcloud.net.cn/api/interceptor.html)

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">src</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">目录下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">utils</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件夹，并新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">http.ts</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件</font>

```javascript
import { useUserStore } from '@/stores'

const baseURL = import.meta.env.VITE_HTTP

// 拦截器配置
const httpInterceptor = {
  // 拦截前触发
  invoke(options: UniApp.RequestOptions) {
    // 非 http 开头需拼接地址
    if (!options.url.startsWith('http')) {
      options.url = baseURL + options.url
    }
    options.timeout = 10000
    // 添加请求头标识
    options.header = {
      'request-client': 'wechart-app',
      ...options.header,
    }
    // 添加 token 请求头标识
    const memberStore = useUserStore()
    const token = memberStore.userInfo?.token
    if (token) {
      options.header.Authorization = token
    }
  },
}

// 拦截 request 请求
uni.addInterceptor('request', httpInterceptor)
// 拦截 uploadFile 文件上传
uni.addInterceptor('uploadFile', httpInterceptor)
```

<font style="color:rgb(51, 51, 51);">由于</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-app</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">的响应拦截器对类型支持并不友好，所以我们自行封装响应拦截器，同一个文件，继续</font>

```javascript
/**
 * 请求函数
 * @param  UniApp.RequestOptions
 * @returns Promise
 */
// Data类型根据后台返回数据去定义
type Data<T> = {
  code: string
    msg: string
result: T
}
export const http = <T>(options: UniApp.RequestOptions) => {
  return new Promise<Data<T>>((resolve, reject) => {
    uni.request({
      ...options,
      // 响应成功
      success(res) {
        if (res.statusCode >= 200 && res.statusCode < 300) {
          resolve(res.data as Data<T>)
        } else if (res.statusCode === 401) {
          // 401错误  -> 清理用户信息，跳转到登录页
          const userStore = useUserStore()
          userStore.clearUserInfo()
          uni.navigateTo({ url: '/pages/login' })
          reject(res)
        } else {
          // 其他错误 -> 根据后端错误信息轻提示
          uni.showToast({
            icon: 'none',
            title: (res.data as Data<T>).msg || '请求错误',
          })
          reject(res)
        }
      },
      // 响应失败
      fail(err) {
        uni.showToast({
          icon: 'none',
          title: '网络错误，换个网络试试',
        })
        reject(err)
      },
    })
  })
}
```

### <font style="color:rgb(51, 51, 51);">使用</font>
<font style="color:rgb(51, 51, 51);">为了统一API文件，我们在 src 目录下新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">api</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件夹，并新建</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">user.ts</font>`

```javascript
import { http } from '@/utils/http'

export const getUserInfoAPI = (data: any) => {
  return http({
    url: '/user/info',
    method: 'POST',
    data,
  })
}
```

<font style="color:rgb(51, 51, 51);">然后在需要的地方调用，比如在</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">page/my/index.vue</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">里调用：</font>

```html
<script setup lang="ts">
import { useUserStore } from '@/stores'
import { getUserInfoAPI } from '@/api/user'
 
const userStore = useUserStore()
 
const getUserInfo = async () => {
  const res = await getUserInfoAPI({ id: 'weizwz' })
  console.log(res)
  const { result } = res
  userStore.setUserInfo(result)
}
</script>
 
<template>
  <view class="">
    <view>用户信息： {{ userStore.userInfo }}</view>
    <button
      @tap="
        userStore.setUserInfo({
          userName: 'weizwz',
        })
      "
      size="mini"
      plain
      type="primary"
    >
      保存用户信息
    </button>
    <button @tap="userStore.clearUserInfo()" size="mini" plain type="primary">清理用户信息</button>
    <button @tap="getUserInfo()" size="mini" plain type="primary">发送请求</button>
  </view>
</template>
 
<style lang="scss"></style>
```

<font style="color:rgb(51, 51, 51);">效果如下，可以看到已经调用成功：  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730949496297-b2f05cd5-613b-4ffb-9a72-69c5ef4c4b0c.png)

**<font style="color:rgb(51, 51, 51);">如果调用被拦截的话，请检查微信小程序里的项目设置，然后选中</font>****<font style="color:rgb(51, 51, 51);"> </font>**`**<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">不检验合法域名、web-view(业务域名)、TLS版本以及HTTPS证书</font>**`**<font style="color:rgb(51, 51, 51);"> </font>****<font style="color:rgb(51, 51, 51);">选项</font>**

## 注意事项
### <font style="color:rgb(51, 51, 51);">开发区别</font>
`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni-app</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">项目每个页面是一个</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.vue</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">文件，数据绑定及事件处理同</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">Vue.js</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">规范：</font>

1. <font style="color:rgb(51, 51, 51);">vue文件中的</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">div</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">标签需替换为</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">view</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">标签</font>
2. <font style="color:rgb(51, 51, 51);">属性绑定</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">src="{ { url }}"</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">升级成</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">:src="url"</font>`
3. <font style="color:rgb(51, 51, 51);">事件绑定</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">bindtap="eventName"</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">升级成</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">@tap="eventName"</font>`<font style="color:rgb(51, 51, 51);">，</font>**<font style="color:rgb(51, 51, 51);">支持（）传参</font>**
4. <font style="color:rgb(51, 51, 51);">支持 Vue 常用</font>**<font style="color:rgb(51, 51, 51);">指令</font>**<font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">v-for</font>`<font style="color:rgb(51, 51, 51);">、</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">v-if</font>`<font style="color:rgb(51, 51, 51);">、</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">v-show</font>`<font style="color:rgb(51, 51, 51);">、</font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">v-model</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">等</font>

### <font style="color:rgb(51, 51, 51);">其他补充</font>
1. <font style="color:rgb(51, 51, 51);">调用接口能力，</font>**<font style="color:rgb(51, 51, 51);">建议前缀</font>**<font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">wx</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">替换为</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">uni</font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">，养成好习惯，</font>**<font style="color:rgb(51, 51, 51);">支持多端开发</font>**<font style="color:rgb(51, 51, 51);">。</font>
2. `<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);"><style></font>`<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">页面样式不需要写</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">scoped</font>`<font style="color:rgb(51, 51, 51);">，小程序是多页面应用，</font>**<font style="color:rgb(51, 51, 51);">页面样式自动隔离</font>**<font style="color:rgb(51, 51, 51);">。</font>
3. **<font style="color:rgb(51, 51, 51);">生命周期分三部分</font>**<font style="color:rgb(51, 51, 51);">：应用生命周期(小程序)，页面生命周期(小程序)，组件生命周期(Vue)</font>
4. <font style="color:rgb(51, 51, 51);">其他参考 </font>[<font style="color:#117CEE;">uniapp-vue语法 官方文档</font>](https://uniapp.dcloud.net.cn/tutorial/migration-to-vue3.html)

