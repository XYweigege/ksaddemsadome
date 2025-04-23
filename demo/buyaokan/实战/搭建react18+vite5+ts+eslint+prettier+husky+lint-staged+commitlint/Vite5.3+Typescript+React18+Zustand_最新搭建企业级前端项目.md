 [**代码地址：**](https://gitee.com/sohucw/vite-react-ts-template)

# <font style="color:rgb(37, 41, 51);">1. 初始化项目</font>
1. <font style="color:rgb(37, 41, 51);">node版本要求：目前我的node版本v18.19.0，Vite需要Node.js版本18+，20+。</font>

<br/>color1
<font style="color:rgb(37, 41, 51);">提示：遇到ts报错，有些时候是配置未生效，可以重启vscode或ts服务（vscode快捷键 ctrl+shift+p调出命令行，输入Restart TS Server）</font>

<br/>

```javascript
"scripts": {
  "ts": "tsc -b",
},
```

<font style="color:rgb(37, 41, 51);">运行 pnpm  ts 即可查看文件是否有ts类型错误</font>

<font style="color:rgb(37, 41, 51);"></font>

# <font style="color:rgb(37, 41, 51);">配置路径别名</font>
1. 安装

```javascript
pnpm  i @types/node -D
```

1. 修改vite.config.ts

```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import path from "path"; //这个path用到了上面安装的@types/node

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  //这里进行配置别名
  resolve: {
    alias: {
      "@": path.resolve("./src"), // @代替src
    },
  },
});
```

1. 修改tsconfig.app.json

```javascript
{
  "compilerOptions":{
    //新增
    "baseUrl": ".", //查询的基础路径
      "paths": { "@/*": ["src/*"] }, //路径映射,配合别名使用
  }
}
```

  


# 4. 配置 ESLint 和 prettier
1. 安装

```javascript
//eslint import规则
yarn add eslint-plugin-import@^2.29.1 -D

//eslint prettier插件安装
yarn add eslint-plugin-prettier@^5.2.1 -D

//用来解决与eslint的冲突
yarn add eslint-config-prettier@^9.1.0 -D 

//安装prettier
yarn add prettier@^3.3.3 -D
```

1. 修改 .eslintrc.cjs

```javascript
module.exports = {
  root: true,
  env: { browser: true, es2020: true },
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:react-hooks/recommended',
    'plugin:prettier/recommended',
    'eslint-config-prettier',
  ],
  ignorePatterns: ['dist', '.eslintrc.cjs', 'commitlint.config.cjs'],
  parser: '@typescript-eslint/parser',
  plugins: ['react-refresh','import','prettier'],
  rules: {
    'react-refresh/only-export-components': ['warn', { allowConstantExport: true }],
    //import导入顺序规则
    'import/order': [
      'error',
      {
        //按照分组顺序进行排序
        groups: ['builtin', 'external', 'parent', 'sibling', 'index', 'internal', 'object', 'type'],
        //通过路径自定义分组
        pathGroups: [
          {
            pattern: 'react*', //对含react的包进行匹配
            group: 'builtin', //将其定义为builtin模块
            position: 'before', //定义在builtin模块中的优先级
          },
          {
            pattern: '@/components/**',
            group: 'parent',
            position: 'before',
          },
          {
            pattern: '@/utils/**',
            group: 'parent',
            position: 'after',
          },
          {
            pattern: '@/apis/**',
            group: 'parent',
            position: 'after',
          },
        ],
        //将react包不进行排序，并放在前排，可以保证react包放在第一行
        pathGroupsExcludedImportTypes: ['react'],
        'newlines-between': 'always', //每个分组之间换行
        //根据字母顺序对每个组内的顺序进行排序
        alphabetize: {
          order: 'asc',
          caseInsensitive: true,
        },
      },
    ],
    '@typescript-eslint/no-explicit-any': ['off'], //允许使用any
    '@typescript-eslint/no-this-alias': [
      'error',
      {
        allowedNames: ['that'], // this可用的局部变量名称
      },
    ],
    '@typescript-eslint/ban-ts-comment': 'off', //允许使用@ts-ignore
    '@typescript-eslint/no-non-null-assertion': 'off', //允许使用非空断言
    'no-console': [
      //提交时不允许有console.log
      'warn',
      {
        allow: ['warn', 'error'],
      },
    ],
    'no-debugger': 'warn', //提交时不允许有debugger
  },
}
```

rules更多配置：[eslint.org/docs/latest…](https://eslint.org/docs/latest/rules/)

1. 新建 .prettierrc

```json
{
  "endOfLine": "auto",
  "printWidth": 120,
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "all",
  "bracketSpacing": true
}
```

1. 新建 .prettierignore

```javascript
# 忽略格式化文件 (根据项目需要自行添加)
node_modules
dist
```

1. 重启vscode使配置生效
2. 配置package.json

可以看到App.tsx文件在import处飘红，因为结尾没有使用分号，并且import导入规则也不符合我们设定的规范

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1727591253175-3e00162c-d92b-4635-9449-9feaf81bdd55.png)

修改package.json

```javascript
"scripts": {
  "lint": "eslint src --fix --ext ts,tsx,js,jsx --report-unused-disable-directives --max-warnings 0",
    },
```

运行 yarn lint，可以看到上述eslint(prettier/prettier)问题都将被修复

# 5. 配置 husky、lint-staged、@commitlint/cli
husky：一个为git客户端增加hook的工具

lint-staged：仅对Git 代码暂存区文件进行处理，配合husky使用

@commitlint/cli：让commit信息规范化

1. 创建git仓库

```javascript
git init
```

1. 安装

```javascript
pnpm i husky@9.1.3 -D 

pnpm i lint-staged@^15.2.7 -D

pnpm i @commitlint/cli@^19.3.0 -D

pnpm i @commitlint/config-conventional@^19.2.2 -D
```

1. 生成 .husky 的文件夹

```javascript
npx husky install
```

1. 修改.husky/pre-commit

```javascript
#!/usr/bin/env sh
npx --no-install lint-staged
```

1. 修改.husky/commit-msg

```javascript
#!/usr/bin/env sh
npx --no-install commitlint --edit $1
```

1. 修改package.json

```javascript
"lint-staged": {
  "src/**/*.{js,jsx,ts,tsx}": [
    "yarn run lint",
    "prettier --write"
  ]
}
```

1. 新建commitlint.config.cjs

提示：由于package.json的"type": "module"，需将commonjs文件显示声明为.cjs

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
};
```

提交格式：

```javascript
git commit -m <type>[optional scope]: <description> //注意冒号后面有空格
  - type：提交的类型（如新增、修改、更新等）
  - optional scope：涉及的模块，可选
  - description：任务描述
