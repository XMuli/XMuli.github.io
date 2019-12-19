---
title: 目录树组件QTreeWidget和停靠区域组件QDockWidget的用法
date: 2019-12-10 00:13:34
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---

**简  述：**  目录树组件`QTreeWidget`和停靠区域组件`QDockWidget`的和`QScrollArea`用法；然后写的一个小的相册查看器例子

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-12-08_18-38-19_mark.png"/>

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [目录树组件QTreeWidget和停靠区域组件QDockWidget的用法](https://blog.csdn.net/qq_33154343/article/details/103467757)

<br>

## QTreeWidget组件：

`QTreeWidget`是 **目录树组件** 控件，用来创建和管理树形结构的类的

<br>

## QDockWidget组件：

`QDockWidget`是可以在`QMainWindow`窗口停靠，或者在桌面最上层浮动的界面组件；在本文里面的例子里面，将`QTreeWidget`控件存放在`QDockWidget`上面，设置其可以在主窗口的左或右侧，也是可以浮动的，还可以浮动；还可以设置其为隐藏/显示不显示窗体的效果



### **allowedAreas**属性，设置允许停靠区域

使用setAllowedAreas()设置允许停靠区域（上下左右的区域）

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-12-08_21-58-56_mark.png"/>



### **feat**属性，设置停靠区组件的特性

由`setFeatures()`函数来设置停靠区域组件的特性，其参数是如下的枚举类型的组合值的使用

| Constant                                | Description                  |
| --------------------------------------- | ---------------------------- |
| QDockWidget::DockWidgetClosable         | 停靠区域可关闭               |
| QDockWidget::DockWidgetMovable          | 停靠区域可移动               |
| QDockWidget::DockWidgetFloatable        | 停靠区域可浮动               |
| QDockWidget::DockWidgetVerticalTitleBar | 在停靠区域左侧显示垂直标题栏 |
| QDockWidget::AllDockWidgetFeatures      | 使用以上所有特征             |
| QDockWidget::NoDockWidgetFeatures       | 不能停靠，移动和关闭         |

<br>

## QScrollArea组件：

`QScrollArea`是一个**自动滚区域组件**的控件，可以用来实现有比较长的显示内容。

<br>

## 运行效果：

这种`gif`录制了一下午（N多次，就是没有特别满意，要么太大要么不清晰），但是`gif`好像录制的并不算太好，录制这张图，但是总是体积太大或者不清晰，在`mac`上面暂时没有找到比较好的录制和压缩**gif**的软件来代替`win10`上面的(**ScreenGif**  + **FSREsizer** + **FSCapture**)这三个软件，有合适的话，可以知道的可以留言推荐一下，合适和录gif工具也在尝试中；

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/2019-12-08-17.27.31.gif"/>

<br>

## 实现代码：

**编程环境：**  `MacOS 10.14.6 (18G103)`   **编程软件：** `Qt 5.9.8`， `Qt Creator 4.8.2`;

在**.h**头文件里面，实现非部分代码：

```cpp
#ifndef EXQTREEWIDGET_H
#define EXQTREEWIDGET_H

#include <QMainWindow>
#include <QLabel>
#include <QTreeWidgetItem>
#include <QFileDialog>

namespace Ui {
class ExQTreeWidget;
}

class ExQTreeWidget : public QMainWindow
{
    Q_OBJECT

public:

    enum treeItemType {         //枚举，节点类型
        itemRoot,
        itemFile,
        itemImage
    };

    enum treeColNum {           //目录树列表的编号
        colItem = 0,
        colItemType = 1
    };

    explicit ExQTreeWidget(QWidget *parent = nullptr);
    ~ExQTreeWidget();

    void initTree();                                                //初始化根节点（唯一）
    void addFolderItem(QTreeWidgetItem *parItem, QString dirName);  //添加目录
    void addImageItem(QTreeWidgetItem *parItem, QString fileName);  //添加图片文件
    QString getFinalFolderName(const QString &pathName);            //从完整的路径里面，获取最后的文件夹名称
    void changeItemCaption(QTreeWidgetItem* parItem);               //遍历item下面的所有节点
    void displayImage(QTreeWidgetItem* item);                       //显示当前item的图片（默认以适配高度）

private slots:
    void on_actAddFolder_triggered();                               //增加文件夹
    void on_actAddFile_triggered();                                 //添加图片文件
    void on_actDeleFile_triggered();                                //删除节点
    void on_actScanItems_triggered();                               //遍历所有的顶层节点(本处只有一个root顶层节点)
    void on_actAdaptiveHeight_triggered();                          //图片自动适应高度
    void on_actAdaptiveWidth_triggered();                           //图片自动适应宽度
    void on_actAmplification_triggered();                           //放大
    void on_actShrink_triggered();                                  //缩小
    void on_actZoomRealSize_triggered();                            //还原
    void on_actDockFloating_triggered(bool check);                  //设置Dock窗口是否浮动
    void on_actDockVisible_triggered(bool checked);                 //设置Dock窗口是否隐藏不显示
    void on_actQiut_triggered();                                    //退出
    void on_treeFiles_currentItemChanged(QTreeWidgetItem *current, QTreeWidgetItem *previous);  //当前节点变化的时候，自动加载当前图片
    void on_dockWidget_visibilityChanged(bool visible);             //单击DockWidget组件的标题栏的关闭按钮时候，会隐藏在停靠区域，并且发射信号visibilityChanged;  停靠区域可见性变化
    void on_dockWidget_topLevelChanged(bool topLevel);              //当拖动DockWidget组件，使其浮动或者停靠时候，会发射信号topLevelChanged;  更新其Action的状态

private:
    Ui::ExQTreeWidget *ui;

    QLabel *m_labFlie;      //状态栏显示当前文件路径
    QPixmap m_curPixmap;    //显示当前文件图片
    float   m_ratio;        //图片缩放比例

};

#endif // EXQTREEWIDGET_H
```

对应的**.cpp**文件：

```cpp
#include "ExQTreeWidget.h"
#include "ui_ExQTreeWidget.h"

ExQTreeWidget::ExQTreeWidget(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::ExQTreeWidget)
{
    ui->setupUi(this);
    setWindowTitle(QObject::tr("QTreeWidget和QDockWidget的讲解和使用"));

    setCentralWidget(ui->scrollArea);                //设置scrollArea为中心控件
    initTree();

    m_labFlie = new QLabel("当前文件的路径:", this);
    ui->statusBar->addWidget(m_labFlie);
}

ExQTreeWidget::~ExQTreeWidget()
{
    delete ui;
}

//+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//初始化根节点（只能够有唯一）
void ExQTreeWidget::initTree()
{
    //准备工作
    ui->treeFiles->clear();
    QString dataStr = "";
    QIcon icon;
    icon.addFile(":/image/Image001.jpg");

    //创建唯一root的节点
    QTreeWidgetItem* root = new QTreeWidgetItem(treeItemType::itemRoot);
    root->setIcon(treeColNum::colItem, icon);
    root->setText(treeColNum::colItem, QString("相簿"));
    root->setText(treeColNum::colItemType, QString("treeItemType"));
    root->setFlags(Qt::ItemIsSelectable | Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsAutoTristate);
    root->setCheckState(treeColNum::colItem, Qt::Unchecked);
    root->setData(treeColNum::colItem, Qt::UserRole, QVariant(dataStr));

    //添加顶层节点
    ui->treeFiles->addTopLevelItem(root);
}

//添加目录节点
void ExQTreeWidget::addFolderItem(QTreeWidgetItem *parItem, QString dirName)
{
    QIcon icon;
    icon.addFile(":/image/Image006.jpg");

    //添加一个新的节点
    QTreeWidgetItem* item = new QTreeWidgetItem(treeItemType::itemFile);
    QString folderName = getFinalFolderName(dirName);
    item->setIcon(treeColNum::colItem, icon);
    item->setText(treeColNum::colItem, folderName);
    item->setText(treeColNum::colItemType, QString("treeItemType"));
    item->setFlags(Qt::ItemIsSelectable | Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsAutoTristate);
    item->setCheckState(treeColNum::colItem, Qt::Unchecked);
    item->setData(treeColNum::colItem, Qt::UserRole, QVariant(dirName));

    //添加子节点
    if (parItem->type() == treeItemType::itemFile) {                 //若是文件节点
        parItem->addChild(item);
    } else if (parItem->type() == treeItemType::itemRoot) {          //若是唯一root节点
        QTreeWidgetItem *root = ui->treeFiles->topLevelItem(0);
        root->addChild(item);
    }

}

//添加图片节点
void ExQTreeWidget::addImageItem(QTreeWidgetItem *parItem, QString fileName)
{
    if (parItem == nullptr)
        return;

    QIcon icon;
    icon.addFile(":/image/Image014.jpg");

    //添加一个新的节点
    QTreeWidgetItem* item = new QTreeWidgetItem(treeItemType::itemImage);
    QString folderName = getFinalFolderName(fileName);
    item->setIcon(treeColNum::colItem, icon);
    item->setText(treeColNum::colItem, folderName);
    item->setText(treeColNum::colItemType, QString("treeItemType"));
    item->setFlags(Qt::ItemIsSelectable | Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsAutoTristate);
    item->setCheckState(treeColNum::colItem, Qt::Unchecked);
    item->setData(treeColNum::colItem, Qt::UserRole, QVariant(fileName));

    //添加子节点
    if (parItem->type() == treeItemType::itemFile) {                 //若是文件节点
        parItem->addChild(item);
    } else if (parItem->type() == treeItemType::itemRoot) {          //若是唯一root节点
        QTreeWidgetItem *root = ui->treeFiles->topLevelItem(0);
        root->addChild(item);
    }
}

//从完整的路径里面，获取最后的文件夹名称
QString ExQTreeWidget::getFinalFolderName(const QString &pathName)
{
    QString path = pathName;
    int cnt = pathName.count();
    int i = pathName.lastIndexOf("/");
    QString str = pathName.right(cnt - i - 1);
    return str;
}

//遍历传进来的父节点下的所有子节点；每遍历过该节点，就在其节点的信息加一个#
void ExQTreeWidget::changeItemCaption(QTreeWidgetItem *parItem)
{
    QString str = "# " + parItem->text(treeColNum::colItem);
    parItem->setText(treeColNum::colItem, str);

    if (parItem->childCount() < 0)
        return;

    for (int i = 0; i < parItem->childCount(); i++) {
        changeItemCaption(parItem->child(i));                       //回调，调用自己
    }
}

//显示当前item的图片（默认以适配高度）
void ExQTreeWidget::displayImage(QTreeWidgetItem *item)
{
    QString fileName = item->data(treeColNum::colItem, Qt::UserRole).toString();
    m_labFlie->setText(fileName);
    m_curPixmap.load(fileName);                                     //从文件载入图片
    on_actAdaptiveHeight_triggered();                               //自动适应高度显示

    ui->actAmplification->setEnabled(true);
    ui->actShrink->setEnabled(true);
    ui->actZoomRealSize->setEnabled(true);
    ui->actAdaptiveHeight->setEnabled(true);
    ui->actAdaptiveWidth->setEnabled(true);
}

//+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//增加文件夹
void ExQTreeWidget::on_actAddFolder_triggered()
{
    QString path = QFileDialog::getExistingDirectory();             //选择目录

    if (! path.isEmpty()) {
        QTreeWidgetItem* item = ui->treeFiles->currentItem();       //获取当前节点

        if(item != nullptr)
            addFolderItem(item, path);
    }
}

//添加图片
void ExQTreeWidget::on_actAddFile_triggered()
{
    QStringList list = QFileDialog::getOpenFileNames(this, "选择多个将要加载的图片", "", "Images(*.jpg, *.png, *.*)");         //选择目录

    if (! list.isEmpty()) {
        QTreeWidgetItem* parItem = nullptr;
        QTreeWidgetItem* item = ui->treeFiles->currentItem();       //获取当前节点

        if (item == nullptr)
            return;

        if (item->type() == treeItemType::itemImage) {              //获得父节点
            parItem = item->parent();
        } else {
            parItem = item;
        }

        for (int i = 0; i < list.size(); i++) {
            QString strName = list.at(i);                          //获得文件名称
            addImageItem(parItem, strName);                        //添加图片文件到文件节点
        }
    }
}

//删除节点
void ExQTreeWidget::on_actDeleFile_triggered()
{
    QTreeWidgetItem* parItem = nullptr;
    QTreeWidgetItem* currItem = ui->treeFiles->currentItem();

    if (currItem->type() != treeItemType::itemRoot)
        parItem = currItem->parent();                              //只能够由其父节点删除
//    else
//        ui->treeFiles->takeTopLevelItem(0);                      //删除顶层节点使用这个

    if (currItem == nullptr || parItem == nullptr)
        return;

    parItem->removeChild(currItem);                                //移除没有从内存中删除，所以delete删除
    delete currItem;
}

//遍历所有的顶层节点(本处只有一个root顶层节点)
void ExQTreeWidget::on_actScanItems_triggered()
{
    for (int i = 0; i < ui->treeFiles->topLevelItemCount(); i++) {
        QTreeWidgetItem* currItem = ui->treeFiles->topLevelItem(i); //顶层item
        changeItemCaption(currItem);
    }
}

//图片自动适应高度
void ExQTreeWidget::on_actAdaptiveHeight_triggered()
{
    int height = ui->scrollArea->height();                        //得到scrollArea的高度
    int realHeight = m_curPixmap.height();                        //原始图片的实际高度
    m_ratio = height * 1.0 / realHeight;                          //当前显示比例，必须转换为浮点数

    QPixmap pixmap = m_curPixmap.scaledToHeight(height - 50);     //图片缩放到指定高度
    ui->labDisplay->setPixmap(pixmap);                            //设置Label的PixMap
}

//图片自动适应宽度
void ExQTreeWidget::on_actAdaptiveWidth_triggered()
{
    int width = ui->scrollArea->width();
    int realWidth = m_curPixmap.width();
    m_ratio = width * 1.0 / realWidth;

    QPixmap pixmap = m_curPixmap.scaledToHeight(width - 50);
    ui->labDisplay->setPixmap(pixmap);
}

//当前节点变化的时候，自动加载当前图片
void ExQTreeWidget::on_treeFiles_currentItemChanged(QTreeWidgetItem *current, QTreeWidgetItem *previous)
{
    if (current != nullptr && previous != nullptr) {
        displayImage(current);
    }
}


//放大
void ExQTreeWidget::on_actAmplification_triggered()
{
    m_ratio *= 1.2;                                             //在当前比例基础上乘以0.8
    int height = m_curPixmap.height() * m_ratio;                // 显示宽度
    int widht = m_curPixmap.width() * m_ratio;                  // 显示宽度

    QPixmap pix = m_curPixmap.scaled(widht, height);            //图片缩放到指定高度和宽度，保持长宽比例
    ui->labDisplay->setPixmap(pix);

}

//缩小
void ExQTreeWidget::on_actShrink_triggered()
{
    m_ratio *= 0.8;
    int height = m_curPixmap.height() * m_ratio;
    int widht = m_curPixmap.width() * m_ratio;

    QPixmap pix = m_curPixmap.scaled(widht, height);
    ui->labDisplay->setPixmap(pix);
}

//还原
void ExQTreeWidget::on_actZoomRealSize_triggered()
{
    m_ratio = 1;
    int height = m_curPixmap.height();
    int widht = m_curPixmap.width();

    QPixmap pix = m_curPixmap.scaled(widht, height);
    ui->labDisplay->setPixmap(pix);
}

//设置Dock窗口是否浮动
void ExQTreeWidget::on_actDockFloating_triggered(bool check)
{
    ui->dockWidget->setFloating(check);
}

//设置Dock窗口是否隐藏不显示
void ExQTreeWidget::on_actDockVisible_triggered(bool checked)
{
    ui->dockWidget->setVisible(!checked);
}

//退出
void ExQTreeWidget::on_actQiut_triggered()
{
    close();
}

//单击DockWidget组件的标题栏的关闭按钮时候，会隐藏在停靠区域，并且发射信号visibilityChanged;  停靠区域可见性变化
void ExQTreeWidget::on_dockWidget_visibilityChanged(bool visible)
{
    ui->actDockVisible->setChecked(visible);
}

//当拖动DockWidget组件，使其浮动或者停靠时候，会发射信号topLevelChanged;  更新其Action的状态
void ExQTreeWidget::on_dockWidget_topLevelChanged(bool topLevel)
{
    ui->actDockFloating->setChecked(topLevel);
}
```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples) 【QtQTreeWidgetEx】