---
title: OpenMesh入门，安装，运行示例Hello World
date: 2019-6-21 15:44:35
updated: 2019-6-21 15:44:35
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - OpenMesh]
 
tags: 
 - c/c++
 - OpenMesh入门
---



​		了解OpenMesh，学会安装，尝试在c++里面使用OpenMesh，书写一个简单地例子。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		了解OpenMesh，学会安装，尝试在c++里面使用OpenMesh，书写一个简单地例子。



**编程环境：**Win10 x64 专业版

**编程软件：** visual studio 2015



## 下载OpenMesh库：

进入OpenMesh官网[**OpenMesh官网**](http://www.openmesh.org/)，下载下面的文件：

备用github直接下载：[**OpenMesh-8.0-VS2015-64-Bit-no-apps.exe**](https://github.com/touwoyimuli/2019_06_OpenMesh/blob/master/software/OpenMesh-8.0-VS2015-64-Bit-no-apps.exe)





## 配置OpenMesh运行环境：

### 安装OpenMesh程序：

安装OpenMesh-8.0-VS2015-64-Bit-no-apps.exe程序

标准安装：![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621135333.png)



### 创建工程：

创建一个新的（空的 项目工程）解决方案：比如项目名称叫`TestOpenMesh`![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/1561096684097.png)



### 配置OpenMesh的运行环境:

**在Debug模式 选择x64:**

- **配置<include>头文件：**

打开`项目属性-VC++目录-包含目录`，添加包含目录`C:\Program Files\OpenMesh 8.0\include`如下：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/1561097612511.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621165309.png)



- **配置库<lib>文件：**

打开`项目属性-链接器-常规-附加依赖库目录`，添加附加库目录`C:\Program Files\OpenMesh 8.0\lib`如下：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621141706.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621141801.png)



- **配置附加依赖项*.lib文件：**

打开`项目属性-链接器-输入-附加依赖项`，添加附加库目录`OpenMeshCored.lib
`和 `OpenMeshToolsd.lib`如下：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/1561098167619.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621165622.png)



- **配置预处理器添加宏**

(可能有的第三方库需要，有的第三方库不需要这一步操作)：

打开`项目属性-V/c++-预处理器`，添加附加宏`_USE_MATH_DEFINES`

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/1561098274278.png)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621142613.png)

一共经过这三步骤，就全部配置好了，接下来就是添加具体的代码了。



## 添加示例程序：

添加如下代码到创建的`TestOpenMesh.cpp`下面：

```c++
#include <iostream>
// -------------------- OpenMesh
#include <OpenMesh\/Core/IO/MeshIO.hh>
#include <OpenMesh/Core/Mesh/PolyMesh_ArrayKernelT.hh>
using namespace std;
 
typedef OpenMesh::PolyMesh_ArrayKernelT<>  MyMesh;
int main()
{
	MyMesh mesh;
	MyMesh::VertexHandle vhandle[8];
	vhandle[0] = mesh.add_vertex(MyMesh::Point(-1, -1, 1));
	vhandle[1] = mesh.add_vertex(MyMesh::Point(1, -1, 1));
	vhandle[2] = mesh.add_vertex(MyMesh::Point(1, 1, 1));
	vhandle[3] = mesh.add_vertex(MyMesh::Point(-1, 1, 1));
	vhandle[4] = mesh.add_vertex(MyMesh::Point(-1, -1, -1));
	vhandle[5] = mesh.add_vertex(MyMesh::Point(1, -1, -1));
	vhandle[6] = mesh.add_vertex(MyMesh::Point(1, 1, -1));
	vhandle[7] = mesh.add_vertex(MyMesh::Point(-1, 1, -1));
	// generate (quadrilateral) faces
	std::vector<MyMesh::VertexHandle>  face_vhandles;
	face_vhandles.clear();
	face_vhandles.push_back(vhandle[0]);
	face_vhandles.push_back(vhandle[1]);
	face_vhandles.push_back(vhandle[2]);
	face_vhandles.push_back(vhandle[3]);
	mesh.add_face(face_vhandles);
 
	face_vhandles.clear();
	face_vhandles.push_back(vhandle[7]);
	face_vhandles.push_back(vhandle[6]);
	face_vhandles.push_back(vhandle[5]);
	face_vhandles.push_back(vhandle[4]);
	mesh.add_face(face_vhandles);
 
	face_vhandles.clear();
	face_vhandles.push_back(vhandle[1]);
	face_vhandles.push_back(vhandle[0]);
	face_vhandles.push_back(vhandle[4]);
	face_vhandles.push_back(vhandle[5]);
	mesh.add_face(face_vhandles);
 
	face_vhandles.clear();
	face_vhandles.push_back(vhandle[2]);
	face_vhandles.push_back(vhandle[1]);
	face_vhandles.push_back(vhandle[5]);
	face_vhandles.push_back(vhandle[6]);
	mesh.add_face(face_vhandles);
 
	face_vhandles.clear();
	face_vhandles.push_back(vhandle[3]);
	face_vhandles.push_back(vhandle[2]);
	face_vhandles.push_back(vhandle[6]);
	face_vhandles.push_back(vhandle[7]);
	mesh.add_face(face_vhandles);
 
	face_vhandles.clear();
	face_vhandles.push_back(vhandle[0]);
	face_vhandles.push_back(vhandle[3]);
	face_vhandles.push_back(vhandle[7]);
	face_vhandles.push_back(vhandle[4]);
	mesh.add_face(face_vhandles);
 
	// write mesh to output.obj
	try
	{
		if (!OpenMesh::IO::write_mesh(mesh, "output.off"))
		{
			std::cerr << "Cannot write mesh to file 'output.off'" << std::endl;
			return 1;
		}
	}
	catch (std::exception& x)
	{
		std::cerr << x.what() << std::endl;
		return 1;
	}
	
	return 0;
}
```



