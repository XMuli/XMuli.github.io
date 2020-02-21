---
title: QStyle设置界面的外观和QCommonStyle继承关系图讲解和使用
date: 2020-02-18 00:30:46
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt
---



**简  述：**  本文章讲解类容如下

- 讲述绘画自定义风格的 Style 的框架结构；

- 使用 QStyle 设置界面的外观
- QCommonStyle 继承关系图
- 如何继承 QCommonStyle 类来创建自己的自定义的类
- 讲解如下函数：polish()，unpolish()，drawPrimitive()，drawControl()，subElementRect()，drawComplexControl()，subControlRect()，pixelMetric()，styleHint()

<!-- more -->

[TOC]

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [QStyle设置界面的外观和QCommonStyle继承关系图讲解和使用](https://blog.csdn.net/qq_33154343/article/details/104367878)

<br>

## QStyle 设置界面的外观：

因 Qt 本身即是跨平台的一个类库，其中 GUI 的控件，在不同的操作系统下，会有着不同的显示效果，就是默认缺省效果。其中 `QApplication::style()` 可以返回应用程序的缺省样式。

Qt 内置的界面的控件都是使用 `QStyle` 来进行绘画的，来确保它们在运行平台的界面效果是一致的。下面以 `QTableWidget`（含`QScrollBar`） 在不同的操作系统上面的样式效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_20200216185631.png"/>



可以看到，即使是相同的控件，使用同一个主题 Style 在不同的操作系统下，其样式也是一样的。严谨说，有那么一丢丢的细节差异，但是整体风格基本是保持一致的。

<br>

## QCommonStyle 继承关系图：

下面给出 `QCommonStyle` 类的继承关系图；这个类是一个比较完整的基类的「eg：某一具体风格 QWindowsStyle、QMacStyle」的基类。**它非常重要**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200216_231155.jpeg"/>

<br>

## QCommonStyle：

### Qt 文档理解：

首先参看 Qt 官方资料：

> ## 详细说明
>
> 这个抽象类实现了一些小部件的外观，这些外观是Qt提供和提供的所有GUI样式所共有的。
>
> 由于QCommonStyle继承了[QStyle](https://doc.qt.io/qt-5/qstyle.html)，因此其所有功能都在[QStyle](https://doc.qt.io/qt-5/qstyle.html)文档中进行了充分记录。
>
> **另请参见**[QStyle](https://doc.qt.io/qt-5/qstyle.html)和[QProxyStyle](https://doc.qt.io/qt-5/qproxystyle.html)。

### 项目中理解：

**接着，就是实际项目中，对此 QCommonStyle 类的理解：**

`QCommonStyle` 是一个已经比较完备的基础类，言外之意是，已经可以看成某一个具体的样式风格了，但是具体的某些细节还是需要打磨（即再派生一个子类）；但是这并不妨碍什么🐶头，可以作为我们在写自定义风格时候的一个 **最重要的、也是最经常翻看源码的参考类** ；像具体的控件 QSlider，QProgressBar、QPushButton 等 GUI 控件的界面样式，都是在这里面实现的。

而 QStyle 是一个纯虚抽象基类。是所有风格的源头祖上，查看他的 .h 文件会收获风格的框架，初看可以看出一个模糊框架，后续有一定实战代码再来反复回味，体会框架。

<br>

## 创建一种自定义 Style 风格：

觉得所有控件的风格样式都是不满足你的审美，那又如何创建一个自定义的风格类型，属于和 MacOS，Windows，Linux 平级的那种风格该如何创建呢？



**这里我创建一个自定义风格 <font color=#D0087E size=4 face="幼圆">class: MyStyle</font>**

因为希望能够跨平台使用该风格，由上图的继承关系图可看，继承于 QCommonStyle  是最佳的选择。

- 优点如下：
  - 接口很全面，不需要写过多地代码（相对直接继承 QStyle 而言）
  - 可以跨平台使用该风格（相对直接继承于 QMacStyle）

然后在该 <font color=#D0087E size=4 face="幼圆">class: MyStyle</font> 里面，重写如下的虚函数即可：👇的是必不可少

```cpp
// QStyle interface
public:
    void polish(QWidget *widget);
    void unpolish(QWidget *widget);
    void drawPrimitive(PrimitiveElement pe, const QStyleOption *opt, QPainter *p, const QWidget *w) const;
    void drawControl(ControlElement element, const QStyleOption *opt, QPainter *p, const QWidget *w) const;
    QRect subElementRect(SubElement subElement, const QStyleOption *option, const QWidget *widget) const;
    void drawComplexControl(ComplexControl cc, const QStyleOptionComplex *opt, QPainter *p, const QWidget *widget) const;
    QRect subControlRect(ComplexControl cc, const QStyleOptionComplex *opt, SubControl sc, const QWidget *widget) const;
    int pixelMetric(PixelMetric metric, const QStyleOption *option, const QWidget *widget) const;
    QSize sizeFromContents(ContentsType ct, const QStyleOption *opt, const QSize &contentsSize, const QWidget *w) const;
    int styleHint(StyleHint stylehint, const QStyleOption *opt, const QWidget *widget, QStyleHintReturn *returnData) const;
```

<br>

## 继承的虚函数的含义：

因为需要在 <font color=#D0087E size=4 face="幼圆">**class: MyStyle**</font> 里面继承重写如上的虚函数；这里讲述它们的基本作用。以及调用顺序，犹记得，网上这一块的教程基本没有，只有那么一篇的文章讲解的很棒，反复揣摩十遍以上，也总是是是而非的感觉，朦胧且模糊。这也算是我出该文章的一个很大原因。



这些函数之间的调用顺序如下「并不是是💯的严谨」，为了理解框架和脉络结构，在一个比较短的时间内入门，可以这样理解，在没有大的错误下：

### 控件的行为的流程图：

- **插件相关**
  1. 安装指定控件的指定功能
  2. 卸载指定控件的指定功能
  3. 预先预备（开启）指定控件的指定功能，准备开启

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_IMG_1585.PNG"/>

### 控件的绘画流程：

- **绘画相关：**
  1. Qt or self 定义好一些基础的宽度，用枚举值记录📝一些基本的像素值（一般 int 类型）
  2. 定义该 GUI 控件的矩形大小，给它一个系统的初始化大小 QSize
  3. 根据该 QRect 大小分为多个小的 QRect（这个是有 Qt 本生就将一个**复杂控件** 用枚举值来划分好了为若干个**简单地控件** ）
  4. 通过`drawComplexControl()`或`drawControl()`向下分派划分任务
  5. 对 GUI 控件进行风格的重绘

<font color=#0000FF size=4 face="幼圆">这里的箭黑色→，是指调用顺序，有时候也可以代指 A 调用了 B 的函数；而棕色➡︎表示也可以跳过黑色一部分，直接跳着调用下面的。</font>

- <font color=#0000FF size=4 face="幼圆">当遇到简单控件时候，可以直接按照褐色，直接进行具体的绘画，而没有拆分步骤。</font>
- <font color=#0000FF size=4 face="幼圆">而遇到复杂控件时候就可以按照黑色→依顺序拆分为多个简单控件，进行绘画</font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_IMG_1584.PNG"/>

<br>

## 虚函数作用:：

上面继承的一些需要重写的虚函数的，它们大致的功能如下，调用步骤也是如下面所讲。犹记得，当初初看代码懵逼，查阅 Qt 文档 Assistant 里面什么都没写，wtf❓❓❓

再来回顾感受一下官方没有说明外加套娃的骚操作：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200218_003815.jpeg"/>

### polish():

#### 解释含义：

> <font color=#D0087E size=4 face="幼圆">polish():</font>  安装 GUI 的某控件的某一功能，启用该行为

```cpp
virtual void polish(QWidget *widget) override;
```



#### 带例分析：

实际上，这个函数理解起来非常有困难，至少一开始是，那么就举一个实际需求的栗子：

**如果检测到该控件是 QScrollBar 的话，就设置其为透明；**

 <font color=#FE7207  size=4 face="幼圆">*Qt::WA_OpaquePaintEvent枚举含义：*</font>

<font color=#FE7207  size=4 face="幼圆"> 注意：与WA_NoSystemBackground不同，WA_OpaquePaintEvent努力避免透明的窗口背景。 </font>



注：可以想象为 MacOS 系统的滚动条，鼠标离开就会逐渐消失（隐藏、透明）的效果。

```cpp
void MyStyle::polish(QWidget *w)
{
    QCommonStyle::polish(w);

    if (qobject_cast<QComboBox *>(w)
            || qobject_cast<QScrollBar *>(w)) {
        w->setAttribute(Qt::WA_Hover, false);
    }

    if (auto scrollBar = qobject_cast<QScrollBar *>(w))
        scrollBar->setAttribute(Qt::WA_OpaquePaintEvent, false);
}
```

<br>

### unpolish():

#### 解释含义：

> <font color=#D0087E size=4 face="幼圆">unpolish():</font>  卸载 GUI 的某控件的某一功能，禁用该行为 

```cpp
virtual void unpolish(QWidget *widget) override;
```



#### 举例分析：

实际上，这个函数理解的困难同上，举一个上面相反的实际需求的栗子：

**如果检测到该控件是 QScrollBar 的话，就设置其不透明；**

```cpp
void MyStyle::unpolish(QWidget *w)
{
    QCommonStyle::unpolish(w);

    if (qobject_cast<QScrollBar *>(w))
      w->setAttribute(Qt::WA_Hover, false);

    if (auto scrollBar = qobject_cast<QScrollBar *>(w))
      scrollBar->setAttribute(Qt::WA_OpaquePaintEvent, true);
}
```

-------👇👇👇👇接着执行下面👇👇👇👇-------

<br>

### styleHint():

#### 解释含义：

> <font color=#D0087E size=4 face="幼圆">styleHint():</font>  开启或关闭某一 GUI 的控件的行为，或开启选择指定的某种特性

```cpp
virtual int styleHint(StyleHint stylehint, const QStyleOption *opt, const QWidget *widget, QStyleHintReturn *returnData) const override;
```



#### 举例分析：

这个同样是不怎么好理解。举一🍐：

**对 QSlider 控件，开启鼠标左键和中键，鼠标🖱的中键(和⬅️键)指滑槽哪一个刻度，其游标就跳转到该刻度值的地方**

```cpp
int MyStyle::styleHint(QStyle::StyleHint sh, const QStyleOption *opt, const QWidget *w, QStyleHintReturn *shret) const
{
    switch (sh) {
    case SH_Slider_AbsoluteSetButtons:
        return Qt::LeftButton | Qt::MidButton;
    default:
        break;
    }

    return QCommonStyle::styleHint(sh, opt, w, shret);
}
```

-------👇👇👇👇接着执行下面👇👇👇👇-------

<br>

### subControlRect():

#### 解释含义：

> <font color=#D0087E size=4 face="幼圆">subControlRect():</font>  返回某一个 GUI 的控件的矩形大小

```cpp
virtual QRect subControlRect(ComplexControl cc, const QStyleOptionComplex *opt, SubControl sc, const QWidget *widget) const override;
```



#### 举例分析：

<br>

### sizeFromContents():

#### 解释含义：

> <font color=#D0087E size=4 face="幼圆">sizeFromContents():</font>  返回某一 GUI 控件的中心矩形的大小

```cpp
virtual QSize sizeFromContents(ContentsType ct, const QStyleOption *opt, const QSize &contentsSize, const QWidget *w) const override;
```

<br>

### subElementRect():

#### 解释含义：

> <font color=#D0087E size=4 face="幼圆">subElementRect():</font>  返回**某一个元素**的矩形大小
>
> **「补充：某一个元素 ≈ 某一个枚举 ≈ 具体控件的某一个部分」**

```cpp
virtual QRect subElementRect(SubElement subElement, const QStyleOption *option, const QWidget *widget) const override;
```

<br>

### pixelMetric():

#### 解释含义：

> <font color=#D0087E size=4 face="幼圆">pixelMetric():</font> 返回**某一个元素**的长度
>
> **「补充：某一个元素 ≈ 某一个枚举 ≈ 具体控件的某一个部分」**

```cpp
virtual int pixelMetric(PixelMetric metric, const QStyleOption *option, const QWidget *widget) const override;
```



#### 举例分析：

获取菜单栏的 item 之间的竖直之间的间隔 PM_MenuVMargin = 8 px；**

```cpp
int pixelMetric(PixelMetric metric, const QStyleOption *option, const QWidget *widget) const override;
{
	switch (metric) {
  case PM_MenuVMargin:
        return 8;
  default:
        break;
  }
  
  return QCommonStyle::pixelMetric(metric, option, widget);
}
```

-------👇👇👇👇接着执行下面👇👇👇👇-------

<br>

### drawComplexControl():

#### 解释含义：

> <font color=#D0087E size=4 face="幼圆">drawComplexControl():</font> 绘画 GUI 某一控件的，将该控件的每一个部分都绘画分派出去，调用 drawControl() 里面对应的枚举。

```cpp
virtual void drawComplexControl(ComplexControl cc, const QStyleOptionComplex *opt, QPainter *p, const QWidget *widget) const override;
```



可参考drawControl();但是是比它更上一层。

<br>

### drawControl():

#### 解释含义：

> <font color=#D0087E size=4 face="幼圆">drawControl():</font>  绘画 GUI 某一控件的某一部分

```cpp
virtual void drawControl(ControlElement element, const QStyleOption *opt, QPainter *p, const QWidget *w) const override;
```



#### 举例分析：

此处以绘画复杂解GUI 控件，进度条 QProgressBar 为例子：

其为复杂控件，用一张图来表示，

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/blog-imange/img/mark_Snip20200217_235136.jpeg"/>

其中实现代码（局部）如下：

```cpp
void ChameleonStyle::drawControl(QStyle::ControlElement element, const QStyleOption *opt, QPainter *p, const QWidget *w) const
{
	case CE_ProgressBar: {  //显示进度区域
        if (const QStyleOptionProgressBar *progBar =  qstyleoption_cast<const QStyleOptionProgressBar *>(opt)) {
            ...
            QStyleOptionProgressBar progGroove = *progBar;
            proxy()->drawControl(CE_ProgressBarGroove, &progGroove, p, w);
          ...
            QStyleOptionProgressBar subopt = *progBar;
            subopt.rect = proxy()->subElementRect(SE_ProgressBarContents, progBar, w);
            proxy()->drawControl(CE_ProgressBarContents, &subopt, p, w);

          ...
          subopt.rect = proxy()->subElementRect(SE_ProgressBarLabel, progBar, w);
          proxy()->drawControl(CE_ProgressBarLabel, &subopt, p, w);
        }
        return;
    }
    case CE_ProgressBarGroove: {  //滑槽显示
      ...实际绘画
    }
	  case CE_ProgressBarContents: { //进度滑块显示
      ...
    }
	  case CE_ProgressBarLabel: {
      ...
    }
}
```

这里的实现我绘画的最终效果如下：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20191008224313.png"/>

<br>

### drawPrimitive():

#### 解释含义：

> <font color=#D0087E size=4 face="幼圆">drawPrimitive():</font>  绘画 GUI 某一控件的某一部分，通常为详细绘画

```cpp
virtual void drawPrimitive(PrimitiveElement pe, const QStyleOption *opt, QPainter *p, const QWidget *w) const override;
```



实际的具体的一个元素的矩形，在此范围内绘画圆角矩形，圆形，三角形等等等，按照需求绘画即可。

