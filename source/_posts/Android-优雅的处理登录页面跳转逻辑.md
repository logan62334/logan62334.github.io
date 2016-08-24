---
title: '[Android] 优雅的处理登录页面跳转逻辑'
date: 2016-07-10 13:12:35
tags: Android笔记
category: Android笔记
---
>一般有用户系统的应用都会有以下两种需求：
1、在执行某个动作时需要判断当前用户是否登录，如果没有登录则跳转至登录页面，登录成功后返回原页面但不执行任何操作，如果已经登录则直接执行相应的操作。
2、在执行某个动作时需要判断当前用户是否登录，如果没有登录则跳转至登录页面，登录成功后返回原页面继续执行相应的操作，如果已经登录则直接执行相应的操作。
但往往一个应用中会有很多地方需要有这样的判断逻辑，所以直觉告诉我们应该把这一重复的处理逻辑封装一下：

##### 1、定义工具类LoginUtil.java
```
public class LoginUtil extends Activity {

  private int REQUEST_CODE_LOGIN = 1;
  static LoginCallback mCallback;

  public interface LoginCallback {
    void onLogin();
  }

  @Override protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Intent intent = new Intent(this, LoginActivity.class);
    startActivityForResult(intent, REQUEST_CODE_LOGIN);
  }

  public static void checkLogin(Context context, LoginCallback callback) {
    //此处检查当前的登录状态
    boolean login = AccountMgr.get().isLogin();
    if (login) {
      callback.onLogin();
    } else {
      mCallback = callback;
      Intent intent = new Intent(context, LoginUtil.class);
      context.startActivity(intent);
    }
  }

  public static void checkLogin(Context context, LoginCallback logged, LoginCallback callback) {
    //此处检查当前的登录状态
    boolean login = AccountMgr.get().isLogin();
    if (login) {
      logged.onLogin();
    } else {
      mCallback = callback;
      Intent intent = new Intent(context, LoginUtil.class);
      context.startActivity(intent);
    }
  }

  @Override protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    finish();
    if (requestCode == REQUEST_CODE_LOGIN && resultCode == RESULT_OK && mCallback != null) {
      mCallback.onLogin();
    }
    mCallback = null;
  }
}
```
##### 2、在AndroidManifest.xml里配置此activity的主题
```
<activity
  android:name="com.xxx.base.utils.LoginUtil"
  android:theme="@style/NoDisplay" />
```
注意：在values中创建主题：
```
<style name="NoDisplay" parent="android:Theme.NoDisplay"/>
```
在values-v23中创建适配主题：
```
<style name="NoDisplay" parent="android:Theme.Translucent.NoTitleBar"/>
```
##### 3、在登录页面成功登录后执行下面语句：
```
 @Subscribe(threadMode = ThreadMode.MAIN)
  public void onEventMainThread(AccountEvent.LoginEvent event) {
    setResult(Activity.RESULT_OK);
    finish();
  }
```
##### 4、在需要判断登录的地方直接调用下面两种重载方法即可：
```
LoginUtil.checkLogin(getActivity(), new LoginUtil.LoginCallback() {
      public void onLogin() {
        //已经登录和未登录状态下进行的操作
      }
    });
```

```
 LoginUtil.checkLogin(getActivity(), new LoginUtil.LoginCallback() {
      @Override public void onLogin() {
        //已经登录状态进行的操作
      }
    }, null);
```
***

![关注FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
