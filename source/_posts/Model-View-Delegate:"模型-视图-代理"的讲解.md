---
title: Model-View-Delegate:"模型-视图-代理"的讲解
date: 2019-12-12 00:21:06
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



**简  述：**  在Qt中，有一种将数据和和视图进行串通起来，就像网页和数据库的关系一样；而这就是**“Model-View-Delegate”**（**模型-视图-代理**）的结构。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-12-09_22-34-00_mark.png"/>

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [Model-View-Delegate:"模型-视图-代理"的讲解](https://blog.csdn.net/qq_33154343/article/details/103501667)

<br>

## Model/View(模型/视图):

在GUI界面交互的时候，用户在（软件/网页的）界面进行查看数据的时候，看到这个这个就被称之为**视图**；而视图展示出来的数据，是从**模型**里面取出来的，你可以将它看做看做一个专门存放数据的容器（黑匣子）；

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-12-10_23-53-09_mark.png"/>

<br>

### 数据（Data）：

实际的数据，比如是数据表里面的一个项，也或许是一个QStringList，也许只是一个数字或者字母或者字符串:abc:

<br>

### 视图（或视图组件 View）：

客户端的界面，用来查看数据的控件，比如表格Table或者树形图目录结构Tree；**视图从数据模型获取得到每一个数据项的模型索引（modelindex），然后通过模型索引获取数据；** Qt提供一些现成的数据模型视图组件：  

- **主要的数据模型：** QListView / QTreeView / QTableView

-  **其对应的简便利类：**  QListWidget / QTreeWidget / QTableWidget

<br>

### 模型（数据模型 Model）:

与实际数据数据进行同行，并且为视图组件提供数据结构

<br>

## 代理(Delegate):

代理是可以让用户，自己更加细致定义绘画自己所需要的窗口的；比如说，可以在一个Table的控件里面，将一个item的项替换成一个QCheckBox或者一个下拉列表表框的控件之类的（eg：让你填表时候，选择年龄等）

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-12-09_23-03-07_mark.png"/>

<font color=#D0087E size=4 face="幼圆">由于数据和模型实际相互分离开，所以是**可以一个数据模型对应⇒多个数据模型**的</font>

<br>

## 数据模型：

所有基于**项数据**（item Data）的**数据模型**（Model）都是基于`QAbstractItemModel`这个抽象类的。

其几个主要的类的层次结构如下：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-12-10_23-39-22_mark.png"/>

其中：

- `QStringList Model：`用于处理字符串列表数据的数据模型类
- `QStandardltem Model：`标准的基于项数据的数据模型类，每个数据项数据都可以是任意数据类型
- `QFileSystem Model：`计算机上文件系统的数据模型类
- `QSortFilterProxy Model：` 用于其他数据模型的结合，提供排序和过滤功能的数据模型类
- `QSqlQuery Model:` 用于数据库SQL查询结果的数据模型类
- `QSqlTable Model:`  用于数据库的一个数据表的数据模型类
- `QSqlRelationalTable Model：`用于关系型数据表的数据模型类

<br>

## 视图模型：

其中它们的关系如下，在使用的时候，往往只是需要调用setModel()，就可以实现一个数据模型和视图组件和数据模型之间的关联；**对视图组件的数据修改，将会直到自动保存到关联的数据模型里面，且一个数据模型是可以在多个视图组件里面显示数据的。**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/Snipaste_2019-12-10_23-44-41_mark.png"/>

其中的详细说明如下：

- `QListView:` 用于显示单列的列表数据模型，适用于一维数据的操作
- `QTreeView:`  用于显示树状结构，使用与树状结构数据的操作
- `QTableView:`  用于显示表格状数据，适用于二维型数据的操作
- `QColumnView:`  用于多个QListView显示树状层次结构，树状结构的一层用一个QLIstView显示
- `QHeaderView:`  提供行表头或者列表头的视图组件

<br>

## 模型索引：

**模型索引（Model Index）**：是来保证数据和其存取方式的隔离。

`QModeIndex`是（临时的）一个索引类。模型索引提供数据存取的一个临时指针，用于通过数据模型的提取或者修改，因为模型内部组织结构的数据很容易变化，所以这个模型索引是临时的；

`QPersistentModeIndex`是持久性的模型索引。

