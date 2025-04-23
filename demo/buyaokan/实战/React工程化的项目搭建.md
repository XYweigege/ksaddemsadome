## 错误监控
这种监控可以帮助开发人员及时发现和解决问题，提高应用程序的稳定性和可靠性。

1. Sentry：Sentry是一款开源的错误监控平台，可以监控前端、后端以及移动端应用程序中的错误和异常。Sentry提供了实时错误报告、错误分析和错误解决方案等功能。
2. Bugsnag：Bugsnag是一款专门用于监控Web应用程序和移动应用程序的错误监控工具。它可以捕获JavaScript异常、网络请求错误、客户端错误等。
3. Google Analytics：Google Analytics可以监控网站的访问量、页面浏览量、访问时长和用户行为等。它还提供了实时报告和错误报告等功能，可以帮助开发人员发现和解决问题。
4. Performance API：Performance API是一个浏览器提供的API，可以监控Web应用程序的性能。它可以捕获页面加载时间、资源下载时间和JavaScript执行时间等信息。
5. 前端错误监控SDK：很多前端错误监控工具都提供了JavaScript SDK，可以通过在应用程序中引入SDK来捕获错误和异常。开发人员可以根据捕获的错误信息来定位和解决问题。



## 脚手架开局
#### <font style="color:rgb(38, 38, 38);background-color:rgb(245, 245, 245);">create-react-app</font>
```javascript
全局安装create-react-app：
$ npm install -g create-react-app

创建一个项目：
$ create-react-app your-app 注意命名方式

Creating a new React app in /dir/your-app.

如果不想全局安装，可以直接使用npx：
$ npx create-react-app your-app	也可以实现相同的效果
```

## webpack创建
  

### 初始化项目空间
```json
新建一个项目目录，在目录下执行：npm init -y
此时将会生成 package.json 文件
之后新建 src、config（配置webpack）文件夹，新建index.html文件
```

**<font style="color:rgb(38, 38, 38);">安装webpack和react相关依赖文件</font>**<font style="color:rgb(38, 38, 38);"></font>

<font style="color:rgb(38, 38, 38);background-color:rgb(245, 245, 245);"></font>

```bash
npm i webpack webpack-cli webpack-dev-server html-webpack-plugin babel-loader path -D
npm i react react-dom
```

### 在src目录配置index.js文件
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <React.StrictMode>
  <div>你好，React-webpack5-template</div>
  </React.StrictMode>,
document.getElementById('root')
);
```

### 在src目录配置index.html文件
```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>react-app</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

### 配置config中webpack配置文件
##### 新建webpack.base.conf.js文件，部分代码仅供参考
```javascript
"use strict";
const path = require("path");
const webpack = require("webpack");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  // 入口起点
  entry: {
    app: "./src/index.js",
  },
  // 输出
  output: {
    path: path.resolve(__dirname, "../dist"),
    filename: "[name].js",
  },
  // 解析
  resolve: {
    extensions: [".ts", ".tsx", ".js", ".json"],
    alias: {
      "@components": path.join(__dirname, "../src/components"),
      "@utils": path.join(__dirname, "../src/utils"),
      "@pages": path.join(__dirname, "../src/pages"),
    },
  },
  // loader
  module: {
    rules: [
      {
        test: /\.js|jsx$/,
        exclude: /(node_modules|bower_components)/, // 屏蔽不需要处理的文件（文件夹）（可选）
        loader: "babel-loader",
      },
      {
        //支持less
        // npm install style-loader css-loader less-loader less --save-dev
        test: /\.(le|c)ss$/, // .less and .css
        use: ["style-loader", "css-loader", "less-loader"], // 创建的css文件存在html的头部
      },
    ],
  },
  // 插件
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: "src/index.html",
      inject: "body",
      hash: false,
      minify: {
        collapseWhitespace: true, //把生成文件的内容的没用空格去掉，减少空间
      },
    }),
  ],
};
```

