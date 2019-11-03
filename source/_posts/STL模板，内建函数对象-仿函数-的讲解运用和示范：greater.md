---
title: STL模板，内建函数对象(仿函数)的讲解运用和示范：greater()
date: 2019-7-8 16:23:15
updated: 2019-7-8 16:23:18
toc: true
categories: 
 - [学习 - c/c++]
---



​			**简述：** 对于 `内建函数对象`（#include <functional>）的算术、关系、逻辑类的函数对象（`仿函`数），做一个讲解和小例子的应用，并且简单地探究一下原理。

<!-- more -->

[TOC]

**编程环境：**  win10 x64 专业版

**编程软件：**  visual studio 2015， Qt 5.9.8



<font color=#D0087E size=4 face="幼圆">**更新时间：**</font>  #

<font color=#D0087E size=4 face="幼圆">**更新内容：**</font>  # 



## 本博文的简述or解决问题？

​		对于 `内建函数对象`（#include <functional>）的算术、关系、逻辑类的函数对象（`仿函`数），做一个讲解和小例子的应用，并且简单地探究一下原理。



## 讲解大纲：

简单说一下，什么叫做`内建函数对象`![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708162640.png)

其中完整的定义如下：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708162823.png)



其所拥有的具体的所有的函数接口，如下图所示。在这里只是将其中几个典型的接口是实例一下如何使用。

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708162856.png)



## 关系运算类函数对象：

### 大于仿函数：

template<class> T greater<T> //大于仿函数

#### 心得思考：

添加头文件不可忘记，要各自对应，在使用`sort(三个参数)`的时候，<font color=#FE7207 size=3 face="幼圆">第三个参数，通常为自定义的  函数对象（仿函数）</font>。因为默认使用`sort(两个参数)`的使用，<font color=#FE7207  size=3 face="幼圆">其底层也是默认调用的 系统的默认的内建函数对象`less<>()`(六个关系运算类函数对象的倒数第二个)</font>。

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708164909.png)



#### 代码演示：

创建一个控的项目，再添加一个空的main.cpp,在里面添加如下代码:

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



#### 运行演示：

默认排序是从小到大，而这里结果是从大到小，复合结果的预期。

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708193257.png)

#### 细节方面：

```c
sort(v.begin(), v.end(), greater<int>());   //内建关系函数对象（无名 临时对象）greater<int>()   大于   //内建关系函数对象（无名 临时对象）greater<int>()   大于
```

可以替换成，

```c
sort(v.begin(), v.end());
```

再查看运行结果为：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708165544.png)





## 算数类函数对象：

###  取反仿函数：

（template<class> T negate<T> //取反仿函数）

仿照上面，重点会在下面的一篇文章[STL模板，适配器的讲解运用和示范：函数对象适配器、、函数指针适配器、成员函数适配器](https://touwoyimuli.github.io/2019/07/08/STL%E6%A8%A1%E6%9D%BF%EF%BC%8C%E9%80%82%E9%85%8D%E5%99%A8%E7%9A%84%E8%AE%B2%E8%A7%A3%E8%BF%90%E7%94%A8%E5%92%8C%E7%A4%BA%E8%8C%83%EF%BC%9A%E5%87%BD%E6%95%B0%E5%AF%B9%E8%B1%A1%E9%80%82%E9%85%8D%E5%99%A8%E3%80%81%E5%8F%96%E5%8F%8D%E9%80%82%E9%85%8D%E5%99%A8%E3%80%81%E5%87%BD%E6%95%B0%E6%8C%87%E9%92%88%E9%80%82%E9%85%8D%E5%99%A8%E3%80%81%E6%88%90%E5%91%98%E5%87%BD%E6%95%B0%E9%80%82%E9%85%8D%E5%99%A8/)，适配器相关里面再继续运行用到。



### 加法仿函数：

（template<class> T plus<T> //加法仿函数） 

仿照上面，重点会在下面的一篇文章[STL模板，适配器的讲解运用和示范：函数对象适配器、、函数指针适配器、成员函数适配器](https://touwoyimuli.github.io/2019/07/08/STL%E6%A8%A1%E6%9D%BF%EF%BC%8C%E9%80%82%E9%85%8D%E5%99%A8%E7%9A%84%E8%AE%B2%E8%A7%A3%E8%BF%90%E7%94%A8%E5%92%8C%E7%A4%BA%E8%8C%83%EF%BC%9A%E5%87%BD%E6%95%B0%E5%AF%B9%E8%B1%A1%E9%80%82%E9%85%8D%E5%99%A8%E3%80%81%E5%8F%96%E5%8F%8D%E9%80%82%E9%85%8D%E5%99%A8%E3%80%81%E5%87%BD%E6%95%B0%E6%8C%87%E9%92%88%E9%80%82%E9%85%8D%E5%99%A8%E3%80%81%E6%88%90%E5%91%98%E5%87%BD%E6%95%B0%E9%80%82%E9%85%8D%E5%99%A8/)，适配器相关里面再继续运行用到。



## 逻辑运算类仿函数 ：

仿照上面，重点会在下面的一篇文章[STL模板，适配器的讲解运用和示范：函数对象适配器、、函数指针适配器、成员函数适配器](https://touwoyimuli.github.io/2019/07/08/STL%E6%A8%A1%E6%9D%BF%EF%BC%8C%E9%80%82%E9%85%8D%E5%99%A8%E7%9A%84%E8%AE%B2%E8%A7%A3%E8%BF%90%E7%94%A8%E5%92%8C%E7%A4%BA%E8%8C%83%EF%BC%9A%E5%87%BD%E6%95%B0%E5%AF%B9%E8%B1%A1%E9%80%82%E9%85%8D%E5%99%A8%E3%80%81%E5%8F%96%E5%8F%8D%E9%80%82%E9%85%8D%E5%99%A8%E3%80%81%E5%87%BD%E6%95%B0%E6%8C%87%E9%92%88%E9%80%82%E9%85%8D%E5%99%A8%E3%80%81%E6%88%90%E5%91%98%E5%87%BD%E6%95%B0%E9%80%82%E9%85%8D%E5%99%A8/)，适配器相关里面再继续运行用到。



## 使用无名的临时对象：

这是一个很重要的骚操作。



<font color=#D0087E size=4 face="幼圆">当看到运行符合预想那的一刻，心里是开心的，然后接着继续敲敲敲！！！</font>

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/gif/20190704180218.gif)







------



**本博文同步到csdn博客：** [STL模板，内建函数对象(仿函数)的讲解运用和示范：greater()](https://blog.csdn.net/qq_33154343/article/details/95089598)

