---
title: STL之类型异常处理、类型转换
date: 2019-7-10 16:04:30
updated: 2019-7-10 16:04:34
toc: true
categories: 
 - [学习 - c/c++]
---



​			**简述：** 了解STL之异常处理、类型转换、书写一个简单地例子。

<!-- more -->

[TOC]

**编程环境：**  win10 x64 专业版

**编程软件：**  visual studio 2015， Qt 5.9.8





## 本博文的简述or解决问题？

​		了解STL之异常处理、类型转换，书写一个简单地例子。

​        

下面逐一介绍这几种比较常见的知识点，算是一个知识点的几个归纳（比较简单地就没有列举出来，在此不做那种工具手册的知识罗列），知识归纳一些比较不容易的掌握的点，属于进阶知识内容。



## c/c++异常的基本语法：

### 大纲如图：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710161033.png)

### 知识点讲解：

- c方式：

```c
返回 return -1  无法辨别是返回值还是异常
```



- c++方式：

```c
try
{
	int ret = fun();
}
catch(int)
{
	cout<<"int 的异常";
}
catch(double)
{
	//double捕获，不想处理异常，就继续往上跑出异常
	throw;
}
catch(MyException e)
{
	cout<<"int 的异常";
}
catch(...)
{
	cout<<"其他类型的 的异常处理";
}

int fun()
{
	//return -1 ;   //c语言的缺陷

	//抛出异常
	//throw 1;
	//throw 3.14;
	//throw 'a';

	//栈解旋：从try代码块开始起，到throw抛出异常前，所有栈上的对象都被释放掉，
    //释放的顺序和构造的顺序是相反的，这个过程称为栈解旋

	Person p1;
	Person p2;
	throw MyException()；//抛出一个MyException匿名对象
}

//++++++++++++++++++++++++++++
void doWork()
{
	throwMyException(）；
}

void test01)
{
	try
	{
		doWork（）；
	}
	//MyException e会调用拷贝构造
	//MyException &e 引用方式接受建议用这种方式节省开销
	//MyException *e 指针方式接受抛出&MyException（）；匿名对象，对象被释放掉，不可以再操作e了
	//MyException *e 指针方式接受抛出new MyException（）；堆区创建的对象记得手动释放 deletee；
	catch （MyExceptione &e)
	{
		cout<<\"MyException的异常捕获\"<<end；
	}
}

```



**本博文同步到csdn博客：**  [STL之类型异常处理、类型转换](<https://blog.csdn.net/qq_33154343/article/details/95357944>)

