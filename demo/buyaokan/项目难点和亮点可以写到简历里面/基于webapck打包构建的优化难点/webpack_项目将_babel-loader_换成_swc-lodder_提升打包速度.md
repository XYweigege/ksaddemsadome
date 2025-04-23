## <font style="color:rgb(60, 60, 67);">前言</font>
<font style="color:rgb(60, 60, 67);">我们都知道 webpack 项目中默认的 loader 是用的</font><font style="color:rgb(60, 60, 67);"> </font>`babel-loader`<font style="color:rgb(60, 60, 67);">，而在打包工具这么卷的今天 babel 打包的速度实在是不敢恭维...</font>

<font style="color:rgb(60, 60, 67);">今天给大家介绍一款号称比 babel 速度快 10 倍的 swc-loader</font>

<font style="color:rgb(60, 60, 67);">我们先来看看 babel 打包项目的速度</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1725605151038-31e8a353-ab55-4409-b97e-0db2e69a4c29.jpeg)

<font style="color:rgb(60, 60, 67);">打包将近 100s，这还是我本地的打包速度，如果是在服务器上打包的话，那么打包时间就更长了</font>

<font style="color:rgb(60, 60, 67);">接下来我们来看看</font><font style="color:rgb(60, 60, 67);"> </font>`swc-loader`<font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">的打包速度</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1725605151101-c13ca3a2-eb96-4b22-9a7a-945adac4d25d.jpeg)

<font style="color:rgb(60, 60, 67);">打包时间只相当于 babel 的一半，这个速度真的是快了不少</font>

## <font style="color:rgb(60, 60, 67);">使用</font>
<font style="color:rgb(60, 60, 67);">接下来我们来看看如何使用</font><font style="color:rgb(60, 60, 67);"> </font>`swc-loader`

<font style="color:rgb(60, 60, 67);">首先我们需要安装</font><font style="color:rgb(60, 60, 67);"> </font>`swc-loader`<font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">和</font><font style="color:rgb(60, 60, 67);"> </font>`@swc/core`

`swc-loder`<font style="color:rgb(60, 60, 67);"> 是 webpack 的 loader，</font>`@swc/core`<font style="color:rgb(60, 60, 67);"> 是 swc 的核心库是 </font>`swc-loader`<font style="color:rgb(60, 60, 67);"> 的依赖</font>

****

```bash
pnpm i swc-loader @swc/core -D
```

1

<font style="color:rgb(60, 60, 67);">然后我们需要在</font>`webpack.config.js`<font style="color:rgb(60, 60, 67);">中配置</font>`swc-loader`

****

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: [
          {
            loader: 'swc-loader',
          },
        ],
      },
    ],
  },
}
```

 

<font style="color:rgb(60, 60, 67);">这样我们在打包的时候使用的就是</font>`swc-loader`<font style="color:rgb(60, 60, 67);">了</font>

## <font style="color:rgb(60, 60, 67);">swc-loader 的配置</font>
`swc-loader`<font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">的配置和</font><font style="color:rgb(60, 60, 67);"> </font>`babel-loader`<font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">的配置基本上是一样的，我们来看看如何配置</font>

****

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: [
          {
            loader: 'swc-loader',
            options: {
              jsc: {
                // jsc配置
                parser: {
                  // 解析器配置
                  syntax: 'typescript', // 支持的语法
                  tsx: true, // 支持tsx
                  dynamicImport: true, // 支持动态导入
                  decorators: true, // 支持装饰器
                  privateMethod: true, // 支持私有方法
                  functionBind: true, // 支持函数绑定
                  exportDefaultFrom: true, // 支持export default from
                  exportNamespaceFrom: true, // 支持export * as ns from
                  nullishCoalescing: true, // 支持空值合并
                  optionalChaining: true, // 支持可选链
                  classPrivateProperty: true, // 支持私有属性
                  classPrivateMethod: true, // 支持私有方法
                  classProperty: true, // 支持类属性
                  numericSeparator: true, // 支持数字分隔符
                  bigInt: true, // 支持大整数
                  importMeta: true, // 支持import.meta
                  throwExpressions: true, // 支持throw表达式
                  pipelineOperator: true, // 支持管道操作符
                  nullishCoalescingOperator: true, // 支持空值合并操作符
                  optionalChainingOperator: true, // 支持可选链操作符
                  logicalAssignment: true, // 支持逻辑赋值
                  partialApplication: true, // 支持部分应用
                  privateIn: true, // 支持私有in
                  recordAndTuple: true, // 支持record和tuple
                  topLevelAwait: true, // 支持顶级await
                  importAssertions: true, // 支持import断言
                  moduleAttributes: true, // 支持模块属性
                  exportExtensions: true, // 支持导出扩展
                  functionSent: true, // 支持函数sent
                  exportNamespaceFrom: true, // 支持export * as ns from
                  exportDefaultFrom: true, // 支持export default from
                },
              },
            },
          },
        ],
      },
    ],
  },
}
```



## <font style="color:rgb(60, 60, 67);">总结</font>
`swc-loader`<font style="color:rgb(60, 60, 67);">的配置和</font>`babel-loader`<font style="color:rgb(60, 60, 67);">的配置基本上是一样的，但是</font>`swc-loader`<font style="color:rgb(60, 60, 67);">的打包速度比</font>`babel-loader`<font style="color:rgb(60, 60, 67);">快了不少</font>

<font style="color:rgb(60, 60, 67);">当然并没有什么技术是完美的，</font>`swc-loader`<font style="color:rgb(60, 60, 67);">也有一些缺点，比如</font>`swc-loader`<font style="color:rgb(60, 60, 67);">不支持</font>`@babel/plugin-transform-runtime`<font style="color:rgb(60, 60, 67);">这个插件，所以如果你的项目中使用了这个插件的话，那么你就不能使用 swc-loader 了</font>

`babel`<font style="color:rgb(60, 60, 67);">毕竟是一个成熟的打包工具，所以在使用</font>`swc-loader`<font style="color:rgb(60, 60, 67);">的时候还是要谨慎一些，毕竟</font>`swc-loader`<font style="color:rgb(60, 60, 67);">还是一个新的打包工具，可能会有一些 bug，所以在使用的时候还是要多多注意一些，如果是早期的成熟项目的话，还是建议使用</font>`babel-loader`

