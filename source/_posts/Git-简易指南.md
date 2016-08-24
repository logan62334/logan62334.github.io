---
title: 'Git简易指南'
date: 2016-03-01 22:48:52
tags: Git
category: Git
---
今天来分享一下我在使用Git的过程中经常用到的一些命令：

1、创建新仓库

```
git init
```
2、检出仓库

```
git clone username@host:/path/to/repository
```
3、添加与提交

```
git add *
git commit -m "代码提交信息"
```
4、推送改动

```
git push origin master
```
5、创建分支

```
git checkout -b feature_x
```
6、切换分支

```
git checkout master
```
7、删除本地分支

```
git branch -d feature_x

```
8、删除远程分支

```
git push origin :feature_x
```
9、将分支推送到远端仓库

```
git push origin <branch>
```
10、更新你的本地仓库至最新改动

```
git pull
```
11、合并其他分支到你的当前分支

```
git merge <branch>
```
***

![FullStackEngineer的公众号，更多分享](https://github.com/logan62334/ImageArchive/raw/master/weixin/weixin.jpg)
