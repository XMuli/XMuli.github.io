---
title: 框架hexo从win10迁移到新的电脑macOS10.14.6，重新设置环境变量
date: 2019-10-25 21:49:52
toc: true
categories: 
 - [学习 - hexo]
tags: 
 - hexo
---



**简  述：** **将我自己的博客框架`hexo`从`win10`迁移到新的电脑`macOS10.14.6`，重新设置环境变量，以便后面更好继续的写博客；**之前一直都是在win10上面工作和写任务，现在前移到了macOS环境，所以以前的所有环境（IDE、图床、github本地用户配置、vim颜色配置、hexo命令等）的这些个环境变量等等，都是需要我自己来重新配置；还有很多软件都是重新寻找替代品；习惯也都是需要在新的系统（向往一个好看的Linux/Uinx环境已经很久了）来适应的；讲真，想用苹果的MacOS已经很久，之前只是在虚拟机感受体验一下过， 界面很好看，但是黑苹果毕竟体验不佳（没有预料的如丝绸一般的体验），这不，新的MacBook Pro 13到手，就迁移新机；嘿嘿;

<!-- more -->

[TOC]

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [框架hexo从win10迁移到新的电脑macOS10.14.6，重新设置环境变量](https://blog.csdn.net/qq_33154343/article/details/102750585)

<br>

## 当前背景：

之前一直是在`win10`上面，发布和维护我的博客[https://touwoyimuli.github.io](https://touwoyimuli.github.io/) ，其中`hexo`工具命令，这会直接将整个文件夹（主要是`*.md`文件）拷贝过来了；现在需要的就是，参考以前的文章，将`hexo`在`macOS`上面再次安装一下，以及一些插件等等，阿西吧！想着就头疼（安装好了，以后用新笔记本写博客，也一定是爽歪歪~），全部就当是复习一遍咯，**系统版本是win10迁移到macOS10.14.6**

<br>

## 安装node.js:

Node.js的官网：[https://nodejs.org/en](https://nodejs.org/en/) ,可以自行下载，推荐下载LTS（长期支持版本）版本；截止2019-10-24 22:55时刻，作者官网最新的LTS下载的版本是12.13.0；

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191024235102.png"/>

<br>

## 安装git：

貌似macOS是自带的git，不过这里还是查询一下node.js是否安装成功，以及git的版本号；

若是没有安装git话，可以自己去git官网下载安装：[https://git-scm.com/downloads](https://git-scm.com/downloads) 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191024235547.png"/>

<br>

## 安装Hexo框架：

先检查一下，本机器有没有安装hexo，确认一番；再上述两个都安装好了之后，即可以安装`nmp`，来安装`Hexo`的安装。(推荐开启全局代理，速度更快运行以下命令)

使用官方[https://hexo.io/zh-cn/docs](https://hexo.io/zh-cn/docs/)的推荐命令：

```bash
$ npm install -g hexo-cli
```

 使用官方命令安装，却发现一直会安装失败？？？出现如下错误提示。。。这是么样个烫饭？？？待我google一番，再回来，哼哼~，报错如下：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191025004530.png"/>

实际过程中，会直接编译失败，通过参考[Mac系统下配置Hexo博客运行环境遇到的问题](https://www.itfanr.cc/2017/10/27/problems-for-configuring-hexo-blog-in-mac/)一篇，虽然没有得到针对我的没有起效，但是却有启发我；**最终在先尝试解决方法一无效之后，感悟出了解决方法二，安装成功。** 在此，仍然把两种方法都再写此处，万一有一种可以适合你捏~

<br>

### 解决方法一（会继续报其它报错，失败）：

- 第一步,赋予目录权限:

```bash
$ sudo chown -R `whoami` /usr/local/lib/node_modules
```

- 第二步,安装hexo:

```bash
$ npm install -g hexo-cli
```

**需要注意的点**: 在安装hexo时,不要用 `sudo` 命令.



安装过程会报其他`ERROR`：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191025004349.png"/>

<br>

### 解决方法二(成功安装)：

或许是方法一有了铺垫吧（开启了第一个文件路径夹得权限）？，当我尝试运行如下命令时候(反着来方法一)，却安装成功了

```bash
sudo npm install -g hexo-cli
```

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191025004615.png"/>

<br>

## 验证迁移成功：

终端进入到自己的文件夹下面的路径下，运行熟悉的命令

```bash
hexo clean
hexo g -d
```

cao,怎么没有成功，看了一眼报错error没想起来，应该是里面的配置数据没有修改（运行推送到github.io仓库）的秘钥不正确（实际不是，原因见下）；大概是如此吧。不过这之前，还是先查查看；我再去改改；报错如下：

### node-sass报错拦路虎：

> Error: Missing binding /Users/yuanyi/touwoyimulier/node_modules/node-sass/vendor/darwin-x64-72/binding.node
>
> Node Sass could not find a binding for your current environment: OS X 64-bit with Node.js 12.x
>
> 
>
> Found bindings for the following environments:
>
> \- Windows 64-bit with Node.js 10.x
>
> 
>
> This usually happens because your environment has changed since running `npm install`.
>
> Run `npm rebuild node-sass` to download the binding for your current environment.
>
> at module.exports (/Users/yuanyi/touwoyimulier/node_modules/node-sass/lib/binding.js:15:13)
>
> at Object.<anonymous> (/Users/yuanyi/touwoyimulier/node_modules/node-sass/lib/index.js:14:35)

### node-sass解决参考：

**解决方法**(对我无效，只能另寻它法)：

- 删除`/Users/yuanyi/touwoyimulier/node_modules/`下面的`node-sass`文件夹

- 运行命令:

- ```bash
    npm i node-sass
    ```

这个等待的时间有点长，起码二十分钟，进度条才走了20%的样子;其那面的卡住时间比较长，后面会很快（相对而言）；意意识到可能是网络原因，下载和上传都为速度为1~2KB/S，开了代理，也试过全局，干其他下载都没有问题，就只这个命令运行好慢，好晚了，先去睡觉，明天一早看结果，或者后面再试试; 我搽，运行结束了，但是报错了。。。。。。难过。。。。

然后又参考了文末的一些参考文章，柑橘是最后一篇的这来两句命令产生了效果(最后一句产生了效果)；因为运行的其他的，全部都失产生了各种各样的错误，**但是总结下来，应该`npm`和`node-sass`这两个鬼东西没有安装好**吧。**若是你也一样， 可以尝试运行下面这些命令（试一下加上`sudo`，或许会OK）；**

//半夜02：37更新：因为我还没睡(明天还要上班),晕

### node-sass解决成功：

#### 以下为最后成功的解决方法全过程：

我依次运行如下命令，供大家参考，也给自己一个日后查看：

- 运行`hexo clean`报错：

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191025021732.png"/>

- 运行`hexo -v`，报错：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191025021958.png"/>

- 运行`cd /Users/yuanyi/touwoyimulier/node_modules`和`ls`,查看和想要删除`node-sass`

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191025022240.png"/>

- 卸载`npm uninstall node-sass`,报错，和错误提示的最后几行，没有提示怎么做？比如修复代码

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191025022417.png"/>

- 运行`npm i node-sass --sass_binary_site=https://npm.taobao.org/mirrors/node-sass/`（**对的，就是这一句，运行成功了）**,再碰碰运气。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191025023004.png"/>

- 去github看一下，确认一下，是彻底成功了；但是github没有那个绿色的小方块格子，应该是没有本地`.git`文件夹，还是用的以前的配置，所以不算贡献，后面有空初始化，再配置一下下；就完美了

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img/20191025023420.png"/>

<br>

#### 最后解决方法，此处小结一下：

```bash
npm uninstall node-sass
npm i node-sass --sass_binary_site=https://npm.taobao.org/mirrors/node-sass/  （应该是这句成功了）

npm audit fix     //再修复一下

hexo clean        //清理缓存
hexo g -d         //发布到github
```

<br>

**参考博文**：

- [hexo博客 Maupassant主题 旧电脑迁移到新电脑]([https://touwoyimuli.github.io/2019/06/29/hexo%E5%8D%9A%E5%AE%A2-Maupassant%E4%B8%BB%E9%A2%98-%E6%97%A7%E7%94%B5%E8%84%91%E8%BF%81%E7%A7%BB%E5%88%B0%E6%96%B0%E7%94%B5%E8%84%91/](https://touwoyimuli.github.io/2019/06/29/hexo博客-Maupassant主题-旧电脑迁移到新电脑/)) (win10->win10)自己参考自己的文章哈哈哈哈哈哈哈
- [Mac系统下配置Hexo博客运行环境遇到的问题](https://www.itfanr.cc/2017/10/27/problems-for-configuring-hexo-blog-in-mac/) 
- [nodejs8.x hexo-renderer-sass报错解决方法](http://www.is17.com/96/)
- [博客迁移到 Hexo 遇到的一些问题 | Hexo](https://www.fangr.cc/2017/09/27/hexo-workflow.html)
- [整理 node-sass 安装失败的原因及解决办法](https://segmentfault.com/a/1190000010984731) (重点参考)