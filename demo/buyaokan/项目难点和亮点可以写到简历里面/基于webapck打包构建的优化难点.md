随着项目选代，构建时间逐渐变慢，采用swc替代babel优化打包，使构建速度提升2倍，并试验性使用esbuild更

是提高了接近3倍，构建物降低26%

## <font style="color:rgb(31, 35, 40);">swc 和 esbuild 为什么快</font>
<font style="color:rgb(31, 35, 40);">Js 的执行流程：</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1725524174347-70117513-e586-46b3-9c7e-1add5f5d4a15.png)

<font style="color:rgb(31, 35, 40);">将源码转变成 AST 树很耗时，而 swc 是基于 Rust 语音的，它直接将源码根据不同平台编译成对应的二进制文件，直接跳过了转AST 步骤，速度大大提升。</font>

<font style="color:rgb(31, 35, 40);"></font>

**<font style="color:rgb(31, 35, 40);">esbuild 为什么快</font>**

+ <font style="color:rgb(31, 35, 40);">它是用 Go 语言编写的，并可以编译为本地代码；</font>
+ <font style="color:rgb(31, 35, 40);">大量使用并行操作；</font>
+ <font style="color:rgb(31, 35, 40);">未引用第三方依赖；</font>
+ <font style="color:rgb(31, 35, 40);">内存的高效利用，尽量复用 AST 数据。</font>

**<font style="color:rgb(31, 35, 40);">swc 在 webpack 中使用</font>**<font style="color:rgb(31, 35, 40);">  
</font><font style="color:rgb(31, 35, 40);">在 webpack 中需要用 swc-loader 来使用</font>

```javascript
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /(node_modules)/,
      use: {
        // `.swcrc` can be used to configure swc
        loader: "swc-loader"
      }
    }
  ];
}
```

**<font style="color:rgb(31, 35, 40);">webpack 中需要用 esbuild-loader 来使用</font>**

<font style="color:rgb(31, 35, 40);">esbuild-loader 可以用于在 Webpack 中使用 esbuild 去编译 JS、TS；压缩脚本、样式等。</font>

```javascript
module: {
      rules: [
          {
                test: /\.(js|jsx)$/,
                loader: 'esbuild-loader',
                options: {
                    loader: 'jsx',
                    target: 'es2015'
                },
          }
      ]
}
```

