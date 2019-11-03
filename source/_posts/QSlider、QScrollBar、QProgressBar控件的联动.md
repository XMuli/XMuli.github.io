---
title: QSlider、QScrollBar、QProgressBar控件的联动
date: 2019-9-19 00:04:24
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt控件
---



**简介：**  `QSlider`移动条、`QScrollBar`滚动条、`QProgressBar`进度条控件的联动，讲解和的使用

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [QSlider、QScrollBar、QProgressBar控件的联动](https://blog.csdn.net/qq_33154343/article/details/101003081) 

<br>

## 系列博文：

**比较紧密的关联博文参考：**

- [滑动条`QSlider`和`QAbstractSlider`的介绍和用法](https://touwoyimuli.github.io/2019/09/18/%E6%BB%91%E5%8A%A8%E6%9D%A1QSlider%E5%92%8CQAbstractSlider%E7%9A%84%E4%BB%8B%E7%BB%8D%E5%92%8C%E7%94%A8%E6%B3%95/)
- [`QSlider`、`QScrollBar`、`QProgressBar`控件的联动](https://touwoyimuli.github.io/2019/09/19/QSlider%E3%80%81QScrollBar%E3%80%81QProgressBar%E6%8E%A7%E4%BB%B6%E7%9A%84%E8%81%94%E5%8A%A8/)
- [仪表盘`QSlider`的讲解和使用](https://touwoyimuli.github.io/2019/09/19/%E4%BB%AA%E8%A1%A8%E7%9B%98QSlider%E5%92%8C%E6%95%B0%E5%80%BC%E6%98%BE%E7%A4%BAQLCD_NUmber%E7%9A%84%E8%AE%B2%E8%A7%A3%E5%92%8C%E4%BD%BF%E7%94%A8/)

<br>

## 运行效果：

先放一张运行效果

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190918191905.gif"/>

<br>

## 继承关系：

`QSlider`移动条、`QScrollBar`滚动条、进度条这三个控件，都是继承于`QAbstractSlider`类，其中关于**QSlider和QAbstractSlider属性**讲解，参考已经发过的文章[https://blog.csdn.net/qq_33154343/article/details/100944831](https://blog.csdn.net/qq_33154343/article/details/100944831) ；关于这几个控件的继承关系如下图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190918192923.png"/>

<br>

## 滚动条QScrollBar：

- QScrollBar 从QAbstractSlider继承而来的，具有QAbstractSlider的基本属性，没有专有属性。

<br>

## 进度条QProgressBar：

- QProgressBar的父类是QWidget，一般用于进度显示，常用属性如下。

|    属性     |                             含义                             |
| :---------: | :----------------------------------------------------------: |
|   minimum   |                            最小值                            |
|   maximum   |                            最大值                            |
|    value    |                 当前值，可以设定或读取当前值                 |
| textVisible |          是否显示文字，文字一般是百分比表示的进度。          |
| orientation |                  可以设置为水平或垂直方向。                  |
|   format    | 显示文字的格式，“%p%”显示百分比，“%v”显示当前值，“%m”显示总步数。缺省为“%p%”。 |

<br>

## 代码演示：

此处实现了，信号于槽函数的**多对一**，从而避免了槽函数的重复写多遍

```cpp
//ui->progressBarHor->setOrient1ation(Qt::Horizontal /*(the default)  Qt::Vertical*/);  设置进度条水平或竖直

connect(ui->sliderHor, SIGNAL(valueChanged(int)), this, SLOT(onValChange(int)));
connect(ui->scrollBarHor, SIGNAL(valueChanged(int)), this, SLOT(onValChange(int)));
connect(ui->scrollBarHor, SIGNAL(valueChanged(int)), this, SLOT(onValChange(int)));
connect(ui->sliderVer, SIGNAL(valueChanged(int)), this, SLOT(onValChange(int)));
connect(ui->scrollBarVer, SIGNAL(valueChanged(int)), this, SLOT(onValChange(int)));
connect(ui->progressBarVer, SIGNAL(valueChanged(int)), this, SLOT(onValChange(int)));

===================================================================================

//对应的槽函数
void ExQProgressBar::onValChange(int val)
{
    ui->sliderHor->setValue(val);
    ui->scrollBarHor->setValue(val);
    ui->progressBarHor->setValue(val);
    ui->sliderVer->setValue(val);
    ui->scrollBarVer->setValue(val);
    ui->progressBarVer->setValue(val);

}
```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples)  【QtQProgressBarEx】

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>