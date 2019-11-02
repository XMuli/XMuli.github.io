---
title:  Linux下搭建v2ray服务器飞机场教程，畅享YouTube 4k体验
date: 2019-6-16 01:58:27
updated: 2019-6-16 01:58:32
toc: true
categories: 
 - [学习 - 科学上网vpn]
tags: 
 - vpn
 - 科学上网
---



​		在Linux服务器上面，搭建v2ray梯子。可以畅享看YouTu 4K视频无压力

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		在Linux服务器上面，搭建v2ray梯子。可以畅享看YouTu 4K视频无压力



好道是规则是用来打破的，王道是用来颠覆的！之前给大家分享过搭建Shadowsocks服务器教程，为了将来的保险起见，这次给大家带来v2ray服务器的搭建教程。

首先，同样你得拥有一台不限带宽的云主机，搬瓦工、vultr、谷歌云、Azure之类的不限带宽服务商，所以，新用户还是推荐你用外币信用卡去申请一年期免费的GoogleCloud，申请地址：[http://cloud.google.com](http://cloud.google.com/)。搭建过程也挺简单的，两步就好。



### 前言

v2ray的优势：v2ray支持的传输方式有很多，包括：普通TCP、HTTP伪装、WebSocket流量、普通mKCP、mKCP伪装FaceTime通话、mKCP伪装BT下载流量、mKCP伪装微信视频流量，不同的传输方式其效果会不同，有可能会遇到意想不到的效果哦！当然国内不同的地区、不同的网络环境，效果也会不同，所以具体可以自己进行测试。现在v2ray客户端也很多了，有windows、MAC、linux和安卓版。

推荐使用centos7服务器，博主之前用debian系统似乎没有搭建成功。

如果想搭建ss/ssr，可以参考 [自建ss/ssr服务器教程](https://masuit.com/s/Shadowsocks)

注意：搭建ss/ssr脚本和搭建v2ray脚本不要在同一台vps上使用，以免互相干扰！如果ss/ssr和v2ray都想搭建，可以用两台vps，一台搭建ss/ssr，另外一台搭建v2ray。

好了，教程正式开始。

### 搭建步骤

**第一步：**

云服务控制台创建一台虚拟主机，配置不需要太高，够用就好。

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190616020553.png)



地区推荐香港或台湾，创建好之后建议先测试一下延迟。

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190616020650.png)

温馨提醒：同样的服务器位置，不同的宽带类型和地区所搭建的账号的翻墙速度会不同，这与中国电信、中国联通、中国移动国际出口带宽和线路不同有关，所以以实测为准。可以先选定一个服务器位置来按照教程进行搭建，熟悉搭建方法，当账号搭建完成并进行了bbr加速后，测试下速度自己是否满意，如果满意那就用这个服务器位置的服务器。如果速度不太满意，就一次性开几台不同的服务器位置的服务器，然后按照同样的方法来进行搭建并测试，选择最优的，之后把其它的服务器删掉，按小时计费测试成本可以忽略。



**第二步：**

开始搭建v2ray，你可以用xshell或者云服务控制台自带的shell窗口进行远程连接。

连接成功后，会出现如图所示，之后就可以复制粘贴代码部署了。

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190616020717.png)

脚本开源地址：<https://github.com/Jrohy/multi-v2ray>

安装脚本命令：

```html
sudo -i
source <(curl -sL https://git.io/fNgqx)
```

卸载脚本命令：

```html
sudo -i
source <(curl -sL https://git.io/fNgqx) --remove
```

复制上面的代码到VPS服务器里，接着按回车键，脚本会自动安装，

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190616024205.png)

以后只需要运行这个快捷命令就可以出现下图的界面进行设置，快捷管理命令为：v2ray

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190616020821.png)

安装好以后，程序生成客户端代码（<font color=green size=5 face="黑体">**绿色部分**</font>），复制下来。



**第三步**：

你可以自定义配置v2ray服务端，选择3更改v2ray配置。

你可以自定义v2ray的端口、加密方式、传输方式、广告屏蔽等，传输方式推荐TCP方式。

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190616020908.png)



### 简单解释一下ssr与VPN的区别：

VPN的开发目的是给企业内网直接传输加密数据，最重要的就是安全性。

VPN目前就科学上网方面来讲，PPTP大部分地区已死，L2TP大部分地区已经出现干扰和断开连接情况，Openvpn一封一个准。而anyconnect大多数都是企业用的，所以墙不敢乱封，IKEv1/IKEv2需要注意证书中间人攻击问题。
所以，在VPN科学上网这方面，一些地区已经根据VPN的流量特征做出了相应的匹配策略，可以有效封杀VPN了。
Shadowsocks的开发目的就是穿透防火墙，最重要的是增加墙的匹配流量效率封杀成本和难度，也就是混淆隐秘性。
Shadowsocks是更注重流量混淆隐秘，VPN则是更注重加密安全性。如果你需要安全你可能需要 VPN 或者 Shadowsocks+TOR匿名 ，否则就抗干扰能力来说Shadowsocks更适合拿来科学上网，VPN中的Opnevpn是最安全的VPN协议之一，然而第一个被墙宣布效率检测、封杀！

### 客户端下载

Windows客户端下载地址：<https://github.com/2dust/v2rayN/releases>

其他平台客户端：[https://www.v2ray.com](https://www.v2ray.com/)



**转载于：   [Linux下搭建v2ray服务器飞机场教程，畅享YouTube 4k体验](https://masuit.com/1409)**