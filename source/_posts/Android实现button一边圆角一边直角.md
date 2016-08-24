---
title: Android实现button一边圆角一边直角
date: 2014-12-22 10:04:02
tags: Android布局UI
category: Android笔记
---
Android中要实现如下图的效果：
![](http://img.blog.csdn.net/20150317122255569?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDQ2MTY1OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
**这个要在真机中才能看出效果！！**
switch_button_left_checked.xml

    <?xml version="1.0" encoding="utf-8"?>
    <shape xmlns:android="http://schemas.android.com/apk/res/android"
        android:shape="rectangle" >

        <!-- 填充的颜色：这里设置背景透明 -->
        <solid android:color="#ff304a" />
        <!-- 边框的颜色 ：不能和窗口背景色一样 -->
        <stroke
            android:width="2dp"
            android:color="#ff304a" />
        <!-- 设置按钮的四个角为弧形 -->
        <!-- android:radius 弧形的半径 -->
        <corners
            android:bottomLeftRadius="5dip"
            android:bottomRightRadius="0dip"
            android:topLeftRadius="5dip"
            android:topRightRadius="0dip" />

        <!-- padding：Button里面的文字与Button边界的间隔 -->
        <padding
            android:bottom="5dp"
            android:left="5dp"
            android:right="5dp"
            android:top="5dp" />

    </shape>

switch_button_left.xml

    <?xml version="1.0" encoding="utf-8"?>
    <shape xmlns:android="http://schemas.android.com/apk/res/android"
        android:shape="rectangle" >

        <!-- 填充的颜色：这里设置背景透明 -->
        <solid android:color="#00000000" />
        <!-- 边框的颜色 ：不能和窗口背景色一样 -->
        <stroke
            android:width="2dp"
            android:color="#ff304a" />
        <!-- 设置按钮的四个角为弧形 -->
        <!-- android:radius 弧形的半径 -->
        <corners
            android:bottomLeftRadius="5dip"
            android:bottomRightRadius="0dip"
            android:topLeftRadius="5dip"
            android:topRightRadius="0dip" />

        <!-- padding：Button里面的文字与Button边界的间隔 -->
        <padding
            android:bottom="5dp"
            android:left="5dp"
            android:right="5dp"
            android:top="5dp" />

    </shape>

switch_button_right_checked.xml

    <?xml version="1.0" encoding="utf-8"?>
    <shape xmlns:android="http://schemas.android.com/apk/res/android"
        android:shape="rectangle" >

        <!-- 填充的颜色：这里设置背景透明 -->
        <solid android:color="#ff304a" />
        <!-- 边框的颜色 ：不能和窗口背景色一样 -->
        <stroke
            android:width="2dp"
            android:color="#ff304a" />
        <!-- 设置按钮的四个角为弧形 -->
        <!-- android:radius 弧形的半径 -->
        <corners
            android:bottomLeftRadius="0dip"
            android:bottomRightRadius="5dip"
            android:topLeftRadius="0dip"
            android:topRightRadius="5dip" />

        <!-- padding：Button里面的文字与Button边界的间隔 -->
        <padding
            android:bottom="5dp"
            android:left="5dp"
            android:right="5dp"
            android:top="5dp" />

    </shape>

switch_button_right.xml

    <?xml version="1.0" encoding="utf-8"?>
    <shape xmlns:android="http://schemas.android.com/apk/res/android"
        android:shape="rectangle" >

        <!-- 填充的颜色：这里设置背景透明 -->
        <solid android:color="#00000000" />
        <!-- 边框的颜色 ：不能和窗口背景色一样 -->
        <stroke
            android:width="2dp"
            android:color="#ff304a" />
        <!-- 设置按钮的四个角为弧形 -->
        <!-- android:radius 弧形的半径 -->
        <corners
            android:bottomLeftRadius="0dip"
            android:bottomRightRadius="5dip"
            android:topLeftRadius="0dip"
            android:topRightRadius="5dip" />

        <!-- padding：Button里面的文字与Button边界的间隔 -->
        <padding
            android:bottom="5dp"
            android:left="5dp"
            android:right="5dp"
            android:top="5dp" />

    </shape>

button.xml

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal"
        android:padding="10dp" >

        <Button
            android:id="@+id/wangdaileiBtn"
            android:layout_width="0dp"
            android:layout_height="40dp"
            android:layout_weight="1"
            android:scaleType="fitXY"
            android:text=""
            android:background="@drawable/switch_button_left_checked" />

        <Button
            android:id="@+id/baobaoleiBtn"
            android:layout_width="0dp"
            android:layout_height="40dp"
            android:layout_weight="1"
            android:text=""
            android:scaleType="fitXY"
            android:background="@drawable/switch_button_right" />

    </LinearLayout>
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
