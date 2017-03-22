---
title: '配置Android项目 - 版本名称和代码'
date: 2017-3-22 11:39:58
tags: Android架构
category: Android架构
---
### Version Name & Code

开发人员通常给android versionName＆versionCode使用一些硬编码值。

![42](https://github.com/logan62334/ImageArchive/raw/master/Android/42.png)

这种方法有几个缺点：

- 你永远不知道哪个提交代表一个特定的版本。
- 每当你增加versionCode和更改versionName，你必须修改build.gradle文件。

如果你使用git作为源代码控制系统，它也可以帮助你生成android versionName＆versionCode。常见的做法是使用git标签来指示新版本的发布。![43](https://github.com/logan62334/ImageArchive/raw/master/Android/43.png)

### Version Name

对于versionName，我们可以使用git describe命令。

> a.该命令查找从提交开始可访问的最近的标记。
>
> b.如果标记指向提交，则只显示标记。
>
> c.否则，它将标记名称与标记对象顶部的附加提交数以及最近提交的缩写对象名称作为后缀。

**Example (a-b)**

![44](https://github.com/logan62334/ImageArchive/raw/master/Android/44.png)

1. 使用tag1.0标记特定提交
2. 签出此提交
3. 调用git describe -tags
4. 输出：1.0

正如你看到的，如果你使用一些标签在一个HEAD提交上调用git describe，它会输出这个标签。

**Example (a-c)**

![45](https://github.com/logan62334/ImageArchive/raw/master/Android/45.png)

1. 标记tag为1.0的提交
2. 再添加两个提交
3. 调用git describe -tags
4. 输出：*1.0-2-gdca226a*

![46](https://github.com/logan62334/ImageArchive/raw/master/Android/46.png)

使用git提交哈希“1.0-2-gdca226a”，我们可以很容易地找出从哪个特定的提交构建。

![47](https://github.com/logan62334/ImageArchive/raw/master/Android/47.png)

### **Version Code**

对于versionCode，我们可以使用标签的总数。因为每个git标签指示一些版本，下一版本的versionCode将总是大于previous。

![48](https://github.com/logan62334/ImageArchive/raw/master/Android/48.png)

在上面的示例中，我们有3个标签。这个值将用于我们的versionCode。

但是我们不会为每个中间版本创建一个git标签，因此对于dev build我们可以使用HEAD提交的时间戳。

![49](https://github.com/logan62334/ImageArchive/raw/master/Android/49.png)

在上面的示例中，HEAD提交的时间戳等于1484407970（自UNIX纪元1970年1月1日00:00:00 UTC以来的秒数）。这个值将用于我们的versionCode。如果你想将它转换为人类可读的日期使用currentmillis.com网站。在我们的情况下，它是Sat 1月14日2017 15:32:50 UTC。

### *Groovy* way to use *git*

要使用git我建议使用一个称为grgit的库。创建具有以下内容的script-git-version.gradle文件：

![50](https://github.com/logan62334/ImageArchive/raw/master/Android/50.png)

将其应用于您的build.gradle文件：

![51](https://github.com/logan62334/ImageArchive/raw/master/Android/51.png)

要检查version name 和 code是否正确生成调用gradle任务./gradlew printVersion它给出类似的输出：

![52](https://github.com/logan62334/ImageArchive/raw/master/Android/52.png)

最后在build.gradle文件中使用gitVersionName，gitVersionCode和gitVersionCodeTime变量。

![53](https://github.com/logan62334/ImageArchive/raw/master/Android/53.png)

运行项目并验证应用版本。

![54](https://github.com/logan62334/ImageArchive/raw/master/Android/54.png)

这种方法的好处：

- 不需要修改build.gradle文件 -  versionCode和versionName是自动生成的。
- 你可以很容易地找出从哪个提交生成。

> 注：你可以尝试其他的方式来标记版本名：包括分支名称，时间戳等。

***
![全栈增长工程师，欢迎关注](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)