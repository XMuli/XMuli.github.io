---
title: 根据给定(任意)的QString文本, 获取其矩形大小
date: 2019-10-9 19:50:26
toc: true
categories: 
 - [学习 - qt]
tags: 
 - qt
---



**简介：**  再进行空间的重绘时候，绘画进度条`QProgressBar`，需要在中间显示（可以根据opt->）进度的百分比文字，且刻度为文字中间的时候，需要左右两侧有着不一样的颜色（左边白色，右边黑色），即：进度条到达的位置文字百分比为白色显示。根据给定(任意)的`QString`文本, 获取其最小矩形。

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [根据给定(任意)的QString文本, 获取其矩形大小](https://blog.csdn.net/qq_33154343/article/details/102468233) 

<br>

## 需求背景：

绘画进度条，需要在中间显示进度的百分比文字，且刻度为文字中间的时候，需要左右两侧有着不一样的颜色（左边白色，右边黑色），即：进度条到达的位置文字百分比为白色显示。

**希望达到的预期效果如下：**

静态图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20191008224313.png"/>



动图**注意中间文字和百分比的颜色变化**（因为gif帧数，故实连续渐变色际参考上图）：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20191008231012.gif"/>

<br>

源码没修改之前：

```cpp
if (const QStyleOptionProgressBar *progBar =  qstyleoption_cast<const QStyleOptionProgressBar *>(opt)) {
            QRect rect = progBar->rect;
            rect.setWidth(rect.width() * 0.28);
            rect.setHeight(rect.height() * 0.57);
            rect.moveCenter(progBar->rect.center());

            double val = progBar->progress * 1.0 / (progBar->maximum - progBar->minimum);
            p->setPen(getColor(opt, DPalette::HighlightedText));
            QString str = "已经下载" + QString::number(val * 100, 'f', 2)  + "%(点击暂停)";
            p->drawText(rect, Qt::AlignCenter, str);

            p->setPen(Qt::red);
            p->setBrush(Qt::NoBrush);
            p->drawRect(rect);
        }
```

效果缺陷如图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20191008223907.png"/>

<br>

## 根据QString文本获取其矩形fontMetrics：

修改之后代码:

```cpp
double val = progBar->progress * 1.0 / (progBar->maximum - progBar->minimum); 
QString strPercent = QString::number(val * 100, 'f', 2)  + "%";               
QString str = "已下载" + strPercent + "(点击暂停)";                                  
QSize size = progBar->fontMetrics.size(Qt::TextSingleLine, str);              
QRect rect(progBar->rect.topLeft(), size);                                    
rect.moveCenter(progBar->rect.center());  
```

根据给定(任意)的QString, 获取其矩形大小，其效果如下：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20191008224525.jpg"/>

<br>

## 设置画笔渐变色(文字渐变色)：

文字有渐变的需求(画笔采用渐变色)：

```cpp
QPointF pointStart(rect.left(), rect.center().y());                              
QPointF pointEnd(rect.right(), rect.center().y());                               
QLinearGradient linear(pointStart, pointEnd);                                    
linear.setColorAt(0, getColor(opt, DPalette::HighlightedText));                  
linear.setColorAt(division, getColor(opt, DPalette::HighlightedText));           
linear.setColorAt(division + 0.01, getColor(opt, DPalette::ButtonText));         
linear.setColorAt(1, getColor(opt, DPalette::ButtonText));                       
linear.setSpread(QGradient::PadSpread);                                          
p->setPen(QPen(QBrush(linear), 1));  //设置画笔渐变 【重点】
```

<br>

## 设置画刷渐变色(图案渐变色)：

图片有渐变的需求(画刷采用渐变色)：

```cpp
//顺变 此处贡献出, 渐变配色方案:

QPointF pointStart(rect.left(), rect.center().y());
QPointF pointEnd(rect.right(), rect.center().y());
QLinearGradient linear(pointStart, pointEnd);
linear.setColorAt(0, QColor("#ff6e7f"));
linear.setColorAt(1, QColor("#bfe9ff"));
linear.setSpread(QGradient::PadSpread);
p->setBrush(linear);         //设置画刷渐变 【重点】
p->drawPath(inter);
```

<br>

## QFontMetrics::boundingRect()获取文本的矩形位置：

相当于计算 （没修改的源码） 中的2-5行 的计算rect  的一种快捷方法：

> //获取字符窜的最小矩形,和其在指定矩形的相对位置QFontMetrics::boundingRect()

```cpp
const QStyleOptionProgressBar *progBar =  qstyleoption_cast<const QStyleOptionProgressBar *>(opt);
double val = progBar->progress * 1.0 / (progBar->maximum - progBar->minimum);
int drawWidth = val * opt->rect.width();
//此rect矩形大小是通过文本得到最小的矩形; 其位置是相对于progBar->rect的
QRect rect = progBar->fontMetrics.boundingRect(progBar->rect, progBar->textAlignment, progBar->text);   
```

效果如图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20191008224956.jpg"/>