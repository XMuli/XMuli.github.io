---
title: QT源码分析QObject
date: 2019-9-25 19:29:53
toc: true
categories: 
 - [学习 - qt]
 - [学习 - 底层原理、思想架构]
tags: 
 - qt
---





**简介：**  **QT**源码分析`QObject`，由此管中窥豹

<!-- more -->

[TOC]

## QT源码分析：QObject：

**QT**框架里面最大的特色就是在**C++**的基础上增加了元对象系统（`Meta-Object System`），而元对象系统里面最重要的内容就是信号与槽机制，这个机制是在**C++**语法的基础上实现的，使用了函数、函数指针、回调函数等概念。当然与我们自己去写函数所不同的是槽与信号机制会自动帮我们生成部分代码，比如我们写的信号函数就不需要写它的实现部分，这是因为在我们编译程序的时候，编译器会自动生成这一部分代码，当我们调用`connect`函数的时候，系统会自动将信号函数与槽函数相连接，于是当我们调用信号函数的时候，系统就会自动回调槽函数，不管你是在同一线程下调用或者在不同线程下调用，系统都会自动评估，并在合理的时候触发函数，以此来保证线程的安全。信号与槽机制是线程安全的，这可以使得我们在调用的时候不用再额外的增加过多保证线程同步的代码，为了实现元对象系统，**QT**把所有相关实现写在了`QObject`类中，所以当你想使用元对象系统的时候，你所写的类需要继承自`QObject`，包括**QT**自带的所有类都是继承自`QObject`，所以分析`QObject`的代码，对了解**QT**的元对象机制有非常大的帮助，我并不打算把`QObject`类的每一行代码都写下来，只想把其中比较关键的内容或者对分析**QT**源码有帮助的内容介绍一下。

<br>

## 1.宏Q_OBJECT:

这个宏展开以后是如下定义：

```cpp
#define Q_OBJECT \
public: \
    QT_WARNING_PUSH \
    Q_OBJECT_NO_OVERRIDE_WARNING \
    static const QMetaObject staticMetaObject; \
    virtual const QMetaObject *metaObject() const; \
    virtual void *qt_metacast(const char *); \
    virtual int qt_metacall(QMetaObject::Call, int, void **); \
    QT_TR_FUNCTIONS \
private: \
    Q_OBJECT_NO_ATTRIBUTES_WARNING \
    Q_DECL_HIDDEN_STATIC_METACALL static void qt_static_metacall(QObject *, QMetaObject::Call, int, void **); \
    QT_WARNING_POP \
    struct QPrivateSignal {}; \
QT_ANNOTATE_CLASS(qt_qobject, "")
```

你可以看到这个宏定义了一些函数，并且函数名都带有`meta`，所以不难猜到这些函数和**QT**的元对象系统是有关系的，实际上你在`qobject.cpp`里面是找不到这些函数的实现的，它们的实现都在`moc_qobject.cpp`里面。QT的元对象系统是这样处理的，当你编译你的工程时，它会去遍历所有**C++**文件，当发现某一个类的私有部分有声明`Q_OBJECT`这个宏时，就会自动生成一个`moc_*.cpp`的文件，这个文件会生成信号的实现函数，`Q_OBJECT`宏里面定义的那些函数也会在这个文件里面实现，并生成与类相关的元对象。这就是为什么我们定义一个信号的时候，不需要实现它，因为它的实现已经写在`moc_*.cpp`文件里面了。

<br>

## 2.宏Q_PROPERTY:

> Q_PROPERTY(QString objectName READ objectName WRITE setObjectName NOTIFY objectNameChanged)

这个宏的定义如下：

```cpp
#define Q_DECLARE_PRIVATE(Class) \
    inline Class##Private* d_func() { return reinterpret_cast<Class##Private *>(qGetPtrHelper(d_ptr)); } \
    inline const Class##Private* d_func() const { return reinterpret_cast<const Class##Private *>(qGetPtrHelper(d_ptr)); } \
friend class Class##Private;
```

