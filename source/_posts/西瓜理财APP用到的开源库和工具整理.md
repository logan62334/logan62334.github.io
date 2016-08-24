---
title: 西瓜理财APP用到的开源库和工具整理
date: 2016-03-17 22:08:15
tags: Android笔记
category: Android笔记
---
今天来聊聊我之前负责过的一款APP——西瓜理财Android版本所用到的一些开源库和开发工具，不过由于微信公众号不支持外链所以就不贴地址了。

#### Android studio 插件
1、Android ButterKnife Zelezny
这是著名的Jake Wharton黄油刀插件，用过的都说好，连注解都不用亲自写了，效率直线提升。
![](https://github.com/logan62334/ImageArchive/raw/master/Android/1.gif)
2、GsonFormat
这个插件可以将mobileapi返回的json数据直接转换为实体类，省去了我们写一大堆的字段属性和Getter、Setter方法所花费的时间。
![](https://github.com/logan62334/ImageArchive/raw/master/Android/2.gif)
3、Android Parcelable code generator
大家如果用到Parcelable来序列化实体类的话，将会面临比Serializable复杂的多的步骤所以通过使用这个插件来帮我们一键生成对应的方法。另外：需要注意的是当有新的属性加入的时候记得重新生成一次不然会出现序列化错误。
![](https://github.com/logan62334/ImageArchive/raw/master/Android/3.jpg)
4、.ignore
这个是配合Git控制来忽略一些本地配置文件和不需要同步的代码文件。
5、Genymotion
这个就不必多说了，用过的都说好。
#### 第三方库
1、Nuwa
最近议论最多的热修复框架，这只是其中一种实现方案。
2、Umeng
用来做APP统计分析的平台，不过建议大家以后可以考虑阿里最近推出的移动应用数据分析平台。
3、诸葛IO
一款精细化数据分析的工具，重点在移动用户行为分析不过由于后期数据激增开始收费了所以放弃了。
4、Cobub Razor
号称私有版的友盟，因为考虑到友盟统计的数据不太能真实反映用户情况，所以我们决定搭建一套自己的数据采集分析系统但考虑到时间成本所以采用了这个开源项目，省去了设计各种上传策略的时间。
5、极光推送
这个要说一点的是注意官网的各种cpu架构下的so文件更新，一定要全都加入工程不然会在个别机型上报本地库加载异常的错误。
6、Fresco
这个是Facebook最近推出的一款图片加载框架，对OOM的问题做了特殊优化。
7、sharesdk
第三方分享首选
8、ButterKnife
都说程序员都是比较懒的，什么事情都想着让程序自动化帮忙减轻工作量，这个开源库可以让我们从大量的findViewById()和setonclicktListener()解放出来，最令人兴奋的是其对性能的影响微乎其微！
9、Gson
谷歌GSON这个Java类库可以把Java对象转换成JSON，也可以把JSON字符串转换成一个相等的Java对象。Gson支持任意复杂Java对象包括没有源代码的对象。
10、EventBus
在编程过程中，当我们想通知其他组件某些事情发生时，我们通常使用观察者模式，正是因为观察者模式非常常见，所以在jdk1.5中已经帮助我们实现了观察者模式，我们只需要简单的继承一些类就可以快速使用观察者模式，在Android中也有一个类似功能的开源库EventBus，可以很方便的帮助我们实现观察者模式，另外注意：EventBus有好几款开源库，github上有人专门做过对比各个库的优缺点大家可以参考。
11、Netroid
Netroid是一个基于Volley实现的Android Http库，提供执行网络请求、缓存返回结果、批量图片加载、大文件断点下载的常见Http交互功能，致力于避免每个项目重复开发基础Http功能，实现显著地缩短开发周期的愿景。
12、腾讯X5浏览内核
腾讯X5浏览服务由QQ浏览器团队出品，致力于优化移动端webview体验的整套解决方案，使用QQ浏览器X5内核SDK和X5云端服务，解决移动端webview使用过程中出现的一切问题，优化用户的浏览体验，同时腾讯还将持续提供后续的更新和优化，为开发者提供最新最优秀的功能和服务。
#### 其他开发工具
1、蒲公英
其实这种应用内侧分发的平台很多，之所以选蒲公英是因为他有mac版的客户端上传比较方便，而且还有对于的gradle代码用来实现自动化打包发布。
2、Charles
这个是一款功能比较强大的抓包工具，在跟mobileapi对接和测试中非常高效。
3、LeakCanary
强烈推荐，帮助你在开发阶段方便的检测出内存泄露的问题，使用起来更简单方便。
4、Logger
让开发调试效率提高至少300%而且心情愉悦的Log神器。
#### 结语
今天就写到这里吧，以后会定期推荐一些好的开源库和工具，详情戳公众号的菜单栏。
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
