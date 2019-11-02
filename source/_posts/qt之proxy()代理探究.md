---
title: qt之proxy()代理探究
date: 2019-9-28 00:07:11
toc: true
categories: 
 - [学习 - qt]
tags: 
 - qt
---



**简介：**  **qt:**  `proxy()`代理探究

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [qt之proxy()代理探究](https://blog.csdn.net/qq_33154343/article/details/101571843) 

<br>

## 查看源码：

**qt**中源码查阅可看到：

```cpp
/*!
    \since 4.6

    \fn const QStyle *QStyle::proxy() const

    This function returns the current proxy for this style.
    By default most styles will return themselves. However
    when a proxy style is in use, it will allow the style to
    call back into its proxy.
*/
const QStyle * QStyle::proxy() const
{
    Q_D(const QStyle);
    return d->proxyStyle;
}
```

<br>

## 问题起因：

在封装函数的时候，
`int radius = proxy()->pixelMetric(PM_FrameRadius, opt, w);`    //为什么这里使用`proxy()->`会显示报错，没有匹配到的相应的函数
`//    int radius = DStyle::pixelMetric(PM_FrameRadius, opt, w);`　　　　　//而下面的可以通过

<br>

## 尝试分析：

检索Qt帮助手册：`pixelMetric`；发现一共有如下<font color=#D0087E size=4 face="幼圆">**QStyle-->QCommonStyle-->QProxyStyle (只有这三个，依次为重写上一个)**</font>

> int       QStyle::pixelMetric(QStyle::PixelMetric metric, const QStyleOption *option = nullptr, const QWidget *widget = nullptr) const
>
> int QCommonStyle::pixelMetric(QStyle::PixelMetric m,      const QStyleOption *opt = nullptr,    const QWidget *widget = nullptr) const
>
> int  QProxyStyle::pixelMetric(QStyle::PixelMetric metric, const QStyleOption *option = nullptr, const QWidget *widget = nullptr) const
>
> int       DStyle::pixelMetric(QStyle::PixelMetric m,      const QStyleOption *opt = nullptr,    const QWidget *widget = nullptr) const override;

<br>

## 正确回答：

然而，上面的分析实际上，并没有什么卵用，用下面一句话解决：

`proxy()`实际上就是返回它自己，相当于当前类的`this`指针；通过自己实际工程中的验证，也的确是这样这理解



**其源码英文的注释翻译如下：**

> 此函数返回此样式的当前代理。 默认情况下，大多数样式都会返回。 然而当使用代理样式时，它将允许样式回调它的代理．





