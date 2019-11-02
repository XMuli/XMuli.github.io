---
title: Qt官方编码Style
date: 2019-10-17 00:06:50
toc: true
categories: 
 - [学习 - qt]
 - [学习 - 编码规范，辅助技巧]
tags: 
 - qt
---



**简  述：**  `Qt_Coding_Style`；`Qt`官方编码风格；

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [Qt官方编码Style](https://blog.csdn.net/qq_33154343/article/details/102568045)

<br>

## 缩进：
- 4个空格用于缩进
- 空格，而不是制表符！

<br>

## 声明变量：
-  在单独的行中声明每个变量
- 避免使用简短或无意义的名称（例如“ a”，“ rbarr”，“ nughdeget”）
- 单字符变量名称仅适用于计数器和临时变量，其中变量的用途显而易见
- 在声明变量之前等待，直到需要它为止

```cpp
// Wrong
 int a, b;
 char *c, *d;

 // Correct
 int height;
 int width;
 char *nameOfThis;
 char *nameOfThat;
```

- 变量和函数以小写字母开头。变量名称中的每个连续单词均以大写字母开头
避免缩写


```cpp
 // Wrong
 short Cntr;
 char ITEM_DELIM = ' ';

 // Correct
 short counter;
 char itemDelimiter = ' ';
```

- 类始终以大写字母开头。`Public classes` 以“ Q”（QRgb）开头，后跟一个大写字母。` Public functions`通常以“ q”（qRgb）开头。
首字母缩写使用驼峰式（例如QXmlStreamReader，而不是QXMLStreamReader）。

<br>

## 空格：
- 使用空行将语句组合在一起
- 始终只使用一个空白行
- 始终在关键字之后和花括号前使用一个空格：

```cpp
 // Wrong
 if(foo){
 }

 // Correct
 if (foo) {
 }
```

- 对于指针或引用，请始终在类型和'*'或'＆'之间使用单个空格，但在'*'或'＆'与变量名称之间请勿使用空格：

```cpp
 char *x;
 const QString &myString;
 const char * const y = "hello";
```

- 带空格的二进制运算符
- 在每个逗号后留一个空格
- 投放后没有空间
- 尽可能避免使用C型转换

```cpp
 // Wrong
 char* blockOfMemory = (char* ) malloc(data.size());

 // Correct
 char *blockOfMemory = reinterpret_cast<char *>(malloc(data.size()));
```

- 不要在一行上放置多个语句
- 通过扩展，对控制流语句的主体使用新行：

```cpp
 // Wrong
 if (foo) bar();

 // Correct
 if (foo)
     bar();
```

<br>

## 大括号：
- 使用附带的括号：开头的括号与语句的开头在同一行。如果右括号后面紧跟着另一个关键字，则它也会进入同一行：

```cpp
 // Wrong
 if (codec)
 {
 }
 else
 {
 }

 // Correct
 if (codec) {
 } else {
 }
```

- 例外：函数实现（但不包括lambdas）和类声明始终在行首处使用左括号：

```cpp
static void foo(int g)
 {
     qDebug("foo: %i", g);
 }

 class Moo
 {
 };
```

- 仅当条件语句的主体包含多行时才使用花括号：

```cpp
 // Wrong
 if (address.isEmpty()) {
     return false;
 }

 for (int i = 0; i < 10; ++i) {
     qDebug("%i", i);
 }

 // Correct
 if (address.isEmpty())
     return false;

 for (int i = 0; i < 10; ++i)
     qDebug("%i", i);
```

- 例外1：如果父语句包含多行/自动换行，也请使用花括号：

```cpp
 // Correct
 if (address.isEmpty() || !isValid()
     || !codec) {
     return false;
 }
```

- 例外2：花括号对称：在if-then-else块中，如果if代码或else代码覆盖几行，也要使用花括号：


```cpp
 // Wrong
 if (address.isEmpty())
     qDebug("empty!");
 else {
     qDebug("%s", qPrintable(address));
     it;
 }

 // Correct
 if (address.isEmpty()) {
     qDebug("empty!");
 } else {
     qDebug("%s", qPrintable(address));
     it;
 }

 // Wrong
 if (a)
     …
 else
     if (b)
         …

 // Correct
 if (a) {
     …
 } else {
     if (b)
         …
 }
```

- 条件语句的主体为空时使用花括号

```cpp
 // Wrong
 while (a);

 // Correct
 while (a) {}
```

<br>

## 括号：
- 使用括号将表达式分组：

```cpp
 // Wrong
 if (a && b || c)

 // Correct
 if ((a && b) || c)

 // Wrong
 a + b & c

 // Correct
 (a + b) & c
```

<br>

## 切换语句：
- `case`标签与开关在同一列中
- 每个`case`的末尾都必须有一个`break`（或`return`）语句，或者Q_FALLTHROUGH()用来表明没有故意的中断，除非立即发生另一`case`。

```cpp
 switch (myEnum) {
 case Value1:
   doSomething();
   break;
 case Value2:
 case Value3:
   doSomethingElse();
   Q_FALLTHROUGH();
 default:
   defaultHandling();
   break;
 }
```

<br>

## 跳转语句（中断，继续，返回和跳转）：
- 不要在跳转语句后加上“ else”：


```cpp
 // Wrong
 if (thisOrThat)
     return;
 else
     somethingElse();

 // Correct
 if (thisOrThat)
     return;
 somethingElse();
```

- 例外：如果代码本质上是对称的，则允许使用“ else”来可视化该对称性

<br>

## 换行：
- 保持少于100个字符的行；必要时包裹
- 注释/ apidoc行应保留在实际文本的80列以下。适应周围的环境，并尝试以避免出现“锯齿状”段落的方式使文本流畅。
- 逗号放在换行符的末尾；运算符从新行的开头开始。如果编辑器太窄，则很容易错过该行末尾的运算符。

```cpp
 // Wrong
 if (longExpression +
     otherLongExpression +
     otherOtherLongExpression) {
 }

 // Correct
 if (longExpression
     + otherLongExpression
     + otherOtherLongExpression) {
 }
```

<br>

## 一般例外：
- 当严格遵循规则会使您的代码看起来很糟糕时，请随意破坏它。
- 如果在任何给定的模块中存在争议，则维护者对接受的样式拥有最终决定权（根据Qt管治模型）。

<br>

## 艺术风格：
- 以下代码片段可以按艺术风格用于重新格式化代码。

```cpp
--style=kr 
--indent=spaces=4 
--align-pointer=name 
--align-reference=name 
--convert-tabs 
--attach-namespaces
--max-code-length=100 
--max-instatement-indent=120 
--pad-header
--pad-oper
```

- 注意“ unlimited” --max-instatement-indent仅用于因为如果后续行需要缩进限制，那么astyle不够聪明，无法包装第一个参数。建议您手动将语句内缩进限制为大约50个列：

```cpp
    int foo = some_really_long_function_name(and_another_one_to_drive_the_point_home(
            first_argument, second_argument, third_arugment));
```

<br>

翻译于英文官网：[https://wiki.qt.io/Qt_Coding_Style](https://wiki.qt.io/Qt_Coding_Style)