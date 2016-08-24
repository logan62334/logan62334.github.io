---
title: '[Android] 移动开发技术周报 #198'
date: 2016-04-03 18:18:35
tags: Android移动开发技术周报
category: Android移动开发技术周报
---
>最近在浏览各大技术类资讯平台的时候发现Android Weekly是目前感觉在质量和时效性上都比较不错的平台，有兴趣的同学可以去http://androidweekly.net/ 订阅，但同时有了一个想法因为这个网站每次推送的都是纯英文版，说实话我一开始也是有点不太习惯因为阅读速度一下慢了好多，于是我打算试着去每周翻译一刊他们的推文希望可以为一些英文不太好但特别喜欢Android的同学带来些帮助，让大家可以快速了解目前的Android技术动态。

### 文章&学习指南
[使用Design Support Library中的Bottom Sheets](http://code.tutsplus.com/articles/how-to-use-bottom-sheets-with-the-design-support-library--cms-26031?utm_source=Android+Weekly&utm_campaign=c1ffc42de6-Android_Weekly_198&utm_medium=email&utm_term=0_4eb677ad19-c1ffc42de6-337919745)
随着时间的推移Design support library在被慢慢的改善，在23.2版本中增加了对Bottom Sheets的支持，在本文中你将很轻松的学会如果在你的应用中实现Bottom Sheets。
[Android Thread Annotations的缺点](http://mcomella.xyz/blog/2016/thread-annotations.html?utm_source=Android+Weekly&utm_campaign=c1ffc42de6-Android_Weekly_198&utm_medium=email&utm_term=0_4eb677ad19-c1ffc42de6-337919745)
当像@UiThread和@WorkerThread这样的Android thread annotations被发布的时候，Michael Comella 很兴奋，然后许多个月过后他发现这种注解并没有像他所希望的那样有效果但又不知道为什么，于是他决定研究一下。
[AutoValue Extensions](http://androidweekly.us2.list-manage.com/track/click?u=887caf4f48db76fd91e20a06d&id=cb5f9ea63c&e=240370e0eb)
谷歌的AutoValue库在即将发布的extensions中提供了简单的值类型，这个演讲介绍了这个扩展的功能，囊括了对Android有用的扩展并且在构建你自己的应用中提供一些建议。
[Vectors](http://androidweekly.us2.list-manage.com/track/click?u=887caf4f48db76fd91e20a06d&id=fe6fb646b1&e=240370e0eb)
这是偶然在第三方平台上看到的一个关于Android VectorDrawable的支持包,另外Google发布了Android 23.2支持库,其中包含了备受期待的VectorDrawableCompat。
[五种很少有人知道的会阻塞主线程的情况](http://androidweekly.us2.list-manage.com/track/click?u=887caf4f48db76fd91e20a06d&id=ee170d5d5b&e=240370e0eb)
一般来说，任何方法的调用导致主线程阻塞时间超过16*N毫秒都会引起掉帧。我们称这种方法为阻塞方法。在这篇文章中，我们将首先看一个阻塞方法的例子，然后再看五个阻塞主线程的方式。
[开源的Android LightCycle](http://androidweekly.us2.list-manage.com/track/click?u=887caf4f48db76fd91e20a06d&id=6bfab89c07&e=240370e0eb)
SoundCloud最近开源了LightCycle，LightCycle是一个Android库有助于把Activity和Fragment中的逻辑拆分成小的代码块。
[在Airbnb的Android版中采用RxJava](http://androidweekly.us2.list-manage.com/track/click?u=887caf4f48db76fd91e20a06d&id=497e831fc0&e=240370e0eb)
这个演讲中分享了Airbnb采用这些新模式和技术的经验，以及遇到的困难和坑。
[第一个五年](http://androidweekly.us2.list-manage2.com/track/click?u=887caf4f48db76fd91e20a06d&id=c2859e69ea&e=240370e0eb)
Mark Allison通过每周写很有深度的文章来跟我们分享他的Android知识，请一定要在Twitter或者G +上感谢他。
[RxJava的一些问题](http://androidweekly.us2.list-manage2.com/track/click?u=887caf4f48db76fd91e20a06d&id=4ac7782453&e=240370e0eb)
Thomas Nield写了一些在使用RxJava中你可能会遇到的一些问题

### 赞助
[Buddybuild：世界上第一个移动的持续集成平台]()
Buddybuild是一个移动的持续集成和部署平台，只需要几分钟设置。我们的SDK使用户能够无比轻松地实时获取用户的反馈和崩溃报告。不用再维护不同的构建、部署、崩溃报告和反馈系统。专注于自己最擅长的：创建人们喜爱的应用程序。
[Hired—Android开发者的招聘平台]()
国外版的100Offer

### LIBRARIES & CODE
[Lightcycle](http://androidweekly.us2.list-manage.com/track/click?u=887caf4f48db76fd91e20a06d&id=f1178a42e7&e=240370e0eb)
[MaterialColorsApp](http://androidweekly.us2.list-manage1.com/track/click?u=887caf4f48db76fd91e20a06d&id=857c42f8d8&e=240370e0eb)
MaterialColorsApp 是一个方便的Mac小应用程序，让您可以快速访问标准的材料设计调色板。
[LandscapeVideoCamera](http://androidweekly.us2.list-manage.com/track/click?u=887caf4f48db76fd91e20a06d&id=866d14b078&e=240370e0eb)
非常强大的android 视频录制库，可以选择视频尺寸以及视频质量，只允许横屏录制。

### 新闻
[Android Experiments I/O 挑战赛](http://androidweekly.us2.list-manage.com/track/click?u=887caf4f48db76fd91e20a06d&id=20f53c827b&e=240370e0eb)
去年夏天谷歌开发者社区启动了 Android Experiments ：一个用于展示创新性 Android 作品的项目。所有开发者都可以提交你的创意作品，通过后将加入到网站收录中。
[Fragmented Podcast 更新了 – TSHIRTS!](http://androidweekly.us2.list-manage1.com/track/click?u=887caf4f48db76fd91e20a06d&id=80f92d1309&e=240370e0eb)
发布限量版的Fragmented T-Shirt
[Android Studio 2.1 Preview 4 可用了](http://androidweekly.us2.list-manage1.com/track/click?u=887caf4f48db76fd91e20a06d&id=03c5fb2bff&e=240370e0eb)
Google在canary渠道推送了Android Studio 2.1 Preview 4，修复了很多bug。

### 工具
[Google新的 Accessibility Scanner](http://androidweekly.us2.list-manage.com/track/click?u=887caf4f48db76fd91e20a06d&id=6e8b5a3f8a&e=240370e0eb)
全新的Accessibility Scanner应用程序允许你检查潜在的问题从而改进你的应用程序。它在Play商店免费下载，但目前它仅限于Android 6.0设备。
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
