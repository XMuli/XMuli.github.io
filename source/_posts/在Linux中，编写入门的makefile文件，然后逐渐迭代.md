---
title: 在Linux中，编写入门的makefile文件，然后逐渐迭代
date: 2020-03-09 16:47:28
toc: true
categories: 
 - [学习 - Linux]
 - [学习 - MacOS]
---



　　**简  述：**　在 `Uinx / Linux` 下，写下这篇适合小白学习的入门教程，理解 `make`，`makefile` 文件。和开始编写自己的 `makefile` 文件，使用 `make` 命令执行，生成我们所需要的项目。

<!-- more -->

[TOC]

### 编程环境：

　　**💻：**  `MacOS 10.14.6 (18G103)` 📎 `gcc/g++ 9.2.0` 

<br>

### make 介绍：

- make 是 Linux 自带的构建器。
- 构建规则是依据 makefile 文件内容



代码变成可执行文件，叫做**编译** ；先编译这个，还是先编译那个（即编译的安排），叫做**构建** 。

更多参考：[make、makefile、cmake、qmake都是什么，有什么区别？](https://blog.csdn.net/qq_33154343/article/details/98170236) 

<br>

### makefile 介绍：

#### 命名：

- 该文件的名称是唯一：`makefile` 或 `Makefile`

<br>

#### 规则：

- 目标，依赖，命令

- 一个完整的 makefile 文件，由多个规则组成

  - 最上面的一行规则，是**终极目标** ，下面的都是**过程目标** 

  

  <font color=#D0087E size=4 face="幼圆">目标: 依赖</font>

  <font color=#D0087E size=4 face="幼圆">	命令          //此处有 Tab 缩进，不能用多个空格代替</font>

  

  ```makefile
  #一个规则 eg :
  mainApp: *.cpp
  	g++ *.cpp -o mainApp  
  ```

<br>

#### 执行原理：

- 检测依赖是否存在？
  - 向下搜索下边的规则，下面的规则通常是用来生成依赖的
- 判断是否重新编译生成？
  - 最终目标文件的创建时间一定是要晚于中间的生成的目标(作为依赖文件)
- 若是本某条规则没有写依赖
  - 那么它永远是最新的目标文件（本条为，分析第五版的 make clean）

<br>

### 编写自己的 makefile 文件：

#### 准备铺垫：

依旧沿用[上一篇](https://blog.csdn.net/qq_33154343/article/details/104692370)文章的工程代码，且有讲解 gcc/g++ 的用法，项目代码内容预览如下：

 <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/81soxRT22.png" width="90%"/>



这里按照想法，以前是执行 `g++-9 *.cpp -o mainApp` 这一行命令，现在使用 makefile 规则，使用 make 命令执行，就相当于使用另外一个语言（规则），来重写这句话。于是乎，我们使用 vim 创建 makefile 文件，在里面写下如下语句；然后执行 make，即可得到 mainApp 可执行程序（我们的目标），就属于大功告成。

<br>

#### 第1版本：

讲道理，将 gcc 命令写成符合 makefile 规则（参考上面）的语句就好了，最简单的想法如下，其中 mainApp 的名称是任意的，是最后生成的终极目标的可执行程序的名称。写好后保存，终端执行执行 make ，就得到了我们所需要的。

```makefile
mainApp: *.cpp
	g++-9 *.cpp -o mainApp
```

<br>

#### 第2版本：

- **改进之处：** 
  - 避免改动一个 .cpp，但是所有 .cpp 文件都要重新编译为 .O 文件

```makefile
mainAPP: *.o
	g++-9 *.o -o mainApp

*.o: *.cpp
	g++-9 -c *.cpp
```

<br>

#### 第3版本：

- **改进之处：**
  - 使用 makefile 里面的变量
  - <font color=#FF0000  size=4 face="宋体">$( )</font>  表示取变量里面的值
  - <font color=#FF0000  size=4 face="宋体">%.o: %.cpp</font>  模式匹配，相当于公式，两个百分号是同一个 "量"

```makefile
val_a = ExAdd.o ExDiv.o ExMul.o ExSub.o main.o  #赋值给变量 val_a
val_app = mainAPP

$(val_app): $(val_a)
	g++-9 $(val_a) -o $(val_app)    #可替换为 g++-9 $^ -o $(val_app)

%.o: %.cpp
	g++-9 -c $< -o $@
```

<br>

#### 第4版本：

- **改进之处：**
  - **使用自动变量：**
    - <font color=#D0087E size=4 face="幼圆">$@</font> 规则中的目标
    - <font color=#D0087E size=4 face="幼圆">$ <</font> 规则中的第一个依赖
    - <font color=#D0087E size=4 face="幼圆">$^</font> 规则中的所有依赖
    - <font color=#FF0000  size=4 face="幼圆">它们只能够认识本条规则的命令，在本条规则中使用</font> 
  - **使用函数：**
    - <font color=#D0087E size=4 face="幼圆">wildcard</font>  为查找指定文件夹，里面的指定类型的文件，后面为参数
    - <font color=#D0087E size=4 face="幼圆">patsubst</font>  为字符替换函数

```makefile
val_a = $(wildcard ./*.cpp)
val_b = $(patsubst %.cpp, %.o, $(val_a))
val_app = mainAPP

$(val_app): $(val_b)
	g++-9 $^ -o $(val_app)


%.o: %.cpp
	g++-9 -c $< -o $@
```

<br>

#### 第5版本：

- **改进之处：**
  - 添加工程项目清理中间功能，避免手动清理，只需要运行 `make clean` 这个中间目标即可
- 若是工程文件夹下，有和 makefile 的中间目标同名的文件。那么依据**检测依赖是否存在的工作原理** ，其会不执行，且冲突。（它认为编译的依赖文件   clean ，最新且存在的）
  - 解决：声明为伪目标 .PHONY: 



(若是最存在同名文件) ，其目标文件 claen 又没有依赖文件，所以总是最新的，也就不会执行 make clean 的下面命令

```makefile
#.PHONY: clean   #明伪目标: 不会做依赖和更新检查( 文件日期时间先后判断)
clean: 
#	-mkdir /abc  # 第一个"-"   表示若是执行失败，则执行后面会的命令，
	-rm $(val_b) $(val_app)   #后面-f 是强制执行 
```

<br>

### 运行效果：

- 写好 makefile 文件

- 执行 make 命令

- 不再需要演示，执行清理命令[运行中间规则]：make clean

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200309_183011.png" width="80%"/>

<br>

### 代码下载：

[05_makefile](https://github.com/touwoyimuli/linuxExample/tree/master/05_makefile) 

<br>

**其它：**

假装有其 ta ，，， （点个赞）？