这个宏首先创建了两个内联函数，返回值都是QObjectPrivate ，并且声明QObjectPrivate 为友元类，`QObjectPrivate`这个类是在qobject_p.h中定义，它继承至QObjectData，你可以看到d_func()是将d_prt强制转换为`QObjectPrivate` 类型，而d_prt这个指针在QObject里面定义的是`QObjectData`的指针类型，所以这里可以进行强转，`QObjectPrivate`这个类主要存放QOject类需要用到的一些子对象，变量等。为什么要介绍这个宏，如果你有看QT源码习惯的话，你会发现几乎每一个类都用到了这个宏，我们自己写的类会经常把类内部用的变量声明在private部分，但是QT源码并不是这样做的，它的做法是给每个类创建一个以`类名+Private`的类，例如QObject对应的就是`QObjectPrivate`，这个类实际上就是用来存放QObject需要用到的所有私有变量和私有对象，而`QObject`更多的是函数实现，你去看其他的源码也是如此，子对象声明在QPrivate中，而本类只实现函数。

<br>

## 3.宏Q_DECLARE_PRIVATE：

> Q_DECLARE_PRIVATE(QObject)

这个宏的定义如下：

```cpp
#define Q_DECLARE_PRIVATE(Class) \
    ``inline` `Class##Private* d_func() { ``return` `reinterpret_cast``<Class##Private *>(qGetPtrHelper(d_ptr)); } \
    ``inline` `const` `Class##Private* d_func() ``const` `{ ``return` `reinterpret_cast``<``const` `Class##Private *>(qGetPtrHelper(d_ptr)); } \
friend` `class` `Class##Private;
```

这个宏首先创建了两个内联函数，返回值都是`QObjectPrivate `，并且声明`QObjectPrivate` 为友元类，`QObjectPrivate`这个类是在qobject_p.h中定义，它继承至`QObjectData`，你可以看到`d_func()`是将`d_prt`强制转换为`QObjectPrivate` 类型，而`d_prt`这个指针在QObject里面定义的是QObjectData的指针类型，所以这里可以进行强转，`QObjectPrivate`这个类主要存放QOject类需要用到的一些子对象，变量等。为什么要介绍这个宏，如果你有看QT源码习惯的话，你会发现几乎每一个类都用到了这个宏，我们自己写的类会经常把类内部用的变量声明在`private`部分，但是QT源码并不是这样做的，它的做法是给每个类创建一个以类名+Private的类，例如QObject对应的就是QObjectPrivate，这个类实际上就是用来存放QObject需要用到的所有私有变量和私有对象，而QObject更多的是函数实现，你去看其他的源码也是如此，子对象声明在QPrivate中，而本类只实现函数。

<br>

## 4.构造函数：

```cpp
QObject::QObject(QObject *parent)
    : d_ptr(new QObjectPrivate)
{
    Q_D(QObject);
    d_ptr->q_ptr = this;
    d->threadData = (parent && !parent->thread()) ? parent->d_func()->threadData : QThreadData::current();
    d->threadData->ref();
    if (parent) {
        QT_TRY {
            if (!check_parent_thread(parent, parent ? parent->d_func()->threadData : 0, d->threadData))
                parent = 0;
            setParent(parent);
        } QT_CATCH(...) {
            d->threadData->deref();
            QT_RETHROW;
        }
    }
#if QT_VERSION < 0x60000
    qt_addObject(this);
#endif
    if (Q_UNLIKELY(qtHookData[QHooks::AddQObject]))
        reinterpret_cast<QHooks::AddQObjectCallback>(qtHookData[QHooks::AddQObject])(this);
}
```

（1）首先第一步就创建d_ptr指针。

（2）Q_D(QObject);这个宏你可以在QT的很多源码里面看到。它展开以后是下面的样子：#define Q_D(Class) Class##Private * const d = d_func();

　　  d_fun()函数前面讲到了，其实就是返回d_ptr了。所以这个宏的意思是定义一个指针d指向d_ptr;

（3）d_ptr->q_ptr = this;

　　 q_ptr是QOject类型，这里把this指针赋给了它，所以使得QObjectPrivate可以回调QOject的函数。

（4）初始化threadData

<br>

## 5.moveToThread：

## 6.connect函数：

这两个部分参见原文：

[QT源码分析：QObject](https://www.cnblogs.com/WushiShengFei/p/9820835.html)



本文同步文章：

[QT源码分析QObject](https://blog.csdn.net/qq_33154343/article/details/101381410)



