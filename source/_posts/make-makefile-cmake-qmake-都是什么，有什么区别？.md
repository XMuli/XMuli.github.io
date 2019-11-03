---
title: make、makefile、cmake、qmake都是什么？有什么区别？
date: 2019-8-1 22:26:44
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - 底层原理、思想架构]
tags: 
 - 原理
---

**简介：**   `make` `makefile` `cmake`   `qmake`都是什么，有什么区别？

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​	  `make` `makefile` `cmake`   `qmake`都是什么，有什么区别？

​         查了一下好像是编译用的，既然是编译为什么我们不用g++ javac 来编译呢？我猜答案是方便一点，但是具体方便在哪呢，请明示。还有它们之间如果有相似性的话，也帮我比较一下吧，谢谢各位大神。



**觉得写的比较好，所以在这里搬运过来了** 

## <font color=#D0087E  face="幼圆">重要提示：</font>

- 若遇`csdn`的博文排版、文字、图片、链接、视频预览等异常，会删除该部分，或用链接代替，或删除该部分，但在文末 [github.io](https://touwoyimuli.github.io/) 中的同步文章，会保证显示正确
- <font color=#D0087E  size=4 face="幼圆">**推荐<font color=#FE7207  size=4 face="幼圆">本文末的同步链接</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好的100%效果体验**</font> 

<br>

## 答一（比较写的好）：

作者：辉常哥

链接：https://www.zhihu.com/question/27455963/answer/89770919

来源：知乎

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

1.`gcc`是`GNU Compiler Collection（就是GNU编译器套件）`，也可以简单认为是编译器，它可以编译很多种编程语言（括`C`、`C++`、`Objective-C`、`Fortran`、`Java`等等）。



2.当你的程序只有一个源文件时，直接就可以用gcc命令编译它。



3.但是当你的程序包含很多个源文件时，用`gcc`命令逐个去编译时，你就很容易混乱而且工作量大



4.所以出现了`make`工具， `make`工具可以看成是一个智能的批处理工具，它本身并没有编译和链接的功能，而是用类似于批处理的方式—通过调用`makefile`文件中用户指定的命令来进行编译和链接的。



5.`makefile`是什么？简单的说就像一首歌的乐谱，make工具就像指挥家，指挥家根据乐谱指挥整个乐团怎么样演奏，`make`工具就根据`makefile`中的命令进行编译和链接的。



6.`makefile`命令中就包含了调用gcc（也可以是别的编译器）去编译某个源文件的命令。



7.`makefile`在一些简单的工程完全可以人工手下，但是当工程非常大的时候，手写`makefile`也是非常麻烦的，如果换了个平台`makefile`又要重新修改。



8.这时候就出现了`Cmake`这个工具，cmake就可以更加简单的生成`makefile`文件给上面那个`make`用。当然`cmake`还有其他功能，就是可以跨平台生成对应平台能用的`makefile`，你不用再自己去修改了。



9.可是cmake根据什么生成`makefile`呢？它又要根据一个叫`CMakeLists.txt`文件（学名：组态档）去生成`makefile`。



10.到最后`CMakeLists.txt`文件谁写啊？亲，是你自己手写的。



11.当然如果你用IDE，类似VS这些一般它都能帮你弄好了，**你只需要按一下那个三角形**。<font color=#FE7207  size=4 face="幼圆">（以前从来没研究过这个邪恶的三角形，所以这那天翻来这两篇文章）</font>



12.接着是`qmake`，`qmake`是什么，先说一下Qt这个东西。Qt是跨平台C++图形用户界面应用程序开发框架。它既可以开发GUI程序，也可用于开发非`GUI`程序，比如控制台工具和服务器。简单的说就是C++的第三方库，使用这个库你可以很容易生成`windows`，`Linux`，`MAC os`等等平台的图形界面。现在的`Qt`还包含了开发各种软件一般需要用到的功能模块（网络，数据库，`XML`，多线程啊等等），比你直接用`C++`（只带标准内裤那种）要方便和简单。



13.你可以用`Qt`简简单单就实现非常复杂的功能，是因为`Qt`对`C++`进行了扩展，你写一行代码，Qt在背后帮你写了几百上千行，而这些多出来的代码就是靠`Qt`专有的`moc`编译器（`The Meta-Object Compiler`）和`uic`编译器（`User Interface Complier`）来重新翻译你那一行代码。问题来了，你在进行程序编译前就必须先调用`moc`和`uic`对`Qt`源文件进行预处理，然后再调用编译器进行编译。上面说的那种普通`makefile`文件是不适用的，它没办法对`qt`源文件进行预处理。所以`qmake`就产生了。



14.`qmake`工具就是Qt公司制造出来，用来生成Qt 专用`makefile`文件，这种`makefile`文件就能自动智能调用`moc`和`uic`对源程序进行预处理和编译。`qmake`当然必须也是跨平台的，跟`cmake`一样能对应各种平台生成对应`makefile`文件。



15.`qmake`是根据`Qt` 工程文件`（.pro）`来生成对应的`makefile`的。工程文件（`.pro`）相对来说比较简单，一般工程你都可以自己手写，但是一般都是由`Qt`的开发环境 `Qt Creato`r自动生成的，你**还是只需要按下那个邪恶三角形就完事了**。



16.还没有完，由于qmake很简单很好用又支持跨平台，而且是可以独立于它的IDE，所以你也可以用在非Qt工程上面，照样可以生成普通的`makefile`，只要在pro文件中加入`CONFIG -= qt`  就可以了。



17. 这样`qmake`和`cmake`有什么区别？

      不好意思，`cmake`也是同样支持`Qt`程序的，`cmake`也能生成针对`qt` 程序的那种特殊`makefile`，

     只是`cmake`的`CMakeLists.txt` 写起来相对与`qmake`的`pro`文件复杂点。

     `qmake` 是为 `Qt` 量身打造的，使用起来非常方便，但是cmake功能比qmake强大。

     一般的`Qt`工程你就直接使用`qmake`就可以了，`cmake`的强大功能一般人是用不到的。

     当你的工程非常大的时候，又有`qt`部分的子工程，又有其他语言的部分子工程，据说用`cmake`会 方便，我也没试过。





## 答二（图很好）:

作者：玟清

链接：https://www.zhihu.com/question/27455963/answer/36722992

来源：知乎

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

1. `make` 是用来执行`makefile`的

     

2. `makefile`是类unix环境下(比如Linux)的类似于批处理的"脚本"文件。其基本语法是: 目标+依赖+命令，只有在目标文件不存在，或目标比依赖的文件更旧，命令才会被执行。由此可见，`makefile`和make可适用于任意工作，不限于编程。比如，可以用来管理latex。

     

3. `makefile`+`make`可理解为类`unix`环境下的项目管理工具，但它太基础了，抽象程度不高，而且在`windows`下不太友好(针对`visual studio`用户)，于是就有了跨平台项目管理工具`cmake`

     

4. cmake是跨平台项目管理工具，它用更抽象的语法来组织项目。虽然，仍然是目标，依赖之类的东西，但更为抽象和友好，比如你可用math表示数学库，而不需要再具体指定到底是math.dll还是libmath.so，在windows下它会支持生成`visual studio`的工程，在linux下它会生成`makefile`，甚至它还能生成eclipse工程文件。也就是说，从同一个抽象规则出发，它为各个编译器定制工程文件。

     

5. cmake是抽象层次更高的项目管理工具，cmake命令执行的CMakeLists.txt文件

     

6. qmake是Qt专用的项目管理工具，对应的工程文件是*.pro，在Linux下面它也会生成`makefile`，当然，在命令行下才会需要手动执行qmake，完全可以在qtcreator这个专用的IDE下面打开*.pro文件，使用qmake命令的繁琐细节不用你管了。

     

总结一下，make用来执行`makefile`，`cmake`用来执行`CMakeLists.txt`，`qmake`用来处理`*.pro`工程文件。`makefile`的抽象层次最低，`cmake`和`qmake`在`Linux`等环境下最后还是会生成一个`makefile`。`cmake`和`qmake`支持跨平台，`cmake`的做法是生成指定编译器的工程文件，而qmake完全自成体系。

具体使用时，`Linux`下，小工程可手动写`makefile`，大工程用automake来帮你生成`makefile`，要想跨平台，就用`cmake`。如果`GUI`用了`Qt`，也可以用`qmake` + `*.pro`来管理工程，这也是跨平台的。当然，`cmake`中也有针对`Qt`的一些规则，并代替`qmake`帮你将`qt`相关的命令整理好了。

另外，需要指出的是，`make`和`cmake`主要命令只有一条，`make`用于处理`makefile`，cmake用来转译`CMakeLists.txt`，而`qmake`是一个体系，用于支撑一个编程环境，它还包含除`qmake`之外的其它多条命令(比如`uic`，`rcc`,`moc`)。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190801221230.png"/>

上个简图，其中`cl`表示`visual studio`的编译器，`gcc`表示`linux`下的编译器

<br>

## 参考博文：

因为有着热心网友的无私分享，

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

<br>

## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到csdn博客：**</font> [`make` `makefile` `cmake` `qmake`都是什么，有什么区别？](https://blog.csdn.net/qq_33154343/article/details/98170236) 