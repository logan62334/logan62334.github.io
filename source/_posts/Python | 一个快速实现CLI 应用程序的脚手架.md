---
title: 'Python | 一个快速实现CLI 应用程序的脚手架'
date: 2017-12-02 22:48:52
tags: Python
category: Python
---
今天跟大家分享一下如何快速实现一个Python CLI应用程序的脚手架，之所以会做这个是因为当时需要做一个运维的小工具希望用命令行的方式来使用，但是搜遍网上很多资料都没有系统讲解从开发、集成、发布、文档等一系列流程的文章。

### 工程结构
***
![](https://github.com/logan62334/ImageArchive/raw/master/Python/1.png)

如上图，这就是一个比较规范的Python CLI应用项目了，下面一一讲下各文件的用途：

### 项目文档
***
这里我们用Sphinx来实现文档的自动生成，当然你要首先通过markdown和rst文件定义好文档的内容，然后进入docs目录执行 make html命令就可以在_build目录下生成对应的静态文件，如下图：

![](https://github.com/logan62334/ImageArchive/raw/master/Python/2.png)

具体Sphinx如何使用以及配置后面会单独文章讲解

### 主工程
***
这里讲几个需要注意的地方
1、日志的配置：
这里可以全局设置日志的一些输出级别和格式化方式

![](https://github.com/logan62334/ImageArchive/raw/master/Python/4.png)

2、cli文件
这里通过click库来实现

![](https://github.com/logan62334/ImageArchive/raw/master/Python/5.png)

3、二进制文件打包

![](https://github.com/logan62334/ImageArchive/raw/master/Python/3.png)

如上图，有时候我们的工程中会包含二进制文件，也就是非Python代码的文件，这时候如果还是像往常一样打包发布，安装的时候会发现无法找到此文件，所以需要在根目录的MANIFEST.in文件中加入

![](https://github.com/logan62334/ImageArchive/raw/master/Python/6.png)

### 脚本
***
如下图，这里的make-release文件主要是用来自动控制版本的，如下图，通过Git 的提交记录了来作为项目的唯一版本号标识，再对__init__文件进行重新写入达到持续集成时版本号自增的目的。

![](https://github.com/logan62334/ImageArchive/raw/master/Python/7.png)

### 单元测试
***
test文件夹中存放的就是项目的单元测试文件了，这里就不细展开讲了，后面会具体讲讲如何跟Jenkins集成实现静态代码检查

### setup
***
最重要的就是setup.py这个文件了，项目最后打包发布到pypi仓库主要的配置信息都在这里了，如下图：

![](https://github.com/logan62334/ImageArchive/raw/master/Python/8.png)

这个脚手架的项目地址：https://github.com/logan62334/python-cli-template
项目会持续更新，可以点击阅读原文访问

***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
