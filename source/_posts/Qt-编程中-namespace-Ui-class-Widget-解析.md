---
title: Qt 编程中 namespace Ui { class Widget; } 解析
date: 2019-8-1 22:53:44
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - qt]
 - [学习 - 底层原理、思想架构]
tags: 
 - c/c++
 - qt
 - 原理、架构
---



**简介：** Qt 编程中 `namespace Ui { class Widget; }` 解析的原理

<!-- more -->

[TOC]



## 本博文的简述or解决问题？

​		 Qt 编程中 namespace Ui { class Widget; } 解析

```cpp
 namespace Ui 
{ 
	class widget; 
} 
```

的原理，和它背后的东东。

<br>

**编程环境：**  `win10 x64 专业版 1803`  操作系统版本：`17134.285` 

**编程软件：**  `visual studio 2015`， `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">重要提示：</font>

- 若遇`csdn`的博文排版、文字、图片、链接、视频预览等异常，会删除该部分，或用链接代替，或删除该部分，但在文末 [github.io](https://touwoyimuli.github.io/) 中的同步文章，会保证显示正确
- <font color=#D0087E  size=4 face="幼圆">**推荐<font color=#FE7207  size=4 face="幼圆">本文末的同步链接</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好的100%效果体验**</font> 

<br>

# 理解一：

简述：

Qt编程中，会见到类似于如下的声明：

```cpp
namespace Ui 
{ 
	class Dialog; 
} 
```

那么，为何要这样声明，这样声明有什么好处。

这是Designer使用了pimpl手法，pImpl手法主要作用是解开类的使用接口和实现的耦合，即为了减少各个源文件之间的联系。

下面详细讲解一下。



## 1、新建Qt 设计师界面类

### dialog.h

```cpp
#ifndef DIALOG_H 
#define DIALOG_H 
   
#include <QtGui/QDialog> 
   
namespace Ui 
{ 
	class Dialog; 
} 
  
class Dialog : public QDialog 
{ 
	Q_OBJECT 
      
public: 
	Dialog(QWidget *parent = 0); 
	~Dialog(); 
      
private: 
	Ui::Dialog *ui; 
}; 
  
#endif // DIALOG_H 
```



### dialog.cpp

```cpp
#include "dialog.h" 
#include "ui_dialog.h" 
 
Dialog::Dialog(QWidget *parent) 
	: QDialog(parent), ui(new Ui::Dialog) 
{ 
	ui->setupUi(this); 
	//QObject::connect(ui->pushButton, SIGNAL(clicked()),this, SLOT(quit())); 
} 
 
Dialog::~Dialog() 
{ 
	delete ui; 
}
```



### ui_dialog.h

```cpp
#ifndef UI_DIALOG_H 
#define UI_DIALOG_H 
 
#include <QtCore/QVariant> 
#include <QtGui/QAction> 
#include <QtGui/QApplication> 
#include <QtGui/QButtonGroup> 
#include <QtGui/QDialog> 
#include <QtGui/QHeaderView> 
#include <QtGui/QListView> 
#include <QtGui/QPushButton> 
	
QT_BEGIN_NAMESPACE 
 
class Ui_Dialog 
{
public: 
	QListView *listView; 
	QPushButton *pushButton
	           
	            
	void setupUi(QDialog *Dialog) 
	{
		if (Dialog->objectName().isEmpty()) 
			Dialog->setObjectName(QString::fromUtf8("Dialog")); 
		Dialog->resize(600, 400); 
		listView = new QListView(Dialog); 
		listView->setObjectName(QString::fromUtf8("listView")); 
		listView->setGeometry(QRect(30, 10, 256, 192)); 
		pushButton = new QPushButton(Dialog); 
		pushButton->setObjectName(QString::fromUtf8("pushButton")); 
		pushButton->setGeometry(QRect(140, 280, 75, 23));
	         
		retranslateUi(Dialog);
	  
		QMetaObject::connectSlotsByName(Dialog);
	} // setupUi
	   
	void retranslateUi(QDialog *Dialog) 
	{ 
		Dialog->setWindowTitle(QApplication::translate("Dialog", "Dialog", 0, QApplication::UnicodeUTF8)); 
		pushButton->setText(QApplication::translate("Dialog", "bye", 0, QApplication::UnicodeUTF8)); 
		Q_UNUSED(Dialog); 
	} // retranslateUi 
	    
}; 
 
namespace Ui { 
	class Dialog: public Ui_Dialog {}; 
} // namespace Ui 
 
QT_END_NAMESPACE 
 
#endif // UI_DIALOG_H 
```



##   2、分析代码

**1 > ui_dialog.h代码中有很多被硬编码的地方。**

```cpp
listView->setGeometry(QRect(30, 10, 256, 192)); 
pushButton = new QPushButton(Dialog); 
pushButton->setObjectName(QString::fromUtf8("pushButton")); 
pushButton->setGeometry(QRect(140, 280, 75, 23));
```



Designer生成的这个东西， 如何让程序的其他代码去使用呢？
最直接的， 它应该产生一个

```cpp
class Ui_Dialog {
    QListView *listView; 
    QPushButton *pushButton;
};
```



这样的类去让其他代码使用：

```cpp
// My.h
#include "ui_dialog.h"
 
