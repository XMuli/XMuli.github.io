---
title: Qt Creator 安装SDK，在MSVC编译模式下使用CDB调试器
date: 2019-8-7 19:52:09
toc: true
categories: 
 - [学习 - qt]
---



**简介：**  `Qt Creator` 安装 `Windows Software Development Kit`(`SDK`）调试器【即`CDB`调试器】。（使用`MSVC`编译项目，进行调试）。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		 `Qt Creator` 安装 `Windows Software Development Kit`(`SDK`）调试器。（使用`MSVC`编译项目，进行调试）。

<br>

**编程环境：**  `win10 x64 专业版 1803`  操作系统版本：`17134.285` 

**编程软件：**  `visual studio 2015`， `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">重要提示：</font>

- 若遇`csdn`的博文排版、文字、图片、链接、视频预览等异常，会删除该部分，或用链接代替，或删除该部分，但在文末 [github.io](https://touwoyimuli.github.io/) 中的同步文章，会保证显示正确
- <font color=#D0087E  size=4 face="幼圆">**推荐<font color=#FE7207  size=4 face="幼圆">本文末的同步链接</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好的100%效果体验**</font> 

<br>

## 为什么安装CDB编调试器？



<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190807194229.png"/>

（1）Kits页面显示QtCreator可用的编译工具，在图中可以看到有3个编译工具可用。

（2）Qt Versions页面显示安装的Qt版本，有Qt 5.9.1MinGW 32bit、Qt 5.9.1MSVC201532bit
和Qt 5.9.1MSVC201564bit3个可用的版本。

（3）Compliers页面显示系统里可用的C和C++编译器，由于安装了MinGW和Visual Studio
2015，Qt Creator会自动检测出这些编译器。

（4）**Debuggers页面显示Qt Creator自动检测到的调试器，有GNU gdb for MinGW调试器**
**和Windows的CDB调试器。**

**注意：** <font color=#FE7207  size=4 face="幼圆">如果只是在计算机上安装了Visual Studio2015，显示的界面上MSVC2015的两个编译器的图标
会变为带有感叹号的一个黄色图标。Debuggers页面没有Windows的CDB调试器，可以用MSVC编译器对
Qt Creator编写的程序进行编译，但是不能调试，**这是因为缺少了Windows Software Development Kit(SDK）。**
**这个SDK不会随Visual Studio一同安装，**需要从Microsoft网站上下载。可以下载Windows Software
Development Kit(SDK)for Windows8.1,安装后重启计算机即可。</font>



## 安装CDB调试器步骤：

- 先关闭Qt Creator 
    **msvc编译器使用windbg下的cdb调试器 所以需要安装windbg**

官网下载链接：<https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190807192915.png"/>

自动跳转到该页面

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190807193021.png"/>



- 下载成功

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190807193142.png"/>



- 右键管理员运行

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190807193304.png"/>

- 选择“NO“

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190807193415.png"/>



- **只用安装调试工具即可**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190807193615.png"/>



- 等待安装成功

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190807193738.png"/>



- 重新打开Qt Creator,在如图所示的位置进行修改

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190807194038.png"/>



- 配置成功标志

参见本文第一张图

## 参考博文：

因为有着热心网友的无私分享，故不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 

**参考一：**  [**百度**](https://www.baidu.com/) 

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

<br>

## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到csdn博客：**</font> [Qt Creator 安装SDK，在MSVC编译模式下使用CDB调试器](https://blog.csdn.net/qq_33154343/article/details/98779698) 