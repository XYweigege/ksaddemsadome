<br/>color4
<font style="color:rgb(44, 62, 80);">在服务器上搭建了 Jenkins，实现前端 Vue 项目的自动打包并部署至服务器上</font>

<br/>

## 通过 docker 安装 Jenkins
<font style="color:rgb(44, 62, 80);">通过 ssh 连接上局域网服务器 192.168.36.2，在 home 目录下新建了一个 Jenkins 文件夹，后续我们的配置文件就放在其中。</font>

```bash
cd
# 将 Jenkins 相关的文件都放在这里
mkdir jenkins
cd jenkins

# 创建 Jenkins 配置文件存放的地址，并赋予权限
mkdir jenkins_home
chmod -R 777 jenkins_home

pwd
# /root/jenkins
```



<font style="color:rgb(44, 62, 80);">创建</font>`docker-compose.yml`<font style="color:rgb(44, 62, 80);">：</font>

```bash
touch docker-compose.yml
vim docker-compose.yml
```

```yaml
version: '3'
services:
  jenkins:
    image: jenkins/jenkins:latest
    container_name: 'jenkins'
    restart: always
    ports:
      - "8999:8080"
    volumes:
      - /root/jenkins/jenkins_home:/var/jenkins_home
```



<font style="color:rgb(44, 62, 80);">Jenkins 启动后会挂在</font>`8080`<font style="color:rgb(44, 62, 80);">端口上，本文笔者将其映射到</font>`8999`<font style="color:rgb(44, 62, 80);">端口，读者可以自行更改。</font>

<font style="color:rgb(44, 62, 80);">关键在于将容器中的</font>`/var/jenkins_home`<font style="color:rgb(44, 62, 80);">目录映射到宿主机的</font>`/root/jenkins/jenkins_home`<font style="color:rgb(44, 62, 80);">目录，这一步相当于将 Jenkins 的所有配置都存放在宿主机而不是容器中，这样做的好处在于，后续容器升级、删除、崩溃等情况下，不需要再重新配置 Jenkins。</font>

<font style="color:rgb(44, 62, 80);">使用</font>`:wq`<font style="color:rgb(44, 62, 80);">保存后可以开始构建了：</font>

```bash
docker compose up -d
```

<font style="color:rgb(44, 62, 80);">这一步会构建容器并启动，看到如下信息就说明成功了：</font>

```bash
[+] Running 1/1
 ✔ Container  Jenkins   Started           1.3s
```

<font style="color:rgb(44, 62, 80);">查看一下容器是否在运行：</font>

```bash
docker ps
```

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729795106-3d4245b8-1abf-4ef3-9c28-25c2dc257df9.png)

image-20240403133238265

<font style="color:rgb(44, 62, 80);">这个时候通过</font>`http://192.168.36.2:8999`<font style="color:rgb(44, 62, 80);">就可以访问 Jenkins 了。</font>

## Jenkins 初次配置向导
#### 解锁
![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729795141-436c8306-8820-40be-b143-c2f90adace93.png)

image-20240403133538015

<font style="color:rgb(44, 62, 80);">第一次打开会出现向导，需要填入管理员密码，获取密码有三种方式：</font>

1. <font style="color:rgb(44, 62, 80);">通过宿主机</font>

```bash
cat /root/jenkins/jenkins_home/secrets/initialAdminPassword

# 2bf4ca040f624716befd5ea137b70560
```

2. <font style="color:rgb(44, 62, 80);">通过 docker 进入容器</font>

```bash
docker exec -it jenkins /bin/bash

#进入了docker
jenkins@1c151dfc2482:/$ cat /var/jenkins_home/secrets/initialAdminPassword

# 2bf4ca040f624716befd5ea137b70560
```

3. <font style="color:rgb(44, 62, 80);">通过查看 docker log</font>

```bash
docker logs jenkins
```

<font style="color:rgb(44, 62, 80);"></font>

<font style="color:rgb(44, 62, 80);">会出现一大串，最后能找到密码：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736730038421-e0b03fa2-ed14-41d6-9f3f-d7b44f7dee0e.png)



