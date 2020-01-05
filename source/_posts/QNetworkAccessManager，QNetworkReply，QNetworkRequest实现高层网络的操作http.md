---
title: QNetworkAccessManager，QNetworkReply，QNetworkRequest实现高层网络的操作http
date: 2020-01-02 21:31:18
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  了解`QNetworkAccessManager`/`QNetworkReply`/`QNetworkRequest`实现高层网络的操作`http`，书写一个简单地例子；然后写了一个小的 **Qt**例子，用来实现和验证它的空间的一些属性和功能的用法。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191230_223624_mark.png"/>

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [QNetworkAccessManager，QNetworkReply，QNetworkRequest实现高层网络的操作http](https://blog.csdn.net/qq_33154343/article/details/103811638)

<br>

## 系统环境：

**编程环境：**  `MacOS 10.14.6 (18G103)`   **编程软件：** `Qt 5.9.8`， `Qt Creator 4.8.2`

<br>

## http请求以及应答：

将上面的三个类进行一个关系图的梳理，可以得到如下如图，看到网络上面都是一些基本介绍不全，连一个图都没有，理解起来会比较抽象，所以这里画上一个图帮助大家理解他们三者之间的关系：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191230_225736_mark.png"/>



### QNetworkRequest：

`QNetworkRequest`通过一个URL地址发送网络请求协议请求，也保存网络请求的信息，目前是支持HTTP，FTP，和局部的URLs的下载和上传；

<br>

### QNetworkAccessManager：

`QNetworkAccessManager`类用于协调网络操作，在`QNetworkRequest`发送一个网络请求之后，`QNetworkAccessManager`类负责发送网络请求，创建网络响应。

<br>

### QNetworkReply：

`QNetworkReply`类表示网络请求的响应。由`QNetworkAccessManager`在发送一个网络请求后创建一个网络响应；`QNetworkReply`提供信号finish(), readyRead(), downloadProgress()可以监测网络执行的情况，执行响应的操作。其`QNetworkReply`也是`QIODevice`的子类，所以`QNetworkReply`支持流读写功能，也支持异步或者同步的工作模式。

<br>

## 运行效果：

这里先放一张运行效果图：

此例子下载的是qtcretor的校验文件.txt；其中若是将下载链接替换为QtCreator的下载·连接（本是想下载exe、dmg文件的）；但是却会发现下载不是预料中，而是另外一个文件，指向另外的一个真实地址的下载文件；但是浏览器可以识别们直接跳转下载之后的地址，但若是这个程序想要直接下载从定向的文件的真实地址的文件，就需要再次做处理。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191230_224505.gif"/>

<br>

## 源码分析：

其中核心部分的源码，重点和一些难点以及需要注意的一些地方，贴出来如下：

其中.h头文件源码：

```cpp
#ifndef EXHTTP_H
#define EXHTTP_H

#include <QMainWindow>
#include <QNetworkAccessManager>
#include <QNetworkReply>
#include <QFile>
#include <QUrl>
#include <QDir>

namespace Ui {
class ExHttp;
}

class ExHttp : public QMainWindow
{
    Q_OBJECT

public:
    explicit ExHttp(QWidget *parent = nullptr);
    ~ExHttp();

private slots:
    void on_btnDown_clicked();  //下载文件
    void on_btnFile_clicked();  //默认的保存路径
    void on_lineEditUrl_textChanged(const QString &arg1);

    void onFinished();          //网络响应结束
    void onReadyRead();         //读取下载的数据
    void onDownloadProgress(qint64 bytesRea, qint64 totalBytes);  //下载进程

private:
    Ui::ExHttp *ui;

    QNetworkAccessManager* m_networkManager;   //网络管理
    QNetworkReply* m_reply;                    //网络响应
    QFile* m_file;                             //下载保存的临时文件
};

#endif // EXHTTP_H

```



其中.cpp源文件源码：

```cpp
#include "ExHttp.h"
#include "ui_ExHttp.h"
#include <QMessageBox>
#include <QDir>
#include <QDesktopServices>

ExHttp::ExHttp(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::ExHttp)
{
    ui->setupUi(this);
    setWindowTitle("QNetworkAccessManager 网络管理使用 Http 协议下载");

    m_networkManager = new QNetworkAccessManager(this);
    m_reply = nullptr;
    m_file = nullptr;
}

ExHttp::~ExHttp()
{
    delete ui;
}

//下载文件
void ExHttp::on_btnDown_clicked()
{
    QString urlSpec = ui->lineEditUrl->text().trimmed();  //去掉字符串的首尾的空格
    if (urlSpec.isEmpty()) {
        QMessageBox::information(this, "提示", "下载地址URL为NULL");
        return;
    }

    QUrl url = QUrl::fromUserInput(urlSpec);
    if (!url.isValid()) {
        QMessageBox::information(this, "提示", QString("无效URL: %1 \n 错误信息: %2").arg(urlSpec, url.errorString()));
        return;
    }

    QString dir = ui->lineEditFile->text().trimmed();
    if (dir.isEmpty()) {
        QMessageBox::information(this, "提示", "保存地址为空");
        return;
    }

    QString fileFileName = dir + url.fileName(); //文件保存地址 + 文件名
    if (QFile::exists(fileFileName))
        QFile::remove(fileFileName);

    m_file = new QFile(fileFileName);    //创建临时文件
    if (!m_file->open(QIODevice::WriteOnly)) {
        QMessageBox::information(this, "提示", "打开临时文件错误");
        return;
    }

    ui->btnDown->setEnabled(false);

    m_reply = m_networkManager->get(QNetworkRequest(url));  //发送get网络请求，创建网络响应
    connect(m_reply, SIGNAL(finished()), this, SLOT(onFinished()));
    connect(m_reply, SIGNAL(readyRead()), this, SLOT(onReadyRead()));
    connect(m_reply, SIGNAL(downloadProgress(qint64,qint64)), this, SLOT(onDownloadProgress(qint64,qint64)));
}

//默认的保存路径
void ExHttp::on_btnFile_clicked()
{
    QString currPath = QDir::currentPath();
    QDir dir(currPath);
    dir.mkdir("temp");

    ui->lineEditFile->setText(currPath + "/temp/");
}

//网络响应结束
void ExHttp::onFinished()
{
    QFileInfo fileInfo;
    fileInfo.setFile(m_file->fileName());

    m_file->close();
    delete m_file;
    m_file = nullptr;

    m_reply->deleteLater();
    m_reply = nullptr;

    if (ui->checkBox->isChecked())   //勾选了，下载完成之后，打开下载的文件               //absoluteFilePath() 返回包含文件名的绝对路径。
        QDesktopServices::openUrl(QUrl::fromLocalFile(fileInfo.absoluteFilePath()));  //使用默认软件的打开下载的文件

    ui->btnDown->setEnabled(true);
}

//读取下载的数据
void ExHttp::onReadyRead()
{
    m_file->write(m_reply->readAll());   //将返回的数据进行读取，写入到临时文件中
}

//下载进程
void ExHttp::onDownloadProgress(qint64 bytesRea, qint64 totalBytes)
{
    ui->progressBar->setMaximum(totalBytes);
    ui->progressBar->setValue(bytesRea);
}

void ExHttp::on_lineEditUrl_textChanged(const QString &arg1)
{
    ui->progressBar->setMaximum(100);
    ui->progressBar->setValue(0);
}

```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples) 【QtHttpEx】