---
title: QStyle/DTK重绘Qt-GUI已有控件,举例QScrollBar
date: 2020-02-27 15:28:55
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
---



　　**简  述：**　使用 QStyle / DTK 来实现重绘 Qt-GUI 已有的控件，此处以重绘 QScrollBar 举例。

<!-- more -->

[TOC]

### 编程环境：

　　**💻：**  `MacOS10.14 (18G103)` 📎 `Qt 5.12.6`

　　**💻：**  `win10 x64 专业版 (1803)` 📎 `Qt 5.9.8`

　　**💻：**  `uos20 amd64 专业版 (Kernel 4.19)` 📎 `Qt 5.11.3`

<br>

### 背景：

　　由[上一篇: QStyle设置界面的外观和QCommonStyle继承关系图讲解和使用](https://blog.csdn.net/qq_33154343/article/details/104367878) 的文章已经可理解 `QStyle` 、 `QCommonStyle` 和自定义的 `MyStyle` 三者之间的关系；对于基础的风格控件也有所学习了。本篇承接上文，开始尝试，自己绘画一个新的风格（样式）的控件，为了本篇的简洁，这里就以 `QScrollBar` 为例。

<br>

### 查看 QScrollBar 的构成，如何查找源码？

　　想要绘画某一个 GUI 控件，必须了解它是由那些小部件组成的，换言之，在前面的重载函数里面，需要的指导重绘哪一些枚举参数❓

　　此处提供另外一篇比的先写的重绘文章: [QStyle自定义重绘QScrollBar](https://blog.csdn.net/qq_33154343/article/details/100943187)，里面是从另外一个角度来解析 `QScrollBar` 控件的构成；可以结合着来看；

<br>

### **解析绘画控件的步骤：**

1. **打开 QtCreator 此 IDE，在自己的左侧的工程项目中，再打开qt-core--gui-base 源码的工程，便于查找和🔍所需的源码**

   `src` 工程打开，只是为了便于在下面搜索框里面查找需要的 qt 源码文件所在的路径在 `/Users/muli/Qt5.12.6/5.12.6/Src/qtbase/src/src.pro`；

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200227_142504.png"  />

   

   win 系统下的路径也差不多；Linux 系统路径则是在自定义安装 qt 源码地方（**命令安装**）或者默认安装的地方（***.run安装** ）。

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200227_142952.png" width="50%" />

   <br>

2. **搜索 qcommonstyle.cpp 这个文件，查看缺省风格是如何写成的**

   通过查看 qcommonstyle.cpp 这个文件，来🔍  `QScrollBar` 在默认缺省风格下面的的绘画方式；其中参考最多的就是 `qcommonstyle` 风格，在某些情况下也可以参考 `qfusionstyle` 风格的绘画思路；两者可以思路互相印衬，得到聊暗花明又一村的柑橘；

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200227_143821.png" width="50%" />

   <br>

3. **提取关键元素枚举**

   打开上面的文件，点击 command + F 进行🔍 `ScrooBar` 这个单词，将所有的枚举都绘画记录下来；用来重构这个元素

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200227_144929.png" width="80%" />

   

   找到对应的 

   CC_ScrollBar， CC_ScrollBar， SC_ScrollBarSubLine ， SC_ScrollBarAddLine， SC_ScrollBarSubPage， SC_ScrollBarAddPage， SC_ScrollBarGroove， SC_ScrollBarSlider，PM_ScrollBarSliderMin， PM_ScrollBarExtent， SH_ScrollBar_ContextMenu， SH_ScrollBar_RollBetweenButtons， SH_ScrollBar_Transient

   这些枚举；初看，这么多的枚举貌似还挺多（划掉貌似）；可以按照你想完成的工足量，其中有些是重要的，属于必不可少；有些是不那么重要的，可以先不用重绘使用到的；**要能按照 CC_ ，SC_， 来组装该控件，组装完成发现各自带边的部分如下；** 

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910224200.png" style="zoom: 67%;" />

   <br>

4. **找到对应的重载函数，开始重写此虚函数**

   为了比较简单的写出这个🌰，简化处理；只需重新绘画里面的五个枚举，顺着 `CE_` 的作为参数，逆向推函数，发现需要只要调用 `drawControl()` 即可。

   ```cpp
   virtual void drawControl(ControlElement element, const QStyleOption *opt, QPainter *p, const QWidget *w) const override;
   };
   ```

   

   <br>

5. **按照 风格 + 需求，来进行绘画**

   这里重绘代码如下：

   ```cpp
   void MyStyle::drawControl(QStyle::ControlElement element, const QStyleOption *opt, QPainter *p, const QWidget *w) const
   {
       p->setRenderHint(QPainter::Antialiasing);
   
       switch (element) {
       case CE_ScrollBarAddPage: {  //增加滑槽
           p->fillRect(opt->rect, QColor("#bfe9ff"));
           return;
       }
       case CE_ScrollBarSubPage: {  //减少滑槽
           p->fillRect(opt->rect, QColor("#EC6EAD"));
           return;
       }
       case CE_ScrollBarSlider: {  //滑块
           p->fillRect(opt->rect, QColor("#A8BFFF"));
           return;
       }
       case CE_ScrollBarAddLine: {  //增加按钮
           p->fillRect(opt->rect, QColor("#21d4fd"));
           return;
       }
       case CE_ScrollBarSubLine: {  //减少按钮
           p->fillRect(opt->rect, QColor("#de6161"));
           return;
       }
       default:
           break;
       }
   
       return QCommonStyle::drawControl(element, opt, p, w);
   }
   ```

   <br>

6. **查看最终效果，若是成功，则项目成功结束🔚**

   这里这里最后的效果如下，其同一个控件在 MacOS 和 Linux(uos20) 下面的表现样式是一样的；可以见其已经绘画成功.

   <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/Snip20200227_152044.png" width="100%" />

   

### 自定义风格完整代码如下：

　　回顾一下代码和绘画流程；创建一个自定义的风格 `class MyStyle : public QCommonStyle`，其中选择自己需要的继承的函数进行重写；然后重写 虚函数。



**MyStyle：MyStyle 的完整声明：**

```cpp
#ifndef MYSTYLE_H
#define MYSTYLE_H
#include <QCommonStyle>

class MyStyle : public QCommonStyle
{
public:
    MyStyle();

    // QStyle interface
public:
    virtual void drawControl(ControlElement element, const QStyleOption *opt, QPainter *p, const QWidget *w) const override;
};

#endif // MYSTYLE_H
```

<br>

### 项目分析：

　　这里，对整个简单的项目进行分析一下，为后面绘画一个复杂的 Qt 没有的控件做准备，也就是绘画自定义控件；在这里再次梳理一下，下一篇提升就是在此文章基础上进行绘画重写。

- **QtStyle**
  - **QStyleEx.pro：** 用 qmake 来生成的 makefile 文件的配置文件
  - myshtyle.h: 自定义风格的类的声明
  - myshtyle.cpp: 自定义风格的类的定义
  - widget.h: 用来衬托 `ScrollBar` 的背景控件类的声明
  - widget.cpp: 用来衬托 `ScrollBar` 的背景控件类的定义
  - main.cpp: 整个项目启动的入口

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img02/mark_Snip20200227_153044.png" width="65%" />

<br>

### 源码下载：

[https://github.com/xmuli/QtExamples](https://github.com/xmuli/QtExamples)【QtMyStyleEx/QtExample02/QtStyleEx】

<br>

> <font color=#D0087E  size=4 face="幼圆">**📌本博文系列的目录:** </font> [QtExamples 系列的目录](https://blog.csdn.net/qq_33154343/article/details/100148539)





