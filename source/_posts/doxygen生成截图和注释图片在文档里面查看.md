---
title: doxygen生成截图和注释图片在文档里面查看
date: 2019年10月29日21:27:43
toc: true
categories: 
 - [学习 - doxygen]
tags: 
 - doxygen
---

**简  述：**  在生成的文件`html`文件可以直接预览到插入的截图图片可以被查看，且手写的注释文档图片也可以被查看

<!-- more -->

[TOC]

## <font color=#D0087E  face="幼圆">同步博文：</font>

- <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font> [doxygen生成截图和注释图片在文档里面查看](https://blog.csdn.net/qq_33154343/article/details/102809510)

<br>

### 添加注释型图片:
添加的代码注释如下:

```cpp
/*!
* \~chinese \class QTest
* \~chinese \brief 在此处描述这个类的详细介绍
*
* \~chinese \htmlonly
* <pre style="font-family: FreeMono, Consolas, Menlo, 'Noto Mono', 'Courier New', Courier, monospace;line-height: 100%;">
* 　　　┏┓　　　┏┓
* 　　┏┛┻━━━┛┻┓
* 　　┃　　　　　　　 ┃
* 　　┃　　　━　　　 ┃
* 　　┃　＞　　　＜┃
* 　　┃　　　　　　　 ┃
* 　　┃ . ⌒　..┃
* 　　┃　　　　　　　 ┃
* 　　┗━┓　　　┏━┛
* 　　　　┃　　　┃　Codes are far away from bugs with the animal protecting
* 　　　　┃　　　┃ 神兽保佑,代码无bug
* 　　　　┃　　　┃
* 　　　　┃　　　┃
* 　　　　┃　　　┃
* 　　　　┃　　　┃
* 　　　　┃　　　┗━━━┓
* 　　　　┃　　　　　　　┣┓
* 　　　　┃　　　　　　　┏┛
* 　　　　┗┓┓┏━┳┓┏┛
* 　　　　　┃┫┫　┃┫┫
* 　　　　　┗┻┛　┗┻┛
* </pre>
* \endhtmlonly
* \~chinese 这里什么都不想写.... know, 且和你说, 写代码注释很容易犯困
*/
```

* 效果图片

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img2/20191029215730.png"/>

<br>

### 添加真实型图片:

* 修改`Doxyfile`文件, 将存放图片的路径添加到`IMAGE_PATH = ` 后面

添加放入图片的路径; 若是存放父路径 "./doc" 也可以;  向下遍历查询到放的图片. 这里的路径是你存放静态图片的路径,  博主为"当前工程项目/doc/images"这个路径

```bash
IMAGE_PATH             = ./doc/images 
```
* 在工程项目里面,  具体的`*.cpp` 文件里面, 添加符合(doxygen)规范的注释,  便于生成

实际用法:

>cd doc/image
>用法: image htmp 图片.png

eg:

```cpp
/*!
 * \~chinese \class QTest
 * \~chinese \brief 这是一个演示的类的详细描述
 * \chiinese \image html gril.png
 */
```
* 然后执行生成文件即可,  查看预览效果

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/dev/img2/20191029215808.png"/>