<font style="color:rgb(44, 62, 80);">填入密码，点击继续。</font>

#### 安装插件
![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729801845-e17be4c2-b7fb-43d5-8a26-ed619bb53145.png)

image-20240403134122512

<font style="color:rgb(44, 62, 80);">选择安装推荐插件即可。</font>

<font style="color:rgb(44, 62, 80);">安装插件可能会非常慢，可以选择换源。</font>

#### 更换 Jenkins 插件源（可选）
<font style="color:rgb(44, 62, 80);">有两种方法：</font>

1. <font style="color:rgb(44, 62, 80);">直接输入地址：</font>

`http://192.168.36.2:8999/manage/pluginManager/advanced`<font style="color:rgb(44, 62, 80);">，在</font>`Update Site`<font style="color:rgb(44, 62, 80);">中填入清华源地址：</font>

```bash
https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
```

<font style="color:rgb(44, 62, 80);">点击</font>`<font style="color:rgb(44, 62, 80);">Submit</font>`<font style="color:rgb(44, 62, 80);">提交保存，并重启容器。</font>

2. <font style="color:rgb(44, 62, 80);">直接更改配置文件：</font>

<font style="color:rgb(44, 62, 80);">宿主机中操作：</font>

```bash
cd /root/jenkins/jenkins_home
vim hudson.model.UpdateCenter.xml
```

<font style="color:rgb(44, 62, 80);">替换其中的地址，然后重启容器即可。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736730121619-be2930f4-d202-44b7-ba06-a26dbda5c348.png)

#### 创建用户
<font style="color:rgb(44, 62, 80);">这一步建议用户名不为 admin ，不然会出现奇怪的问题，比如密码登录不上，需要用上一部的初始密码（2bf4ca040f624716befd5ea137b70560）才能登录。</font>

<font style="color:rgb(44, 62, 80);">我这里创建了一个 root 用户（只是名字叫 root，防止用户名太多记不住而已）。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729797512-c435f328-04e6-4ea2-a34b-7de8fe80e246.png)

image-20240403135909136

<font style="color:rgb(44, 62, 80);">点击保存并完成。</font>

<font style="color:rgb(44, 62, 80);">实例配置按需调整即可，直接下一步，Jenkins 就准备就绪了。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729804849-0dc323ca-94ca-4ce7-9f95-b0e98321284b.png)

image-20240403140101678

<font style="color:rgb(44, 62, 80);">至此 Jenkins 安装就算完成了。</font>

## 安装插件
<font style="color:rgb(44, 62, 80);">以前端项目为例。</font>

<font style="color:rgb(44, 62, 80);">前端项目的打包需要 node 环境，打包完成后通过 ssh 部署到服务器上，并且构建结果通过钉钉机器人推送到群里，因此需要三个插件。</font>

1. <font style="color:rgb(44, 62, 80);">NodeJS</font>
2. <font style="color:rgb(44, 62, 80);">Publish Over SSH</font>
3. <font style="color:rgb(44, 62, 80);">DingTalk（可选）</font>

<font style="color:rgb(44, 62, 80);">在 系统管理 -> 插件管理 -> Available plugins 中搜索并安装。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729798423-8fb4aa8f-f198-4501-ba06-e5c0722ac534.png)



![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729798450-26210be2-861e-40f9-a4ef-1893e46dd786.png)



<font style="color:rgb(44, 62, 80);">勾选安装后重启，让插件生效。</font>

## 插件配置
<font style="color:rgb(44, 62, 80);">我们安装了三个插件，分别进行配置。</font>

### NodeJS
<font style="color:rgb(44, 62, 80);">这个插件可以在不同的项目中使用不同的 node 环境，例如 A项目 使用 node14，B项目 使用 node20 这样。</font>

<font style="color:rgb(44, 62, 80);">进入 系统管理 -> 全局工具配置 -> NodeJS 安装 （在最下面）</font>

