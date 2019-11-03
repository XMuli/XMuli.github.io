---
title: hexo框架Maupassant主题，将valine匿名评论改迁为disqus评论系统
date: 2019-7-1 22:42:21
updated: 2019-7-1 22:42:24
toc: true
categories: 
 - [学习 - hexo]
tags: 
 - disqus
---



`hexo`博客框架`Maupassant`主题，设置评论区，采用的评论区为：`disqus` 和 `valine` 评论系统，且分析两者的利弊与使用性质。

<!-- more -->

[TOC]



## 本博文的简述or解决问题？

`hexo`博客框架`Maupassant`主题，设置评论区，采用的评论区为：`disqus` 和 `valine` 评论系统，且分析两者的利弊与使用性质。



**编程环境：**  win10 x64 专业版



## 两者比较：

`valine`评论可以免登录，直接匿名留言，感觉爽歪歪，是我之所爱。**可是、可是、可是、**，因为本人的`小破站`想被百度等收录，就只有认证，然后这就有点慌慌的。还是改成登录的模式，不过就是的成本比较高，需要自备梯子vpn，才能够留言，**可是、可是、可是、**，如果是技术人员，我觉得有些东东，还是需要自己摸一遍的，这些都是要自己学会的东西。且避免一些，考虑再三，还是采用`disqus`评论系统吧（**这算是真的匿名评论吧！**）。



## 背景：

本人采用的是 `hexo`框架`Maupassant`主题，好就好在他自带集成了很多的评论系统，

> 可以参考的全文：[屠城 大道至简——Hexo简洁主题推荐](https://www.haomwei.com/technology/maupassant-hexo.html)

```yaml
disqus: ## Your disqus_shortname, e.g. username
uyan: ## Your uyan_id. e.g. 1234567
livere: ## Your livere data-uid, e.g. MTAyMC8zMDAxOC78NTgz
changyan: ## Your changyan appid, e.g. cyrALsXc8
changyan_conf: ## Your changyan conf, e.g. prod_d8a508c2825ab57eeb43e7c69bba0e8b
gitment:
  enable: false ## If you want to use Gitment comment system please set the value to true.
  owner: ## Your GitHub ID, e.g. username
  repo: ## The repository to store your comments, make sure you're the repo's owner, e.g. imsun.github.io
  client_id: ## GitHub client ID, e.g. 75752dafe7907a897619
  client_secret: ## GitHub client secret, e.g. ec2fb9054972c891289640354993b662f4cccc50
```



- disqus - [Disqus评论](https://disqus.com/) shortnam   **参考**：[Hexo搭建博客系列：（六）Hexo添加Disqus评论](https://www.jianshu.com/p/d68de067ea74)

- uyan - [友言评论](http://www.uyan.cc/) id

- livere - [来必力评论](https://livere.com/) data-uid

- changyan - [畅言评论](http://changyan.kuaizhan.com/) appid

- gitment - [Gitment评论](https://github.com/imsun/gitment)相关参数

- gitalk - [Gitalk评论](https://github.com/gitalk/gitalk)相关参数

- valine - [Valine评论](https://valine.js.org/)相关参数

    

<font color=#D0087E size=4 face="幼圆">**以下设置均是在`hexo/Maupassant`下面的`_config.yml`下面设置的**</font>



## valine评论设置：

```yaml
valine: ## https://valine.js.org
  enable: true ## If you want to use Valine comment system, please set the value to true.
  appid: 手动打码，自行替换成自己的 ## Your LeanCloud application App ID, e.g. pRBBL2JR4N7kLEGojrF0MsSs-gzGzoHsz
  appkey: 手动打码，自行替换成自己的 ## Your LeanCloud application App Key, e.g. tjczHpDfhjYDSYddzymYK1JJ
  notify: false ## Mail notifier, see https://github.com/xCss/Valine/wiki/Valine-评论系统中的邮件提醒设置
  verify: false ## Validation code.
  placeholder: 我就只是想看看，字都懒得打... ## Comment box placeholders.
  avatar: 'mm' ## Gravatar type, see https://github.com/xCss/Valine/wiki/avatar-setting-for-valine
  pageSize: 10 ## Number of comments per page.
  guest_info: nick,mail,link ## Attributes of reviewers.
```



## valine评论效果：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190701222228.png)









## disqus评论设置：

```yaml
disqus: touwoyimulier ## Your disqus_shortname, e.g. username
```



## disqus评论效果：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190701223406.png)



## 本次心得：

评论系统要慎重，所有人都可以匿名评论的，感觉可能活不长久；还是设置一些简单的门槛，来过滤一些无意义的话语（可以看做一种高逼格的）



**本博文同步到csdn博客：** [hexo框架Maupassant主题，将valine匿名评论改迁为disqus评论系统](https://blog.csdn.net/qq_33154343/article/details/94412888)



**参考博文：** [Hexo搭建博客系列：（六）Hexo添加Disqus评论](https://www.jianshu.com/p/d68de067ea74)







