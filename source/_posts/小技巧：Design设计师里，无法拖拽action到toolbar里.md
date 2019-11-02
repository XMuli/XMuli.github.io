---
title: 小技巧：Design设计师里，无法拖拽action到toolbar里
date: 2019-8-31 00:19:20
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt
 - 技巧
---



**简介：**  在**Qt**的**Design**设计师里面，试图拖拽**action**到**toolbar**里面，却不管怎么移动都是一个红色○里面带个X：位禁止拖拽此处，无法拖拽成功。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**   `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [小技巧：Design设计师里，无法拖拽action到toolbar里Design设计师里，无法拖拽action到toolbar里](https://blog.csdn.net/qq_33154343/article/details/100168170)

<br>

## 知识分享：

**问题：**

试图拖拽**action**到**toolbar**里面，却不管怎么移动都是这个红色○里面带个X：禁止拖拽此处

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831000617.jpg"/>



在Design设计师里面，显示QToolBar工具栏非常小，不能拖拽下面的action到这里，进行可视化的添加；

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831000434.jpg"/>



**可行的方法：**

但是运行之后，想要得到的显示效果的一个方法（不算让我满意）；就是可以添加到QToolBar里面（这是使用代码添加方法）：`ui->mainToolBar->addAction(ui->actListInit);`

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831001448.jpg"/>



## 解决方法：

- <font color=#D0087E size=4 face="幼圆">**方法一：**</font> 仍然坚持暴力拖拽，屡次进行尝试，或许偶尔几次会成功

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190901193120.png"/>



- <font color=#D0087E size=4 face="幼圆">**方法二：**</font>先调整QToolBar的miniumSize的Height的大小，设置大一点，然后进行拖拽，会发现如此容易（下图红线中，是表示可以拖拽放在此处）

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190901193059.png"/>

<br>

## 运行效果：

- 运行成功演示

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831001314.jpg"/>

<br>

## 开心分享：

因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>