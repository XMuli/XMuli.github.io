---
title: 多文档界面MDI(Multi-document Interface)的实现，QMdiArea使用
date: 2019-12-20 00:01:20
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  了解 **多文档界面MDI(Multi-document Interface)**的实现，`QMdiArea`使用，书写一个简单地例子；然后写了一个小的**Qt**例子，用来实现和验证它的空间的一些属性和功能的用法。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_20191218_220755_mark.png"/>

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [多文档界面MDI(Multi-document Interface)的实现，QMdiArea使用](https://blog.csdn.net/qq_33154343/article/details/103625380)

<br>

## 系统环境：

**编程环境：**  `MacOS 10.14.6 (18G103)`   **编程软件：** `Qt 5.9.8`， `Qt Creator 4.8.2`

<br>

## MDI(Multi-document Interface)控件：

`MDI(Multi-document Interface)`是传统的多文档界面应用程序，而Qt为此提供了支持；他们可以共享一个主窗口，以及菜单栏，工具栏等。而 **设计一个MDI的应用程序就是需要在主窗口的工作去放一个QMdiArea控件作为子窗口的容器。**



<br>

## 运行效果：

这里放下一张运行的图片

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/2019-12-18%2022.19.10.gif"/>

<br>

## 源码分析：

两个头文件的类如下：

```cpp
lass ExMDI : public QWidget
{
    Q_OBJECT

public:
    explicit ExMDI(QWidget *parent = nullptr);
    ~ExMDI();

    void loadFromFile(QString& fileName);  //打开文件
    QString currentFileName();             //返回当前文件名
    bool isFileOpended();                  //文件已经打开

    void setEditFont();
    void textCut();
    void textCopy();
    void textPaste();


private:
    Ui::ExMDI *ui;

    QString m_currentFile;  //当前文件
    bool    m_fileOpened;   //true 打开；false 未打开
};


lass ExMainWindow : public QMainWindow
{
    Q_OBJECT

public:
    explicit ExMainWindow(QWidget *parent = nullptr);
    ~ExMainWindow();

private slots:
    void on_actOpen_triggered();   //打开文档
    void on_actQuit_triggered();   //退出程序
    void on_actNew_triggered();    //新建MDI的子窗口
    void on_actFont_triggered();   //设置字体
    void on_actCut_triggered();    //剪切文本
    void on_actCopy_triggered();   //复制文本
    void on_actPaste_triggered();  //粘贴文本
    void on_actView_triggered(bool checked);  //MDI显示： Table 和 子窗口页面显示
    void on_actCascade_triggered();//级联显示
    void on_actTile_triggered();   //平铺显示
    void on_actClose_triggered();  //关闭所有子窗口

private:
    void closeEvent(QCloseEvent *e);   //主窗口关闭时关闭所有子窗口

private:
    Ui::ExMainWindow *ui;
};
```



其对应的源文件：

```cpp
#include "ExMDI.h"
#include "ui_ExMDI.h"

ExMDI::ExMDI(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::ExMDI)
{
    ui->setupUi(this);

    setWindowTitle("多文档界面MDI (Multi-dociment Interface) 的介绍和使用");
    setAttribute(Qt::WA_DeleteOnClose);  //无论是否设置关闭时候删除；在MDI中关闭一个MDI子窗口都会删除子窗口对象
    this->setWindowIcon(QIcon("/Users/yuanyi/picture/发布、头像、测试图片/icon.png"));
}

ExMDI::~ExMDI()
{
    delete ui;
}

//打开文件
void ExMDI::loadFromFile(QString &fileName)
{
    QFile file(fileName);  //文件以读的方式读出

    if (file.open(QIODevice::ReadOnly | QIODevice::Text)) {
        QTextStream stream(&file);  //以文本流方式读取文件
        ui->plainTextEdit->clear();
        ui->plainTextEdit->setPlainText(stream.readAll());
        file.close();

        m_currentFile = fileName;
        QFileInfo fileInfo(fileName);    //文件信息
        setWindowTitle(fileInfo.fileName());
        m_fileOpened = true;
    }
}

QString ExMDI::currentFileName()
{
    return m_currentFile;
}

bool ExMDI::isFileOpended()
{
    return m_fileOpened;
}

void ExMDI::setEditFont()
{
    QFont font = ui->plainTextEdit->font();
    bool ok;
    font = QFontDialog::getFont(&ok, font);
    ui->plainTextEdit->setFont(font);

}

void ExMDI::textCut()
{
    ui->plainTextEdit->cut();
}

void ExMDI::textCopy()
{
    ui->plainTextEdit->copy();
}

void ExMDI::textPaste()
{
    ui->plainTextEdit->paste();
}

```





```cpp
#include "ExMainWindow.h"
#include "ui_ExMainWindow.h"

ExMainWindow::ExMainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::ExMainWindow)
{
    ui->setupUi(this);
    setCentralWidget(ui->mdiArea);
    setWindowState(Qt::WindowMaximized);
    ui->toolBar->setToolButtonStyle(Qt::ToolButtonTextUnderIcon);
}

ExMainWindow::~ExMainWindow()
{
    delete ui;
}

void ExMainWindow::closeEvent(QCloseEvent *e)
{
    ui->mdiArea->closeAllSubWindows();  //关闭所有子窗口
    e->accept();
}

void ExMainWindow::on_actOpen_triggered()
{
    bool needNew = false;

    ExMDI *mdi = nullptr;

    if (ui->mdiArea->subWindowList().count() > 0) {  //如果有打开的主窗口，获取活动窗口
        mdi =  (ExMDI *)ui->mdiArea->activeSubWindow()->widget();
        needNew = mdi->isFileOpended();   //文件已经打开，需要新建窗口
    } else {
        needNew = true;
    }

    QString curPath = QDir::currentPath();
    QString fileName = QFileDialog::getOpenFileName(this, "打开一个文件", curPath, "C程序文件(*.h *cpp);;文本文件(*.txt);;所有文件(*.*)");

    if (fileName.isEmpty())
        return;

    if (needNew) {
        mdi = new ExMDI(this);
        ui->mdiArea->addSubWindow(mdi);
    }

    mdi->loadFromFile(fileName);
    mdi->show();

    ui->actCopy->setEnabled(true);
    ui->actCut->setEnabled(true);
    ui->actPaste->setEnabled(true);
    ui->actFont->setEnabled(true);
}

void ExMainWindow::on_actQuit_triggered()
{
    close();
}

void ExMainWindow::on_actNew_triggered()
{
    ExMDI *mdi = new ExMDI(this);
    ui->mdiArea->addSubWindow(mdi);   //添加一个子窗口MDI
    mdi->show();

    ui->actCopy->setEnabled(true);
    ui->actCut->setEnabled(true);
    ui->actPaste->setEnabled(true);
    ui->actFont->setEnabled(true);
}

void ExMainWindow::on_actFont_triggered()
{
     ExMDI* mdi = (ExMDI *)ui->mdiArea->activeSubWindow()->widget();
     mdi->setEditFont();  //设置编写的字体文档
}

void ExMainWindow::on_actCut_triggered()
{
    ExMDI* mdi = (ExMDI *)ui->mdiArea->activeSubWindow()->widget();
    mdi->textCut();
}

void ExMainWindow::on_actCopy_triggered()
{
    ExMDI* mdi = (ExMDI *)ui->mdiArea->activeSubWindow()->widget();
    mdi->textCopy();
}

void ExMainWindow::on_actPaste_triggered()
{
    ExMDI* mdi = (ExMDI *)ui->mdiArea->activeSubWindow()->widget();
    mdi->textPaste();
}

void ExMainWindow::on_actView_triggered(bool checked)   //MDI 模式设置
{
    if (checked) {  //Tab多页显示模式
        ui->mdiArea->setViewMode(QMdiArea::TabbedView);
        ui->mdiArea->setTabsClosable(true);  //页面可以关闭
        ui->actCascade->setEnabled(false);
        ui->actTile->setEnabled(false);
    } else {
        ui->mdiArea->setViewMode(QMdiArea::SubWindowView);  //子窗口模式
        ui->actCascade->setEnabled(true);
        ui->actTile->setEnabled(true);
    }

}

void ExMainWindow::on_actCascade_triggered()
{
    ui->mdiArea->cascadeSubWindows();   //窗口级联展开
}

void ExMainWindow::on_actTile_triggered()
{
    ui->mdiArea->tileSubWindows();  //平铺展开
}

void ExMainWindow::on_actClose_triggered()
{
    ui->mdiArea->closeAllSubWindows();  //关闭全部子窗口
}

```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples) 【QtMDIEx】