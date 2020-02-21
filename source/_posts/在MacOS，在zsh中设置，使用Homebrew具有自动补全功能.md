---
title: 在MacOS，在zsh中设置，使用Homebrew具有自动补全功能
date: 2020-02-20 20:26:19
toc: true
categories: 
 - [学习 - MacOS]
tags: 
 - Homebrew
---



**简  述：**  使用 MacOS 10.14.6 在，在 zsh 里面使用 Homebrew 命令时候，对其设置补全功能🎃。

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**📌本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [在MacOS，在zsh中设置，使用Homebrew具有自动补全功能](https://blog.csdn.net/qq_33154343/article/details/104424732)

<br>

#### 背景说明：

　　在 MacOS 10.14 中，在 zsh 中使用 Homebrew 命令，是没有补全功能的。让人不优雅🎁。这里设置 Homebrew 的补全功能。

<br>

#### 设置步骤：

1. 安装 HomeBrew 的补全文件，默认是安装到 `/usr/local/Cellar/` 路径。执行命令如下：

   ```bash
   brew install zsh-completions
   ```

2. 将 `zsh-completions` 的路径导入，否则找不到该文件仍无效，执行命令如下：

   ```bash
   fpath=(/usr/local/Cellar/zsh-completions/0.31.0/share/zsh-completions $fpath)
   compinit
   ```

   > 注：zsh-completions/你实际版本号/

3. 验证补全生效

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200220_202007.jpeg"/>

<br>

#### 参考博文：

　　[在 Mac OS X 系统下为 Brew 开启 Zsh 补全功能](https://tommy.net.cn/2015/02/24/enable-zsh-completion-of-brew-under-mac-os-x/)

　　[Homebrew总结](https://zhuanlan.zhihu.com/p/22598799) 