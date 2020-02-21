---
title: QtCreator此IDE的界面基本组成和入门使用讲解(Win,Linux,MacOS搭配不同版本 Qt)
date: 2020-01-12 01:32:36
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：** 讲述 `Qt Creator` 此**IDE**的界面基本组成，和入门使用讲解；这里主要是以 `Windows` 版本讲述为主，但是 `Linux` 和 `MacOS` 版本的会贴出来，这里需要注意的是win版本基本和 Linux版本界面完全一样，而 MacOS 版本的界面和细节部分，则是有着少许的不一样，会标记出来；

感谢自由软件，感谢开源项目，感谢前辈大家们的分享，感谢 Qt 社区的人们，感谢互联网精神，感谢商业利益的推动，感谢时间的给予发展；是 一部人们让很多很有了使得学习的门槛降低，和系统的学习某一项技能（兴趣爱好）变得容易，使得梦想可能更容易实现，很多事情变得未来可期。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200112_013425.png"/>

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [QtCreator此IDE的界面基本组成和入门使用讲解(Win,Linux,MacOS搭配不同版本 Qt)](https://blog.csdn.net/qq_33154343/article/details/103942733)

<br>

## Qt Creator的界面组成：

`Qt Creator`这个轻巧的跨平台的 IDE 集成环境，其界面组成主要是以下的几部分给组成：

- **Qt Creator：** 主要是用来编写代码工程的界面，也是使用时间最多的地方
- **Desiger 设计师:**  用来拖曳窗口；能够短时间生成一个界面出来，开发桌面程序的界面，使用的话会比较便捷
- **Assistant 帮助文档：** 可以看做一个离线的帮助手册的文档，遇到不明白的函数或控件时候，会经常查看的一个工具
- **Linguist 语言家：**  当程序要写完了，制作成多语言国际化的时候，使用它来“本地化本国语言”
- **Qt 5.9.8 for Desktop：** 将项目所需要的.dll系统依赖库全部打包到文件夹，便于只一个文件夹就可以，移植到其它电脑运行

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20200112004632.png"/>

<br>

## Windows 版本：

### 欢迎界面：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20200112005004.png"/>



### 编辑界面：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20200112005417.png"/>



### 设计界面：

使用 UI 的设计师，拖曳界面时候生成一个简单的界面，及其方便；

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20200112010033.png"/>



### 项目构建界面：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20200112010213.png"/>



### 帮助界面：

或者在光标移动到函数或者类名的单词中间，然后按下 F1 也会出现该结果的小的弹窗

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20200112010446.png"/>



### Qt助手：

Qt 助手 == Qt 离线文档 == 上图在 QtCreator 中的小窗口的独立窗口

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20200112010625.png"/>

<br>

## Linux版本：

所有界面，都基本和 Windows一毛一样。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_20200114224503.png"/>

<br>

## MacOS 版本：

### 工具栏在顶部（激活状态下）:

其主要界面和 Win一样，但是其工具栏在最顶部，只有当前窗口处于激活状态，其工具栏才会显示出来。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20200112011018.png"/>



### 如何运行多个 QtCrator?

另外一个就是，这个 QtCreator 刚安装完成，既不会又快捷方式在桌面，也不会有任何图标在启动台里面。

需要自己手动查找它出来，然后将其固定在 Dock（桌面最下方的任务栏）上面

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200112_011511.png"/>



### 如何显示快捷方式？

> 其中我的路径如下：
>
> ` /Users/muli/Qt5.12.6/Qt Creator.app` 
>
> Qt Creator.app：（复制几份，就可以打开几个 QT 的 IDE）
>
> 
>
> 下面的都在此路径下：
> `/Users/muli/Qt5.12.6/5.12.6/clang_64/bin`
>
> Assistant.app
> Designer.app
> Linguist.app

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200112_011926.png"/>

<br>

## 系统环境：

### **编程环境：**  `MacOS 10.14.6 (18G103)`  

**编程软件：** `Qt 5.12.6`， `Qt Creator 4.10.2`



### **编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt 5.9.8`，`Qt Creator 4.8.2 (Enterprise)`



### **编程环境：**  `uos 20 x64 专业版 `    **Kernel：**  `x86_64 Linux 4.19.0-5-amd64`

**编程软件：**  `Qt 5.11.3`，`Qt Creator 4.8.2 (Enterprise)`

<br>

## 归纳小结：

这里因为我会同时使用到windows，Linux，MacOS 三个系统；且使用各种不同的版本环境的 Qt 版本，如此的随心所欲（对了，就是那么巧，我的确是又三台电脑，且有分别各自安装一个系统），也大概是得益于目前的这份工作，以及公司所从事的 Linux系统的开发和开源和软件自由的；晚上在空闲之余，来写一写这些基础的文章，算是一个帮助新手们的入门。因此，无论是你是使用什么系统，使用的哪一个版本的Qt 进行开发，以及是否如何借助第三方工具，比如 VS，或 vscode + 插件的模式，来进行开发c++，都可以在我的这个系列的博客找到blog。最后，这大半夜的，写这篇文章，我是有点脚冷。天冷，脚冷，该钻进被窝了。