##### 新建webpack.deve.conf.js文件，部分代码仅供参考
```javascript
"use strict";
const { merge } = require("webpack-merge");
const baseWebpackConfig = require("./webpack.base.conf");
const path = require("path");
const webpack = require("webpack");

module.exports = merge(baseWebpackConfig, {
  // 模式
  mode: "development",
  // 调试工具
  devtool: "inline-source-map",
  // 开发服务器
  devServer: {
    static: path.resolve(__dirname, "static"),
    historyApiFallback: true, // 在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html
    compress: true, // 启用gzip压缩
    hot: true, // 模块热更新，取决于HotModuleReplacementPlugin
    host: "127.0.0.1", // 设置默认监听域名，如果省略，默认为“localhost”
    port: 9000, // 设置默认监听端口，如果省略，默认为“8080”
  },
  optimization: {
    nodeEnv: "development",
  },
});
```

##### 新建webpack.prod.conf.js文件
```javascript
"use strict";
const { merge } = require("webpack-merge");
const baseWebpackConfig = require("./webpack.base.conf");

const path = require("path");
const webpack = require("webpack");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = merge(baseWebpackConfig, {
  // 模式
  mode: "production",
  // 调试工具
  devtool: "source-map",
  // 输出
  output: {
    path: path.resolve(__dirname, "../dist"),
    filename: "js/[name].[chunkhash].js",
  },
  // 插件
  plugins: [new CleanWebpackPlugin()],
  // 代码分离相关
  optimization: {
    nodeEnv: "production",
    runtimeChunk: {
      name: "manifest",
    },
    splitChunks: {
      minSize: 30000,
      minChunks: 1,
      maxAsyncRequests: 5,
      maxInitialRequests: 3,
      name: false,
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: "vendor",
          chunks: "initial",
        },
      },
    },
  },
});
```

### 新建.babelrc文件
```javascript
{
  "presets": ["latest", "react", "stage-2"],
  "plugins": []
}
```

### 修改package.json中的script代码
```json
"scripts": {
  "dev": "webpack-dev-server --hot  --config config/webpack.dev.conf.js",
  "start": "npm run dev",
  "build": "webpack --progress --colors --config config/webpack.prod.conf.js"
},
```

package.json中部分代码如下

```json
{
  "name": "webpack-react-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "webpack-dev-server --hot  --config config/webpack.dev.conf.js",
    "start": "npm run dev",
    "build": "webpack --progress  --config config/webpack.prod.conf.js"
  },
  "license": "ISC",
  "devDependencies": {
    "babel-core": "^6.26.3",
    "babel-loader": "^7.1.5",
    "babel-plugin-import": "^1.13.5",
    "babel-preset-latest": "^6.24.1",
    "babel-preset-react": "^6.24.1",
    "babel-preset-stage-0": "^6.24.1",
    "clean-webpack-plugin": "^4.0.0",
    "css-loader": "^6.7.1",
    "file-loader": "^6.2.0",
    "html-webpack-plugin": "^5.5.0",
    "less-loader": "^11.0.0",
    "node-less": "^1.0.0",
    "style-loader": "^3.3.1",
    "url-loader": "^4.1.1",
    "webpack": "^5.74.0",
    "webpack-cli": "^4.10.0",
    "webpack-dev-server": "^4.10.1",
    "webpack-merge": "^5.8.0"
  },
  "dependencies": {
    "less": "^4.1.3",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^5.1.2"
  }
}
```

### 在项目中添加代码规范检测
```bash
yarn add babel-eslint --save-dev
yarn add @umijs/fabric -D   //@umijs/fabric一个包含 prettier，eslint，stylelint 的配置文件合集。
yarn add prettier --save-dev  //默认@umijs/fabric已经给我们安装了需要的依赖，但是默认是没有pretter。

结合项目中安装eslint-plugin-react-hooks并在.eslintrc.js中配置
  rules:{
    "react-hooks/rules-of-hooks":'error',
    "react-hooks/exhaustive-deps":'warn',
  } 
可以一键生成hooks依赖
```

