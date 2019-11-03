---
title: qt绘画扇形drawPie()和反三角函数qAcos()
date: 2019-9-29 23:42:17
toc: true
categories: 
 - [学习 - qt]
---



**简介：**  **qt**绘画扇形`drawPie()`，绘画出弧线； 和由三角形的长度计算出角度，利用反三角函数`qAcos()`

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [qt绘画扇形drawPie()和反着三角函数qAcos()](https://blog.csdn.net/qq_33154343/article/details/101694145) 

<br>

## 问题背景：

在绘画进度条控件中，进度滑块当处于一开始和快完成，有弧度部分的时候,想要看的过程比较自然，就要自己手动填充这一部分"梯形(腰是两个段圆弧)";而一开始想到的居然是：直接求直线于弧线(圆角矩形的弧线部分)的交点的函数，我觉得应该是没有的。

<br>

## 解决方法:

将**"腰是圆弧梯形"**拆分为一个两个圆弧和一个(标准)等腰梯形来进行拆分，从而进行填。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190929233536.png"/>
其中需要计算角度

<br>

## 绘画扇形drawPie()：

> void QPainter::drawPie(const QRect &, int a, int alen)
> 参数1: 圆弧的圆心所处于矩形
> 参数2: 圆弧的开始始的角度°(单位1/16度)
> 参数3: 圆弧的转动的角度°(单位1/16度)

**实际使用**，注意其单位是**1/16角度**，而不是**弧度**。

```cpp
p->drawPie(topPointRect,41 * 16, (90 - 41) * 16);
```

<br>

## 反三角计算角度qAcos()：

由三角形的边计算角度；需要包含头文件

```cpp
 #include <QtMath>
```

另外一个计算角度的函数

> qreal qAcos(qreal v)
> Returns the arccosine of v as an angle in radians. Arccosine is the inverse operation of cosine.
> 参数: 直角边/斜边  (注意用double)
> 返回结果: 是**弧度**为单位

使用:

```cpp
qreal raw = qAcos(30 * 1.0 / 40);   //=41.4096弧度
```

<br>

## 弧度和角度转换公式:

> 弧度 = 角度  *  π  / 180   

π所对应的宏为:

```cpp
//M_PI	The ratio of a circle's circumference to diameter, π
```

绘画上图的黑色圆弧代码:

```cpp
qreal raw = qAcos(30 * 1.0 / 40);
int startRadius = raw * 180 / M_PI;
p->drawPie(topPointRect,startRadius * 16, (90 - startRadius) * 16);
```

<br>

## 最后的效果：

如图：然后同理，只需要将左侧的两个角都这样计算，然后同样填充蓝色，即可以做到圆角处的完美（当进度增加时候，加载想的自然）

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190929233601.png"/>