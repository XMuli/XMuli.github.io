---
title: QFileSystemModel数据模型获取本机文件系统的使用
date: 2019-12-14 23:53:18
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  `QFileSystemModel`是可**用于访问本机系统的文件系统**的**数据模型**；其的属性和功能的使用介绍；然后写了一个例子，用来实现和验证它的功能的一些属性和功能的用法。这里主要是数据的读取部分是使用到了`QFileSystemModel`类，然后分别使用`QTreeView`和`QListView`和`ColumnView`和`QTableView`这四种视图控件来显示。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191208_233142_5.jpg"/>

<!-- more -->

[TOC]

**相关博客：**  [Model-View-Delegate:"模型-视图-代理"的讲解](https://blog.csdn.net/qq_33154343/article/details/103501667)

<br>

## 系统环境：

**编程环境：**  `win10 x64 专业版 1803`   **编程软件：**  `Qt 5.9.8，Qt Creator 4.8.2 (Enterprise)`

**编程软件：**  `Qt 5.9.8`，`Qt Creator 4.8.2 (Enterprise)`

<br>

## QFileSystemModel属性：

 `QFileSystemModel`是可**用于访问本机系统的文件系统**的**数据模型**；一开始是需要使用设置一个根目录的；

```cpp
QString currPath = QDir::currentPath();  //获取当前路径
m_model->setRootPath(currPath);          //设置根目录
```

和`QFileSystemModel`一样，可以获取磁盘文件目录的数据模型的还有`QDirModel`，但是`QFileSystemModel`是使用单独的线程来获取目录的文件的结构的，而`QDirModel`不是采用的单独的线程

<br>

## 运行效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191208_233142.gif"/>

<br>

## 源码分析：

这个例子，主要是根据**”Mode-View-Delegate（模型-视图-代理）”** 来实现，我会在前面将其仔细的写出来；这个例子得益于Qt自带的这些模型的强大，看起来需要写好多代码，但是实际只需要的代码量不算多，所以这里我就是将实现的部分源码，直接放在下面：

**.h**头文件的实现：

```cpp
#ifndef EXQFILESYSTEMMODEL_H
#define EXQFILESYSTEMMODEL_H

#include <QMainWindow>
#include <QLabel>
#include <QCheckBox>
#include <QFileSystemModel>

namespace Ui {
class ExQFileSystemModel;
}

class ExQFileSystemModel : public QMainWindow
{
    Q_OBJECT

public:
    explicit ExQFileSystemModel(QWidget *parent = nullptr);
    ~ExQFileSystemModel();

    void init();                //初始化，以及初始化状态栏

private slots:
    void on_treeView_clicked(const QModelIndex &index);  //单击treeView，会在状态栏显示当前节点的信息

private:
    Ui::ExQFileSystemModel *ui;

    QLabel* m_labFileName;       //文件名
    QLabel* m_labFileSize;       //文件大小
    QLabel* m_labFileType;       //文件类型
    QLabel* m_labPath;           //路径
    QCheckBox* m_chkBoxIsFile;   //当前是否为文件或文件夹
    QFileSystemModel* m_model;   //设置文件系统的模型
};

#endif // EXQFILESYSTEMMODEL_H

```

.cpp文件的实现：

```cpp
#include "ExQFileSystemModel.h"
#include "ui_ExQFileSystemModel.h"

ExQFileSystemModel::ExQFileSystemModel(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::ExQFileSystemModel)
{
    ui->setupUi(this);
    setWindowTitle(QObject::tr("文件系统模型QFileSystemModel的介绍和使用"));

    init();
    connect(ui->treeView, &QTreeView::clicked, ui->listView, &QListView::setRootIndex);
    connect(ui->treeView, &QTreeView::clicked, ui->tableView, &QTableView::setRootIndex);
}

ExQFileSystemModel::~ExQFileSystemModel()
{
    delete ui;
}

//初始化，以及初始化状态栏
void ExQFileSystemModel::init()
{
    //设置数据模型，且加载到各个视图上面
    m_model = new QFileSystemModel(this);
    QString currPath = QDir::currentPath();  //获取当前路径
    m_model->setRootPath(currPath);          //设置根目录
    ui->treeView->setModel(m_model);         //设置数据模型
    ui->listView->setModel(m_model);
    ui->tableView->setModel(m_model);
    ui->columnView->setModel(m_model);

    //初始化状态栏
    m_labFileName = new QLabel("名称：", ui->statusBar);
    m_labFileName->setMinimumWidth(180);
    m_labFileSize = new QLabel("大小：", ui->statusBar);
    m_labFileSize->setFixedWidth(130);
    m_labFileType = new QLabel("类型：", ui->statusBar);
    m_labFileType->setFixedWidth(130);
    m_labPath = new QLabel("路径：" + currPath, ui->statusBar);
    m_chkBoxIsFile = new QCheckBox("当前为文件夹", ui->statusBar);
    m_chkBoxIsFile->setFixedWidth(130);

    //各种QLable添加到状态栏
    ui->statusBar->addWidget(m_labFileName);
    ui->statusBar->addWidget(m_labFileSize);
    ui->statusBar->addWidget(m_labFileType);
    ui->statusBar->addWidget(m_chkBoxIsFile);
    ui->statusBar->addWidget(m_labPath);
}

//单击treeView，会在状态栏显示当前节点的信息
void ExQFileSystemModel::on_treeView_clicked(const QModelIndex &index)
{
    m_chkBoxIsFile->setChecked(m_model->isDir(index));                //是否是目录
    m_labFileName->setText("名称：" + m_model->fileName(index));      //文件名称
    double size = m_model->size(index) / 1024.0;

    if (size < 1024)
        m_labFileSize->setText("类型：" + QString::number(size, 'f', 2) + "KB");
    else if (1024 <= size && size < 1024 * 1024)
        m_labFileSize->setText("类型：" + QString::number(size / 1024, 'f', 2) + "MB");
    else
        m_labFileSize->setText("类型：" + QString::number(size / (1024 * 1024), 'f', 2) + "GB");

    m_labFileType->setText(m_model->type(index));
    m_labPath->setText(m_model->filePath(index));
}
```

<br>

## 源码下载：

[https://github.com/xmuli/QtExamples](https://github.com/xmuli/QtExamples)【QtQFileSystemModelEx】