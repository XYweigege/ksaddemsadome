### 代码地址：
[https://github.com/sohucw/Vite5-Vue3-base](https://github.com/sohucw/Vite5-Vue3-base)

### 一. 概述
<br/>color1
如果要部署一个项目大体要经过：代码开发、代码推送、打包dist文件、scp到服务器、服务器nginx配置、完成部署这几个流程，现实中我们希望项目部署尽可能自动且简单，因此诞生了各种CI/CD工具，比如：Jenkins、gitlab ci、gitlab runner， 我们最熟悉的 GitHub 也提供了CI/CD 的能力：GitHub Actions, 支持多种的语言和框架：**Node.js, Python, Java, PHP, Ruby, Go, Rust, C/C++, .NET, Android, iOS**.当然在利用GitHub Actions自动部署项目之前，先要利用GitHub Pages来发布我们的前端项目。

<br/>

### 二. GitHub Pages
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720662950991-580e1349-1468-4bc7-96cf-2f712384b382.png)

什么是 GitHub Pages？  可以利用它,将我们托管在 GitHub 仓库的项目部署为一个可以对外访问的网站，免去了我们自己购买与配置服务器的麻烦。

+ **首先创建一个项目**，以Vue项目为例，利用 Vue 脚手架创建一个项目

```bash
pnpm init vue@latest
```

这里假设你已经熟悉了 Vue 项目创建，如果不熟悉Vue可以去[查看](https://link.juejin.cn?target=https%3A%2F%2Fcn.vuejs.org%2Fguide%2Fquick-start.html%23creating-a-vue-application) 执行如下命令：

```bash
cd <your-project-name>
> pnpm install
> pnpm run dev
```

运行后在浏览器中打开本地地址，得到如下页面：

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720662640626-78c153d7-61ae-428c-ae5e-e2b47fad93c1.png)

+ **在****GitHub****上创建一个新的****Repository****,将项目上传到****GitHub****仓库**

```bash
git init
git add .
git commit -m "备注信息"
git remote add origin 你的远程仓库地址
git push -u origin master
```

+ **配置 GitHub Actions** 回到GitHub,点击Setting->Pages，看到如下界面

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663120752-8d1e9fde-d090-41a0-8c35-f91b41086562.png)

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663158411-4edf3074-2bf3-4ce0-9af3-a420bf04b523.png)

+  需要我们去新建一个名为**gh-pages**的分支，创建完成后再次打开Pages，可以看到页面发生了变化
+ ![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663187384-f3f245a7-7702-4fb4-a12c-bf627ed262e7.png)

**Source**: 选择Deploy from a branch, **Branch**：github pages 默认只能识别项目根目录的 index 文件，我们这里选择新建的gh-pages的root根目录，意思是去这个分支的根目录加载index.html文件.

**打包应用，并发布到 gh-pages 分支** 打包应用，执行npm run build ，在项目根目录下得到打包后的产物dist文件夹,

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663236141-4ddc08a1-da88-4204-8fed-e27a781ef5d8.png)

切换当前分支到gh-pages,并且将原有内容全部删除, 最后将dist文件夹下的内容全部拷贝到gh-pages上，push到远端.

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663283401-506cb7c9-5bf4-462a-84e4-b73ecdd1734f.png)

再次点击Setting->Pages ,稍等一会儿，下面出现了一个网址,这就是项目线上地址

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663343427-65773e4f-be7d-46bf-af0a-1f6fc49b6b66.png)

+ **遇到问题** 点击查看网址，并没有像我们预期的那样展示页面，而是一片空白。打开调试版查看错误信息：
+ ![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663359925-ced9260d-eca8-4079-8065-a9b0713f3bbf.png)

如果有项目部署经验的一看就知道是怎么回事了，这是打包编译后的文件路径配置有问题，资源文件css、js，加载的路径地址不对，加载的是根路径 https://<用户名>.github.io/assets/index.bf782a5b.js,而我们的资源文件在/vue-pages/目录下，所以当然报错404，修复也很简单，如果你的Vue项目是基于 Vite 的构建的，需要修改vite.config.js，添加base:'./'

