苹果的官方文档非常详细并且还有客服支持，所以注册账号多以链接的形式做方向上的引导，细节需要小伙伴们自己去看文档。

硬件条件，需要准备一台 Mac 笔记本，一部 iphone 手机。

我写文时用的软件环境

+ Xcode 14.2
+ flutter 3.7.5

要注册开发者账号，首先要有一个普通 的 Apple Id。

## 创建 Apple Id
如何创建可以查阅官方文档 [如何创建新的 Apple ID](https://support.apple.com/zh-cn/108647)，写得很清楚

## 为 Apple Id 开启双重验证
首先点这里了解下 [双重验证](https://developer.apple.com/cn/support/authentication/)，普通用户是不需要开启双重验证的，只有在 Apple Developer Program、Apple Developer Enterprise Program 或 IOS Developer University Program 中具有“帐户持有人”职能 (旧称“团队代理”) 的开发者才需要启用双重认证。我们做为开发者，肯定是要开启双重验证的。

要想开发并发布 IOS App，必须要有开发者帐号，帐号是收费的，目前是 688 人民币每年。

## 注册开发者帐号
建议在 IOS 手机上下载 App Develop App 进行注册。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433480074-626333cb-c2c3-41e4-9e08-16eab9fb79e5.png)

点这个链接 [注册 developer](https://developer.apple.com/cn/support/app-account/)，按官方说的做就行，写得非常明白。如果实在有问题，还可以直接求助官方客服。在网页下面有联系我们，点获取支持

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433510551-1d04e7ba-b031-496f-bbba-77214d49bb8d.png)

常用的帐号类型是个人账号和公司账号。都是 688 人民币每年。

如果有实体公司的话，建议用公司账号。

真机调试，首先需要添加设备

## 添加设备
点 [这里](https://developer.apple.com/register/agree/?successURL=https://developer.apple.com/account/resources/devices/) 打开添加设备的页面。

![]()

点 **+** 号 新建就行了，需要 UDID（Device ID)。UDID 是 IOS 设备的一个唯一识别码，每台 IOS 设备都有一个独一无二的编码。

获取 UDID 一般用 [蒲公英](https://www.pgyer.com/tools/udid) 就行。用 safari 打开页面，会下载一个描述文件，安装这个描述文件就会获取 UDID。17 的手机的信息是这样的

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433724342-ee5ded9d-1602-42cb-aa06-78207ca0d3fe.png)

或者用 Xcode 。连接手机后，打开 Xcode ，打开 window 菜单下的 device and simulator

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433740468-df8e53a3-0c47-49d7-8824-cbefec9534a6.png)

那个 identifier 就是 UDID

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433753994-e3167b87-0f29-482d-9109-4a47df9d074b.png)

688 人民币每年的帐号，最多可以添加 100 台设备。设备可以随时 disable，enable，但都会占据这 100 个名额，直到下次续费才能有一个调整设备的机会。这个时候 disabled 设备会默认被删除，如果要继续保留，需要先把这个设备 enable。确认所有需要保留的设备都 enable 后，点 continue 按钮。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433763508-a814a45a-d930-452f-94e5-4800f1fc8558.png)

每年这个按钮只出现一次，调整完了想再调整只能等下一年了。

点按钮后会出现一个设备列表，把要保留的勾选后再点 continue 就行了。完成后，名额会被重置，删除的设备的名额会被释放出来。

把你的测试设备添加进来后，就可以开发了。

## 新建项目
```plain
flutter create --org=com.iam17 app17
```

这样设置后，app 的 Bundle ID 也定了，是 com.1am17.app17。Bundle ID 「Bundle identifier」也叫 App ID 或者应用 ID，是每一个 IOS 应用的唯一标识，就像一个人的身份证号码，android 中的 applicationID 也是这样的作用。

在开发之前，先把开发版签名配置好。

### 配置 debug 签名
我们用自动签名的方式。

```plain
cd app17/ios
open Runner.xcworkspace
```

或者你找到 Runner.xcworkspace，直接双击也行。点击左侧的 Runner，再点击右侧的 Signing & Capabilities，再选中左面的 debug 配置开发版签名。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433786676-745625b3-9665-4e1c-8bc7-9ac3efdf540f.png)

