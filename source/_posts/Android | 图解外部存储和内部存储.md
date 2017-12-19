---
title: 'Android | 图解外部存储和内部存储'
date: 2017-12-19 22:53:33
tags: Android 存储
category: Android
---
### 存储概述
Android中根据数据是否为应用私有、是否需要给外部应用暴露以及数据的大小可以有以下几种选择：

* Shared Preferences
* 内部存储
* 外部存储
* 本地数据库存储
* 通过网络在服务器端数据库存储

今天我们重点解释下内外部存储到底是什么有哪些区别，请看下图：
![](https://github.com/logan62334/ImageArchive/raw/master/Android/79.jpeg)

### 内外部存储的区别
* 按照内外部存储：带External字眼则一定是外部存储的方法，如 getExternalFilesDir() ，外部存储需要运行时权限；
* 按照公有私有性质：公有文件是Environment调用函数，而私有文件（包括内部私有与外部私有）是Context调用函数，公有文件不会随着app卸载而删除而私有则会，私有文件不会被Media Scanner扫描到。

***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
