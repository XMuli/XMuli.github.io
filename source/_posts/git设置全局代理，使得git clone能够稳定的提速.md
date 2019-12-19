---
title: git设置全局代理，使得git clone能够稳定的提速800KB/S
date: 2019-11-27 21:22:07
toc: true
categories: 
 - [学习 - git]
 - [学习 - 科学上网vpn]
---

**简  述：**  设置`git `会走全局代理，从而让`git clon`提高下载速度，下载速度稳定在`800KB/S`，不再是`几KB/S`的速度，让人揪心‘

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [git设置全局代理，使得git clone能够稳定的提速800KB/S](https://blog.csdn.net/qq_33154343/article/details/103504497)

<br>

## 背景需求：

**使用`git`或者`brew`或者`apt`时候，`几KB/S`的网速用的落下了眼泪；**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/EIn5WzKUUAEq74G.png"/>

**还经常判断为网络错误为断开；又是更换源，又是架`vps`，又是代理研究，为了下载安装一个软件，通常要耗巨时来做前期准备工作；无意思看到`zhihu`的时候，我真的忍不住的想哭，就不能让我们潜心的研究设计和`code`的原理，变得更加方便一些否?**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/EIn5WzLUEAAKed9_mark.png"/>

实际体验`git+github`几个月之后的测速：一般是为`20~40KB/S`；晚上容易`小于10KB/S`的速度；小概率无法打开被墙；小于前者的概率为稳定的`几M/S`

<br>

## 网上需求它法:

首先想到的就是，看看其他人都是怎么提高`git clone`的下载速度的，查询一番，方法一共如下，若是有我未知的方法，欢迎补充和评论：

- 仅修改`hosts`文件
- 使用`vpn`，开启pac代理或者全局代理
- 使用服务器，搭建公司的私有`gitLab`，然后同步自己所需要的部分`github`仓库

经过实际的测试与使用体验，发现第一个很难起到作用，遇到被墙的仍然是徒劳，且存在不同区域的网络有各自的成功或失败，此方法难以复制；方法二而分为两种。一种是自己购买vps搭建，另外是直接购买现成的线路来使用，属于我比较推荐的一个，花点小钱钱💰，可以避免很多无用的生气，心平气和的敲代码不好吗？方法三，我公司就是这样操作的，此处不做探究

<br>

## 解决方法：

### 查看所有git配置:

```bash
git config --global -l
```



### 只是针对github进行代理：

`git`提供的下载方式有两种，一种是`ssh`，另外一种是`https`的下载方式；应该是一次对应着网上的教程中的`socks5`和`https + http`的两种下载模式；

<font color=#FE7207 size=4 face="幼圆">**设置之前，一定要查看自己的IP和端口是也是如下，具体IP和端口以自己的为准，下面是一般的默认数值**</font>

#### 设置全局代理（https + http）：

```bash
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
```

#### 设置全局代理（socks5）：

```bash
git config --global http.https://github.com.proxy socks5://127.0.0.1:1086
git config --global https.https://github.com.proxy socks5://127.0.0.1:1086
```



### 取消设置的代理:

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

<br>

## 设置之后:

**说明一下，我已经开启了全局代理模式的，因为今天，在不开代理和仅仅只开pac代理模式下，都是无法访问`github`的，可能是阶段性的此片`IP`被墙了吧！！**！真的让人火大~；在终端测试`git clon`自己一个仓库，下载速度为**700-800KB/S**左右，**比较稳定**；网多传的达到稳定的几M/S的速度，我只是峰值达到过，然后就降速下来了，也不知道部分博文是否截图来误导性？？？ 嗯？？
我设置之后，配置截图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-11-24_19-41-32_mark.png"/>

比较稳定的速度：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-11-24_19-41-33_mark.png"/>



<br>

## 更新：

前两天，IP被墙的原因过了两天找到了， 是在这个；居然被我猜中了，还真的是。。。。。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/IMG_9254_mark.JPEG"/>

<br>

**参考：**

[GitHub 代理设置](https://tding.top/archives/cbef72d4)