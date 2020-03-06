---
title: 在MacOS上面通过brew命令安装gcc编译器集
date: 2020-03-02 21:55:50
toc: true
categories: 
 - [学习 - Linux]
 - [学习 - MacOS]
---



　　**简  述：**　在 `MacOS 10.14.6` 里面使用命令 `brew` 下载、安装 `gcc` 编译器集。并且写一个小的例子 .cpp 文件来编译运行，下载的 gcc 是否成功。

<!-- more -->

[TOC]

### 编程环境：

　　**💻：**  `MacOS 10.14.6 (18G103)`

<br>

### GCC 简介：

<font color=#D0087E size=4 face="幼圆">GCC是库和前端的全面集合，使您能够将源代码编译成二进制应用程序。</font> 

GNU 编译器集合（简称**GCC）**包括 C，Objective-C，C ++，Java，Fortran，Go 和 Ada 的前端，以及所提及语言的库。

GCC 是 GNU 工具链的主要组成部分，它是根据 GNU 通用公共许可证发行的，在自由软件的持续增长中起着核心作用。

最初，GCC 仅处理 C 编程语言，但是随着其他前端的开发，GCC 扩展到可以编译 C++，Objective-C，Objective-C ++，Go，Fortran，Ada，Java等。

GCC 还提供对多种处理器体系结构的支持，因此，它经常用作免费和专有应用程序的开发工具。GCC 也可用于大多数嵌入式平台，包括 AMCC，Symbian 和基于 Freescale Power Architecture 的芯片。

GNU 编译器集合还针对各种平台，例如 Dreamcast 和 PlayStation 等视频游戏机。而且，GCC 是许多类Unix 操作系统的标准编译器，包括 Linux 和 BSD 系列，FreeBSD 和 LLVM 系统。

<br>

### GCC 官网：

[GCC for Mac 官网](https://mac.softpedia.com/get/Development/Compilers/GCC.shtml) 

<br>

### GCC 命令安装：

- **查看 gcc 版本：**

  ```bash
  brew search gcc
  ```

其中第一个为 gcc 默认的当前最高版本。

​	<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200302_210541.png" width="90%" alt="图床在此网络是憨憨，请稍等..."/>



- **安装 gcc :**

  ```bash
  brew install gcc
  ```

  默认是安装官网里面最高的版本，截图可知是 gcc 9.2

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200302_211247.png" width="80%" alt="图床在此网络是憨憨，请稍等..."/>

<br>

### 查看 gcc 安装版本:

```bash
gcc -v
```

发现显示的 gcc 这个名字，已经被占了，实际是通过映射来调用 clang ；

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200302_211758.png" width="80%" alt="图床在此网络是憨憨，请稍等..."/>



但其实是安装成功了的，通过一个命令的安装地址可知是 gcc 9.2 版本，所以，可以看出正确版本的查看命令：`gcc-9 -v`  （按下 Tab）；其中要调用正真的 gcc 工具就使用用 `gcc-9` , 而非 gcc（clang）；

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_mark_Snip20200302_211247.png" width="80%" alt="图床在此网络是憨憨，请稍等..."/>

<br>

### 验证 gcc 安装是否成功：

创建一个名称为 `main.cpp` 的文本，写入以下内容：

```cpp
//  Created by muli on 2020/3/2.
//  Copyright © 2020 muli. All rights reserved.
//

#include <iostream>

int main(int argc, const char * argv[]) {
    // insert code here...
    std::cout << "Hello, World!\n";
    return 0;
}
```



运行 `gcc-9 main.cpp -o mainApp -lstdc++`，用 gcc 编译生成 main.cpp 文件生成一个名称为 mainApp 的可执行程序「Linux 下的文件默认是没有后缀名的」；其中编译选项添加 -lstdc++ ，即使用标准C++库，否则你 Mac 下回编译失败。运行命令 `./mainApp` 执行 mainApp 程序，查看输出结果：

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200302_215146.png" width="50%" alt="图床在此网络是憨憨，请稍等..."/>



结论：使用 gcc 编译生成连接运行程序成功。

<br>

### 其它疑问：
关于上面执行 `gcc-9 main.cpp -o mainApp -lstdc++` 中，执行这个语句带上 `-lstdc++` 这个参数；始终觉得有点困惑，需要深究一下。 在下一篇中再作探究。

<br>

### 源码下载：

[01_test_gcc](https://github.com/touwoyimuli/linuxExample/tree/master/01_test_gcc)

<br>

> <font color=#D0087E  size=4 face="幼圆">**📌本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [在MacOS上面通过brew命令安装gcc编译器集](https://blog.csdn.net/qq_33154343/article/details/104639656) 





