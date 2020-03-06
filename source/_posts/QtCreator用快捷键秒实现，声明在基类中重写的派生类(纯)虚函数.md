---
title: 小技巧：QtCreator用快捷键秒实现，声明在基类中重写的派生类(纯)虚函数
date: 2020-02-23 12:22:21
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt
---



　　**简  述：**　Qt Creator 5.9+ 的一个使用技巧，使用快捷键在派生类中直接声明重写基类的（纯）虚函数，和快速🔜实现声明类的实现。

<!-- more -->

[TOC]

### 快捷键声明重写的虚函数：

  1. 源文件顶部有 #include  QCommonStyle 

  2. 该派生类 MyStyle 继承于 QCommonStyle

  3. 光标在单词 QCommonStyle 上 

  4. 按住 Command + 鼠标右键，弹出菜单栏 

     <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200220_213755.jpeg"/>

  5. 选中插入（继承于父类的）虚函数 ✅

     <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200220_213843.jpeg"/> 

<br>

> <font color=#D0087E  size=4 face="幼圆">**📌本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [小技巧：QtCreator用快捷键秒实现，声明在基类中重写的派生类(纯)虚函数](https://blog.csdn.net/qq_33154343/article/details/104457739) 

