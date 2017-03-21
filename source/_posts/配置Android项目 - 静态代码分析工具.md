---
title: '配置Android项目 - 一些重要的事情'
date: 2017-3-21 11:39:58
tags: Android架构
category: Android架构
---
### 静态代码分析工具

静态代码分析工具 - 分析代码而不执行它。通常用于发现错误或确保符合编码指南。有助于保持你的代码健康，并保持代码质量。

在Android上，最流行的代码分析工具是：

- Lint
- PMD
- Findbugs

我通常将静态代码分析脚本和相关文件保存在单独的文件夹中。

### Lint

> lint工具检查你的Android项目源文件是否存在潜在错误，并针对正确性，安全性，性能，可用性，可访问性和国际化进行优化改进。

#### 配置

添加lint到你的android项目创建script-lint.gradle文件。![55](https://github.com/logan62334/ImageArchive/raw/master/Android/55.png)

重要的lint选项：

- *lintConfig* —lint规则集的路径（可以用来配置压制警告）。
- *htmlOutput* —html报告生成的地方。

将script-lint.gradle导入到build.gradle文件。

![56](https://github.com/logan62334/ImageArchive/raw/master/Android/56.png)

#### 测试

重新构建你的项目，然后使用./gradlew lint命令运行lint。如果它发现一些问题，你会看到类似下面的输出。

![57](https://github.com/logan62334/ImageArchive/raw/master/Android/57.png)

当你打开lint.html报告文件时，你将看到问题列表描述，和如何解决它们的建议。

![58](https://github.com/logan62334/ImageArchive/raw/master/Android/58.png)

如果你想忽略此问题，请将以下规则添加到rules-lint.xml文件中。

![59](https://github.com/logan62334/ImageArchive/raw/master/Android/59.png)

注意：还有其他方法可以压制lint警告。有关lint的更多信息，请访问官方网站。

### **Findbugs**

> 静态代码分析工具，用于分析Java字节码并检测各种各样的问题。

#### 配置

要添加findbug到你的android项目需要创建script-findbugs.gradle文件。

![60](https://github.com/logan62334/ImageArchive/raw/master/Android/60.png)

重要的findbugs选项：

- *excludeFilter* —findbugs规则集文件所在的路径，你可以在其中压制问题。
- *classes* — 生成的类的路径（如果你有多个*flavor*，路径由*flavor*名称组成，在当前情况下为“dev”）。
- *source* —源代码的路径
- *html.destination* —html报告生成的路径

将script-findbugs.gradle导入到build.gradle文件。

![61](https://github.com/logan62334/ImageArchive/raw/master/Android/61.png)

#### 测试

为了测试，我们将创建以下方法。

![62](https://github.com/logan62334/ImageArchive/raw/master/Android/62.png)

重新构建你的项目，然后运行findbugs ./gradlew findbugs命令。如果它发现一些问题，你会看到类似下面的输出。

![63](https://github.com/logan62334/ImageArchive/raw/master/Android/63.png)

当你打开findbugs.html报告文件，你将看到问题列表与说明和如何解决它们的建议。

![64](https://github.com/logan62334/ImageArchive/raw/master/Android/64.png)

如果你想忽略此问题，请将以下规则添加到rules-findbugs.xml文件中。

![65](https://github.com/logan62334/ImageArchive/raw/master/Android/65.png)

注意：还有其他方法去压制findbugs警告。有关findbugs的更多信息，请访问官方网站。

### PMD

> PMD是一个源代码分析器。它发现常见的编程缺陷，如未使用的变量，空catch块，不必要的对象创建等等。

#### 配置

要添加pmd到你的android项目那么需要创建script-pmd.gradle文件。

![66](https://github.com/logan62334/ImageArchive/raw/master/Android/66.png)

重要的pmd选项：

- *ruleSetFiles* —pmd规则集文件的路径，你可以在其中压制问题并定义要跟踪的问题。
- *source* —源代码的路径
- *html.destination* —html报告生成的路径

将脚本*script-pmd.gradle*导入到build.gradle文件。

![67](https://github.com/logan62334/ImageArchive/raw/master/Android/67.png)

#### 测试

为了测试，我们将创建以下方法。

![68](https://github.com/logan62334/ImageArchive/raw/master/Android/68.png)

重新构建你的项目，然后使用./gradlew pmd命令运行pmd。如果它发现一些问题，你会看到类似下面的输出。

![69](https://github.com/logan62334/ImageArchive/raw/master/Android/69.png)

当你打开pmd.html报告文件，你将看到问题列表与说明和如何解决它们的建议。

![70](https://github.com/logan62334/ImageArchive/raw/master/Android/70.png)

如果你想忽略此问题，请将以下规则添加到rules-pmd.xml文件中。

![71](https://github.com/logan62334/ImageArchive/raw/master/Android/71.png)

注意：还有其他方法压制pmd警告。有关pmd的更多信息，请访问官方网站。

***
![全栈增长工程师，欢迎关注](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)