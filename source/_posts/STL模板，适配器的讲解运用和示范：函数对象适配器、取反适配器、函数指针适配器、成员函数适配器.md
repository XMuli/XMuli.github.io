---
title: STL模板，适配器的讲解运用和示范：函数对象适配器、函数指针适配器、成员函数适配器
date: 2019-7-8 16:23:15
updated: 2019-7-8 16:23:18
toc: true
categories: 
 - [学习 - c/c++]
---



​			**简述：**对于 `适配器`的函数对象适配器、取反适配器、函数指针适配器、成员函数适配器，做一个讲解和小例子的应用，并且简单地探究一下原理。

<!-- more -->

[TOC]

**编程环境：**  win10 x64 专业版

**编程软件：**  visual studio 2015， Qt 5.9.8



<font color=#D0087E size=4 face="幼圆">**更新时间：**</font>  #

<font color=#D0087E size=4 face="幼圆">**更新内容：**</font>  # 



## 本博文的简述or解决问题？

​		对于 `适配器`的函数对象适配器、取反适配器、函数指针适配器、成员函数适配器，做一个讲解和小例子的应用，并且简单地探究一下原理。

## 讲解大纲：

简单说一下，什么叫做`适配器`，先演示一下子大纲

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708171220.png)

其作用，个人理解，相当于`接口适配器`，生活中的类似于一个USB扩展坞，将电脑的一个USB3.0接口扩展成为有着多个USB3.0接口的排插。code里面相当于反过来看，将二个参数合并成为一个参数，进行输入输出。

**例子：** 将`函数指针 + 变量`通过bind绑定成为一个`对象参数`。



## 函数对象适配器

#### 初始代码：

初始代码：这一个打印的结果，后面的修改，在这里代码之上修改和演示

```c
using namespace std;
#include <vector>
#include <algorithm>  //算法
#include <functional>  //内建函数对象

class MyPrint
{
public:
	void operator()(int val) {cout << val << "   ";}
};

int main()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(3);
	v.push_back(4);
	v.push_back(2);
	v.push_back(6);
	v.push_back(5);
	
	for_each(v.begin(), v.end(), MyPrint());
	cout << endl;

	return 0;
}
```

运行结果:

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708173610.png)



#### 使用步骤：

1. 将参数进行绑定 `bind2nd()`
2. 做继承  `binary_function<类型1， 类型2， 返回值类型>`
3. 加 `const`，因为继承`binary_function`时候，其作为父类已经`重载()`作为常函数

配图文解说，这个算是图文并茂了

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708175525.png)



#### 代码演示：

创建一个控的项目，再添加一个空的main.cpp,在里面添加如下代码:

```c
#pragma once
#include <iostream>
using namespace std;
#include <vector>
#include <algorithm>  //算法
#include <functional>  //内建函数对象
#include <functional>  //适配器

//函数对象适配器
class MyPrint: public binary_function<int, int, void>
{
public:
	void operator()(int val, int nBase) const
	{
		cout << "val:" << val << "   nBase:" << nBase << "   val+nBase:" << val + nBase << endl;
	}
};

int main()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(3);
	v.push_back(4);
	v.push_back(2);
	v.push_back(6);
	v.push_back(5);
	
	int nBase = 100;
	for_each(v.begin(), v.end(), bind2nd(MyPrint(), nBase) );

	return 0;
}
```



#### 运行演示：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708193108.png)



#### 细节方面：

将上面30行代码做如下的变动，再次运行，观察和上一次的结果**稍有区别**

```c
//for_each(v.begin(), v.end(), bind2nd(MyPrint(), nBase) );
for_each(v.begin(), v.end(), bind1st(MyPrint(), nBase));
```



![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708175831.png)

#### 心得思考：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708190801.png)



## 取反适配器：

### 初始代码：

初始代码：这一个打印的结果，后面的修改，在这里代码之上修改和演示

