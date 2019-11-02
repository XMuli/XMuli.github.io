---
title: Linux中apt与apt-get命令的区别与解释
date: 2019-10-11 23:23:09
toc: true
categories: 
 - [学习 - Linux]
tags: 
 - Linux
---



**简  述：**  讲述Linux中`apt`与`apt-get`命令的区别与解释

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [Linux中apt与apt-get命令的区别与解释](https://blog.csdn.net/qq_33154343/article/details/102512268) 

<br>

> **简单来说就是：apt = apt-get、apt-cache 和 apt-config 中最常用命令选项的集合。**

### apt和apt-get命令之间的区别:

虽然 `apt` 与 `apt-get` 有一些类似的命令选项，但它并不能完全向下兼容 apt-get 命令。也就是说，可以用 apt 替换部分 apt-get 系列命令，但不是全部。

|     apt 命令     |      取代的命令      |           命令的功能           |
| :--------------: | :------------------: | :----------------------------: |
|   apt install    |   apt-get install    |           安装软件包           |
|    apt remove    |    apt-get remove    |           移除软件包           |
|    apt purge     |    apt-get purge     |      移除软件包及配置文件      |
|    apt update    |    apt-get update    |         刷新存储库索引         |
|   apt upgrade    |   apt-get upgrade    |     升级所有可升级的软件包     |
|  apt autoremove  |  apt-get autoremove  |       自动删除不需要的包       |
| apt full-upgrade | apt-get dist-upgrade | 在升级软件包时自动处理依赖关系 |
|    apt search    |   apt-cache search   |          搜索应用程序          |
|     apt show     |    apt-cache show    |           显示装细节           |

当然，`apt` 还有一些自己的命令：

|   新的apt命令    |              命令的功能              |
| :--------------: | :----------------------------------: |
|     apt list     | 列出包含条件的包（已安装，可升级等） |
| apt edit-sources |              编辑源列表              |

需要大家注意的是：`apt` 命令也还在不断发展， 因此，你可能会在将来的版本中看到新的选项。

<br>

## apt-get已弃用？

目前还没有任何 Linux 发行版官方放出 apt-get 将被停用的消息，至少它还有比 apt 更多、更细化的操作功能。对于低级操作，仍然需要 apt-get。

<br>

## 我应该使用apt还是apt-get？

既然两个命令都有用，那么我该使用 apt 还是 apt-get 呢？作为一个常规 Linux 用户，系统极客建议大家尽快适应并开始首先使用 apt。不仅因为广大 Linux 发行商都在推荐 apt，更主要的还是它提供了 Linux 包管理的必要选项。

最重要的是，apt 命令选项更少更易记，因此也更易用，所以没理由继续坚持 apt-get。

## 小结

最后结大家提供两点使用上的建议：

- `apt` 可以看作 `apt-get` 和 `apt-cache` 命令的子集, 可以为包管理提供必要的命令选项。
- `apt-get` 虽然没被弃用，但作为普通用户，还是应该首先使用 `apt`。



参考文章：[Linux中apt与apt-get命令的区别与解释](https://www.sysgeek.cn/apt-vs-apt-get/)