class My
{
	Ui_Dialog dlg;
};
 
// My.cpp
#include "My.h"
// 实现My
```



**但是这样存在问题， 如果ui_dialog.h文件的内容被改变，不但My.cpp会被重新编译，**
**所有包含My.h的文件也都会被重新编译。**
**而且这确实是一个问题： Designer确实经常被拖来拖去**



**2 > 如果产生ui_dialog.h的那个程序能将如下代码：**

```cpp
listView->setGeometry(QRect(30, 10, 256, 192)); 
pushButton = new QPushButton(Dialog); 
pushButton->setObjectName(QString::fromUtf8("pushButton")); 
namespace Ui  { 
    class Dialog; 
} 
 
class Dialog : public QDialog  { 
    Ui:: Dialog *ui;  // 使用该类的一个指针
}; 
 
 
pushButton->setGeometry(QRect(140, 280, 75, 23)); 
```

移动到一个ui_dialog.cpp 中， 至少在移动dlg上的那些界面元素时，只会重新编译ui_dialog.cpp。
不会修改ui_dialog.h， 也就不会引发另一连串重编译。

**3 > 但是， 除了将界面元素拖来拖去， Designer还经常干的一些事就是添加，删除一些界面元素，如**

```cpp
class Ui_Dialog 
{ 
public: 
    QListView *listView; 
    QPushButton *pushButton;
    // ...
};
```



**4 > 如何让Designer改变GUI外观后， 不会引发工程大范围的重新编译？**

所以Designer使用了pimpl手法 ……

前置声明一个 Ui:: Dialog类

```cpp
namespace Ui  { 
    class Dialog; 
} 
 
class Dialog : public QDialog  { 
    Ui:: Dialog *ui;  // 使用该类的一个指针
}; 
```

然后用户使用 dialog.h 头文件以及 Dialog类。
该文件被修改的频率就会低很多很多。



无论是将designer上的界面元素拖来拖去， 还是添加删除， dialog.h文件的内容——Dialog类的定义——都不会改变。
然后用户可以使用这个Dialog了：

```cpp
#include "dialog.h"
class My 
{
    Dialog dlg;
};
```

## 3、Ui 创建两种不同的方式

**1 > 在qt4中使用了继承的方式来使用designer创建的窗体，也就是同时继承QDialog和UI_Dialog。**

**2 > 而在Qt Creator自动创建的项目中，使用了组合的方式来使用Designer创建的窗体，就是集成QDialog，而将UI_Dialog作为一个成员变量来使用，也就是**

```cpp
private:
   Ui::Dialog *ui; 
