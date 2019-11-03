---
title: Linux系统下搭建Shadowsocks服务器教程
date: 2019-6-16 02:18:56
updated: 2019-6-16 02:18:56
toc: true
categories: 
 - [学习 - 科学上网vpn]
tags: 
 - vpn
---



​		在Linux服务器上面，搭建Shadowsocks梯子。可以使用Google等查阅文献资料等

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		在Linux服务器上面，搭建Shadowsocks梯子。可以使用Google等查阅文献资料等



Google云新用户注册可以免费领取$300一年期使用权，如果你用的是其他云服务商，教程通用，这里只是以Google云为例，操作系统推荐大家用CentOS6（就没蓝底那个窗口了），也可以使用debian9(推荐）使用debian9 可以只用从第5步开始（当然sudo -i 这一步还是要的）只需2步就可以搭建好。不用装BBR加速器 速度也非常的快 直接破百兆！

Google云地址：[http://cloud.google.com](http://cloud.google.com/)



### Google云创建虚拟机

首先创建好一台VM实例(位置在Google云 Compute Engin-VM实例)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190616022126.png)

创建一个实例，地区一定要选择台湾，为了更快：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190616022147.png)

创建好之后ping测试一下，如果延迟在100ms以内就可以，否则就销毁重建：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190616022716.png)



###  <font color=#CFB5DE  face="gray">开始搭建Shadowsocks</font>

<font color=#FF0000 face="gray">1-4是安装BBR 加速器部分 5-6 是酸酸乳部分</font>

```html
sudo -i(最前面显示root@xxxx)
```

```js
wget -N --no-check-certificate https://raw.githubusercontent.com/FunctionClub/YankeeBBR/master/bbr.sh && bash bbr.sh install蓝底窗口按TAB键选NO
```

选择重启 Y

这里会断开连接，大家可以关掉窗口再重新打开或几秒钟后在界面随便按几个字母 便会提示重新连接。

```html
sudo -i (最前面显示root@xxxx)
```

```html
bash bbr.sh start
```

```js
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh && chmod +x shadowsocksR.sh
```

```html
./shadowsocksR.sh
```

输入shadowsocks 密码

输入端口号

其他一路回车（也可自行选择混淆 协议），大约需要等个十来分钟...

在最后出现红底数据以后，就是Shadowsocks服务端配置信息，在你的Shadowsocks客户端配置上即可。



### <font color=#CFB5DE  face="gray">谷歌云防火墙规则添加 （位置在谷歌云 VPC网络-防火墙）</font>

点击添加新规则，然后按照一下这个设置好。这样 SSR 设置任何端口都可以使用。并且后续不需要再来防火墙规则做设置了。
![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190616022916.png)

其他云服务商的你也得检查一下你的网络防火墙设置。



**转载于：[Linux系统下搭建Shadowsocks服务器教程](https://masuit.com/1303/Linux%E7%B3%BB%E7%BB%9F%E4%B8%8B%E6%90%AD%E5%BB%BAShadowsocks%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%95%99%E7%A8%8B)     懒得勤快**