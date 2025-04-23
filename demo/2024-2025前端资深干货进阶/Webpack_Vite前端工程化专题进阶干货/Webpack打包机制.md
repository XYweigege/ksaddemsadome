
<font style="color:rgb(199, 37, 78);">webpack</font><font style="color:rgb(85, 85, 85);">是一个打包模块化 </font><font style="color:rgb(199, 37, 78);">JavaScript</font><font style="color:rgb(85, 85, 85);"> 的工具，在 </font><font style="color:rgb(199, 37, 78);">webpack</font><font style="color:rgb(85, 85, 85);">里一切文件皆模块，通过 </font><font style="color:rgb(199, 37, 78);">Loader</font><font style="color:rgb(85, 85, 85);"> 转换文件，通过 </font><font style="color:rgb(199, 37, 78);">Plugin</font><font style="color:rgb(85, 85, 85);"> 注入钩子，最后输出由多个模块组合成的文件。</font><font style="color:rgb(199, 37, 78);">webpack</font><font style="color:rgb(85, 85, 85);">专注于构建模块化项目。</font>

<br/>

### [](https://www.123fe.net/principle-docs/webpack/01-Webpack4%E6%89%93%E5%8C%85%E6%9C%BA%E5%88%B6%E5%8E%9F%E7%90%86%E8%A7%A3%E6%9E%90.html#%E7%AE%80%E5%8D%95%E7%89%88%E6%89%93%E5%8C%85%E6%A8%A1%E5%9E%8B%E6%AD%A5%E9%AA%A4)<font style="color:rgb(44, 62, 80);">简单版打包模型步骤</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">我们先从简单的入手看，当 webpack 的配置只有一个出口时，不考虑分包的情况，其实我们只得到了一个bundle.js的文件，这个文件里包含了我们所有用到的js模块，可以直接被加载执行。那么，我可以分析一下它的打包思路，大概有以下4步：</font>

+ <font style="color:rgb(44, 62, 80);">利用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">babel</font><font style="color:rgb(44, 62, 80);">完成代码转换及解析,并生成单个文件的依赖模块</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Map</font>
+ <font style="color:rgb(44, 62, 80);">从入口开始递归分析，并生成整个项目的依赖图谱</font>
+ <font style="color:rgb(44, 62, 80);">将各个引用模块打包为一个立即执行函数</font>
+ <font style="color:rgb(44, 62, 80);">将最终的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bundle</font><font style="color:rgb(44, 62, 80);">文件写入</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bundle.js</font><font style="color:rgb(44, 62, 80);">中</font>

### [](https://www.123fe.net/principle-docs/webpack/01-Webpack4%E6%89%93%E5%8C%85%E6%9C%BA%E5%88%B6%E5%8E%9F%E7%90%86%E8%A7%A3%E6%9E%90.html#%E5%8D%95%E4%B8%AA%E6%96%87%E4%BB%B6%E7%9A%84%E4%BE%9D%E8%B5%96%E6%A8%A1%E5%9D%97map)<font style="color:rgb(44, 62, 80);">单个文件的依赖模块Map</font>
+ <font style="color:rgb(44, 62, 80);">我们会可以使用这几个包：</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@babel/parser</font><font style="color:rgb(44, 62, 80);">：负责将代码解析为抽象语法树</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@babel/traverse</font><font style="color:rgb(44, 62, 80);">：遍历抽象语法树的工具，我们可以在语法树中解析特定的节点，然后做一些操作，如</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ImportDeclaratio</font><font style="color:rgb(44, 62, 80);">n获取通过</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">import</font><font style="color:rgb(44, 62, 80);">引入的模块,</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">FunctionDeclaration</font><font style="color:rgb(44, 62, 80);">获取函数</font>
    - <font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">@babel/core</font><font style="color:rgb(44, 62, 80);">：代码转换，如ES6的代码转为ES5的模式</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">由这几个模块的作用，其实已经可以推断出应该怎样获取单个文件的依赖模块了，转为</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ast</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">->遍历</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">Ast</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">->调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">ImportDeclaration</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">。代码如下</font>

```javascript
// exportDependencies.js
const fs = require('fs')
const path = require('path')
const parser = require('@babel/parser')
const traverse = require('@babel/traverse').default
const babel = require('@babel/core')

const exportDependencies = (filename)=>{
  const content = fs.readFileSync(filename,'utf-8')
  // 转为Ast
  const ast = parser.parse(content, {
    sourceType : 'module'//babel官方规定必须加这个参数，不然无法识别ES Module
  })

  const dependencies = {}
  //遍历AST抽象语法树
  traverse(ast, {
    //调用ImportDeclaration获取通过import引入的模块
    ImportDeclaration({node}){
      const dirname = path.dirname(filename)
      const newFile = './' + path.join(dirname, node.source.value)
      //保存所依赖的模块
      dependencies[node.source.value] = newFile
    }
  })
  //通过@babel/core和@babel/preset-env进行代码的转换
  const {code} = babel.transformFromAst(ast, null, {
    presets: ["@babel/preset-env"]
  })
  return{
    filename,//该文件名
    dependencies,//该文件所依赖的模块集合(键值对存储)
    code//转换后的代码
  }
}
module.exports = exportDependencies
```