<font style="color:rgb(44, 62, 80);">点击新增：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729799475-9e583461-a4c5-4a10-88eb-b0bad73c2a18.png)



<font style="color:rgb(44, 62, 80);">默认的这个使用的是</font><font style="color:rgb(44, 62, 80);"> </font>[<font style="color:rgb(44, 62, 80);">nodejs.org</font>](http://nodejs.org/)<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的官方源，虽然现在</font><font style="color:rgb(44, 62, 80);"> </font>[<font style="color:rgb(44, 62, 80);">nodejs.org</font>](http://nodejs.org/)<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的官方源国内访问也还可以，但为了保险起见，笔者还是换成阿里巴巴源。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729801520-d677ff1d-3cba-4f38-bfc2-761367ad002b.png)



<font style="color:rgb(44, 62, 80);">点击红框里的 X 删除当前安装，在点击新增安装，选择</font><font style="color:rgb(44, 62, 80);"> </font>`Install from nodejs.org mirror`<font style="color:rgb(44, 62, 80);">。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729801391-7af761b8-5f84-42e8-b862-aeff9e4e9d16.png)

image-20240403142605321

<font style="color:rgb(44, 62, 80);">镜像地址填入</font>`https://mirrors.aliyun.com/nodejs-release/`<font style="color:rgb(44, 62, 80);">，版本按需选择，笔者这里选择的是 node20-lts，并且安装了包管理工具 pnpm，如果读者的项目需要别的全局安装的包，也可以写在</font><font style="color:rgb(44, 62, 80);"> </font>`Global npm packages to install`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">，比如</font><font style="color:rgb(44, 62, 80);"> </font>`yarn`<font style="color:rgb(44, 62, 80);">、</font>`cnpm`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">之类的。</font>

<font style="color:rgb(44, 62, 80);">记得起一个别名：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729802635-c15ff0a5-09e8-44d5-b7d5-9440f4c493fd.png)

image-20240403153355639

<font style="color:rgb(44, 62, 80);">配置好后点击保存。</font>

<font style="color:rgb(44, 62, 80);">一般来说，在使用 npm 时，需要更改 npm 的源，同样在 Jenkins 中也是可以的。</font>

<font style="color:rgb(44, 62, 80);">安装完 NodeJS 插件后，系统设置中会多一项</font><font style="color:rgb(44, 62, 80);"> </font>`Managed files`

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729803918-948bffe6-abb9-46d0-9770-355b27aea340.png)

image-20240403143048480

<font style="color:rgb(44, 62, 80);">进入后选择左侧的</font>`Add a new Config`<font style="color:rgb(44, 62, 80);">，然后选择</font><font style="color:rgb(44, 62, 80);"> </font>`Npm config file`<font style="color:rgb(44, 62, 80);">，然后点击</font><font style="color:rgb(44, 62, 80);"> </font>`Next`<font style="color:rgb(44, 62, 80);">。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729804116-887db90f-e3a9-444d-aab0-0cb35f96c9f8.png)

image-20240403143327739

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729811859-57eacf80-9695-4ffe-b33a-1041d55cc316.png)

image-20240403143449389

<font style="color:rgb(44, 62, 80);">新增一个</font><font style="color:rgb(44, 62, 80);"> </font>`NPM Registry`<font style="color:rgb(44, 62, 80);">，填入阿里巴巴镜像源：</font>`http://registry.npmmirror.com`<font style="color:rgb(44, 62, 80);">。</font>

<font style="color:rgb(44, 62, 80);">至此 NodeJS 相关的配置就完成了。</font>

### SSH Server
<font style="color:rgb(44, 62, 80);">打包后需要通过 SSH 部署到服务器上，因此需要先配置好 SSH 服务器。</font>

<font style="color:rgb(44, 62, 80);">打开 系统管理 -> 系统配置 -> Publish over SSH （在最下面）:</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729804897-b95f88f3-4355-4ec4-a166-06c141e94c05.png)

image-20240403143805027

<font style="color:rgb(44, 62, 80);">然后根据实际情况进行填写：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729807114-d447bd15-bfd2-4098-8cbc-8f79011e4ce5.png)

