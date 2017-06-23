---
title: 'Macaca 面向多端的自动化测试解决方案'
date: 2017-6-22 22:39:58
tags: 自动化测试
category: 自动化测试
---
### 背景
***
对于基于 UI 的功能测试的需求其实一直存在，理由其实很简单，不想一直让人去做重复机械的事情，而且可靠性完全是靠人力的堆积产生。然而目前部门的功能测试工作依然主要是依靠人工来完成，从我们公司的实践来看我觉得有几个方面的影响因素：

客户端APP已经实现模块化开发，而且外卖平台移动端的开发迭代流程正在进行改造，目标是从固定每三周一个迭代改造为每周一个发布窗口，版本迭代的提速，设备的碎片化，都给测试工作带来巨大的挑战。
由于版本迭代周期越来越短，而且UI变动比较频繁，因此测试编写测试代码的积极性不是很高，同时由于测试代码的可重复利用性差，导致测试脚本的编写成本和维护成本偏高 。
部分测试人员的编码能力不是很强。由于大部分测试人员可能并没有过多的开发经验，所以在编写测试代码时并不能很顺畅的完成自己想要的效果，这样也会导致测试代码项目的推广阻力会比较大。

如何在有限的时间内，追求尽可能高的产品质量？录放平台是我们推出的解决方案。它支持本地化UI脚本录制，集中式脚本管理，分布式脚本执行。业务测试只要开启我们的服务，就可以在业务测试的过程中，自动生成对Android、iOS和Web页面的自动化脚本，而自动化脚本在批量设备上的回放，可以极大提高关键路径的覆盖率，提升兼容性测试的效率，从而可以把业务测试从冗长重复的步骤中解放出来，把精力放到边界，异常等可以给我们产品带来更多提升的地方。 

通过不断地寻找，不断地对比，最终我们将目标聚焦在阿里巴巴开源解决方案Macaca上。

### 简介
***
Macaca是一套完整的自动化测试解决方案，它的三个特性对我们极具吸引力：

1、周边工具支持（Reliable、app-inspector、UI-Recorder等）
2、它是一个轻量化的开源项目
3、社区活跃，中文文档丰富
4、支持JS、Python、Java编写自动化脚本
5、API比较统一

### 技术栈
***
在落地Macaca之前，需要先部署下列技术栈：
1、Node.js用于部署Macaca
2、Docker用于容器化Macaca的部署环境
3、Gitlab用于存储代码和测试用例
4、Slack用于团队的沟通协调
5、Python用于部署本地Agent

### 使用流程
***
业务测试人员通过在本地录制好测试脚本，然后上传到脚本管理平台，这些测试脚本将会根据业务模块和版本分类管理。使用者在自己的电脑上安装Agent，然后连接测试设备，Agent会将本机的ip、port和设备信息上传注册到录放平台。

新建一个task执行脚本回放操作，可以指定在哪些机器上回放也可以推送到STF手机管理平台批量回放，测试用例运行之后，会有两种情况发生：如果成功，则可以直接查看生成报告；否则会通过Slack或邮件通知开发人员测试失败，重新修改代码。

另外Macaca也提供了相应的分布式持续集成框架Reliable来进行任务管理。

### Reliable
***
下图是Reliable的界面，通过Reliable用户可以查看测试用例和测试结果；并且Reliable天生与Macaca无缝衔接。

![](https://github.com/logan62334/ImageArchive/raw/master/Android/72.jpeg)

![](https://github.com/logan62334/ImageArchive/raw/master/Android/73.jpeg)

![](https://github.com/logan62334/ImageArchive/raw/master/Android/74.jpeg)

### Inspector
***
Macaca中还提供了Inspector工具供用户直观、方便查找到想要选中的元素。图中右侧一栏提供的是XPS、ID、Name数据，用户通过Inspector工具寻找目标界面的元素。

![](https://github.com/logan62334/ImageArchive/raw/master/Android/75.jpeg)

### Debug
***
我们选择Visual Studio Code作为常用的IDE因为它能够轻量地、方便地支持使用者Debug，用户可以根据自己喜好选择相应地调试工具。

![](https://github.com/logan62334/ImageArchive/raw/master/Android/76.jpeg)

### UI Recorder
***
下图是简单的登录测试用例：输入用户名和密码，然后点击登录按钮。UI-Recorder脚本录制工具可以快速的通过录制得到脚本，方便新手入门。

![](https://github.com/logan62334/ImageArchive/raw/master/Android/77.jpeg)

### 测试报告
***
最终的测试结果需要与饿了么的质量平台对接（Macaca产生的测试报告、测试结果数据在导入饿了么质量平台前需要进行数据转换），形成完整的测试流程。

![](https://github.com/logan62334/ImageArchive/raw/master/Android/78.jpeg)

上面总结了一下自己在调研并选择UI自动化框架中的一些思考，希望能给处于UI自动化调研初期的同学们一些帮助，其中很多选择是出于自身业务的需要，仅供参考，希望大家能结合自身业务的需要，找到适合自己的UI自动化框架。另外如果有对此框架感兴趣的同学欢迎一起学习交流。

***
![全栈增长工程师，欢迎关注](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)