```

type类型：

| 类别 | 含义 |
| --- | --- |
| feat | 新功能 |
| fix | 修复 bug |
| style | 样式修改（UI校验） |
| docs | 文档更新 |
| refactor | 重构代码(既没有新增功能，也没有修复 bug) |
| perf | 优化相关，比如提升性能、体验 |
| test | 增加测试，包括单元测试、集成测试等 |
| build | 构建系统或外部依赖项的更改 |
| ci | 自动化流程配置或脚本修改 |
| revert | 回退某个commit提交 |


1. 提交示范（非规范提交，将commit提交失败）

```javascript
git commit -m 'feat: 增加 xxx 功能'
git commit -m 'bug: 修复 xxx 功能'
```

# 6. vscode 保存自动格式化
新建.vscode文件夹，在.vscode下新建 settings.json

```javascript
{
  "editor.codeActionsOnSave": {
    "source.fixAll": "explicit"
  }
}
```

之后每次文件有修改，保存时，都会自动格式化

# 7. 配置路由
[react-router-dom（6）最新最全指南](https://www.yuque.com/sohucw/hzbvu7/oly8p2w4ixgg628u)

# 8.配置状态管理库
<font style="color:rgb(37, 41, 51);">zustand使用方式较为接近vue3的状态管理库pinia</font>

[试试更方便的zustand](https://www.yuque.com/sohucw/hzbvu7/ohebfla3vc2gq62d)

[https://www.yuque.com/sohucw/hzbvu7/ohebfla3vc2gq62d](https://www.yuque.com/sohucw/hzbvu7/ohebfla3vc2gq62d)



# 9. 配置scss
antd5已经采用css-in-js方案，不再需要安装less。我们这里展示安装scss示例

1. 安装

```bash
pnpm i  sass -D
```

1. 配置全局 scss 样式文件

新建 src/assets/styles/index.scss

```css
$test-color: red;
```

配置vite.config.ts

```javascript
css: {
  preprocessorOptions: {
    scss: {
      additionalData: '@import "@/assets/styles/index.scss";',
        },
        },
        },
