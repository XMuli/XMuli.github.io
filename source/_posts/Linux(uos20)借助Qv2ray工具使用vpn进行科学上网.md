---
title: Linux(uos20)借助Qv2ray工具使用vpn进行科学上网
date: 2020-02-07 09:47:26
toc: true
categories: 
 - [学习 - Linux]
 - [学习 - 科学上网vpn]
tags: 
 - deepin
 - vpn
---



**简  述：**  Linux `uos20（deepin20）`  系统通过 `Qv2ray` 配置后，使用 `VPN` 进行 `google`。这也是一篇`Qv2ray` 的使用和配置教程文章（在 Linux 下）。

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  无

<br>

## 系统环境：

**编程环境：**  `uos 20 x64 专业版 `    **Kernel：**  `x86_64 Linux 4.19.0-5-amd64`



这里解释一下：uos 和 deepin 现在是☞同一个操作系统的两个版本；其都是由武汉深之度公司（2019-12月后更名为统信软件）开发的；
区别如下：

- deepin 20 表示社区版
- uos 20 表示专业版
- 两者基本没太大有区别，只是存在 login 和小部分的功能限制差异，但是其他无限制

<br>

## 下载Qv2ray和v2ray-cor：

- 下载`Qv2ray`： `Qv2ray-refs.tags.v2.0.1-linux-libqvb.AppImage`

  - [https://github.com/Qv2ray/Qv2ray/releases](https://github.com/Qv2ray/Qv2ray/releases)

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200207_100614.png" style="zoom:50%;" />

- 下载`v2ray-cor`： `v2ray-linux-64.zip`

  - 其中 linux-64 就是指 amd-64版本
  - [https://github.com/v2ray/v2ray-core/releases](https://github.com/v2ray/v2ray-core/releases)

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200207_114446.png" style="zoom:50%;" />

<br>

## 配置Qv2ray步骤：

- 解压缩`v2ray-linux-64.zip`文件得到文件夹📂`v2ray-linux-64`

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200207_103007.png"/>

  

- 打开📂`v2ray-linux-64`，将这四个文件`geoip.dat   geosite.dat   v2ctl   v2ray`都赋予可执行权限「全部勾选☑️如下的“允许以程序执行”」；

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200207_102901.png"/>

  

- 将这四个文件全部都放到`~/.qv2ray/vcore`文件夹里面，**vcore** 不存在的话就自己创建这个文件夹📂

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200207_104406.png"/>

  <br>

  ## 配置系统的Socks和http的端口代理：

- 设置让Linux（此处 uos20）本机应用使用代理

  - 查看本机`Qv2ray`的`Socks` 和 `Socks` 代理的端口
  - 打开 uos 的**“控制中心-网络-系统代理-手动”** ，按照上面的图的提示进行添加，然后保存

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200207_113437.png"/>

  

- 打开浏览器，访问 [https://www.google.com](https://www.google.com/)这个页面，Linux 借助 Qv2ray 使用 vpn 和成功。

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200207_113602.png"/>

<br>

## 其它补充：

- 第一次使用 Qv2ray 官方教程：
  - 官方教程： [第一次使用 Qv2ray 教程](https://github.com/Qv2ray/Qv2ray/wiki/Getting-Started-step0_zh)

好像这个「新手引导中文页面」页面隐藏的还挺深，想必也是因为一些原因吧。哪有好用，免费，简洁，易上手，无广告，活得久的软件呢？（🐶头，逃）