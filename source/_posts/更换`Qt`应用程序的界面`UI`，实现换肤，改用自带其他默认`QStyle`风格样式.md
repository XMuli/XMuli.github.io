---
title: 更换`Qt`应用程序的界面`UI`，实现换肤，改用自带其他默认`QStyle`风格样式
date: 2019-8-29 22:08:43
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列] 
tags: 
 - QStyle
 - qt
---

**简介：**  显示某一系统自带的几种风格样式`QStyle`， 然后分别进行查看效果样式。更换`Qt`应用程序的界面`UI`，实现换肤，改用自带其他默认`QStyle`风格样式。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程环境：**  `deepin 15.11 x64 专业版 `    **Kernel：**  `x86_64 Linux 4.15.0-30deepin-generic`

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [更换`Qt`应用程序的界面`UI`，实现换肤，改用自带其他默认`QStyle`风格样式](https://blog.csdn.net/qq_33154343/article/details/100148552)

<br>

## 知识讲解：

在 **main.cpp** 中，添加如下代码和头文件

```cpp
#include "Examples.h"
#include <QApplication>

#include <QStyleFactory>
#include <QDebug>
#include "ExCustomStyle.h"

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);

    //若是系统自带的QStyle风格，则需要先创建为QStyleFactory::create(""，然后设置qApp->setStyle()
    QStringList listStyle = QStyleFactory::keys();
    foreach(QString val, listStyle)     //打印当前系统支持的系统风格,,且打印出来
        qDebug()<<val<<"  ";
    
    qApp->setStyle(QStyleFactory::create("Fusion"););
    
    //若是非系统自定义的，则需要自己创建自定义风格对象，然后设置使用
    //ExCustomStyle* customStyle = new ExCustomStyle;
    //qApp->setStyle(customStyle);

    Examples w;
    w.show();

    return a.exec();
}
```



- 在**windows 10** 运行：自带支持的**QStyle**风格:

> "Windows"    "WindowsXP"   "WindowsVista"    "Fusion"

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829222611.png"/>



- 在**Deepin** （**Linux**） 运行：自带支持的**QStyle**风格:

> "chameleon"  "dsemilight"  "dsemidark"  "dlight"  "ddark"  "Windows"  "Fusion"

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831225954.jpg"/>

<br>

## 注意：

**若是使用系统自带的QStyle风格**，则需要先创建为`QStyleFactory::create("")`，然后设置`qApp->setStyle()`；

**若是使用自己自定义的QStyle风格**，则直接自己创建自定义风格对象`new  ExCustomStyle`，然后设置使用

`qApp->setStyle(customStyle);`

<br>

## 运行演示效果：

**－－－－－－－－－－－－－－－－－－win10支持系统风格：－－－－－－－－－－－－－－－－－**

- "**Windows**" 风格：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829223625.jpg"/>

- "WindowsXP" 风格：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829223636.jpg"/>

-  "**WindowsVista**" 风格：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829223651.jpg"/>

- "**Fusion**"  风格：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829223700.jpg"/>



**－－－－－－－－－－－－－－－deepin 15.11（Linux） 支持系统风格：－－－－－－－－－－－－－－**

- "chameleon"  ：这是正在系统开发的一个适配风格

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831230638.jpg"/>

- "dsemilight" 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831230512.jpg"/>

-  "dsemidark"  

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831230449.jpg"/>

- "dlight"  

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831230437.jpg"/>

- "ddark" 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831230418.jpg"/>

-  "Windows"  

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831230334.jpg"/>

- "Fusion"

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831230355.jpg"/>

**－－－－－－－－－－－－－－－－－自己自定义 的`QStyle`系统风格：－－－－－－－－－－－－－－－－**

- 自定义风格**ExCustomStyle**（因为继承的**qt**的 `QCommonStyle`，里面的虚函数都没有重写，所以就会没有效果，没有显示出来，或者显示异常(有上一次切换风格留下的残影)）：

win10 显示效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829223710.jpg"/>

deepin 15.11 显示效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190831230701.jpg"/>

<br>

## 开心分享：

因为有着热心网友的无私分享，故不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>



