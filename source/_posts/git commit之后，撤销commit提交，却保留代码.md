---
title: git commit之后，撤销commit提交，却保留代码
date: 2019-9-3 18:24:23
toc: true
categories: 
 - [学习 - git]
---



**简介：**  已经**git commi**t一次提交，但是想撤销**commit**这次提交，且保留代码不变

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [git commit之后，撤销commit提交，却保留代码](https://blog.csdn.net/qq_33154343/article/details/100524686) 

<br>

## 问题背景：

```bash
写完代码后，我们一般这样
git add . //添加所有文件
git commit -m "本功能全部完成"
```

执行完**commit**后，想撤回**commit**，怎么办？

<br>

## 解决方法：

```bash
git reset --soft HEAD^
```

这样就成功的撤销了你的commit

注意，仅仅是撤回commit操作，您写的代码仍然保留。



- 说一下个人理解：

HEAD^的意思是上一个版本，也可以写成HEAD~1

如果你进行了2次commit，想都撤回，可以使用HEAD~2

<br>

## 关于参数：

> --mixed 

意思是：不删除工作空间改动代码，撤销commit，并且撤销git add . 操作

这个为默认参数,git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的。



> --soft  

不删除工作空间改动代码，撤销**commit**，不撤销**git add .** 



> --hard

删除工作空间改动代码，撤销**commit**，撤销**git add .** 

注意完成这个操作后，就恢复到了上一次的commit状态。



## 顺便说一下，如果commit注释写错了，只是想改一下注释，只需要：

```bash
git commit --amend
```

此时会进入默认vim编辑器，修改注释完毕后保存就好了。

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

参考：

[git使用情景2：commit之后，想撤销commit](https://blog.csdn.net/w958796636/article/details/53611133) 