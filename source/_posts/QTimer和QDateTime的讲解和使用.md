---
title: QTimer和QDateTime的讲解和使用
date: 2019-9-20 00:01:06
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt控件
---



**简介：**  讲解`QTimer` 定时器（不可见控件）和 `QDateTime`日期时间的控件

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `visual studio 2015`， `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## 运行效果：

先上一个最终的运行效果图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190918202243.gif"/>

<br>

## 时间日期相关的类：

**时间日期是经常遇到的数据类型，Qt中时间日期类型的类如下。**

- **QTime**：时间数据类型，仅表示时间，如15:23：13。
- **QDate**：日期数据类型，仅表示日期，如2017-4-5。
- **QDateTime**:日期时间数据类型，表示日期和时间，如2017-03-2308：12:43。

<br>

**Qt中有专门用于日期、时间编辑和显示的界面组件，介绍如下。**

- **QTimeEdit**:编辑和显示时间的组件。
- **QDateEdit**:编辑和显示日期的组件。
- **QDateTimeEdit**：编辑和显示日期时间的组件。
- **OCalendarWidget**:一个用日历形式选择日期的组件。

<br>

## QDateTimeEdit时间属性:

|                属性                |                             含义                             |
| :--------------------------------: | :----------------------------------------------------------: |
|              datetime              |                           日期时间                           |
|                date                |                             日期                             |
|                time                |                             时间                             |
| maximumDate Time、minimumDate Time |                        最大、最小日期                        |
|      maximumDate、minimumDate      |                        最大、最小时间                        |
|           currentSection           | 当前输入光标所在的时间日期数据段，是枚举类型QDateTimeEdit：:Section |
|           QDateTimeEdit            | 显示日期时间数据时分为多个段，单击编辑框右侧的上下按钮可修改当前段的值 |
|        currentSectionIndex         |                 用序号表示的输入光标所在的段                 |
|           calendarPopup            |                  是否允许弹出一个日历选择框                  |
|           displayFormat            |               显示格式，日期时间数据的显示格式               |

**date**和**time**设置其中一个，就会自动修改其中另外一个。

**QDateTimeEdit**：如输入光标在YearSection段，就修改“年”的值。

**calendarPopup**：当取值为true时，右侧的输入按钮变成与QComboBox类似的下拉按钮，单击按钮时出现一个日历选择框，用于在日历上选择日期。对于QTimeEdit，此属性无效。

**displayFormat**：例如设置为“yyyy-MM-dd HH：mm：ss”，一个日期时间数据就显示为“2016-11-0208:23：46”

<br>

## QTimer属性：

定时器是用来处理周期性事件的一种对象，类似于硬件定时器。例如设置一个定时器的定时。周期为1000毫秒，那么每1000毫秒就会发射定时器的timeout）信号，在信号关联的槽函数里就，可以做相应的处理。Qt中的定时器类是QTimer，它直接从QObject类继承而来，不是界面组件类。

```cpp
QTimer* m_timer;     //定时器（不可见控件）
QTime   m_time;      //计时器（此处用作）
```

<br>

## DateTime转QString：

```cpp
//获取当前时间
QDateTime currDateTime = QDateTime::currentDateTime();

//QString转为DateTime
ui->editDateTime->setText(currDateTime.toString("yyyy-MM-dd hh:mm:ss:zzz"));

//DateTime转为QString
ui->labCurrDataTime->setText(currDateTime.toString("yyyy-MM-dd hh:mm:ss:zzz"));
```

<br>

## 常用日期显示格式：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190920000600.png"/>

<br>

## 核心源码：

```cpp
//editDate控件在UI设计师里面，选中了calendarPopup （日历弹出的属性）和displayFormat显示格式

    m_timer = new QTimer(this);
    m_timer->stop();                       //关闭定时器
    m_timer->setInterval(1000);            //设定定时周期， 单位 毫秒
    connect(m_timer, SIGNAL(timeout()), this, SLOT(onTimerOut()));

//+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//获取当前日期和时间，该日期时间栏里面显示
void ExDateTime::on_btnGetDateTime_clicked()
{
    QDateTime currDateTime = QDateTime::currentDateTime();
    ui->timeEdit->setTime(currDateTime.time());
    ui->editTime->setText(currDateTime.toString("hh:mm:ss:zzz"));
    ui->dateEdit->setDate(currDateTime.date());
    ui->editDate->setText(currDateTime.toString("yyyy-MM-dd"));
    ui->dateTimeEdit->setDateTime(currDateTime);
    ui->editDateTime->setText(currDateTime.toString("yyyy-MM-dd hh:mm:ss:zzz"));
    ui->labCurrDataTime->setText(currDateTime.toString("yyyy-MM-dd hh:mm:ss:zzz"));
}

//+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//计时器开始
void ExDateTime::on_btnStatrt_clicked()
{
    m_time.start();                                 //计时器开始
    m_timer->start();                               //定时器开始
    ui->btnStatrt->setEnabled(false);               //开始按下之后，开始按钮禁用
    ui->btnStop->setEnabled(true);                  //同时结束按钮可用
    ui->btnPeriod->setEnabled(false);               //设定周期的按钮为禁用
    ui->labGo->setText(QString("时间流逝在后台计算中..."));
}

//计时器结束
void ExDateTime::on_btnStop_clicked()
{
    m_timer->stop();                                 //定时器停止
    ui->btnStop->setEnabled(false);                  //结束按下之后，结束按钮禁用
    ui->btnStatrt->setEnabled(true);                 //同时开始按钮可用

    int tmMsec = m_time.elapsed();                   //计时器Time没有对应的stop(), elapsed获取它的毫秒数
    int ms = tmMsec % 1000;                          //经过的毫秒
    int sec = tmMsec / 1000;                         //经过的秒
    ui->btnPeriod->setEnabled(true);                 //设定周期的按钮为可用
    ui->labGo->setText(QString("时间已经流逝：%1 秒 %2 毫秒").arg(sec).arg(ms));
}

//处理定时器的槽函数
void ExDateTime::onTimerOut()
{
    QTime currTime = QTime::currentTime();
    ui->lcdHH->display(currTime.hour());             //多种显示时间方法
    ui->lcdmm->display(currTime.toString("mm"));
    ui->lcdSS->display(currTime.toString("ss"));

    int val = ui->progressBar->value();              //设置进度条同时增加
    val++;
    if (val > 100)
        val = 0;
    ui->progressBar->setValue(val);
}

//+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//选择日历时间
void ExDateTime::on_calendarWidget_selectionChanged()
{
    QDate date =ui->calendarWidget->selectedDate();
    ui->editChoose->setText(date.toString("yyyy年MM月dd日"));
}

//设定定时器QTimer周期
void ExDateTime::on_btnPeriod_clicked()
{
    m_timer->setInterval(ui->spinBox->value());
}
```

<br>

## 源码下载：

[https://github.com/xmuli/QtExamples](https://github.com/xmuli/QtExamples)【QtDateTimeEx】

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>