```c
using namespace std;
#include <vector>
#include <algorithm>  //算法
#include <functional>  //内建函数对象

//02取反适配器
class MyFindFour
{
public:
	bool operator()(int val)  {return val > 4;}
};

void test02()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(3);
	v.push_back(4);
	v.push_back(2);
	v.push_back(6);
	v.push_back(5);

	vector<int>::iterator pos = find_if(v.begin(), v.end(), MyFindFour());

	if (pos != v.end())
		cout << "大于4的数值为:" << *pos << endl; 
	else
		cout << "无"  << endl;
}

int main()
{
	test02();
	return 0;
}
```



### 运行结果: 

***“1  3  4  2  6  5”从左往右开始遍历***，第一个大于4的数值就是6

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708181248.png)





### 使用步骤：

1. 一元取反 `not1`
2. 继承 `unary_function`
3. 加 `const`

如上三处修改，也可参照上面的***函数对象适配器***的 图文并茂的这一部分，理解修改的部分。



### 代码演示：

创建一个控的项目，再添加一个空的main.cpp,在里面添加如下代码:

```c
#pragma once
#include <iostream>
using namespace std;
#include <vector>
#include <algorithm>  //算法
#include <functional>  //内建函数对象
#include <functional>  //适配器

//02取反适配器
class MyFindFour: public unary_function<int, bool>
{
public:
	bool operator()(int val)  const
	{return val > 4;} 
};

void test02()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(3);
	v.push_back(4);
	v.push_back(2);
	v.push_back(6);
	v.push_back(5);

	vector<int>::iterator pos = find_if(v.begin(), v.end(), not1(MyFindFour()));

	if (pos != v.end())
		cout << "大(划掉，改小)于4的数值为:" << *pos << endl; 
	else
		cout << "无"  << endl;
}

int main()
{
	test02();
	return 0;
}
```



### 运行演示：

运行结果: ***“1  3  4  2  6  5”从左往右开始遍历***，第一个小于4的数值就是1

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708182733.png)

### 细节方面：

将上面的27行代码做如下的变动，再次运行，观察和上一次的结果**没有区别**，但是写法却更加高级，越加活学活用。

这样写的优点：可以将10行`class MyFindFour: public unary_function<int, bool>`里面的具体数字4，写到外面来，可以修改成任意的数值，不再受局限

```c
//vector<int>::iterator pos = find_if(v.begin(), v.end(), not1(MyFindFour()));
vector<int>::iterator pos = find_if(v.begin(), v.end(), not1(bind2nd(greater<int>(), 4)));
```

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708182733.png)

### 心得思考：

### 注意区别：



## 函数指针适配器：

### 初始代码：

初始代码：这一个打印的结果，后面的修改，在这里代码之上修改和演示

```c
#pragma once
#include <iostream>
using namespace std;
#include <vector>
#include <algorithm>  //算法
#include <functional>  //内建函数对象
#include <functional>  //适配器

//03函数指针适配器
void MyPrintf(int val)
{
	cout << val<<"  ";
};

void test03()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(3);
	v.push_back(4);
	v.push_back(2);
	v.push_back(6);
	v.push_back(5);

	for_each(v.begin(), v.end(), MyPrintf);   //注意这一行，参数3 是使用的函数指针
	cout << endl;

}

int main()
{
	test03();
	return 0;
}
```

### 运行结果:

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708183636.png)



### 使用步骤：

1. 先使用`ptr_fun()`将`MyPrintf函数指针`便成为**对象**，
2. 然后再用`bind2nd()`绑定
3. 修改需要绑定多个参数的`函数MyPrintf()`



### 代码演示：

创建一个控的项目，再添加一个空的main.cpp,在里面添加如下代码:

```c
#pragma once
#include <iostream>
using namespace std;
#include <vector>
#include <algorithm>  //算法
#include <functional>  //内建函数对象
#include <functional>  //适配器

//03函数指针适配器
void MyPrintf(int val, int nBase)
{
	cout << "val:" << val << "   nBase:" << nBase << "   val+nBase:" << val + nBase << endl;
};

void test03()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(3);
	v.push_back(4);
	v.push_back(2);
	v.push_back(6);
	v.push_back(5);

	int nBase = 1000;
	for_each(v.begin(), v.end(), bind2nd(ptr_fun(MyPrintf), nBase) );  //注意这一行，参数3 是使用的函数指针
	cout << endl;

}

int main()
{
	test03();
	return 0;
}
```



