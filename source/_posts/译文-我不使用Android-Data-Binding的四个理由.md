---
title: '[译文] 我不使用Android Data Binding的四个理由'
date: 2016-08-14 19:06:56
tags: Android译文
category: Android译文
---
为什么我还在用ButterKnife。

>免责声明：本文是基于个人经验和实践可以随意反驳，是否采纳自行决定。

![](https://github.com/logan62334/ImageArchive/raw/master/Android/22.png)

**1、专家不建议这么做**
ButterKnife的作者Jake在下面这个github issue中直指要点。
>data binding在最简单的场景下是比较有用的。但它并没有什么创新，所以在复杂度增加的情况下还是会像其他平台上的解决方案一样用起来非常痛苦（例如：XAML）。当这个库扩展到高级的情况下，将会迫使你把绑定的逻辑写到代码中，那里才是它真正该在的地方。

事实上，我同意其中的两点：

1、它的扩展性并不好。
2、业务逻辑应该在代码中。

**2、它让你写出意大利面式的代码**
一旦我们开始实现复杂的布局，将会使我们的Data Binding解决方案越来越复杂。

首先我们将会面临下面的问题：

1、Layout 要求你给他们分别传递数据。

2、你也可能想为你的布局创建不同的数据源。

3、同样的问题也会在ViewStubs中发生。

4、当你使用Picasso加载图片的时候，你需要为他实现一个自定义的data binding adapter，那样的话你就不能作为依赖mock和注入了。

我们可能会试着做些更复杂的事情：

1、在layout中增加presentation的逻辑。
```
<TextView
   android:text="@{user.lastName}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:visibility="@{user.isAdult ? View.VISIBLE : View.GONE}"/>
```
2、在listeners中增加Lambda表达式。
```
android:onLongClick="@{(theView) -> presenter.onLongClick(theView, task)}"
```
3、在layouts中使用导入的class类。
```
<data>
    <import type="android.view.View"/>
</data>
```
我们的逻辑一部分在代码中一部分在布局文件中，这将很快变成一个噩梦，并且闻起来像是意大利面式的代码。

**3、单元测试也不能用了**
我非常喜欢Robolectric和Mockito，他们节约了我很多时间在创建和运行测试实例的时候，没有了他们我将无法工作。

Data Binding的一个特性对于我来说是一个bug：如果layout发生了异步更新，那就意味着在我设置了绑定之后单元测试中我无法确定view上的数据是否正确。

我记得google用Espresso实现的测试框架，但如果有可能的话我还是希望用单元测试的方式来测UI。我喜欢将Activities、Fragments和Views分开来测试而不是在一个大的Instrumentation Test中导入他们。

**4、它比ButterKnife提供的功能少很多**
ButterKnife提供了很多不错的特性，可能有些我们都不记得了：

1、资源绑定

2、View 列表

3、多个监听器的绑定

当我们用自定义控件做一些高级的实现的时候，资源绑定是非常有用的，我们可以通过它获取到Dimensions和Drawables。

当我们有一系列的视图触发同样的操作的时候，多视图绑定和多监听器绑定会让我们少写很多代码，例如：一系列的EditText和Buttons。

而如果你使用Data Binding库将得不到这些功能。

**为什么你会使用Data Binding**

**1、我可以开发的更快**
长远来看，快速并不一定总是好的。当我们开发app的时候，我们是在跑一场马拉松而不是一次百米冲刺……不是吗？

**2、它已经存在于系统sdk中**
不需要引入第三方库总归是好事情。如果你被调入到一个已经出现了方法数快超过限制的项目中时，你的leader将不希望你再引入过多的第三方库。

**3、我在遵循MVVM的模式**
如果你正确的利用观察者模式实现了MVVM，Data Binding库将会帮助你在views中实现观察者模式。

谢谢你看了这么长时间！
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
