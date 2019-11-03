---
title: git将本地代码，提交到github远程仓库
date: 2019-8-8 22:07:24
toc: true
categories: 
 - [学习 - git]
 - [学习 - 编码规范，辅助技巧]
---



**简介：** 当写完了代码的时候，想要提交到自己的远程仓库中时候，主要执行`git commit`; `git push`流程 。

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `deepin 15.11 x64 专业版 `    Kernel： `x86_64 Linux 4.15.0-30deepin-generic`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [git将本地代码，提交到github远程仓库](https://blog.csdn.net/qq_33154343/article/details/98889416)

<br>

## git提交到远程仓库：

写完了代码，提交到远程仓库的准备工作，和最后的提交

```bash
git status                               //查看状态
git diff   或  git diff   ../路径/xxx.*   //查看不同
git add .                                //将所有 修改的文件 加入git监管 (有一个 .)
git commit                               //提交到本地仓库
git log                                  //查看 提交的给log日志
git  remote -v                           //查看远程仓库 （一个本地git， 可以有多个远程仓库）
git  push  myself                        //myself 是自己的远程仓库（某一个他人仓库的fork）
```

团队合作的时候，想git pull 一下代码，变基处理（合并其他人提交的代码合并，当做已经有的基准）

一个更为详细的处理方式（**推荐**）：

```bash
git pull origin master --rebase   //更新代码，变基
//修改冲突（手动修改）
git rebase --continue       //修改完成，但不要提交，先这一步继续变基
git status                  //查看状态
git add .                   //添加到.git管理
git commit                  //提交
git push                    //提交到默认远程仓库
```

若是git push失败，则使用下面这个命令，强制推送：

```bash
git push self master -f     //强制提交到某一个远程仓库
```

<br>

## 参考博文：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

参考：

[git pull --rebase origin master](https://www.jianshu.com/p/0cd05dd1cc73) 





