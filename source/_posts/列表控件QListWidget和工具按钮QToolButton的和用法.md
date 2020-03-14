---
title: 列表控件QListWidget和工具按钮QToolButton的和用法
date: 2019-9-24 21:43:45
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt控件
---



**简介：** 主要讲解 `QListWidget`和`QToolButton`的和用法，其中还有**QToolBar**、**QToolBox**、**QTabWidget**这些简单是讲解和使用

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介，另外还有已下主要知识点

- 删除`item`时候：`takeItem()`
- **QToolButton**的`PopupMode`属性，和`setDefaultAction()`默认行为
- 给**QListWidget**添加鼠标右键弹出菜单

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## 运行效果：

先上一个最终的运行效果图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190921162110.gif"/>

<br>

## 布局设计图:

其中的布局设计，如下图

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190921162322.png"/>

这个其中主要是创建多个`Action()`, 然后使用添加到工具栏和按钮上面，从而得相同的效果

<br>

## takeItem()移除item：

**删除item时候，takeItem(row)函数只是移除，不删除对象：**

- **其中删除QListWidget的item时候，需要注意：**<font color=#FE7207  size=3 face="幼圆">**一定要手动删除其创建的item**</font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190901193426.png"/>

翻译如下：

> 从列表小部件中的给定行中删除并返回项;否则返回0。从列表小部件中删除的项目将不会由Qt管理，并且需要手动删除。

```cpp
//actDel删除一个指定的ListWidget的列表项
void ExQListWidget::on_actDel_triggered()
{
    int row = ui->listWidget->currentRow();
    QListWidgetItem* item = ui->listWidget->takeItem(row);             //删除该为row的item， 并且返回指针
    delete item;                                                       //手动删除对象
}
```

<br>

## PopupMode的属性(下拉小箭头):

**QToolButton的PopupMode属性，和setDefaultAction()默认行为：**

文档如下，例子如下面代码

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190901220227.png"/>

- ToolBar 添加项选择（带下拉框） 和退出；右侧一个按钮也添加一个项的选择

添加下图所示的两个地方

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190901223509.png"/>

【上图靠上的QToolButton】其风格样式属性是`setPopupMode(QToolButton::InstantPopup)`；和靠下面那个有区别，**点击所有地方都会弹出菜单**（由代码的ToolBotton的样式故意设置不一样进行演示的）

【上图靠下的QToolButton】其风格样式属性是`setPopupMode(QToolButton::MenuButtonPopup)`；默认可以点击**左侧主体**（非右侧三角形状的下拉部分），也会有反应(但是不弹出菜单)；**只有点击右侧有三角形区域，才会弹出菜单**，默认激活`setDefaultAction(ui->actSelPopMenu)`；而**actSelPopMenu**又在Design里面与**actSelInvs**进行了关联。所以点击其左侧部分，相当于点击了反选

其中代码如下：

```cpp
//ToolBar 添加项选择（带下拉框） 和退出；右侧一个按钮也添加一个项的选择
void ExQListWidget::creatorPopMenu()
{
    //创建菜单
    QMenu* menu = new QMenu(this);                                         //创建弹出式菜单
    menu->addAction(ui->actSelAll);
    menu->addAction(ui->actSelInvs);
    menu->addAction(ui->actSelNone);

    //右侧ListWidget， 其上方的toolBtn按钮
    //设置toolBtnSelectItem的多个属性：PopupMode、ToolButtonStyle等（在Design已经设置）
    ui->toolBtnSelectItem->setDefaultAction(ui->actSelPopMenu);            //关联action
    ui->toolBtnSelectItem->setMenu(menu);                                  //设置下拉菜单menu

    //工具栏QToolBar上面的下拉菜单样式按钮
    QToolButton* toolBtn = new QToolButton(this);
    toolBtn->setPopupMode(QToolButton::InstantPopup);                      //下拉式菜单样式属性
    toolBtn->setToolButtonStyle(Qt::ToolButtonTextUnderIcon);              //设置汉字出现在icon下面
    toolBtn->setDefaultAction(ui->actSelPopMenu);                          //关联action
    toolBtn->setMenu(menu);                                                //关联菜单
    ui->toolBar->addSeparator();                                           //添加隔栏
    ui->toolBar->addWidget(toolBtn);                                       //添加到工具栏

    //添加退出按钮
    ui->toolBar->addSeparator();
    ui->toolBar->addAction(ui->actExit);                                   //添加退出按钮
}
```

<br>

## 给QListWidget添加鼠标右键弹出菜单：

> 只要是继承于**QWidget**均有**customContextMenuRequested**这个信号，是鼠标右键发射的信号；可以用来创建和运行右键快捷菜单

- **设置QListWidget支持右键菜单,这句话一定要有**， 不然下面的设置不会生效

```cpp
ui->listWidget->setContextMenuPolicy(Qt::CustomContextMenu);
```

实现菜单细节：

```cpp
//鼠标右键自定义快捷菜单
void ExQListWidget::on_listWidget_customContextMenuRequested(const QPoint &pos)
{
    qDebug()<<"11111111";
    Q_UNUSED(pos)
    QMenu* menu = new QMenu(this);                                         //创建菜单
    menu->addAction(ui->actListInit);
    menu->addAction(ui->actAdd);
    menu->addAction(ui->actDel);
    menu->addAction(ui->actClear);
    menu->addAction(ui->actInsert);
    menu->addSeparator();
    menu->addAction(ui->actSelAll);
    menu->addAction(ui->actSelNone);
    menu->addAction(ui->actSelInvs);
    menu->exec(QCursor::pos());                                            //在鼠标光标位置显示右键快捷菜单
    delete menu;                                                           //手工创建的指针必须手工删除
}
```

