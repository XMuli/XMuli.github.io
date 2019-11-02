---
title: git删除本地和远程的仓库分支
date: 2019-9-25 18:43:19
toc: true
categories: 
 - [学习 - git]
tags: 
 - git
---



**简介：** `git`删除本地分支和对应的远程的仓库分支

<!-- more -->

[TOC]

**编程环境：**  `deepin 15.11 x64 专业版 `    **Kernel：**  `x86_64 Linux 4.15.0-30deepin-generic`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [`git`删除本地和远程的仓库分支](https://blog.csdn.net/qq_33154343/article/details/101365411) 

## 目的和背景:

首先需要切换到其他分支,然后删除本地和远程`self`的**slider**分支

- 首先,查看本地(和远程的)所有仓库

```bash
git branch -a
```



- 删除本地**slider**仓库

```bash
git branch -d slider
```



- 查看所有远程仓库

```bash
git remote -v
```



- 可以看到此时远程`self`仓库的**slider**分支没有删除

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925184745.jpg"/>



- 删除远程`self`仓库的**slider**分支

```bash
git push self --delete slider
```



- 验证删除成功,依次查看本地和网站的仓库**slider**分支是否被删除

可以看到，是成功了的

本地的分支被删除：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925184816.jpg"/>

远程的分支也被删除

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925184910.jpg"/>



