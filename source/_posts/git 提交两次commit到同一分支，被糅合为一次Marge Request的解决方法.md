---
title: git 提交两次commit到同一分支，被糅合为一次Marge Request的解决方法:cherry-pick
date: 2019-10-13 10:09:18
toc: true
categories: 
 - [学习 - git]
tags: 
 - 版本控制
---



**简  述：** `git` 提交两次`commit`到同一分支，且也`push`到同一个远程仓库的分支，会被糅合为一次**Marge Request**的解决方法：使用`cherry-pick`解决

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [git 提交两次commit到同一分支，被糅合为一次Marge Request的解决方法:cherry-pick](https://blog.csdn.net/qq_33154343/article/details/102530349)

<br>

## 需求背景：

我在本地分支master进行编码完成，然后请求合并代码，进行了如下行为：

> 1. commit  A1 ， 然后 push1 到 self 仓库；
> 2. 网页gitLab上请求合并到公共分支origion；
> 3. commit  A2 ， 然后 push2 到 self 仓库；

然后我写A3的时候，意识到有如下情况会发生，赶紧新建一个新的分支branch_a3 临时保存一下A3修改

【问题】这个时候， 问题就产生了，origion 的 master 会自动将A1 +A2 的两次修改看做一次 Marge Request ；但是我想要的是，当做两次Marge Request 提交记录 ？现在的各个分支，情况如下图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20191013100800.png"/>

<br>

## 解决方案：

先来看一些远程仓库名称，作为背景：origion 是所有人请求合并的公共远程仓库，self 是自己从origion 进行 fork的个人远程仓库，如图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20191013102554.png"/>

<br>

### 可行思路：

> 1. ----------以下为单独抽取A2进行一次提交请求合并-----------
> 2. 先进行回退到 A0 版本： reset A0
> 3. 新建分支和跳转到 newBranch
> 4. 使用 cherry-pick A2  ; **只获取A2 的代码改动** 添加到 newBranch 分支的 A0上面
> 5. 再次推送到self的新的newBranch分支  （到这里已经把A2单独作为新的分支，提交一次改动）
> 6. 网页进行请求合并
> 7. -----------以下为A1+A2 恢复为 A1-----------
> 8. checkout 跳转到 master 分支
> 9. 先进行回退到 A1 版本：reset A1
> 10. 再次推送到self的旧的master分支 （origion 的A1 提交也会自动刷新覆盖，只有A1提交）
>

<br>

### 命令实现：

```bash
466e5d0  A0
73e40aa  A1
b7a3dc5  A2

git reset --hard 466e5d0
git checkout  -b newBranch
git cherry-pick 73e40aa
git push self newBranch -f
网页进行请求合并

git checkout master
git reset --hard b7a3dc5
git push self master -f
```

