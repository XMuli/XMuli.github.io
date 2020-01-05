---
title: Upd通信之QUdpSocket的unicast单播、broadcast广播、multicast组播
date: 2020-01-01 00:07:12
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  了解`Upd`通信之`QUdpSocket`的`unicast`单播、`broadcast`广播、`multicast`组播，书写一个简单地例子；然后写了一个小的`Qt`例子，用来实现和验证它的空间的一些属性和功能的用法。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191230_205857_mark.png"  />

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191230_214149_mark.png" />

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [Upd通信之QUdpSocket的unicast单播、broadcast广播、multicast组播](https://blog.csdn.net/qq_33154343/article/details/103789843)

<br>

## 系统环境：

**编程环境：**  `MacOS 10.14.6 (18G103)`   **编程软件：** `Qt 5.9.8`， `Qt Creator 4.8.2`

<br>

## QUdpSocket讲解:

`UDP`通信是轻量的，不可靠（表示有概率会丢包），面向数据报，无连接的协议。用途可以比如：远程视频等

对于UDP通信而言，其实是没有区分客户端或者服务端的，应为任何一个UdpSocket既可以看做客户端，也可以看做服务端；这点与TCP的不一样的。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191230_222351_mark.png"/>



另外就是`QUdpSocket`是以数据报的形式传输数据，而非连续的数据流。发送的数据报一般也都是`QByteArray`

类型的字节数组；数据报的长度一般是低于512字节的，且每一个数据报都是要包含有发送者和接受者的`IP`和`port`等信息。

其中udp的消息传播方式有如下三种：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191230_215944_mark.png"/>

- **unicast单播：**

    一个udp客户端只能够发送数据报到另外一个指定的地址和端口的udp客户端，是一对一的数据传输

- **broadcast广播：**

    一个udp客户端发送的数据报，在同一网络范围内其他所有的udp客户端都可以收到。其支持`IPV4`广播📢，只需要将接收对象设置为`QHostAddress::Broadcast`，且`ip`地址为`255.255.255.255`(代表整个地址段的所有ip)

- **multicast组播：**

    也被称为多播。就是相当于群聊功能。udp客户端加入到 **另一个组播IP地址指定的**多播组，成员向组播地址发送的数据报组内成员都可以接收到，使用`QUdpSocket::joinMuliticastGroup()`函数实现加入多播功能；加入多播后，UDP的发送和正常的UDP数据收发一样。

**而关于组播IP地址，是有着一些约定的：**



综上：若是在家庭或者办公室或局域网中进行udp的测试，可以使用的组播地址是范围是：239.0.0.0~238.255.255.255



**QUdpSocket的主要常用接口：**

| 函数                                                         | 功能                                                        |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| bool bind(quint16 port = 0)                                  | 为udp通信绑定一个端口                                       |
| qint64 writeDatagram(QByteArray kdatagram, QHostAddress &host, quint16 port) | 向目标地址和端口的udp客户端发送数据报，返回成功发送的字节数 |
| bool hasPendingDatagrams()                                   | 至少有一个数据报需要读取的时，返回true                      |
| qint64 pendingDatagramSize()                                 | 返回第一个待读取数据报的大小                                |
| qint64 readDatagram(char *data, qint64 maxSize)              | 读取一个数据报，返回成功读取的数据报的字节数                |
| bool joinMulticastGroup(QHostAddress &groupAddress）         | 加入一个多播组                                              |
| bool leaveMulticastGroup(QHostAddress &groupAddress)         | 离开一个多播组                                              |

<br>

## unicast单播/broadcast广播：

其中单播和组播的图解如下：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191230_221822_mark.png"/>
<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191230_221804_mark.png"/>

<br>

## multicast组播：

其中组播的关系图如下：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191230_221740_mark.png"/>

<br>

## 运行效果：

这里先放一张运行效果图：

**unicast单播/broadcast广播：**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191230_211005.gif"/>



**multicast组播:**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191230_221005.gif"/>

<br>

## 源码分析：

**其中核心部分的源码，重点和一些难点以及需要注意的一些地方，贴出来如下**

### unicast单播/broadcast广播：

其中.h头文件：

```cpp
#ifndef EXTRANS_H
#define EXTRANS_H

#include <QMainWindow>
#include <QLabel>
#include <QUdpSocket>
#include <QString>

namespace Ui {
class ExTrans;
}

/*!
 * \class ExTrans 一个UDP的Deam测试，同时测试单播和广播
 * \brief 因为是在同一台电脑测试，所以IP相同，需要绑定两个不同端口的，这样不会冲突；
 * 若是两台电脑进行测试，那么可以约定使用相同的端口号，使用不同的IP；来进行通讯
 */

class ExTrans : public QMainWindow
{
    Q_OBJECT

public:
    explicit ExTrans(QWidget *parent = nullptr);
    ~ExTrans();

private slots:
    void on_actBind_triggered();     //绑定端口
    void on_actDisbind_triggered();  //解除绑定
    void on_actClean_triggered();    //清除文本信息
    void on_actQuit_triggered();     //关闭程序
    void on_btnUnicast_clicked();    //单播消息
    void on_btnBroadcast_clicked();  //广播消息

    void onSocketStateChange(QAbstractSocket::SocketState socketState);  //socket 状态发生变化
    void onSocketReadyRead();        //读取 socket 传入的数据

private:
    QString getLocalIp();            //获取本机IP

private:
    Ui::ExTrans *ui;

    QLabel* m_labSocketState;
    QUdpSocket* m_udpSocket;
};

#endif // EXTRANS_H

```

其中.cpp源文件：

```cpp
#include "ExTrans.h"
#include "ui_ExTrans.h"

#include <QHostInfo>

ExTrans::ExTrans(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::ExTrans)
{
    ui->setupUi(this);
    setWindowTitle("Udp通信：unicast(单播) + broadcast(广播)的使用");

    QString hostName = QHostInfo::localHostName();
    QString ip = getLocalIp();
    ui->plainTextEdit->appendPlainText("主机名称：" + hostName + "\n主机IP：" + ip + "\n");

    m_labSocketState = new QLabel("Socket状态：");
    m_labSocketState->setMinimumWidth(200);
    ui->statusBar->addWidget(m_labSocketState);

    ui->comboBoxIp->addItem(getLocalIp());
    m_udpSocket = new QUdpSocket(this);

    connect(m_udpSocket, SIGNAL(stateChanged(QAbstractSocket::SocketState)), this, SLOT(onSocketStateChange(QAbstractSocket::SocketState)));
    onSocketStateChange(m_udpSocket->state());
    connect(m_udpSocket, SIGNAL(readyRead()), this, SLOT(onSocketReadyRead()));
}

ExTrans::~ExTrans()
{
    delete ui;
}
//绑定端口
void ExTrans::on_actBind_triggered()
{
    quint16 port = ui->spinBoxBind->value();   //本机UDP端口
    if (m_udpSocket->bind(port)) {   //端口绑定成功
        ui->plainTextEdit->appendPlainText("端口绑定成功：" + QString::number(m_udpSocket->localPort()));

        ui->actBind->setEnabled(false);
        ui->actDisbind->setEnabled(true);
    } else {
        ui->plainTextEdit->appendPlainText("端口绑定失败");
    }
}

//解除绑定
void ExTrans::on_actDisbind_triggered()
{
    m_udpSocket->abort();  //断开，中止套接字
    ui->plainTextEdit->appendPlainText("端口解除绑定成功");

    ui->actBind->setEnabled(true);
    ui->actDisbind->setEnabled(false);
}

//清除文本信息
void ExTrans::on_actClean_triggered()
{
    ui->plainTextEdit->clear();
}

//关闭程序
void ExTrans::on_actQuit_triggered()
{
    close();
}

//单播消息
void ExTrans::on_btnUnicast_clicked()
{
    QString targetIp = ui->comboBoxIp->currentText();
    QHostAddress targetAddr(targetIp);    //目标 Ip
    quint16 targetPort = ui->spinBoxPort->value();   //目标 port
    QString msg = ui->lineEdit->text();   //发送的消息

    QByteArray str = msg.toUtf8();
    m_udpSocket->writeDatagram(str, targetAddr, targetPort);  //发送数据报

    ui->plainTextEdit->appendPlainText(QString("[Send: ] %1").arg(msg));
    ui->lineEdit->clear();
    ui->lineEdit->setFocus();
}

//广播消息
void ExTrans::on_btnBroadcast_clicked()
{
    quint16 targetPort = ui->spinBoxPort->value();   //目标 port
    QString msg = ui->lineEdit->text();   //发送的消息

    QByteArray str = msg.toUtf8();
    m_udpSocket->writeDatagram(str, QHostAddress::Broadcast, targetPort);  //发送 数据报 给所有IP

    ui->plainTextEdit->appendPlainText(QString("[广播: ] %1").arg(msg));
    ui->lineEdit->clear();
    ui->lineEdit->setFocus();
}

//socket 状态发生变化
void ExTrans::onSocketStateChange(QAbstractSocket::SocketState socketState)
{
    switch (socketState) {
    case QAbstractSocket::UnconnectedState:
        m_labSocketState->setText("socket状态：UnconnectedState");
        break;
    case QAbstractSocket::HostLookupState:
        m_labSocketState->setText("socket状态：HostLookupState");
        break;
    case QAbstractSocket::ConnectingState:
        m_labSocketState->setText("socket状态：ConnectingState");
        break;
    case QAbstractSocket::ConnectedState:
        m_labSocketState->setText("socket状态：ConnectedState");
        break;
    case QAbstractSocket::BoundState:
        m_labSocketState->setText("socket状态：BoundState");
        break;
    case QAbstractSocket::ClosingState:
        m_labSocketState->setText("socket状态：ClosingState");
        break;
    case QAbstractSocket::ListeningState:
        m_labSocketState->setText("socket状态：ListeningState");
        break;
    default:
        m_labSocketState->setText("socket状态：其他未知状态...");
        break;
    }
}

//读取 socket 传入的数据
void ExTrans::onSocketReadyRead()
{
    while (m_udpSocket->hasPendingDatagrams()) {
        QByteArray datagram;
        datagram.resize(m_udpSocket->pendingDatagramSize());

        QHostAddress peerAddr;
        quint16 peerPort;

        m_udpSocket->readDatagram(datagram.data(), datagram.size(), &peerAddr, &peerPort);  //读取数据包，消息+来自的Ip和port
        QString str = datagram.data();
        QString peer = QString("[From: %1  %2] %3").arg(peerAddr.toString()).arg(QString::number(peerPort)).arg(str);
        ui->plainTextEdit->appendPlainText(peer);
    }
}

//获取本机IP
QString ExTrans::getLocalIp()
{
    QString hostName = QHostInfo::localHostName();
    QHostInfo hostInfo = QHostInfo::fromName(hostName);
    QString Ip = "";

    if (hostInfo.addresses().isEmpty())
        return 0;

    foreach (QHostAddress addr, hostInfo.addresses()) {
        if (addr.protocol() == QAbstractSocket::IPv4Protocol) {
            Ip = addr.toString();
            break;
        }
    }

   return Ip;
}

```



### multicast组播:

其中.h头文件：

```cpp
#ifndef EXMULTICAST_H
#define EXMULTICAST_H

#include <QMainWindow>
#include <QUdpSocket>
#include <QLabel>
#include <QHostInfo>
#include <QHostAddress>

namespace Ui {
class ExMulticast;
}

class ExMulticast : public QMainWindow
{
    Q_OBJECT

public:
    explicit ExMulticast(QWidget *parent = nullptr);
    ~ExMulticast();

private slots:
    void on_actStart_triggered();
    void on_actStop_triggered();
    void on_actClear_triggered();
    void on_actQuit_triggered();

    void onSocketStateChange(QAbstractSocket::SocketState socketState);  //socket 状态发生变化
    void onSocketReadyRead();        //读取 socket 传入的数据
    void on_btnSend_clicked();

private:
    QString getLocalIp();            //获取本机IP

private:
    Ui::ExMulticast *ui;

    QUdpSocket* m_udpSocket;      //用于通讯的 socket
    QLabel* m_labSocketState;
    QHostAddress m_groupAddress;  //组播地址
};

#endif // EXMULTICAST_H

```



其中.cpp源文件：

```cpp
#include "ExMulticast.h"
#include "ui_ExMulticast.h"

ExMulticast::ExMulticast(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::ExMulticast)
{
    ui->setupUi(this);
    setWindowTitle("Udp通信：multicate(组播)的使用");

    QString hostName = QHostInfo::localHostName();
    QString ip = getLocalIp();
    ui->plainTextEdit->appendPlainText("主机名称：" + hostName + "\n主机IP：" + ip + "\n");

    m_labSocketState = new QLabel("Socket状态：");
    m_labSocketState->setMinimumWidth(200);
    ui->statusBar->addWidget(m_labSocketState);

    ui->comboBoxIp->addItem(getLocalIp());
    m_udpSocket = new QUdpSocket(this);     //用于通讯使用的 Socket
    //Multicast路由层次，1表示只在同一局域网内
    //组播TTL: 生存时间，每跨1个路由会减1，多播无法跨过大多数路由所以为1
    //默认值是1，表示数据包只能在本地的子网中传送。
    m_udpSocket->setSocketOption(QAbstractSocket::MulticastTtlOption, 1);



    connect(m_udpSocket, SIGNAL(stateChanged(QAbstractSocket::SocketState)), this, SLOT(onSocketStateChange(QAbstractSocket::SocketState)));
    onSocketStateChange(m_udpSocket->state());
    connect(m_udpSocket, SIGNAL(readyRead()), this, SLOT(onSocketReadyRead()));
}

ExMulticast::~ExMulticast()
{
    delete ui;
}

void ExMulticast::on_actStart_triggered()
{
    QString ip = ui->comboBoxIp->currentText();
    m_groupAddress = QHostAddress(ip);
    quint16 port = ui->spinBoxBind->value();

    if (m_udpSocket->bind(QHostAddress::AnyIPv4, port, QUdpSocket::ShareAddress)) {  //绑定端口
        m_udpSocket->joinMulticastGroup(m_groupAddress);  //加入多播组

        ui->plainTextEdit->appendPlainText("[用户：" + getLocalIp() +"]  加入组播(组播地址：" + ip + " 端口：" + QString::number(port) + ")成功");
        ui->actStart->setEnabled(false);
        ui->actStop->setEnabled(true);
    } else {
        ui->plainTextEdit->appendPlainText("[用户：" + getLocalIp() +"]  加入组播(组播地址：" + ip + " 端口：" + QString::number(port) + ")失败");
    }
}

void ExMulticast::on_actStop_triggered()
{
    m_udpSocket->leaveMulticastGroup(m_groupAddress);  //退出组播
    m_udpSocket->abort();       //解除绑定
    ui->plainTextEdit->appendPlainText("[用户：" + getLocalIp() +"]  退出组播成功");
    ui->actStart->setEnabled(true);
    ui->actStop->setEnabled(false);
}

void ExMulticast::on_actClear_triggered()
{
    ui->plainTextEdit->clear();
}

void ExMulticast::on_actQuit_triggered()
{
    close();
}

void ExMulticast::onSocketStateChange(QAbstractSocket::SocketState socketState)
{
    switch (socketState) {
    case QAbstractSocket::UnconnectedState:
        m_labSocketState->setText("socket状态：UnconnectedState");
        break;
    case QAbstractSocket::HostLookupState:
        m_labSocketState->setText("socket状态：HostLookupState");
        break;
    case QAbstractSocket::ConnectingState:
        m_labSocketState->setText("socket状态：ConnectingState");
        break;
    case QAbstractSocket::ConnectedState:
        m_labSocketState->setText("socket状态：ConnectedState");
        break;
    case QAbstractSocket::BoundState:
        m_labSocketState->setText("socket状态：BoundState");
        break;
    case QAbstractSocket::ClosingState:
        m_labSocketState->setText("socket状态：ClosingState");
        break;
    case QAbstractSocket::ListeningState:
        m_labSocketState->setText("socket状态：ListeningState");
        break;
    default:
        m_labSocketState->setText("socket状态：其他未知状态...");
        break;
    }
}

void ExMulticast::onSocketReadyRead()
{
    while (m_udpSocket->hasPendingDatagrams()) {
        QByteArray datagram;
        datagram.resize(m_udpSocket->pendingDatagramSize());

        QHostAddress peerAddr;
        quint16 peerPort;

        m_udpSocket->readDatagram(datagram.data(), datagram.size(), &peerAddr, &peerPort);  //读取数据包，消息+来自的Ip和port
        QString str = datagram.data();
        QString peer = QString("[From: %1  %2] %3").arg(peerAddr.toString()).arg(QString::number(peerPort)).arg(str);
        ui->plainTextEdit->appendPlainText(peer);
    }
}

QString ExMulticast::getLocalIp()
{
    QString hostName = QHostInfo::localHostName();
    QHostInfo hostInfo = QHostInfo::fromName(hostName);
    QString Ip = "";

    if (hostInfo.addresses().isEmpty())
        return 0;

    foreach (QHostAddress addr, hostInfo.addresses()) {
        if (addr.protocol() == QAbstractSocket::IPv4Protocol) {
            Ip = addr.toString();
            break;
        }
    }

   return Ip;
}

void ExMulticast::on_btnSend_clicked()
{
    quint16 port = ui->spinBoxBind->value();
    QString  msg = ui->lineEdit->text();
    QByteArray  datagram = msg.toUtf8();

    m_udpSocket->writeDatagram(datagram, m_groupAddress, port);
//    m_udpSocket->writeDatagram(datagram.data(), datagram.size(), m_groupAddress, port);
    ui->plainTextEdit->appendPlainText("[multicst] " + msg);
    ui->lineEdit->clear();
    ui->lineEdit->setFocus();
}

```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples) 【QtUdpEx】