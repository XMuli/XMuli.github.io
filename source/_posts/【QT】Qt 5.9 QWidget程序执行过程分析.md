---
title: 【QT】Qt 5.9 QWidget程序执行过程分析
date: 2019-9-10 21:57:17
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - qt]
 - [学习 - 底层原理、思想架构]
tags: 
 - c/c++
 - qt
 - 原理、架构
---



**简介：**  讲述**QWidget**程序执行过程分析，以及变量**q**和**d**，以及函数**q_func**和**d_func**；和**QWidget**相关的类所有类的继承图

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介   <font color=#FE7207  size=4 face="幼圆">**附：** 本文图片是清晰大图，可能一开始会加载不出来，需要等待刷新一会</font>

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [【QT】Qt 5.9 QWidget程序执行过程分析](https://blog.csdn.net/qq_33154343/article/details/100714565) 

<br>

##  QWidget程序执行过程分析：

一个最简单的QWidget程序可能是下面这个样子：

```cpp
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);

    QWidget w;
    w.show();

    return a.exec();
}
```

首先是`QApplication`实例化，然后创建`QWidget`对象并`show`出来，最后通过`exec`进入事件循环，下面逐个分析这三个过程。

<br>

### 1、QApplication:

在Qt源码中，经常会看到变量q和d，以及函数q_func和d_func，这是Qt的架构风格，大多数类都对应地有一个私有类，例如QApplication的私有类为QApplicationPrivate，其中变量q和函数q_func是一个意思，均指的是QApplication对象指针，而变量d和函数d_func指的是QApplicationPrivate对象指针，QApplication的类层次关系如下图所示。
<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910220207.png"/>

`QApplication`实例化时，最主要的是加载`QPA（Qt Platform Abstraction）`插件，详细如下图所示。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910220307.png"/>

<br>

### 2、QWidget:

创建`QWidget`时，最重要的一点是保证在**GUI**主线程完成，以及`sendEvent`和`postEvent`这个两个函数，`QWidget`相关的所有类型如下图所示。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910220458.png"/>

`QWidget`继承自`QPaintDevice`，`QPaintDevice`是一个很重要的类，继承关系如下图所示。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910220535.png"/>

`QWidget`执行时序如下图所示。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910220635.png"/>

`QWidget`执行时序如下图所示。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910220713.png"/>

<br>

### 3、exec:

**exec**就是进入主事件循环，相关类为**QEventLoop**和**QAbstractEventDispatcher**，最终通过**poll**来处理事件，如下图所示。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910220745.png"/>

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>



转载原文:[【QT】Qt 5.9 QWidget程序执行过程分析](https://blog.csdn.net/iEearth/article/details/76895220) 

