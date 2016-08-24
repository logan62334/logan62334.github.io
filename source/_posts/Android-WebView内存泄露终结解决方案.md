---
title: WebView内存泄露终结解决方案
date: 2016-02-28 22:46:50
tags: webview
category: Android笔记
---
但凡是做Android开发的相信都对webview不会陌生，而且也对系统自带的webview本身存在的问题也是怨念很久了，一方面是本身对js的支持不是很好另外一方面就是经常被人诟病的内存泄露了。

不知道各位遇到同样问题的朋友是怎么解决的，网上也有很多解析和方案但至少在我的项目中是没任何效果的，今天我就分享一下我最终是怎么解决这些问题的（其实是很蠢的一个办法）。

需求背景：
需要一个带有加载进度条的webview来正常的显示合作方和自己的web页面。

1、解决webview对一些js的支持：

```
用JsBridge代替系统原生的webview，
github地址：https://github.com/lzyzsd/JsBridge
```
2、解决webview内存泄露：

```
@Bind(R.id.pb)
    ProgressBar pb;
    @Bind(R.id.mWebView)
    BridgeWebView mWebView;

    pb.setMax(100);
    mWebView.setWebChromeClient(new WebViewClient());
    mWebView.loadUrl(strWebsite);

    private class WebViewClient extends WebChromeClient {
        @Override
        public void onProgressChanged(WebView view, int newProgress) {
            try {
                pb.setProgress(newProgress);
                if (newProgress == 100) {
                    pb.setVisibility(View.GONE);
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
            super.onProgressChanged(view, newProgress);
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        try {
            if (mWebView != null) {
                mWebView.removeAllViews();
                mWebView.destroy();
                mWebView = null;
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```
最后介绍大家一个用来检测应用内存泄露的工具：leakcanary
![](https://github.com/logan62334/ImageArchive/raw/master/Android/5.jpg)
github地址：https://github.com/square/leakcanary
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
