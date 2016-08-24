---
title: '[译文] Android开发最佳实践'
date: 2016-08-06 10:53:33
tags: Android译文
category: Android译文
---
>你的软件开发效率不仅仅取决于你有多么深的知识和经验，还依赖于你的开发工具、环境配置和团队协作能力。

我最近在 [Droidcon Berlin](http://droidcon.de/en/sessions/effective-android-development)做了一个关于在我们Zalando Tech team的一个Android开发者最佳实践的分享。接下来你可以找到我演讲中的一些要点，将使你的开发生活更加愉快并且让你的应用更加稳定。


![](https://github.com/logan62334/ImageArchive/raw/master/Android/21.jpeg)

#### 1、你的AndroidManifest文件真实的样子是什么
我们大多数人早已知道我们在文本编辑器中看到的*AndroidManifest.xml* 文件跟实际编译好生成的的内容并不一样。这主要是因为你在工程中包含的第三方库中存在额外的<*uses-permission/*>标签，将和你自己的manifest文件混合。（[The Commons Blog](https://commonsware.com/blog/2015/06/25/hey-where-did-these-permissions-come-from.html)查看更详细的信息）

在构建APK前检查你的manifest，我们可以用Android Studio 2.2中提供的新特性：[Merged Manifest Viewer](http://android-developers.blogspot.de/2016/05/android-studio-22-preview-new-ui.html)。这个工具将会给你展示你的AndroidManifest是如何基于build types, flavors, and variants这些配置和你的依赖包merge的。你可以导航到*AndroidManifest.xml*文件点击底部的Merged Manifest标签使用这个工具。

![](https://github.com/logan62334/ImageArchive/raw/master/Android/19.png)

#### 2、Support annotations是你的朋友
另外一个特别有用的工具是support annotations library。你可以通过在你的build.gradle文件中添加“*com.android.support:support-annotations:23.4.0*” 来包含入你的工程。使用这些metadata annotations去修饰你的代码帮助你发现bug定义编码规则。最常规的使用场景是标记nullable和non-nullable，链接资源，并且指定从哪个线程被回调。

既然这些annotations是metadata annotations，所以即使你违反了它定义的语法规则你的工程也会编译。然而它会被Android Studio 和 Lint标记为高亮，并且在你的持续集成工具输出中对你的团队成员是可见的。

#### 3、快速、无痛的code review
假设你想要去做一个code review，检查开发feature是如何工作的是有意义的，所以你需要去编译你的工程。这种情况有下面这样的一种通用工作流：

1.在你当前分支Stash changes
2.Checkout branch来做review
3.在你的IDE中重新加载gradle配置
4.在IDE中读代码
5.编译启动并且测试app
6.重复 (1) — (5)的动作

“这里有什么问题吗？”你会问。是的，除了下面这种情况之外没什么问题，当你的工程有1000+的类和配置文件不同的时候你将会用你强劲的MacBook花超过三分钟用在编译你的代码上。

我们的解决方案是使用一个专用的IDE实例和存储库文件夹来做code review。在这种情况下你的工作在一段时间内不会停止，你可以在任何时候回到你的主IDE和分支。这里有个小的免责声明：我们建议你使用一台至少有16GB RAM的机器，它为你节省的时间绝对非常值。

#### 4、快速的应用修改
即使你的工程很小，你会经常花时间在编译部署最新的变化到测试机和模拟器上。如果你有上百个类和xml文件，每次编译和部署会花去你很多时间，即使你用配置很高的电脑。此外，你需要手动定位到应用变化的地方，这也需要一些时间。

在2015年底，Android社区收到两个可以让代码变化应用得更快的工具。第一个是[JRebel](https://zeroturnaround.com/software/jrebel-for-android/)，它是来自Java后端世界并且已经成为很长一段时间的行业标准。另外一个工具便是google在Android Studio 2.0中发布的[Instant Run](https://developer.android.com/studio/run/index.html#instant-run)，这些工具都有共同的目的，但是JRebel有更多的特性，并且需要按年支付许可证。

这两个工具单看看不出有什么不同，于是我们通过文档和一些博客分析了他们的不同：

![](https://github.com/logan62334/ImageArchive/raw/master/Android/20.png)

来源：
[https://developer.android.com/studio/run/index.html#instant-run](https://developer.android.com/studio/run/index.html)
*Reto Meier:* [“Instant Run: How Does it Work?!”](https://goo.gl/mEP7N5)
*Oleg Selajev:* [“Looking at JRebel for Android and Instant Run …”](https://goo.gl/NvFHpN)

几乎每周这两个工具都在积极的开发和改进。根据我们的经验，虽然很多用例还没有涉及，但你已经可以受益于这些工具了。

#### 5、衡量执行时间
另一个非常有用的特性是在应用程序调试和性能分析日志记录方法输入/输出和执行时间。对于这些需求，我们使用一个简单的和优雅的方法注释工具——Hugo by Jake Wharton。如果你仅仅只是想看日志输出并不需要像Systrace这样深度和复杂的工具。

你需要做的只是去标注目标方法，如图所示：
```
@DebugLog
public String getName(String first, String last) {/* ... */}
```
在日志中找到相应的方法调用的打印信息：
```
V/Example: --> getName(first="Jake", last="Wharton")
V/Example: <-- getName [16ms] = "Jake Wharton"
```
#### 6、如何从你的设备上读取日志输出信息
为了日常的需要，我们大多数人使用Android Studio内置的Monitor来读取日志。在简单的场景下它使用很方便，但我们注意到几个权衡使用这种方法:

1.日志是难以阅读,你应该使用外部工具或格式化配置。

2.Android Studio的日志工具是和你部署的应用程序的进程ID关联的。如果你重新部署应用程序或杀死进程,你以前的日志都会丢失,因为Android Studio是跟进程ID绑定的。

为了解决这个问题我们可以使用Jake Wharton — [pidcat](https://github.com/JakeWharton/pidcat)。它的主要好处如下：

1.好颜色模式和格式。

2.由包名称连接到调试应用程序,而不是进程ID。所有的日志都将被保持在重新部署应用程序后。

#### 7、网络输出日志记录和分析
最常见的阅读你应用网路交互日志输出的方式是通过HTTP客户端。然而,这种方法有几个权衡:

1.如果你要在开发过程中保持所有的网络请求日志，你将注意到,应用程序的性能有所下降,需要一些时间来打印日志。

2.如果你的应用有一些额外的库需要用到网络，（例如：Google Analytics）你需要为这些额外的库做一些配置  来让所有的数据被记录。

这里有另外一种方法：使用HTTP监控软件 [Charles Proxy](https://www.charlesproxy.com/)，这种类型的工具会提供如下的功能，把你的应用包装为黑盒：

1.HTTP/HTTPS通道的监控和记录

2.重新修改返回值和服务器响应的边界情况

3.在网络回调的地方打断点

4.将SSL证书安装到设备读取加密流量

更新，评论中* *[*Nahuel Barrios*](https://medium.com/u/8bb3d456205a)提到的另外一个可以替代的网络监控工具是* *[*Facebook Stetho*](http://facebook.github.io/stetho/)我们仍然不能通过*Stetho*读取*Google Analytics*中的SSL加密的通信信息，如果你知道任何关于这个的信息请联系我。

#### 8、保证在不同版本的操作系统上测试
我一直在做并且推动我同事做的事情是在Lollipop和更高的(API 21+)的版本中测试每一个功能。这样我可以在测试期间捕获这些bug。

你会发现通常的兼容问题都是些触摸反馈和系统颜色的不一致，我们经常看到app由于兼容问题在老的API中崩溃。

#### 9、自动化屏幕交互测试
我们经常需要检查一些场景在不同设备上做重复的UI点击和输入。如果你有三到四台测试设备那将是非常烦人的，你需要一个回归测试计划去测试通过30各场景。

自动化的第一步，我们通过输入adb命令或者脚本去实现从而避免每次都要手动和设备交互。你可以通过系统按键、键盘输入和屏幕触控来实现adb命令的输入。

但是如果你有三台设备要同时测试一个场景，你会怎么做？我们可以使用[adb-ninja](https://github.com/romannurik/env/blob/master/bin/ninja-adb)来让不同设备同时测试。

#### 10、检查你的build.gradle配置
有时甚至一些有经验的开发者也会使用一些过时的配置实践。让我们检查一下你的build.gradle文件，看看你有没有中招：

1.舍弃mavenCentral，去用jcenter作为依赖仓库。jcenter拥有更快的响应时间并且已经集成了mavenCental的内容。

2.检查*Android Plugin for Gradle*的版本，保持最新的版本可以提高编译性能并且会得到像Instant Run这样好用的功能。

3.不要指定依赖库的版本范围，要使用像“23.4.0”这样的版本常量，减少每次编译依赖库时的网络请求。

4.设置编译版本minSdkVersion 21或者更高，将会提高构建速度。
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
