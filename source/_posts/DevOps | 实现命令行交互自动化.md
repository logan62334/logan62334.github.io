---
title: 'DevOps | 实现命令行交互自动化'
date: 2017-12-01 22:48:52
tags: DevOps Python
category: DevOps
---
>嗨呀，好久没有更新了，最近几个月一直忙着部门内质量平台的建设，因为是新成立的小组所以很多东西都是从零开始做，这期间做了很多跟自动化、代码质量和工程效率相关的事情，接下来一段时间会慢慢把其中一些有趣的东西整理出来跟大家分享。

今天先来介绍一个Python中用来实现命令行交互自动化的模块，之所以会有这样的需求是因为我们希望把一些繁琐的命令行交互过程给透明化这样对用户来说会友好很多降低使用成本，如下图：

![](https://github.com/logan62334/ImageArchive/raw/master/DevOps/1.png)

这里是一个典型的需要用户交互的命令行操作，当执行命令后会提示用户输入测试脚本文件名，回车后会再提示用户输入app的路径，如何让这一过程自动化呢？

![](https://github.com/logan62334/ImageArchive/raw/master/DevOps/2.png)

就是它了shutit，其实还有个工具 pexpect 但是我试了好多次都没能达到想要的效果，而且网上大部分给出的解决方案也都是针对ssh登录自动化的，对于一个普遍的交互式命令行却不支持，当然也可能是我使用姿势不对？如果大家有通过pexpect实现的还请跟我交流哈

***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