```



**区别：**
**在前一种方式中，你可以在继承类中直接使用UI_Dialog上的组件。**
**在后一种方式中，你要使用ui->XXX的方式使用UI_Dialog上的组件。** 
**两种方式都可以，但个人感觉第二种好一些，毕竟组合比继承的耦合度来的弱一些，就是稍有点麻烦，要加ui->，但同时也带来了更清晰的代码结构。**



## 4、pImpl的原理

**1 > pImpl惯用手法的主要作用是解开类的使用接口和实现的耦合。**
**如果不使用pImpl惯用手法，代码会像这样**



```cpp
#include<x.hpp>
class C
{
public:
	void f1();
private:
	X x; //与X的强耦合
};
```



像上面这样的代码，C与它的实现就是强耦合的，从语义上说，x成员数据是属于C的实现部分，不应该暴露给用户。从语言的本质上来说，在用户的代码中，每一次使用”new C”和”C c1”这样的语句，都会将X的大小硬编码到编译后的二进制代码段中（如果X有虚函数，则还不止这些）——这是因为，对于”new C”这样的语句，其实相当于operator new(sizeof(C))后面再跟上C的构造函数，而”C c1”则是在当前栈上腾出sizeof(C)大小的空间，然后调用C的构造函数。因此，每次X类作了改动，使用c.hpp的源文件都必须重新编译一次，因为X的大小可能改变了。
在一个大型的项目中，这种耦合可能会对build时间产生相当大的影响。

**pImpl惯用手法可以将这种耦合消除，使用pImpl惯用手法的代码像这样：**

```cpp
//c.hpp
class X; //用前导声明取代include
class C
{
	...
	private:
	X* pImpl; //声明一个X*的时候，class X不用完全定义
};
```

**2 > 在一个既定平台上，任何指针的大小都是相同的。之所以分为X*，Y*这些各种各样的指针，主要是提供一个高层的抽象语义，即该指针到底指向的是那个类的对象，并且，也给编译器一个指示，从而能够正确的对用户进行的操作（如调用X的成员函数）决议并检查。但是，如果从运行期的角度来说，每种指针都只不过是个32位的长整型（如果在64位机器上则是64位，根据当前硬件而定）。**




正由于**pImpl是个指针**，所以这里X的二进制信息（sizeof(C)等）不会被耦合到C的使用接口上去，也就是说，当用户”new C”或”C c1”的时候，编译器生成的代码中不会掺杂X的任何信息，并且当用户使用C的时候，使用的是C的接口，也与X无关，从而X被这个指针彻底的与用户隔绝开来。只有C知道并能够操作pImpl成员指向的X对象。



**3 > 防火墙**
“修改X的定义会导致所有使用C的源文件重新编译”这种事就好比“城门失火，殃及池鱼”，其原因是“护城河”离“城门”太近了（耦合）。


pImpl惯用手法又被成为“编译期防火墙”，什么是“防火墙”，指针？不是。C++的编译模式为“分离式编译”，即不同的源文件是分开编译的。也就是说，不同的源文件之间有一道天然的防火墙，一个源文件“失火”并不会影响到另一个源文件。
但是，这里我们考虑的是**头文件**，如果头文件“失火”又当如何呢？头文件是不能直接编译的，它包含于源文件中，并作为源文件的一部分被一起编译。


这也就是说，如果源文件S.cpp使用了C.hpp，那么class C的（接口部分的）变动将无可避免的导致S.CPP的重新编译。但是作为class C的实现部分的class X却完全不应该导致S.cpp的重新编译。
因此，我们需要把class X隔绝在C.hpp之外。这样，每个使用class C的源文件都与class X隔离开来（与class X不在同一个编译单元）。但是，既然class C使用了class X的对象来作为它的实现部分，就无可避免的要“依赖”于class X。只不过，这个“依赖”应该被描述为：“class C的实现部分依赖于class X”，而不应该是“class C的用户使用接口部分依赖于class X”。


如果我们直接将X的对象写在class C的数据成员里面，则显而易见，使用class C的用户“看到”了不该“看到”的东西——class X——它们之间产生了耦合。然而，如果使用一个指向class X的指针，就可以将X的二进制信息“推”到class C的实现文件中去，在那里，我们#include”x.hpp”，定义所有的成员函数，并依赖于X的实现，这都无所谓，因为C的实现本来就依赖于X，重要的是：此时class X的改动只会导致class C的实现文件重新编译，而用户使用class C的源文件则安然无恙！
指针在这里充当了一座桥。将依赖信息“推”到了另一个编译单元，与用户隔绝开来。而防火墙是C++编译器的固有属性。



**4 > 穿越C++编译期防火墙**

是什么穿越了C++编译期防火墙？是指针！使用指针的源文件“知道”指针所指的是什么对象，但是不必直接“看到”那个对象——它可能在另一个编译单元，是指针穿越了编译期防火墙，连接到了那个对象。
从某种意义上说，只要是代表地址的符号都能够穿越C++编译期防火墙，而代表结构(constructs)的符号则不能。
例如函数名，它指的是函数代码的始地址，所以，函数能够声明在一个编译单元，但定义在另一个编译单元，编译器会负责将它们连接起来。用户只要得到函数的声明就可以使用它。而类则不同，类名代表的是一个语言结构，使用类，必须知道类的定义，否则无法生成二进制代码。变量的符号实质上也是地址，但是使用变量一般需要变量的定义，而使用extern修饰符则可以将变量的定义置于另一个编译单元中。



//+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# 理解二：

## Qt 编程中 namespace Ui { class Widget; } 解析

class Widget 里面有个声明 Ui::Widget *ui，这个 ui 是使用 namespace Ui 里的 Widget 类声明的，该类只是简单的继承了 ui_widget.h 里的 Ui_Widget 类（没有添加任何成员）。现在就很清楚了，这两个看起来名字一样的 Widget 其实是两个类，一个是 namespace Ui 里的，另一个是 namespace Ui 之外的 Widget 类，namespace 声明的类其实就是个空壳，它的基类功能是将此窗口上的所有控件的声明、实例化、初始化。声明的原因就是为了使 ui 布局控制和其他的控制代码分离。

## 用一段 C++ 代码来说明这一切：

test.h 文件内容：

```cpp
#ifndef _TEST_H_
#define _TEST_H_

#include <iostream>

class Test{
public:
    void display(){
        std::cout << "This is a test!(no namespace)" << std::endl;
    }
};

class Base{
public:
    void display(){
        std::cout << "This is a test!(namespace)" << std::endl;
    }
};

/* 使用 namespace 声明 */
namespace UI {
    class Test: public Base {};
}

#endif
```

main.cpp 文件内容：

```cpp
#include "test.h"

int main()
{
    Test t;
    UI::Test tt;
    
    t.display();
    tt.display();
    
    return 0;
}
```

运行结果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190801230704.png"/>

感谢花费宝贵的时间浏览。 转载请注明出处，谢谢！



## 参考博文：

因为有着热心网友的无私分享，

[QT namespace UI ](https://blog.csdn.net/hebbely/article/details/79267348) 

[Qt 编程中 namespace Ui { class Widget; } 解析](https://www.cnblogs.com/GyForever1004/p/9043839.html) 

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

<br>

## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到csdn博客：**</font> [Qt 编程中 namespace Ui { class Widget; } 解析](https://blog.csdn.net/qq_33154343/article/details/98122981)