## 运行该项目：

在vs 2015 里面按下`Ctrl + F5`，运行该项目

看到运行成功：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621143426.png)



## 验证成功：

打开`D:\programming\OpenMesh\TestOpenMesh\TestOpenMesh`目录下，查看是否生成`output.off`文件

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621143552.png)



使用MeshLab打开这个`output.off`文件，看看时候加载之后，显示是否成功

MeshLab官网下载地址：[MeshLab官网](http://www.meshlab.net/#download)

备用github直接下载：[**MeshLab2016.12.exe**](https://github.com/touwoyimuli/2019_06_OpenMesh/blob/master/software/MeshLab2016.12.exe)

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621144002.png)



选中该`output.off`文件，打开：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621144405.png)



发现运行成功：

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190621144507.png)



## 源码下载：

源码：[**TestOpenMesh源码**](https://github.com/touwoyimuli/2019_06_OpenMesh/tree/master/TestOpenMesh)



## 心得：

OpenMesh的网上教程感觉都是十分的少，唯一比较全面的话，可能就是相关的英文文档，然鹅，里面涉及的概念很多，且在现在工作了发现都是直接上手，根本不会让你去系统的学习OpenMesh的相关知识，时间仓促，且系统学习，所耗费的时间和精力，也容易使得人比较倦怠。故此好的解决方法就是，就是直接在项目中直接上手；

可以按照如下步骤实现：

1. ->从了解OpenMesh
2. ->你所需要的项目需求的实现
3. ->需要完成的什么任务
4. ->百度、谷歌、Bing轮着搜索怎么实现
5. ->查找函数或者迭代器的或者属性实例的使用
6. ->自己项目移植尝试
7. ->多次尝试
8. ->还是有问题，向~~大佬~~（划掉，巨佬）们求助
9. ->仍然无果;歇一天再按照上面的步骤尝试
10. ->仍然无果;建议放弃，学习其他。或者日后有机会在学



## 资源：

提供一个良好的中文入门手册：[**OpenMesh入门文档.pdf**](https://github.com/touwoyimuli/2019_06_OpenMesh/tree/master/docORpdf) （可以帮助弄清楚OpenMesh基本概念，值得一读）



同步的csdn本片博文：[OpenMesh入门，安装，运行示例Hello World](https://blog.csdn.net/qq_33154343/article/details/93206255)



**参考博文：**[OpenMesh学习笔记1 安装 配置 入门示例](https://blog.csdn.net/chaojiwudixiaofeixia/article/details/50460887)