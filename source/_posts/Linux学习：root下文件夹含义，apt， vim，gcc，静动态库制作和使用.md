---
title: Linux学习:root，apt，vim，gcc，静动态库制作和使用
date: 2020-01-24 21:03:01
toc: true
categories: 
 - [学习 - Linux]
---



**简  述：**  Linux学习：root下文件夹含义，apt， vim，gcc，静动态库制作和使用

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [Linux学习：root下文件夹含义，apt， vim，gcc，静动态库制作和使用](https://blog.csdn.net/qq_33154343/article/details/104081624)

<br>



## 磁盘文件夹含义：

> **几个比较重要的的文件夹含义，  root根的结构**
> cd 
> .   => 当前目录
> ..  => 当前上一级的目录
>
> -[英文短横] => 在两个临近的目录之间直接进行切换
> ~   => /home 里面， 用户的家目录（宿主目录）     pwd直接切换过去
>
> $: 当前用户为普通用户
>
> #：超级用户 -- root   (使用exit命令退出超级用户)

一些其他的文件夹📂含义：

- `/dev` 一切皆抽象为文件
- `/etc` 全局配置文件
- `/usr` 
  - `/usr/local` 第三方软件安装 
  - `/opt`
- `/bin`
- `/lib` 作用
- `~` 当前用户的顶层目录，比如：`/Users/muli` 
- 创建一个软连接的命令； ln 推荐绝对路径 软连接名称 

**一个 ls -al 命令查看文件夹下所有文件的详细：**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200118_174025.png"/>

<br>

## apt软件/安装卸载：

- `apt install/remove 软件名` 卸载软件

- /var/cache/apt/archives    `apt clean` 清空缓存

- `sudo dpkg -i xxx.deb` 安装 .deb 软件
- `sudo dpkg -r 软件的名字`  卸载 .deb 软件

<br>

## Vim:

### vim 三种模式切换图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_20200117000312942.png" style="zoom:50%;" />

<br>

### vim 使用：

- 保存之后退出：`ZZ`
- 代码格式化 `gg=G`
- 四个方向 `hjkl`
- 移动光标，行首尾，顶行，末行，跳转指定213行[附：行号n回车]； `0， $，gg， D， 213D`
- 删除一个单词， 删出一行的前后；删除所有：`dw, d0, d$(D), dd（删除当前行）， ndd（删除 n行） `
- 撤销，反撤销：`u,  crtl + r`

- 光标删除前后字母，撤销，删除单词：`X，x，u，?`



- 复制一行，多行，粘贴一行，多行：`yy, nyy, p（光标下一行）,P（光标上一行） [视图模式：y 粘贴,d删除,p复制下一行,P复制到上一行]`

- 替换一个，替换多个；`r, R`

- 查找：当前往下卷一圈 `/xxxx`； 当前往上卷一圈  `?xxxx`  关键字切换：`n/N`

- 光标移到单词，`#`

- vim分屏： :`sp`（线水平） `vsp`(线垂直)， `crtl+ww`（屏幕之间切换）， `wqall`(保存退出所有)

<br>

### **vim 配置文件：**

- 「1」用户级别(先)：`~/.vimrc`
- 「2」系统级别:`/etc/vim/vimrc`

<br>

## gcc:

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

### 静态库和动态库的制作和使用：

- **库是什么？**
  - 二进制文件
  - 加密之后的.c/.cpp的文件
- **库文件制作出来后，如何给用户使用？**
  - 需要头文件 + 制作出来的库
  - **动态库：** `.dll  或  .so`
  - **静态库：** `.lib  或   .a`


<br>

#### 静态库(.a/.lib)的制作和使用：

- **命令规则：** <font color=#FF0000 size=4 face="幼圆">lib</font>xxx<font color=#FF0000 size=4 face="幼圆">.a</font> 

- **静态库的制作步骤：**

  - 原材料：源代码.c .cpp

  - 将.cpp 文件生成.o 文件  

    ```bash
    #可以得到 `test01.o` 和 `test02.o`文件
    `gcc -C test01.c test02.cpp` 
    ```

  - 将.o 文件打包为.a 文件：

    ```bash
    #用法：ar rcs 静态库的名字 原材料  
    #ar -archive
    ar rcs libmytest.a test01.o test02.o   //所生成静态文件名称为libmytest.a
    
    
    #成功之后，可以查看libmytest.a里面的包含具体的所有的*.o文件
    nm libmytest.a
    ```

- **静态库的使用：**

  此刻文件夹的 tree 结构如下：

  > .
  >
  > ├─ Test.cpp
  >
  > ├─ include
  >
  > │   └─ xxxx.h
  >
  > └─ lib
  >
  > ​    └─ libmytest.a

  然后执行如下命令，就可以在得到本路径下得到可执行程序`appTest`：

  ```bash
  gcc Test.cpp -I ./include -L ./lib -l mytest -o appTest
  ```

  - Test.cpp：测试静态库的使用的头文件

  - -I：`./include/`为指定的库的头文件的路径

  - -L：`./lib/`为指定的库的二进制文件

  - -l：为静态库的库的名称，将要调用的库

    所生成**静态库的文件名称**为 `libmytest.a`， 按照规则，其**静态库的名称**叫`mytest`

  - -o：所生成的指定名称的为appTest的可执行程序的名称

<br>

#### 动态态库(.so/.dll)的制作和使用：

- **命令规则：** <font color=#FF0000 size=4 face="幼圆">lib</font>xxx<font color=#FF0000 size=4 face="幼圆">.so</font> 

- **动态库的制作步骤**

  - 原材料：源代码.c .cpp

  - 将.cpp 文件生成.o 文件  

    ```bash
    #用法：使用 gcc 加参数 -fpic(或 FPIC)制作.o文件，
    gcc test01.cpp test02.cpp -C -fpic
    ```

  - 将.o 文件打包为.so 文件：

    ```bash
    #用法：使用 gcc 加参数 -shared,将.o打包为.so文件，
    gcc -shared test01.o test02.o -o  libmytest.so
    ```

- **动态库的使用**

  - 此刻文件夹的 tree 结构如下：

    > .
    >
    > ├─ Test.cpp
    >
    > ├─ include
    >
    > │   └─ xxxx.h
    >
    > └─ lib
    >
    > ​    └─ libmytest.so

    然后执行如下命令，就可以在得到本路径下得到可执行程序`appTest`：

    ```bash
    gcc Test.cpp -I ./include/ -L ./lib/ -l mytest -o appTest
    ```

    **运行之后，若首次未做设置，则就会发现运次失败！！！，会报错误提示，说找不到libmytest.so **; 

    **原因是：** <font color=#70AD47 size=4 face="幼圆">**因为linux 的工作机制，其加载动态库.so的顺序。要知道所依赖库的.so 的名称，还要知道其绝对路径。** </font>

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200118_164520.png"/>

<br>

- **解决 Linux 加载动态库 .so 文件失败的方法：**

  思路：添加动态库的（绝对）路径。

  - <font color=#0000FF size=4 face="幼圆">**「方法1」拷贝自己制作的共享库：** </font>
    -  拷贝自己制作的共享库 .so 文件到 `/lib` 或 `/usr/lib`[不推荐，容易重名替换让系统崩溃]
  - <font color=#0000FF size=4 face="幼圆">**「方法2」使用环境变量：** </font>
    - 临时设置：`export LD_LIBRARY_PATH = 动态库的绝对路径:$LD_LIBRARY_PATH`(只添加新值)
    - 永久设置：
      - 「用户级别」将`export LD_LIBRARY_PATH = 动态库的加载路径`添加到`~/.bashrc`文件；然后重新加载，方法为重启终端或者 运行 `sources ./bashrc`; 即可生效.
      - 「系统级别」将绝对路径加入到`/etc/profile`文件，然后用命令`source /etc/profile `重新加载该文件.

  - <font color=#0000FF size=4 face="幼圆">**「方法3」使用文件列表：** </font>
    - 将动态库的绝对路径添加到`/etc/ld.so.conf`文件中；然后执行 `sudo ldconfig -v` 
  - <font color=#0000FF size=4 face="幼圆">**「方法4」使用函数调动态库：** </font>
    - 函数名：`dlopen()`  `dlclose()`  `dlsym()`

  <br>

  **补充一个小的知识点：**

  - `file appTest(可执行程序)`: 查看文件的类型，可以看到为 elf 类型
  - ldd appTest(可执行程序)` 查看 appTest 的所需要的加载动态库的文件，以及显示这些文件是否能找到` 
  - `echo $PATH` each 显示一个字符串，这里取 PATH 的值

