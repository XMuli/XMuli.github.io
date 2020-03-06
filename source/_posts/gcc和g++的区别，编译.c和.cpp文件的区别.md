---
title: gcc和g++的区别，编译.c和.cpp文件的区别
date: 2020-03-03 20:09:42
toc: true
categories: 
 - [学习 - Linux]
 - [学习 - MacOS]
---



　　**简  述：**　在[上一篇](https://blog.csdn.net/qq_33154343/article/details/104639656)中，最后为了测试 gcc 9.2.0 版本的安装是否成功。对创建的测试文件 main<font color=#D0087E size=4 face="幼圆">.cpp</font> 文件，执行 `gcc-9 main.cpp -o mainApp -lstdc++` 命令；虽然最后运行成功了；但是是始终是有一点困惑：

- 调用 gcc 和 gcc-9❗️
- 能否自动执行 gcc 的时候，去掉 `-lstdc++`，使得看起来清爽起来 ⁉️

最后查询一下。发现其中另有一番天地，差点就错过了。 **本章主要讲解 gcc 和 g++的区别，编译.c和.cpp文件的区别** ❓

<!-- more -->

[TOC]

### 编程环境：

　　**💻：**  `MacOS 10.14.6 (18G103)` 📎 `gcc 9.2.0`

<br>

### 背景：

因为博主使用的是 MacOS 系统，也就是 Uinx 系统，和 Linux 还是有一点区别的。

在上面执行 `gcc-9 main.cpp -o mainApp -lstdc++` 的命令中，-lstdc++ 表示会去链接 STL 库。查询可知，是 gcc 和 g++ 对 .c .cpp 文件的执行严格程度不一样造成的；且还有是否会自动链接 STL 库的区别造成。

<br>

### MacOS 下 gcc 和 clang 区别：

Mac 在是自带 gcc 命令的（安装 xcode 后）。但这并非说真的有 gcc 编译器群，只是将终端里面的命令 gcc 映射调用 clang 编译器。在昨天我下载了真正的 gcc9.2.0 之后，调用它，就必须的使用 gcc-9 这个命令。



<font color=#0000FF size=4 face="幼圆">**为了后面的学习的完整性；后面坚持使用 gcc 9.2.0 在 MacOS 10.14 下进行 Linux 的相关命令学习。偶尔也会将 uos20（Debain 8） 系统作为验证一些命令或者习惯的使用。**</font> 



<font color=#0000FF size=4 face="幼圆">**以后文章中的 gcc 指代的就是终端中调用的 gcc-9；文章中的 g++ 指代的就是终端中调用的 g++-9；**</font>



也就是坚持在终端里面调用 gcc-9 和 g++-9 ；

<br>

### gcc 和 g++的区别：

首先看一下两者的区别：

GCC:GNU Compiler Collection(GUN 编译器集合)，它可以编译C、C++、JAV、Fortran、Pascal、Object-C、Ada等语言。

gcc是GCC中的GUN C Compiler（C 编译器）

g++是GCC中的GUN C++ Compiler（C++编译器）

一个有趣的事实就是，就本质而言，gcc和g++并不是编译器，也不是编译器的集合，它们只是一种驱动器，根据参数中要编译的文件的类型，调用对应的GUN编译器而已，比如，用gcc编译一个c文件的话，会有以下几个步骤：

Step1：Call a preprocessor, like cpp.

Step2：Call an actual compiler, like cc or cc1.

Step3：Call an assembler, like as.

Step4：Call a linker, like ld

由于编译器是可以更换的，所以gcc不仅仅可以编译C文件

所以，更准确的说法是：**gcc调用了C compiler，而g++调用了C++ compiler**

**gcc和g++的主要区别：** 

1. 对于 *.c和*.cpp文件，gcc分别当做c和cpp文件编译（c和cpp的语法强度是不一样的）

2. 对于 *.c和*.cpp文件，g++则统一当做cpp文件编译

3. 使用g++编译文件时，**g++会自动链接标准库STL，而gcc不会自动链接STL**

4. gcc在编译C文件时，可使用的预定义宏是比较少的

5. gcc在编译cpp文件时/g++在编译c文件和cpp文件时（这时候gcc和g++调用的都是cpp文件的编译器），会加入一些额外的宏，这些宏如下：

   ```cpp
   #define __GXX_WEAK__ 1
   #define __cplusplus 1
   #define __DEPRECATED 1
   #define __GNUG__ 4
   #define __EXCEPTIONS 1
   #define __private_extern__ extern
   ```

6. 在用gcc编译c++文件时，为了能够使用STL，需要加参数 –lstdc++ ，但这并不代表 gcc –lstdc++ 和 g++等价。

<br>

### 再次实践：

<br>

创建 main.c 和 main.cpp 两个文件；里面内容都为

```cpp
#include <iostream>

int main(int argc, const char * argv[]) {
    // insert code here...
    std::cout << "Hello, World!\n";
    return 0;
}
```



#### 使用 gcc 编译 .c 文件：

运行 `gcc-9 main.c ` 和 `gcc-9 main.c -lstdc++` 都编译不过。运行结果：

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200303_204442.png" width="50%" alt="图床在此网络是憨憨，请稍等..."/>

**结论：** 即使添加 -lstdc++ 依旧报错，因为 iostream 是属于 c++的内容，*.c 文件使用 gcc 会当做 C语言的严格程度和范围来编译。<font color=#FF0000  size=4 face="幼圆">**c语言** 应该用 stdio.h，**c++** 才用 iostream</font> 



#### 使用 gcc 编译 .cpp 文件：

运行 `gcc-9 main.cpp` 编译失败；运行 `gcc-9 main.cpp -lstdc++` 编译运行成功。运行结果如下：

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200303_205222.png" width="70%" alt="图床在此网络是憨憨，请稍等..."/>

**结论：**  gcc 都是 将 cpp 按照 c++ 的严格程度和范围来编译，但是不会自动链接 STL 库。



#### 使用 g++ 编译 .c 文件：

运行命令，运行结果：

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200303_210956.png" width="50%" alt="图床在此网络是憨憨，请稍等..."/>

**结论：** g++ 将 .c 按照 c++ 的严格程度和范围来编译，且会自动链接 STL 库。



#### 使用 g++ 编译 .cpp 文件「推荐」：

运行 `g++-9 main.cpp`，简直太清爽了。运行结果：

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200303_210444.png" width="50%" alt="图床在此网络是憨憨，请稍等..."/>

**结论:**  g++ 用来编译 .cpp 文件是最为清爽的和合适的。

<br>

### 总结：

gcc 和 g++ 都是根据 文件的后缀名称 .c .cpp；来判断是否以 C语言 还是 c++ 来编译文件。

且**g++会自动链接标准库STL，而gcc不会自动链接STL** 。



以后更推荐使用 g++。

<br>

### 源码下载：

[02_c_cpp](https://github.com/touwoyimuli/linuxExample/tree/master/02_c_cpp) 

**参考文章：**

[https://www.zhihu.com/question/20940822/answer/536826078](https://www.zhihu.com/question/20940822/answer/536826078) 

<br>

> <font color=#D0087E  size=4 face="幼圆">**📌本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [gcc和g++的区别，编译.c和.cpp文件的区别](https://blog.csdn.net/qq_33154343/article/details/104645129)





