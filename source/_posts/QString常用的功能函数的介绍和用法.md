---
title: QString常用的功能函数的介绍和用法
date: 2019-9-15 18:19:02
toc: true
categories: 
 - [学习 - qt]
 - [专栏 - Qt推倒重学系列]
tags: 
 - qt控件
---



**简介：**  `QString`常用的功能函数的介绍和用法。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		**详情：**  见简介

<br>

**编程环境：**  `win10 x64 专业版 1803`  

**编程软件：**  `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## 运行效果：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190914233130.gif"/>

<br>

## 同系列文章：

[`QString`在2 ／8／10／16进制之间转换](https://blog.csdn.net/qq_33154343/article/details/100860030) 

<br>

## QString：

`QString`存储字符串采用的是**Unicode**码，每一个字符是一个16位的`QChar`，而不是8位的`char`，所以`QString`处理中文字符没有问题，而且一个汉字算作是一个字符。

<br>

## 常用函数：

<font color=#70AD47 size=4 face="幼圆">**注意：QString只要赋值，就在字符串的末尾自动加上“\\0”**</font>

### 字符串相关:

- append()        在字符串后面添加字符串
- perpend()      在字符串的前面添加字符串
- toUpper()      将字符串的字母全部转换为大写字母
- toLower()      将字符串的字母全部转换为大写字母
- left()               返回包含字符串中最左n个字符的子字符串。如果n大于或等于size()或小于零，则返回整个字符串。
- right()            返回包含字符串中最右n个字符的子字符串。如果n大于或等于size()或小于零，则返回整个字符串。
- section()        从字符串中提取以“子字符串”作为分隔符，从start到end端的字符串
- simplified()   不仅去掉字符串的所首尾空格，中间连续的空格也用一个空格替换
- trimmed        去掉字符串首尾的空格

<br>

### 数字相关：

- count()               返回字符串的字符个数。函数同size()、同length()。(字符串中若有汉字，一个汉字算一个字符）
- size()                  同上
- indexOf()           在字符串中查找子字符串str出现的位置。（Qt::CaseSensitivity cs 参数指定是否区分大小写）

- lastIndexOf()    在字符串中查找子字符串str最后出现的位置

<br>

### 逻辑判断：

- startsWith()     判断是否以某个字符串开头
- endsWith()      判断是否以某个字符串结尾
- contains()        判断某个字符串中是否包含某个字符串
- isNull()             判断字符串是否为空。（若是只有“\\0”，isNull返回false； 只有未赋值的字符串，isNull返回true）
- isEmpty()         判断字符串是否为空.（若是只有“\\0”，isEmpty返回true）



<font color=#FE7207  size=4 face="幼圆">**isNull()和isEmpty()的区别：**</font>

```cpp
isNull()和isEmpty()
两个函数都判读字符串是否为空，但是稍有差别。如果一个空字符串，只有“0”，isNull)
返回false，而isEmpty0返回true；只有未赋值的字符串，isNull（）才返回true。
QString strl,str2=""；
N=str1.isNul1()；     //N=true未赋值字符串变量
N=str2.isNull()；     //N=false只有“\\0”的字符串，也不是Nul1
N=strl.isEmpty();     //N=true
N=str2.isEmpty()；    //N=true

QString只要赋值，就在字符串的末尾自动加上“10”，所以，如果只是要判断字符串内容是
否为空，常用isEmptyO。

```

<br>

## 源码下载：

[https://github.com/xmuli/QtExamples](https://github.com/xmuli/QtExamples) 【QtQStringFunEx】

<br>

## 开心分享：

<font color=#D0087E size=4 face="幼圆">因为有着许许多多的热心网友的无私分享，从他们的博客中学习成长，学会很多，故也不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 </font>
