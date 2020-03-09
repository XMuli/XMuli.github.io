---
title: doxygen介绍和安装和在Linux下使用
date: 2019-10-29 21:27:43
toc: true
categories: 
 - [学习 - doxygen]
---



　　**简  述：**  `Doxygen`是一个程序的文档产生工具，可以将程序中的注释转换成说明文档或者说是API参考手册，从而减少程序员整理文档的时间。当然这里程序中的注释需要遵循一定的规则书写，才能让`Doxygen`识别和转化。

目前`Doxygen`可处理的程序语言包含`C/C++`、`Java`、`Objective-C`、`IDL`等，可产生出来的文档格式有`HTML`、`XML`、`LaTeX`、`RTF`等，此外还可衍生出不少其它格式，如`HTML`可以打包成`CHM`格式，而`LaTeX`可以通过一些工具产生出`PS`或是`PDF`文档等。

即: 通过代码里面的注释,生成`html` 或 `chm`这一类的手册说明,极其类似于qt帮助文档

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [doxygen介绍和安装和在Linux下使用](https://blog.csdn.net/qq_33154343/article/details/102809157)

<br>

## 下载doxygen:
官网: [http://www.doxygen.nl/download.html](http://www.doxygen.nl/download.html)

支持**Windows**/**macOS**/**Linux**平台(简直就是好人)

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191029213518.png"/>

<br>

## Linux下使用dexygen生成文档:

这里在`deepin v20( Linux) `演示:

* 安装命令
```bash
sudo apt install doxygen
```
截止到我安装的时候, 最新的版本是**1.8.13** (2019-10-29官网Linux版下载)
* 进入到工程项目文件夹下`~/projects/dtkwidget`, 使用如下命令生成`Doxyfile`文件(这是一个配置文件)
```bash
doxygen -g      //生成带详细注释的Doxyfile文件 约2120行
//或
doxygen -s -g   //生成不带详细注释的Doxyfile文件  约374行
```
* 编辑使用`vim`编辑如下的`Doxyfile`文件;  其中这几点必须修改, 不然可能和预期的效果不一样
>//必须要填写:
>OUTPUT_DIRECTORY       =/home/yuanyi/projects/out     //输出存放文档的路径
>RECURSIVE                           = YES                          //文件递归,包括子文件也要输出为文档
>IMAGE_PATH                        = ./doc/images      //若是想要文档里面插入图片,需要存放图片路径
> 
>//推荐修改
>PROJECT_NAME                 = "My Project"               //生成文档的名称
>OUTPUT_LANGUAGE        = Chinese                          //生成文档为中/英文  English
>PROJECT_NUMBER           =1.0.0                                //项目文档的版本号码
* 生成的`Doxyfile`文件是必须放在工程项目的根目录下`~/[projects/dtkwidget](file:///home/yuanyi/projects/dtkwidget)`,默认也是生成在这里的
```bash
├── dtkwidget
    ├── ...很多文件及文件夹
    ├── doc
    ├── Doxyfile
    ├── LICENSE
    └── README.md
```
* 在项目里面,  添加符合doxygen规则的代码注释,  便于生成文档
* 运行命令生成文档的
```bash
doxygen     //默认是等价于下面的doxygen Doxyfile,也可执行自定义Doxyfile文件
//或
doxygen Doxyfile
```
* 到输出目录查看生成的`html`文件夹,使用浏览器打开`index.html`文件,  其像动态网站一样,  帮助你早日熟悉项目工程的结构

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191029213442.png"/>

<br>

## 生成其他格式如chm和注意:
这里是可以很多其他格式的, 一些常用的设置,  参考下图:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191029213604.png"/>

```bash
# 如果是制作 C 程序文档，该选项必须设为 YES，否则默认生成 C++ 文档格式
OPTIMIZE_OUTPUT_FOR_C  = YES
```