先把 Bundle Identifier 修改为 com.iam17.app17.debug 就是后面加一个 .debug，这样在手机上 debug 版的 app 和 release 版的 app 就可以共存了。

Automatically manage signing 保持勾选。选择下面的 team。如果没有可选的 team 可以选择 Add an Account 增加 team。

选择好 team 后，Xcode 会为我们配置好证书和描述文件。

用 vscode 打开项目，连上手机（注册过的)，command + shit + p 打开命令菜单，选择或输入命令 Flutter:Select Device，选中你的手机。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433795501-fcdbbdf8-1449-4b68-ad05-09a984d01b05.png)

打开 main.dart，按 F5 走起。会弹出一个窗口

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433812554-926fe010-5cde-428a-b84b-bab7ed0c2ed9.png)

这里要求输入的密码就是你本机 mac 的开机密码。连始终允许，过一会，大功告成！应用已经安装到 iphone 上了，是不是有点小激动？

在开发的过程中，可能会用到一些插件，这些插件可能会用到 IOS 的一些能力（capability)，我们需要申请一下。如果你没有用到插件，这一步可以跳过。

选 All，这样只可以同时为 debug，release 添加能力，点左边的 +Capability，会弹出能力列表供你选择。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433819785-77dedcfe-a537-466d-84e3-d187709d7205.png)

单击显示能力的介绍，双击添加能力，每次只能增加一个。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433828589-26045ba7-adb1-4027-9591-42441242d3bc.png)

添加能力后，可能还会让你添加额外的信息，比如 Associated Domains，还会让你添加 domain。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433834054-a2991cb2-d230-4142-93a6-aa671ded3dd8.png)

......

开发完成，准备发布。

### 配置 App Name
打开 ios/Runner/Info.plist

```vbnet
<key>CFBundleName</key> 
<string>你的app名称</string>
```

## 其它设置请参照上一篇
设置版本号，添加 launcher icon ，设置 Splash Screen 详见 [https://www.yuque.com/sohucw/hzbvu7/rm2seitdab9y7gf5](https://www.yuque.com/sohucw/hzbvu7/rm2seitdab9y7gf5)

添加 launcher icon ，设置 Splash Screen 都是用的插件，插件会同时完成 Android，IOS 的配置。

## 配置 release 签名
```plain
cd app17/ios
open Runner.xcworkspace
```

再次打开 Xcode，确保 Bundle Identifier 是 com.iam17.app17。 还是用自动签名的方式。在 release 选项选择好 team，剩下的交给 Xcode 处理。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433884711-3b878852-95f7-4611-982b-5dc7e194a7bd.png)

## 在苹果网站上新建 app
点 [这里](https://appstoreconnect.apple.com/apps) 打开 app 页面。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433905647-96572a1a-6a20-4ef9-a87b-e92155064430.png)

有两个选项，新建 App 和新建 App 套装。因为我们现在只有一个 App，所以选新建 App。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433927072-8f261b15-e7da-4d5c-83de-734ef86a8321.png)

SKU 是你 App 专有的 ID，此 ID 不会在 App Store 中显示。 SKU 和 App ID 一样就行。

App 新建成功会出现下面的界面，按要求把其它信息填写好。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433937265-fd36c015-8979-4296-9e5e-cfc383fc1721.png)

## 打包和发布
和之前一样，打开 Xcode

```plain
cd app17/ios
open Runner.xcworkspace
```

注意这里选择一个物理设备（不是模拟器），比如可以选择你开发用的 iphone。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433949357-32c99007-0c21-4637-b50e-ff2c54037cbc.png)

在 product 菜单中选择 archive 命令

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433956561-c171b725-ade2-4c5a-8599-3a4349afaacf.png)

在 archive 成功后弹出的窗口中选择 Distruibute App

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433967972-8ea00451-c59a-408d-a971-7ada6f303e80.png)

这个界面关掉后要再次打开，先找到 Window 菜单，然后选择 Organizer。

因为是要发到 App Store，所以选择第一个 App Store Connnect

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433975162-2d28a9ee-c09a-41d1-9f19-67294a04d226.png)

然后新弹窗有两个选项，upload 和 export。 upload 是上传，上传后在网站上可以看到，然后可以提交到 App Store 审核。另一个选项是导出到本地，我们先选第一项，上传。

