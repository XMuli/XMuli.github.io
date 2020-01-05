---
title: TCP通信之QTcpServer和QTcpSocket，服务器和客户端通讯
date: 2019-12-30 00:20:45
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  了解`TCP`通信之`QTcpServer`和`QTcpSocket`，服务器和客户端通讯，书写一个简单地例子；然后写了一个小的 **Qt**例子，用来实现和验证它的空间的一些属性和功能的用法。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191225_232206_mark.png"/>

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [TCP通信之QTcpServer和QTcpSocket，服务器和客户端通讯](https://blog.csdn.net/qq_33154343/article/details/103759735)

<br>

## 相关博文：

**有着一些关联的博文其他参考：**

- 

<br>

## 系统环境：

**编程环境：**  `MacOS 10.14.6 (18G103)`   **编程软件：** `Qt 5.9.8`， `Qt Creator 4.8.2`

<br>

## Tcp通信概述：

我想能够看到网络这一部分的人，基本都是有着基本的网络认知；这里为了这一个系列的完整性，还是决定将本系列的基本知识仍旧写出来。万一之前的你是没有怎么接触过这类篇文章的鸭鸭呢？就算是完全不知，但是一些基础的还是能够让你能够有所了解，再具体的就进行自行goole和专业📚进行扩展深入扩展学习，博文尽量都提一笔，其余更多待你自行发掘。



**TCP特点：**

可靠，面向流传播，面向协议的一种传输协议；特别适合用于连续数据传输。实际🌰：QQ的在线发送文件，就是为了确保文件发送的完整性，使用的TCP协议。

<br>

## QTcpServer属性：

TCP通信，必须先进行TCP连接，通信端口分为服务器端口和客户端。

`QTcpServer`是从`QObject`继承的类，主要是用于服务器建立网络监听，创立`Socket`连接的；其中主要的一些常用接口，提供·如下：

| 类型     | 函数                                                         | 功能                                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 公共函数 | void close()                                                 | 关闭服务器，停止网络监听                                     |
|          | bool listen()                                                | 在给定IP地址和端口上开始监听，若是成功，就返回true           |
|          | bool isListening()                                           | 返回true表示服务器处于监听状态                               |
|          | QTcpSocket * nextPendingConnection()                         | 返回下一个等待接入的连接                                     |
|          | QHostAddres serverAddress()                                  | 如果服务器处于监听状态，返回服务器地址                       |
|          | quint16 serverPort()                                         | 如果服务器处于监听状态，返回服务器端口                       |
|          | bool waitForNewConnection()                                  | 以阻塞方式等待新的连接                                       |
|          |                                                              |                                                              |
| 信号     | void acceptError( QAbstractSocket::SocketError socketError ) | 当接受一个新的连接时大声了错误，就发射此信号；参数socketError描述错误信息 |
|          | void new Connection()                                        | 当有新的信号连接时候，发射此信号                             |
|          |                                                              |                                                              |
| 保护函数 | void incomingConnection(qintptr socketDescriptor)            | 当有一个新的连接可用时，QTcpServer内部调用此函数，创建一个QTcpSocket对象，添加到内部可用新连接列表，然后发射newConnetion()信号，用户若是从QTcpServer继承定义类，可以重定义此函数，但是必须调用QtcpSocket继承定义类，可以重新定义此函数，但是必须调用addPendingConnetion() |
|          | void addPendingConnection(QTcp Socket *socket)               | 由incomingtion()调用，将创建的QTcpSocket添加到内部新可用连接列表 |



服务器的程序首先是需要调用QTcpServer进行监听的【这个时候，是指定监听的IP和port的】，然后当QScoket连接的时候，会发射信号📶，`newConnection()`，在`newConnection()`的对应好的槽函数中，可以用nextPendingConnetion()接收客户端的连接，然后使用QSocket与客户端进行通信。





## QTcp/QUdp继承关系图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191229_181530_mark.png"/>

<br>

## QAbstractSocket接口讲解：

**其中主要的`QAbstractSocket`的主要的接口函数如下：**

| 类型     | 函数                                                        | 功能                                                         |
| -------- | ----------------------------------------------------------- | ------------------------------------------------------------ |
| 公共函数 | void connectToHost(QHostAddress headdress, quint16 port,)   | 以异步的方式连接到指定的IP地址和端口的TCP服务器，连接成功会发射connected()信号 |
|          | void disconnectFromHost()                                   | 断开sokcet，关闭成功后发射disconnection()信号                |
|          | bool waitForConnected()                                     | 等待直到建立socket连接                                       |
|          | bool waitForDisconnected()                                  | 等待直到断开socket连接                                       |
|          | QHostAddress localAddress()                                 | 返回本socket的地址                                           |
|          | quint16 localPort()                                         | 返回本socket的端口                                           |
|          | QHostAddress peerAddress()                                  | 在已经连接状态下，返回对方socket的地址                       |
|          | QString peerName()                                          | 返回connetToHost()连接到对方的主机的名                       |
|          | quint16 peerPort()                                          | 在已连接的状态下，返回对方的socket的port                     |
|          | qint64 readBuffer Size()                                    | 返回内部读取缓冲区的大小的数据的字节数；该大小决定了read()和readAll()函数能够读取出来的数据的大小 |
|          | void setReadBuAerSize(qint64 size)                          | 数据内部读取缓冲区的数据的字节数                             |
|          | qint64 bytesAvailable()                                     | 返回需要读取的缓冲区的字节数字                               |
|          | bool canReadLine()                                          | 如果有行数据要从socket缓冲区读取，就返回true                 |
|          | SocketState state()                                         | 返回当前socket状态                                           |
|          |                                                             |                                                              |
| 信号📶    | void connected()                                            | connectionToHost()成功连接到服务器后发射此信号               |
|          | void disconnected()                                         | 当socket断开连接时，发射此信号                               |
|          | void error(QAbstractSocket::SocketError socketError)        | 当socket发生错误时，发射此信号                               |
|          | void hostFound()                                            | 调用connectToHost()找到主机后发射此信号                      |
|          | void stateChanged(QAbstractSocket::SocketState socketState) | 当socket的状态变化时候发射此信号，参数socketState表示了socket当前的状态 |
|          | void ready Read()                                           | 当缓冲区的数据需要读取时候，发射此信号，在此信号的槽函数里面读取缓冲区的数据 |

<br>

## 运行效果：

这里先放一张运行效果图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191216_232611.gif"/>

<br>

## 源码分析：

这里将TCP通信的服务器和客户端拆分为两个小的项目来写，其结构如下：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191225_232719_mark.png" style="zoom:67%;" />

其中核心部分的源码，重点和一些难点以及需要注意的一些地方，贴出来如下：

### 其中服务器端：

```cpp
/*
 * Copyright (C)  2019 ~ 2019 touwoyimuli.  All rights reserved.
 *
 * Author:  touwoyimuli <touwoyimuli@gmai.com>
 *
 * github:  https://github.com/touwoyimuli
 * blogs:   https://touwoyimuli.github.io/
 *
 * This program is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program.  If not, see <https://touwoyimuli.github.io/>.
 */
#ifndef EXTCPSERVER_H
#define EXTCPSERVER_H

#include <QMainWindow>
#include <QLabel>
#include <QTcpServer>
#include <QTcpSocket>
#include <QHostInfo>

namespace Ui {
class ExTcpServer;
}

class ExTcpServer : public QMainWindow
{
    Q_OBJECT

public:
    explicit ExTcpServer(QWidget *parent = nullptr);
    ~ExTcpServer();

private:
    QString getLocalIp();      //获取本机 IP

protected:
    void closeEvent(QCloseEvent* event);

private slots:
//UI的槽函数
    void on_actStart_triggered();      //开始监听
    void on_actStop_triggered();       //停止监听
    void on_actClear_triggered();      //清除文本框内容
    void on_actQuit_triggered();       //退出程序
    void on_btnSend_clicked();         //发送消息

//自定义的槽函数
    void onSocketReadyRead();          //读取 socket 传入时候的数据
    void onClientConnected();          //client socket conneted
    void onClientDisonnected();        //client socket disconneted
    void onNewConnection();            //QTcpServer 的 newConnect() 信号
    void onSocketStateChange(QAbstractSocket::SocketState socketState);

private:
    Ui::ExTcpServer *ui;

    QLabel* m_labListen;
    QLabel* m_labSocket;
    QTcpServer* m_tcpServer;
    QTcpSocket* m_tcpSocket;


};

#endif // EXTCPSERVER_H

```



```cpp
#include "ExTcpServer.h"
#include "ui_ExTcpServer.h"

ExTcpServer::ExTcpServer(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::ExTcpServer)
{
    ui->setupUi(this);

    m_labListen = new QLabel("监听状态：");
    m_labSocket = new QLabel("socket状态：");
    m_labListen->setMidLineWidth(200);
    m_labSocket->setMinimumWidth(200);
    ui->statusBar->addWidget(m_labListen);
    ui->statusBar->addWidget(m_labSocket);

    QString localeIp = getLocalIp();
    setWindowTitle(windowTitle() + "---IP地址：" + localeIp);
    ui->comboBox->addItem(localeIp);

    m_tcpServer = new QTcpServer(this);
    connect(m_tcpServer, SIGNAL(newConnection()), this, SLOT(onNewConnection()));
}

ExTcpServer::~ExTcpServer()
{
    delete ui;
}

QString ExTcpServer::getLocalIp()
{
    QString hostName = QHostInfo::localHostName();
    QHostInfo hostInfo = QHostInfo::fromName(hostName);
    ui->plainTextEdit->appendPlainText("本机名称：" + hostName);
    QString locaIp;

    QList<QHostAddress> list = hostInfo.addresses();

    if (list.empty())
        return "null QString";

    foreach (QHostAddress addr, list) {
        if (addr.protocol() == QAbstractSocket::IPv4Protocol) {
            locaIp = addr.toString();
            break;
        }
    }

    return locaIp;
}

void ExTcpServer::closeEvent(QCloseEvent *event)   //关闭窗口时候停止监听
{
    if (m_tcpServer->isListening())
        m_tcpServer->close();

    event->accept();
}

void ExTcpServer::on_actStart_triggered()
{
    QString Ip = ui->comboBox->currentText();
    quint16 port = ui->spinBox->value();

    QHostAddress addr(Ip);
    m_tcpServer->listen(addr, port);  //监听指定的 IP 和指定的 port
    ui->plainTextEdit->appendPlainText("服务器地址为:" + m_tcpServer->serverAddress().toString() + "   服务器端口：" + QString::number(m_tcpServer->serverPort()));
    ui->plainTextEdit->appendPlainText("开始监听...");

    ui->actStart->setEnabled(false);
    ui->actStop->setEnabled(true);
    m_labListen->setText("监听状态：正在监听...");
}

void ExTcpServer::on_actStop_triggered()
{
    if (!m_tcpServer->isListening())
        return;

    m_tcpServer->close();  //停止监听

    ui->actStart->setEnabled(true);
    ui->actStop->setEnabled(false);
    m_labListen->setText("监听状态：监听已经停止");
}

void ExTcpServer::on_actClear_triggered()
{
    ui->plainTextEdit->clear();
}

void ExTcpServer::on_actQuit_triggered()
{
    close();
}

void ExTcpServer::on_btnSend_clicked()
{
    QString msg = ui->lineEdit->text();
    ui->plainTextEdit->appendPlainText("[服务器:]" + msg);
    ui->lineEdit->clear();
    ui->plainTextEdit->hasFocus();

    QByteArray str = msg.toUtf8();
    str.append('\n');
    m_tcpSocket->write(str);
}

void ExTcpServer::onSocketReadyRead()     //读取缓冲区行文本
{
    while (m_tcpSocket->canReadLine()) {
        ui->plainTextEdit->appendPlainText("[客户端:]" + m_tcpSocket->readLine());
    }
}

void ExTcpServer::onClientConnected()    //客户端连接时
{
    ui->plainTextEdit->appendPlainText("客户端套接字连接\n对等(peer)地址：" + m_tcpSocket->peerAddress().toString()
                                       + "    对等(peer)端口：" +  QString::number(m_tcpSocket->peerPort()));

}

void ExTcpServer::onClientDisonnected()  //客户端断开连接时
{
    ui->plainTextEdit->appendPlainText("客户端套接字断开");
    m_tcpSocket->deleteLater();
}

void ExTcpServer::onNewConnection()
{
    m_tcpSocket = m_tcpServer->nextPendingConnection();   //创建 socket

    connect(m_tcpSocket, SIGNAL(connected()), this, SLOT(onClientConnected()));
    connect(m_tcpSocket, SIGNAL(disconnected()), this, SLOT(onClientDisonnected()));
    connect(m_tcpSocket, SIGNAL(stateChanged(QAbstractSocket::SocketState)), this, SLOT(onSocketStateChange(QAbstractSocket::SocketState)));
    onSocketStateChange(m_tcpSocket->state());
    connect(m_tcpSocket, SIGNAL(readyRead()), this, SLOT(onSocketReadyRead()));
}

void ExTcpServer::onSocketStateChange(QAbstractSocket::SocketState socketState)
{
    switch (socketState) {
    case QAbstractSocket::UnconnectedState:
        m_labSocket->setText("socket状态：UnconnectedState");
        break;
    case QAbstractSocket::HostLookupState:
        m_labSocket->setText("socket状态：HostLookupState");
        break;
    case QAbstractSocket::ConnectingState:
        m_labSocket->setText("socket状态：ConnectingState");
        break;
    case QAbstractSocket::ConnectedState:
        m_labSocket->setText("socket状态：ConnectedState");
        break;
    case QAbstractSocket::BoundState:
        m_labSocket->setText("socket状态：BoundState");
        break;
    case QAbstractSocket::ClosingState:
        m_labSocket->setText("socket状态：ClosingState");
        break;
    case QAbstractSocket::ListeningState:
        m_labSocket->setText("socket状态：ListeningState");
        break;
    default:
        m_labSocket->setText("socket状态：其他未知状态...");
        break;
    }
}

```



### 其中客户端：

```cpp
#ifndef EXTCPCLIENT_H
#define EXTCPCLIENT_H

#include <QMainWindow>
#include <QLabel>
#include <QTcpSocket>
#include <QHostInfo>

namespace Ui {
class ExTcpClient;
}

class ExTcpClient : public QMainWindow
{
    Q_OBJECT

public:
    explicit ExTcpClient(QWidget *parent = nullptr);
    ~ExTcpClient();

private:
    QString getLocalIp();                  //获取本本机 IP

protected:
    void closeEvent(QCloseEvent *event);

private slots:
    //UI 定义的槽函数
    void on_actConnect_triggered();     //请求连接到服务器
    void on_actDisconnect_triggered();  //断开与服务器的连接
    void on_actClear_triggered();       //清除内容
    void on_actQuit_triggered();        //退出程序
    void on_btnSend_clicked();          //发送文本消息

    //自定义的槽函数
    void onConnected();
    void onDisconnected();
    void onSocketReadyRead();           //从socket读取传入的数据
    void onSocketStateChange(QAbstractSocket::SocketState socketState);

private:
    Ui::ExTcpClient *ui;

    QLabel* m_labSocket;
    QTcpSocket* m_tcpSocket;
};

#endif // EXTCPCLIENT_H

```



```cpp
#include "ExTcpClient.h"
#include "ui_ExTcpClient.h"

ExTcpClient::ExTcpClient(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::ExTcpClient)
{
    ui->setupUi(this);

    m_labSocket = new QLabel("socket状态：");
    m_labSocket->setMidLineWidth(150);
    ui->statusBar->addWidget(m_labSocket);

    m_tcpSocket = new QTcpSocket(this);

    QString localIp = getLocalIp();
    this->setWindowTitle(windowTitle() + "----本机IP:" + localIp);
    ui->comboBox->addItem(localIp);

    connect(m_tcpSocket, SIGNAL(connected()), this, SLOT(onConnected()));
    connect(m_tcpSocket, SIGNAL(disconnected()), this, SLOT(onDisconnected()));
    connect(m_tcpSocket, SIGNAL(stateChanged(QAbstractSocket::SocketState)), this, SLOT(onSocketStateChange(QAbstractSocket::SocketState)));
    connect(m_tcpSocket, SIGNAL(readyRead()), this, SLOT(onSocketReadyRead()));
}

ExTcpClient::~ExTcpClient()
{
    delete ui;
}

QString ExTcpClient::getLocalIp()
{
    QString hostName = QHostInfo::localHostName();
    QHostInfo hostInfo = QHostInfo::fromName(hostName);
    ui->plainTextEdit->appendPlainText("本机名称：" + hostName);
    QString localIp;

    foreach (QHostAddress addr, hostInfo.addresses()) {
        if (QAbstractSocket::IPv4Protocol == addr.protocol()) {
            localIp = addr.toString();
            break;
        }
    }
    return localIp;
}

void ExTcpClient::closeEvent(QCloseEvent *event)
{
    if (m_tcpSocket->state() == QAbstractSocket::ConnectedState)
        m_tcpSocket->disconnectFromHost();

    event->accept();
}

void ExTcpClient::onConnected()
{
    ui->plainTextEdit->appendPlainText("已经连接到服务器\n客户端套接字连接\n对等(peer)地址：" + m_tcpSocket->peerAddress().toString()
                                       + "    对等(peer)端口：" +  QString::number(m_tcpSocket->peerPort()));
    ui->actConnect->setEnabled(false);
    ui->actDisconnect->setEnabled(true);
}

void ExTcpClient::onDisconnected()
{
    ui->plainTextEdit->appendPlainText("已经断开与服务器的连接\n");
    ui->actConnect->setEnabled(true);
    ui->actDisconnect->setEnabled(false);
}

void ExTcpClient::onSocketReadyRead()
{
    while (m_tcpSocket->canReadLine()) {
        ui->plainTextEdit->appendPlainText("[服务器:]" + m_tcpSocket->readLine());
    }
}

void ExTcpClient::onSocketStateChange(QAbstractSocket::SocketState socketState)
{
    switch (socketState) {
    case QAbstractSocket::UnconnectedState:
        m_labSocket->setText("socket状态：UnconnectedState");
        break;
    case QAbstractSocket::HostLookupState:
        m_labSocket->setText("socket状态：HostLookupState");
        break;
    case QAbstractSocket::ConnectingState:
        m_labSocket->setText("socket状态：ConnectingState");
        break;
    case QAbstractSocket::ConnectedState:
        m_labSocket->setText("socket状态：ConnectedState");
        break;
    case QAbstractSocket::BoundState:
        m_labSocket->setText("socket状态：BoundState");
        break;
    case QAbstractSocket::ClosingState:
        m_labSocket->setText("socket状态：ClosingState");
        break;
    case QAbstractSocket::ListeningState:
        m_labSocket->setText("socket状态：ListeningState");
        break;
    default:
        m_labSocket->setText("socket状态：其他未知状态...");
        break;
    }
}

void ExTcpClient::on_actConnect_triggered()
{
    QString addr = ui->comboBox->currentText();
    quint16 port = ui->spinBox->value();
    m_tcpSocket->connectToHost(addr, port);
}

void ExTcpClient::on_actDisconnect_triggered()
{
    if(m_tcpSocket->state() == QAbstractSocket::ConnectedState)
        m_tcpSocket->disconnectFromHost();
}

void ExTcpClient::on_actClear_triggered()
{
    ui->plainTextEdit->clear();
}

void ExTcpClient::on_actQuit_triggered()
{
    close();
}

void ExTcpClient::on_btnSend_clicked()
{
    QString msg = ui->lineEdit->text();
    ui->plainTextEdit->appendPlainText("[客户端:]" + msg);
    ui->lineEdit->clear();
    ui->lineEdit->setFocus();

    QByteArray str = msg.toUtf8();
    str.append('\n');
    m_tcpSocket->write(str);
}

```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples) 【QtTcpEx】