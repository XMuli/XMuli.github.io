---
title: QStyle之PenStyle的CustomDashLine使用
date: 2019-9-9 20:53:26
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - QStyle
 - qt
---



**简介：**  `QStyle`之`PenStyle`的`CustomDashLine`使用，使用自定义风格样式的画笔

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `deepin 15.11 x64 专业版 `    **Kernel：** `x86_64 Linux 4.15.0-30deepin-generic`

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [QStyle之PenStyle的CustomDashLine使用](https://blog.csdn.net/qq_33154343/article/details/100659576)

<br>

## 系列博文：

- [`QStyle`自定义重绘`QSlider`控件](https://touwoyimuli.github.io/2019/09/04/QStyle%E8%87%AA%E5%AE%9A%E4%B9%89%E9%87%8D%E7%BB%98QSlider%E6%8E%A7%E4%BB%B6/) 
-  [QStyle之PenStyle的CustomDashLine使用](https://touwoyimuli.github.io/2019/09/09/QStyle%E4%B9%8BPenStyle%E7%9A%84CustomDashLine%E4%BD%BF%E7%94%A8/) 【更新：更加精准的绘画滑槽】
- [重绘的QStyle中sizeFromContents()没有被调用](https://touwoyimuli.github.io/2019/09/17/%E9%87%8D%E7%BB%98%E7%9A%84QStyle%E4%B8%ADsizeFromContents()%E6%B2%A1%E6%9C%89%E8%A2%AB%E8%B0%83%E7%94%A8/)
- [QStyle自定义重绘QSlider控件二](https://touwoyimuli.github.io/2019/09/17/QStyle%E8%87%AA%E5%AE%9A%E4%B9%89%E9%87%8D%E7%BB%98QSlider%E6%8E%A7%E4%BB%B6%E4%BA%8C/)（重要）

<br>

## 系列文章：

<br>

## 运行效果：

- 设计师给的图片：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190909205624.jpg"/>



- 第一次绘画效果:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190909205925.png"/>

- 本次修改效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190909205749.png"/>

<br>

## 区别：

其中第一次绘画，和第二次绘画滑槽，都是使用划线**void QPainter::drawLine(const [QLineF](../qtcore/qlinef.html) &*line*)** 来进行绘画的，之前是使用系统自带的**Qt::DotLine**风格，而现在是使用自定义样式的**Qt::CustomDashLine**风格绘画；原因是系统自带Qt::DotLine的虽可以控制`pen.setWidthF(3);`线的粗细，但是不能控制其高度（保持宽度和间隔不变的情况下）；很难长得和设计图一样。

<br>

## PenStyle介绍：

想看一下**enum Qt::PenStyle**的官方文档：一共有7（统风格）+1（自定义风格）的样式：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190909205515.jpg"/>

<br>

## CustomDashLine使用方法：

官方教程如下：使用**void setDashPattern(const [QVector](../qtcore/qvector.html)<[qreal](../qtcore/qtglobal.html#qreal-typedef)> &*dashPattern*)** 函数

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190909211752.png"/>

> 译文如下：
>
> 将此笔的虚线模式设置为给定模式。这隐式地将笔的样式转换为Qt :: CustomDashLine。
> 必须将模式指定为偶数个正条目，其中条目1,3,5 ...是短划线，而2,4,6 ......是空格。例如：
>
> QPen pen;
> QVector<qreal> dashes;
> qreal space = 4;
> dashes << 1 << space << 3 << space << 9 << space << 27 << space;
> pen.setDashPattern(dashes);
>
> 仪表板图案以笔宽度为单位指定;例如宽度为10的长度为5的长度为50像素。请注意，宽度为零的笔相当于宽度为1像素的化妆笔。
> 每个破折号也受制于帽子样式，因此带有方帽设置的短划线将在每个方向上延伸0.5像素，从而导致总宽度为2。
> 请注意，默认的上限样式是Qt :: SquareCap，这意味着方形线末端覆盖终点并超出线宽的一半。
> 另请参见setStyle（），dashPattern（），setCapStyle（）和setCosmetic（）。

**其中具体使用如下：**

其中便是绘画一条（自定义）的线：其**线的一个完整的周期**为（单位为像素）：

> 1(线段)       4(间隔)       3(线段)       4(间隔)       9(线段)       4(间隔)       27(线段)       4(间隔)  

注意：其中**dashes**只能够出现为偶数对，不可为奇数个；

<br>

## 用于项目：

首先贴出第一次的"**绘画滑槽**"代码(局部):

```cpp
QPen pen;
//绘画 滑槽(线)
if (opt->subControls & SC_SliderGroove) {
    pen.setStyle(Qt::DotLine);
    pen.setWidthF(3);
    pen.setColor(getColor(opt, QPalette::Highlight));
    p->setPen(pen);
    p->setRenderHint(QPainter::Antialiasing);
    p->drawLine(QPointF(rectGroove.left(), rectHandle.center().y()), QPointF(rectHandle.left(), rectHandle.center().y()));
    pen.setColor(getColor(opt, QPalette::Foreground));
    p->setPen(pen);
    p->drawLine(QPointF(rectGroove.right(), rectHandle.center().y()), QPointF(rectHandle.right(), rectHandle.center().y()));
}
```

另外一个绘画方法是，在规定大小矩形内，绘画n多个小矩形，充当滑槽，但是可能需绘画多个小矩形来实现(考虑到效率开销),得到细密的线的效果；其使用for循环 遍历绘画很多次 (同样也是大的)，所以此方法被否定了。

只需要上面的`pen.setStyle(Qt::DotLine);（第四行）`修改为如下代码

```cpp
pen.setStyle(Qt::CustomDashLine);
    QVector<qreal> dashes;
    qreal space = 1.3;
    dashes << 0.1 << space;
    pen.setDashPattern(dashes);
```

即可得到这次的效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190909205749.png"/>

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>