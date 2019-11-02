---
title: '`Qt图片的绘图类`QPixmap`/`QBitmap`/`QImage`/`QPicture`区别和使用'
date: 2019-07-27 15:05:57
toc: true
categories: 
 - [学习 - qt]
tags: 
 - qt
 - QPixmap
 - QBitmap
 - QImage
 - QPicture
---

**简介：**   `Qt`图片的绘图类`QPixmap`/`QBitmap`/`QImage`/`QPicture`区别和使用

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		   `Qt`图片的绘图类`QPixmap`/`QBitmap`/`QImage`/`QPicture`区别和使用

<br>

## 开发平台环境：

**编程环境：**  `win10 x64 专业版`

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">重要提示：</font>

<font color=#70AD47 size=4 face="幼圆">推荐点击文末的同步博客链接，查看本文，获得100%的浏览体验效果</font>

- 若遇csdn的博文的排版异常，图片无法加载，无法替换显示，则会删除异常部分（文末为无删减版）
- 无法预览的视频，则会替换为其链接；若学习资源分享失效，请评论区留言或留下邮箱
- <font color=#D0087E  size=4 face="幼圆">**请点击<font color=#FE7207  size=4 face="幼圆">本文末的同步链接</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好的100%效果体验**</font> 

<br>

## 知识点讲解：

> `绘图设备`： 绘图设备是指继承`QPaintDevice`的子类，你可以使用QPainter直接在其上面绘制图形，`Qt`一共提供了四个这样继承QPaintDevice的绘图设备类，分别是QPixmap、QBitmap、QImage和 QPicture。
>
> `QPixmap`：针对屏幕进行优化了，和平台相关，不能对图片进行修改
>
> `QBitmap`：是QPixmap的一个子类，它的色深限定为1，你可以使用 QPixmap的isQBitmap()函数来确定这    个QPixmap是不是一个QBitmap；
>
> `QImage`：和平台无关，可以对图片进行修改，在线程中绘图，专门为图像的像素级访问做了优化；
>
> `QPicture`：保存绘图的状态（二进制文件），则可以记录和重现QPainter的各条命令；



## **QPixmap和QBitmap**

QPixmap可以接受一个字符串作为一个文件的路径来显示这个文件，比如你想在程序之中打开png、jpeg之类的文件，就可以使用 QPixmap。使用QPainter的drawPixmap()函数可以把这个文件绘制到一个QLabel、QPushButton或者其他的设备上面。QPixmap是针对屏幕进行特殊优化的，因此，它与实际的底层显示设备息息相关。注意，这里说的显示设备并不是硬件，而是操作系统提供的原生的绘图引擎。所以，在不同的操作系统平台下，QPixmap的显示可能会有所差别。

QPixmap提供了静态的grabWidget()和grabWindow()函数，用于将自身图像绘制到目标上。同时，在使用QPixmap时，你可以直接使用传值也不需要传指针，因为QPixmap提供了“隐式数据共享”。关于这一点，我们会在以后的章节中详细描述，这里只要知道传递QPixmap不必须使用指针就好了。

QBitmap继承自QPixmap，主要用于显示单色位图。是QPixmap子类，因此具有其所有特性。QBitmap的色深始终为1. 色深这个概念来自计算机图形学，是指用于表现颜色的二进制的位数。我们知道，计算机里面的数据都是使用二进制表示的。为了表示一种颜色，我们也会使用二进制。比如我们要表示8种颜色，需要用3个二进制位，这时我们就说色深是3. 因此，所谓色深为1，也就是使用1个二进制位表示颜色。1个位只有两种状态：0和1，因此它所表示的颜色就有两种，黑和白。所以说，QBitmap实际上是只有黑白两色的图像数据。

由于QBitmap色深小，因此只占用很少的存储空间，所以适合做光标文件和笔刷。

下面我们来看同一个图像文件在QPixmap和QBitmap下的不同表现：

