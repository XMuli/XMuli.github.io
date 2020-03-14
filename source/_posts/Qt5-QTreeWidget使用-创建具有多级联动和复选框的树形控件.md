---
title: Qt5 QTreeWidget使用 创建具有多级联动和复选框的树形控件
date: 2019-7-10 15:10:53
updated: 2019-7-10 15:10:57
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - qt]
---



​	**简述：**  **通过使用树形控件`QTreeWidget`创建具有联动功能的和复选框树形控件，实现勾选一个（选中），其父节点也会改变相应的状态（**且父亲节点会迭代修改状态），弥补参考文章的不足之处，创建工作中更加有效且实用的控件。

<!-- more -->

[TOC]

**编程环境：**  win10 x64 专业版

**编程软件：**  visual studio 2015， Qt 5.9.8



## 本博文的简述or解决问题？

​		通过使用树形控件`QTreeWidget`创建具有联动功能和复选框的树形控件，实现勾选一个（选中），其父节点也会改变相应的状态（**且父亲节点会迭代修改状态**），弥补参考文章的不足之处，创建工作中更加有效且实用的控件。在Qt中的树形控件称为QTreeWidget，而控件里的树形节点称为`QTreeWidgetItem`。



## 功能实现：

- 带有复选框的树形控件
- 多级联动更新状态（迭代）
- 文末附带添加右键菜单功能（已经实现，此处就只贴出参考文章）
- 弥补所参考文章，只能够实现二级（大于二级就会无关联）的缺陷



## 思路架构：

1. 在ui界面，拖曳控件`QTreeWidget`控件，命名
2. 初始化，添加多个（爷、父亲、儿子、 孙子等多级）节点
3. 动态节点的时刻迭代更新状态



## 运行演示：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710150847.gif)





## 源码下载：

<font color=#FE7207  size=4 face="幼圆">QDlgTreeWidget.h</font>

```c
#pragma once

#include <QWidget>
#include<QTreeWidgetItem>
namespace Ui { class QDlgTreeWidget; };

class QDlgTreeWidget : public QWidget
{
	Q_OBJECT

public:
	QDlgTreeWidget(QWidget *parent = Q_NULLPTR);
	~QDlgTreeWidget();


public: //申明初始化函数
	void init();
	void updateParentItem(QTreeWidgetItem* item);

public slots:   //申明信号与槽,当树形控件的子选项被改变时执行
	void onTreeItemChanged(QTreeWidgetItem* item, int column);


private:
	Ui::QDlgTreeWidget *ui;
};

```



<font color=#FE7207  size=4 face="幼圆">QDlgTreeWidget.cpp</font>