最后看到下面的界面，说明 ipa 已经成功上传到 apple。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433982936-491f7cc6-1444-47cf-8edd-3b0064361e80.png)

打开 apple 网站上新建的 IAM17 app，过一段时间，你上传 app 会出现在这里。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433990290-722404d5-9626-4886-8672-9e1e2a83285e.png)

构建版本几个字的后面如果出现一个蓝色的 **+** 号，点进去看看，你的 ipa 会在那里。

然后就可以提交审核了。审核通过就可以发版了。

恭喜你，完成了 Flutter IOS 从 0 到 1 的跨越！撤花~

接下来的内容作为选读，有兴趣的同学可以了解下。

## 自动签名 Xcode 帮我们做了什么
### 生成 Bundle ID
点 [这里](https://developer.apple.com/account/resources/identifiers/list) 查看

Xcode 生成了两个 Bundle ID，一个用于开发，一个用于测试

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723434016316-d400dc79-ed95-470e-8cc9-21ed37749b70.png)

### 生成并安装证书
点 [这里](https://developer.apple.com/account/resources/certificates/list) 查看

Xcode 生成了证书

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723434037670-3ce5fdd3-9a46-4dc6-aef9-ec83ddf40ac7.png)

并且证书已经安装到了本地，找到 macOS 系统中的钥匙串访问并打开，可以看到已经安装的证书。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723434044685-b5658b42-bb1e-400d-807c-5c20437f3927.png)

### 生成描述文件并安装
执行下面的命令

```plain
ll ~/Library/MobileDevice/Provisioning Profiles
```

你会看到描述文件的信息，前面一串是描述文件的名字。不管原来的描述文件叫什么名字，安装后都会被重命名，名字就是这样子的。后面的 `.mobileprovision` 表明这是一个描述文件。

```plain
c21c9bdb-e6e9-41ec-b0c0-f6b658dca07f.mobileprovision
```

里面的描述文件都是 Xcode 生成并安装的。

## 手动签名如何做
手动签名其实就是把 Xcode 为我们做的事情手动做一遍。

## 申请 Bundle ID。
Bundle ID 「Bundle identifier」也叫 App ID 或者应用 ID，是每一个 IOS 应用的唯一标识，就像一个人的身份证号码，android 中的 applicationID 也是这样的作用。

1. 打开 [App Ids](https://developer.apple.com/account/resources/identifiers)
2. ![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435695500-4832fbca-2a2a-4f1b-9b8c-02445c1d578b.png)

点 **+** 后，会出现让你选 id 的类型。按默认选第一个就行。因为是首次申请，不用太多了解其它 id，将来用到的时候知道到这申请就行了。比如将来如果要推送消息，就需要在这里再申请 Website Push IDs

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435705216-a5330647-ed25-4f7d-a313-0ea5d559ca81.png)

点右边的 continue 会来到选择类型的页面

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435714763-86470240-2fb5-43ae-baf5-79b027b2288c.png)

选 App 就行。至于 App Clip ，IOS 14 才开始支持，还少有人用，先不用管它。

点右边的 continue，开始设置 Bundle Id。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435726081-c3601ff1-20c5-45d7-9e1a-2a80104923e3.png)

### 填写 Bundle ID
有两个选项，Explicit 和 Wildcard。区别在于能不能带 * 号。Wildcard 先不用管它，我们选择 Explicit。

Explicit 的官方提示： 我们建议使用反向域名样式字符串（即 com.domainname.appname）。它不能包含星号 (*)。

比如，可以输入 com.iam17.app17，左面的 Description 写明 Id 的用途等信息。

### 选择 Capabilities
接下来是选择 app 需要有哪些能力，选项很多。

Flutter 主要是做 UI，对于系统的能力，一般是由插件来完成的，所以只需要仔细阅读插件的说明，选择相关的能力就行了，不需要对所有能力都做深入了解。

没有用到的先不要开，这些能力后面如果需要是可以增加的。相比于删除能力，增加能力要好的多。

选择能力后，点右边的 register 按钮，就注册成功了。

### 配置证书
配置证书分两步，先在本机申请，然后上传到网站生成证书。

