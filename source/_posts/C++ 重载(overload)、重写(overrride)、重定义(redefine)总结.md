---
title: C++ 重载(overload)、重写(overrride)、重定义(redefine)总结
date: 2019-9-25 19:11:09
toc: true
categories: 
 - [学习 - c/c++]
tags: 
 - c/c++
---



**简介：**  `C++`重载(**overload**)、重写(**overrride**)、重定义(**redefine**)总结

<!-- more -->

[TOC]

**编程环境：**  `deepin 15.11 x64 专业版 `    **Kernel：**  `x86_64 Linux 4.15.0-30deepin-generic`

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [C++ 重载(overload)、重写(overrride)、重定义(redefine)总结](https://blog.csdn.net/qq_33154343/article/details/101298525) 

<br>

## 一、重载（overload）
指函数名相同，但是它的参数表列个数或顺序，类型不同。但是不能靠返回类型来判断。
（1）相同的范围（在同一个作用域中） ；
（2）函数名字相同；
（3）参数不同；
（4）virtual 关键字可有可无。
（5）返回值可以不同；

<br>

## 二、重写（也称为覆盖 override）
是指派生类重新定义基类的虚函数，特征是：
（1）不在同一个作用域（分别位于派生类与基类） ；
（2）函数名字相同；
（3）参数相同；
（4）基类函数必须有 virtual 关键字，不能有 static 。
（5）返回值相同（或是协变），否则报错；<—-协变这个概念我也是第一次才知道…

（6）重写函数的访问修饰符可以不同。尽管 virtual 是 private 的，派生类中重写改写为 public,protected 也是可以的

<br>

## 三、重定义（redefine,也称隐藏）
（1）不在同一个作用域（分别位于派生类与基类） ；
（2）函数名字相同；
（3）返回值可以不同；
（4）参数不同。此时，不论有无 virtual 关键字，基类的函数将被隐藏（注意别与重载以及覆盖混淆） 。
（5）参数相同，但是基类函数没有 virtual关键字。此时，基类的函数被隐藏（注意别与覆盖混淆） 。

<br>

### 重定义redefine的代码演示:

#### 情况一：参数不同:
```cpp
/*重定义（也成隐藏）:
参数不同。此时，不论有无 virtual 关键字，基类的函数将被隐藏（注意别与重载以及覆盖混淆）*/
class My
{
public:
    My();
    void fun();
    void fun(int n);
    void fun(QString str);
    void fun(int n, QString str);
};

class MySon : public My
{
public:
    MySon();
    void fun(double n);
};

---------------------------------------
//或

class My
{
public:
    My();
    virtual void fun();
    virtual void fun(int n);
    virtual void fun(QString str);
    virtualvoid fun(int n, QString str);
};

class MySon : public My
{
public:
    MySon();
    void fun(double n);
};

---------------------------------------
//运行以下代码,会得到一样测试结果 
int main(int argc, char *argv[])
{
    MySon mySon;
    mySon.fun(5.2);      //使用时候,会有唯一"智能提示",只有double的版本
    //mySon.fun("str");  //尝试调用My::fun(QString), 但是被隐藏,会编译不过
    return a.exec();
}

```

**运行结果：**

**编码智能提示时候：只有一个**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925191404.png"/>

**运行结果：**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925191439.png"/>

<br>

#### 情况二：参数相同:

```cpp
/*重定义（也成隐藏）:
参数相同，但是基类函数没有 virtual关键字。此时，基类的函数被隐藏（注意别与覆盖混淆） */
class My
{
public:
    My();
    void fun();
    void fun(int n);
    void fun(QString str);
    void fun(int n, QString str);
};

class MySon : public My
{
public:
    MySon();
    void fun(int n);
};
```
**运行效果：**

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190925191630.png"/>

<br>

**参考文章:**

 [C++ 重载(overload)、重写(overrride)、重定义(redefine)总结](https://www.cnblogs.com/tanky_woo/archive/2012/02/08/2343203.html)

