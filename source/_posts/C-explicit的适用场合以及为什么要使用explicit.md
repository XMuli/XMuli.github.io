---
title: 'C++: explicit的适用场合以及为什么要使用explicit'
date: 2019-08-01 23:25:45
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - 底层原理、思想架构]
tags: 
 - 原理
---

**简介：**  explicit是个C++关键子,但是关注过它的人远远没有其他关键字的多,但是往往忽略了它,就会在一些不经意的地方造成错误,而花费更多的时间去寻找.

explicit可以抑制内置类型隐式转换,所以在类的构造函数中,最好尽可能多用explicit关键字,防止不必要的隐式转换.

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		explicit是个C++关键子,但是关注过它的人远远没有其他关键字的多,但是往往忽略了它,就会在一些不经意的地方造成错误,而花费更多的时间去寻找.

**explicit可以抑制内置类型隐式转换,所以在类的构造函数中,最好尽可能多用explicit关键字,防止不必要的隐式转换.**

<br>

## <font color=#D0087E  face="幼圆">重要提示：</font>

- 若遇`csdn`的博文排版、文字、图片、链接、视频预览等异常，会删除该部分，或用链接代替，或删除该部分，但在文末 [github.io](https://touwoyimuli.github.io/) 中的同步文章，会保证显示正确
- <font color=#D0087E  size=4 face="幼圆">**推荐<font color=#FE7207  size=4 face="幼圆">本文末的同步链接</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好的100%效果体验**</font> 

<br>

## 原文：

看下下面这个例子:

```cpp
#include <iostream>
using namespace std;

class A
{
public:
    A(int i = 5)
    {
        m_a = i;
     }
private:
    int m_a;
};

int main()
{
    A s;
    //我们会发现,我们没有重载'='运算符,但是去可以把内置的int类型赋值给了对象A.
    s = 10;
    //实际上,10被隐式转换成了下面的形式,所以才能这样.
    //s = A temp(10);

    system("pause");
    return 0;
}
```

我们发现成员变量的值被修改了. 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190801231403.png"/>



由此可知:当类构造函数的参数只有一个的时候,或者所有参数都有默认值的情况下,类A的对象时可以直接被对应的内置类型隐式转换后去赋值的,这样会造成错误,所以接下来会体现出explicit这个关键词的作用.



```cpp
#include <iostream>
using namespace std;

class A
{
public:
    //这里用explicit关键词来修饰类构造函数.
    explicit A(int i = 5, int j = 10)
    {
        m_a = i;
        m_b = j;
    }
private:
    int m_a;
    int m_b;
};

int main()
{
    A s;
    //这样直接赋值,会被提示错误,因为explicit抑制隐式转换的进行
    s = 10;//这样会报错!!!
    //当然显示转换还是可以的.
    s = A(20);

    system("pause");
    return 0;
}
```



**通过两个例子我们知道:explicit可以抑制内置类型隐式转换,所以在类的构造函数中,最好尽可能多用explicit关键字,防止不必要的隐式转换.**



## 参考博文：

因为有着热心网友的无私分享，

[C++: explicit的适用场合以及为什么要使用explicit](https://blog.csdn.net/qq_37233607/article/details/79051075)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

<br>

## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到github.io博客：**</font> ['C++: explicit的适用场合以及为什么要使用explicit'](https://blog.csdn.net/qq_33154343/article/details/98122268) 