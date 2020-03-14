---
title: hexo+github+coding开启双线程搭建博客，提高访问速度
date: 2020-03-12 15:34:13
toc: true
categories: 
 - [学习 - hexo]
---



[TOC]

　　**简  述：**　使用 `hexo` + `githubPage` + `codingPage` 开启双线程；使用同一个域名 `xmuli.tech` 访问，效果为：

- 国内用户 IP 访问国内 coding 服务器
- 国外用户 IP 访问国外 github 服务器

使得访问博客速度哒哒提高。本博客已经搭建成功。本篇作为记录，作为日后备查。

<br>

### 背景铺垫：

- **域名：** 付费极低
  - **域名解析：** 免费
- **github：** 免费
- **coding：** 免费
- **hexo：** 免费

<br>

### hexo + githubPage 搭建：

- [Hexo](https://hexo.io/zh-cn/)

- [屠城](https://www.haomwei.com/) 

- [个人博客的Hexo搭建，主题maupassant](https://xmuli.tech/2019/06/09/%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E7%9A%84Hexo%E6%90%AD%E5%BB%BA%EF%BC%8C%E4%B8%BB%E9%A2%98maupassant/) 

<br>

### hexo + codingPage 搭建：

- [coding 如何搭建静态网站？](https://help.coding.net/docs/devops/cd/static-website.html#%E6%90%AD%E5%BB%BA%E9%9D%99%E6%80%81%E7%BD%91%E7%AB%99)

- [Pages 迁移至新版 CODING](https://support.qq.com/products/104149/faqs/61222) 

<br>

### 设置域名解析：

- [刚买的域名怎么绑定自己博客？再白嫖一年的SSL，使用https访问博客](https://blog.csdn.net/qq_33154343/article/details/104727225) 
- [Hexo 双线部署到 Coding Pages 和 GitHub Pages 并实现全站 HPPTS](https://blog.csdn.net/qq_36759224/article/details/100879609) 