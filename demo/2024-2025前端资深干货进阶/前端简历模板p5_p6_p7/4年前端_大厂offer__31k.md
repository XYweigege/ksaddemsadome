

# 张天天
写一段简短的个人简介，如：.

💬 **微信 / 手机：**18888888888 | 📮 **邮箱：**xxx@xxx.com

🎂 **生日：**1995.01 | 📌 **所在地：**北京 | 🧑🏻💻** 求职意向：web前端**

🔗 **个人主页：**xxx.com



## <font style="color:rgb(36,91,219);">🎓</font><font style="color:rgb(36,91,219);"> 个人优势</font>
+ **扎实的前端基础知识：****<font style="color:#DF2A3F;">对 Event Loop, 浏览器渲染原理，渲染帧，垃圾回收机制，ast语法树掌握</font>**
+ **深入理解Vue，React原理, 并研究过其内部实现;**
+ **对Vue2/3响应式原理实现包括 对象 数组等数据类型的监听有深入的研究；**
+ **Vue3相对于Vue2性能上做的优化提升：如****<font style="color:rgb(31, 35, 40);">diff算法优化</font>****, ****<font style="color:rgb(31, 35, 40);">静态标记和提升，事件监听缓存也有自己的理解</font>**
+ **掌握Vue和React框架的diff算法，组件粒度的更新渲染机制；**
+ **对React架构的演进（react15-18版本）有深入的研究：如 ****<font style="color:rgba(0, 0, 0, 0.75);">Scheduler调度器 Reconciler协调器 Renderer渲染器， Fiber架构</font>**
+ **有丰富的微信小程序与h5开发经验，对 冷启动，热启动，离线换成方案，增量更新，h5和natvie混合开发有一定的经验，了解jsbridage实现原理**
+ **熟悉webpack和rollup, vite的使用，通过实现loader，plugins解决业务上的问题。把抽象出来的功能打包成npm包，赋能团队业务**
+ **对JavaScript编译器，swc和bable有一定的理解**
+ **掌握node的基本使用，可做工具和web服务。熟悉monorepo开发及包管理器之间的差异**
+ **熟悉面向对象、函数式等编程范式，深入理解单元测试、TDD等开发模式**
+ **良好的git操作，清晰的commit提交和code review,保证代码质量**
+ **日常使用工具：charles、chrome、vscode开发及调试**

## <font style="color:rgb(36,91,219);">💻</font><font style="color:rgb(36,91,219);"> 工作经历</font>
#### a公司
web前端

_<font style="color:rgb(143,149,158);">2023-</font>_

工作描述：围绕XXX业务线进行选代开发，参与公司基础架构开发，推动typeScript迁移，开发搭建一整套monorepo体系。积极参与公司的技术分享会，吸收并输出自己的理解

技术栈：Vue生态，微信生态，typeScript,jest,ssr 

#### b公司
web前端

_<font style="color:rgb(143,149,158);">2021-2023</font>_

工作描述：一家泛金融公司，主要负责招行开发，参与公司的组件库开发与ui制定统一规范，选代脚手架模板，对新项目进行技术选型，为解决业务痛点，封装出多个库用于业务上。

技术栈：Vue生态，微信生态，银行SDK

#### c公司
web前端

_<font style="color:rgb(143,149,158);">2020-2021</font>_

工作描述：业务范围广泛，针对业务造轮子解决痛点：小程序版axios、受koa启发，通过中间件方式+发布订阅拆解业务

技术栈：React生态，微信生态

## <font style="color:rgb(36,91,219);">💻</font><font style="color:rgb(36,91,219);"> 项目经验(4年做近10个项目，写了重点项目)</font>
#### A项目
前端leader

项目背景：项目复用包是采用git子模块形式管理，为了高效复用和重构，决定用monorepo体系来维护发布npm包

工作：搭建多包管理体系，把核心模块抽离出来，进行开发、维护、重构

1. 基于对monorepo的理解，实现一个新的monorepocli(开源)配合pnpm统筹项目整体，额外提供了人性&智

能化功能

1. 随着项目选代，构建时间逐渐变慢，采用swc替代babel优化打包，使构建速度提升2倍，并试验性使用esbuild更

是提高了接近3倍，构建物降低26%

1. 抽离出来的包比较“简陋”，为了更好的开发以及维护体验，使用typescript进行重构，并补充文档。编写测试提供

可维护性，权衡之下，使用vitest替换jest编写单元测试，

1. 通过eslint,commitlint和husky统一项目开发与代码提交风格
2. 基于roullp编写通用配置骨架，为包提供开箱即用功能同时具备灵活配置

#### B项目
核心开发

项目背景：该项目主要分为20几个模块，其中有日志模块 和监模块是项目的重点

1. 工作：前端的业务迭代维护，优化脚手架，抽离复用组件，优化业务流程，指导分配任务到实习生
2. 客户端新开页面频繁，白屏问题明显，使用vite +egg搭建服务端渲染，提升用户体验
3. 对活动数据结构与玩法沉淀，进行渐进式拆解，抽离常量到后台JSON配置，将页面级组件转换成后台运营编辑
4. 后台管理系统业务模块众多，开发环境构建时间过长，通过指定业务模块构建，将构建时间从分钟级别降低为秒级别
5. 因为第三方数据问题，后端做排序会很影响性能，全量数据返回到前端做处理。前端使用IndexedDB接收数据做分页排序，并基于ElementTabelrender数据结构定义本地数据库表结构、表单渲染与数据导出

## <font style="color:rgb(36,91,219);">🔧</font><font style="color:rgb(36,91,219);"> 个人blog</font>
Gitee: https://asdfadfasdfadsfsadfdsffd

平常学习代码地址：https://asdfadfasdfadsfsadfdsffd

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1729836530955-d0f3685f-50dc-4ca0-a20b-e7e23b8e441b.png)

