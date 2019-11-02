---
title: error [Makefile1689 moc_ xxxx .cpp] 解决方法
date: 2019-9-12 21:37:09
toc: true
categories: 
 - [学习 - c/c++]
tags: 
 - c/c++
---



**简介：** **Qt**编译项目时候出现 的错误**error**信息

> error: [Makefile:1689: moc_ * .cpp] Error 1

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程环境：**  `deepin 15.11 x64 专业版 `    **Kernel：**  `x86_64 Linux 4.15.0-30deepin-generic`

**编程软件：**  `visual studio 2015`， `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`、

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [error: [Makefile:1689: moc_ * .cpp] Error 1 解决方法](https://blog.csdn.net/qq_33154343/article/details/100728451)  

<br>

## 产生原因：

创建一个新的类,需要使用信号于槽机制,添加`Q_OBJECT`宏:

**DMessageManager.h**

```cpp
#include "dmessagemanager.h"

DMessageManager::DMessageManager()
{
}
```



**DMessageManager.cpp**

```cpp
#ifndef DMESSAGEMANAGER_H
#define DMESSAGEMANAGER_H

#include <QObject>

class DMessageManager
{
    Q_OBJECT
public:
    DMessageManager();
};

#endif // DMESSAGEMANAGER_H
```

<br>

## 解决方法:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190912214619.png"/>

这可能是因为其他人提到的其他事情很少。我想补充另一个在这里丢失的。

如果您创建一个类并向其添加Q_OBJECT但不从QObject继承，您将获得“moc error 1"”。

如果你看一下编译输出，就会有一行说：
`Error: Class contains Q_OBJECT macro but does not inherit from QObject`

>    错误：类包含Q_OBJECT宏但不从QObject继承

因此，解决此问题的一般方法是查看“编译输出”窗口。

<br>

## 总结：

除了查看<font color=#FE7207  size=4 face="幼圆">问题警告</font>,还可查看<font color=#FE7207  size=4 face="幼圆">**编译输出**</font>,查看第一个标红的地方(或许会有惊喜)；

中文难以查询到满意的答案， 试试**stack overflow**这个网站；

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>

