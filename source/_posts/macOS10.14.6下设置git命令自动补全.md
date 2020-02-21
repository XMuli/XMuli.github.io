---
title: macOS10.14.6下设置git命令自动补全
date: 2019-10-30 18:56:15
toc: true
categories: 
 - [学习 - MacOS]
---



**简  述：**  这周不久前， 更换了`MacBook Por`的本子，才上手不到一周， 越加喜爱此系统，且很多的喜欢也是要从`windows 10`上面迁移过来，包裹很多的习惯都是；在公司里面，因为是使用`Linux`系统，也经常使用`git`,在`macOS 10 .14.6`上面，发现git是自带的，都不用自己下载安装的，但是实际过程中却发现， 是没有代码补全功能的比如`git clo + Tab按键`不会自动变成`git clone`；本篇就是解决这个问题，

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [macOS10.14.6下设置git命令自动补全](https://blog.csdn.net/qq_33154343/article/details/102887559)

<br>

## 安装bash-completion:

打开终端，运行命令：

```bash
brew install bash-completion
```

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img2/Snipaste_2019-10-30_19-18-21_mark.png"/>

<br>

## 编辑~/.bash_profile:

运行命令：

```bash
vim ~/.bash_profile 
```

在文件结尾处添加如下代码：

```bash
if [ -f $(brew --prefix)/etc/bash_completion ]; then
  . $(brew --prefix)/etc/bash_completion
fi
```

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img2/Snipaste_2019-10-30_19-19-01_mark.png"/>

<br>

## 安装git-completion.bash:

运行命令：

```bash
cd $(brew --prefix)/etc/bash_completion.d
curl -L -O https://raw.github.com/git/git/master/contrib/completion/git-completion.bash
chmod a+x git-completion.bash
```

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img2/Snipaste_2019-10-30_19-22-03_mark.png"/>

<br>

## 重启终端：

重启终端之后， 使用 `git` + `Tab按键`, 即可有补全命令功能

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img2/Snipaste_2019-10-30_19-25-47_mark.png"/>