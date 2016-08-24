---
title: '[Android] Facebook Redex 压缩优化Apk实践'
date: 2016-04-16 13:29:11
tags: Android redex
category: Android新技术实践
---
>最近Facebook 又放出了一个用于Android apk字节码优化的工具包——redex，经过redex的优化apk会变的体积更小，速度更快。至于原理大家可以到https://code.facebook.com/posts/1480969635539475/optimizing-android-bytecode-with-redex这个网站去看，今天我想分享一下具体的实践过程。

前两天刚看到FB放出的这个工具包就迫不及待的去尝试了下，结果一直报下面这个错误：
```
configure: error: Please install double-conversion library
```

但其实这些library都已经安装好了的，那天倒腾了好久也跟群里的朋友交流过，感觉应该是FB的一个小bug于是去github上提了issue，果然第二天得到了回应官方更新了使用说明。下面是我在Mac OS X上的实践过程：

1、首先需要你的Xcode安装了命令行工具：

```
xcode-select --install
```

2、利用homebrew安装依赖包：

```
brew install autoconf automake libtool python3 
brew install boost double-conversion gflags glog libevent openssl 
brew link openssl --force
```

3、通过Git将redex的源码checkout到电脑上：

```
git clone https://github.com/facebook/redex.git 
cd redex 
git submodule update --init
```

4、通过autoconf和make来构建redex：

```
autoreconf -ivf && ./configure && make 
sudo make install
```

在执行步骤四的时候就出现了问题：
```
configure: error: Please install google-gflags library 
configure: error: ./configure failed for third-party/folly/folly
```

于是我又提了issue，下面是跟沟通的过程：
![](https://github.com/logan62334/ImageArchive/raw/master/Android/16.png)按照他的方法执行下面的命令：

```
xcode-select --install 
sudo xcode-select --switch /Library/Developer/CommandLineTools/
```

再执行步骤四就OK了当然如果看到很多warn也不用担心，最终可以编译通过。
接下来就可以通过redex执行最后的优化命令了：

```
redex path/to/your.apk -o path/to/output.apk
```

不过这里又出现了个问题：
![](https://github.com/logan62334/ImageArchive/raw/master/Android/17.png)又是环境问题😂 ,下面是配置过程：
```
mafeideAir:~ mafei$ vi ~/.bash_profile 
export PATH=/Users/mafei/Development/adt-bundle-mac-x86_64-20140702/sdk/build-tools/23.0.2:$PATH
```

因为这个是系统只读文件所以退出的时候要输入!wq才行
这下执行下面的命令就完全没问题啦！

```
mafeideAir:~ mafei$ cd GitHub/
mafeideAir:GitHub mafei$ cd redex/
mafeideAir:redex mafei$ redex metis_release_v1.0.2.apk -o out.apk
```

刚刚又去看了下redex的官网发现FB已经把这几天遇到的一些典型问题都汇总了一下：
![](https://github.com/logan62334/ImageArchive/raw/master/Android/18.png)
***
![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
