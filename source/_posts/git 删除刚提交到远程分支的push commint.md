---
title: git 删除刚提交到远程分支的push commint
date: 2019-11-06 21:17:00
toc: true
categories: 
 - [学习 - git]
---



**简  述：**  刚刚使用另外一个账号在本地`commit`， 然后`push self`了，然后想要删除此次的远程提交；但是全部代码和工作区内容， 然后更换一个正确的git账号（ **user.name** 和 **user.email** ）,再次生成一次`commit`，再`push self`；

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [git 删除刚提交到远程分支的push commint](https://blog.csdn.net/qq_33154343/article/details/102943552)

<br>

这次比较简单， 但是感觉会比较实用，网上教程写的比较繁琐，我的步骤如下：

```bash
git reset HEAD^                  //等价于git reset -–mixed HEAD^，撤回本地上一次的commit提交，但是保留改动代码和工作区内容 
git push self master -f          //因为版本落后远程分支，故加上-f
```