image-20240403144201158

| **<font style="color:rgb(44, 62, 80);">字段</font>** | **<font style="color:rgb(44, 62, 80);">解释</font>** |
| :--- | :--- |
| <font style="color:rgb(44, 62, 80);">Name</font> | <font style="color:rgb(44, 62, 80);">显示在 Jenkins 中的名称，可随意填写</font> |
| <font style="color:rgb(44, 62, 80);">Hostname</font> | <font style="color:rgb(44, 62, 80);">服务器地址，ip 或 域名</font> |
| <font style="color:rgb(44, 62, 80);">Username</font> | <font style="color:rgb(44, 62, 80);">SSH 登录的用户名</font> |
| <font style="color:rgb(44, 62, 80);">Remote Directory</font> | <font style="color:rgb(44, 62, 80);">SSH 登录后进入的目录，必须是服务器中已经存在的目录，设置好之后所有通过 SSH 上传的文件只能放在这个目录下</font> |


<font style="color:rgb(44, 62, 80);">这里笔者使用用户名-密码的方式登录 SSH，如果要通过 SSH Key 的方式的话，需要在字段</font><font style="color:rgb(44, 62, 80);"> </font>`Path to key`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">填入 key 文件的地址，或者直接将 key 的内容填入</font><font style="color:rgb(44, 62, 80);"> </font>`Key`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">字段中:</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729807125-6632b251-4a3b-460d-8809-0c11722ce0f2.png)

image-20240403144737759

<font style="color:rgb(44, 62, 80);">设置好可以通过</font>`Test Configuration`<font style="color:rgb(44, 62, 80);">，测试 SSH 连通性：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729806984-0d0a196d-9c1a-47a4-af19-79bd20dbe466.png)

image-20240403144822057

<font style="color:rgb(44, 62, 80);">出现</font><font style="color:rgb(44, 62, 80);"> </font>`Success`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">代表 SSH 配置成功。</font>

### 钉钉通知（可选）
<font style="color:rgb(44, 62, 80);">如果不需要通过钉钉通知，可以不装 DingTalk 插件，并跳过本节内容。</font>

#### 钉钉部分设置
<font style="color:rgb(44, 62, 80);">该功能需要一个钉钉群，并打开钉钉群机器人：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729807210-585193f1-7431-4727-8e9d-a60ce329ebc3.png)

image-20240403145500789

<font style="color:rgb(44, 62, 80);">点击添加机器人，选择自定义：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729808880-dca2e36b-b082-4662-99cc-a5f0164326a9.png)

image-20240403145604092

<font style="color:rgb(44, 62, 80);">这里笔者的安全设置选择了加签：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729809260-403f6981-ca87-4e95-8f85-f56eea87227f.png)

image-20240403145717147

<font style="color:rgb(44, 62, 80);">将签名保存下来备用。</font>

<font style="color:rgb(44, 62, 80);">点击完成后，出现了钉钉机器人的 Webhook 地址。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729809829-da767be8-f8f4-4bd6-b055-5716186d1bd1.png)

image-20240403145823192

<font style="color:rgb(44, 62, 80);">将地址保存下来备用。</font>

<font style="color:rgb(44, 62, 80);">至此钉钉部分的设置就结束了。</font>

#### Jenkins 部分
<font style="color:rgb(44, 62, 80);">打开 系统设置 -> 钉钉 （在最下面的未分类中）:</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729809484-497594ca-4559-4a58-b92a-85c12262c57e.png)

image-20240403145150439

<font style="color:rgb(44, 62, 80);">根据需要配置通知时机：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729811582-69430ce6-2f3c-4a50-889e-4b8e51517f27.png)

image-20240403145231249

<font style="color:rgb(44, 62, 80);">然后点击机器人-新增：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729811503-5e67a028-b955-4d7c-8015-45ca2d05194a.png)

image-20240403145303034