```javascript
export default defineConfig({
  plugins: [vue(), vueJsx()],
  base:'./',// 将根路径换成相对路径
  resolve: {
    alias: {
      "@": fileURLToPath(new URL("./src", import.meta.url)),
    },
  },
})
```

如果是基于webpack构建，修改vue.config.js添加publicPath: './'.

```javascript
module.exports = {
  /**
   * publicPath 默认是 / 是根路径，这个是指服务的根路径：https://xxx.github.io/，发布后会从这个路径下找 js.css 等资源，
   而生成的网站路径是这个 Vite5-Vue3-base显然是找不到的
   * 我们需要修改为 相对路径'./' 或是‘.’ 或是 直接设置的项目子路径 :/项目名称/ 就可找到资源了
   */
  publicPath: './',
  outputDir: 'dist', // dist
  assetsDir: 'static',
  lintOnSave: process.env.NODE_ENV === 'development',
  productionSourceMap: false,
  ...
```

重新打包，将dist文件夹下内容拷贝到gh-pages分支下，并重新打开pages链接：https://<用户名>.github.io/Vite5-Vue3-base/ 成功部署！

每一次修改后都要重新打包，切换分支拷贝dist文件夹，实属麻烦，能不能让GitHub自动检测push动作,自动进行打包部署吗？那就是GitHub Actions的工作了.

