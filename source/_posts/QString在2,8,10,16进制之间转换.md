---
title: QString在2,8,10,16进制之间转换
date: 2019-9-15 18:18:22
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt控件
---



**简述：** 初探字符串`QString`的输入和输出，和数值在2 ／8／10／16**进制**之间相互转换。



<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简述

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [`QString`在2 ／8／10／16进制之间转换](https://blog.csdn.net/qq_33154343/article/details/100860030)

<br> 

## 运行效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190914221639.gif"/>

<br>

## 同系列文章：

[QString常用的功能函数的介绍和用法](https://blog.csdn.net/qq_33154343/article/details/100860270) 

<br>

## 字符串QString：

字符串有`QString`， `char *`, `char a[]`, `string`,其中后面三个都是属于c++的，而只有**QString**是属于**Qt**所特有的；**其中QString内部是以utf8存储的**，而c自带的后面三个却不是同样以utf8存储的，所以会经常遇到情况，**乱码原因和解决方法参考：**补充部分的[乱码相关](https://github.com/touwoyimuli/QtExamples)的系内文章

<br>

## String转化为整/浮点数：

下面列举一些比较常用的转换函数:

```cpp
double    QString::toDouble(bool **ok* = Q_NULLPTR) const
float     QString::toFloat(bool *ok = Q_NULLPTR) const
int       QString::toInt(bool *ok = Q_NULLPTR, int base = 10) const
long      QString::toLong(bool *ok = Q_NULLPTR, int base = 10) const
qlonglong QString::toLongLong(bool *ok = Q_NULLPTR, int base = 10) const
short     QString::toShort(bool *ok = Q_NULLPTR, int base = 10) const

//转换为带u开头的
uint      QString::toUInt(bool *ok = Q_NULLPTR, int base = 10) const
...
```

<br>

## 整/浮点数转化为String：

下面列举一些比较常用的转换函数:

```cpp
[static] QString QString::number(double n, char format = 'g', int precision = 6)
[static] QString QString::asprintf(const char *cformat, ...)

QString &QString::setNum(float n, char format = 'g', int precision = 6)
```

其中使用如下：

eg:`str = QString::number(n, 'f', 2);`    其中2为保留小数点后两位

<br>

## 进制之间的转换：

```cpp
QString str = ui->edit2->text();
bool ok;
int val = str.toInt(&ok, 2);      //获取二进制

str = str.setNum(val, 8);         //显示八进制
ui->edit8->setText(str);
str = str.setNum(val, 10);        //显示十进制
ui->edit10->setText(str);
str = str.setNum(val, 16);        //显示十六进制
ui->edit16->setText(str);
```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples) 【QtQStringEx】

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>

