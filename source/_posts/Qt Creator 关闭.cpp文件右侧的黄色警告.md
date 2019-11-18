---
title: Qt Creator 关闭.cpp文件右侧的黄色警告
date: 2019-11-06 20:41:26
toc: true
categories: 
 - [学习 - qt]
---



**简  述：**  在`Qt Creator`里面，默认打开一个项目文件，点开一个`*.cpp`文件里面，是很容易在右侧看到成片的黄色警告⚠️或者红颜色的`error`提示， 总是让看的人比较恐慌，然后在此文章中，给出如何关闭这个黄色、红色警告的的方法

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [Qt Creator 关闭.cpp文件右侧的黄色警告](https://blog.csdn.net/qq_33154343/article/details/102943623)

<br>

## 关闭黄色警告（和红色）提示：

### 未修改之前：

**编程环境：**  `MacOS 10.14.6 (18G103)`  

**编程软件：** `Qt 5.12.5`， `Qt Creator 4.10.0`



有着比较多的黄色（红色）警告，虽然不影响编译， 但是看起来很不爽：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-11-06_20-06-30_mark.png"/>



### 修改方法和效果：

**步骤：** 点击屏幕左上角的`Qt Cretor - 关于插件 - Name - c++: ClangcodeModel `，将这一栏的对钩✔️取消掉，然后重启`Qt Cretor`，再次打开之后， 就好了

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191106203719.png"/>

**成功重启后：**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191106203852.png"/>





