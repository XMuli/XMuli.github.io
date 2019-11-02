---
title: error failed to push some refs to
date: 2019-9-25 00:18:18
toc: true
categories: 
 - [学习 - git]
tags: 
 - git
---



**简介：**  刚才使用`git`进行`push` 的时候，突然`push`不上去， 且提示错误：

> ! [rejected]        master -> master (non-fast-forward)
> error: failed to push some refs to 'git@github.com:  xxx / xxxx.git'



<!-- more -->

[TOC]

**编程环境：**  `win10 x64 专业版 1803`  

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [error: failed to push some refs to 'git@github.com:  xxx / xxxx.git'](https://blog.csdn.net/qq_33154343/article/details/101322324)

<br>

## 错误提示：

如图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925001029.png"/>

<br>

## 解决方法：

猜想是，可能远程仓库，比本地更新，所以像`pull`一下，在`push`；命令如下：

```bash
 git pull origin master --rebase  //拉取远程最新，和本地的仓库变基
 add... commit...
 git push 
```

`pull`之后期间使用 `git log`，确认一下，本地是否领先远程仓库，确保可以`push`；

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925001608.png"/>

如期：推送成功

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925001718.png"/>