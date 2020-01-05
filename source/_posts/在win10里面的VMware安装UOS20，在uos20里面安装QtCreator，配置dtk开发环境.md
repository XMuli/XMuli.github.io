---
title: 在win10里面的VMware安装UOS20，在uos20里面安装QtCreator，配置dtk开发环境
date: 2019-12-27 13:13:15
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  在**UOS20** （Linux系统）安装  **Qt Creator 5.11.3， 64bit**的开发教程，并且配置好 **dtk**的开发环境；其中本篇文章主要内容，介绍如下

- 在`win10`的机器里面使用`VMware15.5`安装`UOS20（Linux）`操作系统
- 在`uos20`里面安装`QtCreator`
- 在`QtCretor`里面配置`dtk`开发环境和工程模板

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [在win10里面的VMware安装UOS20，在uos20里面安装QtCreator，配置dtk开发环境](https://blog.csdn.net/qq_33154343/article/details/103733327)

<br>

## 背景缘由：

某笔记本一台，已经安装win10，且已经在VMware虚拟机里面，安装好了UOS20操作系统（一款颜值在线的Linux系统）；然后为了实际工作中为uos20添砖加瓦，需要安装QtCretor此IDE作为开发环境，且配置好那些的dtk的开发环境（写应用开发的开发人员），因他们是有所需要的；而我，作为是给他们写dtk库的人，表示是不需要设置的此环境的。但写应用的开发者们，有时候会使用dtk控件的时候，会有一些诡异的bug，我就需要测试一下，故此搭建 **Dtk Widgets Application**模版环境。



**ps: uos20 的前一个大版本是deepin15， 都是同一家操作系统公司研发，是一个国际排名稳定在10左右的Linux的版本，衍生于Debian**



**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt 5.9.8`，`Qt Creator 4.8.2 (Enterprise)`

**&&**

**编程环境：**  `uos 20 x64 `    **Kernel：**  `x86_64 Linux 4.19.0-5-amd64`

**编程软件：**  `Qt 5.11.3`，`Qt Creator 4.8.2 `

<br>

## win10里面的VMware安装UOS20系统：

- 选择新创建一个虚拟机，选择 **自定义（高级）**点击 **下一步**：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227141804.png)

- 直接 **下一步**，不用修改什么

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227141952.png)

- 选择 **稍后安装操作系统**， 点击 **下一步**：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227142130.png)

- 选择 **Ubuntun x64（这点很重要）**, 点击 **下一步**，

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227142326.png)

- 选择安装目录， 点击下一步：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227142713.png)

- 建议还是多分配几颗，这是我的个人配置颗数，越多越好

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227142809.png)

- 选择分配内存数，然后点击下一步（后面也可以修改内存数大小）

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227143137.png)

- 选择网络模式
  - **桥接网络**：虚拟机里面的系统会有一个独立IP，不和物理主机的系统公用一个IP
  - **NAT网络**：虚拟机里面的系统不会有一个独立IP，和物理主机的系统公用一个IP

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227143407.png)

- 接下来几个步骤不用修改什么，直接选择下一步：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227143623.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227143726.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227143749.png)

- 选择磁盘大小，然后点击下一步：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227143852.png)

- 直接下一步：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227143938.png)

- 选择镜像源之后，点击开启，便会自动安装好虚拟机

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227144542.png)

- 常规的选择用户名，设置密码等，然后等待之后，就会安装成功

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227144915.png)



经过前面一些列操作，uos20已经成功运行成功；接下来就是安装QtCretor了；不过在此之前，先建议更换源：

【方式一】可以在 **“控制中心-更新-更新设置“**，GUI界面手动选择换源；

【方式二】也可以去官网找稳定的更新的源，添加到`/etc/apt/sources.list`这个文件里面）；



然后执行下面更新的命令，更新一下系统：

```bash
sudo apt update
sudo apt dist-upgrade
```

<br>

## 在`uos20`里面安装`QtCreator`：

直接执行以下语句:

```bash
sudo apt install qtcreator     //安装Craetor IDE
sudo apt install qt5-default   //安装qt的配置
sudo apt install libdtkwidget-dev  //安装dtk开发所需要环境

sudo apt source qt5-default    //qt源码包，此会安装在当前所处目录下，建议更换目录再执行，推荐放在

//==============================================================
//也可以将上面的合成一个命令
sudo apt install libdtkwidget-dev qt5-default qtcreator
```





其中安qt源码学习的时候，会出现如下提示，蛋蛋蛋蛋蛋是，并不影响😎😎😎😎，进到当前文件夹下查看，所需要的文件已经被下载下来了：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227150930.png)

我们所需啊都在这里

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227151528.png)

- 创建一个空的工程项目，验证安装成功

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227151730.png" style="zoom:80%;" />

<br>

## 在`QtCretor`里面配置`dtk`开发环境和工程模板：

在没有设置之前打开新建工程项目，是这样的，没有 **”Dtk Widgets Application“**这个选项的：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227152000.png)



### 安装 **Dtk Widgets Application**工程模板：

#### 【方式一】：命令安装

运行命令，然后重启QtCretor：

```bash
sudo apt install qtcreator-template-dtk
```

#### 【方式二】：手动安装

下载此文件：![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227153249.png)

链接：https://pan.baidu.com/s/1GD7ky9iFjYQ5NZBIU9kjOw 
提取码：8q0b

> 下载之后，将此压缩包解压到 /usr/share/qtcreator/templates/wizards/projects 目录下，之后重启 QtCreator，在创建项目的向导中即可选择“Dtk Widgets Application”

重启之后，会发现有如下，且运行一个新的工程运行验证安装：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227152501.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191227153749.png)

<br>

