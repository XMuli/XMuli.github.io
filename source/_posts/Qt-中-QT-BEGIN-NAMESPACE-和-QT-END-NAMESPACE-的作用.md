---
title: '`Qt`中`QT_BEGIN_NAMESPACE`和`QT_END_NAMESPACE`的作用'''
date: 2019-08-02 22:29:45
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - qt]
 - [学习 - 底层原理、思想架构]
tags: 
 - c/c++
 - qt
 - 原理、架构
---

**简介：**   `Qt`中`QT_BEGIN_NAMESPACE`和`QT_END_NAMESPACE`的作用

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		  `Qt`中`QT_BEGIN_NAMESPACE`和`QT_END_NAMESPACE`的作用。

**写在之前：**

觉得写的好的，但是担心忘记的，有感觉有帮助与理解加深底层和原理等，**但是不要本末倒置做成了网络搬运工。**为了写文章而写文章，忘记了本质是知识点的学习，记忆在脑海。因为最近一段时间，在疯狂学习，qt和linux和底层知识，了解IDE里面的那个邪恶的三角形（run）背后的东西。写于此。

## <font color=#D0087E  face="幼圆">重要提示：</font>

- 若遇`csdn`的博文排版、文字、图片、链接、视频预览等异常，会删除该部分，或用链接代替，或删除该部分，但在文末 [github.io](https://touwoyimuli.github.io/) 中的同步文章，会保证显示正确
- <font color=#D0087E  size=4 face="幼圆">**推荐<font color=#FE7207  size=4 face="幼圆">本文末的同步链接</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好的100%效果体验**</font> 

<br>

## 原文：

在Qt中，我们经常会看到

```cpp
QT_BEGIN_NAMESPACE
class QAction;
class QMenu;
class QPlainTextEdit;
QT_END_NAMESPACE

这样的方式表达方式！这样做有什么意义呢？
只要深入最终这个宏就知道了。嘻嘻

在qglobal.h中我们可以看到这样的定义

# define QT_BEGIN_NAMESPACE namespace QT_NAMESPACE {
# define QT_END_NAMESPACE }

```

也就是说，如果你定义以下内容：

```cpp
QT_BEGIN_NAMESPACE
class QAction;
class QMenu;
class QPlainTextEdit;
QT_END_NAMESPACE
```



那么，在编译时就会变成这样：

```cpp
namespace QT_NAMESPACE 
{
	class QAction;
	class QMenu;
	class QPlainTextEdit;
}

QT_NAMESPACE是Qt自己定义的命名空间。
```



> **QT_NAMESPACE是Qt自己定义的命名空间。**

## 参考博文：

因为有着热心网友的无私分享，

[Qt中QT_BEGIN_NAMESPACE和QT_END_NAMESPACE的作用](https://blog.csdn.net/a3125504x/article/details/69278270)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

<br>

## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到csdn博客：**</font> [`Qt`中`QT_BEGIN_NAMESPACE`和`QT_END_NAMESPACE`](https://blog.csdn.net/qq_33154343/article/details/98122648) 