---
title: QStyle自定义重绘QSlider控件二
date: 2019-9-17 22:06:54
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt
 - QStyle
---



**简介：**  修改创建控件时候，的默认矩形大小，重写`sizeFromContents()`函数，给定默认控件大小

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `deepin 15.11 x64 专业版 `    **Kernel：**  `x86_64 Linux 4.15.0-30deepin-generic`

**编程软件：**   `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> 

<br>

## 系列博文：

- [`QStyle`自定义重绘`QSlider`控件](https://touwoyimuli.github.io/2019/09/04/QStyle%E8%87%AA%E5%AE%9A%E4%B9%89%E9%87%8D%E7%BB%98QSlider%E6%8E%A7%E4%BB%B6/) 
-  [QStyle之PenStyle的CustomDashLine使用](https://touwoyimuli.github.io/2019/09/09/QStyle%E4%B9%8BPenStyle%E7%9A%84CustomDashLine%E4%BD%BF%E7%94%A8/) 【更新：更加精准的绘画滑槽】
- [重绘的QStyle中sizeFromContents()没有被调用](https://touwoyimuli.github.io/2019/09/17/%E9%87%8D%E7%BB%98%E7%9A%84QStyle%E4%B8%ADsizeFromContents()%E6%B2%A1%E6%9C%89%E8%A2%AB%E8%B0%83%E7%94%A8/)
- [QStyle自定义重绘QSlider控件二](https://touwoyimuli.github.io/2019/09/17/QStyle%E8%87%AA%E5%AE%9A%E4%B9%89%E9%87%8D%E7%BB%98QSlider%E6%8E%A7%E4%BB%B6%E4%BA%8C/)（重要）

<br>

## 更新原因：

因为前一个版本，出现了一个小的bug：那就是，遗漏了当`QSlider`无需刻度的时候（枚举值为`QSlider::NoTicks`），会发生**opt->rect**仍然会比矩形（滑块和滑槽的最小公共矩形）要大；即：没有去除掉用来保留绘画刻度的矩形区域。

<br>

## 运行效果：

上一张最终的运行效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190917231536.png"/>

<br>

## QSlider枚举含义：

下面对一些枚举进行一些含义的解释：

|           枚举            |                中文含义                | Qt文档（其英文含义比较模糊，此处以约定中文含义为准） |
| :-----------------------: | :------------------------------------: | :--------------------------------------------------: |
|    PM_SliderThickness     | Slider总的高度　＝　滑块高度＋刻度高度 |               Total slider thickness.                |
| PM_SliderControlThickness |           只是滑块的单独高度           |           Thickness of the slider handle.            |
|      PM_SliderLength      |             只是滑块的长度             |           Thickness of the slider handle.            |
|      PM_SliderLength      |             只是滑块的长度             |                Length of the slider.                 |
|  PM_SliderTickmarkOffset  |        用作slider的刻度线的高度        |   The offset between the tickmarks and the slider.   |
|  PM_SliderSpaceAvailable  |               暂时未用到               |     The available space for the slider to move.      |

上面表格参考出处代码：**qcommonstyle.cpp**　 4526行

```cpp
case PM_SliderTickmarkOffset:
    if (const QStyleOptionSlider* sl = qstyleoption_cast<const QStyleOptionSlider*>(opt))
    {
        int space = (sl->orientation == Qt::Horizontal) ? sl->rect.height()
                    : sl->rect.width();
        int thickness = proxy()->pixelMetric(PM_SliderControlThickness, sl, widget);
        int ticks = sl->tickPosition;

        if (ticks == QSlider::TicksBothSides)
            ret = (space - thickness) / 2;
        else if (ticks == QSlider::TicksAbove)
            ret = space - thickness;
        else
            ret = 0;
    }
    else
    {
        ret = 0;
    }
    break;
```

<br>

## 绘画思路：

上一篇的思路是：

- 将矩形拆分为三个小矩形
- 对于每一个矩形进行单独的判断（是否有无），若是有则绘画



而这一篇则是，在第一步骤之前，就进行修改，在`sizeFromContents()`里面进行初始的矩形计算；

先绘制默认大小，给出默认opt->rect矩形的大小（此时还没有开始计算每一个矩形的大小:将这个最大的矩形进行拆分）：

在**virtual QSize sizeFromContents(ContentsType ct, const QStyleOption *opt, const QSize &contentsSize, const QWidget *w) const override;**里面，填写如下代码：

```cpp
int sliderContHeight = proxy()->pixelMetric(PM_SliderControlThickness, opt, widget);  //单独滑块高度
            int tickMarkHeight = proxy()->pixelMetric(PM_SliderTickmarkOffset, opt, widget);      //单独刻度线高度
            sliderContHeight += tickMarkHeight;

            if (slider->tickPosition == QSlider::NoTicks) {
                sliderContHeight -= tickMarkHeight;
            } else if (slider->tickPosition == QSlider::TicksBothSides) {
                sliderContHeight += tickMarkHeight;
            } else {
            }

            if (slider->orientation == Qt::Horizontal){
                size.setHeight(qMax(size.height(), sliderContHeight));
            } else {
                size.setWidth(qMax(size.width(), sliderContHeight));
            }
        }
        break;
    }
    case CT_MenuBarItem: {
        int frame_margins = DStyle::pixelMetric(PM_FrameMargins, opt, widget);
        size += QSize(frame_margins * 2, frame_margins * 2);
        break;
    }
    default:
        break;
    }

    return size;
}
```

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>