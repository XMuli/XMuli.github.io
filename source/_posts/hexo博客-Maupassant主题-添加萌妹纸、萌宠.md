---
title: hexo博客 Maupassant主题 添加萌妹纸、萌宠
date: 2019-6-30 15:10:58
updated: 2019-6-30 15:11:02
toc: true
categories: 
 - [学习 - hexo]
---



​		给`hexo`博客养一个 萌妹子或者萌宠，也是偶然之间发现可以添加可爱的妹纸或者是萌宠，发现挺好玩的，而且作者提供了很多模型供你选择，没事的时候还可以逗一逗 ?

<!-- more -->

[TOC]



## 本博文的简述or解决问题？

​		给`hexo`博客养一个 萌妹子或者萌宠，也是偶然之间发现可以添加可爱的妹纸或者是萌宠，发现挺好玩的，而且作者提供了很多模型供你选择，没事的时候还可以逗一逗 ?



**编程环境：**  win10 x64 专业版



## 获取：

```bash
npm install --save hexo-helper-live2d
```



## 选择自己喜欢的萌妹子

可以到github中查看，选择喜欢的妹子造型

```bash
live2d-widget-model-chitose
live2d-widget-model-epsilon2_1
live2d-widget-model-gf
live2d-widget-model-haru/01 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)
live2d-widget-model-haruto
live2d-widget-model-hibiki
live2d-widget-model-hijiki
live2d-widget-model-izumi
live2d-widget-model-koharu
live2d-widget-model-miku
live2d-widget-model-ni-j
live2d-widget-model-nico
live2d-widget-model-nietzsche
live2d-widget-model-nipsilon
live2d-widget-model-nito
live2d-widget-model-shizuku
live2d-widget-model-tororo
live2d-widget-model-tsumiki
live2d-widget-model-unitychan
live2d-widget-model-wanko
live2d-widget-model-z16
```

例如选择： live2d-widget-model-miku



## 安装:

```bash
npm install live2d-widget-model-miku
```



## 配置:

在站点的 `_config.yml` 下配置

```bash
live2d:
  enable: true
  scriptFrom: local
  model:
    use: live2d-widget-model-wanko
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true
```

`use` 用来配置模型，目前有很多模型可以选择。 [模型](https://github.com/xiazeyu/live2d-widget-models)

也有对应模型的预览效果。[touwoyimuli.github.io](https://touwoyimuli.github.io/)

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190926232733.png"/>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190926233018.png"/>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190926232757.png"/>