### 新增如下文件（用于规范项目组代码）
##### .eslintrc.js文件
```bash
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: [require.resolve("@umijs/fabric/dist/eslint")],
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 12,
    sourceType: "module",
  },
  parser: "babel-eslint",
  globals: {
    gtag: true,
    $: true,
    _g_deflang: true,
    require: true,
    envConfig: true,
    process: true,
    React: true,
    ysf: true,
    initNECaptcha: true,
    initNECaptchaWithFallback: true,
  },
  // plugins: ["react"],
  rules: {
    //"react/jsx-uses-react": 2,
    "no-nested-ternary": 0, // 允许嵌套三元表达式
    "no-script-url": 0, // 允许javascript:;
    "prefer-destructuring": 0, // 关闭强制使用解构
    "no-plusplus": 0, // 允许使用++和--的操作
    "array-callback-return": 0, // 允许数组map不返回值
    "consistent-return": 0,
    "no-param-reassign": 0, // 允许修改函数形参
    "no-unused-expressions": 0,
    "no-restricted-syntax": 0,
    "react/prop-types": 0,
    "no-prototype-builtins": 0,
    "react/no-deprecated": 0, // 关闭react弃用检测
    "react/no-string-refs": 0,
    "no-useless-escape": 0,
    "react-hooks/rules-of-hooks":'error',
    "react-hooks/exhaustive-deps":'warn',
  },
};
```

##### .eslintignore文件
```json
/lambda/
/scripts/*
.history
serviceWorker.ts
/config/*
/public/*
*.js
```

##### .prettierrc.js文件
```javascript
module.exports = {
  singleQuote: true,
  jsxSingleQuote: true,
  semi: true,
};
```

**.prettierignore文件**

```javascript
**/*.svg
package.json
.umi
.umi-production
/dist
.dockerignore
.DS_Store
.eslintignore
*.png
*.toml
docker
.editorconfig
Dockerfile*
.gitignore
.prettierignore
LICENSE
.eslintcache
*.lock
yarn-error.log
.history
```

### 替换package.json中命令
```javascript
  "scripts": {
    "lint": "umi g tmp && npm run lint:js && npm run lint:style && npm run lint:prettier",
    "lint-staged:js": "eslint --ext .js,.jsx,.ts,.tsx ",
    "lint:fix": "eslint --fix --cache --ext .js,.jsx,.ts,.tsx --format=pretty ./src && npm run lint:style",
    "lint:js": "eslint --cache --ext .js,.jsx,.ts,.tsx --format=pretty ./src",
    "lint:prettier": "prettier --check \"src/**/*\" --end-of-line auto",
    "lint:style": "stylelint --fix \"src/**/*.less\" --syntax less",
    "prettier": "prettier -c --write \"src/**/*\"",
    "precommit": "lint-staged",
    "precommit:fix": "npm run lint:fix && npm run prettier && npm run lint:prettier && npm run lint:style",
    "dev": "webpack-dev-server --hot  --config config/webpack.dev.conf.js",
    "start": "npm run dev",
    "build": "webpack --progress  --config config/webpack.prod.conf.js"
  },
```

### 安装cross-env（运行跨平台设置和使用环境变量的脚本）
```javascript
1、安装：npm install --save-dev cross-env
2、修改启动命令（原命令前加上 cross-env APP_ENV=development 环境变量）：
	 "start:dev": "cross-env APP_ENV=development webpack serve --config webpack.development.js",
 	 "testing": "cross-env APP_ENV=testing webpack --config webpack.testing.js",
   "build": "cross-env APP_ENV=production webpack --config webpack.production.js",
   "preBuild": "cross-env APP_ENV=preProduction webpack --config webpack.production.js",
3、读取环境变量：process.env.APP_ENV
```

### 添加提交前检测
```javascript
#使用husky lint-staged在commit的时候校检你提交的代码是否符合规范
yarn add husky lint-staged -D
```

### package.json新增如下代码
```javascript
  "lint-staged": {
    "**/*.less": "stylelint--syntaxless",
    "**/*.{js,jsx,ts,tsx}": "npmrunlint-staged:js",
    "**/*.{js,jsx,tsx,ts,less,md,json}": [
      "prettier--write"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "npmrunlint-staged"
    }
  }
```

