---
title: macOS执行npm install -g hexo-cli失败的解决方法
date: 2019-11-03 21:49:15
toc: true
categories: 
 - [学习 - Linux]
 - [学习 - hexo]
tags: 
 - hexo
---

**简  述：**  按照官网安装`hexo`的教程命令；执行命令`npm install -g hexo-cli`时候， **报错**如下的解决方法

```bash
npm WARN checkPermissions Missing write access to /usr/local/lib/node_modules/hexo-cli
npm WARN checkPermissions Missing write access to /usr/local/lib/node_modules/hexo-cli/node_modules/chokidar
npm ERR! code EACCES
npm ERR! syscall access
npm ERR! path /usr/local/lib/node_modules/hexo-cli
npm ERR! errno -13
npm ERR! Error: EACCES: permission denied, access '/usr/local/lib/node_modules/hexo-cli'
npm ERR!  [Error: EACCES: permission denied, access '/usr/local/lib/node_modules/hexo-cli'] {
npm ERR!   stack: "Error: EACCES: permission denied, access '/usr/local/lib/node_modules/hexo-cli'",
npm ERR!   errno: -13,
npm ERR!   code: 'EACCES',
npm ERR!   syscall: 'access',
npm ERR!   path: '/usr/local/lib/node_modules/hexo-cli'
npm ERR! }
npm ERR! 
npm ERR! The operation was rejected by your operating system.
npm ERR! It is likely you do not have the permissions to access this file as the current user
npm ERR! 
npm ERR! If you believe this might be a permissions issue, please double-check the
npm ERR! permissions of the file and its containing directories, or try running
npm ERR! the command again as root/Administrator.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/yuanyi/.npm/_logs/2019-10-30T11_43_55_100Z-debug.log
```

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [macOS执行npm install -g hexo-cli失败的解决方法](https://blog.csdn.net/qq_33154343/article/details/102887721)

<br>

## 安装背景：

`hexo`官网：[https://hexo.io/zh-cn/docs/index.html](https://hexo.io/zh-cn/docs/index.html)

使用`macOS10.14.6`安装`hexo`的时候，采用官方的命令：`npm install -g hexo-cli`,安装出现，如下错误；

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img2/ss.png"/>

<br>

## 报错原因：

### 错误一(通常是这个原因)：

- 错误原因❎：没有使用管理员权限安装

    注意：**需要注意的点**: 在安装`hexo`时,不要用 `sudo` 命令.

    #### 解决方法：

    第一步,赋予目录权限:

    第二步，安装`hexo`：

```bash
sudo chown -R `whoami` /usr/local/lib/node_modules
npm install hexo-cli -g
```

<br>

### 错误二：

- 错误原因❎：由于`npm`的官方镜像源在国外,而由于国内”众所周知的”的网络原因,访问默认的官方镜像源常常会出问题.我们可以更改为国内的镜像源来加速软件的安装.

    #### 解决方法：

更换`npm`镜像源，根据众多网友推荐， 推荐使用**taobao**的镜像源：

**永久替换`npm`为`taobao`的方法:**

```bash
npm config set registry https://registry.npm.taobao.org
```

**永久替换`npm`为`官方源`的方法:**

```bash
npm config set registry https://registry.npmjs.org/
```

**查看设置是的`npm`源否生效：**

```bash
npm config get registry
```

#### 提醒：使用淘宝源和使用官方源具有可能会请求失败， 这里推荐两种都尝试一下，根据自己的情况而定

结合上面，附上一张安装成功的图片（因为之前安装过， 所以如下）：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img2/Snipaste_2019-10-30_20-26-41_mark.png"/>

<br>

## 错误三：

忘记是哪里看到的一篇文章，结合我自己的上一篇关于hexo安装的教程经验， 也的确示发现有可能是权限不足够，在macOS系统下， 运行需要添加sudo命令:

```bash
sudo npm install hexo-cli -g
```

此处解决方案三为以上两种方法均失败下， 可以一试； 原理还有很多不懂，望各网友见笑， 此处此文到此，权当一处记录，但倘若能够给你启发， 帮助到你， 我也是很高兴的。 朝闻道，夕可死！ 且学海无涯， 愿所见所闻有所记录，不曾叹叹息在此间走一遭；数风流人物， 不过百载，时间逝，仍然记几许？野草于群星，各自精彩，愿今后皆可以望

<br>

下面这一篇文章， 真的写的很棒，也很是清晰，相比网上的其他一大都是一抄十，十传百，百传遍全网；但是都是不符合，或者无法解决实际， 且没有说原因， 直接几个命令，令我痛惜， 下面放一篇比较精雕细琢的文章，花了心思写的，在此参考，表示感谢

参考文章：[Mac系统下配置Hexo博客运行环境遇到的问题](https://www.itfanr.cc/2017/10/27/problems-for-configuring-hexo-blog-in-mac/) 