### 运行演示：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708184240.png)

### 细节方面：

注意这一行是怎么修改的，以及还需要修改的地方

```c
for_each(v.begin(), v.end(), bind2nd(ptr_fun(MyPrintf), nBase) );  //注意这一行，参数3 是使用的函数指针
```



### 心得思考：



## 成员函数适配器：

### 初始代码：

初始代码：这一个打印的结果，后面的修改，在这里代码之上修改和演示

```c
#pragma once
#include <iostream>
using namespace std;
#include <vector>
#include <algorithm>  //算法
#include <functional>  //内建函数对象
#include <functional>  //适配器
#include <string>

//04成员函数适配器
class Person
{
public:
	Person(string name, int age) :m_strName(name), m_nAge(age) {}
	~Person() {}

public:
	string m_strName;
	int m_nAge;
};

void MyPrintf04(Person& p)
{
	cout << "函数MyPrintf04==》  姓名：" << p.m_strName << "   年龄：" << p.m_nAge << endl;
}

void test04()
{
	Person p1("p1", 11);
	Person p2("p2", 12);
	Person p3("p3", 13);
	Person p4("p4", 14);
	Person p5("p5", 15);
	Person p6("p6", 16);

	vector<Person> v;
	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);
	v.push_back(p6);

	for_each(v.begin(), v.end(), MyPrintf04);  //注意这一行，参数3 是使用的函数指针
	cout << endl;
}

int main()
{
	test04();
	return 0;
}
```



### 运行结果:

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708185700.png)



### 使用步骤：

1. 取到成员函数的地址`&Person::MyPrintfPerson`
2. 通过`mem_fun_ref()`对成员函数进行适配



### 代码演示：

创建一个控的项目，再添加一个空的main.cpp,在里面添加如下代码:

```c
#pragma once
#include <iostream>
using namespace std;
#include <vector>
#include <algorithm>  //算法
#include <functional>  //内建函数对象
#include <functional>  //适配器
#include <string>

//04成员函数适配器
class Person
{
public:
	Person(string name, int age) :m_strName(name), m_nAge(age) {}
	~Person() {}

	void MyPrintfPerson()
	{
		cout << "类的成员函数MyPrintfPerson==》  姓名：" << m_strName << "   年龄：" << m_nAge << endl;
	}

public:
	string m_strName;
	int m_nAge;
};

void MyPrintf04(Person& p)
{
	cout << "函数MyPrintf04==》  姓名：" << p.m_strName << "   年龄：" << p.m_nAge << endl;
}

void test04()
{
	Person p1("p1", 11);
	Person p2("p2", 12);
	Person p3("p3", 13);
	Person p4("p4", 14);
	Person p5("p5", 15);
	Person p6("p6", 16);

	vector<Person> v;
	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);
	v.push_back(p6);

	//for_each(v.begin(), v.end(), MyPrintf04);  //注意这一行，参数3 是使用的函数指针
	for_each(v.begin(), v.end(), mem_fun_ref(&Person::MyPrintfPerson));  //注意这一行，参数3 是使用的函数指针
	cout << endl;
}

int main()
{
	test04();
	return 0;
}
```



### 运行演示：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708190104.png)

### 细节方面：

不用再去修改类的成员函数



### 心得思考：

这个算是这些里面，最简单方便的一个方法了的吧，个人感觉。





## 邂逅感：

写完这一篇博客，感觉耗费时间还是比较久的，虽然知识点比较简单，但是为了简单说清楚，图文并茂，还是花了两个小时的。算是比较适合对新人小白入门的一个友好的教程，毕竟第一次接触c++的适配器，可能会感觉比较玄乎，希望这一篇可以帮到大家。（如果觉得真的有帮助到你，还觉得本文写的不错，可以适当点个赞，让更多的人可以看到。觉得一般的话，可以看过就就过了。）

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190708191412.png)

