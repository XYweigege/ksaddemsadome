<font style="color:rgb(51, 51, 51);">在vite运行build时，默认会把所有的库文件合并到一个大文件中，产生一个打包后的js文件，其中会包含各种库文件，会导致最后打包完成后的文件过大，同时减慢打包速度，这时可以把库文件代码使用cdn访问，加快打包和网页打开速度。  
</font>

<font style="color:rgb(51, 51, 51);">首先需要确认的是，vite在开发时使用的是自己的一套机制，也就是将代码转换成浏览器原生可以使用的</font><font style="color:rgb(192, 0, 0);">type="module"</font><font style="color:rgb(51, 51, 51);">，然后让浏览器的js引擎去直接运行js，不再需要使用构建工具打包，所以可以做到秒开。而vite的编译使用的是另外一个开源的包</font><font style="color:rgb(192, 0, 0);">rollup</font><font style="color:rgb(51, 51, 51);">，这也是这篇文章的主角，而且需要注意的是，开发时dev server是不会去使用cdn的代码的，只会引用我们安装的node_modules内部的代码。</font>

<font style="color:rgb(51, 51, 51);">但是这个时候又出现问题了，搞定这些问题前前后后花了我大半天的时间，终于功夫不负有心人，搞定了。</font>

## 使用rollup自带的ouput.globals
<font style="color:rgb(51, 51, 51);">首先参考的是vite用户在github的一条issues，其中尤大提到了</font><font style="color:rgb(192, 0, 0);">rollupOptions.external</font><font style="color:rgb(51, 51, 51);">这个东西，然后我参考了vite的文档，文档中指向了rollup的配置。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1727425066991-b8a93418-6eef-4578-a99d-379ae69cb707.jpeg)

<font style="color:rgb(51, 51, 51);">跳转之后就可以看到官方的文档的第一条就是说明的external，经过百度查找，我打开了第一篇csdn的文章。  
</font>

[<font style="color:rgb(51, 51, 51);">https://blog.csdn.net/qq_29722281/article/details/95596372</font>](https://blog.csdn.net/qq_29722281/article/details/95596372)

<font style="color:rgb(51, 51, 51);">内部提到了使用</font><font style="color:rgb(192, 0, 0);">globals</font><font style="color:rgb(51, 51, 51);">来指定全局模块名称，这个地方给的样例代码其实已经有问题了：  
</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1727425065762-0078079d-9ab1-4dc6-bad6-bc1832cee47f.png)

<font style="color:rgb(51, 51, 51);">新版的rollup更新后，globals选项已经被移动到了output.globals，如果不修改则会发生以下错误：</font>

> <font style="color:rgb(85, 85, 85);background-color:rgb(246, 246, 246);">Unknown input options: globals. Allowed options: acorn, acornInjectPlugins, cache, context, experimentalCacheExpiry, external, inlineDynamicImports, input, manualChunks, moduleContext, onwarn, perf, plugins, preserveEntrySignatures, preserveModules, preserveSymlinks, shimMissingExports, strictDeprecations, treeshake, watch</font>
>

<font style="color:rgb(51, 51, 51);">提示我们globals选项不存在，我们需要将globals放入output中才可以使用。</font>

<font style="color:rgb(51, 51, 51);">于是我就按照文档，添加了一个globals选项，同时使用external选项来排除打包。</font>

<font style="color:rgb(51, 51, 51);">当时代码是这样的：</font>

```javascript
external: ["vue"],
output: {
    globals: {
        vue: "Vue",
    },
},
```

<font style="color:rgb(51, 51, 51);">添加完成之后，问题就开始出现了：</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(246, 246, 246);">rollup Failed to resolve module specifier "vue"</font>

<font style="color:rgb(51, 51, 51);">查看打包后的代码可以发现，我们的import代码还是原样import了vue，但是这种语法浏览器其实是解析不了了，因为网页没有node_modules，浏览器也就没有办法去别处找这个包。</font>

<font style="color:rgb(51, 51, 51);">就是因为这个问题，前前后后搞了半天，终于在谷歌搜到了一个rollup用户的issue，其中的用户和我具有一样的情况，也就是globals选项无效。</font>

<font style="color:rgb(51, 51, 51);">参考下方地址：</font>

