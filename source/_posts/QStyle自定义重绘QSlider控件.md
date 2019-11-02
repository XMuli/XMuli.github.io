---
title: QStyle自定义重绘QSlider控件
date: 2019-9-4 18:24:19
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - QStyle
 - qt
---

**简介：**  根据`QStyle`的继承关系和重绘原理；通过实现一个继承`QCommonStyle`类的实现，实现自己的自定义控件`QSlider`控件。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `deepin 15.11 x64 专业版 `    **Kernel：**  `x86_64 Linux 4.15.0-30deepin-generic`

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [`QStyle`自定义重绘`QSlider`控件](https://blog.csdn.net/qq_33154343/article/details/100545769)

<br>

## 系列博文：

- [`QStyle`自定义重绘`QSlider`控件](https://touwoyimuli.github.io/2019/09/04/QStyle%E8%87%AA%E5%AE%9A%E4%B9%89%E9%87%8D%E7%BB%98QSlider%E6%8E%A7%E4%BB%B6/) 
-  [QStyle之PenStyle的CustomDashLine使用](https://touwoyimuli.github.io/2019/09/09/QStyle%E4%B9%8BPenStyle%E7%9A%84CustomDashLine%E4%BD%BF%E7%94%A8/) 【更新：更加精准的绘画滑槽】
- [重绘的QStyle中sizeFromContents()没有被调用](https://touwoyimuli.github.io/2019/09/17/%E9%87%8D%E7%BB%98%E7%9A%84QStyle%E4%B8%ADsizeFromContents()%E6%B2%A1%E6%9C%89%E8%A2%AB%E8%B0%83%E7%94%A8/)
- [QStyle自定义重绘QSlider控件二](https://touwoyimuli.github.io/2019/09/17/QStyle%E8%87%AA%E5%AE%9A%E4%B9%89%E9%87%8D%E7%BB%98QSlider%E6%8E%A7%E4%BB%B6%E4%BA%8C/)（重要）

<br>

## 运行效果：

先上一张最终的重绘运行效果图

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190910213128.png"/>

运行代码，在 **main（）**函数里面：

```cpp
QApplication app(argc, argv);
    qApp->setStyle("chameleon");

    QWidget wTemp;
    wTemp.resize(800, 600);
    
//    NoTicks = 0,
//    TicksAbove = 1,
//    TicksLeft = TicksAbove,
//    TicksBelow = 2,
//    TicksRight = TicksBelow,
//    TicksBothSides = 3

	QHBoxLayout *layout = new QHBoxLayout(&wTemp);
    QSlider *slider1 = new QSlider(&wTemp);               //竖直刻度在右侧的Slider
    slider1->setOrientation(Qt::Vertical);
    slider1->setTickPosition(QSlider::TicksRight);
    slider1->setTickInterval(10);
    slider1->resize(40, 300);

    QSlider *slider2 = new QSlider(&wTemp);              //竖直刻度在左侧的Slider
    slider2->setOrientation(Qt::Vertical);
    slider2->setTickPosition(QSlider::TicksLeft);
    slider2->setTickInterval(10);
    slider2->resize(40, 300);
    slider2->move(150, 0);

    QSlider *slider3 = new QSlider(&wTemp);             //水平刻度在下侧的Slider
    slider3->setOrientation(Qt::Horizontal);
    slider3->setTickPosition(QSlider::TicksBelow);
    slider3->resize(300, 40);
    slider3->setTickInterval(10);
    slider3->move(0, 400);

    QSlider *slider4 = new QSlider(&wTemp);             //水平刻度在上侧的Slider
    slider4->setOrientation(Qt::Horizontal);
    slider4->setTickPosition(QSlider::TicksAbove);
    slider4->resize(300, 40);
    slider4->setTickInterval(10);
    slider4->move(400, 400);

    QSlider *slider5 = new QSlider(&wTemp);             //竖直刻度在两侧的Slider
    slider5->setOrientation(Qt::Vertical);
    slider5->setTickPosition(QSlider::TicksBothSides);
    slider5->resize(40, 300);
    slider5->setTickInterval(10);
    slider5->move(300, 0);

    QSlider *slider6 = new QSlider(&wTemp);            //水平刻度在两侧的Slider
    slider6->setOrientation(Qt::Horizontal);
    slider6->setTickPosition(QSlider::TicksBothSides);
    slider6->resize(300, 40);
    slider6->setTickInterval(10);
    slider6->move(0, 500);

    wTemp.show();
    return app.exec();
```

<br>

## QSlider属性：

- **Qt文档：**

先看**Qt**官方文档介绍，我搽，就这么两个属性？？？然后大致浏览完了整篇。怎么给我一点，怎么只有这么一点讲解的内容啊！！！这可不行，谁受得了啊 ，然后继续看源码，看其继承的基类**QAbstractSlider**等之后。这还差不多。不然我怎么使用**QStyle**重绘这个控件

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190904183814.png"/>

- **tickInterval : int**
此属性保存tickmarks之间的间隔这是一个值间隔，而不是像素间隔。如果为0，滑块将在singleStep和pageStep之间进行选择。
- **tickPosition : TickPosition**
此属性保存此滑块的tickmark位置，有效值由QSlider::TickPosition enum描述。默认值是QSlider:: notice。



**刻度条绘画方向：**

| Constant                | Description          |
| ----------------------- | -------------------- |
| QSlider::NoTicks        | 不显示刻度           |
| QSlider::TicksBothSides | 两边都显示刻度       |
| QSlider::TicksAbove     | 刻度显示在上边(水平) |
| QSlider::TicksBelow     | 刻度显示在下边(水平) |
| QSlider::TicksLeft      | 刻度显示在左边(竖直) |
| QSlider::TicksRight     | 刻度显示在右边(竖直) |





**QSlider的摆放方向：**

| orientation : Qt::Orientation | Description    |
| ----------------------------- | -------------- |
| Qt::Vertical                  | 滑动条竖直绘画 |
| Qt::Horizontal                | 滑动条水平绘画 |



其他几个涉及**QStyle**重绘的重要属性：

| val            | 含义                                                         |
| -------------- | ------------------------------------------------------------ |
| minimum        | 滑动条最小值                                                 |
| maximum        | 滑动条最大值                                                 |
| singleStep     | 在min-max之间显示的步长,移动一步的改变数值                   |
| pageStep       | 用于计算刻度的个数(有阈值限制,不完全按这个来,没详细研究,柑橘用来有点迷); |
| value          | 当前的显示数值(在min-max之间,也是本信号的槽的参数的数值)     |
| sliderPosition | ？？                                                         |
| tickInterval   | 两个刻度之间的间隔数值（重绘使用）                           |
| tickPosition   | 刻度的位置                                                   |



<font color=#70AD47 size=3 face="幼圆">注意：</font> pageStep   这个用于计算刻度的个数(有阈值限制,不完全按这个来,没详细研究,柑橘用来有点迷);  <font color=#D0087E size=4 face="幼圆">**刻度个数 - 1 = 刻度间隔的个数 = (span - 0) / pageStep**</font>; 关于0, span, min, max, val 的关系,参见**sliderPositionFromValue()**的实现 [此处特指**qfusionstyle.cpp** 里面的, 不知道qcommstyle.cpp实现原理是否相同?]

<br>

## 理解属性步长sigleStep、pageSteop：

因为**QSlider = 滑块（句柄）+ 滑槽 + 刻度（矩形）**；

创建一个简单的小例子，核心代码如下：

```cpp
void Widget::on_sliderHor_valueChanged(int value)
{
    int minimum = ui->sliderHor->minimum();                 //滑动条最小值
    int maximum = ui->sliderHor->maximum();                 //滑动条最大值
    int sigleStep = ui->sliderHor->singleStep();            //在min~max之间显示的步长,移动一步的改变数值
    int pageSteop = ui->sliderHor->pageStep();              //用于计算刻度的个数(有阈值限制,不完全按这个来）
    int val = ui->sliderHor->value();                      //当前的显示数值(在min~max之间,也是本信号的槽的参数的数值)
    int sliderPosition = ui->sliderHor->sliderPosition();   //?? 不是很清楚
    int tickinterval = ui->sliderHor->tickInterval();       //两个刻度之间的间隔数值（重绘使用）
    int tickPosition = ui->sliderHor->tickPosition();       //刻度的位置

    QString str = QString("value:%1,  minimum:%2,  maximum:%3,  sigleStep:%4,  pageSteop:%5,  val:%6,  sliderPosition:%7,  tickinterval:%8,  tickPosition:%9").arg(value).arg(minimum).arg(maximum).arg(sigleStep).arg(pageSteop).arg(val).arg(sliderPosition).arg(tickinterval).arg(tickPosition);
                       
    qDebug()<<str;
}
```

其效果如下：有**qDebug**可以查看每一个数值的含义，以及变化可以看出其含义（每次按下按下一个方向→或←按键）；就会显示一行数据， 重点观察sigleStep、sigleStep、pageSteop的数值；

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190904185953.png"/>

感觉其上面这些矩形，大多是使用时候，而非更底层的重绘该空间使用的；而**sliderPosition**、**tickPosition**才是重绘使用得。有了上面的基础之后,下面开始正经的重绘：

<br>

## 重绘QSlider：

**重绘思路：**

- 先预先计算好**滑块（矩形）+ 滑槽（矩形） + 刻度（矩形）**
- 对该部分矩形进行绘画

### 计算所需要的三个部分的矩形：

```cpp
QRect CustomStyle::subControlRect(QStyle::ComplexControl cc, const QStyleOptionComplex *opt,
                                     QStyle::SubControl sc, const QWidget *w) const
{
    switch (cc) {
    case CC_Slider: {
        if (const QStyleOptionSlider *option = qstyleoption_cast<const QStyleOptionSlider *>(opt)) {
            QRectF rect = option->rect;                                                    //Slider控件总的大小矩形
            int slider_size = proxy()->pixelMetric(PM_SliderControlThickness, opt, w);     //滑块的高度
//            int tick_size = proxy()->pixelMetric(PM_SliderTickmarkOffset, opt, w);         //刻度的高度
            QRectF slider_handle_rect = rect;                                              //滑块和滑漕的的最小公共矩形 (后面被用作临时且被改变的)

            if (option->orientation == Qt::Horizontal) {
                slider_handle_rect.setHeight(slider_size);
                if (option->tickPosition == QSlider::TicksAbove) slider_handle_rect.moveBottom(rect.bottom());
                if (option->tickPosition == QSlider::TicksBelow) slider_handle_rect.moveTop(rect.top());
                if (option->tickPosition == QSlider::TicksBothSides) slider_handle_rect.moveCenter(rect.center());
            } else {
                slider_handle_rect.setWidth(slider_size);
                if (option->tickPosition == QSlider::TicksRight)  slider_handle_rect.moveLeft(rect.left());
                if (option->tickPosition == QSlider::TicksLeft)   slider_handle_rect.moveRight(rect.right());
                if (option->tickPosition == QSlider::TicksBothSides) slider_handle_rect.moveCenter(rect.center());
            }

            QRectF rectStatic =  slider_handle_rect;   //rectStatic作为 滑块和滑漕的的最小公共矩形(不改变)

            switch (sc) {
            case SC_SliderGroove: {  //滑漕
                qreal groove_size = slider_size / 4.0;
                QRectF groove_rect;

                if (option->orientation == Qt::Horizontal) {
                    groove_rect.setWidth(slider_handle_rect.width());
                    groove_rect.setHeight(groove_size);
                } else {
                    groove_rect.setWidth(groove_size);
                    groove_rect.setHeight(slider_handle_rect.height());
                }

                groove_rect.moveCenter(slider_handle_rect.center());
                return groove_rect.toRect();
            }
            case SC_SliderHandle: {  //滑块
                int sliderPos = 0;
                int len = proxy()->pixelMetric(PM_SliderLength, option, w);
                bool horizontal = option->orientation == Qt::Horizontal;
                sliderPos = sliderPositionFromValue(option->minimum, option->maximum, option->sliderPosition,
                                                    (horizontal ? slider_handle_rect.width() : slider_handle_rect.height()) - len, option->upsideDown);
                if (horizontal) {
                    slider_handle_rect.moveLeft(slider_handle_rect.left() + sliderPos);
                    slider_handle_rect.setWidth(len);
                    slider_handle_rect.moveTop(rectStatic.top());
                } else {
                    slider_handle_rect.moveTop(slider_handle_rect.top() + sliderPos);
                    slider_handle_rect.setHeight(len);
                    slider_handle_rect.moveLeft(rectStatic.left());
                }

                return slider_handle_rect.toRect();
            }
            case SC_SliderTickmarks: {  //刻度的矩形
                QRectF tick_rect = rect;

                if (option->orientation == Qt::Horizontal) {
                    tick_rect.setHeight(rect.height() - slider_handle_rect.height());

                    if (option->tickPosition == QSlider::TicksAbove) {
                        tick_rect.moveTop(rect.top());
                    } else if (option->tickPosition == QSlider::TicksBelow) {
                        tick_rect.moveBottom(rect.bottom());
                    }
                } else {
                    tick_rect.setWidth(rect.width() - slider_handle_rect.width());

                    if (option->tickPosition == QSlider::TicksLeft) {
                        tick_rect.moveLeft(rect.left());
                    } else if (option->tickPosition == QSlider::TicksRight) {
                        tick_rect.moveRight(rect.right());
                    }
                }

                return tick_rect.toRect();
            }
            default:
                break;
            }
        }
        break;
    }
    default:
        break;
    }
    
    return QCommonStyle::subControlRect(cc, opt, sc, w);
}
```



### 再对其中每一个矩形（一共3个）进行重绘：

```cpp
void ChameleonStyle::drawComplexControl(QStyle::ComplexControl cc, const QStyleOptionComplex *opt,
                                        QPainter *p, const QWidget *w) const
{
    switch (cc) {
    case CC_Slider : {
        if (const QStyleOptionSlider *slider = qstyleoption_cast<const QStyleOptionSlider *>(opt)) {
            //各个使用的矩形大小和位置
            QRectF rect = opt->rect;                                                                            //Slider控件最大的矩形(包含如下三个)
            QRectF rectHandle = proxy()->subControlRect(CC_Slider, opt, SC_SliderHandle, w);                    //滑块矩形
            QRectF rectSliderTickmarks = proxy()->subControlRect(CC_Slider, opt, SC_SliderTickmarks, w);        //刻度的矩形
            QRect rectGroove = proxy()->subControlRect(CC_Slider, opt, SC_SliderGroove, w);                     //滑槽的矩形

//            qDebug()<<"____04_____Slider控件最大的矩形(包含如下三个):"<<rect<<"  滑块矩形:"<<rectHandle<<"  刻度的矩形:"<<rectSliderTickmarks<<"   滑槽的矩形:"<<rectGroove<<endl;

//            //测试(保留不删)
            p->fillRect(rect, Qt::gray);
            p->fillRect(rectSliderTickmarks, Qt::blue);
            p->fillRect(rectGroove, Qt::red);
            p->fillRect(rectHandle, Qt::green);
            qDebug()<<"---rect:"<<rect<<"  rectHandle:"<<rectHandle<<"   rectSliderTickmarks:"<<rectSliderTickmarks<<"   rectGroove:"<<rectGroove;

            QPen pen;
            //绘画 滑槽(线)
            if (opt->subControls & SC_SliderGroove) {
                pen.setStyle(Qt::CustomDashLine);
                QVector<qreal> dashes;
                qreal space = 1.3;
                dashes << 0.1 << space;
                pen.setDashPattern(dashes);
                pen.setWidthF(3);
                pen.setColor(getColor(opt, QPalette::Highlight));
                p->setPen(pen);
                p->setRenderHint(QPainter::Antialiasing);

                if (slider->orientation == Qt::Horizontal) {
                    p->drawLine(QPointF(rectGroove.left(), rectHandle.center().y()), QPointF(rectHandle.left(), rectHandle.center().y()));
                    pen.setColor(getColor(opt, QPalette::Foreground));
                    p->setPen(pen);
                    p->drawLine(QPointF(rectGroove.right(), rectHandle.center().y()), QPointF(rectHandle.right(), rectHandle.center().y()));
                } else {
                    p->drawLine(QPointF(rectGroove.center().x(), rectGroove.bottom()), QPointF(rectGroove.center().x(),  rectHandle.bottom()));
                    pen.setColor(getColor(opt, QPalette::Foreground));
                    p->setPen(pen);
                    p->drawLine(QPointF(rectGroove.center().x(),  rectGroove.top()), QPointF(rectGroove.center().x(),  rectHandle.top()));
                }
            }

            //绘画 滑块
            if (opt->subControls & SC_SliderHandle) {
                pen.setStyle(Qt::SolidLine);
                p->setPen(Qt::NoPen);
                p->setBrush(getColor(opt, QPalette::Highlight));
                p->drawRoundedRect(rectHandle, DStyle::pixelMetric(DStyle::PM_FrameRadius), DStyle::pixelMetric(DStyle::PM_FrameRadius));
            }

            //绘画 刻度,绘画方式了参考qfusionstyle.cpp
            if ((opt->subControls & SC_SliderTickmarks) && slider->tickInterval) {                                   //需要绘画刻度
                p->setPen(opt->palette.foreground().color());
                int available = proxy()->pixelMetric(PM_SliderSpaceAvailable, slider, w);  //可用空间
                int interval = slider->tickInterval;                                       //标记间隔
//                int tickSize = proxy()->pixelMetric(PM_SliderTickmarkOffset, opt, w);      //标记偏移
//                int ticks = slider->tickPosition;                                          //标记位置

                int v = slider->minimum;
                int len = proxy()->pixelMetric(PM_SliderLength, slider, w);
                while (v <= slider->maximum + 1) {                                          //此处不添加+1的话, 会少绘画一根线
                    const int v_ = qMin(v, slider->maximum);
                    int pos = sliderPositionFromValue(slider->minimum, slider->maximum, v_, available) + len / 2;

                    if (slider->orientation == Qt::Horizontal) {
                        if (slider->tickPosition == QSlider::TicksBothSides) {              //两侧都会绘画, 总的矩形-中心滑槽滑块最小公共矩形
                            p->drawLine(pos, rect.top(), pos, rectHandle.top());
                            p->drawLine(pos, rect.bottom(), pos, rectHandle.bottom());
                        } else {
                            p->drawLine(pos, rectSliderTickmarks.top(), pos, rectSliderTickmarks.bottom());
                        }
                    } else {
                        if (slider->tickPosition == QSlider::TicksBothSides) {
                            p->drawLine(rect.left(), pos, rectHandle.left(), pos);
                            p->drawLine(rect.right(), pos, rectHandle.right(), pos);
                        } else {
                            p->drawLine(rectSliderTickmarks.left(), pos, rectSliderTickmarks.right(), pos);
                        }
                    }
                    // in the case where maximum is max int
                    int nextInterval = v + interval;
                    if (nextInterval < v)
                        break;
                    v = nextInterval;
                }
            }

        }
        break;
    }
    default:
        break;
    }

    DStyle::drawComplexControl(cc, opt, p, w);
}
```

<br>

## 若刻度异常情况(非bug)：

**注意：**  下图中是使用`pen.setStyle(Qt::DotLine);`来绘画的，上面的代码修改为了`pen.setStyle(Qt::CustomDashLine);`来绘画，所以会**（看到的滑槽）**略有不一样；



当显示区域比较小的时候,而刻度条的个数又比较多的时候(密密麻麻的那种),当超过某一阈值时候,系统会自动压缩显示,一个变成**"胖瘦相间隔"**,此时如果将该粗窗口放大,则会被重绘画显示正常,刻度条均匀相间隔。

原图效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190904192819.png"/>







本来还以为是自己重新绘画的效果造成的,后面确定是过密形成的显示异常情况.就想着有没有能够的解决方法,能够重新绘画显示.想着参考`QFusion` 风格的实现,于是翻看源码，**只看绘画刻度部分**的Qt源码：

```cpp
if (option->subControls & SC_SliderTickmarks) {
                painter->setPen(outline);
                int tickSize = proxy()->pixelMetric(PM_SliderTickmarkOffset, option, widget);
                int available = proxy()->pixelMetric(PM_SliderSpaceAvailable, slider, widget);
                int interval = slider->tickInterval;
                if (interval <= 0) {
                    interval = slider->singleStep;
                    if (QStyle::sliderPositionFromValue(slider->minimum, slider->maximum, interval,
                                                        available)
                            - QStyle::sliderPositionFromValue(slider->minimum, slider->maximum,
                                                              0, available) < 3)
                        interval = slider->pageStep;
                }
                if (interval <= 0)
                    interval = 1;

                int v = slider->minimum;
                int len = proxy()->pixelMetric(PM_SliderLength, slider, widget);
                while (v <= slider->maximum + 1) {
                    if (v == slider->maximum + 1 && interval == 1)
                        break;
                    const int v_ = qMin(v, slider->maximum);
                    int pos = sliderPositionFromValue(slider->minimum, slider->maximum,
                                                      v_, (horizontal
                                                           ? slider->rect.width()
                                                           : slider->rect.height()) - len,
                                                      slider->upsideDown) + len / 2;
                    int extra = 2 - ((v_ == slider->minimum || v_ == slider->maximum) ? 1 : 0);

                    if (horizontal) {
                        if (ticksAbove) {
                            painter->drawLine(pos, slider->rect.top() + extra,
                                              pos, slider->rect.top() + tickSize);
                        }
                        if (ticksBelow) {
                            painter->drawLine(pos, slider->rect.bottom() - extra,
                                              pos, slider->rect.bottom() - tickSize);
                        }
                    } else {
                        if (ticksAbove) {
                            painter->drawLine(slider->rect.left() + extra, pos,
                                              slider->rect.left() + tickSize, pos);
                        }
                        if (ticksBelow) {
                            painter->drawLine(slider->rect.right() - extra, pos,
                                              slider->rect.right() - tickSize, pos);
                        }
                    }
                    // in the case where maximum is max int
                    int nextInterval = v + interval;
                    if (nextInterval < v)
                        break;
                    v = nextInterval;
                }
            }
```

等等， 这根本没有考虑这个问题，好不好。而且感觉绘画刻度的方法，是对一个矩形矩形多次精确的计算，eeemmmmmmmmmm，这样子是不是有点复杂了了。感觉没有我的将这一个超大矩形，分割成为三个小矩形这一思路简单，然后在对每一块小矩形进行相应的绘画。

<br>

## 解决方法：

决定尝试一下其原生**`QFusion`**风格（）是怎么解决这个效果,当设置刻度间隔比较小的时候,显示宽度比较窄时候,这个东西出现了"胖瘦相互间隔"，然后利用上面的一开始创建的小例子**，改变步长和刻度间个和max值，看看效果**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190904191258.png"/>

甚至再次改变步长和刻度间个和max值，看看效果。过于密集，成了线

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190904192704.jpg"/>



握草,握了个草,我握了个大草.hhhhhhhhhhhhhhhhhh,原来老哥你也没有设置这个问题啊,果断的出结论,这不是bug.果断不再继续修改了重绘画了.坏坏的笑了几下之后。

于是将一开始出现问题的地方，将该窗口最大化，然后局部拉大，看到这个效果（果然得到了验证）：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190904192930.png"/>



**最后总结：**这个不是bug，或者显示异常。冷静下来是思考:如此窄的矩形里面,显示如此多根刻度线,由于像素限制,只能后绘画的比较密集,当这个数值更大的时候,会发现,这个会变成一条直线(放大拉开显示,才会显示其实是均匀相间隔的).

<br>

## 思考总结：

- 首先检查代码，是否是相关部分的代码逻辑有问题
- 仍然觉得不应该之后，试一下Qt自带的是否会重现，排除是自己还是非自己原因
- 改变相关的值，写小例子验证，查看效果
- 发现经验：+1；  完美结束

<br>

## 更新：更加精准的绘画滑槽
更新于2019-09-09   参见： [QStyle自定义重绘QSlider控件]([https://touwoyimuli.github.io/2019/09/09/QStyle%E4%B9%8BPenStyle%E7%9A%84CustomDashLine%E4%BD%BF%E7%94%A8/](https://touwoyimuli.github.io/2019/09/09/QStyle之PenStyle的CustomDashLine使用/)) 一文

<br>

## 互联网分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190829225308.jpg"/>

