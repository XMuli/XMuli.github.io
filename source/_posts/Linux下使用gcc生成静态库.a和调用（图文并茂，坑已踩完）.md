---
title: Linux下使用gcc生成静态库.a和调用（图文并茂，坑已踩完）
date: 2020-03-04 23:37:22
toc: true
categories: 
 - [学习 - Linux]
---



　　**简  述：** 　在Linux系统下，使用gcc来编译，生成静态库，且调用静态库.a文件，生成可执行程序。此处例子实际：使用g++9.2在mac平台下完成这个知识点的教程。在 [下一篇](https://blog.csdn.net/qq_33154343/article/details/104692370) 踩坑生成动态库的.so制作和使用。

<!-- more -->

--- -

[TOC]

### 编程环境：

　　**💻：**  `MacOS 10.14.6 (18G103)` 📎 `gcc/g++ 9.2.0` 

<br>

### 静态库概念：

**静态库** 是在可执行程序运行之前就已经加入到执行代码中，成为执行程序的一部分；静态库的后缀一般是 `.a`作为后缀。



**库：** 二进制文件；是加密之后的.c/.cpp的文件

**静态库：** `.lib 或 .a`

<br>

### 前期铺垫：

创建如下例子几个文件：注意是.cpp文件，里面写的也是c++的语法。

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/20200308003020.png" width="35%"/>

<br>

### 将.cpp生成.o：

- 执行命令 `g++-9 -c *.cpp` 将所有.cpp 文件都编译不链接生成对应的.o；且将.o文件都存放于lib文件夹下。

  ```bash
   g++-9 -c *.cpp  //注意 -c 是小写，非大写
   mv *.o ./lib 
  ```

  <br>

### 将.o生成.a：

- 执行命令 `ar cr libxxx.a ./lib/*.o` 将所有的.o文件全部归档，然后替换创建为一个静态文件.a；<font color=#D0087E size=4 face="幼圆">ar 是GNU 归档工具，rcs 表示（replace and create）</font> 

  ```bash
  #用法：ar rcs 静态库的名字 原材料  
  #ar -archive
  
  ar cr libxxx.a ./lib/*.o  //生成静态库文件.a
  mv *.h ./include          //将所有的.h文件放在include文件夹下
  mv libxxx.a ./lib         //将.a文件也放在lib文件夹下
  ```

  

  此时的工程结构如下:

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/20200308003054.png" width="35%"/>

  <br>

### 链接库，调用静态函数.a：

- 执行命令 `g++-9 -I ./include -L ./lib -lxxx -o mainApp` 调用生成的.a文件，并且链接，生成可执行程序mainApp

  - <font color=#D0087E size=4 face="幼圆">注意，所要生成的.a文件的名字前三位必须是lib，否则在链接的时候，就可能导致找不到这个库</font> 
  - **命令规则：** <font color=#FF0000  size=4 face="幼圆">lib</font>xxx<font color=#FF0000  size=4 face="幼圆">.a</font> ；<font color=#D0087E size=4 face="幼圆">**红色部分为固定的格式，中间的xxx才是库名**</font>
  - <font color=#D0087E size=4 face="幼圆">-l 为调用的库的名称(-l 后面没有空格，直接加库名)</font>

  ```bash
  #-I 为指定的库的头文件的路径
  #-L 为指定的库的二进制文件文件
  #-l 为调用的库的名称(-l 后面没有空格，直接加库名)
  #-o 输出文件，生成的可执行程序的名称
  
  g++-9 -I ./include -L ./lib -lxxx -o mainApp
  ```



- 此时工程的结构：

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/20200308003118.png" width="35%"/>



- 运行成功可以查看.a文件里面的内容，本质是把其余的.o文件全部塞到了.a里面

  ```bash
  #成功之后，可以查看.a里面的包含具体的所有的.o文件 
  nm libmytest.a
  ```

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/20200308003143.png" width="50%"/>    

<br>

### 运行可执行程序成功：

运行命令 `./mainApp`，执行可执行程序，能够看到结果输出，表示成功。

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/20200308003233.png" width="80%"/>

<br>

### 源码下载：

[04_make_so_a](https://github.com/xmuli/linuxExample/tree/master/04_make_so_a)

<br>

### 总结：

看起来很简单，**殊不知，纸上学来终觉浅，** 还是亲自实现一番才以记忆深刻。若是成功的白嫖到了这篇文章，可以**点个赞，** 给我留个言，鼓励一下。你的加油，我们持续不断的出新的文章，那啥，白嫖一时爽，一直白嫖一直 。🥺



