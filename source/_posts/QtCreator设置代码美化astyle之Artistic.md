---
title: QtCreator设置代码美化astyle之Artistic
date: 2019-9-26 00:04:44
toc: true
categories: 
 - [学习 - qt]
 - [学习 - 编码规范，辅助技巧]
tags: 
 - astyle
 - QCreator小技巧
---



**简介：**  在**Qt Creator**里面使用代码美化工具**astyle**：按照想要的c++风格来格式化`code`。

<!-- more -->

[TOC]

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [QtCreator设置代码美化astyle之Artistic](https://blog.csdn.net/qq_33154343/article/details/101397429) 

<br>

## 知识讲解：

常用的**C/C++**代码格式优化工具有两个，一是老牌的`indent`，再一个就是`astyle`了。

**astyle 官网下载：** [https://sourceforge.net/projects/astyle](https://sourceforge.net/projects/astyle/)

其他风格： [Google 开源项目 c/c++风格](https://zh-google-styleguide.readthedocs.io/en/latest/)

<br>

## QtCreator设置：

具体设置如图：**“工具--选项--美化--Artistic Style”**，

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190924222146.png"/>

<br>

## 样式参考：

博主喜欢的样式风格：

```bash
--style=allman
indent=spaces=4	      # 缩进采用4个空格
indent-switches           # -S  设置 switch 整体缩进
indent-cases 	      # -K  设置 cases 整体缩进
indent-namespaces         # -N  设置 namespace 整体缩进
indent-preproc-block      # -xW 设置预处理模块缩进
indent-preproc-define     # -w  设置宏定义模块缩进	
pad-oper                  # -p  操作符前后填充空格
#delete-empty-lines       # -xe 删除多余空行
#add-braces               # -j  单行语句加上大括号
#align-pointer=name       # *、&这类字符靠近变量名字
align-pointer=type        # *、&这类字符靠近类型
```

deepin（Linux）开源的一种的风格：

```bash
indent=spaces=4
style=kr
indent-labels
pad-oper
unpad-paren
pad-header
keep-one-line-statements
convert-tabs
indent-preprocessor
align-pointer=name
align-reference=name
keep-one-line-blocks
keep-one-line-statements
attach-namespaces
max-instatement-indent=120
```

好像现阶段，跟对下面的这一种更加感冒，已经使用了好几个月了，该风格，也算比较推荐；



或者想自己自定义，可以参考`google`的风格，如链接 [https://zh-google-styleguide.readthedocs.io/en/latest/](https://zh-google-styleguide.readthedocs.io/en/latest/)

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190924222700.png"/>

<br>

## 风格样式：

**Style-格式配置：**
最常用的就是ansi或或kr格式，实际上，kr，stroustrup和linux这三种格式是非常接近的了，试了好几个文件，只有非常微小的区别，可以忽略不计。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190924221500.png"/>

<br>

## 参数含义：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190924221621.png"/>

<br>

## 使用方法：

使用图下图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190924223021.png"/>



参考文章：

[C/C++代码格式优化工具----astyle](http://www.cppblog.com/jerryma/archive/2012/02/02/164813.html) 

[`Qt`资料大全和`Google`编程规范（中文版，含`c++`、`java`等）](https://blog.csdn.net/qq_33154343/article/details/98512180) 

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>

