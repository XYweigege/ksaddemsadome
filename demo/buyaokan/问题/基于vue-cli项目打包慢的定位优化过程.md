

<font style="color:rgb(51, 51, 51);">基于vue-cli的项目打包超级慢，我接手项目的时候，</font>**<font style="color:rgb(51, 51, 51);">打包需要5min。</font>**<font style="color:rgb(51, 51, 51);">为了解决这个问题，</font>**<font style="color:rgb(51, 51, 51);">以前没有做过这方面的优化，在结合网上的一些优化博客，</font>**<font style="color:rgb(51, 51, 51);">就开启了以下的优化之路。</font>

**<font style="color:rgb(51, 51, 51);">一、怀疑vue-cli打包配置文件被修改过</font>****<font style="color:rgb(51, 51, 51);">（</font>****<font style="color:rgb(255, 0, 0);">不行</font>****<font style="color:rgb(51, 51, 51);">）</font>**

<font style="color:rgb(51, 51, 51);">基于此点的怀疑，使用vue-cli重新搭建环境。</font>

**<font style="color:rgb(0, 204, 255);">步骤一：vue</font>****<font style="color:rgb(0, 204, 255);"> </font>****<font style="color:rgb(0, 0, 255);">init webpack 项目名称。</font>**

**<font style="color:rgb(0, 0, 255);">步骤二：把以前的代码一步步移植到新的开发环境，期间发现以前的代码很多不符合eslint规则的代码，需要一步步的修改，超级无语。</font>**

**<font style="color:rgb(0, 0, 255);">步骤三：npm run build --report，发现打包时间依旧没有下降，还是2个半小时，瞬间泪奔。打包分析如下图。</font>**

**<font style="color:rgb(0, 0, 255);">步骤四：此怀疑可以跳过了。</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728378187936-bed568b9-2dae-4dd9-9c9f-cd8c21fcb75d.png)

**<font style="color:rgb(51, 51, 51);">二、经过第一步，发现有一些打包的文件非常大，如Element UI没有按需加载，cityData.js等都是非常大的，由此怀疑是不是该问题导致打包时间长</font>****<font style="color:rgb(51, 51, 51);">（</font>****<font style="color:rgb(255, 0, 0);">不行</font>****<font style="color:rgb(51, 51, 51);">）</font>**

**<font style="color:rgb(0, 204, 255);">步骤一：按需加载Element UI组件</font>**

**<font style="color:rgb(0, 204, 255);">步骤二：查看cityData.js的内容，发现是一些省市区的数据，前期的数据都是通过这个文件来获取省市区数据，后来改为通过后台获取的了，上个离职人员忘记把代码注释掉了</font>**

**<font style="color:rgb(0, 204, 255);">步骤三：</font>****<font style="color:rgb(0, 0, 255);">npm run build --report，发现打包时间依旧没有下降，还是半小时，打包分析如下图。确实打包的体积小了一点，算是有一点优化了吧。。。</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728378187969-551f0009-2b37-4ada-9b0f-9ecd5fc28011.png)

<font style="color:rgb(0, 204, 255);">步骤四：此怀疑可以去掉了。</font>

**<font style="color:rgb(0, 0, 0);">三、结合网上的优化方式，进行了以下的优化方式，效果都不是不好。（</font>****<font style="color:rgb(255, 0, 0);">不行</font>****<font style="color:rgb(0, 0, 0);">）</font>**

<font style="color:rgb(0, 204, 255);">方式1：不生成.map文件，修改config/index.js文件中的productionSourceMap属性值为false</font>

<font style="color:rgb(0, 204, 255);">方式2：resolve.alias 配置路径别名</font>

<font style="color:rgb(0, 204, 255);">方式3：babel-loader优化，排除node_modules模块，准确获取src的目录，并且开启缓存</font>

<font style="color:rgb(0, 204, 255);">方式4：使用webpack-parallel-uglify-plugin多线程压缩JS</font>

<font style="color:rgb(0, 204, 255);">方式5：使用HappyPack多进程进行loader处理</font>

<font style="color:rgb(0, 204, 255);">上述5中方式不知道怎么配置看这个了：</font>[https://www.jb51.net/article/137449.htm](https://www.jb51.net/article/137449.htm)

**<font style="color:rgb(51, 51, 51);">四、使用DllPlugin和DllReferencePlugin处理，效果很明显，由原来的20个半小时变为了3分钟 （</font>****<font style="color:rgb(255, 0, 0);">该方法可行</font>****<font style="color:rgb(51, 51, 51);">）</font>**

参考这个：[https://segmentfault.com/a/1190000011795931](https://segmentfault.com/a/1190000011795931)

**<font style="color:rgb(51, 51, 51);">五、在完成第四步之后，以为优化已经完成了，准备收工的时候，突然想到了该项目的router在开发环境的时候不进行路由懒加载，在打包生产包的时候使用路由懒加载的，会不会是该问题导致的呢？</font>**

**<font style="color:rgb(51, 51, 51);">所以重新改写了router/index.js文件的懒加载方式，无论是开发环境还是生产环境都是使用懒加载方式。</font>**

**<font style="color:rgb(51, 51, 51);">原来的加载路由的方式：</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728378188224-0c586e12-91fe-411b-a022-043c22fcfd4f.png)

**<font style="color:rgb(51, 51, 51);">_import_development.js文件：本地开发的时候，不使用懒加载的方式，而是使用Common.js的方式直接引入，这样热加载就会很快完成的。</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728378187972-0fdce0c4-9727-4ca6-8ca1-6214e9fa9013.png)

**<font style="color:rgb(51, 51, 51);">_import_production.js文件：</font>**

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728378187955-8a5e5b42-6c89-4057-8849-7a531feb00e7.png)

**<font style="color:rgb(51, 51, 51);">修改之后，不使用_import_development.js和_import_production.js文件，而是直接在router/index.js中直接使用 () => import('@/components/xxx')进行路由懒加载。</font>**

<font style="color:rgb(51, 51, 51);"></font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1728378188458-c63a0776-0a4d-4fb2-aa15-8c393973262d.png)

<font style="color:rgb(255, 0, 0);">在重新打包的时候，发现竟然只要1分钟左右就完成了打包任务，但是本地开发的时候，热加载的时间由原来的5s变为了22s。坑爹的，因小失大啊。</font>

**<font style="color:rgb(0, 0, 0);">六、结论</font>**

**<font style="color:rgb(0, 0, 0);">抛弃原来的路由加载方式，使用第五步的路由懒加载方式只会，可以结合第二步、第三步、第四步进行打包优化，这样也会加快打包速度以及减少打包体积。</font>**

