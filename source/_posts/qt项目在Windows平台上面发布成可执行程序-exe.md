---
title: qt项目在Windows平台上面发布成可执行程序.exe
date: 2019-7-18 18:56:13
updated: 2019-7-18 18:56:17
toc: true
categories: 
 - [学习 - 项目实战开发]
 - [学习 - c/c++]
 - [学习 - qt]
 
tags: 
 - 项目实战开发
 - c/c++
 - qt
---



​		**简介：**  `qt`项目在`Windows`平台上面发布成可执行程序`.exe`，或是免安装的绿色版本、亦或者安装形式的安装包。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		 `qt`项目在`Windows`平台上面发布成可执行程序`.exe`，或是免安装的绿色版本、亦或者安装形式的安装包。亲测有效。测试项目参见 [QT5/C++项目：基于QT的跨平台网络对战象棋](https://blog.csdn.net/qq_33154343/article/details/89284983) 

<font color=#FE7207    size=4 face="幼圆">**实现了QT5的项目在windows、Linux、MacOS、Android平台的发布**</font> 



## 该博文系列:

- [qt项目在MacOS平台上面发布成可执行程序.app](https://blog.csdn.net/qq_33154343/article/details/96448938) 
- [qt项目在Linux平台上面发布成可执行程序.run](https://blog.csdn.net/qq_33154343/article/details/96448621) 
- [qt项目在Windows平台上面发布成可执行程序.exe](https://blog.csdn.net/qq_33154343/article/details/96448388) 
- [qt项目在Android平台的发布(未单独列举出来)](https://blog.csdn.net/qq_33154343/article/details/89286553) 



## 开发平台环境：

**编程环境：**  `win10 x64 专业版`  `windows7 x64 旗舰版`

**编程软件：**  `visual studio 2015`， `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.7`



## <font color=#D0087E  face="幼圆">重要提示：</font>

<font color=#70AD47 size=4 face="幼圆">推荐点击文末的同步博客链接，查看本文，获得100%的浏览体验效果</font>

- 若遇csdn的本篇博文的排版异常，图片无法加载
- csdn中无法预览效果视频，已替换为视频链接
- 部分链接分享资源失效
- 部分文章查看失效
- <font color=#70AD47 size=4 face="幼圆">请点击<font color=#FE7207  size=4 face="幼圆">**本文末的同步链接**</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好体验</font> 



## 项目在Windows平台发布：

本次发布测试的平台为`Windows 7 x64` 和 `Windows 10 x64`。

步骤：

（1）打开`ChineseChess`项目。点击`Qt Creator`左下角的运行程序，选择构建的套件为：`Desktop Qt 5.9.7 MinGW 32bit`，再构建里面选择Release版本，点击绿色三角形图案。等待程序跑起来之后，关闭掉程序。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183408554.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)



(2)打开项目文件管理器，找到生成的便以文件。然后打开路径`D:\programming\qt\build-ChineseChess-Desktop_Qt_5_9_7_MinGW_32bit-Release\release(这里以我的路径为例)`。将里面的C`hineseChess.exe`复制一份，将拷贝的程序另外保存在一个名为英文（这里我为`Chess`）的空的文件夹里里面。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183420239.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183429782.png)



(3)双击运行`Qt 5.9.7 for Desktop (MinGW 5.3.0 32 bit)`，用cd命令进入到上一步创建的空文件夹路径（eg: `D:\Chess`）。然后运行命令

```c
windeployqt+ 运行程序名

（eg：windeployqt ChineseChess.exe）
```

,回车，将所需的库文件全都拷贝到该.exe程序的当前文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183437790.png)            ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183445970.png)



![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183453573.png)



(4)双击运行`Enigma+Virtual+Box+7.80`程序，在这里面，在“主程序文件名称里面”选中刚刚的拷贝版本`D:\Chess\ChineseChess.exe`。然后会自动生成输出虚拟文件名称的路径。在文件的`Virtual Box Files`里面，将上一步骤的上面。然后点击右下角的“文件选项-压缩文件”。最后一步点击右下角的打包。件夹里面生成许多库等，全选，除了`ChineseChess.exe`以外，全部拖进这里

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183502168.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)



(5)等待一分钟，生成的绿色单机版，免安装的有游戏.exe文件,放到任意一个没有任何环境的`windows`系统的都可以跑起来。不会提示那种缺少`xxx.dll`的错误提示。完美打包发布这一份作品。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183509128.png)![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183515850.png)



![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183522403.png)



![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183530128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)



![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183537181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)



## 本次心得总结：

将该部分从从之前的一篇之前的 [QT5/C++项目：基于QT的跨平台网络对战象棋](https://blog.csdn.net/qq_33154343/article/details/89284983) 原创文章之中，分离开来，感觉还是比较有用一篇文章。



## 参考博文：

因为有着热心网友的无私分享，故不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 



## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到csdn博客：**</font> [qt项目在Windows平台上面发布成可执行程序.exe](https://blog.csdn.net/qq_33154343/article/details/96448388)