<font style="color:rgb(44, 62, 80);">将刚刚的钉钉机器人的签名和 Webhook 地址填入对应的地方，并点击测试：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729812024-8c571365-10b7-4003-81ee-4d8fad82a69e.png)

image-20240403150049799

<font style="color:rgb(44, 62, 80);">此时钉钉机器人也在群中发了消息：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729812087-d763a135-72e7-4b80-9eb1-cd312117dd9c.png)

image-20240403150138516

<font style="color:rgb(44, 62, 80);">至此钉钉机器人配置完毕。</font>

## 创建任务（job）
<font style="color:rgb(44, 62, 80);">本文中，笔者将以存储在 Git 仓库中的项目为例。</font>

### Github 项目
**<font style="color:rgb(44, 62, 80);">注意，如果想让 Github 项目全自动构建的话，需要你的 Jenkins 能被公网访问到，例如部署在云服务器上，像笔者这样部署在本地局域网中，是无法实现“提交代码 -> 自动构建 -> 自动部署”的，只能实现“提交代码 -> 手动点击开始构建 -> 自动部署”</font>**

<font style="color:rgb(44, 62, 80);">如果在 Jenkins 新手向导里选择了 安装推荐插件，那么现在就不需要额外安装 Github 相关的插件了，否则的话需要手动安装 Github 相关的插件：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729813669-6ddaeb2a-9a58-4279-a4ea-7f391cbc5362.png)

image-20240403151242880

#### 创建项目
<font style="color:rgb(44, 62, 80);">选择 Dashboard -> 新建任务：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729812954-2219825d-4827-40ab-ae0d-5fb6e4db3d51.png)

image-20240403151424735

<font style="color:rgb(44, 62, 80);">选择</font>`构建一个自由风格的软件项目`<font style="color:rgb(44, 62, 80);">，点击确定。</font>

#### General
<font style="color:rgb(44, 62, 80);">这部分可以添加钉钉机器人：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729814254-37ca79be-7d70-40c8-b144-7046873aefc9.png)

image-20240403151545166

#### 源码管理
<font style="color:rgb(44, 62, 80);">这里选择 Git：</font>

<font style="color:rgb(44, 62, 80);">输入仓库地址：</font>`https://github.com/baIder/homepage.git`

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729814381-f73d25a7-31b3-4522-bd65-a4031405a5c8.png)

image-20240403151822101

<font style="color:rgb(44, 62, 80);">由于这是一个私有仓库，因此会报错。</font>

<font style="color:rgb(44, 62, 80);">在下面的</font>`Credentials`<font style="color:rgb(44, 62, 80);">中，添加一个。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729814492-0d739814-9c45-4ac6-a3da-62d31ab103e7.png)

image-20240403151941812

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729815981-50fa6c2d-e123-4f58-bb93-46ba59a28c0c.png)

image-20240403152135370

**<font style="color:rgb(44, 62, 80);">注意，这里的用户名是 Github 用户名，但是密码不是你的 Github 密码，而是你的 Github Access Token！！！</font>**

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729815768-cd7b8985-0f96-4294-bc0f-a1e786cb7bc4.png)

image-20240403152324115

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729816400-38110caa-ec71-46e4-a308-19edeac7af90.png)

image-20240403152429183

<font style="color:rgb(44, 62, 80);">可以在这里创建 Token，需要勾选</font><font style="color:rgb(44, 62, 80);"> </font>`admin:repo_hook`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">、</font>`repo`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">权限。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729816785-3a7b4237-8c53-4935-b1f5-fbc9e916654a.png)

image-20240403152535951

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729816784-6ebbadef-7d4c-4031-b578-9de8ab403412.png)

image-20240403152729685

<font style="color:rgb(44, 62, 80);">这里的报错是网络问题，连接 Github 懂得都懂。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729817673-c70591b0-a053-4afc-8c90-0d7c19d4e026.png)

image-20240403152824725

<font style="color:rgb(44, 62, 80);">分支可以根据实际情况选择。</font>

