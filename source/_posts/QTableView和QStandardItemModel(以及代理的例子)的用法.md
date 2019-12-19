---
title: QTableView和QStandardItemModel(以及代理的例子)的用法
date: 2019-12-17 00:03:55
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  继续学习`QTableView`和`QStandardItemModel`的用法，以及这里例子里面有具体使用代理的例子（**模型-视图-代理）**，其中专门给代理写一成一个类来实现他们；书写一个简单的`Qt`例子🌰，用来实现和验证它的控件的一些属性和功能的用法。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-12-12_23-59-14_mark.png"/>

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [QTableView和QStandardItemModel(以及代理的例子)的用法](https://blog.csdn.net/qq_33154343/article/details/103572418)
>
> **相关博文：** [Model-View-Delegate:"模型-视图-代理"的讲解](https://blog.csdn.net/qq_33154343/article/details/103501667)

<br>

## 系统环境：

**编程环境：**  `MacOS 10.14.6 (18G103)`   **编程软件：** `Qt 5.9.8`， `Qt Creator 4.8.2`

<br>

## QStandardItemModel：

`QStandardItemModel`是<font color=#D0087E size=4 face="幼圆">标准的以**项数据（item data）**为基础</font>的 **数据模型类**；

<br>

## QTableView：

`QTableView`是一个二维数据表视图组件，当通过`setModel（）`的设置一个`QStandardItemModel`的时候，一个单元格显示`QStandardItemModel`数据模型的一个项。

```cpp
m_model = new QStandardItemModel(2, 5, this);                          //设置数据模型，一开始设置为默认的2行6列表的一个表
m_selectModet = new QItemSelectionModel(m_model, this);                //设置选择模型

ui->tableView->setModel(m_model);                                      //设置数据模型
ui->tableView->setSelectionModel(m_selectModet);                       //设置选择模型
ui->tableView->setSelectionMode(QAbstractItemView::ExtendedSelection); //设置选择模式
ui->tableView->setSelectionBehavior(QAbstractItemView::SelectItems);   //设置选择行为
```

<br>

## QItemSelectionModel:

`QItemSelectionModel`是一个用于跟踪视图组建的单元格选择状态类；当在QTableView选择某一个或者一些单元格的时候，可以通过`QItemSelectionModel`获取选中的单元格的模型索引，为单元格的选择系统方便；

<br>

## 设计思路：

在 **ExQStandardItemModel.h**和 **ExQStandardItemModel.cpp**里面，使用中规中矩的Table视图和数据进行修改，可以显示出来下面的这些数据等；然后再就单独设计一个代理来实现，在 **ExDelegate.h**和  **ExDelegate.cpp** 里面，单独写一个代理组件（eg：创建用于编辑的模型数据的widget组件，如一个QComboBox组件），将QComboBox插入到Table控件里面。这样就可以将所有“视图-模型-代理”这三个全部都使用小例子的方式显示出来。

<font color=#FE7207 size=4 face="幼圆">若是想要使用代理部分，就必须重在如下的4个函数</font>

```cpp
virtual QWidget* createEditor(QWidget *parent, const QStyleOptionViewItem &option, const QModelIndex &index) const override;      //创建用于编辑的模型数据的widget组件，如一个QComboBox组件
    virtual void setEditorData(QWidget *editor, const QModelIndex &index) const override;                                             //从数据模型获取数据，显示在代理组件editor之中，让其编辑
    virtual void setModelData(QWidget *editor, QAbstractItemModel *model, const QModelIndex &index) const override;                   //代理组件editor上的数据更新到数据模型
    virtual void updateEditorGeometry(QWidget *editor, const QStyleOptionViewItem &option, const QModelIndex &index) const override;  //用于给widget组件设置一个合适的大小
```



<br>

## 运行效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/20191213_00.gif"/>

<br>

## 源码分析：

其中核心部分的源码，重点和一些难点以及需要注意的一些地方，贴出来如下:

### 模型-视图部分：

在 **.h**头文件

```cpp
class ExQStandardItemModel : public QMainWindow
{
private:
    void init(QStringList& list);           //从list初始化数据模型

private slots:
    void onCurrentChanged(const QModelIndex& current, const QModelIndex& previous);  //当前单元格发生变化时
    void on_actOpen_triggered();            //打开和导入文件，并且在plainTextEdit里面显示
    void on_actAppend_triggered();          //在表格的最后一行添加一行
    void on_actSave_triggered();            //保存文件
    void on_actInsert_triggered();          //在当前选中的一行，其前面插入一行
    void on_actDelete_triggered();          //删除一行
    void on_actExit_triggered();            //关闭退出
    void on_actModelData_triggered();       //预览模型
    void on_actAlignLeft_triggered();       //左对齐
    void on_actAlignCenter_triggered();     //文本居中
    void on_actAlingRight_triggered();      //文本右对齐
    void on_actBold_triggered(bool checked);//文本加粗

private:
    QLabel  *m_labCurrFile;                 //当前文件
    QLabel *m_labCellPos;                   //当前单元格行列号
    QLabel *m_labCellText;                  //当前单元格数据内容
    QStandardItemModel *m_model;            //数据模型
    QItemSelectionModel *m_selectModet;     //选择模型

};
```

在 **.cpp**源文件里面

```cpp
#define COLUMN 6  //数据表的列数

ExQStandardItemModel::ExQStandardItemModel(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::ExQStandardItemModel)
{
    ui->setupUi(this);
    setWindowTitle(QObject::tr("QTableView和QStandardItemModel的用法"));

    ui->mainToolBar->setToolButtonStyle(Qt::ToolButtonTextUnderIcon);     //设置主工具栏的图标样式风格

    m_labCurrFile = new QLabel("当前文件：", this);                       //设置状态栏
    m_labCellPos = new QLabel("当前单元格：", this);
    m_labCellText = new QLabel("单元格内容：", this);
    m_labCurrFile->setMinimumWidth(200);
    m_labCellPos->setMinimumWidth(200);
    m_labCellText->setMinimumWidth(200);
    ui->statusBar->addWidget(m_labCurrFile);
    ui->statusBar->addWidget(m_labCellPos);
    ui->statusBar->addWidget(m_labCellText);

    m_model = new QStandardItemModel(2, COLUMN, this);                          //设置数据模型，一开始设置为默认的2行6列表的一个表
    m_selectModet = new QItemSelectionModel(m_model, this);                //设置选择模型

    ui->tableView->setModel(m_model);                                      //设置数据模型
    ui->tableView->setSelectionModel(m_selectModet);                       //设置选择模型
    ui->tableView->setSelectionMode(QAbstractItemView::ExtendedSelection); //设置选择模式
    ui->tableView->setSelectionBehavior(QAbstractItemView::SelectItems);   //设置选择行为

    connect(m_selectModet, SIGNAL(currentChanged(QModelIndex, QModelIndex)), this, SLOT(onCurrentChanged(QModelIndex, QModelIndex)));  //选择当前单元格变化时的信号与槽
}

ExQStandardItemModel::~ExQStandardItemModel()
{
    delete ui;
}

//从list初始化数据模型QTableView里面
void ExQStandardItemModel::init(QStringList &list)
{
    int rowCount = list.count();                                           //文本行数，第一行为表头
    m_model->setRowCount(rowCount - 1);

    QString header = list.at(0);
    QStringList headerList = header.split(QRegExp("\\s+"), QString::SkipEmptyParts); //通过一个或者多个空格或者tab按键切割
    m_model->setHorizontalHeaderLabels(headerList);                       //设置表头

    QStandardItem *item = nullptr;                                        //此处开始，设置表格数据
    QStringList tempList;
    int j = 0;

    for (int i = 1; i < rowCount; i++) {
        QString aLineText = list.at(i);
        tempList = aLineText.split(QRegExp("\\s+"), QString::SkipEmptyParts);//正则表达式中\s匹配任何空白字符，包括空格、制表符、换页符等等, 等价于[ \f\n\r\t\v]

        for ( j = 0; j < COLUMN - 1; j++) {                                     //设置前5列的item

            if (j == 3) {
                ExDelegate *itemDelegate = new ExDelegate();
                ui->tableView->setItemDelegateForColumn(3, itemDelegate);
            }

            item = new QStandardItem(tempList.at(j));
            m_model->setItem(i - 1, j, item);
        }



        item = new QStandardItem(tempList.at(j));                          //最后一列的item
        item->setCheckable(true);                                          //设置有检查框

        if ( tempList.at(j) == "https://www.google.com")
            item->setCheckState(Qt::Unchecked);
        else
            item->setCheckState(Qt::Checked);

        m_model->setItem(i - 1, COLUMN -1, item);
    }
}

//当前单元格发生变化时
void ExQStandardItemModel::onCurrentChanged(const QModelIndex &current, const QModelIndex &previous)
{
    if (current.isValid()) {                                               //当前模型索性有效
        m_labCellPos->setText(QString("当前单元格：%1行, %2列").arg(current.row()).arg(current.column()));
        QStandardItem *item = m_model->itemFromIndex(current);             //从模型索引获得Item
        m_labCellText->setText(QString("当前文件：%1").arg(item->text()));  //显示item的文字内容

        QFont font = item->font();
        ui->actBold->setChecked(font.bold());                              //更新actFontBold的check状态
    }
}

//action+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//打开和导入文件，并且在plainTextEdit里面显示
void ExQStandardItemModel::on_actOpen_triggered()
{
    QString currPath = QCoreApplication::applicationDirPath();
    QString fileName = QFileDialog::getOpenFileName(this, "打开一个文件", currPath, "导入数据文件(*txt);;所有文件(*.*)");

    if (fileName.isEmpty())
        return;

    QStringList list;
    QFile file(fileName);
    if (file.open(QIODevice::ReadOnly | QIODevice::Text)) {                //只读形式打开文本文件
        QTextStream stream(&file);                                         //用文本流读取文件
        ui->plainTextEdit->clear();

        while (!stream.atEnd()) {                                          //读取文本中文本的内容
            QString str = stream.readLine();
            ui->plainTextEdit->appendPlainText(str);
            list.append(str);
        }

        file.close();                                                      //关闭
        m_labCurrFile->setText("当前文件：" + fileName);                    //设置状态栏

        ui->actAppend->setEnabled(true);                                   //设置action的Enabled的属性
        ui->actInsert->setEnabled(true);
        ui->actDelete->setEnabled(true);
        ui->actSave->setEnabled(true);
    }

    init(list);                                                            //初始化.txt的数据
}

//在表格的最后一行添加一行
void ExQStandardItemModel::on_actAppend_triggered()
{
    QList<QStandardItem *> list;
    QStandardItem *item;

    //获取表头前五列
    for (int i = 0; i < COLUMN - 1; i++) {
        item = new QStandardItem("添加一行");
        list<<item;
    }

    //获取最后一列
    QString str = m_model->headerData(m_model->columnCount() - 1, Qt::Horizontal, Qt::DisplayRole).toString();
    item = new QStandardItem(str + "添加一行");
    item->setCheckable(true);
    list<<item;

    m_model->insertRow(m_model->rowCount(), list);
    QModelIndex currIndex = m_model->index(m_model->rowCount() - 1, 0);  //创建最后一行的ModelIndex
    m_selectModet->clearSelection();
    m_selectModet->setCurrentIndex(currIndex, QItemSelectionModel::Select);
}

//保存文件
void ExQStandardItemModel::on_actSave_triggered()
{
    QString path = QCoreApplication::applicationDirPath();
    QString fileName = QFileDialog::getSaveFileName(this, tr("选择一个文件"), path, "另存数据(*.txt);;所有数据(*.*)");
    if ( fileName.isEmpty())
        return;

    QFile file(fileName);

    if (!(file.open(QIODevice::ReadWrite | QIODevice::Text | QIODevice::Truncate))) //读写，文本，覆盖原有内容打开
        return;

    QTextStream stream(&file);                                            //用文本流读取文件
    QStandardItem *item;
    int i = 0 ;
    int j = 0;
    QString str = "";
    ui->plainTextEdit->clear();

    //获取表头文字
    for (i = 0; i < m_model->columnCount(); i++) {
        item =  m_model->horizontalHeaderItem(i);
        str = str + item->text() + "\t\t";                                 //以TAB按键隔开
    }
    stream<<str<<"\n";
    ui->plainTextEdit->appendPlainText(str);

    //获取数据区文字
    for (i = 1; i < m_model->rowCount(); i++) {
        str = "";

        for (j = 0; j < m_model->columnCount(); j++) {
            item = m_model->item(i, j);
            str = str + item->text() + "\t\t";
        }
        stream<<str<<"\n";
        ui->plainTextEdit->appendPlainText(str);
    }
}

//在当前选中的一行，其前面插入一行
void ExQStandardItemModel::on_actInsert_triggered()
{
    QList<QStandardItem *> list;
    QStandardItem *item;

    for (int i = 0; i < COLUMN - 1; i++) {
        item = new QStandardItem("插入一行");
        list<<item;
    }

    QString str = m_model->headerData(m_model->columnCount() - 1, Qt::Horizontal, Qt::DisplayRole).toString();
    item = new QStandardItem(str + "插入一行");
    item->setCheckable(true);
    list<<item;

    QModelIndex currIndex = m_selectModet->currentIndex();
    m_model->insertRow(currIndex.row(), list);                 //在当前行的前面插入一行
    m_selectModet->clearSelection();
    m_selectModet->setCurrentIndex(currIndex, QItemSelectionModel::Select);
}


//删除一行
void ExQStandardItemModel::on_actDelete_triggered()
{
    QModelIndex currIndex = m_selectModet->currentIndex();
    m_model->removeRow(currIndex.row());
}

//关闭退出
void ExQStandardItemModel::on_actExit_triggered()
{
    close();
}

//预览数据模型:模型数据导出到PlainTextEdit显示
void ExQStandardItemModel::on_actModelData_triggered()
{                                          //用文本流读取文件
    QStandardItem *item;
    int i = 0 ;
    int j = 0;
    QString str = "";
    ui->plainTextEdit->clear();

    //获取表头文字
    for (i = 0; i < m_model->columnCount(); i++) {
        item =  m_model->horizontalHeaderItem(i);
        str = str + item->text() + "\t\t";                                 //以TAB按键隔开
    }

    ui->plainTextEdit->appendPlainText(str);

    //获取数据区文字
    for (i = 1; i < m_model->rowCount(); i++) {
        str = "";

        for (j = 0; j < m_model->columnCount(); j++) {
            item = m_model->item(i, j);
            str = str + item->text() + "\t\t";
        }
        ui->plainTextEdit->appendPlainText(str);
    }
}

//左对齐
void ExQStandardItemModel::on_actAlignLeft_triggered()
{
    if (!m_selectModet->hasSelection())
        return;

    QModelIndexList list = m_selectModet->selectedIndexes();     //获得选中的List<item>

    for (int i = 0; i < list.count(); i++) {
        QModelIndex index = list.at(i);
        QStandardItem *item = m_model->itemFromIndex(index);
        item->setTextAlignment(Qt::AlignLeft);                   //设置文本左对齐
    }
}

//文本居中
void ExQStandardItemModel::on_actAlignCenter_triggered()
{
    if (!m_selectModet->hasSelection())
        return;

    QModelIndexList list = m_selectModet->selectedIndexes();     //获得选中的List<item>

    for (int i = 0; i < list.count(); i++) {
        QModelIndex index = list.at(i);                          //获取一个模型索引
        QStandardItem *item = m_model->itemFromIndex(index);
        item->setTextAlignment(Qt::AlignCenter);                 //设置文本居中
    }
}

//文本右对齐
void ExQStandardItemModel::on_actAlingRight_triggered()
{
    if (!m_selectModet->hasSelection())
        return;

    QModelIndexList list = m_selectModet->selectedIndexes();

    for (int i = 0; i < list.count(); i++) {
        QModelIndex index = list.at(i);
        QStandardItem *item = m_model->itemFromIndex(index);
        item->setTextAlignment(Qt::AlignRight);
    }
}

//文本加粗
void ExQStandardItemModel::on_actBold_triggered(bool checked)
{
    if (!m_selectModet->hasSelection())
        return;

    QModelIndexList list = m_selectModet->selectedIndexes();

    for (int i = 0; i < list.count(); i++) {
        QModelIndex index = list.at(i);
        QStandardItem *item = m_model->itemFromIndex(index);
        QFont font = item->font();
        font.setBold(checked);
        item->setFont(font);
    }
}
```

### 代理部分：

在 **.h**头文件

```cpp
#ifndef EXDELEGATE_H
#define EXDELEGATE_H

#include <QStyledItemDelegate>

class ExDelegate : public QStyledItemDelegate
{
public:
    ExDelegate();

public:
    //若是写代理的组件，那么必须要有下面这是个重写这四个函数
    virtual QWidget* createEditor(QWidget *parent, const QStyleOptionViewItem &option, const QModelIndex &index) const override;      //创建用于编辑的模型数据的widget组件，如一个QComboBox组件
    virtual void setEditorData(QWidget *editor, const QModelIndex &index) const override;                                             //从数据模型获取数据，显示在代理组件editor之中，让其编辑
    virtual void setModelData(QWidget *editor, QAbstractItemModel *model, const QModelIndex &index) const override;                   //代理组件editor上的数据更新到数据模型
    virtual void updateEditorGeometry(QWidget *editor, const QStyleOptionViewItem &option, const QModelIndex &index) const override;  //用于给widget组件设置一个合适的大小
};

#endif // EXDELEGATE_H
```

在 **.cpp**头文件

```cpp
#include "ExDelegate.h"
#include <QComboBox>

ExDelegate::ExDelegate()
{
}

//创建用于编辑的模型数据的widget组件，如一个QComboBox组件
QWidget *ExDelegate::createEditor(QWidget *parent, const QStyleOptionViewItem &option, const QModelIndex &index) const
{
    QComboBox *comboBox = new QComboBox(parent);
    comboBox->addItem("github：https://github.com/touwoyimuli");
    comboBox->addItem("csdn博客：https://blog.csdn.net/qq_33154343");
    comboBox->addItem("github.io博客：https://touwoyimuli.github.io/");
    comboBox->addItem("https://www.google.com/ qt 学习 加油 2019-09-30");

    return  comboBox;                      //返回此编辑器
}

//从数据模型获取数据，显示在代理组件之中，让其编辑
void ExDelegate::setEditorData(QWidget *editor, const QModelIndex &index) const
{
    QString str = index.model()->data(index, Qt::EditRole).toString();
    QComboBox *comboBox = static_cast<QComboBox *>(editor);
    comboBox->addItem(str);
    //其他组件的话， 此处可以设定写的值一类，eg：setText(str)  setValue()
}

//将widget上的数据更新到数据模型
void ExDelegate::setModelData(QWidget *editor, QAbstractItemModel *model, const QModelIndex &index) const
{
    QComboBox *comboBox = static_cast<QComboBox *>(editor);
    QString str = comboBox->currentText();
    model->setData(index, str, Qt::EditRole);
}

//用于给widget组件设置一个合适的大小
void ExDelegate::updateEditorGeometry(QWidget *editor, const QStyleOptionViewItem &option, const QModelIndex &index) const
{
    editor->setGeometry(option.rect);      //设置组件的大小
}
```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples) 【QtQStandardItemModelEx】