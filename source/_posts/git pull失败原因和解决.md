---
title: git pull失败原因和解决
date: 2019-9-2 22:11:56
toc: true
categories: 
 - [学习 - git]
 - [学习 - 编码规范，辅助技巧]
tags: 
 - git
---



**简介：**  使用`git pul`l出现**error**如下：其解决方法

>  error: Your local changes to the following files would be overwritten by merge:
>         xxx/xxx/xxx.java   Please, commit your changes or stash them before you can           merge.  Aborting

[TOC]

## 本博文的简述or解决问题？

​		`git`十分常用命令：团队协作中，开发过程中，会经常遇到使用的命令

<br>

**编程环境：**  `deepin 15.11 x64 专业版 `    Kernel： `x86_64 Linux 4.15.0-30deepin-generic`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [git pull失败原因和解决](https://blog.csdn.net/qq_33154343/article/details/100397800)

<br>

## git pull失败原因和解决：

`pull`更新代码，遇到了下面的问题：

>  error: Your local changes to the following files would be overwritten by merge:
>         xxx/xxx/xxx.java   Please, commit your changes or stash them before you can           merge.  Aborting

**解决方案：**

- 放弃本地修改，直接覆盖之

```bash
git reset --hard
git pull
```



- 或`stash`

```bash
git stash       //保存贮藏
git pull        //拉取
git stash pop   //弹出贮藏 （此时会将更新的代码和自己写的代码合并，可能会有冲突，需要手动解决冲突）
```

<br>

## 参考博文：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

参考：

[[git拉取的时候报冲突解决方法](https://www.cnblogs.com/xie-xiao-chao/p/9779179.html)](https://www.cnblogs.com/xie-xiao-chao/p/9779179.html) 

