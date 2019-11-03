---
title: 小技巧：设置`QToolBox`的每一页page的使用单独的布局（`QTabWidget`同理）
date: 2019-9-1 16:29:46
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt技巧
---



**简介：** 在使用QT的**Design**设计师的时候，当需要使用控件`QToolBox`时候，里面是有多页的时候，为每一页都设置成一个单独的布局方式。其中`QTabWidget`控件的方法一致。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [小技巧：设置`QToolBox`的每一页page的使用单独的布局](https://blog.csdn.net/qq_33154343/article/details/100185025) 

<br>

## 知识分享：

**小技巧：设置`QToolBox`的每一页page的使用单独的布局（`QTabWidget`同理）**

- 1、首先选中QToolBox控件
- 2、再在Design设计师（右侧具体的某一page_xxx）里面，点击选中，打开此page
- 3、<font color=#D0087E size=4 face="幼圆">**重要的一步：**</font> 先必须随意拖曳一个控件到这一page页里面（才能够进行下一步）
- 4、回到中间，鼠标右键单点击，选中 `Lay out`一栏， 里面选择其他具体的布局方式，如：**Lay out in a Grid**
- 5、继续设置其他page页面， 同上1~4步骤循环

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190901165542.png"/>

<br>

## 运行效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190901165612.png"/>

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>