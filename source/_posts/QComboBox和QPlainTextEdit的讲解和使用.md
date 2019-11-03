---
title: QComboBox和QPlainTextEdit的讲解和使用
date: 2019-9-22 00:01:30
toc: true
categories: 
 - [学习 - qt]
 - [学习 - 数据结构]
 - [学习 - 底层原理、思想架构]
 - [学习 - 编码规范，辅助技巧]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt控件
---



**简介：**  下拉列表框`QComboBox`和富文本编辑器`QPlainTextEdit`的介绍和使用

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [QComboBox和QPlainTextEdit的讲解和使用](https://blog.csdn.net/qq_33154343/article/details/101127870) 

<br>

## 运行效果：

先上一个最终的运行效果图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190921170623.gif"/>

<br>

## QComboBox属性：

`QComboBox`是下拉列表框组件类，它提供一个下拉列表供用户选择，也可以直接当作一个`QLineEdit` 用作输入。`QComboBox`除了显示可见下拉列表外，每个项（**item**，或称列表项）还可以关联一个**QVariant**类型的变量，用于存储一些不可见数据。

`QComboBox`存储的项是一个列表，但是`QComboBox`不提供整个列表用于访问，可以通过索引访问某个项。访问项的一些函数主要有以下几种。其一些基本属性和常用函数介绍：

| 常用函数                                           | 含义                                                 |
| -------------------------------------------------- | ---------------------------------------------------- |
| int currentIndex( )                                | 返回当前项的序号，第一个项的序号为0                  |
| QString currentText( )                             | 返回当前项的文字                                     |
| QVariant currentData(int role=Qt:UserRole)         | 返回当前项的关联数据，数据的缺省角色role=Qt:UserRole |
| QString itemText(int index)                        | 返回指定索引号的项的文字                             |
| QVariant itemData(int index,int role=Qt::UserRole) | 返回指定索引号的项的关联数据                         |
| int count（)                                       | 返回项的个数                                         |

**在一个QComboBox组件上选择项发生变化时，会发射如下两个信号：**

这两个信号只是传递的参数不同，一个传递的是当前项的索引号，一个传递的当前项的文字。

> void currentIndexChanged(int index)
> void currentIndexChanged(const QString &text)

<br>

## QPlain TextEdit属性：

`QPlainTextEdit`是一个多行文本编辑器，用于显示和编辑多行简单文本。另外，还有一个**QTextEdit**
组件，是一个所见即所得的可以编辑带格式文本的组件，以**HTML**格式标记符定义文本格式。

`QPlainTextEdit` 提供**cut( )、copy( )、paste( )、undo( )、redo( )、clear( )、selectAll( )**等标准编辑功
能的槽函数，`QPlainTextEdit`还提供一个标准的右键快捷菜单。

<font color=#D0087E size=4 face="幼圆">`QPlainTextEdit`的文字内容以**QTextDocument**类型存储，函数`document()`返回这个文档对象的
指针。
**QTextDocument**是内存中的文本对象，以文本块的方式存储，一个文本块就是一个段落，每
个段落以回车符结束。**QTextDocument**提供一些函数实现对文本内容的存取。</font>

| 常用函数                                      | 含义()                                                |
| --------------------------------------------- | ----------------------------------------------------- |
| appendPlainText()                             | 向QPlain TextEdit添加一行话                           |
| int blockCount()                              | 获得文本块个数                                        |
| QTextBlock findBlockByNumber(int blockNumber) | 读取某一个文本块，序号从0开始，至blockCount()-1结束。 |

一个**document**有多个**TextBlock**，从**document**中读取出的一个文本块类型为**QTextBlock**，通过`QTextBlock.：text()`函数可以获取其纯文本文字。

<br>

## 核心源码：

```cpp
//左上角区域+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//初始化简单的QComboBox控件
void ExQcomboBox::on_btnLeftInit_clicked()
{
    QIcon ico;
    ico.addFile(":/images/github.ico");

    ui->comBoxLeft->clear();
    for (int i = 0; i < 13; i++) {
        ui->comBoxLeft->addItem(ico, QString("第%1个item项").arg(i));   //带有ico图标的项
    }
}

//清除简单的QComboBox控件
void ExQcomboBox::on_btnLeftClear_clicked()
{
    ui->comBoxLeft->clear();
}

//勾选QComboBox为可以编辑状态
void ExQcomboBox::on_checkBoxOnlyWrite_clicked()
{
    if(ui->checkBoxOnlyWrite->isChecked())
        ui->comBoxLeft->setEditable(true);
    else
        ui->comBoxLeft->setEditable(false);
}

//右上角区域+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//初始化复杂的QComboBox控件（给每一项都添加一个对应的自定义数据[不显示]）
void ExQcomboBox::on_btnRightInit_clicked()
{
    QIcon ico;
    ico.addFile(":/images/gril.ico");

    QMap<QString, QString> map;
    map.insert("张投", "16岁");
    map.insert("张我", "17岁");
    map.insert("张以", "18岁");
    map.insert("张木", "19岁");
    map.insert("张李", "20岁");
    map.insert("张，", "21岁");
    map.insert("张报", "22岁");
    map.insert("张之", "23岁");
    map.insert("张以", "24岁");
    map.insert("张琼", "25岁");
    map.insert("张玖", "26岁");
    map.insert("张。", "27岁");

    ui->comBoxRight->clear();
    foreach(QString str, map.keys()){
        ui->comBoxRight->addItem(ico, str, map.value(str));           //因为有Map，所以QComboBox显示会按照key排序，而非上面的定义顺序,注意不是map.key(str)
    }
}

//底部区域+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//文本框内容每次读取一行，添加到ComboBox作为item项
void ExQcomboBox::on_btnBottomAdd_clicked()
{
    QTextDocument* doc = ui->plainTextEdit->document();               //获取文本对象
    int cnt = doc->blockCount();                                      //回车符是一个block
    QIcon ico;
    ico.addFile(":/images/github.ico");
    ui->comBoxLeft->clear();
    ui->comBoxRight->clear();

    for (int i = 0; i < cnt; i++) {
        QTextBlock text = doc->findBlockByNumber(i);                  //获取文本中一段（以换行为标志）
        ui->comBoxLeft->addItem(ico, text.text());
        ui->comBoxRight->addItem(ico, text.text(), QString("附加内容:%1").arg(i));
    }
}

//清除可编辑的富文本的编辑器的所有内容
void ExQcomboBox::on_btnBottomClear_clicked()
{
    ui->plainTextEdit->clear();
}

//设置富文本的编辑器(plainTextEdit)只可读
void ExQcomboBox::on_checkBoxOnlyRead_clicked()
{
    if(ui->checkBoxOnlyRead->isChecked())
        ui->plainTextEdit->setEnabled(false);
    else
        ui->plainTextEdit->setEnabled(true);
}


//公共的槽函数区域+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//显示当前选中的ComboBox的item项的内容
void ExQcomboBox::onSelectDisplay(QString str)
{
    QString strData = ui->comBoxRight->currentData().toString();     // 获取当前item的关联数据的内容
    ui->labDisplay->setText(str + "  " + strData);
    ui->plainTextEdit->appendPlainText(str + "  " + strData);
}

```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples) 【QtQlistWidgetEx】

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>