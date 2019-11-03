---
title: git pull --rebase讲解
date: 2019-9-2 22:12:24
toc: true
categories: 
 - [学习 - git]
 - [学习 - 编码规范，辅助技巧]
---



**简介：** `git pull --rebase`讲解

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `deepin 15.11 x64 专业版 `    Kernel： `x86_64 Linux 4.15.0-30deepin-generic`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [git pull --rebase讲解](https://blog.csdn.net/qq_33154343/article/details/100398247) 

<br>

## git pull --rebase讲解：

```cpp
git pull = git fetch && git merge
git pull --rebase = git fetch && git rebase
```

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190808220109.png"/>



- C的基础上开发到D，小明在C的基础上开发到E，这个时候要把E合并到`origin`，两种办法:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190902213450.png"/>

```bash
如果有冲突, 解决冲突
git add .
不需要commit
git rebase --continue
git push 到远端
```

<br>

## 参考博文：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

参考：

[git pull --rebase origin master](https://www.jianshu.com/p/0cd05dd1cc73)

