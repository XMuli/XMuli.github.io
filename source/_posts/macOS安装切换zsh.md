---
title: macOS安装切换到zsh
date: 2019-11-04 20:01:11
toc: true
categories: 
 - [学习 - git]
 - [学习 - MacOS]
tags: 
 - zsh
---



**简  述：**  之前看旁边的~~同事~~大佬，在`deepin`里面的终端，和我用的终端有些不一样，那会大概是以为是使用了什么插件之类的吧。和浏览器插件相似，觉得安装之类的，可能会比较麻烦，于是乎没有压下去好奇心；但时间久了，总会心里痒痒难耐，一询问，告知是`zsh`，google一波，然后就在Linux上面也给布置了一套，还选用一套自己喜欢的主题， 美滋滋；回家再自己的mac上面，今天也想着也给换上`zsh`；习惯统一。

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [macOS安装切换到zsh](https://blog.csdn.net/qq_33154343/article/details/102905596)

<br>

## zsh介绍：

**全名称：**`oh my zsh`

**官网：**[https://github.com/robbyrussell/oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) 

**个人评价：**相当于平行于`bash`的东西，或者是给加了一个漂亮的主题外壳，使用起来会看起来很舒服，且会有这很智能提示，当按下`Tab`按键后；主题很多，也可以喜欢自己搭配

**其他：**或许有那种使用高级东西，尝试新鲜没有试过的工具，遇到一个喜欢的会很开心，在新人面前可以~~秀一把~~（显得专业）；**最主要的自己使用的舒服，就会一直用下去，喜欢就好**

<br>

## 检查是否已安装zsh：

- 检查电脑是否有安装`zsh`

```bash
zsh --version
```

可以看到我的`MacOS 10.14.6 (18G103)`默认安装有的，其版本为`zsh 5.3 (x86_64-apple-darwin18.0)`;

- 查看配置文件，在`~/.zshrc`是否存在；我使用`ls -a`查看，是没有的；于是`git clone`后，想着复制文件到`~/`下面， 但是经过下载之后， 发现里面并没有这个文件； 于是乎我决定自己重新安装（下图为重新安装`zsh`之后的截图）

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191104204548.png"/>

<br>

## 官网安装zsh:

运行如下命令：随后需要按一次`回车`， 和一次`Y + 回车`；

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

安装成功， 查看`~/.zshrc`配置文件;

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191104204427.png"/>

<br>

## 更换zsh主题：

官方自带的100多个主题，可以直接预览 [https://github.com/robbyrussell/oh-my-zsh/wiki/Themes](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes) ，其中博主比较喜欢的一款主题是`cloud`；

- 使用`vim`编辑主题配置文件

```bash
vim ~/.zshrc
```

- 将`ZSH_THEME=`设置为你喜欢的主题名称后， 保存退出；**重启终端，即可见到主题切换成功**

    > ZSH_THEME="robbyrussell"  修改为 ZSH_THEME="cloud “

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-11-04_20-57-22_mark.png"/>

<br>

## 效果图：

没有切换之前的`bash`效果，感觉前缀太长：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-11-04_21-03-47_mark.png"/>

切换后，当进入到含有`.git`的文件夹中，若是文件有修改或增加部分，但是没有`git stash`或者`git add .`里面的时候，是会有一个闪电提示；且时刻会一个小的`[branch]`当前分支的提醒，很是简介，但我想要的信息都给显示出来了，

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-11-04_20-51-37_mark.png"/>

<br>

## 查看zsh安装位置:

```bash
which zsh
```

会发现安装是在`/bin/zsh`里面的；所以下面的理解

<br>

## 从bash切换到zsh：

```bash
chsh -s /bin/zsh
```

<br>

## 从zsh切换到bash：

```bash
chsh -s /bin/bash
```