<font style="color:rgb(44, 62, 80);">可以跑一个例子:</font>

```javascript
//info.js
const a = 1
export a
// index.js
import info from'./info.js'
console.log(info)

//testExport.js
const exportDependencies = require('./exportDependencies')
console.log(exportDependencies('./src/index.js'))
```

**<font style="color:rgb(44, 62, 80);">单个文件的依赖模块Map</font>**

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">有了获取单个文件依赖的基础，我们就可以在这基础上，进一步得出整个项目的模块依赖图谱了。首先，从入口开始计算，得到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">entryMap</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">，然后遍历</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">entryMap.dependencies</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">,取出其</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">value</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">(即依赖的模块的路径)，然后再获取这个依赖模块的依赖图谱，以此类推递归下去即可，代码如下：</font>

```javascript
const exportDependencies = require('./exportDependencies')

//entry为入口文件路径
const exportGraph = (entry)=>{
  const entryModule = exportDependencies(entry)
  const graphArray = [entryModule]
  for(let i = 0; i < graphArray.length; i++){
    const item = graphArray[i];
    //拿到文件所依赖的模块集合,dependencies的值参考exportDependencies
    const { dependencies } = item;
    for(let j in dependencies){
      graphArray.push(
        exportDependencies(dependencies[j])
      )//关键代码，目的是将入口模块及其所有相关的模块放入数组
    }
  }
  //接下来生成图谱
  const graph = {}
  graphArray.forEach(item => {
    graph[item.filename] = {
      dependencies: item.dependencies,
      code: item.code
    }
  })
  //可以看出，graph其实是 文件路径名：文件内容 的集合
  return graph
}
module.exports = exportGraph
```

### [](https://www.123fe.net/principle-docs/webpack/01-Webpack4%E6%89%93%E5%8C%85%E6%9C%BA%E5%88%B6%E5%8E%9F%E7%90%86%E8%A7%A3%E6%9E%90.html#%E8%BE%93%E5%87%BA%E7%AB%8B%E5%8D%B3%E6%89%A7%E8%A1%8C%E5%87%BD%E6%95%B0)<font style="color:rgb(44, 62, 80);">输出立即执行函数</font>
<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">首先，我们的代码被加载到页面中的时候，是需要立即执行的。所以输出的</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bundle.js</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">实质上要是一个立即执行函数。我们主要注意以下几点：</font>

+ <font style="color:rgb(44, 62, 80);">我们写模块的时候，用的是</font><font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">import/export</font><font style="color:rgb(44, 62, 80);">.经转换后,变成了</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">require/exports</font>
+ <font style="color:rgb(44, 62, 80);">我们要让</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">require/exports</font><font style="color:rgb(44, 62, 80);">能正常运行，那么我们得定义这两个东西，并加到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bundle.js</font><font style="color:rgb(44, 62, 80);">里</font>
+ <font style="color:rgb(44, 62, 80);">在依赖图谱里，代码都成了字符串。要执行，可以使用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">eval</font>

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">因此，我们要做这些工作：</font>

+ <font style="color:rgb(44, 62, 80);">定义一个</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">require</font><font style="color:rgb(44, 62, 80);">函数，</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">require</font><font style="color:rgb(44, 62, 80);">函数的本质是执行一个模块的代码，然后将相应变量挂载到</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">exports</font><font style="color:rgb(44, 62, 80);">对象上</font>
+ <font style="color:rgb(44, 62, 80);">获取整个项目的依赖图谱，从入口开始，调用</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">require</font><font style="color:rgb(44, 62, 80);">方法。完整代码如下：</font>

```javascript
const exportGraph = require('./exportGraph')
// 写入文件，可以用fs.writeFileSync等方法，写入到output.path中
const exportBundle = require('./exportBundle')

const exportCode = (entry)=>{
  //要先把对象转换为字符串，不然在下面的模板字符串中会默认调取对象的toString方法，参数变成[Object object]
  const graph = JSON.stringify(exportGraph(entry))
  exportBundle(`
        (function(graph) {
            //require函数的本质是执行一个模块的代码，然后将相应变量挂载到exports对象上
            function require(module) {
                //localRequire的本质是拿到依赖包的exports变量
                function localRequire(relativePath) {
                    return require(graph[module].dependencies[relativePath]);
                }
                var exports = {};
                (function(require, exports, code) {
                    eval(code);
                })(localRequire, exports, graph[module].code);
                return exports;
                //函数返回指向局部变量，形成闭包，exports变量在函数执行后不会被摧毁
            }
            require('${entry}')
        })(${graph})`)
}
module.exports = exportCode
```

