---
title: hexo博客 Maupassant主题 旧电脑迁移到新电脑
date: 2019-6-29 23:01:12
updated: 2019-6-29 23:01:17
toc: true
categories: 
 - [学习 - hexo]
 
tags: 
 - hexo 
 - Maupassant
 - 迁移到新电脑/环境
---



​		将旧电脑/环境下面的`hexo`博客环境，迁移到新电脑/环境里面，所需要的运行操作。

<!-- more -->

[TOC]



## 本博文的简述or解决问题？

​		将旧电脑/环境下面的`hexo`博客环境，迁移到新电脑/环境里面，所需要的运行操作。



**发博配置：**  win10 x64 专业版



## 安装Hexo博客框架：

> 参考：[**Hexo文档**](https://hexo.io/zh-cn/docs/)

什么是 Hexo？

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。



### 安装前提

安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：

安装Node.js

[Node.js](http://nodejs.org/) (此刻最新：10.16.0 LTS ; win x64)

安装成功:

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190629231243.png)



验证是否安装成功：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190629231913.png)

#### 安装git：

#### 官网[Git](http://git-scm.com/)下载

验证是否安装成功：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190630003732.png)



### 安装Hexo框架：

如果您的电脑中已经安装上述必备程序，那么恭喜您！接下来只需要使用 npm 即可完成 Hexo 的安装。

```html
$ npm install -g hexo-cli
```

如果您的电脑中尚未安装所需要的程序，请根据以下安装指示完成安装。

安装成功：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190630003502.png)





## 安装Maupassant主题皮肤：

> 参考来源：[**Maupassant安装**](https://www.haomwei.com/technology/maupassant-hexo.html)

安装主题和渲染器：

```css
$ git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant (已存在安装过，此句不用执行)

$ npm install hexo-renderer-pug --save
$ npm install hexo-renderer-sass --save
```

验证安装是否成功：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190630010358.png)



## 安装文章字数统计、阅读量统计：

> 参考：[Hexo文章计数插件WordCount](https://www.jianshu.com/p/e122fc6f5946)

和`maupassant`主题的`_config.yml`配置文件有这句话：

`wordcount: true ## If you want to display the word counter and the reading time expected to spend of each post please set the value to true, and you must have hexo-wordcount installed.`

运行命令：

```css
npm i --save hexo-wordcount
```

显示效果：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190630152429.png)



可能会有几分钟的延时，才会显示正常（我也不小的为什么，一开始还以为设置无效了，巨坑）。



## 验证迁移成功：

然后再次文件夹里面的博文发布一下，看是否已经确定移植OK？

运行命令：

```css
hexo clean
hexo g -d
```

 看到如下结果就表示OK：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190630010819.png)



## 本次心得：

​		注意要到`hexo/`文件夹下进行相关的命令：

​		以上步骤简单概述就是如下：

- 安装Node.js

- 安装Hexo框架
- 安装主题的渲染器



**本博文同步到csdn博客：** [**hexo博客 Maupassant主题 旧电脑迁移到新电脑**](https://blog.csdn.net/qq_33154343/article/details/94236384)