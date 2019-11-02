---
title: qt项目在MacOS平台上面发布成可执行程序.app
date: 2019-7-18 19:30:21
updated: 2019-7-18 19:30:21
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



​		**简介：**  `qt`项目在`MacOS`平台上面发布成可执行程序`.app`，或是免安装的绿色版本、亦或者安装形式的安装包。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		 `qt`项目在`MacOS`平台上面发布成可执行程序`.app`，或是免安装的绿色版本、亦或者安装形式的安装包。亲测有效。测试项目参见 [QT5/C++项目：基于QT的跨平台网络对战象棋](https://blog.csdn.net/qq_33154343/article/details/89284983) 

<font color=#FE7207    size=4 face="幼圆">**实现了QT5的项目在windows、Linux、MacOS、Android平台的发布**</font> 



## 该博文系列:

- [qt项目在MacOS平台上面发布成可执行程序.app](https://blog.csdn.net/qq_33154343/article/details/96448938) 
- [qt项目在Linux平台上面发布成可执行程序.run](https://blog.csdn.net/qq_33154343/article/details/96448621) 
- [qt项目在Windows平台上面发布成可执行程序.exe](https://blog.csdn.net/qq_33154343/article/details/96448388) 
- [qt项目在Android平台的发布(未单独列举出来)](https://blog.csdn.net/qq_33154343/article/details/89286553) 



## 开发平台环境：

**编程环境：**  `MacOS 10.14 x64` 

**编程软件：**  `Qt Creator 4.7.1 (Enterprise)`， `Qt 5.9.7`



## <font color=#D0087E  face="幼圆">重要提示：</font>

<font color=#70AD47 size=4 face="幼圆">推荐点击文末的同步博客链接，查看本文，获得100%的浏览体验效果</font>

- 若遇csdn的本篇博文的排版异常，图片无法加载

- csdn中无法预览效果视频，已替换为视频链接

- 部分链接分享资源失效

- 部分文章查看失效

- <font color=#70AD47 size=4 face="幼圆">请点击<font color=#FE7207  size=4 face="幼圆">**本文末的同步链接**</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好体验</font> 

    



## 项目在MacOS平台发布：

（1）同样子，在`MacOS`的`Qt Creator`里面编译运行`Release`版本的`ChineseChess`项目。



（2）打开`build-ChineseChess-Desktop_Qt_5_9_7_clang_64bit-Release`文件夹。进到该目录，会看到有一个`ChineseChess.app`。这个并不是文件，而是一个目录，只是`OS X`系统看到某个目录的扩展名是`app`，就会将其认为是`Bundle`目录，所以双击会直接执行（当必须要是真正的`Bundle`）。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183714131.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)                                 



（3）使用`QT`提供了一个非常方便的打包工具`macdeployqt`，该文件位于如下目录
 `/Users/yuanyi/Qt5.9.7/5.9.7/clang_64/bin`，可以将这个目录加入到`PATH`环境变量中。这里有点复杂，（需要注意的是，`Unix`【`MacOS`就是其中一种】及类`Unix`系统里，每行结尾只有换行`“\n”`，`Windows`系统里面，每行结尾是换行+回车`“\n\r”`）。当在终端里面，将上面目录路径添加到`PATH`之后。输入一下命令在终端。

```c
:set ff=unix #转换为unix格式
:wq #保存、退出
方可保存和退出成功。
```



（4）现在只需要执行如下的命令，系统就会自动该着`Bundle`，把相关的文件和目录都放到`Bundle`中的相关位置。命令语句如下：

```c
 macdeployqt ChineseChess.app
```



（5）处理完后，`Bundle`的目录结构发生改变，很明显，`macdeployqt`命令将相关文件和目录都放到了`Bundle`中。现在将这个处理完的`ChineseChess.app`复制到任何`OS X`系统上都可以运行了，无论安装没安装`QT`，都可以运行

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183751297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183728437.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183735290.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183741864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)



## 本次心得总结：

将该部分从从之前的一篇之前的 [QT5/C++项目：基于QT的跨平台网络对战象棋](https://blog.csdn.net/qq_33154343/article/details/89284983) 原创文章之中，分离开来，感觉还是比较有用一篇文章。



## 参考博文：

因为有着热心网友的无私分享，故不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 



## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到csdn博客：**</font> [qt项目在MacOS平台上面发布成可执行程序.app](https://blog.csdn.net/qq_33154343/article/details/96448938)

