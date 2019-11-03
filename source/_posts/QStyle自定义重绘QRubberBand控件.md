---
title: QStyle自定义重绘QRubberBand控件
date: 2019-9-7 00:04:30
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - QStyle
---



**简介：** 根据`QStyle`的继承关系和重绘原理；通过实现一个继承`QCommonStyle`类的实现，实现自己的自定义控件`QRubberBand`控件。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `deepin 15.11 x64 专业版 `    Kernel： `x86_64 Linux 4.15.0-30deepin-generic`

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [QStyle自定义重绘QRubberBand控件](https://blog.csdn.net/qq_33154343/article/details/100588428) 

<br>

## 运行效果： 

先上一张最终的重绘运行效果图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190904195212.png"/>

<br>

## QRubberBand重绘：

这几就不翻开官方简介了，话说初次看到QRubberBand这个名字的时候，我一脸的懵逼。这个控件，我怎么听都没有听说过，更没有见过。

**但是**: 如果有人和你讲，这就是windows的这个东西，那就很大家都见过了，就是**多选文件时候的那个高亮矩形**，如下图

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190904195656.png"/>

**绘画思路：**

- 对应的重载函数里面，case 这个控件，然后绘画一个矩形（其画刷和边框的颜色是不一样的）

代码如下：

```cpp
QPainter *p, const QWidget *w) const
{
    switch (element) {
    case CE_RubberBand: {
        if (const QStyleOptionRubberBand *rubber = qstyleoption_cast<const QStyleOptionRubberBand *>(opt)) {
            QColor color = opt->palette.highlight().color();
            color.setAlphaF(0.1);
            p->setBrush(color);
            color.setAlphaF(0.2);
            p->setPen(QPen(color, 1));
            p->drawRect(opt->rect.adjusted(0, 0, -1, -1));
            return;
        }
        break;
    }
    default:
        break;
    }

    QCommonStyle::drawControl(element, opt, p, w);
}
```

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>