```c
#include "QDlgTreeWidget.h"
#include "ui_QDlgTreeWidget.h"

QDlgTreeWidget::QDlgTreeWidget(QWidget *parent)
	: QWidget(parent)
{
	ui = new Ui::QDlgTreeWidget();
	ui->setupUi(this);

	init();
	
	connect(ui->treeWidget, SIGNAL(itemChanged(QTreeWidgetItem*, int)), this, SLOT(onTreeItemChanged(QTreeWidgetItem*, int)));
}

QDlgTreeWidget::~QDlgTreeWidget()
{
	delete ui;
}

void QDlgTreeWidget::init()
{
	ui->treeWidget->clear();    //初始化树形控件
	ui->treeWidget->setHeaderHidden(true);  //隐藏表头


	//定义第一个树形组 爷爷项
	QTreeWidgetItem* group1 = new QTreeWidgetItem(ui->treeWidget);
	group1->setText(0, QStringLiteral("文件夹"));    //树形控件显示的文本信息
	group1->setFlags(Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsSelectable | Qt::ItemIsEditable);   //设置树形控件子项的属性
	group1->setCheckState(0, Qt::Unchecked); //初始状态没有被选中
											 //第一组子项
	QTreeWidgetItem* subItem11 = new QTreeWidgetItem(group1);
	subItem11->setFlags(Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsSelectable | Qt::ItemIsEditable);
	subItem11->setText(0, "subItem11");  //设置子项显示的文本
	subItem11->setCheckState(0, Qt::Unchecked); //设置子选项的显示格式和状态

	QTreeWidgetItem* subItem12 = new QTreeWidgetItem(group1);
	subItem12->setFlags(Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsSelectable | Qt::ItemIsEditable);
	subItem12->setText(0, "subItem12");
	subItem12->setCheckState(0, Qt::Unchecked);

	QTreeWidgetItem* subItem13 = new QTreeWidgetItem(group1);
	subItem13->setFlags(Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsSelectable | Qt::ItemIsEditable);
	subItem13->setText(0, "subItem13");
	subItem13->setCheckState(0, Qt::Unchecked);


	//父亲项
	QTreeWidgetItem* group2 = new QTreeWidgetItem(subItem13);
	group2->setText(0, "group2");
	group2->setFlags(Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsSelectable | Qt::ItemIsEditable);
	group2->setCheckState(0, Qt::Unchecked);

	QTreeWidgetItem* group3 = new QTreeWidgetItem(subItem13);
	group3->setText(0, "group3");
	group3->setFlags(Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsSelectable | Qt::ItemIsEditable);
	group3->setCheckState(0, Qt::Unchecked);

	//孙子项
	QTreeWidgetItem* subItem21 = new QTreeWidgetItem(group2);   //指定子项属于哪一个父项
	subItem21->setFlags(Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsSelectable | Qt::ItemIsEditable);
	subItem21->setText(0, "subItem21");
	subItem21->setCheckState(0, Qt::Unchecked);

	QTreeWidgetItem* subItem22 = new QTreeWidgetItem(group2);
	subItem22->setFlags(Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsSelectable | Qt::ItemIsEditable);
	subItem22->setText(0, "subItem22");
	subItem22->setCheckState(0, Qt::Unchecked);

	QTreeWidgetItem* subItem23 = new QTreeWidgetItem(group2);
	subItem23->setFlags(Qt::ItemIsUserCheckable | Qt::ItemIsEnabled | Qt::ItemIsSelectable | Qt::ItemIsEditable);
	subItem23->setText(0, "subItem23");
	subItem23->setCheckState(0, Qt::Unchecked);

	ui->treeWidget->expandAll();  //展开树

}

void QDlgTreeWidget::updateParentItem(QTreeWidgetItem* item)
{
	QTreeWidgetItem *parent = item->parent();
	if (parent == NULL)
		return;

	int nSelectedCount = 0;
	int childCount = parent->childCount();

	for (int i = 0; i < childCount; i++) //判断有多少个子项被选中
	{
		QTreeWidgetItem* childItem = parent->child(i);
		if (childItem->checkState(0) == Qt::Checked || childItem->checkState(0) == Qt::PartiallyChecked)
		{
			nSelectedCount++;
		}
	}

	if (nSelectedCount <= 0)  //如果没有子项被选中，父项设置为未选中状态
		parent->setCheckState(0, Qt::Unchecked);
	else if (nSelectedCount > 0 && nSelectedCount < childCount)    //如果有部分子项被选中，父项设置为部分选中状态，即用灰色显示
		parent->setCheckState(0, Qt::PartiallyChecked);
	else if (nSelectedCount == childCount)    //如果子项全部被选中，父项则设置为选中状态
		parent->setCheckState(0, Qt::Checked);

	updateParentItem(parent);

}

void QDlgTreeWidget::onTreeItemChanged(QTreeWidgetItem* item, int column)
{
	int count = item->childCount(); //返回子项的个数

	if (Qt::Checked == item->checkState(0))
	{
		if (count > 0)
		{
			for (int i = 0; i < count; i++)
				item->child(i)->setCheckState(0, Qt::Checked);
		}
		else
			updateParentItem(item);
	}
	else if (Qt::Unchecked == item->checkState(0))
	{
		if (count > 0)
		{
			for (int i = 0; i < count; i++)
				item->child(i)->setCheckState(0, Qt::Unchecked);
		}
		else
			updateParentItem(item);
	}
}

```



## 细节方面：

其中主要是参考下面好儿郎的这一篇博客，在[Qt: 创建具有复选框的树形控件](https://blog.csdn.net/rl529014/article/details/51355968) 的一文中，其中主要修改如下：

`void Widget::updateParentItem(QTreeWidgetItem\* item)` 函数的最后一行添加如下一句话，`updateParentItem(parent)`，是在在函数里面添加一个迭代函数，（向自己的父节点进行遍历）；

且在判断子项中个数被选中，将`if (childItem->checkState(0) == Qt::Checked)`修改为`if (childItem->checkState(0) == Qt::Checked || childItem->checkState(0) == Qt::PartiallyChecked)`，半选中也算在父节点也算选中



<font color=#D0087E size=4 face="幼圆">当看到运行成功那的一刻，心里或许就是这样的</font>

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/gif/20190704175742.gif)









## **参考博文：** 

[Qt: 创建具有复选框的树形控件](https://blog.csdn.net/rl529014/article/details/51355968) （推荐）

[建立QTreeWidget下QTreeWidgetItem的右键菜单](https://blog.csdn.net/fengyutongtt/article/details/52372164) （推荐）

 