---
title: QPushButton使用从右往左的显示之LayoutDirection
date: 2019-9-25 18:54:17
toc: true
categories: 
 - [学习 - qt]
tags: 
 - qt控件
---



**简介：**  QPushButton使用从右往左的显示控件，设置属性`LayoutDirection`

<!-- more -->

[TOC]

**编程环境：**  `deepin 15.11 x64 专业版 `    **Kernel：**  `x86_64 Linux 4.15.0-30deepin-generic`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [QPushButton使用从右往左的显示之LayoutDirection](https://blog.csdn.net/qq_33154343/article/details/101380385) 

<br>

## QPushButton 使用 从右往左 的文字：

在绘画自定义控件`QPushButton`的时候,发现有一个问题：需要考虑到一些少数地区的人使用的文字,是属于从右往左写,比如说 "阿文"等(忘记是哪一篇文章看到过,说的是有这种文字,反正鹅没有见过,哪一位见过的,贴个图我看一下, 滑稽)~~~

在绘画`QPushButton`控件时候,我有写类似如下一段代码,专门照顾这这种情况

```cpp
if (button->direction == Qt::LeftToRight) {
	textRect.setRight(rectArrowAndLine.left() - frameRadius);
} else {
	textRect.setLeft(rectArrowAndLine.right() + frameRadius);
}
```

<br>

## 测试代码：

```cpp
QPushButton btn2("abcdefghijk2222220000000022000011111222222a", &wTemp);
btn2.setLayoutDirection(Qt::RightToLeft);    //设定下拉标志在左边，打开时候是在右边
btn2.move(0, 80);
QMenu* menu2 = new QMenu(&btn2);
btn2.setMenu(menu2);
menu2->addAction("act测试一b");
menu2->addAction("act测试2");
menu2->addAction("act测试23");
```

<br>

## 运行效果：

实际若是想使用`QPushButton`,使用如上面代码,可以看到效果(图中为`deepin`系统的风格中)

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925190005.jpg"/>

查看效果可知，实际上`btn2.setLayoutDirection(Qt::RightToLeft);`并不是指文字从右往左书写显示，而是，控件排列顺序是从右往左