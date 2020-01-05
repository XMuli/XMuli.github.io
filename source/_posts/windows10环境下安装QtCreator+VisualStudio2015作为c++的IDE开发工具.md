---
title: windows10环境下安装QtCreator+VisualStudio2015作为c++的IDE开发工具
date: 2019-12-26 21:36:55
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  `windows`环境下安装`Qt Creator` + `Visual Studio 2015`作为`c++`的`IDE`开发工具，学习和使用**qt** (备选)； **本文详细介绍安装VS2015的过程和安装番茄助手Visual Assist X；以及如何配置QtCreator的环境和插件**，使得可以在VS2015里面运行Qt的程序，使用Qt自带的设计师等。

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [windows10环境下安装QtCreator+VisualStudio2015作为c++的IDE开发工具](https://blog.csdn.net/qq_33154343/article/details/103755569)

<br>

## 相关博文：

**有着一些关联的博文其他参考：**

- [在win10环境下安装QtCreator5.9.8作为c++的IDE开发工具](https://blog.csdn.net/qq_33154343/article/details/103674579)

<br>

## 系统环境：

**编程环境：**  `win10 x64 专业版 1903`  

**编程软件：** `Qt 5.9.8`， `Qt Creator 4.8.2`， `Visual Studio 2015(专业版)`

<br>

## 下载安装QtCreator:

参考本篇详细文章：[在win10环境下安装QtCreator5.9.8作为c++的IDE开发工具](https://blog.csdn.net/qq_33154343/article/details/103674579)

<br>

## 下载Visual Studio 2015专业版：

官网纯净版下载，推荐地址：[https://msdn.itellyou.cn/](https://msdn.itellyou.cn/)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191223230926.png)

其中详细链接如下：

```bash
文件名：
cn_visual_studio_professional_2015_with_update_3_x86_x64_dvd_8923256.iso

校验文件完整性SHA1值：
99E6C061FFB3194D28682D75D5F2F0F12A8D614F

文件大小：
7.21GB

发布时间：
2016-06-27

磁力链接：
ed2k://|file|cn_visual_studio_professional_2015_with_update_3_x86_x64_dvd_8923256.iso|7745202176|DD35D3D169D553224BE5FB44E074ED5E|/
```

<br>

## 安装VS2015专业版：

<font color=#D0087E size=5 face="幼圆">**下载好完整的离线安装包之后，建议断开网络运行安装程序;避免联网下载新更新文件，导致安装时间变得更长；** </font>

以管理员身份运行VS2015的运行程序：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191223232608.png)



- 选择 **自定义（安装功能）**，然后点击 **下一步**：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191223233207.png)

- 选择所需要的的模块（**Visual C++** 模块全选）

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191223233754.png)



- 点击 **安装**：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191223234550.png)



- 耐心等待，一般固态约20min左右即可安装完成：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191223234705.png)



- 启动：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191224001504.png)

<br>

## 安装VS小番茄助手Visual Assist X 10.9.2248：

番茄小助手，搭配VS使用，使用起来，不仅会港剧编码效率有所提高，效率变快，且会感觉寿命都在增加；

 **分享一个破解版本：链接: https://pan.baidu.com/s/1ZHKufAQ6IzbdIcUT-7k5kg 提取码: h4c2**

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191224002041.png)

**正常安装和破解方法：**

- **第一步：安装2248版本**
  （1）解压缩。共有两个文件（ **VA_X_Setup_10-9-2248.exe** 和  **readMe.txt**）和一个文件夹（Crack）。
  （2）关闭VS所有打开界面。
  （3）双击 **VA_X_Setup_10-9-2248.exe**安装Visual Assist 2248版本。

- **第二步：破解**
  （1）打开C盘。地址栏中输入：`C:\Users\`用户名，如果没看到`AppData`目录，直接在地址栏中输入`\AppData`再回车确认进入该目录。
  （2）寻找目标位置。打开`Local\Microsoft\VisualStudio\15.0_281df2b7\Extensions\`一个目录。
  注意：
  1.本地安装的VS版本为2017，所以本地的文件夹名称为15.0_281df2b7。若是VS其他版本可能会有差异。
  2.“一个目录”为安装时自动生成的，名称随机。
  3.因为“一个目录”的名称不固定，建议到Extension目录下时，搜索名称为 **“VA_X.dll”**的文件，然后确定目标位置。
  （3）重命名。找到 **“VA_X.dll”**文件后，不要随便删除。重命名为`VA_X_old.dll`作为备份（ps:避免以下步骤4之后导致VS打不开的尴尬局面！）
  （4）替换。拷贝Crack文件夹中的 **VA_X.dll**到该文件夹中。

**注意：比较懒或者实在找不到目标位置，请参考以下通用方法：**
（1）下载Everything.exe，一路下一步安装完。
（2）在搜索框中输入VA_X.dll，回车。
（3）用解压文件夹Crack中的VA_X.dll替换掉搜索出来的VA_X.dll。（一般这里会有两个VA_X.dll，通过看路径判断哪个是破解版，哪个是安装的原版） 

- **第三步：重启VS**

<br>

## 安装“VS-Qt插件”：

打开VS2015，选中 **“工具-扩展和更新”**，（此时保证电脑处于联网状态）

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191224002217.png)



- 在搜索栏里面输入“qt”，然后选中如下图这个，点击下载和安装：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191224002912.png)

- 点击配置，添加生成的Qt的文件夹目录：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191224004107.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191224004155.png)

<br>

## 验证安装成功：



![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191224004530.png)

- 点击下一步，按照如下图所示的方式，创建一个新的Qt的工程项目（后面会单独写一篇，介绍QtCraetor和使用VS2015创建一个项目，和解析深究这些IDE里面常用的功能和区分模块等）：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191224004609.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191224004728.png)

- 运行成功：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191226203330.png)