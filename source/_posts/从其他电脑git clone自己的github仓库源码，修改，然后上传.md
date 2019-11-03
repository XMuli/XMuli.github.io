---
title: 从其他电脑git clone自己的github仓库源码，修改，然后上传
date: 2019-8-26 18:36:34
toc: true
categories: 
 - [学习 - git]
 - [学习 - 编码规范，辅助技巧]
---



**简介：**  从其他的一台电脑（`Linux`）**git clone**自己的**github**仓库（主力电脑**win10**创建的仓库test）源码，修改，然后上传云端的**github**

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  在Linux的电脑环境下， 终端输入命令,用来拷贝一份远程的仓库

```bash
git clone git@github.com:touwoyimuli/test.git    
```

遇到如下的**错误提示**：

> 正克隆到 'test'...
>
> The authenticity of host 'github.com (140.82.118.3)' can't be established.
>
> RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
>
> Are you sure you want to continue connecting (yes/no)? yes 
>
> Warning: Permanently added 'github.com,140.82.118.3' (RSA) to the list of known hosts.
>
> git@github.com: Permission denied (publickey).
>
> fatal: 无法读取远程仓库。
>
> 请确认您有正确的访问权限并且仓库存在。

难过，伤心，这里记录一些解决方法。

<br>

**编程环境：**  `deepin 15.11 x64 专业版 `    **Kernel：** `x86_64 Linux 4.15.0-30deepin-generic`

## <font color=#D0087E  face="幼圆">重要提示：</font>

- 若遇`csdn`的博文排版、文字、图片、链接、视频预览等异常，会删除该部分，或用链接代替，或删除该部分，<font color=#FE7207 size=4 face="幼圆">**但在   [github.io](https://touwoyimuli.github.io/) 博客上体验完美,**</font>  <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [从其他电脑git clone自己的github仓库源码，修改，然后上传](https://blog.csdn.net/qq_33154343/article/details/100083909)

<br>

## 错误原因：

这是因为`Git`使用`SSH`连接，而`SSH`第一次连接需要验证`GitHub`服务器的`Key`。确认`GitHub`的`Key`的指纹信息是否真的来自`GitHub`的服务器。解决办法。其实就是在本地生成`key`配置到`github`服务器。这样子接收过来就`gitHub`服务器了。

<br>

## 解决方法：

- 查看当用户目录下是否有相关的**ssh**密钥

```bash
ls -al ~/.ssh     //查看用户目录下的.shh文件夹下所有文件
```



- 配置用户，需要按 **“回车--Y和回车--回车”**， 一共三次

```bash
ssh-keygen -t rsa -C "touwoyimuli@gmail.com"   //配置用户
```



- 查看生成的**github**的**Key**

```bash
cat ~/.ssh/id_rsa.pub     //查看生成的key：cat
```



- 登陆**github**,点击**头像-settings-new SSH**,复制新生成的SSH**配置**到服务器,记住拷贝是上一步的秘钥信息以**ssh-rsa**开始邮箱结束的
- 再次克隆，输入一开始的命令`git clone git@github.com:touwoyimuli/test.git`   ， 正常克隆跟同步代码到**github**。完美解决。



## 全程图片：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190826185945.jpg"/>

<br>

## 开心分享：

因为有着热心网友的无私分享，故不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190825170829.jpg"/>