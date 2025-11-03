 

# 【前端工程化】使用dumi2搭建React组件库和函数库详细教程和最佳实践

[Ausra无忧](/user/395479919369575/posts)

2023-04-17 8,661 阅读12分钟

专栏： 

前端工程化

关注

## 目录

1.  dumi简介
2.  创建dumi项目
3.  开发基础组件
4.  基于antd二次开发
5.  开发工具函数
6.  jest单元测试优化
7.  打包部署

**全文概览**

![dumi2.jpeg](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/10366cd3709f4feb99b6d5358c1a82fb~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

本文使用dumi2详细的带你搭建React组件库和函数库，编写单元测试，部署文档站点和发布npm包，以及搭建过程中遇到的问题和解决方案。

## 一. dumi简介

直接引用[官网](https://link.juejin.cn?target=https%3A%2F%2Fd.umijs.org%2F "https://d.umijs.org/")的介绍

### 1.1 什么是 dumi

dumi，中文发音**嘟米**，是一款为组件开发场景而生的静态站点框架，与 [father](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fumijs%2Ffather "https://github.com/umijs/father") 一起为开发者提供一站式的组件开发体验，**father 负责组件源码构建，而 dumi 负责组件开发及组件文档生成**。

简单来说**dumi**是一个开发组件库和函数库的框架，可以方便的开发组件和示例文档，最终把**示例文档**打包为静态站点部署，并且把组件源码打包为最终发布的**npm包**，我们可以专注于组件开发，节省了大量配置的时间。

### 1.2 特性

全新的 dumi 2.0 主要具备以下特性：

* 🚀 **更好的编译性能**：通过结合使用 [Umi 4 MFSU](https://link.juejin.cn?target=https%3A%2F%2Fumijs.org%2Fblog%2Fmfsu-faster-than-vite "https://umijs.org/blog/mfsu-faster-than-vite")、esbuild、SWC、持久缓存等方案，带来比 dumi 1.x 更快的编译速度
* 🔍 **内置全文搜索**：不需要接入任何三方服务，标题、正文、demo 等内容均可被搜索，支持多关键词搜索，且不会带来产物体积的增加
* 🎨 **全新主题系统**：为主题包增加插件、国际化等能力的支持，且参考 [Docusaurus](https://link.juejin.cn?target=https%3A%2F%2Fdocusaurus.io%2Fdocs%2Fswizzling "https://docusaurus.io/docs/swizzling") 为主题用户提供局部覆盖能力，更强更易用
* 🚥 **约定式路由增强**：通过拆分路由概念、简化路由配置等方式，让路由生成一改 dumi 1.x 的怪异、繁琐，更加符合直觉
* 💡 **资产元数据 2.0**：在 1.x 及 JSON Schema 的基础上对资产属性定义结构进行全新设计，为资产的流通提供更多可能
* 💎 **继续为组件研发而生**：提供与全新的 NPM 包研发工具 [father 4](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fumijs%2Ffather "https://github.com/umijs/father") 集成的脚手架，为开发者提供一站式的研发体验

### 1.3 什么时候会用到

搭建自己的函数库和组件库通常是为了提高代码的复用性和开发效率，特别适合在以下情况下使用：

1.  多个项目共用的函数或组件。如果你有多个项目需要使用同样的函数或组件，可以将其封装成库来提高复用性和开发效率。这样做可以避免重复编写代码，同时也方便后续维护和更新。
2.  团队协作开发。如果你在一个团队中进行协作开发，搭建自己的函数库和组件库可以方便不同成员之间的代码共享和协作。这样做可以避免不同成员之间重复编写代码，提高开发效率和代码质量。
3.  提高代码可维护性。搭建自己的函数库和组件库可以方便后续的代码维护和更新。这样做可以避免代码中存在大量的冗余代码和重复代码，提高代码的可读性和可维护性。

## 二. 安装dumi

### 2.1 环境准备

确保正确安装 [Node.js](https://link.juejin.cn?target=https%3A%2F%2Fnodejs.org%2Fen%2F "https://nodejs.org/en/") 且版本为 14+ 即可。

```bash
$ node -v
v14.19.1

```

### 2.2 创建项目

先找个地方建个空目录

```bash
mkdir dumi2-demo && cd dumi2-demo

```

目录创建好后通过官方工具创建项目，选择你需要的模板

```bash
npx create-dumi

```

dumi2目前模版有三种

* Static Site：静态文档站点
* React Library: react组件库
* Theme Package: 主题包

![0.jpeg](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/48a3fd2d46c547dfb557d7128e8ad79b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

在进行模版类型的时候我们选择**React Library**模版，可以开发组件和生成静态站点文档，其他的是选择npm管理工具，包名称，描述和邮箱，按自己情况来输入就好了，这里包名称输入的是**dumi2-demo**。

等待安装完依赖后启动项目：

```bash
npm start

```

项目就启动起来了，输入[http://localhost:8000](https://link.juejin.cn?target=http%3A%2F%2Flocalhost%3A8000 "http://localhost:8000")，可以看到启动成功后的页面，这个界面也就是我们打包后的静态站点。

![1.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/09c71105e6904a699b943361c9179a7e~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 2.3 目录结构

一个普通的、使用 dumi 做研发的组件库目录结构大致如下：

```markdown
.
├── docs               # 组件库文档目录
│   ├── index.md       # 组件库文档首页
│   ├── guide          # 组件库其他文档路由表（示意）
│   │   ├── index.md
│   │   └── help.md
├── src                # 组件库源码目录
│   ├── Button         # 单个组件
│   │   ├── index.tsx  # 组件源码
│   │   ├── index.less # 组件样式
│   │   └── index.md   # 组件文档
│   └── index.ts       # 组件库入口文件
├── .dumirc.ts         # dumi文档的配置文件
└── .fatherrc.ts       # 组件库打包npm包的配置文件

```

### 2.4 完善工作

在运行成功后的页面，可以看到有几个问题，一个是左上角项目名称换行，一个是要把Foo修改为组件库选项。

修改项目名称项目名称换行问题可以根据类名覆盖原有样式，在.dumirc.ts文件中添加注入的css配置:

```ts
import { defineConfig } from 'dumi';

export default defineConfig({
 // ...
 styles: [
   `.dumi-default-header-left {
      width: 220px !important;
   }`,
 ],
});

```

修改Foo选项为组件库，在.dumirc.ts文件中添加配置:

```ts
import { defineConfig } from 'dumi';

export default defineConfig({
  // ...
  themeConfig: {
    name: 'dumi2-demo',
    nav: [
      { title: '介绍', link: '/guide' },
      { title: '组件', link: '/components/Foo' }, // components会默认自动对应到src文件夹
    ],
  },
  // ...
});

```

调整后的界面就变成了，此时就可以开始组件和函数的开发了。

![2.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/43ca55d8ece64b78ad4b224709a39081~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

## 三. 开发基础组件

> mac电脑可以使用自带的命令行操作，window系统建议使用git命令行操作。

### 3.1 **添加组件源代码**

这里先新增一个简单的Button组件，在src下面新增Button文件内写入index.tsx文件。

```bash
mkdir src/Button && touch src/Button/index.tsx

```

在文件中新增一个简单的Button组件代码：

```tsx
import React, { memo } from 'react';
import './styles/index.less' // 引入样式
export interface ButtonProps {
  /** 按钮类型 */
  type?: 'primary' | 'default';
  /** 按钮文字 */
  children?: React.ReactNode;
  onClick?: React.MouseEventHandler<HTMLButtonElement>
}

/** 按钮组件 */
const Button: React.FC<ButtonProps> = (props) => {
  const { type = 'default', children, onClick } = props
  return (
    <button
      type='button'
      className={`dumi-btn ${type ? 'dumi-btn-' + type : ''}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
};

export default memo(Button);

```

### 3.2 **添加less样式和变量**

先定义less样式变量，方便在项目中使用的时候改变组件主题样式。

在src下创建variables.less文件

```bash
touch src/variables.less

```

添加主题代码：

```less
// src/variables.less
@dumi-primary: #4d90fe; // 主题颜色

```

然后在Button组件库下面新建styles文件夹，里面新建index.less文件

```less
mkdir src/Button/styles && touch src/Button/styles/index.less

```

添加样式文件

```less
// src/Button/styles/index.less
@import '../../variables.less';

.dumi-btn {
  font-size: 14px;
  height: 32px;
  padding: 4px 15px;
  border-radius: 6px;
  transition: all .3s;
  cursor: pointer;
}

.dumi-btn-default {
  background: #fff;
  color: #333;
  border: 1px solid #d9d9d9;

  &:hover {
    color: @dumi-primary;
    border-color: @dumi-primary;
  }
}

.dumi-btn-primary {
  color: #fff;
  background: @dumi-primary;
  border: 1px solid @dumi-primary;
}

```

组件源代码添加好后，需要在src/index.ts中引入后暴露一下：

```ts
// src/index.ts
export { default as Button } from './Button';

```

在这里引入并暴露出去以后，就可以在项目中通过`import { Button } from 'dumi2-demo';`来引入了。

### 3.3 **添加demo示例**

每一个组件我们可以加一个demo示例，方便使用者能更方便的使用。

在Button目录下新建一个demo文件夹，内建一个基础演示base.tsx文件:

```bash
mkdir src/Button/demo && touch src/Button/demo/base.tsx

```

然后添加组件的演示代码：

```tsx
// src/Button/demo/base.tsx

import React from 'react';
import { Button } from 'dumi2-demo';

export default () => {

  return (
    <>
      <Button type="default">默认按钮</Button> &nbsp;
      <Button type="primary">主要按钮</Button>
    </>
  );
}

```

### 3.4**添加组件文档**

再在该文件同目录新建一个index.md文件作为文档说明，这也是生成静态文档站点所需要的。

```bash
touch src/Button/index.md

```

添加文档内容，具体内容描述可以看官网[MakeDown配置项](https://link.juejin.cn?target=https%3A%2F%2Fd.umijs.org%2Fconfig%2Fmarkdown "https://d.umijs.org/config/markdown")，这里只在注释里面讲一下用到的配置。

```markdown
---
category: Components
title: Button 按钮 # 组件的标题，会在菜单侧边栏展示
toc: content # 在页面右侧展示锚点链接
group: # 分组
  title: 基础组件 # 所在分组的名称
  order: 1 # 分组排序，值越小越靠前
---

# Button 按钮

## 介绍

基础的按钮组件 Button。

## 示例 

<!-- 可以通过code加载示例代码，dumi会帮我们做解析 -->
<code src="./demo/base.tsx">基础用法</code>

## APi

<!-- 会生成api表格 -->
| 属性 | 类型                   | 默认值   | 必填 | 说明 |
| ---- | ---------------------- | -------- | ---- | ---- |
| type | 'primary' | 'default' | 'default |  false  | 按钮类型 |

```

全部配置好后，需要重启一下dumi2项目，重启后就可以在浏览器看到效果了。

![3.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c054950d954412b9bc9d31c263eb57b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 3.5 添加单元测试

**准备工作**

添加好基础组件后，通常需要加上给组件添加测试代码，来保障组件的健壮性。

测试框架采用react最常用的jest工具，再配合react配套的单元测试库，安装依赖：

```bash
npm i jest @testing-library/react @types/jest ts-jest jest-environment-jsdom jest-less-loader typescript@4 -D

```

* Jest: jest单元测试核心库
* @testing-library/react：React的测试工具库，在React应用中进行单元测试、集成测试和端到端测试。
* @types/jest：jest的类型。
* ts-jest：让jest支持ts语法的预设。
* jest-environment-jsdom: jest的测试环境，使用js-dom库模拟dom环境，默认是node环境。
* jest-less-loader: jest不认识less和css，使用该插件使jest支持less和css。
* tytpescript: 这是安装了4版本，最新的5版本会有警告。

装好依赖后在项目根目录新建jest的配置文件jest.config.js

```bash
touch jest.config.js

```

在jest.config.js添加以下配置：

```js
/** @type {import('ts-jest').JestConfigWithTsJest} */
module.exports = {
  preset: 'ts-jest', // 使用ts-jest预设，支持用ts写单元测试
  testEnvironment: 'jsdom', // 设置测试环境为jsdom环境
  roots: ['./src'], // 查找src目录中的文件
  collectCoverage: true, // 统计覆盖率
  coverageDirectory: 'coverage', // 覆盖率结果输出的文件夹
  transform: {
    '\.(less|css)$': 'jest-less-loader' // 支持less
  },
  // 单元覆盖率统计的文件
  collectCoverageFrom: ['src/**/*.tsx', 'src/**/*.ts', '!src/index.ts', '!src/**/demo/*'],
};

```

**编写单元测试**

在Button目录下新建一个`__tests__`文件夹放置单元测试代码，在里面新建index.test.tsx。

```bash
mkdir src/Button/__tests__ && touch src/Button/__tests__/index.test.tsx

```

在index.test.tsx文件中编写我们的单元测试代码

```tsx
// src/Button/__tests__/index.test.tsx

import React from 'react';
import { render, fireEvent } from '@testing-library/react';
import Button from '..';

describe('Button组件', () => {
  it('能够正确渲染按钮文字', () => {
    const buttonText = '按钮文字';
    const { getByRole } = render(<Button>{buttonText}</Button>);
    const buttonElement = getByRole('button');
    expect(buttonElement.innerHTML).toBe(buttonText);
  });

  it('能够正确渲染默认样式的按钮', () => {
    const { getByRole } = render(<Button >默认按钮</Button>);
    const buttonElement = getByRole('button');
    expect(buttonElement.classList.contains('dumi-btn')).toBe(true);
  });

  it('能够正确渲染主要样式的按钮', () => {
    const { getByRole } = render(<Button type="primary">主要按钮</Button>);
    const buttonElement = getByRole('button');
    expect(buttonElement.classList.contains('dumi-btn-primary')).toBe(true);
  });

  it('能够触发点击事件', () => {
    const handleClick = jest.fn();
    const { getByRole } = render(<Button type="primary" onClick={handleClick}>点击按钮</Button>);
    const buttonElement = getByRole('button');
    fireEvent.click(buttonElement);
    // 断言函数被调用了一次。
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});

```

单元测试代码分别测试了按钮基础渲染，渲染默认样式，渲染主要样式以及点击测试功能。

保存后在命令行执行npx jest执行一下单元测试:

```bash
npx jest

```

> jest会自动寻找目录中`__tests__`文件夹，去执行内部以.test.{js, jsx, ts, tsx}结尾的文件。

![4.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6fb62832cbdf49f584513268ce096da3~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

可以看到执行了Button组件的单元测试，并且最后生成了测试报告，标注了测试了哪些文件，每个文件的覆盖率，测试情况。

并且会在项目根目录下生成测试报告的静态站点**coverage**文件夹，在浏览器打开coverage/lcov-report/index.html文件，即可看到测试报告，通过对每个文件的分析，可以看到有哪些单元测试没覆盖到的地方，可以帮助我们更好的写单元测试。

![5.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2e4394d3a0bf463e90d5d978c09c64ea~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

## 四. 基于antd二次开发

有时候除了从0封装基础组件之外，还会基于antd等组件库进行二次开发，方式和开发基础组件是一样的，只是要在打包package包时注意css的引入。

### 4.1 添加组件代码

先安装antd库，安装到开发依赖，后面会添加到peerDependencies依赖中。

```bash
npm i antd -D

```

先做一个基础的例子，二次封装一下antd的按钮组件, 让它是primary风格的组件。

在src下新建一个PrimaryButton文件夹，内建index.tsx

```bash
mkdir src/PrimaryButton && touch src/PrimaryButton/index.tsx

```

在index.tsx里面编写代码：

```tsx
// src/PrimaryButton/index.tsx

import React, { memo } from "react";
import { Button, ButtonProps } from "antd";

type IPrimaryButtonProps = Omit<ButtonProps, 'type'>

const PrimaryButton: React.FC<IPrimaryButtonProps> = (props) => {

  const { children, ...rest } = props

  return (
    <Button {...rest} type='primary'>
      {children}
    </Button>
  );
};

export default memo(PrimaryButton);

```

组件源代码添加好后，需要在src/index.ts中引入后暴露一下：

```tsx
// src/index.ts
export { default as PrimaryButton } from './PrimaryButton';

```

在这里引入并暴露出去以后，dumi会帮我门放在包名称dumi2-demo里面，就可以在项目中通过

`import { PrimaryButton } from 'dumi2-demo';`来引入了。

### 4.2 **添加demo示例**

在PrimaryButton目录下新建一个demo文件夹，内建一个基础演示base.tsx文件。

```bash
mkdir src/PrimaryButton/demo && touch src/PrimaryButton/demo/base.tsx

```

添加组件示例代码：

```tsx
// src/Button/demo/base.tsx

import React from 'react';
import { PrimaryButton } from 'dumi2-demo';

export default () => {

  return (
    <PrimaryButton>默认按钮</PrimaryButton>
  );
}

```

### 4.3 添加组件文档

再在该文件同目录新建一个index.md文件作为文档说明，这也是生成静态文档站点所需要的。

```bash
touch src/PrimaryButton/index.md

```

添加文档内容，具体内容描述可以看官网[MakeDown配置项](https://link.juejin.cn?target=https%3A%2F%2Fd.umijs.org%2Fconfig%2Fmarkdown "https://d.umijs.org/config/markdown")，这里只在注释里面讲一下用到的配置。

```markdown
---
category: Components
title: PrimaryButton # 组件的标题，会在菜单侧边栏展示
toc: content # 在页面右侧展示锚点链接
group: # 分组
  title: 二次封装组件 # 所在分组的名称
  order: 2 # 分组排序，值越小越靠前
---

# PrimaryButton 按钮

## 介绍

基础的按钮组件 PrimaryButton。

## 示例 

<!-- 可以通过code加载示例代码，dumi会帮我们做解析 -->
<code src="./demo/base.tsx">基础用法</code>

## APi

<!-- 会生成api表格 -->
| 属性 | 类型                   | 默认值   | 必填 | 说明 |
| ---- | ---------------------- | -------- | ---- | ---- |
| size | 'small' | 'midlle' | 'large |  false  | 按钮大小 |

```

然后重启一下dumi2项目，可以看到页面上已经有最新添加的组件了。

![6.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba59ceceaeb74540aa2994b299850f36~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 4.4 添加单元测试

在PrimaryButton目录下新建一个`__tests__`文件夹放置单元测试代码，在里面新建index.test.tsx。

```bash
mkdir src/PrimaryButton/__tests__ && touch src/PrimaryButton/__tests__/index.test.tsx

```

在index.test.tsx文件中编写我们的单元测试代码

```tsx
// src/PrimaryButton/__tests__/index.test.tsx

import { render } from '@testing-library/react';
import { ButtonProps } from 'antd';
import React from 'react';
import PrimaryButton from '..';

describe('PrimaryButton按钮', () => {
  const buttonProps: ButtonProps = {
    loading: false,
    onClick: jest.fn(),
  };

  it('正确渲染按钮', () => {
    const buttonText = '点击按钮';
    const { getByRole } = render(
      <PrimaryButton {...buttonProps}>{buttonText}</PrimaryButton>,
    );
    const buttonElement = getByRole('button');
    expect(buttonElement.textContent).toBe(buttonText);
  });

  it('正确渲染按钮默认type', () => {
    const { getByRole } = render(
      <PrimaryButton {...buttonProps}>点击按钮</PrimaryButton>,
    );
    const buttonElement = getByRole('button');
    expect(buttonElement.classList.contains('ant-btn-primary')).toBe(true);
  });
});

```

一个基于antd4二次开发简单组件就封装好了，由于在封装的时候没有引入antd原Button组件的样式，打包后会出现样式丢失问题，在最后打包章节会有处理方式。

## 五. 开发工具函数

dum2i除了开发组件库之外，也能开发函数库，开发函数库要比组件库简单很多，而且不限制前端框架，vue，react里面都能使用。

### 5.1 添加工具函数代码

写一个时间格式化的工具函数，在src下新建一个formatTime文件夹，新增index.ts。

```bash
mkdir src/formatTime && touch src/formatTime/index.ts

```

在index.ts里面编写代码：

```ts
// src/formatTime/index.ts

/**
  格式化时间戳
  @param timestamp 时间戳，单位为毫秒
  @param format 时间格式，如YYYY-MM-DD hh:mm:ss
  @returns 返回格式化后的时间字符串
*/
function formatTime(timestamp: number, format='YYYY-MM-DD hh:mm:ss'): string {
  const date = new Date(timestamp);
  const year = date.getFullYear();
  const month = ('0' + (date.getMonth() + 1)).slice(-2);
  const day = ('0' + date.getDate()).slice(-2);
  const hours = ('0' + date.getHours()).slice(-2);
  const minutes = ('0' + date.getMinutes()).slice(-2);
  const seconds = ('0' + date.getSeconds()).slice(-2);
  const map: { [key: string]: string } = {
    YYYY: String(year),
    MM: month,
    DD: day,
    hh: hours,
    mm: minutes,
    ss: seconds,
  };
  return format.replace(/YYYY|MM|DD|hh|mm|ss/g, (matched) => map[matched]);
}

export default formatTime;

```

组件源代码添加好后，需要在src/index.ts中引入后暴露一下：

```ts
// src/index.ts
export { default as formatTime } from './formatTime';

```

在这里引入并暴露出去以后，dumi会帮我门放在包名称dumi2-demo里面，就可以在项目中通过

`import { formatTime } from 'dumi2-demo';`来引入了。

### 5.2 **添加demo示例**

在formatTime目录下新建一个demo文件夹，内建一个基础演示base.tsx文件:

```tsx
// 
import React, { useEffect, useState } from 'react';
import { formatTime } from 'dumi2-demo';

const App: React.FC = () => {
  const [currentDate, setCurrentDate] = useState(formatTime(Date.now(), 'YYYY年MM月DD日 hh:mm:ss'));
  const [siteDate, setSiteDate] = useState<string>();

  useEffect(() => {
    // 指定时间戳时间
    const timestamp=1673850986000 //2023-01-16 14:36:26
    const siteStr: string = formatTime(timestamp);
    setSiteDate(siteStr);
  }, []);

  useEffect(() => {
    // 每秒更新一次时间
    const timer = setInterval(() => {
      const date = Date.now();
      const dateStr = formatTime(date, 'YYYY年MM月DD日 hh:mm:ss');
      setCurrentDate(dateStr);
    }, 1000);
    return () => {
      clearInterval(timer);
    }
  }, []);

  const inputRef = React.createRef<HTMLInputElement>();
  const onFormatData = () => {
    const value = inputRef.current?.value;
    if (value) {
      const dateStr = formatTime(Number(value), 'YYYY年MM月DD日 hh:mm:ss');
      setSiteDate(dateStr);
    }
  };

  return (
    <>
      当前时间：{currentDate}
      <hr />
      指定时间转换：
      <input type="number" ref={inputRef} defaultValue={1673850986000} />
      &nbsp;<button type='button' onClick={onFormatData}>转换</button>&nbsp;
      {siteDate}
    </>
  );
};

export default App;

```

### 5.3 添加工具文档

在formatTime目录下新建一个index.md文件:

```markdown
---
category: Components
title: 时间格式化 # 组件的标题，会在菜单侧边栏展示
toc: content # 在页面右侧展示锚点链接
group: # 分组
  title: 工具函数 # 所在分组的名称
  order: 3 # 分组排序，值越小越靠前
---

### formatTime

将时间戳格式化成指定的日期时间格式。

#### 示例

<!-- 可以通过code加载示例代码，dumi会帮我们做解析 -->

<code src="./demo/base.tsx">基础用法</code>

### 参数

| 参数名    | 类型   | 是否必填 | 默认值                  | 说明                                                       |
| --------- | ------ | -------- | ----------------------- | ---------------------------------------------------------- |
| timestamp | number | 是       | -                       | 要格式化的时间戳，单位为毫秒                               |
| format    | string | 否       | `'YYYY-MM-DD hh:mm:ss'` | 要格式化成的日期时间格式，默认为 `'YYYY-MM-DD hh:mm:ss'`。 |

#### 返回值

类型：string
格式化后的日期时间字符串。

```

然后重启一下dumi项目，可以看到页面上已经有最新添加的formatTime函数了。

![7.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3193fd15a0c24320857c723868689e8a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 5.4 添加单元测试

在formatTime目录下新建一个`__tests__`文件夹放置单元测试代码，在里面新建index.test.tsx。

在index.test.tsx文件中编写我们的单元测试代码

```tsx
// src/formatTime/__tests__/index.test.tsx
import formatTime from '..';

describe('formatTime', () => {
  it('正确格式化指定时间', () => {
    const timestamp = 1681216363389;
    const formattedDate = formatTime(timestamp, 'YYYY年MM月DD日 hh时mm分ss秒');
    expect(formattedDate).toEqual('2023年04月11日 20时32分43秒');
  });

  it('默认格式化指定时间', () => {
    const timestamp = 1681216363389;
    const formattedDate = formatTime(timestamp);
    expect(formattedDate).toEqual('2023-04-11 20:32:43');
  });
});

```

到这里工具函数库的示例也就添加成功了。

## 六. jest单元测试优化

刚才开发了两个组件和一个工具函数，并且都编写了jest单元测试，现在再来执行一下npx jest看一下效果:

![8.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ef20ed8ec50748da9979a91b2ccc2c20~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

可以看到每个单元测试执行情况和覆盖率。

### 6.1 全量单元测试

每次手动执行执行npx不太方便，可以把命令添加在package.json的脚本script标签里面。

```json
// package.json

"scripts": {
  "test:all": "jest --coverage --bail"
}

```

jest的命令参数:

* \--bail: 遇到测试用例失败时立即停止测试，不再执行剩余的测试用例，可以节省时间。
* \--coverage：生成单元测试覆盖率报告。
* \--findRelatedTests: 可以指定要执行的单元测试文件。

再次执行npm run test:all就可以进行单元测试了。

### 6.2 按需单元测试

直接npx jest的方式是全量进行单元测试，但在正常开发过程中一般只需要测试我们修改了的方法，不需要每一次都进行全量测试，可以有效节省时间。

可以自己写一个脚本，通过git diff --staged --diff-filter=ACMR --name-only命令获取到本次修改的文件列表，然后进行分析需要执行哪些单元测试，通过--findRelatedTests参数去精准执行对应的单元测试文件。

按这个思路在项目根目录新建jest.staged.js，添加代码：

```js
const fs = require('fs').promises;
const path = require('path');
const { execSync } = require('child_process');

/** 处理jest只执行本次修改到的工具方法内的测试用例 */
async function start() {
  /** 1. 获取git add 的文件的列表 */
  const addFiles = execSync(`git diff --staged --diff-filter=ACMR --name-only`)
    .toString()
    .split('\n');
  /** 2. 获取文件的绝对路径 */
  const diffFileList = addFiles
    .filter(Boolean)
    .map((item) => path.join(__dirname, item));

  /** 3. 获取src源码目录 */
  const srcPath = path.join(__dirname, './src');

  /** 4. 记录本次修改的函数方法 */
  const diffFileMap = {};
  diffFileList.forEach((filePath) => {
    if (
      filePath.includes(srcPath) &&
      (filePath.endsWith('.ts') || filePath.endsWith('.tsx'))
    ) {
      const relativePath = path.relative(srcPath, filePath);
      if (relativePath.includes('/')) {
        diffFileMap[relativePath.split('/')[0]] = true;
      }
    }
  });

  console.log('本次改动的方法', Object.keys(diffFileMap));

  /** 5. 找到改动方法下面所有的单元测试文件 */
  const list = (
    await Promise.all(
      Object.keys(diffFileMap).map(async (toolPath) => {
        const testsDir = path.join(srcPath, toolPath, '__tests__');
        try {
          const files = await fs.readdir(testsDir);
          return files.map((item) => path.join(testsDir, item));
        } catch (error) {
          return [];
        }
      }),
    )
  ).flat();

  /** 6. 执行单元测试脚本 */
  if (list.length) {
    try {
      execSync(`npx jest --bail --findRelatedTests ${list.join(/ /)}`, {
        cwd: __dirname,
        stdio: 'inherit',
      });
    } catch (error) {
      process.exit(1);
    }
  }
}

start();

```

写好脚本代码后，在package.json的脚本script标签里面。

```json
// package.json

"scripts": {
  "test:staged": "node jest.staged.js"
}

```

然后每次修改代码提交前都可以通过执行npm run test:staged来精确执行修改到的单元测试文件。

> 前提项目要git init，有.git文件

也可以把命令加到lint-staged里面每一次提交代码时自动执行按需执行jest单元测试。

lint-staged在dumi项目创建时已经默认安装依赖配置好了，我们只要修改一下package.json的lint-staged字段就可以了。

```json
// package.json

"lint-staged": {
  "src/**/*.{md,json}": [
    "prettier --write --no-error-on-unmatched-pattern"
  ],
  "src/**/*.{css,less}": [
    "stylelint --fix",
    "prettier --write"
  ],
  "src/**/*.{ts, tsx}": [
    "eslint --fix",
    "prettier --parser=typescript --write",
    "npm run test:staged"
  ]
},

```

再改一下.husky/pre-commit文件把里面的npx lint-staged改为npx lint-staged -p true -v，这样才能在执行lint-staged的时候在控制台看到打印的信息。

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx lint-staged -v # 加上-v参数(--verbose 缩写)才能看到执行的js文件打印的日志

```

现在每次git commit提交代码如果组件或函数代码有改动，会通过lint-staged触发npm run test:staged命令，然后再内部使用git获取到修改到的文件，对文件进行分析，去执行对应组件或工具函数下面的jest单元测试。

## 七. 打包部署

在组件或者工具函数开发完成后，就需要进行部署操作了，部署分为两部分部署：

一是打包文档静态站点文档，让用户可以通过域名访问到文档站点，方便其使用。

二是打包组件库源码，部署到npm仓库上面，让其他人可以通过npm安装使用。

### 7.1 打包静态站点

打包静态站点dumi2在创建项目时已经配置好了命令，只需要在控制台执行

```bash
npm run docs:build

```

打包完成后会在项目中生成docs-dist文件夹，该文件夹就是部署静态文档站点的静态资源。

![9.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ef8944e639bf4a5fbf32184a7e3a0669~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

在本地可以借助serve起一个服务托管静态进行测试一下，全局安装serve:

```bash
npm i serve -g

```

安装完成后在项目根目录执行命令托管文档静态站点：

```bash
serve -s docs-dist

```

打开提示的服务访问链接[http://localhost:3000/](https://link.juejin.cn?target=http%3A%2F%2Flocalhost%3A3000%2F "http://localhost:3000/")，就可以在浏览器访问到打包后的静态站点，实际情况下需要部署到服务器上面，可以借助nginx来部署。

### 7.2优化静态站点打包

在静态站点默认配置下，会把每一个组件或者函数单独打包一份静态文件在components文件夹下，在上图我们也可以看到，但实际上一般是不需要再单独生成一份的，可以修改打包配置，取消打包单个静态资源。

修改.dumirc.ts文件

```ts
import { defineConfig } from 'dumi';

export default defineConfig({
  // ...
  // 取消打包静态单个组件库和函数工具
  exportStatic: false
});

```

### 7.3 打包npm源码包

静态站点打包好后，就需要打包组件和函数库最终的npm包了，dumi2也在创建项目时就提供了npm包打包的命令，直接执行:

```bash
npm run build

```

打包完成后会在项目中生成dist文件夹，该dist文件夹就是最终要发布到npm仓库上的源码。

![10.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e428861a86fd4fa18d7c5b262e32c786~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

发布的时候除了dist文件之外，还需要在package.json里面做配置:

把antd添加到，表示使用该组件库，必须要先安装antd对应版本。

```json
"peerDependencies": {
  "react": ">=16.9.0",
  "react-dom": ">=16.9.0",
  "antd": ">=5.4.2"
},

```

package.json的name字段对应npm包的版本号，第一次发可以0.0.1，后面再发就需要修改版本号。

其他的字段files和module，types在项目初始化的时候dumi就帮我们设置好了。

然后去npm官网注册账号，在命令行通过npm login进行登录，最后回到项目目录，打开命令行，输入

```bash
npm publish

```

就可以发布到npm仓库了。

### 7.4 优化npm源码打包

在上面打包源码包图中，我们可以看到在函数源码下面依然有demo文件夹，但实际使用过程中是不会用到的，可以通过配置在打包npm源码包的时候把demo文件夹过滤掉。

因为打包npm源码是用father来打包的，所以我们要修改.fatherrc.ts配置文件:

```ts
import { defineConfig } from 'father';

export default defineConfig({
  esm: {
    // ...
    ignores: [
      'src/**/demo/**', // 避免打包demo文件到npm包里面
    ],
  },
  // ...
});

```

再一次打包就会发现demo文件夹不会出现在最终的npm包dist文件夹里面了。

### 7.5 解决antd打包npm后没有样式

上面虽然基于antd封装好了按钮，在文档预览没有问题，但是在把npm包引入使用的时候会出现样式丢失的问题，有三个常见的解决方案。

1.  直接全局引入antd的样式，但这样虽然可以解决问题，但是会引入很多不必要的css资源，增加项目体积，所以不推荐。
2.  在二次封装的组件内手段引入对应antd组件的样式，这样虽然可以解决样式丢失问题，并且支持按需引入，但需要手动加比较麻烦。
3.  可以在npm包打包配置.fatherrc.ts添加antd按需引入css的配置，安装按需引入依赖：

```bash
npm i babel-plugin-import -D

```

然后在.fatherrc.ts添加extraBabelPlugins配置

```ts
import { defineConfig } from 'father'

export default defineConfig({
  // ...
  // 打包的时候自动引入antd的样式链接
  extraBabelPlugins: [
    [
      'babel-plugin-import',
      {
        libraryName: 'antd',
        libraryDirectory: 'es',
        style: true,
      },
    ],
  ],
})

```

添加配置后打包npm包就会添加上antd的样式了。

### 7.6 解决组件不能按需引入

在发布到npm上面后本地安装使用时发现组件库没有tree-shaking效果。

```ts
npm i dumi2-demo -S

import { PrimaryButton } from 'dumi2-demo'

```

问题原因是由样式less文件引起的，构建工具认为样式有副作用，所以没有进行tree-shaking操作，解决方案只需要在dmi2组件代码的package.json里面加上sideEffet，告诉构建工具这个npm包没有副作用可以进行tree-shaking。

修改组件库的package.json，添加：

```json
{
  // ...
  "slideEffects": false,
}

```

修改后使用组件就会按需加载，有tree-shking效果了。

## 八. 总结

到目前为止，一个基础的组件库和函数库demo就开发好了，在开发过程中也遇到了一些问题，还有待优化的地方，比如现在打包的是es模块，有时候也会需要支持common模块和在浏览器上面直接使用的umd，以及打包时把less编译为css，还有开发移动端的组件。

本文是总结自己在工作中使用dumi2搭建组件库和函数库中使用到的配置, 也很多没有做好的地方，后续有好的使用技巧和配置也会继续更新记录，附上本文demo相关的地址：

demo静态文档站点地址：[dumi2.guojiongwei.top](https://link.juejin.cn?target=https%3A%2F%2Fdumi2.guojiongwei.top "https://dumi2.guojiongwei.top")

demo源码github地址：[github.com/guojiongwei…](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fguojiongwei%2Fdumi2-demo "https://github.com/guojiongwei/dumi2-demo")

demo npm包地址：[www.npmjs.com/package/dum…](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fdumi2-demo "https://www.npmjs.com/package/dumi2-demo")

往期前端工程化系列文章:

[【前端工程化】webpack5从零搭建完整的react18+ts开发和打包环境](https://juejin.cn/post/7111922283681153038)

[【前端工程化】配置React+ts项目完整的代码及样式格式和git提交规范](https://juejin.cn/post/7101596844181962788)

[【前端工程化】使用verdaccio搭建公司npm私有库完整流程和踩坑记录](https://juejin.cn/post/7096701542408912933)

[【前端工程化】vue2+webpack3老项目迁移vite2流程和踩坑总结](https://juejin.cn/post/7094130826471964708)[juejin.cn/post/709413…](https://juejin.cn/post/7094130826471964708))