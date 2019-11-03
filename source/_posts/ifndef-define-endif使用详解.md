---
title: ＃ifndef／＃define／＃endif使用详解
date: 2019-8-1 23:16:43
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - 底层原理、思想架构]
tags: 
 - 原理
---



**简介：**  #ifndef/#define/#endif使用详解。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		#ifndef/#define/#endif使用详解

## <font color=#D0087E  face="幼圆">重要提示：</font>

- 若遇`csdn`的博文排版、文字、图片、链接、视频预览等异常，会删除该部分，或用链接代替，或删除该部分，但在文末 [github.io](https://touwoyimuli.github.io/) 中的同步文章，会保证显示正确
- <font color=#D0087E  size=4 face="幼圆">**推荐<font color=#FE7207  size=4 face="幼圆">本文末的同步链接</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好的100%效果体验**</font> 

<br>

 **想必很多人都看过“头文件中的 #ifndef/#define/#endif 防止该头文件被重复引用”。但是是否能理解“被重复引用”是什么意思？是不能在不同的两个文件中使用include来包含这个头文件吗？如果头文件被重复引用了，会产生什么后果？是不是所有的头文件中都要加入#ifndef/#define/#endif 这些代码？**



**其实“被重复引用”是指一个头文件在同一个cpp文件中被include了多次，这种错误常常是由于include嵌套造成的。**比如：存在a.h文件#include "c.h"而此时b.cpp文件导入了#include "a.h" 和#include "c.h"此时就会造成c.h重复引用。



**头文件被重复引用引起的后果：**

有些头文件重复引用只是增加了编译工作的工作量，不会引起太大的问题，仅仅是编译效率低一些，但是对于大工程而言编译效率低下那将是一件多么痛苦的事情。
有些头文件重复包含，会引起错误，比如在头文件中定义了全局变量(虽然这种方式不被推荐，但确实是C规范允许的)这种会引起重复定义。



**是不是所有的头文件中都要加入#ifndef/#define/#endif 这些代码？**

答案：不是一定要加，但是不管怎样，用#ifnde xxx #define xxx #endif或者其他方式避免头文件重复包含，只有好处没有坏处。个人觉得培养一个好的编程习惯是学习编程的一个重要分支。



**下面给一个#ifndef/#define/#endif的格式：**

#ifndef A_H意思是"if not define a.h"  如果不存在a.h

接着的语句应该#define A_H  就引入a.h

最后一句应该写#endif   否则不需要引入

```cpp
#ifndef GRAPHICS_H // 防止graphics.h被重复引用 
#define GRAPHICS_H 
#include <math.h> // 引用标准库的头文件 
… 
#include “header.h” // 引用非标准库的头文件 
… 
void Function1(…); // 全局函数声明 
… 
class Box // 类结构声明 
{ 
… 
};
#endif
```

<br>

## 参考博文：

因为有着热心网友的无私分享，

[#ifndef/#define/#endif使用详解](https://blog.csdn.net/abc5382334/article/details/18052757) 

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

<br>

## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到csdn博客：**</font> [#ifndef/#define/#endif使用详解](https://blog.csdn.net/qq_33154343/article/details/98124027)

 