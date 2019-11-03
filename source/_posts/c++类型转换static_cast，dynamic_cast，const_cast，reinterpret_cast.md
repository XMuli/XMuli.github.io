---
title: c++类型转换`static_cast`，`dynamic_cast`，`const_cast`，`reinterpret_cast`
date: 2019-9-27 00:11:14
toc: true
categories: 
 - [学习 - c/c++]
---



**简介：**  **c++**类型转换`static_cast`，`dynamic_cast`，`const_cast`，`reinterpret_cast`这四种类型转换的区别

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [c++类型转换`static_cast`，`dynamic_cast`，`const_cast`，`reinterpret_cast`](https://blog.csdn.net/qq_33154343/article/details/101486253)

<br>

## 思维导图：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190927000813.png"/>

<br>

## static_cast：

- 静态类型转换:内置、自定义数据类型

    ```c
    //++++++++++++++++++++++++
    //内置数据类型
    char a  = 'a';
    double d = static_cast<double>(a)
    
    //++++++++++++++++++++++++
    //自定义类型
    Base* base = NULL;
    Son* son = NULL;
    
    //向下类型转换  不安全
    Son* son = static_cast<Son*>(base);
    
    //向上类型转换   安全
    Base* base = static_cast<Base*>(son);
    ```

    <br>

## static_cast:

- 动态类型转换：内置、自定义数据类型

    ```c
    //++++++++++++++++++++++++
    //内置数据类型， 不可以转换
    有精度损失的，都不可以转换
    char c  = 'c';
    double d = dynamic_cast<double>(c)
    
    //++++++++++++++++++++++++
    //自定义类型
    Base* base = NULL;
    Son* son = NULL;
    
    //向下类型转换  不安全   失败
    Son* son = dynamic_cast<Son*>(base);
    
    //向上类型转换   安全   成功
    Base* base = dynamic_cast<Base*>(son);
    
    ```

    <br>

## const_cast:

- 常量转换：指针之间的转换

    ```c
    const int* p = NULL;
    
    //将const int * 转换为 int*
    int* p2 = const_cast<int*>(p);    
    
    //将 p2 转换为const int*
    const int* p2 = const_cast<const int*>(p);    
    ```

    <br>

- 常量转换：引用之间的转换

    ```c
    const int a = 10;
    const int& aRef = a;
    
    int& aRef2 = const_cast<int&>(aRef);
    
    //不可以对 非指针 或者 非引用 做const_cast转换
    ```

    <br>

## reinterpret_cast:

- 重新解释转换 最不安全 不建议使用

    ```c
    //将base* 转换为 Other*
    Base* base = NULL;
    Other* other = reinterpret_cast<Other*>(base);
    ```

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/gif/20190704175742.gif)