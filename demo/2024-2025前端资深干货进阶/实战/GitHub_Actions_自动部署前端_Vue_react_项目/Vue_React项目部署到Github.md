<br/>color1
这是一种更简单的部署方式

<br/>

## 1. 前提：你的代码库已经提交到Github上
<font style="color:rgb(51, 51, 51);">如果没有的话，请到GitHub上新建仓库，并把你本地的项目提交到GitHub上新建的仓库里。</font>

<font style="color:rgb(51, 51, 51);">一定的是public仓库，不能是private仓库</font>

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730877052153-6aa86fbd-b71d-4146-b803-35929010df81.png)<font style="color:rgb(51, 51, 51);">  
</font>

## 2. 在GitHub上设置部署配置
![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730876995373-89adf1dc-875c-412c-9453-c286660dbc77.png)

## 3. 到你的项目根目录创建工作流文件
<font style="color:rgb(51, 51, 51);">根目录下新建 </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.github</font>`<font style="color:rgb(51, 51, 51);"> 文件夹，然后在其目录下新建 </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">workflows</font>`<font style="color:rgb(51, 51, 51);"> 文件夹，在里面新建 </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">main.yml</font>`<font style="color:rgb(51, 51, 51);"> 。  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730876751139-6c995c76-a677-4940-8517-6974afb797ce.png)

`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">main.yml</font>`<font style="color:rgb(51, 51, 51);"> 里的内容如下：  (使用pnpm构建的配置文件)</font>

```yaml
# 将静态内容部署到 GitHub Pages 的简易工作流程
name: Deploy static content to Pages
 
on:
  # 仅在推送到默认分支时运行。
  push:
    branches: ['main']
 
  # 这个选项可以使你手动在 Action tab 页面触发工作流
  workflow_dispatch:      
 
# 设置 GITHUB_TOKEN 的权限，以允许部署到 GitHub Pages。
permissions:
  contents: read
  pages: write
  id-token: write
 
# 允许一个并发的部署
concurrency:
  group: 'pages'
  cancel-in-progress: true
 
jobs:
  # 单次部署的工作描述
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Set up pnpm
        uses: pnpm/action-setup@v4
        with:
          version: 9
      - name: Set up Node
        uses: actions/setup-node@v3
        with:
          node-version: 20
          cache: 'pnpm'
      - name: Install dependencies
        run: pnpm install
      - name: Build
        run: pnpm run build        
      - name: Setup Pages
        uses: actions/configure-pages@v3
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          # Upload dist repository
          path: './dist'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```

<font style="color:rgb(51, 51, 51);">其中我们需要修改的内容：</font>

1. <font style="color:rgb(51, 51, 51);">node版本，根据你的项目实际使用版本设置</font>
2. <font style="color:rgb(51, 51, 51);">打包目录，一般都是</font><font style="color:rgb(51, 51, 51);"> </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">dist</font>`<font style="color:rgb(51, 51, 51);">，如果不是的话请修改</font>
3. <font style="color:rgb(51, 51, 51);">项目脚本，此项目是使用pnpm构建，如果你使用的是npm等，需要手动修改。下面给出npm的设置：</font>

```yaml
# 将静态内容部署到 GitHub Pages 的简易工作流程
name: Deploy static content to Pages
 
on:
  # 仅在推送到默认分支时运行。
  push:
    branches: ['main']
 
  # 这个选项可以使你手动在 Action tab 页面触发工作流
  workflow_dispatch:      
 
# 设置 GITHUB_TOKEN 的权限，以允许部署到 GitHub Pages。
permissions:
  contents: read
  pages: write
  id-token: write
 
# 允许一个并发的部署
concurrency:
  group: 'pages'
  cancel-in-progress: true
 
jobs:
  # 单次部署的工作描述
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Set up Node
        uses: actions/setup-node@v3
        with:
          node-version: 20
          cache: 'npm'
      - name: Install dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Setup Pages
        uses: actions/configure-pages@v3
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          # Upload dist repository
          path: './dist'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```

<font style="color:rgb(51, 51, 51);">npm的版本也根据你的实际使用而定，后面的内容都一样。其他的请自行查找。</font>

## 4. 修改你的项目部署根目录
<font style="color:rgb(51, 51, 51);">正常情况下一般都在</font>**<font style="color:rgb(51, 51, 51);"> vite.config.ts 或 vue.config.js 或 webpack.config.js</font>**<font style="color:rgb(51, 51, 51);"> 里，取决于你使用了哪种脚手架。  
</font><font style="color:rgb(51, 51, 51);">以 vite.config.ts 为例，存在 base 字段中  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730948334783-88422b32-0ad2-45ad-9274-6035c4652dac.png)

<font style="color:rgb(51, 51, 51);">如果你有封装的话，如上图，可能是一个变量，一般都在根目录的 </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">.env.production</font>`<font style="color:rgb(51, 51, 51);"> 文件中，修改此变量的值即可。如下图：  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730878271838-269d5a64-b3b3-49f6-9946-faad8b84e33a.png)

## 5. 提交代码，你的项目即可自动部署
<font style="color:rgb(51, 51, 51);">或者到GitHub网站仓库目录，在 </font>`<font style="color:rgb(192, 52, 29);background-color:rgb(251, 229, 225);">Actions</font>`<font style="color:rgb(51, 51, 51);"> 页签中，手动部署  
</font>![](https://cdn.nlark.com/yuque/0/2024/png/207857/1730878125975-556b37ba-00b5-48c3-8ba0-019f8e1246b7.png)

## 6. 访问路径
<font style="color:rgb(51, 51, 51);">访问路径：[github账号名称].github.io/[仓库名称]  
</font><font style="color:rgb(51, 51, 51);">例如：</font>[https://sohucw.github.io/vue-map-echarts](https://sohucw.github.io/vue-map-echarts)

<font style="color:rgb(51, 51, 51);">实例项目代码参考</font>[vue-map-echarts](https://github.com/sohucw/vue-map-echarts)