```cpp
void PaintedWidget::paintEvent(QPaintEvent *event)
{
	QPainter painter(this);
	QPixmap pixmap("butterfly.png");  //图片背景透明
	QBitmap bitmap("butterfly.png");  //图片背景透明
	painter.drawPixmap(0, 0 pixmap);
	painter.drawPixmap(200, 0, bitmap);
	QPixmap pixmap2("butterfly2.png"); //图片背景白色
	QBitmap bitmap2("butterfly2.png"); //图片背景白色
	painter.drawPixmap(0, 200, pixmap2);
	painter.drawPixmap(200, 200, bitmap2);
}
```

先来看一下运行结果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190727145004.png"/>



这里我们给出了两张png图片。`butterfly.png`是具有透明色的背景，而`butterfly2.png`是没有透明色的纯白背景，。我们分别使用QPixmap和QBitmap来加载它们。注意看它们的区别：白色的背景在Qbitmap中消失了，**而透明色在QBitmap中转换成了黑色；其他颜色则是使用点的疏密程度来体现的。** 

## QPixmap：

```cpp
//QPixmap
QPixmap pixmap(200, 200);                                 //绘画设备 400*400
pixmap.fill(Qt::white);                                   //填充背景色（默认为黑色）
QPainter p1(&pixmap);                                     //设置画家
p1.drawPixmap(0, 0, 200, 200, QPixmap("../plum.png"));    //画家在绘图设备绘画
pixmap.save("../QPixmap_plum.png");                      //绘画保存(默认为build-QtExample-Desktop_Qt_5_9_8_MinGW_32bit-Debug里面)
```



## QImage：

QPixmap使用底层平台的绘制系统进行绘制，无法提供像素级别的操作，而QImage则是使用独立于硬件的绘制系统，实际上是自己绘制自己，因此提供了像素级别的操作，并且能够在不同系统之上提供一个一致的显示形式。

```cpp
//QImage
QImage image(200, 200, QImage::Format_ARGB32);
QPainter p2(&image);
p2.drawImage(0, 0, QImage("../plum.png"));
image.save("../QImage_plum.png");
```



## QPicture：

QPicture是一个可以记录和重现QPainter命令的绘图设备。QPicture将QPainter的命令序列化到一个IO设备，保存为一个平台独立的文件格式。这种格式有时候会是“元文件(meta- files)”。Qt的这种格式是二进制的，不同于某些本地的元文件，Qt的pictures文件没有内容上的限制，只要是能够被QPainter绘制的元素，不论是字体还是pixmap，或者是变换，都可以保存进一个picture中。

QPicture是平台无关的，因此它可以使用在多种设备之上，比如svg、pdf、ps、打印机或者屏幕。回忆下我们这里所说的QPaintDevice，实际上是说可以有QPainter绘制的对象。QPicture使用系统的分辨率，并且可以调整 QPainter来消除不同设备之间的显示差异。

如果我们要记录下QPainter的命令，**首先要使用QPainter::begin()函数在QPicture上进行绘图，将QPicture实例作为参数传递进去，以便告诉系统开始记录，记录完毕后使用QPainter::end()命令终止，**最后使用save()保存，代码示例如下：

```cpp
//QPicture
QPicture picture;
QPainter p3;
p3.begin((&picture));
p3.drawPixmap(0, 0, 200, 200, QPixmap("../plum.png"));    //任意绘画
p3.drawEllipse(10, 20, 80,70);
p3.end();
picture.save("../QPicture_plum.png");
```

**效果图：**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190727143406.png"/>

因为为二进制文件，所以图片无法显示，但是文件是保存完好的正确的

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190727143654.png"/>

但是可以通过其来`load()`直接读取该二进制文件：

## 加载QPicture：

```cpp
QPicture picture;
picture.load("../QPicture_plum.png");

QPainter p(this);
p.drawPicture(0, 0 , picture);
```



## QImage与pixmap的区别

1、QPixmap主要是用于绘图，针对屏幕显示而最佳化设计，QImage主要是为图像I/O、图片访问和像素修改而设计的

2、QPixmap依赖于所在的平台的绘图引擎，故例如反锯齿等一些效果在不同的平台上可能会有不同的显示效果，QImage使用Qt自身的绘图引擎，可在不同平台上具有相同的显示效果

