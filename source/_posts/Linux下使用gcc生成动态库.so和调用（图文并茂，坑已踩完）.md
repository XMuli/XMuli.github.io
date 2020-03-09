---
title: Linux下使用gcc生成动态库.so和调用（图文并茂，坑已踩完）
date: 2020-03-05 21:03:23
toc: true
categories: 
 - [学习 - Linux]
---



　　**简  述：**　 继 [上一篇](https://blog.csdn.net/qq_33154343/article/details/104692241)。本篇就继续实践踩坑，实现在Linux上面，使用gcc编译动态库.so，文件，且调用动态库。此处例子实际：使用g++8.3.0在uos20(Linux)平台下完成这个知识点的实践教程。

<!-- more -->

[TOC]

### 编程环境：

　　**💻：**  `UOS20 (即deepin20)` 📎 `gcc/g++ 8.3.0` 

　　**💻：**  `MacOS 10.14.6 (18G103)` 📎 `gcc/g++ 9.2.0` 

<br>

### 动态库概念：

**动态库** 在程序编译时并不会被连接到目标代码中，而是在**程序运行是才被载入，因此在程序运行时还需要动态库存在** 。动态库的后缀一般是 `.so`作为后缀。



**库：** 二进制文件；是加密之后的.c/.cpp的文件

**动态库：**  `.dll 或 .so`

<br>

### 前期铺垫：

- **重要提醒：**

<font color=#FF0000  size=5 face="幼圆">**本次运行环境是在uos20（Linux）系统上面跑成功的，**</font> <font color=#FF0000  size=4 face="幼圆">  在运行之前，添加动态库的（绝对）路径时候，需要执行 `export LD_LIBRARY_PATH=动态库的绝对路径:$LD_LIBRARY_PATH` </font>

​	

<font color=#FF0000  size=4 face="幼圆">若是在MacOS10.14上面跑成功（已经测试成功），在运行之前，添加动态库的（绝对）路径时候，需要执行 `export DYLD_LIBRARY_PATH=动态库的绝对路径:$DYLD_LIBRARY_PATH` </font>



<font color=#0000FF size=4 face="幼圆">设置环境变量的时候：Linux 的 `LD_LIBRARY_PATH` 对应的就是 Mac 的 `DYLD_LIBRARY_PATH` 。（动态链接器会查找）。</font>



下面每个文件里面写的代码如下：**注意此处，其中所有的.cpp文件都没有包含ExHeader.h或者其他的.h头文件**， 是为了验证一个猜想，.cpp生成的.o文件，在到.o生成的.so文件，是单独的二进制文件。只在最后编译和链接main.cpp文件时候，才会包含这个ExHeader.h文件。重最后结果来看，这个结果是✅的。 

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/81soxRT22.png" width="100%"/>

 创建的几个文件目录结构如图：

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_Snip20200307_181130.png" width="35%"/>

<br>

### 将.cpp文件生成.o文件：

- 运行命令 `g++-8 -c ExAdd.cpp ExDiv.cpp ExMul.cpp ExSub.cpp -fpic`，将除了main.cpp之外的所有.cpp文件，全部制作为二进制文件。

  ```bash
  #用法：使用 gcc 加参数 -fpic(或 FPIC)制作.o文件
  g++-8 -c ExAdd.cpp ExDiv.cpp ExMul.cpp ExSub.cpp -fpic
  ```

<br>

### 将.o文件大包为.so文件：

- 执行命令 `g++-8 -shared ExAdd.o ExDiv.o ExMul.o ExSub.o -o libxxx.so` 将上面生成的所有.o文件，打包为一个动态库.so文件

  ```bash
  #用法：使用 gcc 加参数 -shared,将.o打包为.so文件
  g++-8 -shared ExAdd.o ExDiv.o ExMul.o ExSub.o -o libxxx.so
  mv *.so ./lib       //将.so文件放入lib文件夹
  mv *.o ./lib        //将.o文件放入lib文件夹
  mv *.h ./include    //将.h文件放入include文件夹
  ```

 此时该文件的结构如下：

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/20200307181518.png" width="35%"/>

<br>

### 调用.so文件：

- 执行命令 `g++-8 main.cpp -I ./include -L ./lib -lxxx -o mainApp` ，使用gcc将main.cpp进行编译链接，其中需要的头文件在include文件夹里面找，需要的动态链接库文件在lib文件夹里面找，调用的动态库的库名叫xxx，最后输出一个名为mainApp的可执行程序

  - <font color=#D0087E size=4 face="幼圆">注意，所要生成的.a文件的名字前三位必须是lib，否则在链接的时候，就可能导致找不到这个库</font> 
  - **命令规则：** <font color=#FF0000  size=4 face="幼圆">lib</font>xxx<font color=#FF0000  size=4 face="幼圆">.a</font> ；<font color=#D0087E size=4 face="幼圆">**红色部分为固定的格式，中间的xxx才是库名**</font>
  - <font color=#D0087E size=4 face="幼圆">-l 为调用的库的名称(-l 后面没有空格，直接加库名)</font> 

  ```bash
  #-I 为指定的库的头文件的路径
  #-L 为指定的库的二进制文件文件
  #-l 为调用的库的名称(-l 后面没有空格，直接加库名)
  #-o 输出文件，生成的可执行程序的名称
  
  g++-8 main.cpp -I ./include -L ./lib -lxxx -o mainApp
  ```

 此时文件夹的结构如下：

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/20200307181554.png" width="35%"/>

<br>

### 设置.so的运行时的PATH：

- 执行命令 `export LD_LIBRARY_PATH=库的绝对路径:$LD_LIBRARY_PATH` 来设置临时的环境变量；告知系统这里有一个绝对路径的动态库路径，**动态库不能够使用相对路径。** 若是设置绝对路径的环境变量，则程序一定会跑失败。

  ```bash
  export LD_LIBRARY_PATH=/home/muli/Desktop/04_02_so/lib:$LD_LIBRARY_PATH
  ```



#### 解决 Linux 加载动态库 .so 文件失败的方法：

- 若是**运行之后，若首次未做设置，则就会发现运次失败！！！，会报错误提示，说找不到libxxx.so;** 

  **原因是：** <font color=#70AD47 size=4 face="幼圆">**因为linux 的工作机制，其加载动态库.so的顺序。要知道所依赖库的.so 的名称，还要知道其绝对路径。** </font>



 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/20200307181652.png" width="60%"/>



  <br>

  - **解决 Linux 加载动态库 .so 文件失败的方法：**

    思路：添加动态库的（绝对）路径。

    - <font color=#0000FF size=4 face="幼圆">**「方法1」拷贝自己制作的共享库：** </font>
      -  拷贝自己制作的共享库 .so 文件到 `/lib` 或 `/usr/lib`[不推荐，容易重名替换让系统崩溃]
    - <font color=#0000FF size=4 face="幼圆">**「方法2」使用环境变量：** </font>
      - 临时设置：`export LD_LIBRARY_PATH=动态库的绝对路径:$LD_LIBRARY_PATH`(只添加新值，*等号两边不能空格，路径也不需要双引号* )
      - 永久设置：
        - 「用户级别」将`export LD_LIBRARY_PAT = 动态库的加载路径`添加到`~/.zshrc`文件；然后重新加载，方法为重启终端或者 运行 `sources ./zshrc`; 即可生效.
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


<br>

### 运行可执行程序：

- 执行可执行程序 `./mainApp` ，可以看到程序跑起来的成功结果。

  ```bash
  ./mainApp
  ```

 此时运行效果为（uos20[Linux]下）：

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_Snip20200307_182031.png" width="80%"/>



 此时运行效果为（MacOS10.14 [Uinx]下）：

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/20200307183004.png" width="90%"/>

<br>

### 源码下载：

[04_02_so](https://github.com/touwoyimuli/linuxExample/tree/master/04_make_so_a/04_02_so) （Linux版：uos20 例子）

[04_03_so](https://github.com/touwoyimuli/linuxExample/tree/master/04_make_so_a/04_03_so) （Uinx版：MacOS10.14 例子）

<br>

### 总结：

看起来很简单，**殊不知，纸上学来终觉浅，** 还是亲自实现一番才以记忆深刻。若是成功的白嫖到了这篇文章，可以点个赞，给我留个言，鼓励一下。你的加油，我会持续不断的出新的文章，那啥，白嫖一时爽，一直白嫖一直     。🥺