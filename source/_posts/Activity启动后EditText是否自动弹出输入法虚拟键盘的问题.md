---
title: Activity启动后EditText是否自动弹出输入法虚拟键盘的问题
date: 2016-02-27 17:54:53
tags: Android布局UI
category: Android笔记
---
在开发过程中，我们经常会遇到Activity中包含EditText控件时会自动弹出虚拟键盘的情况，这是由于EditText自动获得焦点的缘故，只要让EditText失去焦点就行了，解决办法如下：

1、在Manifest.xml文件中相应的Activity下添加如下代码：

```
android:windowSoftInputMode="stateHidden"
```
2、让EditText失去焦点，用EditText的clearFocus：

```
EditText edt = (EditText)findViewById(R.id.edt);
edt.clearFocus();
```
3、强制隐藏Android输入法窗口：

```
EditText edt = (EditText)findViewById(R.id.edt);  
InputMethodManager imm = (InputMethodManager)getSystemService(INPUT_METHOD_SERVICE);  
imm.hideSoftInputFromWindow(edt.getWindowToken(), 0);  
```
4、要求EditText始终不弹出虚拟键盘：

```
EditText edt = (EditText)findViewById(R.id.edt);  
edt.setInputType(InputType.TYPE_NULL);  
```

但有时我们确实是想让EditText自动获得焦点并弹出软键盘，在设置了EditText自动获得焦点后，软件盘不会弹出。
注意：此时是由于刚跳到一个新的界面，界面未加载完全而无法弹出软键盘。此时应该适当的延迟弹出软键盘，如500毫秒（保证界面的数据加载完成，如果500毫秒仍未弹出，则延长至1000毫秒）。

1、可以在EditText后面加上一段代码：

```
Timer timer = new Timer();  
timer.schedule(new TimerTask() {  

    public void run() {  
        InputMethodManager inputManager = (InputMethodManager) editText.getContext().getSystemService(Context.INPUT_METHOD_SERVICE);  
        inputManager.showSoftInput(editText, 0);  
    }  

}, 500);  
```
2、给activity配置加入属性：

```
android:windowSoftInputMode="adjustResize"
```
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
