---
title: 关联macOS10.14.6的git本地环境和自己的github仓库
date: 2019-10-25 21:21:42
toc: true
categories: 
 - [学习 - git]
tags: 
 - vim
---



**简  述：**  关联`macOS10.14.16`的`git`本地环境和自己的`github`仓库,出现`Key is invalid. You must supply a key in OpenSSH public key format`的解决方法。是因为vim复制出来的格式有问题导致

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [关联macOS10.14.16的git本地环境和自己的github仓库](https://blog.csdn.net/qq_33154343/article/details/102750380)

<br>

## macOS配置git，在github生成ssh:

**编程环境：** `macOS 10.14.6`, 在此出记录一个坑；

步骤按照之前写的这篇教程来[关联windows10的git本地环境和自己的github仓库]([https://touwoyimuli.github.io/2019/08/25/%E5%85%B3%E8%81%94windows%E7%9A%84%E6%9C%AC%E5%9C%B0%E7%8E%AF%E5%A2%83%E5%92%8C%E8%87%AA%E5%B7%B1%E7%9A%84github%E4%BB%93%E5%BA%93/](https://touwoyimuli.github.io/2019/08/25/关联windows的本地环境和自己的github仓库/)) 来进行配置，其中在本地生成秘钥的时候，复制到github的时候，采用的`vim`打开文件 `vim ~/.ssh/id_rsa.pub`, 来进行复制到new ssh里面， 但是出现了如下错误信息：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191025222414.png"/>

> Key is invalid. You must supply a key in OpenSSH public key format

<br>

## 错误原因：

使用`vim`复制出来，会发生格式变化，所以会一直不成功；

<br>

## 解决方法：

使用其他任意的文本编辑器， 打开这个隐藏文件夹下面的文件， 然后进行复制出来，粘贴到github即可成功。

- `Command+Shift+.` 可以显示隐藏文件、文件夹，再按一次，恢复隐藏；
- finder下使用`Command+Shift+G` 可以前往任何文件夹，包括隐藏文件夹。