#### 构建触发器
<font style="color:rgb(44, 62, 80);">勾选</font>`GitHub hook trigger for GITScm polling`<font style="color:rgb(44, 62, 80);">，这样在 Git 仓库产生提交时，就会触发构建，属于是真正的核心。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729818122-1a142e2d-4082-4f96-91df-775e02d01c13.png)

image-20240403153134664

#### 构建环境
<font style="color:rgb(44, 62, 80);">勾选</font><font style="color:rgb(44, 62, 80);"> </font>`Provide Node & npm bin/ folder to Path`

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729818653-4c952212-7fdb-4ffb-9383-e12a9cc90b57.png)

image-20240403153444910

#### Build Steps
<font style="color:rgb(44, 62, 80);">到这里，可以理解为 Jenkins 已经将仓库克隆到本地，并且已经安装好了</font>`node`<font style="color:rgb(44, 62, 80);">、</font>`npm`<font style="color:rgb(44, 62, 80);">、</font>`pnpm`<font style="color:rgb(44, 62, 80);">，接下来就是执行命令：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729818384-6598a100-c13a-4a54-af4f-e7ee91a68f47.png)

image-20240403153625159

<font style="color:rgb(44, 62, 80);">我们需要执行命令：</font>

```bash
node -v
pnpm -v

rm -rf node_modules
rm -rf dist

pnpm install
pnpm build
```

<font style="color:rgb(44, 62, 80);">这里的</font>`pnpm build`<font style="color:rgb(44, 62, 80);">需要按情况更换为</font>`package.json`<font style="color:rgb(44, 62, 80);">中设定的命令。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729818988-2dec1deb-061d-4546-abe9-89a34ddfb9ef.png)

image-20240403153850007

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729819691-fd5346ca-e4c7-4746-b84e-4be16f11b5e0.png)

image-20240403153750787

#### 构建后操作
<font style="color:rgb(44, 62, 80);">经过所有的流程到这里，项目应该已经打包在</font>`dist`<font style="color:rgb(44, 62, 80);">目录下了。现在可以通过 SSH 将打包好的产物上传到服务器上了：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729820129-5711b482-0bad-4b1b-a24f-67ced7fc590d.png)

image-20240403154044658

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729820442-cf6d6a38-e720-4c22-b978-2b9df5515ef5.png)

image-20240403155757484

<font style="color:rgb(44, 62, 80);">这里的</font><font style="color:rgb(44, 62, 80);"> </font>`Source files`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">字段一定要写成</font>`dist/**/**`<font style="color:rgb(44, 62, 80);">，如果写成</font>`dist/*`<font style="color:rgb(44, 62, 80);">，则只会将第一层的文件上传。</font>

`Remove prefix`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">需要填写，否则会将</font>`dist`<font style="color:rgb(44, 62, 80);">这个目录也上传到服务器上。</font>

`Remote directory`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是相对于配置 SSH Server 时的</font><font style="color:rgb(44, 62, 80);"> </font>`Remote directory`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">的，本例中就是</font><font style="color:rgb(44, 62, 80);"> </font>`/data/sites/homepage`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">。</font>

`Exec command`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">是文件上传后执行的命令，可以是任何命令，可以是让nginx有权限访问这些数据，重启nginx等等，根据服务器实际情况更改。</font>

当然也可以在 `Build Steps` 中 build 完成后将 dist 目录打包，然后在通过 SSH 将压缩包上传到服务器，然后在 `Exec command` 中解压。

<font style="color:rgb(44, 62, 80);">至此所有的配置已经完成，保存。</font>

#### 测试
<font style="color:rgb(44, 62, 80);">点击左侧的 立即构建：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729820639-0dd6274f-15be-440f-a503-d9368c22ccf4.png)

image-20240403154858929

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729820452-c1496172-143f-430e-a493-68c350e6d054.png)

image-20240403154950197

<font style="color:rgb(44, 62, 80);">第一次构建会比较慢，因为需要下载node，安装依赖等等，可以从控制台看到，命令都如期执行了：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729821868-6844a54d-6327-4dfa-9634-4b1a5c3186e9.png)

image-20240403155359524

