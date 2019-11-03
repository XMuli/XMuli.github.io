---
title: 关联windows的本地环境和自己的github仓库
date: 2019-8-25 16:53:25
toc: true
categories: 
 - [学习 - git]
 - [学习 - 编码规范，辅助技巧]
tags: 
 - github
---



**简介：** 关联**windows**的本地环境和自己的**github**仓库

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  看简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  **操作系统版本：**`17134.829` 

## <font color=#D0087E  face="幼圆">重要提示：</font>

- 若遇`csdn`的博文排版、文字、图片、链接、视频预览等异常，会删除该部分，或用链接代替，或删除该部分，<font color=#FE7207 size=4 face="幼圆">**但在   [github.io](https://touwoyimuli.github.io/) 博客上体验完美,**</font>  <font color=#D0087E  size=4 face="幼圆">**本篇[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [关联windows的本地环境和自己的github仓库](https://blog.csdn.net/qq_33154343/article/details/100064928)

## 教程步骤：

- 输入自己的用户名和邮箱（为注册**github**账号时的用户名和邮箱）

```bash
git config --global user.name "touwoyimuli"
git config --global user.email "touwoyimuli@gmail.com"
```

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190824191058.jpg"/>

- 设置`SSH Key`

先检验本机是否生成密钥，执行命令：

```bash
   $ cd ~/.ssh
   $ ls
```

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190824191123.jpg"/>

如果没有密钥，则执行以下命令来生成密钥：

```bash
$ ssh-keygen -t rsa -C "touwoyimuli@gmail.com"
```

生成过程中按3次回车键就好（默认路径，默认没有密码登录），生成成功后，去对应默认路径里用记事本打开**id_rsa.pub**，得到`ssh key`公钥。



- 复制生成的公钥的内容，如下

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190824191426.jpg"/>



- 为[github官网](https://github.com) 账号配置`SSH key`  

进入：右上角--个人信息--setting

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190824191141.jpg"/>



- 创建一个新的`ssh`，并且将上一步的内容赋值进去，生成一个新的`ssh`钥匙

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190824191554.jpg"/>

- 添加新的`SSH keys`


<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190824191619.jpg"/>

添加成功

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190824191705.jpg"/>



- 关联`github`仓库，复制仓库地址：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190824192321.jpg"/>



- 输入命令`git clone 仓库ssh链接`下载

```bash
git clone git@github.com:touwoyimuli/QtExamples.git
```

这样就拷贝了完整的项目，当自己修改文件之后，便是提交 `git push`到远程的**github**仓库

<br>

## 分享开心：

因为有着热心网友的无私分享，故不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)
