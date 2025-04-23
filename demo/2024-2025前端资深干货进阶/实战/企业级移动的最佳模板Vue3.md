### 前言
[**代码地址：**](https://gitee.com/sohucw/vue3-h5-pro)**实战demo**



<br/>color1
<font style="color:rgba(0, 0, 0, 0.847);">做移动端其实很简单，做过管理系统，很容易，区别在于</font>

**<font style="color:rgba(0, 0, 0, 0.847);">1：设计稿给的比例： 750 还是375</font>**

**<font style="color:rgba(0, 0, 0, 0.847);">2：主流的最佳方式 flex + vw </font>**

**<font style="color:rgba(0, 0, 0, 0.847);">3:  ui给的设计稿是px的单位 ，用好</font>****<font style="color:rgba(0, 0, 0, 0.847);">postcss-px-to-viewport 这个插件就行了</font>**

**<font style="color:rgba(0, 0, 0, 0.847);">4：如何继承主流的移动端组件库 vant</font>**

**<font style="color:rgba(0, 0, 0, 0.847);">5：从面试观的角度：怎么做适配的？把 px rem vw 讲明白  顺带再讲一下 响应式布局 和 媒体查询（media）</font>**

**<font style="color:rgba(0, 0, 0, 0.847);">6：更深度一些，讲一下原生native 和h5 还有小程序架构的区别 </font>**

<br/>

<font style="color:rgba(0, 0, 0, 0.847);"> </font>

### <font style="color:rgba(0, 0, 0, 0.847);">项目目录共性的内容</font>
```bash

├── .husky
│   └── commit-msg           # commit 信息校验
|   └── pre-commit           # eslint 代码检验
├── src
│   ├── api                  # Api ajax 等
│   ├── assets               # 本地静态资源
│   ├── layouts              # 封装布局组件
│   ├── components           # 业务通用组件和基础布局组件
│   ├── router               # Vue-Router
│   ├── store                # Pinia
│   ├── utils                # 工具库
│   ├── views                # 业务页面入口和常用模板
│   ├── App.vue              # Vue 模板入口
│   └── main.ts              # Vue 入口 JS
│   └── app.less             # 全局样式
│   └── env.d.ts             # 全局公用 TypeScript 类型
├── build/mock               # mock 服务
├── mock                     # mock 数据
├── plop-templates           # 代码块生成
├── public                   # 静态文件
├── scripts                  # 公共执行脚本
├── tests                    # 单元测试
├── auto-imports.d.ts        # Vue3 组合式API 类型声明文件
├── components.d.ts          # 组件自注册类型声明文件
├── vite.config.ts           # Vite 配置文件
├── tsconfig.json            # TS 配置文件
├── index.html               # 浏览器入口
├── README.md                # 简单介绍
└── package.json             # 项目的依赖

```

**<font style="color:rgba(0, 0, 0, 0.847);">  
</font>**

  