<font color=#D0087E size=4 face="幼圆">当看到弄清知识点的时候，或者帮助到有需要的人的时候，毕竟我的心里是开心的，然后接着继续敲敲敲！！！</font>

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/gif/20190704180218.gif)





## 代码下载：

想一了下，虽然上面都还是单独可以运行代码，当时是决定把代码集合发布出来，可以当做一个代码查看：与那比较四种不同的适配器的区别：

```c
#pragma once
#include <iostream>
using namespace std;
#include <vector>
#include <algorithm>  //算法
#include <functional>  //内建函数对象
#include <functional>  //适配器
#include <string>

//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//01 函数对象适配器
class MyPrint: public binary_function<int, int, void>
{
public:
	void operator()(int val, int nBase) const
	{cout << "val:" << val << "   nBase:" << nBase << "   val+nBase:" << val + nBase << endl;}
};

void test01()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(3);
	v.push_back(4);
	v.push_back(2);
	v.push_back(6);
	v.push_back(5);

	int nBase = 100;
	for_each(v.begin(), v.end(), bind2nd(MyPrint(), nBase) );
	//for_each(v.begin(), v.end(), bind1st(MyPrint(), nBase));
}

//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//02取反适配器
class MyFindFour: public unary_function<int, bool>
{
public:
	bool operator()(int val)  const
	{return val > 4;} 
};

void test02()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(3);
	v.push_back(4);
	v.push_back(2);
	v.push_back(6);
	v.push_back(5);

	//vector<int>::iterator pos = find_if(v.begin(), v.end(), not1(MyFindFour()));
	vector<int>::iterator pos = find_if(v.begin(), v.end(), not1(bind2nd(greater<int>(), 4)));

	if (pos != v.end())
		cout << "大(划掉，改小)于4的数值为:" << *pos << endl; 
	else
		cout << "无"  << endl;
}


//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//03函数指针适配器
void MyPrintf(int val, int nBase)
{ cout << "val:" << val << "   nBase:" << nBase << "   val+nBase:" << val + nBase << endl; };

void test03()
{
	vector<int> v;
	v.push_back(1);
	v.push_back(3);
	v.push_back(4);
	v.push_back(2);
	v.push_back(6);
	v.push_back(5);

	int nBase = 1000;
	for_each(v.begin(), v.end(), bind2nd(ptr_fun(MyPrintf), nBase) );  //注意这一行，参数3 是使用的函数指针
	cout << endl;
}

//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
//04成员函数适配器
class Person
{
public:
	Person(string name, int age) :m_strName(name), m_nAge(age) {}
	~Person() {}

	void MyPrintfPerson()
	{ cout << "类的成员函数MyPrintfPerson==》  姓名：" << m_strName << "   年龄：" << m_nAge << endl; }

public:
	string m_strName;
	int m_nAge;
};

void MyPrintf04(Person& p)
{ cout << "函数MyPrintf04==》  姓名：" << p.m_strName << "   年龄：" << p.m_nAge << endl; }



void test04()
{
	Person p1("p1", 11);
	Person p2("p2", 12);
	Person p3("p3", 13);
	Person p4("p4", 14);
	Person p5("p5", 15);
	Person p6("p6", 16);

	vector<Person> v;
	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	v.push_back(p5);
	v.push_back(p6);

	//for_each(v.begin(), v.end(), MyPrintf04);  //注意这一行，参数3 是使用的函数指针
	for_each(v.begin(), v.end(), mem_fun_ref(&Person::MyPrintfPerson));  //注意这一行，参数3 是使用的函数指针

	cout << endl;
}
//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

int main()
{
	//test01();
	//test02();
	//test03();
	test04();

	return 0;
}
```



------



**本博文同步到csdn博客：** [TL模板，适配器的讲解运用和示范：函数对象适配器、取反适配器、函数指针适配器、成员函数适配器](https://blog.csdn.net/qq_33154343/article/details/95090677)