```

新建src/index.scss

```css
body {
  color: $test-color;
}
```

在组件中引入，修改main.tsx

```css
import './index.scss';
```

重启项目，查看效果

提示：平时写react组件样式时，推荐使用styled-components

# 10. 配置antd
1. 安装

```javascript
pnpm i  antd
```

1. 使用

```javascript
import { Button } from 'antd';

const Login = () => {
  return (
    <div>
    登录页
    <Button type="primary">Button</Button>
    </div>
  );
};

export default Login;
```

# 11. 配置环境变量
新建 .env（所有环境生效）.env.development（开发环境配置） .env.production（生产环境配置）

1. 定义变量

以 VITE_ 为前缀定义变量

```javascript
VITE_BASE_URL = '//127.0.0.1:9000/api'
```

1. 定义变量ts类型

修改vite-env.d.ts

```javascript
/// <reference types="vite/client" />
interface ImportMetaEnv {
  readonly VITE_BASE_URL: string;
}

interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

1. 使用变量

```javascript
import.meta.env.VITE_BASE_URL
```

1. 在vite.config.ts中使用环境变量

使用 loadEnv 读取环境变量

```javascript
import { defineConfig, loadEnv } from 'vite';
//...

export default ({ mode }) => {
  console.log('mode', loadEnv(mode, process.cwd()).VITE_BASE_URL); //127.0.0.1:9000/api  
  return defineConfig({
    //...
  });
};
```

使用 pnpm dev 启动命令，读取 .env 与 .env.development的内容

修改package.json

```json
"scripts": {
  "test":"vite --mode test", //新增
},
```

使用 yarn test 启动命令，读取.env 与 .env.test的内容

# 12. 配置请求
1. 安装

```javascript
pnpm i axios 

pnpm i  ahooks
```

1. 新建utils/axios.ts

```javascript
import axios from 'axios';

/*
 * 创建实例
 * 与后端服务通信
 */
const HttpClient = axios.create({
  baseURL: import.meta.env.VITE_BASE_URL,
});

/**
 * 请求拦截器
 * 功能：配置请求头
 */
HttpClient.interceptors.request.use(
  (config) => {
    const token = '222';
    config.headers.authorization = 'Bearer ' + token;
    return config;
  },
  (error) => {
    console.error('网络错误，请稍后重试');
    return Promise.reject(error);
  },
);

/**
 * 响应拦截器
 * 功能：处理异常
 */
HttpClient.interceptors.response.use(
  (config) => {
    return config;
  },
  (error) => {
    return Promise.reject(error);
  },
);

export default HttpClient;
```

1. 新建apis/model/userModel.ts

定义请求参数类型与返回数据类型

```javascript
//定义请求参数
export interface ListParams {
  id: number; //用户id
}

export interface RowItem {
  id: number; //文件id
  fileName: string; //文件名
}

//定义接口返回数据
export interface ListModel {
  code: number;
  data: RowItem[];
}
```

1. 新建apis/user.ts

```javascript
import HttpClient from '../utils/axios';
import type { ListParams, ListModel } from './model/userModel';

export const getList = (params: ListParams) => {
  return HttpClient.get<ListModel>('/list', { params });
};
```