<font style="color:rgb(44, 62, 80);">构建成功，钉钉机器人也提示了（因为 Github 访问失败的原因，多试了几次）：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729822257-8b7c27c3-fd75-4651-a646-ac071d7d7782.png)

image-20240403155855959

<font style="color:rgb(44, 62, 80);">笔者已经配置好了nginx，因此可以直接访问网页，查看效果：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729822740-59912161-92f2-4bd6-a7ce-176a043bf4c6.png)

image-20240403160008179

#### 通过 Git 提交触发构建
<font style="color:rgb(44, 62, 80);">目前虽然构建成功了，但是需要手动点击构建，接下来实现如何将代码提交 Git 后自动触发构建。</font>

<font style="color:rgb(44, 62, 80);">打开仓库设置 -> Webhooks 添加一个：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729822872-3b3417e2-9179-437f-a81d-0f0238050bc7.png)

image-20240403160353025

<font style="color:rgb(44, 62, 80);">这里的</font><font style="color:rgb(44, 62, 80);"> </font>`Payload URL`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">就是 Jenkins 地址 +</font><font style="color:rgb(44, 62, 80);"> </font>`/github-webhook`<font style="color:rgb(44, 62, 80);">，例如笔者的就如图所示。</font>

<font style="color:rgb(44, 62, 80);">但是由于笔者的 Jenkins 部署在本地局域网，因此是不行的，Github 肯定是无法访问到笔者的局域网的，有公网地址的读者可以试试，在笔者的阿里云服务器上是没有问题的。所以目前如果是 Github 项目的话，笔者需要提交代码后手动点击 立即构建：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729822829-58fad742-6a12-488e-8013-bd6633eb7c58.png)

image-20240403161026497

### Gitlab 项目
<font style="color:rgb(44, 62, 80);">实际上笔者所在公司是在局域网中部署了 Gitlab 的，因此针对 Gitlab 项目的自动化才是核心。</font>

<font style="color:rgb(44, 62, 80);">安装 Gitlab 插件：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729824043-8e975147-4f12-49ed-8091-2f9c522335a5.png)

image-20240403161442736

<font style="color:rgb(44, 62, 80);">安装完毕后重启 Jenkins。</font>

#### 获取 Gitlab token
<font style="color:rgb(44, 62, 80);">与 Github 的流程类似，也需要在 Gitlab 中创建一个 token：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729824219-0b85e4ef-500f-4b51-9c14-31a16e2c0732.png)

image-20240403161807711

<font style="color:rgb(44, 62, 80);">创建好之后保存 token 备用。</font>

#### 在 Jenkins 中配置 Gitlab
<font style="color:rgb(44, 62, 80);">打开 Jenkins -> 系统管理 -> 系统配置 -> Gitlab</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729825020-c2d84bde-aa6b-4303-b2c4-3ceb1e2948ea.png)

image-20240403162301361

<font style="color:rgb(44, 62, 80);">这里需要新建一个</font>`Credentials`<font style="color:rgb(44, 62, 80);">，点击下方的添加：</font>

<font style="color:rgb(44, 62, 80);">类型选择</font>`GitLab API token`<font style="color:rgb(44, 62, 80);">，将刚刚保存的 token 填入到</font><font style="color:rgb(44, 62, 80);"> </font>`API token`<font style="color:rgb(44, 62, 80);"> </font><font style="color:rgb(44, 62, 80);">字段中。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729824531-b86e2d31-4a75-4fbb-bcde-975bdc3b866f.png)

image-20240403162144399

<font style="color:rgb(44, 62, 80);">点击</font>`Test Connection`<font style="color:rgb(44, 62, 80);">：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729825579-b7414fe8-b8dd-46aa-9f60-7bc663195b88.png)

image-20240403162651637

<font style="color:rgb(44, 62, 80);">出现</font>`Success`<font style="color:rgb(44, 62, 80);">说明配置成功。</font>

#### 创建项目
<font style="color:rgb(44, 62, 80);">大多数过程与 Github 项目雷同。</font>

