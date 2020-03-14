---
title: 主机信息查询QHostInfo和QNetworkInterface查询IP等
date: 2019-12-25 21:52:42
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  了解主机信息查询`QHostInfo`和`QNetworkInterface`查询IP等函数接口的使用，书写一个简单地例子；然后写了一个小的 **Qt**例子，用来实现和验证它的空间的一些属性和功能的用法。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191225_214953_mark.png"/>

<!-- more -->

[TOC]

## 系统环境：

**编程环境：**  `MacOS 10.14.6 (18G103)`   **编程软件：** `Qt 5.9.8`， `Qt Creator 4.8.2`

<br>

## 网络相关知识：

终于讲解到了我之前就心心恋恋的网络模块的知识了，有几个点一直是很想涉及的知识模块👏👏👏；一个是网络，另外一个就是多线程的相关知识；柑橘🍊现在很多电脑💻的应用程序都会使用到这两个部分的知识模块，再➕一个数据库📚就是==App？  成为一个独立的开发者，可以写一个自己喜欢的exe或者app，慢慢打磨；人生易短，做自己喜欢的事情，💰不￥$$💰的都是无所谓的。谁都有离开的一天，die是不可避免的，可我们如何证明自己来这这里玩过几十载呢？总的留下一些什么的痕迹吧~



**或许朝闻道，夕死可矣~       ???**



来自一个`12-25`🎄🎄🎄🎄：夜间没人约会的+上班的孤独狗 ➜ 🐶👨 的一篇博客📝



有点跑偏了：

网络模块的相关知识，主要就是七层协议原理 和它们衍生出来的网络协议，有N多种，其中经过时间的检验和筛选，现在常用的就是`TCP/IP`协议族，展开就是TCP，UDP，HTTP，HTTPS，ftp，socket等等



### 网络相关：

建议开始之前，先问问自己如下问题，自己能够区分概念是什么，自己懂了吗❓❓❓

- mac地址
- IP地址
- port端口
- 主机名
- 子网掩码，A/B/C/D四类地址
- 数据包？ 报文？自定义协议？
- 通信协议有哪些？
- 三次握手🤝，四次挥手👋？
- TCP/UDP/Http区别
- 数据包经过路由器如何转到下一台设备？
- 一款程序是怎么在局域网之间通信的？
- 一款程序是怎么在互联网之间通信的？



关于网络更底层的协议和实现的原理的相关学习知识，可以多看看👀下面这两个视频链接，我就是之前学习的此mooc网络课，觉得讲解的很棒，故此口口相传的推荐出来：

