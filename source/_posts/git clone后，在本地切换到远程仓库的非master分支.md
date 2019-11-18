---
title: git clone后，在本地切换到远程仓库的非master分支
date: 2019-11-06 21:09:00
toc: true
categories: 
 - [学习 - git]
---



**简  述：** 因为有一个需求， 在`github`上面的仓库有两个分支（`master`和`markdown`），且两个分支的内容完全不相同，现在需要另外一台电脑上面，克隆下来本地仓库，且同时能够在两个分支上面都进行不同的开发

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [git clone后，在本地切换到远程仓库的非master分支](https://blog.csdn.net/qq_33154343/article/details/102943685)

<br>

## 下面为自己的仓库为示范：

* `git clone`远程仓库拷贝到本地仓库：

```bash
git clone git@github.com:touwoyimuli/touwoyimuli.github.io.git
```

执行的时候， 其实是将**touwoyimuli.github.io仓库**的所有分支全部都拷贝下来的

* 查看本地所有分支`git branch -a`，默认是`本地master`+`远程origin: master `+`远程origin: 非master分支等 `

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/2019-11-06_10-39_mark.png"/>

* 切换到本地新建的分支`markdown`（也可以为其他名）；`markdown`为本地新分支，后面对应远程的origin/markdown为远程仓库的非master分支

```bash
git checkout -b markdown origin/markdown   
```

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/2019-11-06_10-41_mark.png"/>