##### General
<font style="color:rgb(44, 62, 80);">会多出一个选项，选择刚刚添加的：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729826714-0dcadb8a-e9f5-4eba-8b9e-bc86d24a4f25.png)

image-20240403163406501

##### 源码管理
<font style="color:rgb(44, 62, 80);">Git 仓库地址填 Gitlab 仓库地址，同样会报错，添加一个</font>`Credentials`<font style="color:rgb(44, 62, 80);">便可解决：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729826641-abef8b0b-1e20-4575-a8ff-7d4ba1f407d2.png)

image-20240403163538105

<font style="color:rgb(44, 62, 80);">用户名密码填登录 Gitlab 的用户名密码即可。</font>

##### 构建触发器
<font style="color:rgb(44, 62, 80);">按需选择触发条件，这里笔者仅选择了提交代码：</font>

<font style="color:rgb(44, 62, 80);">这里红框中的 url 需要记下，后面要用。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729833914-8989f9aa-9e40-43af-b066-eb3bf8c9d270.png)

image-20240403164252265

#### 其他配置
<font style="color:rgb(44, 62, 80);">与 Github 项目相同。</font>

#### 测试构建
<font style="color:rgb(44, 62, 80);">点击立即构建，查看是否能构建成功：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729834311-01b97279-ef2b-43e5-a826-1f0941ad4682.png)

image-20240403163945821

<font style="color:rgb(44, 62, 80);">构建成功：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729827540-282941f3-cd83-4f32-876b-c0f2e298d3ca.png)

image-20240403164002293

#### 提交代码自动构建
<font style="color:rgb(44, 62, 80);">进入 Gitlab 仓库 -> 设置 -> 集成：</font>

<font style="color:rgb(44, 62, 80);">这里的 url 填入刚刚 Jenkins 构建触发器 中红框内的 url 地址。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729828597-32ef9612-859b-48ec-93dd-4f2638f36f67.png)

image-20240403164203308

<font style="color:rgb(44, 62, 80);">看情况是否开启 SSL verification。</font>

<font style="color:rgb(44, 62, 80);">点击 Add webhook：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729829121-b06db00f-56a9-444e-93e5-686d11876b19.png)

image-20240403164449921

<font style="color:rgb(44, 62, 80);">测试一下：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729829325-458ff182-cb16-4968-bbcd-7b1cf268f5a0.png)

image-20240403164513668

<font style="color:rgb(44, 62, 80);">可以看到 Jenkins 那边已经开始构建了：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729831047-f44be711-9a99-4c11-9af5-d0be9f04cfa3.png)

image-20240403164551282

<font style="color:rgb(44, 62, 80);">构建成功：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729830950-27203ff0-23fd-4c32-9f77-e23f3126d67e.png)

image-20240403164606737

#### 测试 Git 提交触发构建
<font style="color:rgb(44, 62, 80);">目前页面：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729831077-02a2eddc-3ab8-45d9-9450-0770a8635e48.png)

image-20240403164712339

<font style="color:rgb(44, 62, 80);">我们将</font>`v2.0-f`<font style="color:rgb(44, 62, 80);">改成</font>`v2.0-g`<font style="color:rgb(44, 62, 80);">：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729833061-5d641345-151a-4a21-9540-0dc8402010a2.png)

image-20240403164817371

<font style="color:rgb(44, 62, 80);">提交代码，Jenkins 开始了自动构建：</font>

![]()

image-20240403164852625

<font style="color:rgb(44, 62, 80);">构建成功，页面也发生了变化：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/207857/1736729840152-bee967e3-9f7b-4996-9f24-5851e14184dd.png)

image-20240403164912343

<font style="color:rgb(44, 62, 80);">至此，Gitlab 提交代码后自动打包并部署至服务器的流水线就完成了。</font>

## 后记
<font style="color:rgb(44, 62, 80);">本文实现了从提交代码到部署上线的自动化工作流，适合小公司的小型项目或自己的演示项目，大公司一定会有更规范更细节的流程 </font>