### 在 macOS 上请求证书
找到 macOS 系统中的钥匙串访问并打开，在左上角的菜单中依次选择【钥匙串访问】,【证书助理】，【从证书颁发机构请求证书...】来创建签名文件。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435743016-b8a327ac-4235-421b-9561-45fae938bdd3.png)

在弹出的窗口中填写证书的信息，其实只要填写一个邮件地址就行。注意下面选择存储到磁盘。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435749881-a6a3dda2-8806-4e1b-b661-b0de8275cbde.png)

最后得到这样一个文件 CertificateSigningRequest.certSigningRequest，先保存，接下来有用。

### 在网站上申请证书
点 [这里](https://developer.apple.com/account/resources/certificates/) 打开苹果开发者创建证书的页面。看左面的菜单，和申请 Bundle Id 是在一个地方。还有其它几个菜单，记住都到这里找就行了。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435770036-cfdcd907-ecb0-44a3-97fc-137012c690d2.png)

点那个蓝色的 **+** 创建就行了。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435777459-9777e3a2-6f5c-4f77-98bf-4250450e0515.png)

我们选择第二项 Apple Distribution，用于发布到 Apple Store。点 continue ，接下来选本地的文件证书。

点 Choose File，选之前创建的已经保存在本地的证书文件 CertificateSigningRequest.certSigningReques，点右边的 continue，证书就创建完成了。把它download 下来，双击就会案例到钥匙串中。或者你直接把证书拖动到钥匙串界面上也行。

### 配置描述文件
点 [这里](https://developer.apple.com/account/resources/profiles/) 申请描述文件。和上面的申请 Bundele Id，Identifier 和证书是在一个地方。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435805855-fb82bb85-fc26-40d4-96ab-43296254543e.png)

现在应该空的，因为 Xcode 为我们生成的描述文件直接装在本地了。 直接点那个蓝色的 **+** 号或下面的创建按钮。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435814106-0de51411-41c5-4d80-9be2-2d87a9488b2b.png)

选择 App Store 这一项，发布到 app Store。

接下来会让你选一个 App Id，就选前面创建的 com.iam17.app17 就行。

接下来会让你选证书，选完证书后，点右边的 continue，会让你给描述文件起一个名字。起完名字，就完成了。

### Xcode 配置手动签名
取消 Automatically manage signing

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435825015-7007c3c1-542a-45e9-9be0-192daa606a39.png)

这样就需要我们手动配置 Bundle Id，证书和描述文件。 provisioning Profile 也就是描述文件需要我们本地选取或从网上下载。如果要 import 需要把证书先下载到本地。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723435832305-fd56c6d7-15c1-4550-a2cf-faf61789315b.png)

选中描述文件后，可能会显示有错误，说不包含证书，只要你确认证书已经安装在本地，就不用管它，直接点 product 菜单下面的 archive 打包，一路下去就行了。

## IOS 签名原理
要理解 IOS 签名，先得理解两个算法 MD5，RSA。MD5 信息摘要算法，RSA 非对称加密。

如果只有 app store 这一个发 ipa 的地方，事件就简单了。苹果准备 好 公钥 M 和私钥 N，可以这样做：

1. 每台 iphone 上面都预装有公钥 M
2. app store 上的 ipa 的 md5 摘要经私钥 N 加密后连同 ipa 下发到 iphone
3. iphone 计算 ipa md5 得到摘要 A，iphone 用公钥对加密后的摘要经过解密得到摘要 B，如果 A B 一样，说明 ipa 是从 app store 下发的。

为什么不对 ipa 直接加密，因为 RSA 算法慢，不方便直接对大文件进行加密。

但是，除了 app store ，还有其它方式可以安装 ipa，供开发测试用。为此，苹果想出了双重加密算法。有兴趣的

可以参看这两篇

[图解iOS签名背后的原理](https://juejin.cn/post/7087975702288564238)

[深入理解iOS签名原理](https://juejin.cn/post/6844903989792735240)

 

 文章的定位是从 0 到 1 完成第一次打包，所以就要精选出最关键，最核心的部分。只要成功打包一次，后面再有需求，可以自行查阅文档了。第一次困难是因为没有方向感，处于完全陌生的领域，这个时候最需要的是一个指引。

 

 

  


 

