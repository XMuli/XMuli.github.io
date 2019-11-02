---
title: qt项目在Linux平台上面发布成可执行程序.run
date: 2019-7-18 19:30:21
toc: true
categories: 
 - [学习 - 项目实战开发]
 - [学习 - c/c++]
 - [学习 - qt]
tags: 
 - c/c++
 - qt
 - QTreeWidget
 - 复选框
---



​		**简介：**  `qt`项目在`Linux`平台上面发布成可执行程序`.run`，或是免安装的绿色版本、亦或者安装形式的安装包。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​	 `qt`项目在`Linux`平台上面发布成可执行程序`.run`，或是免安装的绿色版本、亦或者安装形式的安装包。亲测有效。测试项目参见 [QT5/C++项目：基于QT的跨平台网络对战象棋](https://blog.csdn.net/qq_33154343/article/details/89284983) 

<font color=#FE7207    size=4 face="幼圆">**实现了QT5的项目在windows、Linux、MacOS、Android平台的发布**</font> 



## 该博文系列:

- [qt项目在MacOS平台上面发布成可执行程序.app](https://blog.csdn.net/qq_33154343/article/details/96448938) 
- [qt项目在Linux平台上面发布成可执行程序.run](https://blog.csdn.net/qq_33154343/article/details/96448621) 
- [qt项目在Windows平台上面发布成可执行程序.exe](https://blog.csdn.net/qq_33154343/article/details/96448388) 
- [qt项目在Android平台的发布(未单独列举出来)](https://blog.csdn.net/qq_33154343/article/details/89286553) 



## 开发平台环境：

**编程环境：**  `Ubuntu 16.04.5 x64 （LTS）`  

**编程软件：**   `Qt Creator 4.7.1 (Enterprise)`， `Qt 5.9.7`



## <font color=#D0087E  face="幼圆">重要提示：</font>

<font color=#70AD47 size=4 face="幼圆">推荐点击文末的同步博客链接，查看本文，获得100%的浏览体验效果</font>

- 若遇csdn的本篇博文的排版异常，图片无法加载

- csdn中无法预览效果视频，已替换为视频链接

- 部分链接分享资源失效

- 部分文章查看失效

- <font color=#70AD47 size=4 face="幼圆">请点击<font color=#FE7207  size=4 face="幼圆">**本文末的同步链接**</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好体验</font> 

  ​    



## 项目在Linux平台发布：

**步骤：Linux发布免安装的运行程序**

（1）同样，将完整的`ChineseChess`项目在`Linux`用`Qt Creator`编译运行`Release`版本。
           ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183546108.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)                                        



（2）但是编译不通过，会出现两个错误提示。

```c
error:-1: error: cannot find –lGL和
error:-1: error: collect2: error: ld returned 1 exit status。
```

原因：**是这是由于 `Qt5.0` 以上的版本默认将`OpenGL`加入了工程，但是在`Linux`的机器上没有安装`OpenGL`，所以只需要在机器系统安装`OpenGL`即可，此时打开终端，运行**

```c
sudo apt-get install libgl1-mesa-dev
```

**即可，再次编译运行。即可成功。**



（3）同样将生成的`Release`文件夹下`ChineseChess`文件放到一个`英文文件夹(eg: qwer)`里面.



（4）在上一步的文件夹中新建文件pack.sh。内容如下：

```c
\#!/bin/sh  
exe="ChineseChess" #你需要发布的程序名称
des="/home/yuanyi/Desktop/qwer" #步骤1中的目录即本文件所在目录
deplist=$(ldd $exe | awk  '{if (match($3,"/")){ printf("%s "),$3 } }')  
```



（5）cp $deplist $des
 在此目录下再新建一个ChineseChess.sh文件（文件名必须与可执行文件名字一样）， 文件内容如下（不需要修改）：

```c
\#!/bin/sh  
appname=`basename $0 | sed s,\.sh$,,`  
dirname=`dirname $0`  
tmp="${dirname#?}"  
if [ "${dirname%$tmp}" != "/" ]; then  
dirname=$PWD/$dirname  
fi  
LD_LIBRARY_PATH=$dirname  
export LD_LIBRARY_PATH  
$dirname/$appname "$@"
```



（6）打开终端，执行命令如下

```c
chmod +x ChineseChess.sh 
或者 ./ ChineseChess.sh 
```

会自动`ChineseChess.sh`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183638377.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)



（7）将此目录打包发布即可，点击即可以执行。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183645406.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183653888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413183701746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzMTU0MzQz,size_16,color_FFFFFF,t_70)



## 本次心得总结：

将该部分从从之前的一篇之前的 [QT5/C++项目：基于QT的跨平台网络对战象棋](https://blog.csdn.net/qq_33154343/article/details/89284983) 原创文章之中，分离开来，感觉还是比较有用一篇文章。



## 比心分享：

因为有着热心网友的无私分享，故不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)



## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到csdn博客：**</font> [qt项目在Linux平台上面发布成可执行程序.run](https://blog.csdn.net/qq_33154343/article/details/96448621) 

