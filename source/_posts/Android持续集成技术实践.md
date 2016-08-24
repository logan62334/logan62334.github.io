---
title: Android持续集成技术实践
date: 2016-02-02 16:08:59
tags: Android自动化构建
category: Android新技术实践
---
![](https://github.com/logan62334/ImageArchive/raw/master/Android/6.jpg)

**背景**

随着业务需求的演进，工程的复杂度会逐渐增加，自动化的践行日益强烈。事实上，工程的自动化一直是我们努力追求的目标，能有效提高我们的生产效率，最大化减少人为出错的概率，实现一些复杂的业务需求应变。

以我现在的公司为例，我们有22个渠道包，而且分为测试环境和生产环境，新的迭代开始除去要经常给测试人员直接烧测试版APP偶尔还会被商务和运营打断要求新增一个渠道包。尤其临近发版的一周，几乎每天都要新版本。这样的话，有两方面的影响：第一，打断了开发人员的开发进度；第二，开发人员打包效率低下。

要解决这个问题，必须实现移动端应用的自动化构建。具体说来就是，使用持续集成（CI）系统jenkins，自动检测并拉取Git上的最新代码，自动打包成不同的渠道apk，自动上传到内测分发平台蒲公英上和自建的FTP服务器上。（接下来，测试人员只要打开一个（或多个）固定的网址，扫描一下二维码，就能下载最新的版本了…）

**环境**

因为公司内网的服务器都是Windows操作系统，所以下面的操作都是以Windows为例，无论是哪个操作系统，jenkins的配置是一样的。

安装Jenkins

官网地址： http://jenkins-ci.org/，具体安装过程就不详写了跟平常装软件没什么区别。
默认访问 http://localhost:8080/ , 可进入jenkins配置页面。

安装Jenkins相关插件

点击系统管理>管理插件>可选插件，可搜索以下插件安装

git插件(GIT plugin)

ssh插件(SSH Credentials Plugin)

Gradle插件(Gradle plugin) - android专用

注：
 1. 这里要用VPN或者修改系统的hosts文件才可以搜索到插件；
 2. 还有就是Windows中要装好JDK、Git、Gradle的环境。

装好后的效果图：
![](https://github.com/logan62334/ImageArchive/raw/master/Android/7.jpg)

新建Job

主页面，新建 -> 构建一个自由风格的软件项目即可。

配置git仓库

如果安装了git插件，在源码管理会出现Git，选中之后：
![](https://github.com/logan62334/ImageArchive/raw/master/Android/8.jpg)

配置自动拉取最新代码

在构建触发器中，有两种自动拉取代码并编译的策略:

 1. 设置Poll SCM，设置定时器，定时检查代码更新，有更新则编译，否则不编译。
 2. 也可以设置Build periodically，周期性的执行编译任务。
![](https://github.com/logan62334/ImageArchive/raw/master/Android/9.jpg)

配置gradle

如果安装gradle插件成功的话，应该会出现下图的Invoke Gradle script，配置一下:
![](https://github.com/logan62334/ImageArchive/raw/master/Android/10.jpg)

这样，就能自动在project下的app的build/outputs/apk下生成相应的apk.

因为要区分测试环境和生产环境，所以我建了两个任务分别对应git上的主分支和子分支：
![](https://github.com/logan62334/ImageArchive/raw/master/Android/11.jpg)

FTP服务器搭建：

如果不会利用IIS搭建FTP的同学可以自行百度这里就不详细介绍了，记得把FTP根路径指向编译结果的目录：D:\Jenkins\jobs\XXForAndroidTest\workspace\app\build\outputs\apk
![](https://github.com/logan62334/ImageArchive/raw/master/Android/12.jpg)

如果编译失败，请检查以下问题：
 1. 确保gradle、git、jdk的环境变量都配好
 2. 找不到local.properties中sdk定义，因为一般来说local.properties不会添加到版本库。
 3. 还有就是子项目中build.gradle的签名秘钥的路径问题

关于local.properties的定义：

    sdk.dir=xx/xx/android-sdk

再编译一般就会编译成功，当然当那些第三方库需要重新下载的话，编译可能会很慢。

**总结一下**

经过以上的折腾，以后终于可以彻底解放开发人员的双手去专心写代码了，我们在以后的工作中也要尽量去把精力放在业务上面提高工作效率。
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