[<font style="color:rgb(51, 51, 51);">https://github.com/rollup/rollup/issues/3188</font>](https://github.com/rollup/rollup/issues/3188)

<font style="color:rgb(51, 51, 51);">然后经过一番研究，我才找到了最终的问题（当然我之前也有思考过是不是不支持，但是用其他的插件也是无效）：</font>

> <font style="color:rgb(85, 85, 85);background-color:rgb(246, 246, 246);">output.globals only sets the global variable for external imports in IIFE bundles (and UMD bundles when they are used in a script tag), in other formats it has no effect. https://www.npmjs.com/package/rollup-plugin-external-globals is probably what you want.</font>
>
> <font style="color:rgb(85, 85, 85);background-color:rgb(246, 246, 246);">Ah sorry, I see you found that plugin, is it working for you? There are technical reasons this is not supported in core, mostly because it would need to work very differently for non-IIFE formats, potentially provide larger output after minification, and it is totally unclear how to handle this with UMD.</font>
>

<font style="color:rgb(51, 51, 51);">翻译过来，就是说自带的global只支持iife或者umd打包的库文件，这时候我才恍然大悟，我使用的包都是commonjs打包的（关于各种打包的方式使用大家可以谷歌学习），所以才出现这种问题，其实就是本身不支持（不过经过测试其他方式的好像也不可以，可能就是vite本身的问题吧）。</font>

<font style="color:rgb(51, 51, 51);">反正经过这个提示，问题已经大概解决了，我们只需要使用插件就能解决这个问题。</font>

## 使用rollup-plugin-external-globals插件来解决问题  

<font style="color:rgb(51, 51, 51);">参考：</font>[<font style="color:rgb(51, 51, 51);">https://github.com/rollup/rollup/issues/2374</font>](https://github.com/rollup/rollup/issues/2374)

<font style="color:rgb(51, 51, 51);">插件地址：</font>[<font style="color:rgb(51, 51, 51);">https://www.npmjs.com/package/rollup-plugin-external-globals</font>](https://www.npmjs.com/package/rollup-plugin-external-globals)

### 安装插件  

```bash
yarn add -D rollup-plugin-external-globals
```

### 添加配置  

<font style="color:rgb(51, 51, 51);">使用方法与上方定义方法几乎相同，传入参数给插件初始化方法就行。  
</font>

<font style="color:black;background-color:rgb(207, 207, 207);"></font>

```javascript
plugins: [
        commonjs(),
        externalGlobals({
          vue: "Vue",
          "ant-design-vue": "antd",
          moment: "moment",
        }),
      ],
```

<font style="color:rgb(51, 51, 51);">参数对解释：</font>

+ <font style="color:rgb(51, 51, 51);">vue - 这里需要和external对应，这个字符串就是(import xxx from aaa)中的aaa，也就是包的名字</font>
+ <font style="color:rgb(51, 51, 51);">Vue - 这个是js文件导出的全局变量的名字，比如说vue就是Vue，查看源码或者参考作者文档可以获得  
</font>

<font style="color:rgb(51, 51, 51);">下面是全部vite.config.js，供参考：</font>

```javascript
import vue from "@vitejs/plugin-vue";
import commonjs from "rollup-plugin-commonjs";
import externalGlobals from "rollup-plugin-external-globals";

/**
 * https://vitejs.dev/config/
 * @type {import('vite').UserConfig}
 */
export default {
  plugins: [vue()],
  build: {
    rollupOptions: {
      external: ["vue", "ant-design-vue", "moment"],
      plugins: [
        commonjs(),
        externalGlobals({
          vue: "Vue",
          "ant-design-vue": "antd",
          moment: "moment",
        }),
      ],
    },
  },
};
```

### 在index.html中导入静态文件
<font style="color:rgb(51, 51, 51);">修改根目录下的index.html，添加cdn文件：  
</font>

<font style="color:black;background-color:rgb(207, 207, 207);"></font>

```plain
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ant-design-vue@2.0.0-rc.9/dist/antd.min.css">
  <script src="https://cdn.jsdelivr.net/npm/vue@3.0.5/dist/vue.global.prod.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/moment@2.29.1/moment.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/ant-design-vue@2.0.0-rc.9/dist/antd.js"></script>
```

<font style="color:rgb(51, 51, 51);">整个文件参考下方：</font>

<font style="color:black;background-color:rgb(207, 207, 207);"></font>

```html
<!DOCTYPE html>
<html lang="en">

  <head>
    <meta charset="UTF-8" />
    <link rel="icon" href="/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ant-design-vue@2.0.0-rc.9/dist/antd.min.css">
    <script src="https://cdn.jsdelivr.net/npm/vue@3.0.5/dist/vue.global.prod.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/moment@2.29.1/moment.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/ant-design-vue@2.0.0-rc.9/dist/antd.js"></script>
    <title>大伟聊前端</title>
  </head>

  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>

</html>
```

### 编译测试
<font style="color:rgb(51, 51, 51);">vite中提供了preview，可以供我们预览编译后的结果</font>

```bash
yarn run build
yarn run serve
```

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1727425068770-ec85c2cb-3c0e-499f-a1b9-acd7c1104423.jpeg)

<font style="color:rgb(51, 51, 51);">打开网页发现原先报错已经不存在，问题解决。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/207857/1727425066955-19a95698-c020-4a75-a47e-9f2319f4432d.jpeg)

