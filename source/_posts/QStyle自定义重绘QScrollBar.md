---
title: QStyle自定义重绘QScrollBar
date: 2019-9-17 22:59:21
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - QStyle
---

**简介：**  `QStyle`自定义重绘`QScrollBar`（滚动条）样式。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [QStyle自定义重绘QScrollBar](https://blog.csdn.net/qq_33154343/article/details/100943187) 

<br>

## 运行效果：



<br>

## QScrollbar官方教程:

QScrollBar小部件提供垂直或水平滚动条。

滚动条是一种控件，使用户能够访问比用于显示文档的小部件更大的文档部分。它提供了用户在文档中的当前位置和可见文档数量的可视化指示。滚动条通常配备了其他控件，以支持更精确的导航。Qt以适合每个平台的方式显示滚动条。

如果需要在另一个小部件上提供滚动视图，使用QScrollArea类可能更方便，因为它提供了一个viewport小部件和滚动条。如果您需要为使用QAbstractScrollArea的特定小部件实现类似的功能，QScrollBar非常有用;例如，如果您决定子类化QAbstractItemView。对于使用滑块控件在给定范围内获取值的大多数其他情况，QSlider类可能更适合您的需要。

￼<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910224730.png"/>

> 滚动条通常包括四个单独的控件:滑块、滚动箭头和页面控件。
>
> 滑块提供了快速进入文档任何部分的方法，但不支持在大型文档中精确导航。
>
> b.滚动箭头是按钮，可以用来精确地导航到文档中的特定位置。对于连接到文本编辑器的垂直滚动条，它们通常将当前位置向上或向下移动一个“行”，并少量调整滑块的位置。在编辑器和列表框中，“行”可能表示一行文本;在图像查看器中，它可能意味着20个像素。
>
> 页面控件是拖动滑块的区域(滚动条的背景)。点击这里将滚动条移动到单击一个“页面”。这个值通常与滑块的长度相同。



每个滚动条都有一个值，该值指示滑块距滚动条起点的距离;这是通过value()和setValue()获得的。此值始终位于为滚动条定义的值范围内，从最小()到包含最大值()。可以使用setMinimum()和setMaximum()设置可接受值的范围。在最小值处，滑块的顶部边缘(垂直滚动条)或左侧边缘(水平滚动条)将位于滚动条的顶部(或左侧)。在最大值处，滑块的底部(或右侧)边缘将位于滚动条的底部(或右侧)末端。

滑块的长度通常与页面步骤的值相关，通常表示滚动视图中显示的文档区域的比例。page step是当用户按下页面向上和向下键时值的变化量，并使用setPageStep()设置。使用游标键对行步骤定义的值进行更小的更改，这个数量由setSingleStep()设置。

注意，使用的值范围与滚动条小部件的实际大小无关。在为范围和页步骤选择值时，不需要考虑这一点。

为滚动条指定的值范围通常与为QSlider指定的值范围不同，因为需要考虑滑块的长度。如果我们有一个100行文档，而我们只能在一个小部件中显示20行，那么我们可能希望构造一个滚动条，其中页面步骤为20，最小值为0，最大值为80。这将为我们提供一个包含五个“页面”的滚动条。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910224805.png"/>

> 在许多常见情况下，文档长度、滚动条中使用的值范围和页面步骤之间的关系很简单。滚动条的值范围是通过从表示文档长度的值中减去选定的页面步骤来确定的。在这种情况下，下面的公式很有用:文档长度= maximum() - minimum() + pageStep()。

QScrollBar只提供整数范围。注意，尽管QScrollBar处理非常大的数字，但是当前屏幕上的滚动条不能有效地表示超过100,000像素的范围。除此之外，用户很难使用键盘或鼠标来控制滑块，滚动箭头的使用也很有限。

<br>

## QScrollbar属性理解：

若是需要重绘，查看Qt源码，有如下几个`enum QStyle::ControlElement`是关于滚动条的元素：

|      Constant       |  Description   |
| :-----------------: | :------------: |
| CE_ScrollBarAddPage | 增加页(在滑槽) |
| CE_ScrollBarSubPage | 减少页(在滑槽) |
| CE_ScrollBarSlider  |      滑块      |
| CE_ScrollBarAddLine |    增加按钮    |
| CE_ScrollBarSubLine |    减少按钮    |
|  CE_ScrollBarFirst  |    未测出来    |
|  CE_ScrollBarLast   |    未测出来    |

使用代码显示如下：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910224200.png"/>

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>