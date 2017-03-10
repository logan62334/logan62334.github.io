---
title: 'Model-View-Presenter:Android指南'
date: 2017-3-10 11:39:58
tags: Android架构
category: Android架构
---
> 原文地址：https://medium.com/@cervonefrancesco/model-view-presenter-android-guidelines-94970b430ddf#.nqgbpr2bj

网上有很多关于MVP架构的文章和示例，并且有很多不同的实现。但开发者社区仍不断努力，想以尽可能最好的方式将此模式应用在Android上。

如果你决定采用这种模式，你正在做一个架构选择，你必须知道你的代码库将改变，以及你新的功能也要用新的方法来开发。另外你需要面对常见的Android问题如Activity生命周期，然后你还应该问问自己下面这些问题：

- 我应该保存*presenter*的状态吗？
- 我应该将*presenter*做持久化处理吗？
- *presenter*需要有生命周期吗？

在本文中，我将提供一系列准则或最佳做法，以便：

- 解决采用这个架构遇到的最常见问题（至少是一些我遇到过的）
- 发挥这个架构的最大优势

首先，让我们先解释一下这个模式：

![1](https://github.com/logan62334/ImageArchive/raw/master/Android/32.png)

- **Model**：它是负责管理数据的接口。模型的职责包括使用API，缓存数据，管理数据库等。该模型还可以是与负责这些职责的其他模块通信的接口。例如，如果你使用Repository模式，则模型可以是Repository。如果你使用的是Clean架构，那么Model可以是一个Interactor。
- **Presenter**：presenter是model和view的中间人。你的所有业务逻辑都应该放在这里面。presenter负责查询model和更新view，对更新模型的用户交互作出反应。
- **View**：它只负责以presenter定义的方式来显示数据。view可以被Activities、 Fragments、任何Android widget或者其他一些像显示ProgressBar、更新TextView、填充RecyclerView等等可执行操作的视图。



下面是以我的观点列出的一些指南，你可能不会全部赞同，不过我会试着解释为什么这么做。

### 1. 让View变得被动和无知

Android中最大的一个问题就是view（Activities、Fragments等）不是那么容易被测试因为Android框架很复杂。为了解决这个问题，你需要实现[**Passive View**](https://martinfowler.com/eaaDev/PassiveScreen.html)模式。这种实现方式通过利用一个controller来减少view的业务行为，在我们的例子中，这个controller是presenter。这种方式显著的提高的代码的可测试性。

例如，如果你有一个username/password的表单和一个提交按钮，你不需要在view中写验证逻辑而是将它写在presenter中。你的view只管接受用户名和密码的输入然后将他们传递给presenter即可。

### 2. 使presenter与框架无关

为了提高代码的可测试性，那么就要确保presenter不能依赖Android类文件。presenter用纯java代码实现的两个理由：首先你要将具体的实现抽象到presenter中，这样的话你就可以写不依赖于设备的测试代码了（甚至都不需要[Robolectric](http://robolectric.org/)），可以快速的在你的本地JVM中运行而不需要模拟器。

> 如果我需要用到Context呢?

那么就不要用它。在这种情况下，你应该问一下自己为什么需要context呢。我猜你可能想要存储数据或者获取资源。但是你不需要在presenter中做这些：你可以在view中获取资源，在model中存储数据。这里只是两个简单的例子，不过我敢打赌大多数情况下都是因为类的职责不明确导致的。

顺便说一下，依赖倒置原则可以帮助你在这种情况下解耦。

### 3. 写一个contract类来描述View和Presenter之间的交互

当你准备开始写一个新功能时，第一步最好先写一个contract类。contract描述了view和presenter之间的交互，它帮助你以更干净的方式设计交互。

我喜欢用Google在 [Android Architecture](https://github.com/googlesamples/android-architecture) repository中建议的解决方案：这个contract接口类中包含两个接口一个是view另一个是presenter。

让我们举个例子。

```
public interface SearchRepositoriesContract {
  interface View {
    void addResults(List<Repository> repos);
    void clearResults();
    void showContentLoading();
    void hideContentLoading();
    void showListLoading();
    void hideListLoading();
    void showContentError();
    void hideContentError();
    void showListError();
    void showEmptyResultsView();
    void hideEmptyResultsView();
  }
  interface Presenter extends BasePresenter<View> {
    void load();
    void loadMore();
    void queryChanged(String query);
    void repositoryClick(Repository repo);
  }
}
```

看到这个方法的名字，你应该就明白这个例子中的contract是干什么的了吧。

如果你还不知道，那一定是你的问题哈哈。

在这个例子中你可以看到view中定义的方法非常简单而且不包含任何逻辑。

#### The View contract

正如我之前说过的，view接口是要被Activity或者Fragment实现的。presenter必须依赖于view接口而不是直接依赖于Activity：通过这种方式，你可以将presenter从视图实现解耦，遵循SOLID原则的D：“依赖抽象，不要依赖具体实现）。

我们不需要更改presenter中的一行代码就可以替换具体的视图。因此我们可以非常容易的通过创建一个mock view来进行单元测试。

#### The presenter contract

> 等等，我们真的需要一个Presenter接口吗？

事实上不需要，但我认为还是要的。

关于这个话题有两种不同的思想流派。

一些人认为应该写一个Presenter接口因为你要将具体的presenter和view解耦。

然而另外一些开发者认为你在抽象的东西已经是一个抽象的了所以不需要再写一个接口了。另外不管怎么样，有了一个接口后可以帮你方便的写mock presenter，不过如果你采用了[Mockito](http://site.mockito.org/)这样的工具类那么你就不需要接口了。

我个人还是喜欢写这么一个**Presenter**接口的，下面是两个简单的理由：

- 我不是去为presenter写一个接口而是写一个Contract类来描述view和presenter之间的交互。
- 写这么个接口并不费什么力。

我已经这么写超过一年了甚至更长，至今没有发现什么问题。

### 4. 定义一个名称方便区分责任

presenter通常有两种类型的方法：

- **Actions**（e.g: load()）：presenter的一些行为操作。
- **User events**（e.g:queryChanged(…)）：用户触发的操作比如在搜索框中键入字符或者是点击列表中的某个选项。

你定义的action越多那么view中的逻辑也就越多。

当用户滚动到列表的结尾时将调用loadMore()方法，然后presenter加载另外一页的结果。这意味着当用户滚动到结尾时，view知道必须加载新页面。我可以命名方法onScrolledToEnd（）让具体的presenter处理具体做什么。

我想说的是，在“contract设计”阶段，你必须定义好每个用户事件，相应的action是什么，逻辑应该属于谁。

### 5. 不要在Presenter接口中创建Activity-lifecycle-style回调

我使用这个标题的意思是presenter不应该有像onCreate（...），onStart（），onResume（）等方法原因如下：

- 如果这么做了的话presenter将会和Activity产生耦合。如果我想用一个Fragment替换Activity怎么办？我什么时候应该调用presenter.onCreate（state）方法？在fragment的onCreate(…)、onCreateView(...)还是onViewCreated(…)中？如果我使用自定义view怎么办？
- presenter不应该有这么复杂的生命周期。事实上，主要的Android组件都是以这种方式设计的，但并不意味着你必须也这么做。如果你有机会可以简化的话那就简化它吧。

### 6. Presenter和view有1对1的关系

如果没有view的话presenter就没有意义了。presenter随着view一起被创建也随着view一起被销毁。一个presenter管理一个view。

你可以通过多种方式处理presenter中view的依赖。一种方式是在presenter接口中提供像attach(View view)和detach()的方法就像之前例子中展示的那样。不过这样做有一个问题就是你需要注意view是否为null，每次presenter用到它的时候都要检查一下是否为null。这点确实有点烦……

我说了presenter和view是一对一的关系。我们可以利用这一点，实际上具体的presenter可以将view实例作为构造函数的参数传入。顺便说一句，你可能需要一个方法来订阅presenter的一些事件。所以我建议定义一个方法start（）（或类似的方法）来运行Presenter中的业务。

> 关于*detach()*呢？

如果你有一个叫start（）的方法，那么你可能至少还需要一个来释放依赖的方法。既然我们定义订阅presenter一些事件的方法叫start（），那么另一个方法就叫stop（）吧。

```
public interface BasePresenter<V> {
  void attach(V view);
  void detach();
}
```

```
public interface BasePresesnter {
  void start();
  void stop();
}
```

### 7. 不要在presenter中保存状态

我的想着是要用Bundle来保存。但考虑到上面的第二条准则就不能这么做了。你不能将数据序列化到Bundle中，因为这样的话presenter就与Android类耦合了。

我说presenter应该是无状态的，但其实也不然。在我之前描述的例子中，presenter应该至少具有页码/偏移量之类的状态。

### 8. 不要持久化presenter

我不喜欢这种方式主要是因为我认为presenter不是我们应该持久化的，要清楚它不是一个数据类。

一些建议提供了一种在配置发生改变的时候通过恢复fragments或者 [Loaders](https://medium.com/@czyrux/presenter-surviving-orientation-changes-with-loaders-6da6d86ffbbf#.ii7px6adf)的方式记住presenter的状态。我不认为这是最好的解决方案。通过这种方式presenter可以在方向发生变化恢复，但是当Android杀死了进程并销毁Activity，后者将与新的presenter一起重新创建。因此，该解决方案仅解决了一半的问题。

### 9. 为Model提供缓存以恢复视图状态

在我看来，解决“恢复状态”问题需要一些应用架构的知识。基本上，作者建议使用类似Repository或任何旨在管理数据的接口来缓存网络结果，范围限定于应用程序而不是Activity。

这个接口只是一个更聪明的Model。后者应至少提供磁盘缓存策略和可能的内存缓存。这样的话，即使进程被杀，presenter也可以使用磁盘缓存恢复视图状态。

view应该只关心必要的请求参数以恢复状态。例如，在我们的示例中，我们只需要保存查询。

现在，你有两个选择：

- 你在model层中抽象这个行为，当presenter调用repository.get（params）时，如果页面已经在缓存中，数据源只返回它，否则再调用API。
- 在contract中的presenter添加一个方法来恢复视图状态。restore（params），loadFromCache（params）或reload（params）这些是描述相同动作的不同名称你可以随便选一个。

### 结论

以上是我对应用于Android的Model-View-Presenter架构的看法，希望通过不断的尝试可以找到最佳实践。
***
![全栈增长工程师，欢迎关注](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)