<font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">至此，简单打包完成。贴一下我跑的demo的结果。</font><font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">bundle.js</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的文件内容为：</font>

```javascript
(function(graph) {
  //require函数的本质是执行一个模块的代码，然后将相应变量挂载到exports对象上
  function require(module) {
    //localRequire的本质是拿到依赖包的exports变量
    function localRequire(relativePath) {
      returnrequire(graph[module].dependencies[relativePath]);
    }
    var exports = {};
    (function(require, exports, code) {
      eval(code);
    })(localRequire, exports, graph[module].code);
    return exports;//函数返回指向局部变量，形成闭包，exports变量在函数执行后不会被摧毁
  }
  require('./src/index.js')
})({"./src/index.js":{"dependencies":{"./info.js":"./src/info.js"},"code":"\"use strict\";\n\nvar _info = _interopRequireDefault(require(\"./info.js\"));\n\nfunction _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { \"default\": obj }; }\n\nconsole.log(_info[\"default\"]);"},"./src/info.js":{"dependencies":{"./name.js":"./src/name.js"},"code":"\"use strict\";\n\nObject.defineProperty(exports, \"__esModule\", {\n  value: true\n});\nexports[\"default\"] = void 0;\n\nvar _name = require(\"./name.js\");\n\nvar info = \"\".concat(_name.name, \" is beautiful\");\nvar _default = info;\nexports[\"default\"] = _default;"},"./src/name.js":{"dependencies":{},"code":"\"use strict\";\n\nObject.defineProperty(exports, \"__esModule\", {\n  value: true\n});\nexports.name = void 0;\nvar name = 'winty';\nexports.name = name;"}})
```

### [](https://www.123fe.net/principle-docs/webpack/01-Webpack4%E6%89%93%E5%8C%85%E6%9C%BA%E5%88%B6%E5%8E%9F%E7%90%86%E8%A7%A3%E6%9E%90.html#webpack%E6%89%93%E5%8C%85%E6%B5%81%E7%A8%8B%E6%A6%82%E6%8B%AC)<font style="color:rgb(44, 62, 80);">webpack打包流程概括</font>
<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">webpack</font><font style="color:rgb(85, 85, 85);background-color:rgb(255, 249, 249);">的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：</font>

+ **<font style="color:rgb(44, 62, 80);">初始化参</font>**
+ **<font style="color:rgb(44, 62, 80);">开始编译</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">用上一步得到的参数初始Compiler对象，加载所有配置的插件，通 过执行对象的run方法开始执行编译</font>
+ **<font style="color:rgb(44, 62, 80);">确定入口</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">根据配置中的 Entry 找出所有入口文件</font>
+ **<font style="color:rgb(44, 62, 80);">编译模块</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">从入口文件出发，调用所有配置的 Loader 对模块进行编译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理</font>
+ **<font style="color:rgb(44, 62, 80);">完成模块编译</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在经过第4步使用 Loader 翻译完所有模块后， 得到了每个模块被编译后的最终内容及它们之间的依赖关系</font>
+ **<font style="color:rgb(44, 62, 80);">输出资源</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk,再将每个 Chunk 转换成一个单独的文件加入输出列表中，这是可以修改输出内容的最后机会</font>
+ **<font style="color:rgb(44, 62, 80);">输出完成</font>**<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">在确定好输出内容后，根据配置确定输出的路径和文件名，将文件的内容写入文件系统中。</font>


<font style="color:rgb(85, 85, 85);">在以上过程中， </font><font style="color:rgb(199, 37, 78);">Webpack</font><font style="color:rgb(85, 85, 85);"> 会在特定的时间点广播特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，井且插件可以调用 </font><font style="color:rgb(199, 37, 78);">Webpack</font><font style="color:rgb(85, 85, 85);"> 提供的 </font><font style="color:rgb(199, 37, 78);">API</font><font style="color:rgb(85, 85, 85);"> 改变 </font><font style="color:rgb(199, 37, 78);">Webpack</font><font style="color:rgb(85, 85, 85);"> 的运行结果。其实以上7个步骤，可以简单归纳为初始化、编译、输出，三个过程，而这个过程其实就是前面说的基本模型的扩展。</font>

<br/>

