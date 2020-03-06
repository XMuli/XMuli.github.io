---
title: MacOS10.14安装虚拟机Parallels_Desktop，安装UOS20和 QtCreaor，搭建dtk开发环境
date: 2020-02-03 15:41:43
toc: true
categories: 
 - [学习 - MacOS]
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简 述：** 本篇讲述和演示和如下的几个知识📚：

- 在 `MacOS10.14` 上面安装`Parallels_Desktop14.0.1`虚拟机
- 再在 `Parallels_Desktop` 上面安装`uos20 x64` 
- PD 给虚拟机里面的系统安装 `Parallels Tools`；解决最大化非全屏的现象（调整分辨率后也是一个正方形的窗口）
- 在 `uos20` 操作系统上，安装和配置最新的 `Qt Creator` 集成环境IDE
- 配置 **dtk** 的开发环境，
- 在 `uso20` 上使用 `openVPN` 从家庭外网连接到公司内网，实现远程办公
- 在 `uos20` 获取开发者权限，拥有 `sudo` 命令的权限

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [MacOS10.14安装虚拟机Parallels_Desktop，安装UOS20和 QtCreaor，搭建dtk开发环境](https://blog.csdn.net/qq_33154343/article/details/104180794) 

<br>

## 相关博文：

- [在win10里面的VMware安装UOS20，在uos20里面安装QtCreator，配置dtk开发环境](https://blog.csdn.net/qq_33154343/article/details/103733327) 

<br>

## 前期准备：

所要使用到的一些软件和具体版本号，前期需要准备的工作如下：

- **硬件：** 一台的笔记本或者一体机
- **软件**：
  - **系统环境：** `MacOS 10.14.6 (18G103)`  
  - **虚拟机软件： **`Parallels_Desktop14.0.1.dmg`
  - **uos20 镜像：** `uos-20-desktop-amd64.iso`

- 其它(可选)：
  - **opnvpn 配置文件：** 联系自己的管理员获取
  - 或者自备梯子或其他工具

<br>

## 安装虚拟机 Parallels_Desktop：

查看 MacOS操作系统版本：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_114750.png" style="zoom: 50%;" />

- 双击`Parallels_Desktop14.0.1.dmg` 运行程序

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_120547.png" style="zoom:50%;" />

- 再次双击运行程序

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_120726.png" style="zoom:50%;" />

- 点击 **打开** 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_120737.png" style="zoom:50%;" />

- 点击**跳过该版本**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_120811.png" style="zoom:50%;" />

- 还会弹出一个小的窗口，提示建议安装更新的版本，点击**不，使用现有的**，这里这个图截掉了。

- 弹出软件许可协议窗口，点击请**接受**， 等待几秒钟，虚拟机自动安装成功。

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_120845.png" style="zoom:50%;" />

  <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_121415.png" style="zoom:50%;" />

- 看到安装成功

<br>

## 禁止 Parallels_Desktop 🚫更新 ，外加优化 PD

### 优化 PD，赋予权限：

第一次启动，会提示如下，按照提示，打开`偏好设置`

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_121442.png" style="zoom:50%;" />

点击`允许`即可

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_121514.png" style="zoom:50%;" />

且这里也赋予 PD 相应的权限，且➕🔐避免被再次更改。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_123526.png" style="zoom:50%;" />

- 都操作完成之后，点击查看版本信息，发现已经是激活的版本了。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_121730.png" style="zoom:50%;" />

### 禁止 PD 更新：

这里按照个人喜好，我是不喜欢如下改动的，且不想检测自动更新。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_121905.png" style="zoom:50%;" />

<br>

## 安装 uos20 操作系统：

- 将 uos20 的镜像，拖曳于这里点击打开

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_122300.png"/>

- 若是弹出，无法检测到镜像，不予理会即可，实际会检测到

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_122531.png" style="zoom:50%;" />

- 因为 uos20 和 deepin15 都是基于 Debian 系统，选中如下

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_122611.png" style="zoom:50%;" />

- 选择安装路径（可以更改）

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_122636.png" style="zoom:50%;" />

- 点击创建，即可进入安装 uos20 的界面

<br>

## 配置uos20操作系统：

- 按照提示，选择即可，下面👇只显示出重要的步骤：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_122726.png" style="zoom:50%;" />

- 这里不会的话，就默认全盘安装，不作任何修改；**加密磁盘** 没有必要勾选☑️，如果你的系统没有存放什么国家机密文件的话，普通人是用不着的。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_122801.png" style="zoom:50%;" />

- 等待进度条进行 100% 即可

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_123230.png" style="zoom:50%;" />

- 显示安装成功

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_123446.png" style="zoom:50%;" />

<br>

## 安装 Parallels Tools：

<font color=#D0087E size=4 face="幼圆">因为在 macOS 系统里面，将 uos20 全屏的话，会发现总是一个正方形的窗口，即使更改分辨率也不可以，反正就是不是全屏效果，让人抓狂💥💥💥；</font>

**解决方法如下：**

- 只需要选中当前开启的虚拟机，打开`操作-Parallels Tools`， 然后在本目录路径下，

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_125121.png" style="zoom:50%;" />

- 打开终端，执行命令

```bash
sudo ./install
```

- 即可，会出现如下终端的可视化界面，依次点击`确定`和`下一步`，然后耐心等待，等待界面如下图

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200211_195948.png" style="zoom:33%;" />

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_130508.png" style="zoom: 33%;" />



<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200224_132510.png" style="zoom:33%;" />

- 再次重启该虚拟机即可，点击最大化（可选择觉得舒服的分辨率），发现窗口已经占满整块屏幕了。 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_163250.png" style="zoom:50%;" />

<br>

## 安装 qtcrator 环境：

参考[在win10里面的VMware安装UOS20，在uos20里面安装QtCreator，配置dtk开发环境](https://blog.csdn.net/qq_33154343/article/details/103733327) 一文， 中间的[在`QtCretor`里面配置`dtk`开发环境和工程模板： ](https://blog.csdn.net/qq_33154343/article/details/103733327#QtCretordtk_148) 部分；此部分有着详细的描述

<br>

## 配置 dtk 的开发环境：

运行命令，然后重启QtCretor：

```bash
sudo apt install qtcreator-template-dt
```

<br>

## 配置 openVPN 回到公司内网：

- 向管理员获取 openVPN 的配置文件，比如`wh-vpn.conf`

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_201612.png" style="zoom:50%;" />

- 将其导入到 uos20 系统里面，具体位置如下：**控制中心-网络-vpn-导入** 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_171623.png" style="zoom:50%;" />

若是连接成功之后，仍然无法访问内网资源，请将**“使用相对应的网络的资源”** 开启☑️

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_202254.png" style="zoom:50%;" />

<br>

## 打开开发者模式，获取 sudo 权限：

- **控制中心-系统信息-关于本机-版本授权** 中，点击激活，若是没有永久激活码，可以点击试用激活，这个有 90 天的时间

- 在**控制中心-通用-开发者模式** 中，点击到处机器码到桌面上，默认是 1.json 文件
- 登录网址 [https://www.chinauos.com/applyfor](https://www.chinauos.com/applyfor) ，注册用户为 uos 账号
- <font color=#D0087E size=4 face="幼圆">在登录状态下</font> 点击“**请参与内测-点击<u>这里</u>了解如何开启开发者模式**”，
- 上传机器码文件，点击下载注册码文件，然后**控制中心-通用-开发者模式** 中导入注册码

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200203_203928.png" style="zoom:50%;" />