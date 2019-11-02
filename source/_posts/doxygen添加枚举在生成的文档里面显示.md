---
title: doxygen添加枚举在生成的文档里面显示
date: 2019-10-29 22:03:49
toc: true
categories: 
 - [学习 - doxygen]
tags: 
 - doxygen
---

**简  述：**  类里面有着**枚举**；想要在生成的文件`html`文件可以直接预览

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [doxygen添加枚举在生成的文档里面显示](https://blog.csdn.net/qq_33154343/article/details/102809718)

<br>

在类里面有**枚举**数值, 但是需要在生成html里面显示出来; 其中类里面的枚举如下:

```cpp
class DFloatingMessage : public DFloatingWidget
{
...
public:
    enum MessageType {
        TransientType,  //临时的消息
        ResidentType    //常驻的消息
        };
...
}
```
在对应的项目工程中, 可以写下如下的注释代码:

```cpp
/*!
 *
 * \~chinese \enum DFloatingMessage::MessageType
 * \~chinese DFloatingMessage::MessageType 定义了 DFloatingMessage 通知类型
 * \~chinese \var DFloatingMessage:MessageType DFloatingMessage::TransientType
 * \~chinese 临时的消息
 * \~chinese \var DDFloatingMessage:MessageType DFloatingMessage::ResidentType
 * \~chinese 常驻的消息
 */
```
生成的效果图如下:

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img2/20191029220402.png"/>

