---
title: '配置Android项目 - 一些重要的事情'
date: 2017-3-10 11:39:58
tags: Android架构
category: Android架构
---
### gitignore

当你在Android Studio中创建一个新的Android项目时，它已经生成了gitignore文件，但通常它不包含所有必要的规则。

为了快速生成和下载gitignore文件，我建议您使用gitignore.io网站。只需输入必要的关键字，如 — Android，Intellij并点击生成按钮。

![1](https://github.com/logan62334/ImageArchive/raw/master/Android/33.png)

> 在模板项目中查看[*gitignore*](https://github.com/dmytrodanylyk/template/blob/master/.gitignore)文件。

### tools folder

如果你有一些第三方脚本，规则集或其他与您的项目相关的文件不要只是简单的把它们放在根目录 —它会造成混乱。（特别是对于那些使用Project视图，而不是Android视图）

尝试创建一个文件夹（例如tools），并将所有这些文件放入此文件夹。

![2](https://github.com/logan62334/ImageArchive/raw/master/Android/34.png)

通常我在那里放一些自定义的gradle脚本文件，proguard和静态代码分析工具的规则，如pmd，findbugs，lint。

> 在模板项目中查看 [tools](https://github.com/dmytrodanylyk/template/tree/master/tools)文件夹。

### flavors

Flavours用于创建具有不同设置的构建。在大多数情况下，我会立即设置两种flavors — dev和prod：

- applicationId
- versionCode / versionName
- server endpoints
- google services keys
- …

![3](https://github.com/logan62334/ImageArchive/raw/master/Android/35.png)

> 在模板项目中查看 [productFlavors](https://github.com/dmytrodanylyk/template/blob/master/app/build.gradle#L33)。

### keystore

keystore是一个二进制文件，其中包含一个或多个用于签署应用程序的私钥。

当从IDE运行或调试项目时，Android Studio会使用Android SDK工具生成的调试证书自动为您的APK签名。

使用本地调试keystore时有几个问题：

- 到期日365天
- 从多台计算机安装应用程序需要先卸载
- google服务需要密钥库SHA-1指纹

这就是为什么我通常生成调试密钥库并提交到版本控制系统。

![4](https://github.com/logan62334/ImageArchive/raw/master/Android/36.png)

> 在模板项目中查看 [signingConfigs](https://github.com/dmytrodanylyk/template/blob/master/app/build.gradle#L18)。

### proguard

Android proguard用来做三件事：

- 压缩未使用的代码 — 帮助你不超出64k限制
- 优化代码和apk
- 混淆代码 — 使你的APK难以做逆向工程

问题是混淆和代码优化显着增加了编译时间，使调试更困难。

这就是为什么最好对发布和调试版本使用不同的proguard规则：

- rules-proguard.pro
- rules-proguard-debug.pro

![5](https://github.com/logan62334/ImageArchive/raw/master/Android/37.png)

用于调试构建的Proguard规则必须具有以下行以强制proguard忽略警告，跳过代码混淆和优化：

![6](https://github.com/logan62334/ImageArchive/raw/master/Android/38.png)

对于发布版本，设置proguard规则将会更加困难，因为几乎每个库都有自己的特定规则。幸运的是，有一个开源代码库 —  [*android-proguard-snippets*](https://github.com/krschultz/android-proguard-snippets)，它包含所有主要库的proguard规则。

![7](https://github.com/logan62334/ImageArchive/raw/master/Android/39.png)

> 在模板项目中查看 [rules-proguard.pro](https://github.com/dmytrodanylyk/template/blob/master/tools/rules-proguard.pro)和[rules-proguard-debug.pro](https://github.com/dmytrodanylyk/template/blob/master/tools/rules-proguard-debug.pro)。

### strict mode

Android StrictMode可帮助您检测不同类型的问题：

- 可关闭对象没关闭
- 在主线程中读写文件或者访问网络
- uri 暴露
- …

每当检测到这样的问题，它可以显示适当的日志或应用程序崩溃，具体取决于你的配置。

我建议你只在调试的时候打开它并且使用detectAll方法来检测所有类型的问题。

![8](https://github.com/logan62334/ImageArchive/raw/master/Android/40.png)

这里是当你忘记关闭SQLiteCursor的日志的例子：

![9](https://github.com/logan62334/ImageArchive/raw/master/Android/41.png)

> 在模板代码中查看[StrictMode](https://github.com/dmytrodanylyk/template/blob/master/app/src/main/java/com/dd/template/TemplateApplication.java#L12)。
***
![全栈增长工程师，欢迎关注](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)