---
title: 重绘的QStyle中sizeFromContents()没有被调用
date: 2019-9-17 21:27:26
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - c/c++
 - qt
 - QStyle重绘
---



**简介：**  在自定义重绘`QStyle`的时候，继承于`class ExCustomStyle : public QCommonStyle`的类，在重写虚函数`sizeFromContents()`时候，却发现并没有被调用。在此处记录一个**"硬核深坑"sizeFromContents()没有被调用**。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `deepin 15.11 x64 专业版 `    **Kernel：** `x86_64 Linux 4.15.0-30deepin-generic`

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [重绘的QStyle中sizeFromContents()没有被调用](https://blog.csdn.net/qq_33154343/article/details/100941134) 

<br>

## 系列博文：

- [`QStyle`自定义重绘`QSlider`控件](https://touwoyimuli.github.io/2019/09/04/QStyle%E8%87%AA%E5%AE%9A%E4%B9%89%E9%87%8D%E7%BB%98QSlider%E6%8E%A7%E4%BB%B6/) 
-  [QStyle之PenStyle的CustomDashLine使用](https://touwoyimuli.github.io/2019/09/09/QStyle%E4%B9%8BPenStyle%E7%9A%84CustomDashLine%E4%BD%BF%E7%94%A8/) 【更新：更加精准的绘画滑槽】
- [重绘的QStyle中sizeFromContents()没有被调用](https://touwoyimuli.github.io/2019/09/17/%E9%87%8D%E7%BB%98%E7%9A%84QStyle%E4%B8%ADsizeFromContents()%E6%B2%A1%E6%9C%89%E8%A2%AB%E8%B0%83%E7%94%A8/)
- [QStyle自定义重绘QSlider控件二](https://touwoyimuli.github.io/2019/09/17/QStyle%E8%87%AA%E5%AE%9A%E4%B9%89%E9%87%8D%E7%BB%98QSlider%E6%8E%A7%E4%BB%B6%E4%BA%8C/)（重要）

<br>

## 错误起因：

在自定义**QStyle**的**CustomStyle**的**.h/.cpp**中,有重写如下一些函数等虚函数的时候：

```cpp
QSize sizeFromContents(QStyle::ContentsType ct, const QStyleOption *opt, const QSize &contentsSize, const QWidget *widget = nullptr) const override;
void drawControl(QStyle::ControlElement element, const QStyleOption *opt, QPainter *p,
                     const QWidget *w = nullptr) const override;
```

但是**项目自带的例子**(QWidget+多个布局+控件)被成功运行,而**自己创建的Qwidge**t上面添加的QSlider却唯独没有运行sizeFromContents()函数?

**【预期期待现象：】**灰色矩形的宽度应该是和滑块矩形的宽度是一样大的。



**【疑惑不解：】**为什么重写的代码后运行,有的QWidget运行drawControl()和sizeFromContents(); 而有的（另一个QWidget）drawControl()，没有运行sizeFromContents()????    **真的是想的挠头挠头头好几个小时后，都想的不出来原因**???无果， 请教大佬之后。

<br>

## 解决原因：

自己创建的**QWidget**中,多个**Slider**是没有添加进布局,而是直接制指定父对象为**WQidget**,且调用`resize(300, 40);`指定**Slider**的大小.而Qt源码思路就是,那我就不调用`sizeFromContents()`函数了.caocaocao 哦~  破案了~～　真的是他妈的坑．<font color=#D0087E size=4 face="幼圆">**总结：只有控件添加到布局里面，才会触发该函数sizeFromContents（）**</font>

<br>

## 测试代码：

- **在main()中的测试代码：**

```cpp
//不显示的代码
QApplication::setStyle("CustomStyle");

QWidget wTemp;
wTemp.resize(800, 600);

QSlider *slider = new Widget(&wTemp);             //水平无刻度的QSlider
slider->setOrientation(Qt::Horizontal);
slider->setTickPosition(QSlider::NoTicks);
slider->resize(300, 40);
slider->setTickInterval(10);

wTemp.show();
```

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190917214120.png"/>



- 修改之后，可以会执行**sizeFromContents()**函数：

```cpp
会显示的代码
QApplication::setStyle("CustomStyle");

QWidget wTemp;
wTemp.resize(800, 600);

QHBoxLayout *layout = new QHBoxLayout(&wTemp);

QSlider *slider = new Widget(nullptr);             //水平无刻度的QSlider
slider->setOrientation(Qt::Horizontal);
slider->setTickPosition(QSlider::NoTicks);
slider->resize(300, 40);
slider->setTickInterval(10);

layout->addWidget(slider);                         //Slider添加到布局里面

wTemp.show();
```

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190917214045.png"/>

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>