- **华南理工大学 计算机网络 MOOC** 

    [https://www.bilibili.com/video/av40766904](https://www.bilibili.com/video/av40766904)

- **州电子科技大学 计算机网络自学笔记 MOOC  **[https://www.bilibili.com/video/av40761275](https://www.bilibili.com/video/av40761275) 

<br>

## QHostInfo属性：

得益于`Qt`强大的封装库，上面的很多细节都不用深究，只需要创建一两个对象，然后调用他们的函数，就可以获得他们的网络相关的信息；



`QHostInfo`类可以通过静态函数localHostName()获取 **本机的主机名** 再通过fromName()函数可以获取到 **IP地址**，而lookupHost()则是通过异步方式查询到这个主机的IP地址。



|   类别   | 函数原型                                                     | 作用                                                         |
| :------: | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 公共函数 | QList<QHostAddress> addresses()                              | 返回与hostName()关联的IP地址列表                             |
| 公共函数 | HostInfoError error()                                        | 如果主机查找失败，返回失败类型                               |
| 公共函数 | QString errorString()                                        | 如果主机查找失败，返回错误描述字符串                         |
| 公共函数 | QString hostName()                                           | 返回通过IP查找的主机名称                                     |
| 公共函数 | int lookupld()                                               | 返回本次查找的id                                             |
| 静态函数 | void abortHostLookup(int id)                                 | 中断主机查找                                                 |
| 静态函数 | QHostInfo fromName(QString &name)                            | 返回指定主机名IP的地址                                       |
| 静态函数 | QString localDomainName()                                    | 返回本机DNS域名                                              |
| 静态函数 | QString localHostName()                                      | 返回本机主机名                                               |
| 静态函数 | int lookupHost(QString byname, QObject *receiver, char *member) | 以异步方式根据主机名查找主机的IP地址，并返回一个表示本次查找的ID，可用于abortHostLookup() |



```cpp
//QHostInfo 获取主机信息
    QString hostName = QHostInfo::localHostName();
    ui->plainTextEdit->appendPlainText("本地主机名称:" + hostName + "\n");

    QHostInfo hostInfo = QHostInfo::fromName(hostName);
    QList<QHostAddress> list = hostInfo.addresses();

    if (list.isEmpty())
        return;

    foreach (QHostAddress var, list) {
        bool bIPv4 = ui->checkBox->isChecked();

        if (bIPv4) {   //只显示 IPv4
            bIPv4 = QAbstractSocket::IPv4Protocol == var.protocol();
        } else {
            bIPv4 = true;   //显示 IPv4 和 IPv6
        }

        if (bIPv4) {
            ui->plainTextEdit->appendPlainText("协议：" + protocolName(var.protocol()));
            ui->plainTextEdit->appendPlainText("本机IP地址" + var.toString() + "\n");
        }
    }
```

<br>

## QNetworkInterface属性：

`QNetworkInterface`类是可以获得应用程序的主机的所有IP地址和网络地址接口的列表。静态函数allInterfaces()返回主机上所有网络接口的列表，一个网络接口可能包含多个IP地址，每个IP地址与地址掩码或广播地址的关联；当然也有一个简版的获取函数allAddresses()可以获取到，但是不会返回子网掩码和广播的IP地址。



| 类别     | 函数原型                                      | 作用                                                         |
| -------- | --------------------------------------------- | ------------------------------------------------------------ |
| 公共函数 | QList<QNetworkAddress Entry> addressEntries() | 返回该网络接口（包含子网掩码+广播地址）的IP地址列表          |
| 公共函数 | QString hardwareAddress()                     | 返回该接口的低级硬件地址，以太网里就是MAC地址                |
| 公共函数 | QString humanReadableName()                   | 返回可以读懂的接口名称没如果名称不确定，得到的就是name()的返回值 |
| 公共函数 | bool isValid()                                | 如果接口信息有效就返回true                                   |
| 公共函数 | QString name()                                | 返回主机上所有IP地址的列表                                   |
| 静态函数 | QList<QHostAddress> allAddresses()            | 返回主机上面的所有IP地址的列表                               |
| 静态函数 | QList<QNetworklnterface> allInterfaces()      | 返回主机上面的所有接口的网络列表                             |

<br>

## QAbstractSocket属性：

**QAbstractSocket::NetworkLayerProtocol 枚举**:

该枚举描述了Qt中使用的网络层协议值。

| Constant                                     | Value | Description              |
| -------------------------------------------- | ----- | ------------------------ |
| QAbstractSocket::IPv4Protocol                | 0     | IPv4                     |
| QAbstractSocket::IPv6Protocol                | 1     | IPv6                     |
| QAbstractSocket::AnyIPProtocol               | 2     | Either IPv4 or IPv6      |
| QAbstractSocket::UnknownNetworkLayerProtocol | -1    | Other than IPv4 and IPv6 |

<br>

## 运行效果：

这里上一张运行效果图：

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191216_201205_GIF.gif"/>

<br>

## 源码分析：

其中核心部分的源码，重点和一些难点以及需要注意的一些地方，贴出来如下

**其中.h头文件如下：**

```cpp
#ifndef EXQHOSTINFO_H
#define EXQHOSTINFO_H

#include <QWidget>
#include <QAbstractSocket>
class QHostInfo;


namespace Ui {
class ExQHostInfo;
}

class ExQHostInfo : public QWidget
{
    Q_OBJECT

public:
    explicit ExQHostInfo(QWidget *parent = nullptr);
    ~ExQHostInfo();

private:
    QString protocolName(QAbstractSocket::NetworkLayerProtocol protocol);  //通过协议类型返回协议名称

private slots:
    void on_btnGetHostInfo_clicked();    //QHostInfo查询主机名和IP
    void on_btnAllAddresses_clicked();   //QNetworkInterface::allAddresses()
    void on_btnAllInterfaces_clicked();  //QNetworkInterface::allInterfaces()
    void on_btnFindIP_clicked();         //QHostInfo查询左侧域名IP地址
    void on_btnClean_clicked();          //清空文本框信息

    void onLookedUpHostInfo(const QHostInfo& host);  //查询主机信息的槽函数

private:
    Ui::ExQHostInfo *ui;

};

#endif // EXQHOSTINFO_H
```



**其中.cpp源文件如下：**

```cpp
#include "ExQHostInfo.h"
#include "ui_ExQHostInfo.h"

#include <QHostInfo>
#include <QNetworkInterface>

ExQHostInfo::ExQHostInfo(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::ExQHostInfo)
{
    ui->setupUi(this);

    setWindowTitle("QHostInfo/QNetworkInterface查询主机网络状态：");
}

ExQHostInfo::~ExQHostInfo()
{
    delete ui;
}

//通过协议类型返回协议名称
QString ExQHostInfo::protocolName(QAbstractSocket::NetworkLayerProtocol protocol)
{
    switch (protocol) {
    case QAbstractSocket::IPv4Protocol:
        return "IPv4 Protocol";
    case QAbstractSocket::IPv6Protocol:
        return "IPv6 Protocol";
    case QAbstractSocket::AnyIPProtocol:
        return "Any IP Protocol";
    default:
        return "Unknow Network Layer Protocol";
    }
}

//QHostInfo 获取主机信息
void ExQHostInfo::on_btnGetHostInfo_clicked()
{
    QString hostName = QHostInfo::localHostName();
    ui->plainTextEdit->appendPlainText("本地主机名称:" + hostName + "\n");

    QHostInfo hostInfo = QHostInfo::fromName(hostName);
    QList<QHostAddress> list = hostInfo.addresses();

    if (list.isEmpty())
        return;

    foreach (QHostAddress var, list) {
        bool bIPv4 = ui->checkBox->isChecked();

        if (bIPv4) {   //只显示 IPv4
            bIPv4 = QAbstractSocket::IPv4Protocol == var.protocol();
        } else {
            bIPv4 = true;   //显示 IPv4 和 IPv6
        }

        if (bIPv4) {
            ui->plainTextEdit->appendPlainText("协议：" + protocolName(var.protocol()));
            ui->plainTextEdit->appendPlainText("本机IP地址" + var.toString() + "\n");
        }
    }
}

void ExQHostInfo::on_btnAllAddresses_clicked()
{
    QList<QHostAddress> list = QNetworkInterface::allAddresses();

    if (list.isEmpty())
        return;

    foreach (QHostAddress var, list) {
        bool bIPv4 = ui->checkBox->isChecked();

        if (bIPv4) {   //只显示 IPv4
            bIPv4 = QAbstractSocket::IPv4Protocol == var.protocol();
        } else {
            bIPv4 = true;   //显示 IPv4 和 IPv6
        }

        if (bIPv4) {
            ui->plainTextEdit->appendPlainText("协议：" + protocolName(var.protocol()));
            ui->plainTextEdit->appendPlainText("本机IP地址" + var.toString() + "\n");
        }
    }
}

void ExQHostInfo::on_btnAllInterfaces_clicked()
{
    QList<QNetworkInterface> list = QNetworkInterface::allInterfaces();

    if (list.isEmpty())
        return;

    foreach (QNetworkInterface var, list) {
        if (!var.isValid())
            continue;

        ui->plainTextEdit->appendPlainText("设备名称：" + var.humanReadableName());
        ui->plainTextEdit->appendPlainText("硬件地址：" + var.hardwareAddress());

        QList<QNetworkAddressEntry> entry = var.addressEntries();
        foreach (QNetworkAddressEntry ent, entry) {
            ui->plainTextEdit->appendPlainText("  IP 地址：" + ent.ip().toString());
            ui->plainTextEdit->appendPlainText("  子网掩码：" + ent.netmask().toString());
            ui->plainTextEdit->appendPlainText("  子网广播：" + ent.broadcast().toString() + "\n");
        }
    }
}

void ExQHostInfo::on_btnFindIP_clicked()
{
    QString hostName = ui->lineEdit->text();  //域名
    ui->plainTextEdit->appendPlainText("正在查找域名的服务器的主机信息：" + hostName);
    QHostInfo::lookupHost(hostName, this, SLOT(onLookedUpHostInfo(QHostInfo)));
}

void ExQHostInfo::on_btnClean_clicked()
{
    ui->plainTextEdit->clear();
}

//查询主机信息的槽函数
void ExQHostInfo::onLookedUpHostInfo(const QHostInfo &host)
{
    QList<QHostAddress> list = host.addresses();

    if (list.isEmpty())
        return;

    for (int i = 0; i < list.count(); i++) {
        QHostAddress host = list.at(i);
        bool bIpv4 = ui->checkBox->isChecked();  //只显示IPv4

        if (bIpv4) {   //只显示 IPv4
            bIpv4 = QAbstractSocket::IPv4Protocol == host.protocol();
        } else {
            bIpv4 = true;   //显示 IPv4 和 IPv6
        }

        if (bIpv4) {
            ui->plainTextEdit->appendPlainText("协议：" + protocolName(host.protocol()));
            ui->plainTextEdit->appendPlainText(host.toString());
        }
    }
}

```



<br>

## 源码下载：

[https://github.com/xmuli/QtExamples](https://github.com/xmuli/QtExamples) 【QtQHostInfoEx】
