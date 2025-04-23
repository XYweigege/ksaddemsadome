可能有些地方讲的还不是很清楚   android flavor 项目示例，完整代码 [在这里](https://github.com/sohucw/android-demo-publish)。

操作系统 Mac OS

+ Flutter 版本 3.7.5
+ android SDK 33

在打包之前，首先检查 pubspec.yaml 中的依赖，删除没有用到的依赖，减少包的大小。如果需要网络权限，需要在 android/app/src/main/AndroidManifest.xml 中添加网络权限申请。

```json
...
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
</manifest>
```

权限 INTERNET 允许网络连接。

权限 ACCESS_NETWORK_STATE 允许程序获取网络信息状态。如果用不到，这条权限可以不申请。

如果要用 http ，但是需要在 AndroidManifest.xml 的 applictation 加如下配置 `android:usesCleartextTraffic="true"`

下面详细说下打包的主要过程。

## 设置应用 ID 和包名
在建项目的时候可以直接设置应用 ID 和包名。

--org 指定 applicationId 的前缀，默认为 "com.example”，我们指定为 com.iam17，项目名为 app17。

```shell
flutter create --org=com.iam17  app17
```

创建成功后，项目的应用 ID(applicationId） 是 com.iam17.app17。包名（package) 也是 com.iam17.app17

默认情况下，项目的软件包名称与应用 ID 是一样的。

如果开始无法确定 org，到发版的时候才确定怎么办。 对于新手来说，与其做修改，不如直接新建项目，把 lib 下面的文件 copy 过来，并对 android 下面的内容做相应修改，这样肯定效率更高，也不容易出错。

### 应用 ID 和包名的区别
你一定会很奇怪，既然他们两个一样，为什么不用一个？

他们两个一个主外，一个主内。

应用 ID 主外，是 android app 的身份证，全球唯一。一般是把域名倒过来，加上你的 app 的名字。

包名主内，包名是 app 代码需要的。

+ 它将此名称用作应用生成的 `R.java` 类的命名空间。

示例：对于上面创建的项目，`R` 类将为 `com.iam17.app17.R`。

+ 它会使用此名称解析清单文件中声明的任何相关类名。

一个应用使用一个包名，但可以使用不同的应用 ID 发布为多个不同的应用，在应用市场上可以共存，这就是为什么要把应用 ID 和 包名分开的原因。

### 应用 ID 的位置
应用 ID 在gradle在 android/app/build.gradle 的 defaultConfig 中设置，新版本的 flutter 在新建项目的时候已经自动设置好了。

```dart
android {
  defaultConfig {
    applicationId = "com.iam17.app17"
      ...
```

如果没有设置 applicationId，为了与以前的 SDK 工具兼容，构建工具会将 `AndroidManifest.xml` 文件中的包名称用作应用 ID。在这种情况下，重构您的软件包名称也会更改您的应用 ID。

### 包名的位置
AndroidManifest.xml 文件中的 package 属性就是包名。

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.iam17.app17"
  ...
  >
```

package 轻易不要修改，修改起来比较麻烦。不是光修改这一个地方就完事了，还要调整目录结构，修改文件中的代码。

包名和应用 ID 的位置了解一下即可，一般不需要我们手动去修改。

## 修改版本号
打开 pubspec.yaml 找到` version: 1.0.0+1`。

可能有的小伙伴对版本号不知道为什么用 3 位，我来解释一下。

一般来讲大部分的软件版本号 分 3 段，比如 A.B.C

**A 表示大版本号**，一般当软件整体重写，或出现不向后兼容的改变时，增加A。

一般来说，第一版发布的时候是 1.0.0。

**B 表示功能更新**，出现新功能时增加B。

比如原来没有点赞功能，增加点赞功能的版本可以是 1.1.0。

有的时候，软件还在快速迭代中，很容易出现向前不能兼容的情况，这个时候，也可以直接用 B 来代替 A 的作用。比如发第一版的时候，用 0.1.0。等稳定后再发 1.0.0 版本。一般来说 A 是不应该经常变动的。

**C 表示小修改**，如修复bug，只要有修改就增加C

C 的更新可以不经测试，直接更新。如果功能要修改，也必须要兼容以前的版本，也就是说接口是不能修改的，只能修改逻辑实现。

还有后面的 +1 是怎么回事？ +号可以看做是分隔符。1 是累计版本，从 1 开始，只能增加，不能减小，并且，每次发版必须增加。这个版本号是给内部用的。比如苹果商店，android 平台都用这个版本号来判断软件的新旧。3 位版本号是给用户看的。3 位版本号包含软件更新的信息，让人一看就知道是重大更新，还是功能增加，还是 bug 修复。

## 消除 build.gradle 中的错误提示
打开 android/app/build.gradle 后，你可能会发现有一个错误提示：unable to resolve class GradleException。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723432668801-c240ac30-ad31-4e52-819d-7f5e04090cb3.png)

新建的项目可能也有这个问题，虽然不影响编译，但是有这个错误在总是心里不舒服。我们可以把 ~~GradleException~~ 换成 FileNotFoundException 错误提示就消失了。

## 设置 sdk 版本
在 android 下面有三个版本相关的配置 compileSdkVersion，minSdkVersion，targetSdkVersion，需要我们关注一下。

1. compileSdkVersion 告诉 gradle 用哪个 Android SDK 版本编译你的应用。compileSdkVersion 决定了你可以用哪个版本的 api。比如你的 compileSdkVersion 是 31，如果你在代码中用了 33 的 api，编译会报错。一般我们都把 compileSdkVersion 设置为最高版本。
2. minSdkVersion 是应用可以运行的最低版本要求。也就是你要向下兼容哪些版本。根据 app 的用户特点，尽量设得高一些，否则很多插件不能用。
3. targetSdkVersion 告诉 android 系统，app 要在哪个版本运行。android 的 api 在不同版本之间，虽然接口没有变化，但内部的行为可能发生了变化。当你指定了 targetSdkVersion 后，一定要在这个版本做完整的测试。新开项目 targetSdkVersion 应该和 compileSdkVersion 保持一致。支持新近的 API 级别有助于让您的应用利用平台的最新功能，为用户提供愉悦的体验。

说了半天，你可能最想知道，到底写什么？既然是新手第一次发包，那项目应该是新的。推荐这样设置。

```json
compileSdkVersion 33
minSdkVersion 23
targetSdkVersion 33
```

33 是当前 Android SDK 最新版本，如果你本机没有安装 SDK 33，需要先安装。

具体要如何修改呢？两个方案

1. 修改 flutter安装目录/packages/flutter_tools/gradle/flutter.gradle

```dart
lass FlutterExtension {
  static int compileSdkVersion = 33
    static int minSdkVersion = 16
    static int targetSdkVersion = 33
    ...
```

默认的 minSdkVersion 竟然是 16，一定得修改，这个确实太低了。注意，在这里修改会影响到所有项目。

1. 修改项目中的 android/app/build.gradle 比如修改 minSdkVersion，把变量换成常就行

```dart
defaultConfig {
  applicationId "com.iam17.app17"
    minSdkVersion flutter.minSdkVersion
    ...
```

修改为

```dart
defaultConfig {
  applicationId "com.iam17.app17"
    minSdkVersion 23
    ...
```

另外两个已经是最新了，就不用修改了。如果需要修改，修改为 33 就行。虽然有两种方案，但 17 推荐第 2 种，直接在项目中修改。

## 添加 launcher icon
launcher icon 就是下面图中的这些小方块。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723432713988-440303de-4dd9-4f22-99ae-fa5f25d48d80.png)

如果要手动设置 launcher icon 还是很麻烦的。幸运的是，这个问题被一个叫 flutter_launcher_icons 的插件解决了。所以原来那个超级麻烦的问题现在就转变为学会使用这个插件就行了。

1. 安装 flutter_launcher_icons 插件

```shell
 flutter pub add dev:flutter_launcher_icons
```

注意前面的 `dev:` 表示作为开发期间的依赖，flutter_launcher_icons 会出现在 dev_dependencies 下面。出现在这里的好处是等发版的时候 flutter_launcher_icons 不会被包括里 apk 中。

1. 准备一张 1024x1024 的图片。在根目录下建一个文件夹 images，把图片命名为 icon1024.png 放入 images 文件夹中。
2. 添加配置信息，打开 pubspec.yaml

```yaml
flutter_icons:
  image_path: "images/icon1024.png"
  android: true 
  ios: true
```

1. 执行创建命令

```shell
flutter pub run flutter_launcher_icons:main
```

1. 检查生成的图片

打开 android/app/src/main/res/drawable 文件夹查看已经生成好的图片，发现所有图片都已经自动生成好了。

为 ios 生成的图片路径 在 ios/Runner/Assets.xcassets/AppIcon.appiconset

### 注意事项
1. Format: 32-bit PNG
2. icon 大小必须是  1024x1024
3. 确保在 40px 大小时也能清晰可见，这是 icon 最小的尺寸。
4. icon 不能大于 1024KB
5. icon 不能是透明的。
6. 必须是正方形，不能有圆角。

icon 的边可能会被切掉，因为 Icon 最后展示的可能不是正方形，所以一般边上需要留白。

## 设置 App Name
App Name 就是 App 安装成功后在图标下面显示的名字。

打开 flutter根目录/android/app/src/main/AndroidManifest.xml，

```plain
<application
    android:label="App Name" > //在这修改 Android App Name。
```

## 设置 Splash Screen
虽然可以手动设置，但对于新手来说，用 flutter_native_splash 这个插件会更方便一些。 安装

```shell
flutter pub add flutter_native_splash
```

在项目根目录下找到文件夹 images，把 [splash.png](https://link.juejin.cn?target=https%3A%2F%2Fp3-juejin.byteimg.com%2Ftos-cn-i-k3u1fbpfcp%2F8b056d82ae2649f29976da1ac524d90b~tplv-k3u1fbpfcp-watermark.image%3F) 放入其中。

在 pubspec.yaml 的末尾加上配置

```plain
flutter_native_splash:
  color: "#42a5f5"
  image: images/splash.png
```

执行创建命令

```plain
flutter pub run flutter_native_splash:create
```

运行，第一时间就会看到启动画面了。splash.png 会居中显示。

说明

1. splash.png 要准备 4x 的。插件会自动为我们生成小分辨率的
2. 每次修改配置都得重新执行 `flutter pub run flutter_native_splash:create`
3. 不光是配置了 android 的启动页面，ios，web 的也一起生成好了。

有一个小福利。默认情况下当 flutter 开始绘制的时候， Splash Screen 就自动消失了。但这个时候，首屏的数据可能还没有准备好，还无法显示画面，我们可能还得准备一个 loading 的画面。现在你可以继续显示 Splash Screen，直到首屏数据准备完毕。

代码很好写的。我们用 Timer 来模拟数据准备。

```dart
void main() {
  WidgetsBinding widgetsBinding = WidgetsFlutterBinding.ensureInitialized();
  FlutterNativeSplash.preserve(widgetsBinding: widgetsBinding);
  runApp(const MyApp());

  Timer(Duration(seconds: 10), () {
    FlutterNativeSplash.remove();
  });
}
```

如果你哪天想自己配置可以恢复到默认状态

```plain
flutter pub run flutter_native_splash:remove
```

现在的启动页面只有一张图片，但一般在启动页面的底部还会有一个 品牌 logo。你可以把 logo 合并在主图上，但这样就无法保证 logo 出现在底部的位置。所以 logo 需要单独用一张图片。

在 pubspec.yaml 中增加 [branding](https://link.juejin.cn?target=https%3A%2F%2Fp1-juejin.byteimg.com%2Ftos-cn-i-k3u1fbpfcp%2Fda7b8c2185194492aa39e3434e04f0f1~tplv-k3u1fbpfcp-watermark.image%3F) 的配置。

```plain
flutter_native_splash:
  color: "#42a5f5"
  image: images/splash.png
  branding: images/logo.png
```

重新执行` flutter pub run flutter_native_splash:create` 就可以看到 logo 了。如果你觉得 logo 太靠下了，可以把 logo 的图片下面留点空白。

## 签名
要想把 app 发布到商店，必须要对 app 进行签名。签名有两种方式，命令式，和交互式。命令式对新手不大友好，我用交互的方式来说明。

在 IDE 中运行或调试项目时，AS 会自动使用 Android SDK 工具生成的调试证书为我们的应用签名，路径为 `$HOME/.android/debug.keystore`，但是应用商店可不接受使用调试证书发布的应用签名。为了给应用签名，需要先创建密钥。

### 创建密钥
打开 Android Studio ，找到 Build 菜单下面的 Generated Signed Bundle/APK

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723432810354-9d393bc4-e9b7-4170-9788-78d7d5ad1465.png)

会弹出下面这个对话框

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723432819718-a327e515-3557-414b-9512-d80968b5774e.png)

有两个选项

+ **Android App Bundle** 如果你要上传到 google 商店，选这个。你暂可以理解为 这是 APK 优化方案，可惜不是所有的商店都支持。
+ **APK** 就是传统的打包，为了兼容所有商痁，我们选这个就好了。

接下来又会弹窗

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723432827305-2a5ca16e-a748-4674-b1d2-f84d12da2129.png)

**Key store path**： 是你要将 key 保存在哪里，可以先找个地方放，后面可以 copy 到其它地方。应在位置路径末尾添加一个扩展名为 `.jks` 的文件名。点击 Create new 新建一个，比如文件名可以叫 key17.jks。

**key store passowrd**： 为您的密钥库创建并确认一个安全的密码。设置好了必须要记住。

+ **Alias**：为您的密钥输入一个标识名。 比如可以叫 key17。
+ **Password**：为您的密钥创建并确认一个安全的密码。它应该与密钥库密码相同。具体原因就不用管了，把它设成和 key store passowrd 一样就没问题了。密码要求至少 6 位。
+ **Validity (years)** ：以年为单位设置密钥的有效时长。密钥的有效期应至少为 25 年，以便您可以在应用的整个生命期内使用同一密钥为应用更新签名。我直接写 100 年，让它没有过期的可能。

**Certificate**： 为证书输入一些关于您本人的信息。此信息不会显示在应用中，但会作为 APK 的一部分包含在您的证书中。只有第一项是必须填的，其它的空着就行。当然了，如果写全了更好。



![]()

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723432858745-b43b5ddb-0d1a-4912-8217-f84c7bd56191.png)

选中 release，点下面的按钮 create。然后签名文件就生成好了。

### 使用密钥为应用自动签名
先把创建好的 key 文件 copy 到 android 目录下。

#### 建立 keystore.properties 文件
在 android 文件夹下建立名为 keystore.properties 文件，不叫这个名也可以，但一般都这么叫，没有特别的原因，还是用这个名字为好。在文件里放如下内容

```plain
storePassword=123456
keyPassword=123456
keyAlias=key17
storeFile=../key17.jks
```

注意：这里的 keyAlias 要和前面设置的 key alias 保持一致，否则找不到 key。密码当然也要一样。

#### 添加 gradle 配置
在 build.gradle 文件中，按如下方式加载 keystore.properties 文件（必须在 android 代码块前面）

```yaml
def keystorePropertiesFile = rootProject.file("keystore.properties")
def keystoreProperties = new Properties()
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))

android {
  ...
}
```

#### 输入存储在 `keystoreProperties` 对象中的签名信息
```plain
android {
  signingConfigs {
    release {
      keyAlias keystoreProperties['keyAlias']
      keyPassword keystoreProperties['keyPassword']
      storeFile file(keystoreProperties['storeFile'])
      storePassword keystoreProperties['storePassword']
    }
  }
  buildTypes {
       release {
            signingConfig signingConfigs.release
       }
       
  }
  ...
}
...
```

## 多渠道统计
apk 会发到不同的商店，你可能想知道哪个商店的下载量最大。这就需要渠道统计，这个功能要用到 dart 参数。

`--dart-define` 来指定不同的 dart 参数，比如

```plain
flutter build apk --dart-define channel=huawei
```

在 dart 代码里可以通过 `String.fromEnvironment` 获取到对应的自定义配置参数。

```plain
const channel = String.fromEnvironment('channel');
```

dart 代码拿到 channel 后就可以发统计数据出来。

## 发布优化
启用缩减、混淆处理和优化功能，

```java
android {
buildTypes {
    release {
    minifyEnabled true
    shrinkResources true
    proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
}
}
...
}
```

## 发布和安装
现在执行发布命令

```shell
flutter build apk --release
```

`--release` 可以省略，是默认值。

不出意外的话，build 成功可以看到类似如下信息

```shell
Built build/app/outputs/flutter-apk/app-release.apk (37.6MB)
```

apk 已经生成了，怎么装到手机上呢？三种方法。

1. 通过网络把 apk 发到手机上。比如有的包就是通过一个 http 链接发布的，下载后可以直接安装。
2. 通过 adb 命令安装

```plain
adb install build/app/outputs/flutter-apk/app-release.apk
```

1. 通过 `flutter install` 命令安装

## 优化
虽然做为新手，能打包成功就已经满心欢喜了，但还可以做的更好。

### apk 大小优化
不同的 Android 设备使用不同的 CPU，而不同的 CPU 支持不同的指令集。CPU 与指令集的每种组合都有专属的应用二进制接口 (ABI)。ABI 目前有两大类 arm 和 x86。arm 是当前手机的主流，x86 出现在模拟器上。flutter 默认支持 arm 和 x86 两种 ABI。我们用 android studio 查看一下。把刚才生成的 apk 直接拖到 android studio 上就行。查看 lib 文件夹，发现 flutter 为我们生成了三种 ABI 的文件。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723432921257-5de1242a-68a5-4291-af33-3992ed0ded27.png)

一种手机只用一种 ABI，所以把它们分别打包会更好些，体积会大大减小。只需要加一个参数就好 。

```shell
flutter build apk --split-per-abi
```

再看输出目录，原来的一个 apk 包现在分成三个了，每个只有原来 apk 1/3 大小。

或者用下面的命令，只打特定 ABI 的包。

```plain
flutter build apk --target-platform android-arm64
```

ABI 的更多信息见 [这里](https://developer.android.com/ndk/guides/abis?hl=zh-cn)

到此，基本的打包流程就讲完了，可以发布了。恭喜你，完成了从 0 到 1 的跨越！

如果你的产品只发布为一个版本，下面的内容可以先跳过。虽然有开发环境，测试环境，也需要打不同的包，因为差异不大，可以用简单的办法解决。比如用 `--dart-define` 设置变量。

用 applicationIdSuffix 为开发版设置不同的 applicationId。

```lua
buildTypes{
  ...
  debug {
    applicationIdSuffix ".debug"
    debuggable true
  }
}
```

## 多版本打包
可能你的产品还需要根据不同的用户出不同的版本，比如免费版本，收费版。

多版本要面临的问题主要有三个

1. 如何让多版本共存。
2. 如何做个性化设置
3. 优化打包。你可以在所有版本中把所有的功能都加上，简单的用开启关闭来控制。但这样会导致 apk 过大，也不大安全。

如果都手动配置，学习成本有点高，对于新手来说，我们可以用 flutter_flavorizr 这个插件，帮我们处理好许多事情，让我们有一个好的起点。

### 安装
```shell
flutter pub add dev:flutter_flavorizr
```

如果你这样执行安装命令的是话，会报错，原因是和前面的插件 flutter_launcher_icons，flutter_native_splash 的依赖冲突。解决的方法是把所有冲突的插件的 版本都改成 any，执行 `flutter pub get` 让 flutter 自己去处理冲突的问题。等安装成功后，再到 pubspec.lock 中找到对应的版本写入 pubspec.yaml 中。

### 配置
在 pubspec.yaml 的末尾加上如下配置 、

```yaml
flavorizr:
  flavors:
    apple:
      app:
        name: "Apple App"

      android:
        applicationId: "com.example.apple"

      ios:
        bundleId: "com.example.apple"

    banana:
      app:
        name: "Banana App"

      android:
        applicationId: "com.example.banana"
      ios:
        bundleId: "com.example.banana"
```

flavors 可以理解为多版本发布。这个配置中有两个版本发布配置，apple 和 banner。

name 指定 App Name 为 apple App，Banana App，App Name 就是 App 安装成功后在图标下面显示的名字。

applicationId 是 app 的身份证。这里我们看到 applicationId 是不同的，代表两个完全不同的 app，但包名是同一个。从这个实例中我们可以更加清楚的看到 applicationId 和 package 是独立的两个设置，不能混为一谈。

### 执行命令
```shell
flutter pub run flutter_flavorizr
```

等命令执行完成，发现插件为我们做了很多事情。

#### 自动完成 flavor 配置
build.grade 中增加了 flavor 的配置

```vbnet
// ----- BEGIN flavorDimensions (autogenerated by flutter_flavorizr) -----
flavorDimensions "flavor-type"

productFlavors {
    apple {
        dimension "flavor-type"
        applicationId "com.example.apple"
        resValue "string", "app_name", "Apple App"
        }
    banana {
        dimension "flavor-type"
        applicationId "com.example.banana"
        resValue "string", "app_name", "Banana App"
        }
    }

// ----- END flavorDimensions (autogenerated by flutter_flavorizr) -----
```

注意：BEGIN ，END 这两行注释不要做修改，也不能删除，否则插件无法维护这里面的配置了。

#### 自动添加资源文件
在 android/app/src 下多了两个文件夹 banner，apple 里面有启动 icon。

#### 自动增加不同版本的入口文件
lib 下面增加了四个文件：main_apple.dart，main_banana.dart，app.dart，flavios.dart

原来的 main.dart 还是在的，我们执行 flutter run 还是会启动 main.dart，就像没有执行过 `flutter pub run flutter_flavorizr` 一样。

#### flavor 打包
为了能进行 flavor 打包，可以这样执行命令

```plain
flutter run --flavor apple -t lib/main_apple.dart
```

你会发现，这次启动入口是 main_apple.dart，启动图标也换了。这次会安装一个新的 app，和原来 main.dart 生成的 app 共存。

我们把 banner app 也装一下。

```plain
flutter run --flavor banana -t lib/main_banana.dart
```

这次也会新安装一个 app，和前面的 apple app，main app 并存。

main app 的 applicationId 是 com.iam17.app17，apple app 的 applicationId 是 com.example.apple ，它们可以完全不一样，applicationId 就是一个身份标识，只要全球唯一就行。我们做的产品的时候，不可能这样随意设置 applicationId，一般前面的不变，只修改最后的名字。比如可以这样设置

```plain
com.iam17.app17.free
com.iam17.app17.pro
com.iam17.app17.test
com.iam17.app17.debug
```

### 设置 api url
不同版本的 api 的地址可能是不同的。我们可以打开 flavors.dart，仿照 title 增加一个apiUrl，在需要的地方，用 `F.apiUrl` 获取地址。

```plain
static String get apiUrl {
    switch (appFlavor) {
      case Flavor.APPLE:
        return 'http://apple/api';
      case Flavor.BANANA:
        return 'http://banana/api';
      default:
        throw UnimplementedError();
    }
  }
```

flavors.dart 默认是受 插件管理的，如果你再次执行 `flutter pub run flutter_flavorizr`，flavors.dart 的内容会被重置。为了避免内容被重置，下次再执行命令的时候，需要挑选 processor 执行。

```plain
flutter pub run flutter_flavorizr -p <processor_1>,<processor_2>
```

## vscode 配置 launch.json
为了启用不同的 flavor，需要在执行命令的时候加个 flavor 参数 和 target（-t） 参数，每次写这么长的命令很不方便。我们可以在 vscode 中增加 lauch.json 把命令记录下来。下次运行的时候，只要点个按钮就可以了。

### 增加 launch.json
如果你的项目里没有 lauch.json 点红框那个图标，就会有添加提示，点击 create a lauch.json file，就会自动为你添加一个。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433003351-a25eea7c-cb3e-45d5-8cb3-d77854300e49.png)

替换成下面的内容

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "apple",
      "program": "./lib/main_apple.dart",
      "request": "launch",
      "type": "dart",
      "args": ["--flavor","apple"]

    },
    {
      "name": "banana",
      "program": "./lib/main_banana.dart",
      "request": "launch",
      "type": "dart",
      "args": ["--flavor","banana"]
    }
  ]
}
```

同样点左面那个 run and debug 图标，这时会出现我们刚配置好的两从此启动，apple和 banana。apple 前面打勾，表示默认，直接按 F5 会用启动 apple，可以点 banana，把默认改为 banana。

![](https://cdn.nlark.com/yuque/0/2024/png/207857/1723433016957-7ef4f1d0-9ae6-4885-9c06-c1a646bbfebe.png)

为了齐整，现在可以把默认的 main.dart 删除，把闪屏相关的代码也 copy 到 main_apple.dart 中。把 launcher icon 也 copy 到 apple 下面。为了能让大家看到执行 launcher icon 插件的效果，main 下面的 icon 就不删除 了。



本文用了三个插件来解决三个不方便处理的问题，以便降低打包难度，快速上手。在使用插件的时候，应该了解插件做了什么，最后应该能做到手动也可以处理。

 

  


 

