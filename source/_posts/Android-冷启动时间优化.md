---
title: Android冷启动时间优化
date: 2016-03-03 13:12:49
tags: Android 冷启动 启动黑屏
category: Android新技术实践
---
冷启动时间是指当用户点击你的app那一刻到系统调用Activity.onCreate()之间的时间段。在这个时间段内，WindowManager会先加载app主题样式中的windowBackground做为app的预览元素，然后再真正去加载activity的layout布局，而通常情况下这个默认背景是黑色或者白色所以如果不加以优化会让用户感觉到app很卡很慢。

知道了Android冷启动时间的原理之后，就可以通过一些小技巧来对冷启动时间进行优化，从而让你app加载变得”快“一些（视觉体验上的快）。我是通过使用app闪屏页的图片来做为windowBackground这样可以传达企业的形象。

1、为启动的Activity自定义一个Theme

```
<style name="AppTheme.Launcher">
    <item name="android:windowBackground">@drawable/window_background</item>
</style>
```

2、将新的Theme应用到设置到 AndroidManifest.xml 中

```
<activity
  android:name=".MainActivity"
  android:theme="@style/AppTheme.Launcher">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

3、由于给MainActivity设置了一个新的Theme，这样做会覆盖原来的Theme，所以在MainActivity中需要设置回原来的Theme

```
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
  // Make sure this line comes before calling super.onCreate().
  setTheme(R.style.AppTheme);
  super.onCreate(savedInstanceState);
    }
}
```

最后推荐大家一个开源项目也是用来实现冷启动优化的不过是MaterialDesign风格的：https://github.com/DreaminginCodeZH/MaterialColdStart
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