1. 使用

```javascript
import { useRequest } from 'ahooks';
import { getList } from '@/apis/user';

const Login = () => {

  useRequest(getList, {
    defaultParams: [{ id: 2 }],
  });

  return (
    <div>
    登录页
    </div>
  );
};

export default Login;
```

在chrome的network中可以看到请求了http://127.0.0.1:9000/api/list?id=2

提示：useRequest可以帮助我们避免在请求中执行setState，而路由已切换导致内存泄漏问题。以及请求防抖，节流，轮询等功能。

# 13. 配置端口与代理
修改vite.config.ts

```javascript
export default defineConfig({
  ...
    server: {
  host: '0.0.0.0',
    port: 8080, 
    open: true,
    proxy: {
    '/api': {
      target: '要代理的地址',
        changeOrigin: true,
        ws: true,
        rewrite: (path: string) => path.replace(/^\/api/, ''),
        },
        },
        },
  });
```

# 14. 打包配置
修改vite.config.ts

## 1. 分包
```javascript
build: {
  rollupOptions: {
    output: {
      manualChunks: {
        react: ['react', 'react-dom', 'react-router-dom', 'zustand'],
          antd: ['antd'],
          },
          },
          },
    },
```

vite的打包基于rollup，对rollup感兴趣的，可以看Rollup官网

## 2. 生成 .gz 文件
1. 安装

```javascript
pnpm i vite-plugin-compression -D
```

1. 修改vite.config.ts

默认情况下插件在开发 (serve) 和生产 (build) 模式中都会调用，使用 apply 属性指明它们仅在 'build' 或 'serve' 模式时调用

这里打包生成 .gz 插件仅需在打包时使用

```javascript
import viteCompression from 'vite-plugin-compression'

plugins: [
  //...
  {
    ...viteCompression(),
    apply: 'build',
  },
],
```

## 3. js和css文件夹分离
```javascript
export default defineConfig({
  build: {
    rollupOptions: {
      output: {
        chunkFileNames: "static/js/[name]-[hash].js",
        entryFileNames: "static/js/[name]-[hash].js",
        assetFileNames: "static/[ext]/[name]-[hash].[ext]",
      },
    },
  },
});
```

## 4. 分析生成包的大小
1.安装

```javascript
rollup-plugin-visualizer
```

1. 修改vite.config.ts

```javascript
import { visualizer } from 'rollup-plugin-visualizer';

plugins: [
  //...
  visualizer({ open: true }),
],
```

# 15. vite与webpack使用区别
## 1. 静态资源处理
webpack：使用require处理

vite：使用 new URL(url, import.meta.url).href 处理

import.meta.url 包含了对于目前 ES 模块的绝对路径

new URL(url [, base]) 构造函数返回一个新创建的 URL 对象，如果url 是相对 URL，则会将 base 用作基准 URL。如果 url 是绝对 URL，则无论参数base是否存在，都将被忽略

```javascript
new URL('../assets/images/home.png', import.meta.url).href

//在src/constants/menus.ts下引入图片
//import.meta.url返回 http://localhost:8080/src/constants/menus.ts

//new URL(...).href返回
//http://localhost:8080/src/assets/images/home.png
```

示例：

1. 新建utils/share.ts

```javascript
//引入静态资源
export const getAssetsFile = (url: string) => {
  return new URL(`../assets/images/${url}`, import.meta.url).href;
};
```

1. 在assets下新建images文件夹，并放入一张home.png图片
2. 使用

```javascript
import { getAssetsFile } from '@/utils/share'; 

<img src={getAssetsFile('home.png')} /> //在组件内使用
```

## 2. 读取文件资源
webpack

```javascript
//1.directory:要查找的文件路径
//2.useSubdirectories:是否查找子目录
//3.regExp:要匹配文件的正则
const files = require.context(directory,useSubdirectories,regExp)
```

vite

```javascript
//读取同文件夹下所有.tsx文件
const modules = import.meta.glob('./*.tsx');
```

#  