3、目前的Qt会把QPixmap都存储在graphics memory中，QImage是存储在客户端的，是独立于硬件的。在 X11, Mac 以及 Symbian平台上，QPixmap 是存储在服务器端，而QImage则是存储在客户端，在Windows平台上，QPixmap和QImage都是存储在客户端，并不使用任何的GDI资源。

4、由于QImage是独立于硬件的，也是一种QPaintDevice，因此我们可以在另一个线程中对其进行绘制，而不需要在GUI线程中处理，使用这一方式可以很大幅度提高UI响应速度。

5、QImage可通过setPixpel()和pixel()等方法直接存取指定的像素。

当图片较大时，我们可以先通过QImage将图片加载进来，然后把图片缩放成需要的尺寸，最后转换成QPixmap 进行显示。

## QPixmap -->  image：

```cpp
//QPixmap -->  image
QPainter p(this);

QPixmap pixmap;
pixmap.load("../QPixmap_plum.png");
QImage image = pixmap.toImage();
p.drawImage(0, 0 , image);
```

## image -->  QPixmap：

```cpp
//image -->  QPixmap
QPainter p(this);

QImage image2;
image2.load("../QImage_plum.png");
QPixmap pixmap2;
pixmap2 = QPixmap::fromImage(image);
p.drawPixmap(200, 0, pixmap);
```

效果图如下：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190727144015.png"/>

<br>

## 完整代码：

```cpp
#include "QtExample.h"
#include "ui_QtExample.h"
#include <QPixmap>
#include <QImage>
#include <QPainter>
#include <QPicture>

QtExample::QtExample(QWidget *parent) :
    QWidget(parent),
    ui(new Ui::QtExample)
{
    ui->setupUi(this);
        
    //QPixmap
    QPixmap pixmap(200, 200);                                 //绘画设备 400*400
    pixmap.fill(Qt::white);                                   //填充背景色（默认为黑色）
    QPainter p1(&pixmap);                                     //设置画家
    p1.drawPixmap(0, 0, 200, 200, QPixmap("../plum.png"));    //画家在绘图设备绘画
    pixmap.save("../QPixmap_plum.png");                      //绘画保存(默认为build-QtExample-Desktop_Qt_5_9_8_MinGW_32bit-Debug里面)

    //QImage
    QImage image(200, 200, QImage::Format_ARGB32);
    QPainter p2(&image);
    p2.drawImage(0, 0, QImage("../plum.png"));
    image.save("../QImage_plum.png");

    //QPicture
    QPicture picture;
    QPainter p3;
    p3.begin((&picture));
    p3.drawPixmap(0, 0, 200, 200, QPixmap("../plum.png"));    //任意绘画
    p3.drawEllipse(10, 20, 80,70);
    p3.end();
    picture.save("../QPicture_plum.png");

}

QtExample::~QtExample()
{
    delete ui;
}

void QtExample::paintEvent(QPaintEvent *)
{
#if 0
    QPicture picture;
    picture.load("../QPicture_plum.png");

    QPainter p(this);
    p.drawPicture(0, 0 , picture);
#endif
    QPainter p(this);

    //QPixmap -->  image
    QPixmap pixmap;
    pixmap.load("../QPixmap_plum.png");
    QImage image = pixmap.toImage();
    p.drawImage(0, 0 , image);

    //image -->  QPixmap
    QImage image2;
    image2.load("../QImage_plum.png");
    QPixmap pixmap2;
    pixmap2 = QPixmap::fromImage(image);
    p.drawPixmap(200, 0, pixmap);
}
```

<br>

## 参考博文：

因为有着热心网友的无私分享，故不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 

**参考：**  [Qt图片绘图类QPixmap/QImage/QPicture](https://blog.csdn.net/qq_33266987/article/details/73187140)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

<br>

## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到csdn博客：**</font> [`Qt`图片的绘图类`QPixmap`/`QBitmap`/`QImage`/`QPicture`区别和使用 ](