实现效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190901224141.png"/>

<br>

## 关于初始化`QListWidgetItem`按钮：

左侧的初始化ListWidget，点击之前会确认右上角的**□可编辑复选框**是否被勾上√， **其完整含义是：□ 框 初始化的成员，是否设置为可编辑，若勾选，那么初始化的成员都是可以别编辑的</u>****

且新创建的**QListWidgetItem**项，应该的**Flags**属性一般设置为如下

```cpp
//actListInit初始化ListWidget的列表项listWidget
void ExQListWidget::on_actListInit_triggered()
{
    QListWidgetItem* item;                                          //每一行都是一个QListWidgetItem
    QIcon icon;
    icon.addFile(":/images/github.png");
    bool chk = ui->checkBox->isChecked();                           //是否可编辑

    ui->listWidget->clear();
    for (int i = 0; i < 13; i++) {
        item = new QListWidgetItem();//(ui->listWidget);            //创建一个item
        item->setText(QString("初始化 第%1个项item").arg(i));        //设置文字
        item->setIcon(icon);                                        //设置icon图标
        item->setCheckState(Qt::Unchecked);                         //设置选中方式

        if (chk)                                                    //可编辑
            item->setFlags(Qt::ItemIsSelectable | Qt::ItemIsEditable | Qt::ItemIsUserCheckable | Qt::ItemIsEnabled);
        else
            item->setFlags(Qt::ItemIsSelectable | Qt::ItemIsUserCheckable | Qt::ItemIsEnabled);

        ui->listWidget->addItem(item);                              //添加项item到listWidget里面
    }
}
```

<br>

## 核心源码：

```cpp
//actInsert在ListWidget的列表项中插入一个项item
void ExQListWidget::on_actInsert_triggered()
{
    QListWidgetItem* item;                                           //每一行都是一个QListWidgetItem
    QIcon icon;
    icon.addFile(":/images/gril.png");
    bool chk = ui->checkBox->isChecked();                             //是否可编辑

    item = new QListWidgetItem();                                     //创建一个item
    item->setText(QString("插入一个项item: " + QDateTime::currentDateTime().toString("yyyy-mm-dd hh:MM:ss:zzz")));  //设置文字
    item->setIcon(icon);                                              //设置icon图标
    item->setCheckState(Qt::Unchecked);                               //设置选中方式

    if (chk)                                                          //可编辑
        item->setFlags(Qt::ItemIsSelectable | Qt::ItemIsEditable | Qt::ItemIsUserCheckable | Qt::ItemIsEnabled);
    else
        item->setFlags(Qt::ItemIsSelectable | Qt::ItemIsUserCheckable | Qt::ItemIsEnabled);

    ui->listWidget->insertItem(ui->listWidget->currentRow(), item);    //添加项item到listWidget里面
}

=====================================================================================

//actAdd添加一个指定的ListWidget的列表项
void ExQListWidget::on_actAdd_triggered()
{
    QListWidgetItem* item;                                           //每一行都是一个QListWidgetItem
    QIcon icon;
    icon.addFile(":/images/TREE.png");
    bool chk = ui->checkBox->isChecked();                             //是否可编辑

    item = new QListWidgetItem();                                     //创建一个item
    item->setText(QString("添加一个项item: " + QDateTime::currentDateTime().toString("yyyy-mm-dd hh:MM:ss:zzz")));  //设置文字
    item->setIcon(icon);                                              //设置icon图标
    item->setCheckState(Qt::Unchecked);                               //设置选中方式

    if (chk)                                                          //可编辑
        item->setFlags(Qt::ItemIsSelectable | Qt::ItemIsEditable | Qt::ItemIsUserTristate | Qt::ItemIsEnabled);
    else
        item->setFlags(Qt::ItemIsSelectable | Qt::ItemIsUserTristate | Qt::ItemIsEnabled);

    ui->listWidget->addItem(item);                                    //添加项item到listWidget里面
}

=====================================================================================

//设置全选
void ExQListWidget::on_actSelAll_triggered()
{
    int nCount = ui->listWidget->count();
    for (int i = 0; i < nCount; i++) {
        QListWidgetItem* item = ui->listWidget->item(i);
        item->setCheckState(Qt::Checked);
    }
}

=====================================================================================

//设置QListWidget的当前item发生变化，触发的信号,会在右侧显示出来
void ExQListWidget::on_listWidget_currentItemChanged(QListWidgetItem *current, QListWidgetItem *previous)
{
    if (current != nullptr) {
        if (previous == nullptr)
            ui->lineEditRight->setText(QString("当前项:%1；  前一项%2").arg(current->text()).arg("不存在"));
        else
            ui->lineEditRight->setText(QString("当前项:%1；  前一项%2").arg(current->text()).arg(previous->text()));
    }
}
```

<br>

## 源码下载：

[https://github.com/xmuli/QtExamples](https://github.com/xmuli/QtExamples)【QtQListWidgetEx】

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

