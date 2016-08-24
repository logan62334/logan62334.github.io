---
title: '[Android] 防止连续点击打开两个重复页面的小技巧'
date: 2016-04-12 23:00:28
tags: Android笔记
category: Android笔记
---
>我们在开发APP的过程中经常会遇到在某些低端机或者在机器响应比较慢的情况下手抖连续点击某个页面（当然不排除有些人故意这么做）重复弹出好几个相同的页面，不过我发现微信这样的应用都没有做处理……但还是要分享一下我是怎么解决的。

#### 1、通过判断两次点击的时间间隔来防止重复点击
工具类：
```
 /**
 * Created by mafei on 15/12/8.
 */
public class NoDoubleClickUtils {
    private static long lastClickTime;
    private final static int SPACE_TIME = 500;

    public static void initLastClickTime() {
        lastClickTime = 0;
    }

    public synchronized static boolean isDoubleClick() {
        long currentTime = System.currentTimeMillis();
        boolean isClick2;
        if (currentTime - lastClickTime >
                SPACE_TIME) {
            isClick2 = false;
        } else {
            isClick2 = true;
        }
        lastClickTime = currentTime;
        return isClick2;
    }
}
```
使用方式：
```
/**
     * 点击事件
     */
    private View.OnClickListener logListener = new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            if (!NoDoubleClickUtils.isDoubleClick()) {
                事件响应方法
            }
        }
    };
```
#### 2、通过修改manifest中页面的launchMode属性改为单例模式
```
<!-- 市场网贷产品页 -->
        <activity
            android:name=".activity.market.CreditRecordActivity"
            android:launchMode="singleTask"
            android:screenOrientation="portrait" />
```
#### 3、利用RxBinding实现防重复点击
RxBinding 是 Jake Wharton 的一个开源库，它提供了一套在 Android 平台上的基于 RxJava 的 Binding API。
```
RxView.clickEvents(button)
    .throttleFirst(500, TimeUnit.MILLISECONDS)
    .subscribe(clickAction);
```
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
