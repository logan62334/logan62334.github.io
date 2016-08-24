---
title: Android热修复技术实践
date: 2016-02-01 17:54:53
tags: Android 热修复
category: Android新技术实践
---
![](https://github.com/logan62334/ImageArchive/raw/master/Android/13.jpg)
 1. 背景
我所在的公司是一家互联网金融领域的初创型公司，这类公司向来都是奉行快速迭代敏捷开发，所以基本我们的APP两周一个迭代有时候工期比较紧可能一周一个迭代，在缺乏专业QA人员的情况下单凭团队内其他做产品和运营的小伙伴的用最原始的人工点击测试下难免会遗漏一些潜在bug，于是当我们的应用发布之后，经过十多万用户的随机点击和产生的随机数据突然发现了一个严重bug需要进行紧急修复的时候公司各方就会忙得焦头烂额：还原bug、修复、重新打包App、测试、向各个应用市场和渠道换包、提示用户升级、用户下载、覆盖安装。有时候仅仅是为了修改了一行代码，也要付出巨大的成本进行换包和重新发布。 这时候就产生了一个问题：有没有办法以补丁的方式动态修复紧急Bug，不再需要重新发布App，不再需要用户重新下载，覆盖安装？ 虽然Android系统并没有提供这个技术，但是很幸运的告诉大家，答案是：可以的！而且在最初由QQ空间团队提出的热补丁动态修复技术方案后各大公司也都纷纷效仿，网上也出现了很多开源的解决方案。
 2. 实际案例
让我们先来看一下网上都有哪些解决方案：
Xposed
dexposed
AndFix
DroidFix
DynamicAPK
Nuwa
根据其描述，原理都来自：Android dex分包方案（http://codecloud.net/android-hot-load-6575.html）。这里就不对这些框架做过多对比了，因为原理都一致，实现的代码可能有些差异并不是特别大。
这里我就先讲讲我是怎么去对热修复框架进行选型的吧，大家也看到了能实现这个功能的开源框架最近出了很多但并不是每个都适合我们现在的应用，Xposed它因为需要手机获得root权限才能生效所以首先pass。
dexposed和AndFix是阿里基于Xposed的思路沉淀出的两套热修复解决方案，但是经过实际测试后发现它由于缺少动态库so文件所以在大部分机型上都不能正常运行：
![](https://github.com/logan62334/ImageArchive/raw/master/Android/14.jpg)
在其官方github上有类似很多的问题所以也只能放弃虽然它的补丁生成器做的很完善但是然并卵……
DynamicAPK这个是携程最新公布的一个解决方案，但是由于它功能实在过于庞大而且其主要功能是用来做多apk动态加载的所以对热修复这块并不是重点实现。
最后我把注意力放在了Nuwa上，但是经过实际部署测试后发现总是编译出错于是翻看了github上的issue列表发现有很多人也遇到了类似问题最后通过排除掉Nuwa.init所在的类后就fix好了，原来是Nuwa所在的class文件是无法进行修复的，所以在这个文件中尽量只写一些初始化代码和配置保证不会出问题。然后接下来遇到的就是补丁包生成不了的窘境了……继续google：
https://github.com/jasonross/Nuwa/issues/23在这篇文章中找到了答案。
好了接下来就是考虑到我们有20个渠道并且可能出现不同版本的补丁包的情况了，于是我申请了一台ftp服务器专门用来做为每次补丁包的存放路径，在这个路径下：
![](https://github.com/logan62334/ImageArchive/raw/master/Android/15.jpg)
这样在客户端通过每次拉取config配置文件的api进行判断当前版本是否需要打补丁，如果需要则根据当前渠道号从对应的ftp服务器下载，当用户第二次打开应用的时候就会load这个patch，这样就在用户毫不知情的情况下完成了问题修复。
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
