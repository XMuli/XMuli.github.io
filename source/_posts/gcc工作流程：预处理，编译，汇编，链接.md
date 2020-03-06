---
title: gcc工作流程：预处理，编译，汇编，链接
date: 2020-03-04 14:49:24
toc: true
categories: 
 - [学习 - Linux]
---



　　**简  述：**　<font color=#D0087E size=4 face="幼圆">**在 uinx/Linux 下，使用 gcc 的工作流程：预处理，编译，汇编，链接。**</font> 这里实际测试，举例分析：使用 g++（用 c++）的编译 main.cpp ，最终得到可执行程序的过程分析。

<!-- more -->

[TOC]

### 编程环境：

　　**💻：**  `MacOS 10.14.6 (18G103)` 📎 `gcc/g++ 9.2.0` 

<br>

### gcc工作流程：

- 预处理(`gcc -E`;  **预处理器cpp**)  :`xxx.c --> xxx.i`  (.i 文件本质还是.c 文件)
  - 宏替换
  - 头文件展开
  - 注释去掉

- 编译（`gcc -S`; **编译器 gcc**）: `xxx.i --> xxx.s`
  - 生成汇编文件
- 汇编（`gcc -c`； **汇编器as**）: `xxx.s  -->  xxx.o`
  - 生成二进制文件

- 链接（`gcc`； **链接器ld**）
  - `xxx.o  -->  xxx（可执行）` 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_image-20200118120643285.png"/>

<br>

### gcc常用参数：

- `-v/--version：` 查看版本
- `-I：` 指定包含头文件路径（推荐相对路径）
- `-c： ` 汇编文件生成二进制文件 
- `-o： ` 指定生成文件的名字
- `-g：` gcc调试时候，添加的参数
- `-D： ` 编译的时候，生成一个你所指定的宏（场景：多使用在测试程序中）
- `-Wall： ` gcc 编译时候，添加显示警告⚠️信息
- `-On：` 优化代码，n 是优化级别：1，2，3（3 就是最大，填写 100 也是 3）

<br>

### 例子验证：

- **创建c++源文件：**

  创建一个 main.cpp 的源文件，内容如下：

  ```cpp
  #include <iostream>
  
  int main(int argc, const char * argv[]) {
      // insert code here...
      std::cout << "Hello, World!\n";
      return 0;
  }
  ```

<br>

- **gcc 预处理：**

  ```bash
  g++-9 -E main.cpp -o main.i 
  ```

   原始的 .cpp 文件：

​       <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200304_142044.png" width="50%"/>



- **gcc 编译：**

  ```bash
  g++-9 -S main.i -o main.s
  ```

   头文件展开，宏替换，去掉注释之后的 .cpp 文件。(.i 文件本质还是 .cpp 文件)

​     <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200304_142209.png" width="50%"/>



- **gcc 汇编：**

  ```bash
  g++-9 -c main.s -o main.o  //-c 是小写，非大写C
  ```

  生成的汇编文件：

     <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200304_142314.png" width="50%"/>

  

- **gcc 链接：**

  ```bash
  g++-9 main.s -o main
  ```

  生成的二进制文件（打开会看到乱码）：

     <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200304_142338.png" width="50%"/>

  

- **运行可执行程序：**

  ```bash
  ./main
  ```

  终端看到输出结果：Hello, World!

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200304_140529.png" width="60%"/>