### 三. GitHub Actions
#### 什么是GitHub Actions?
GitHub Actions是GitHub推出的一款持续集成**（CI/CD）**服务，它给我们提供了虚拟的服务器资源，让我们可以基于它完成自动化测试、集成、部署等操作。这里简单介绍一下它的几个基本概念，更多内容可以去官网[查看](https://github.com/features/actions)

#### 基本概念
+ Workflows（工作流程） 持续集成的运行过程称为一次工作流程，也就是我们项目开始自动化部署到部署结束的这一段过程可以称为工作流程.
+ job （任务） 一个工作流程中包含多个任务，简单来说就是一次自动部署的过程需要完成一个或多个任务.
+ step（步骤） 部署项目需要按照一个一个的步骤来进行，每个job由多个step构成.
+ action（动作） 每个步骤step可以包含一个或多个动作，比如我们在一个步骤中执行打包命令这个Action.

#### 语法简介
+ **name**name字段是workflow的名称。如果省略该字段，默认为当前workflow的文件名.

```bash
name: GitHub CI
```

+ **on**on字段指定触发workflow的条件，通常是某些事件,比如代码推送push,拉取pull_request,可以是事件的数组.

```bash
on: push
or
on: [push, pull_request]
```

指定触发事件时，可以限定分支或标签:

```bash
on:
  push:
    branches:    
      - master
```

上面代码表示：只有master分支发生push事件时，才会触发workflow.

+ **jobs**workflow的核心就是jobs，任务job放在jobs这个集合下，每一个job都有job_id，用job_id标识一个具体任务
+ jobs.<job_id>.name 任务说明

```bash
jobs:
  my_first_job: // job_id
    name: My first job 
  my_second_job:// job_id
    name: My second job
```

上面的jobs字段包含两项任务，job_id分别是my_first_job和my_second_job。

+ jobs.<job_id>.runs-onruns-on字段指定运行所需要的虚拟机环境,它是必填字段。

```bash
runs-on: ubuntu-18.04
```

GitHub Actions给我们提提供的运行环境主要有以下几种： **ubuntu-latest**，**ubuntu-18.04或ubuntu-16.04****windows-latest，windows-2019或windows-2016****macOS-latest或macOS-10.14**

+ jobs.<job_id>.steps 任务步骤，一个job可以包含多个步骤，我们需要分为多个步骤来完成这个任务，每个步骤包含下面三个字段：

```yaml
jobs.<job_id>.steps.name：步骤名称。
jobs.<job_id>.steps.run：该步骤运行的命令或者 action。
jobs.<job_id>.steps.env：该步骤所需的环境变量。
```

#### 使用介绍
+ 新建.yml文件 点击主页Actions -> New workflow -> set up a workflow yourself，当然你也可以选择一个模板，点击start commit则会自动在我们项目目录下新建.github/workflows/main.yml文件.
+ ![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663601479-d0f094d8-147b-4c0e-b64c-2c060e36ef3d.png)

整个workflow的核心就是yml脚本的书写。**<font style="color:#DF2A3F;">如果你需要某个action，不必自己写复杂的脚本，直接引用他人写好的 action即可</font>**，整个持续集成过程，就变成了一个actions的组合，你可以在GitHub的[官方市场](https://github.com/marketplace?type=actions)，可以搜索到他人提交的actions. 下面是我们要自动发布GitHub pages所写的脚本：

```yaml
name: CI Github Pages
on:
  #监听push操作
  push:
    branches:
      - master # 这里只配置了master分支，所以只有推送master分支才会触发以下任务
jobs:
  # 任务ID
  build-and-deploy:
    # 运行环境
    runs-on: ubuntu-latest
    # 步骤
    steps:
      # 官方action，将代码拉取到虚拟机
      - name: Checkout  ️ 
        uses: actions/checkout@v3

      - name: Install and Build   # 安装依赖、打包，如果提前已打包好无需这一步
        run: |
          npm install
          npm run build

      - name: Deploy   # 部署
        uses: JamesIves/github-pages-deploy-action@v4.3.3
        with:
          branch: gh-pages # 部署后提交到那个分支
          folder: dist # 这里填打包好的目录名称
```

上面整个workflow的说明：

<br/>color1
+ 只有当main分支有新的push推送时候才会执行整个workflow.
+ 整个workflow只有一个job,job_id是build-and-deploy,name被省略.
+ job 有三个step： 第一步是Checkout,获取源码，使用的action是GitHub官方的actions/checkout.
+ 第二步：Install and Build,执行了两条命令：npm install,npm run build,分别安装依赖与打包应用.
+ 第三步：Deploy 部署，使用的第三方action：JamesIves/github-pages-deploy-action@v4.3.3,它有两个参数：分别是branch、folder，更多关于这个action的详情可以去[查看](https://github.com/marketplace/actions/deploy-to-github-pages).

<br/>



### 四. 设置Custom domain
其实经过上面的三步已经可以实现自动部署的目的了，但是还是有点瑕疵。我们部署后的项目地址是：https://<用户名>.github.io/vue-pages/,域名还是GitHub的,能不能改成我们自己的专属域名呢？比如改成http://<用户名>.com/,那就需要设置Custom domain了。

#### 1. 购买域名
如果想将项目地址改成自己的专属域名，首先需要你去购买一个域名，目前**<font style="color:#DF2A3F;">阿里云,腾讯云</font>**都支持域名的购买，搜索自己喜欢的域名直接付款就好了。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663892285-2ea3a7f6-18ae-4b28-9e26-e85771301145.png)

#### 2. 购买域名后，还需要我们进行实名认证以及备案，按照平台的提示进行操作就好了，这里不再涉及.
#### 3. 进行DNS解析配置
这里以阿里云为例，打开域名解析控制台，点击解析按钮

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720664160132-43187c65-6aee-421c-ba96-05f975399a3a.png)

 点击添加记录按钮，将下面两种类型的记录值添加上，记录类型是：CNAME，记录值就是你GitHub的主域名.

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663921939-e016ff5c-c89f-446d-88b1-11ae072fb227.png)

#### 4. 设置Custom domain
返回到项目的GitHub pages设置页面，将我们购买的域名添加在Custom domain中，点击save，可以看到pages的地址变成了我们自己的域名，访问它就会看到你的网站了.

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1720663930567-0d38c407-e46b-4acb-af5b-5f4cd11832bc.png)

### 五. 代码地址：
[https://github.com/sohucw/Vite5-Vue3-base](https://github.com/sohucw/Vite5-Vue3-base)

   
 

