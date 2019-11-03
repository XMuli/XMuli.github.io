---
title: 在子类里使用 using 父类 父类函数名fun
date: 2019-9-25 19:54:03
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - qt]
 - [学习 - 底层原理、思想架构]
---



**简介：**  在子类里使用 `using` 父类:: 父类函数名`fun`;  这算是一个**c++**的知识点吧，自己在一个项目中看到却不理解的地方， 自己的搜索关键词为：**派生类** 中 **使用 using** `父类 :: 函数名`；所以这一篇的名称就取名为这个

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [在子类里使用 using 父类::父类函数名fun](https://blog.csdn.net/qq_33154343/article/details/101381793)

<br>

## 派生类里使用using原因：

如果基类中成员函数有多个重载版本，派生类可以重定义所继承的<font color=#FF0000 size=4 face="幼圆"> 0 个或多个版本</font>，但是<font color=#FF0000 size=4 face="幼圆"> **通过派生类型只能访问派生类中重定义的那些版本**</font>，所以如果派生类想通过自身类型使用所有的重载版本，则派生类必须**要么重定义所有重载版本**，**要么一个也不重定义**。有时类需要仅仅重定义一个重载集中某些版本的行为，并且想要继承其他版本的含义，在这种情况下，为了重定义需要特化的某个版本而不得不重定义每一个基类版本，可能会令人厌烦。可以在派生类中为重载成员名称提供 using 声明（为基类成员函数名称而作的 using 声明将该函数的所有重载实例加到派生类的作用域），使派生类不用重定义所继承的每一个基类版本。一个 using 声明只能指定一个名字，不能指定形参表，使用using声明将名字加入作用域之后，派生类只需要重定义本类型确实必须定义的那些函数，对其他版本可以使用继承的定义。



<font color=#FF0000 size=4 face="幼圆">**“隐藏”是指派生类的函数屏蔽了与其同名的基类函数，规则如下：**</font>

<font color=#FF0000 size=4 face="幼圆">1、如果派生类的函数与基类的函数同名，但是参数不同。</font>此时，不论有无`virtual`关键字，基类的函数将被隐藏（注意别与重载混淆）。

<font color=#FF0000 size=4 face="幼圆">2、如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有`virtual`关键字。</font>此时，基类的函数被隐藏（注意别与覆盖混淆）

<br>

```cpp
class My
{
public:
    My();

    void fun();
    void fun(int n);
    void fun(QString str);
    void fun(int n, QString str);

    virtual void fun02();
    virtual void fun02(int n);
    virtual void fun02(QString str);
    virtual void fun02(int n, QString str);
};

class MySon : public My
{
public:
    MySon();
    void fun(int n);

    virtual void fun02(int n) override;

    using My::fun;    //本篇所讲
    using My::fun02;  //本篇所讲
};

int main(int argc, char *argv[])
{
    MySon mySon;
    mySon.fun(1);        //只一个"智能提示":是int类型

    mySon.fun();         //此(含自己)后面三个都是得益于 using My::fun; 而可以使用
    mySon.fun("str");    //仍然手写调用My::fun(QString)其他类型, 可以被调用
    mySon.fun(2, "ac");

    qDebug("");

    mySon.fun02(4);      //重写的那个函数,也是唯一的一个的"智能提示"的 int类型
    mySon.fun02();       //此(含自己)后面三个都是得益于 using My::fun02; 而可以使用
    mySon.fun02("ab");
    mySon.fun02(3, "test");
}

```

调用同名函数`fun` 或者 `fun02`  就只有通过一个`My::`来调用

<br>

## 运行效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925194915.jpg"/>



**补充一点：** 使用`using My::fun;    //本篇所讲`和`using My::fun02;  //本篇所讲`，在派生类MySon若是想调用同名函数`fun` 或者 `fun02`  就只有通过一个`My::`来调用

<br>

## 源码下载：

测试源码：[test c++ 使用 using 测试](https://github.com/touwoyimuli/2018_02_C_CPlus/tree/master/08_%E5%9C%A8%E5%AD%90%E7%B1%BB%E9%87%8C%E7%94%A8%20using%20%E7%88%B6%E7%B1%BB%EF%BC%9A%EF%BC%9A%E7%88%B6%E7%B1%BB%E5%87%BD%E6%95%B0%E5%90%8D/test)

<br>

**参考文章:** 

 [C++ using关键字作用 （重载父类函数）](https://blog.csdn.net/CNHK1225/article/details/47152311)



