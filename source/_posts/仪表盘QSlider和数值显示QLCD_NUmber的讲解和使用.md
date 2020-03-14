---
title: 仪表盘QSlider和数值显示QLCD_NUmber的讲解和使用
date: 2019-9-19 00:04:16
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt控件
---



**简介：**  仪表盘`QSlider`和数值显示`QLCD_NUmber`的讲解和使用。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `visual studio 2015`， `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## 运行效果：

先上一个最终的运行效果图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190918200740.gif"/>

<br>

## 系列博文：

**比较紧密的关联博文参考：**

- [滑动条`QSlider`和`QAbstractSlider`的介绍和用法](https://touwoyimuli.github.io/2019/09/18/%E6%BB%91%E5%8A%A8%E6%9D%A1QSlider%E5%92%8CQAbstractSlider%E7%9A%84%E4%BB%8B%E7%BB%8D%E5%92%8C%E7%94%A8%E6%B3%95/)
- [`QSlider`、`QScrollBar`、`QProgressBar`控件的联动](https://touwoyimuli.github.io/2019/09/19/QSlider%E3%80%81QScrollBar%E3%80%81QProgressBar%E6%8E%A7%E4%BB%B6%E7%9A%84%E8%81%94%E5%8A%A8/)
- [仪表盘`QSlider`的讲解和使用](https://touwoyimuli.github.io/2019/09/19/%E4%BB%AA%E8%A1%A8%E7%9B%98QSlider%E5%92%8C%E6%95%B0%E5%80%BC%E6%98%BE%E7%A4%BAQLCD_NUmber%E7%9A%84%E8%AE%B2%E8%A7%A3%E5%92%8C%E4%BD%BF%E7%94%A8/)

<br>

## 继承关系：

`QSlider`移动条、`QScrollBar`滚动条、进度条这三个控件，都是继承于`QAbstractSlider`类，其中关于**QSlider和QAbstractSlider属性**讲解，参考已经发过的文章[https://blog.csdn.net/qq_33154343/article/details/100944831](https://blog.csdn.net/qq_33154343/article/details/100944831) ；关于这几个控件的继承关系如下图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190918192923.png"/>

<br>

## QDial属性：

- QDial是仪表盘式的组件，通过旋转表盘获得输入值。QDial的特有的属性包括以下两种。

|      属性      |          含义          |
| :------------: | :--------------------: |
| notchesVisible |  表盘的小刻度是否可见  |
|  notchTarget   | 表盘刻度间的间隔像素值 |

<br>

## QLCDNumber属性：

- QLCDNumber是模拟LCD显示数字的组件，可以显示整数或小数，但就如实际的LCD一样，要设定显示数字的个数。显示整数时，还可以选择以不同进制来显示，如十进制、二进制、十六进制。其主要属性如下。

|       属性        |                             含义                             |
| :---------------: | :----------------------------------------------------------: |
|    digitCount     |        显示的数的位数，如果是小数，小数点也算一个数位        |
| smallDecimalPoint |          是否有小数点，如果有小数点，就可以显示小数          |
|       mode        | 数的显示进制，通过调用函数setDecMode）、setBinMode（）、setOctMode）、setHexMode（）可以设置为常用的十进制、二进制、八进制、十六进制格式。 |
|       value       | 返回显示值，浮点数。若设置为显示整数，会自动四舍五入后得到整数，设置为intValue的值。如果smallDecimalPoint=true，设置value时可以显示小数，但是数的位数不能超过digitCount。 |
|     intValue      |                       返回显示的整数值                       |

**例如**，若smallDecimalPoint-true，digitCount=3，设置value=2.36，则界面上LCDNumber组件会显示为2.4；若设置value=1456.25，则界面上LCDNumber组件只会显示145。所以，用QLCDNumber作为显示组件时，应注意这些属性的配合。

<br>

## 核心源码：

```cpp
//notchesVisible:表盘的小刻度是否可见
//notchTarget：表盘刻度间间隔的像素值
connect(ui->dial, SIGNAL(valueChanged(int)), this, SLOT(onDisplayLCD(int)));
setWindowTitle(QObject::tr("QDial表盘输入，在LCD以多种进制显示"));

-------------------------------------------------------------------------------------
    
ui->lcdNumber->display(val);    //显示LCD
ui->lcdNumber->setBinMode();    //设置LCD显示二进制数
ui->lcdNumber->setOctMode();    //设置LCD显示八进制数
ui->lcdNumber->setDecMode();    //设置LCD显示十进制数
ui->lcdNumber->setHexMode();    //设置LCD显示十六进制数
```

<br>

## 源码下载：

[https://github.com/xmuli/QtExamples](https://github.com/xmuli/QtExamples) 【QtQdialQLCDEx】

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

