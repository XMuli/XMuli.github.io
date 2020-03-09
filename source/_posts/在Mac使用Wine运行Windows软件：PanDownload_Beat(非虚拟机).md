---
title: 在Mac使用Wine运行Windows软件：PanDownload_Beat(非虚拟机)
date: 2020-02-26 00:12:42
toc: true
categories: 
 - [学习 - MacOS]
tags: 
 - WineHQ
---



　　**简  述：**　**在 MacOS10.14.6 中，安装 Wine 运行 Windows 软件百度网盘。详细讲述 Wine 的下载，安装，使用教程。** <font color=#0000FF size=4 face="幼圆">**本篇也是在 Mac下使用破解版不限速百度云 PanDownload_Beat ，直接运行该程序，非安装虚拟机方式使用。**</font>

<!-- more -->

[TOC]

### 运行环境：

　　**💻：**  `MacOS 10.14.6 ` 📎 `XQuartz >= 2.7.7 ` 📎 `winehq-stable-5.0` 📎 `PanDownload_Beat2.2.2`

<br>

### Wine 简述：

　　Wine （“Wine Is Not an Emulator” 的首字母缩写）是一个能够在多种 POSIX-compliant 操作系统（诸如 Linux，macOS 及 BSD 等）上运行 Windows 应用的兼容层。Wine 不是像虚拟机或者模拟器一样模仿内部的 Windows 逻辑，而是將 Windows API 调用翻译成为动态的 POSIX 调用，免除了性能和其他一些行为的内存占用，让你能够干净地集合 Windows 应用到你的桌面。

　　Wine　通过提供一个兼容层来将　Windows　的系统调用转换成与　POSIX　标准的系统调用。它还提供了　Windows　系统运行库的替代品和一些系统组件的替代品。为了避免著作权问题，Wine　主要使用黑箱测试逆向工程来编写。

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_193945.png" width="60%"/>



<font color=#D0087E size=5 face="幼圆">其相关官网：</font>

1. **wine：**

   官网：[https://www.winehq.org/](https://www.winehq.org/)

   github: [https://github.com/wine-mirror/wine](https://github.com/wine-mirror/wine)

2. **wineBottle:**

   官网：[https://winebottler.kronenberg.org/](https://winebottler.kronenberg.org/)

3. **WineSkin：**

   官网：[http://wineskin.urgesoftware.com/](http://wineskin.urgesoftware.com/)

4. **PlayOnMac:**

   [https://www.playonmac.com/en/](https://www.playonmac.com/en/)

5. **CrossOver：**

   官网：[https://www.codeweavers.com/](https://www.codeweavers.com/)



注：CrossOver 若想支持正版，请选择🇺🇸官网网址， 不要误进入某马丁杰克的代理的国内网址；

<br>

### 前期准备：

#### xquartz 介绍：

　　<font color=#FF0000  size=4 face="幼圆">**xquartz 是苹果系统中支持窗口界面的一个项目；** </font> 
**先决条件：**

1. XQuartz >= 2.7.7
2. 不能将Gatekeeper设置为阻止未签名的程序包。

**安装：** .pkg文件和tarball存档均可在[https://dl.winehq.org/wine-builds/macosx/download.html上获得](https://dl.winehq.org/wine-builds/macosx/download.html)。对于没有经验的用户，建议从.pkg文件安装。

　　Wine 的运行需要 `xquartz` 的支持，如果先没有安装 xquartz 就直接安装Wine 程序，就会直接报错。

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_113553.png" width="60%" />

#### 安装 xquartz，执行安装命令：

```bash
brew cask install xquartz //XQuartz可以使用安装；安装最后一步需要输入密码
```

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_114138.png" width="60%" />

<br>

### 下载 wine （MacOS 版）程序：

　　<font color=#FF0000  size=4 face="幼圆">开发和稳定分支的正式WineHQ软件包可用于macOS 10.8至10.14 ***（Wine不适用于macOS Catalina 10.15）***。</font>



官网：[https://dl.winehq.org/wine-builds/macosx/download.html](https://dl.winehq.org/wine-builds/macosx/download.html) 

地址：https://pan.baidu.com/s/1fjoK8BFANtEt5fzyD2Dx_Q  密码:4qrz

**版本说明：**

- "Wine Stable"：公开的稳定版（推荐）
- "Wine Development"：开发版本
- "Wine Staging"：在发布稳定版之前的最后一个测试版本

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_120637.png" width="60%"

<br>

### 安装 wine 程序：

1. 双击 winehq-stable-5.0.pkg 程序

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_114927.png" width="60%" />

2. 选择为所有用户安装

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_114953.png" width="60%" />

3. 勾选 64bit，用于可以安装 windows 的 64 位软件

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_115047.png" width="60%" />

4. 等待安装过程

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_115103.png" width="60%" />

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_115117.png" width="60%" />

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_115124.png" width="60%" />

5. 安装成功

   安装成功会在启动器里面，出现如下如的图标；但是启动之后

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_115149.png" style="zoom:50%;" />

<br>

### 运行 wine 程序：

　　Wine Stable 这个软件，启动之后，并不会有常见的 GUI 界面，只有一个终端的界面，并且运行该软件只能够使用命令；幸运的是，命令非常简单

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_115313.png" width="70%"/> 



运行我想要安装的 `BaiduNetdisk_6.8.9.1.exe` （带路径）程序，在终端输入如下命令：

```bash
wine /Users/muli/softInstll/BaiduNetdisk_6.8.9.1.exe
```

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_115703.png" width="70%" />



运行之后，会发现没有更新 wine 的配置，要安装 Wine Gecko 安装器，点击安装即可。下载速度非常之慢，挂着梯子耐心等待；若是中间失败，可以点击取消，多次重复安装，一直到等到全部下载和安装成功。

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_115800.png" width="70%"/>

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_120422.png" width="70%"/>



安装成功：

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_133245.png" width="70%"/>



也可以执行命令 `winecfg`，来进行修改配置文件：

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_122856.png"  width="50%" />



就是享受使用的成果时候，在 MacOS 里面使用该 windows 的百度网盘， 享受 mac 没有的隐藏空间功能和正常的使用快乐。可以看到，在 svip 的加速下，可以实现高速下载。 

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_192533.png" width="90%"/>

<br>

### 用 wine 运行 win(官网百度网盘) 程序：

- **方式一：**

  ```bash
  //执行命令
  wine 路径+xxx.exe  
  ```

  

- **方式二：**

  鼠标右键执行，如图

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200225_200943.png"  width="70%"/>

<br>

### 用 wine 运行 win(破解PanDownload) 程序【非虚拟机】：

下载 PanDownload_v2.2.2 版本之后，打开文件，运行如下，然后扫码登录自己的账号，自行下载，可见速度可以没有被线速。

 <img src="https://i.imgur.com/7qOjcx1.png" width="70%"/>



 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_mark_Snip20200306_123148.png" width="90%"/>

<br>

### PanDownload_v2.2.2下载地址：

- **PC 内测版：** 
  - 链接:https://pan.baidu.com/s/1FNdXvzoWJX4JfWSsaJsoUw  密码:87d1



- **PC 正式版：**
  - 链接:https://pan.baidu.com/s/1vSxzgw2wWHb5fQ_OCfoc5w  密码:nibj



- **Android 版本：** 
  - 链接:https://pan.baidu.com/s/1DDPwdpBpA0oP7uJgTAdsdw  密码:k69u



更新时间 20202-03-06

<br>

### 点个赞？

不看看本小结的小标题？





