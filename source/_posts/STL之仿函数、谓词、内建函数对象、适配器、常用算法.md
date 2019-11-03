---
title: STL之仿函数、谓词、内建函数对象、适配器、常用算法
date: 2019-7-10 16:04:30
updated: 2019-7-10 16:04:34
toc: true
categories: 
 - [学习 - c/c++]
---



​			**简述：** 了解STL之仿函数、谓词、、内建函数对象、适配器、常用算法归纳等，书写一个简单地例子。

<!-- more -->

[TOC]

**编程环境：**  win10 x64 专业版

**编程软件：**  visual studio 2015， Qt 5.9.8





## 本博文的简述or解决问题？

​		了解STL之仿函数、谓词、、内建函数对象、适配器、常用算法归纳等，书写一个简单地例子。其中适配器在该文章中有详细讲：[STL之函数对象适配器、取反适配器、函数指针适配器、成员函数适配器的讲解运用](https://blog.csdn.net/qq_33154343/article/details/95090677)

下面逐一介绍这几种比较常见的知识点，算是一个知识点的几个归纳（比较简单地就没有列举出来，在此不做那种工具手册的知识罗列），知识归纳一些比较不容易的掌握的点，属于进阶知识内容。





## 函数对象/谓词/内建函数对象/适配器：

### 大纲如图：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710170721.png)

### 函数对象：

总结：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710170956.png)



### 谓词:

定义:

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710171104.png)



### 内建函数对象：

定义：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710171203.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710171359.jpg)



#### 代码演示：

```c
#pragma once

#include <iostream>
using namespace std;
#include <vector>
#include <algorithm>  //算法
#include <functional>  //内建函数对象

int main()
{
	vector<int> v;
	v.push_back(10);
	v.push_back(30);
	v.push_back(40);
	v.push_back(20);
	v.push_back(60);
	v.push_back(50);

	sort(v.begin(), v.end(), greater<int>());   //内建关系函数对象（无名 临时对象）greater<int>()   大于
	for_each(v.begin(), v.end(), [](int val) {cout << val << endl; });
	
	return 0;
}
```

#### 运行结果:

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710172037.png)



## 常用算法：

### 大纲如图：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710170815.png)

### 知识点讲解：

如上图

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/gif/20190704175742.gif)

------



**本博文同步到csdn博客：**  [STL之仿函数、谓词、内建函数对象、适配器、常用算法](https://blog.csdn.net/qq_33154343/article/details/95358967)