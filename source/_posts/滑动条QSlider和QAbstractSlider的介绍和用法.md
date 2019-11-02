---
title: 滑动条QSlider和QAbstractSlider的介绍和用法
date: 2019-9-18 00:07:54
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - QSlider
 - qt
 - QAbstractSlider
---



**简介：** 滑动条`QSlider`的介绍和用法，其通过滑动来设置数值，也可以用于数值的输入。以及他们的基类`QAbstractSlider`的众多属性的详细讲解

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [滑动条QSlider和QAbstractSlider的介绍和用法](https://blog.csdn.net/qq_33154343/article/details/100944831) 

<br>

## 运行效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190915000559.gif"/>

<br>

## 滑动条QSlider：

`QSlider`、`QScrollBar`和`Qdial`3个组件都从`QAbstractSlider`继承而来，有一些共有的属性。
**QSlider**是滑动的标尺型组件，滑动标尺上的一个滑块可以改变值。
基类**QAbstractSlider**的主要属性包括以下几种。



- `minimum`、`maximum`：设置输入范围的最小值和最大值，例如，用红、绿、蓝配色时，每
    种基色的大小范围是0~255，所以设置**minimum**为0，**maximum**为255。

- `singlestep`:单步长，拖动标尺上的滑块，或按下左/右光标键时的最小变化数值。

- `pageStep`:在**Slider**上输入焦点，按**PgUp**或**PgDn**键时变化的数值。

- `value`：组件的当前值，拖动滑块时自动改变此值，并限定在**minimum**和**maximum**定义的
    范围之内。

- `sliderPosition`:滑块的位置，若**tracking**属性设置为true，**sliderPosition**就等于value。

- **`tracking`：sliderPosition**是否等同于**value**，如果**tracking**=true，改变value时也同时改变
    sliderPosition。

- `orientation`: **Slider**的方向，可以设置为水平或垂直。方向参数是Qt的枚举类型enum

- `Qt：Orientation`，取值包括以下两种。  

    |      枚举      |   含义   |
    | :------------: | :------: |
    | Qt::Horizontal | 水平方向 |
    |  Qt::Vertical  | 垂直方向 |

- `invertedAppearance`:显示方式是否反向，**invertedAppearance**=false时，水平的Slider由左向右数值增大，否则反过来。
- `invertedControls`：反向按键控制，若**invertedControls**=true，则按下**PgUp**或**PgDn**按键时调整数值的反向相反。



- **属于QSlider的专有属性有两个，如下。**

- `tickPosition`:标尺刻度的显示位置，使用枚举类型**QSlider:TickPosition**，取值包括以下6种

    |          枚举           |        含义        |
    | :---------------------: | :----------------: |
    |    QSlider::NoTicks     |     不显示刻度     |
    | QSlider::TicksBothSides | 标尺两侧都显示刻度 |
    |   QSlider::TicksAbove   |  标尺上方显示刻度  |
    |   QSlider::TicksBelow   |  标尺下方显示刻度  |
    |   QSlider::TicksLeft    |  标尺左侧显示刻度  |
    |   QSlider::TicksRight   |  标尺右侧显示刻度  |

- `ticklnterval`：标尺刻度的间隔值，若设置为0，会在**singleStep**和**pageStep**之间自动选择。

<br>

## 部分源码：

```cpp
//设置QSlider的最大值为255  (默认范围为0~100)
ui->sliderRed->setMaximum(255);
ui->sliderGreen->setMaximum(255);
ui->sliderBlue->setMaximum(255);
ui->sliderAlpha->setMaximum(255);

//初始化QTextEdit的颜色 (否则默认就是白灰色)
QColor color;
color.setRgb(145, 190, 251, 255);
QPalette palette = ui->textColour->palette();
palette.setColor(QPalette::Base, color);
ui->textColour->setPalette(palette);

//连接信号与槽，多个信号，对应一个槽函数
connect(ui->sliderRed, SIGNAL(valueChanged(int)), this, SLOT(onSetClolor(int)));
connect(ui->sliderGreen, SIGNAL(valueChanged(int)), this, SLOT(onSetClolor(int)));
connect(ui->sliderBlue, SIGNAL(valueChanged(int)), this, SLOT(onSetClolor(int)));
connect(ui->sliderAlpha, SIGNAL(valueChanged(int)), this, SLOT(onSetClolor(int)));

----------------------------------------------------------------------------------

int nRed = ui->sliderRed->value();             //获取红绿蓝(RGB)的Slider的数值
int nGreen = ui->sliderGreen->value();
int nBlue = ui->sliderBlue->value();
int nAlpha = ui->sliderAlpha->value();

QColor color;
color.setRgb(nRed, nGreen, nBlue, nAlpha);
QPalette palette = ui->textColour->palette();   //获取textColour控件的所有颜色值(调色板)
palette.setColor(QPalette::Base, color);        //设置textColou的某一角色(即控件)的颜色
ui->textColour->setPalette(palette);

ui->labRgbVal->setText(QString("RGB(%1, %2, %3, %4)").arg(nRed).arg(nGreen).arg(nBlue).arg(nAlpha));   //时刻显示RGBa的具体值
```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples) 【QtQSliderEx】

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>