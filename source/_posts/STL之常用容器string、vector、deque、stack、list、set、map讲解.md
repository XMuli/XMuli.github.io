---
title: STL之常用容器string、vector、deque、stack、list、set、map讲解
date: 2019-7-10 16:04:30
updated: 2019-7-10 16:04:34
toc: true
categories: 
 - [学习 - c/c++]
---



​	**简述：** 了解STL之常用容器string、vector、deque、stack、list、set、map讲解书写一个简单地例子。

<!-- more -->

[TOC]

**编程环境：**  win10 x64 专业版

**编程软件：**  visual studio 2015， Qt 5.9.8





## 本博文的简述or解决问题？

了解STL之常用容器string、vector、deque、stack、list、set、map讲解，且书写一个简单地例子。

​        

下面逐一介绍这几种比较常见的知识点，算是一个知识点的几个归纳（比较简单地就没有列举出来，在此不做那种工具手册的知识罗列），知识归纳一些比较不容易的掌握的点，属于进阶知识内容。



## 容器：

### 大纲如图：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710162226.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710162255.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710162327.png)



### 知识点讲解：

其中这六种的主要区别如下：**它们的使用时机**

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710162708.png)



------

### string：

比较简单，注意一下：[ ]和at( )区别：前者会越界直接挂掉，后者会抛出异常out_of_range即可

------

### vector：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710163212.png)

#### 内存结构样子：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710163750.png)





收缩内存技巧:

关于<font color=#FE7207  size=4 face="幼圆">收缩内存</font>一个技巧使用：

```c
vectoKint>v;
for(int i=0;i<100000;i++)
	v.pusthback(i)；

cout<<"v的容量："<<v.capacity()<<endl；
cout<<"v的大小："<<v.size)<<endd；

v.resize(3)；

cout<<"v的容量："<<v.capacity()<<endl；
cout<<"v的大小："<<v.size()<<endl；

//收缩内存
ecto<int>(v).swap(v);
cout<<"v的容量："<<v.capacity(）<<endl；
cout<<"v的大小："Kv.size(）<<endd；
```

#### 原理图示：![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710164014.png)

------

### deque：

（双端 中控器 分段 数组）

#### 内存结构样子：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710163318.png)

------

### stack：

（栈）

#### 内存结构样子：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710163423.png)

------

### queue：

（队、先进先出）

#### 内存结构样子：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710163523.png)



------

### list：

（链表）

#### 内存结构样子：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710163613.png)



#### 验证list是一个验证list是循环双向链表：

```c
#include <iostream>
using namespace std;
#include <list>

int main()
{
	
	list<int> mylist;
	for (int i = 0; i < 10; i++)
		mylist.push_back(i);

	list<int>::_Nodeptr node = mylist._Myhead()->_Next;

	for (int i = 0; i < mylist._Mysize() * 2; i++)
	{
		cout << "Node:" << node->_Myval << endl;
		node = node->_Next;

		if (node == mylist._Myhead())
			node = node->_Next;
	}

	return 0;
}
```

#### 运行结果:

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710164835.png)

------

### set/multiset：

（底层均为红黑树[平衡二叉树的一种]）

#### 平衡二叉树讲解：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710163708.png)

#### set插入自定义数据类型：

```c
#include <iostream>
using namespace std;
#include <set>  //set和multiset均使用此头文件
#include <string>

//自定义类型
class Person  
{
public:
	Person(string name, int age, int height):m_strName(name), m_nAge(age), m_nHeight(height){}
	~Person(){}

public:
	string m_strName;
	int m_nAge;
	int m_nHeight;

};

//利用仿函数 指定set容器的排序
class MyCompare 
{
public:
	bool operator()(Person p1, Person p2)
	{
		if (p1.m_nAge == p2.m_nAge)
		{
			return p1.m_nHeight > p2.m_nHeight;
		}
		
		return p1.m_nAge < p2.m_nAge;  //年龄从小到大排序
	}

};

void printfset(multiset<Person, MyCompare>& s)
{
	for (multiset<Person, MyCompare>::iterator it = s.begin(); it != s.end(); it++)
	{
		cout << "姓名：" << it->m_strName << "  年龄：" << it->m_nAge << " 身高：" << it->m_nHeight << endl;
	}
	cout << endl;
}



int main()
{
	Person p1("p1", 12, 109);
	Person p2("p2", 13, 109);
	Person p3("p3", 13, 180);
	Person p4("p4", 13,  79);
	Person p5("p5", 17, 109);
	Person p6("p6", 10, 99);
	Person p7("p7",  6, 199);

	//set<Person> s;  //直接写，不知按照什么“大小规则”进行插入
	//set<Person, MyCompare> s;   //加入仿函数， 使用“自定义规则”插入排序
	multiset<Person, MyCompare> s;   
	s.insert(p1);
	s.insert(p2);
	s.insert(p3);
	s.insert(p4);
	s.insert(p5);
	s.insert(p6);
	s.insert(p7);

	printfset(s);
	return 0;
}
```

#### 运行结果：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710165205.png)

------

### map/multimap：

（底层均为红黑树[平衡二叉树的一种]）

#### multimap示例使用：

```c
#pragma once

#include <iostream>
using namespace std;
#include <string>
#include <map>

class Person
{
public:
	Person(string name, int age, string tel, int salary):m_strName(name), m_nAge(age), 
		m_strTel(tel), m_nSalary(salary){}
	~Person(){}

	string m_strName;
	int m_nAge;
	string m_strTel;
	int m_nSalary;

};

int main()
{
	//自定义数据类型
	//创建对象
	//仿函数（或回调函数）
	//插入
	//按照部门输出
	Person p1("p1", 12, "0001", 110);
	Person p2("p2", 13, "0002", 109);
	Person p3("p3", 13, "0003", 180);
	Person p4("p4", 13, "0004",  79);
	Person p5("p5", 17, "0005", 109);
	Person p6("p6", 10, "0006",  99);
	Person p7("p7",  6, "0007", 199);

	multimap<int, Person> m;
	m.insert(make_pair(1, p1));
	m.insert(make_pair(1, p2));
	m.insert(make_pair(2, p3));
	m.insert(make_pair(2, p4));
	m.insert(make_pair(4, p5));
	m.insert(make_pair(3, p6));
	m.insert(make_pair(2, p7));

	for (multimap<int, Person>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "所属部门：" << it->first << "   姓名：" << it->second.m_strName << "   年龄：" << it->second.m_nAge
			<< "   电话号码：" << it->second.m_strTel << "   薪资：" << it->second.m_nSalary << endl;
	}

	return 0;
}
```

#### 运行结果：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190710165347.png)



## 本次心得：

学习是是一件让人开心的事情，但是写成博文，就是一件让人痛苦中快乐的事情



<font color=#D0087E size=4 face="幼圆">当看到运行OK那的一刻，心里或许就是这样的</font>

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/gif/20190704175742.gif)





------



**本博文同步到csdn博客：**  [STL之常用容器string、vector、deque、stack、list、set、map讲解](https://blog.csdn.net/qq_33154343/article/details/95358234)

