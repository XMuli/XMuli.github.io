---
title: 应用软件在「ous20和MAcOS10.14下」显示应用不同的QStyle「即：所有控件的样式换肤」
date: 2020-02-13 23:08:07
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - QStyle
---



**简  述：**  应用软件在「`ous20 和 MAcOS10.14` 下」显示应用不同的`QStyle`「即：所有控件的样式换肤」， 自定义风格 **QStyle**：显示当前 **OS** 的所有支持的风格Style；

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [应用软件在「ous20和MAcOS10.14下」显示应用不同的QStyle「即：所有控件的样式换肤」](https://blog.csdn.net/qq_33154343/article/details/104305154)

<br>

## 起初背景：

创建一个简单地🌰「Qt 的工程项目」，在 uos20 和 MacOS10.14.6 系统上面都跑一边，简单的摆放几个控件在窗口上面，用来查看使用不同的 Style 的时候，用来表现不同的效果，给人以以直观的**软件换肤** 感受。

<br>

## QStyleFactory 和 QStyle 讲解：

QStyleFactory:看做很多风格模式的集合

QStyle：看做具体操作系统的里面有几种默认的风格

这次的源码讲解比较少，其中主要就是这一段 `QStyleFactory` 和 `qApp->setStyle()`的使用

```cpp
//查看当前 OS 下的所有支持风格
QStringList listStyle = QStyleFactory::keys();
foreach(QString val, listStyle)
  qDebug()<<val<<"  ";

//若为 OS 自带的QStyle，则
qApp->setStyle("系统自带风格");

//若为自定义新的QStyle，则
qApp->setStyle(QStyleFactory::create("自定义风格"）)
```



- 在 UOS20 下，该系统支持的风格如下：
  - "chameleon"   
  - "dsemilight"   
  - "dsemidark"   
  - "dlight"   
  - "ddark"   
  - "Windows"   
  - "Fusion"   



- 在 MacOS10.14.6 下，该系统支持的风格如下：
  - "macintosh"   
  - "Windows"   
  - "Fusion"   

<br>

## 改变 OS 高亮色（活动色）：

在 MacOS 和 uos(deepin) 里面是可以改变系统的**高亮色** 「有时候也被称之为**活动色** 」，而默认一般都是蓝色的，且 windows 里面也是在设置里面修改。另外两个其修改设置的地方如下图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_235035.png"/>



<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_235430.png"/>

## 运行效果：

先上一个最终的运行效果图：

### 在 UOS20 下效果图：

#### chameleon:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_230443.png"/>

#### dsemilight:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_230504.png"/>

#### dsemidark:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_230544.png"/>

#### dlight:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_230603.png"/>

#### ddark:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_230617.png"/>

#### Windows:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_230633.png"/>

#### Fusion:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_230646.png"/>

### 在 MacOS10.14.6 下效果图：

#### macintosh:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_233813.png"/>

#### Windows:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_233823.png"/>

#### Fusion:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200213_233830.png"/>

<br>

## 核心源码：

```cpp
void Widget::init()
{
    QTableWidget *table = new QTableWidget(10, 10, this);
    table->move(10, 10);

    QScrollBar *scrollBarH = new QScrollBar(this);
    scrollBarH->move(300, 50);
    scrollBarH->setRange(0, 100);
    scrollBarH->setValue(34);
    scrollBarH->resize(380, 20);
    scrollBarH->setOrientation(Qt::Horizontal);

    QScrollBar *scrollBarV = new QScrollBar(this);
    scrollBarV->move(50, 250);
    scrollBarV->setRange(0, 100);
    scrollBarV->setValue(67);
    scrollBarV->resize(20, 380);
    scrollBarV->setOrientation(Qt::Vertical);

    QProgressBar* progreH = new QProgressBar(this);
    progreH->move(300, 100);
    progreH->resize(250, 40);
    progreH->setValue(37);
    progreH->setOrientation(Qt::Horizontal);

    QProgressBar* progreV = new QProgressBar(this);
    progreV->move(100, 300);
    progreV->resize(40, 250);
    progreV->setValue(67);
    progreV->setOrientation(Qt::Vertical);

    int i = 0;
    QStringList listStyle = QStyleFactory::keys();
    foreach(QString val, listStyle) {   //打印当前系统支持的系统风格
        qDebug()<<val<<"  ";
        QPushButton *btn = new QPushButton(val, this);
        btn->move(this->rect().right() - 100, this->rect().top() + i++ * 40);
        connect(btn, &QPushButton::clicked, this, [=](){
            qApp->setStyle(val);
        });
    }

}
```

<br>

## 源码下载：

[https://github.com/touwoyimuli/QtExamples](https://github.com/touwoyimuli/QtExamples) 【QtMyStyleEx/QtExample01/QtStyleEx】