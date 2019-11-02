---
title: OpenMesh模型分割：区域增长实现
date: 2019-6-24 16:38:10
updated: 2019-6-24 16:38:14
toc: true
categories: 
 - [学习 - c/c++]
 - [学习 - OpenMesh]
 
tags: 
 - c/c++
 - OpenMesh
 - 区域增长
 - 模型分割
---



​		将一个数据结构的模型OpenMesh进行分割，用区域增长的方式，来遍历所有，且此算法耗时比较短。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		将一个数据结构的模型OpenMesh进行分割，用区域增长的方式，来遍历所有，且此算法耗时比较短。



**编程环境：**Win10 x64 专业版

**编程软件：** visual studio 2015



## 思路：

1. 将所有的面进行标记为-1(表示没有属于那一块)和false(表示还没有遍历过)
2. 选择一个种子面fhSeed，然后向它的周边相邻的(三个）面进行区域增长，也给做上标
3. 对于这三个面，也把它作为一个新的面，然后向自己周边进行增长（然后辐射周边）
4. 然后用一个小的容器储存，正在增长的面，每次增长结束，就把自己这个已经遍历过的面，从容器中删除，当容器值为0的时候，便是一块完整的被遍历出来了
5. 重复以上2-3-4步骤
6. 将已经遍历了的面，打上标记，已经标记了的，不在标记；当所有的面全部都遍历结束，也就分割开了（这样的话，一个物体的面，属于哪一块，都被标记出来了）。



## 代码：

​	假设其中的一个模型变量为 ：`RefineMesh refineMeshNew`;

```c++
OpenMesh::FPropHandleT<int> FPropTriMark;    //属于那一块
	OpenMesh::FPropHandleT<bool> FPropTriFlag;   //是否遍历过

	refineMeshNew.add_property(FPropTriMark, "FPropTriMark");
	refineMeshNew.add_property(FPropTriFlag, "FPropTriFlag");

	for (auto f_it = refineMeshNew.faces_begin(); f_it != refineMeshNew.faces_end(); f_it++)
	{
		refineMeshNew.property(FPropTriMark, *f_it) = -1;
		refineMeshNew.property(FPropTriFlag, *f_it) = false;
	}

	int nMark = 0; //标记属于哪一块Mesh

	//外层循环------------------------------------------------------------------------------
	while (true)
	{
		//外层结束标志
		OpenMesh::FaceHandle fhSeed;
		for (auto f_itTemp = refineMeshNew.faces_begin(); f_itTemp != refineMeshNew.faces_end(); f_itTemp++)
		{
			if (!refineMeshNew.property(FPropTriFlag, *f_itTemp))
			{
				fhSeed = *f_itTemp;
				break;
			}
		}
		
		if (!fhSeed.is_valid())
			break;

		refineMeshNew.property(FPropTriFlag, fhSeed) = true;
		refineMeshNew.property(FPropTriMark, fhSeed) = nMark;
		
		vector<OpenMesh::FaceHandle> vecMarkFH;
		vecMarkFH.push_back(fhSeed);

		//内层循环------------------------------------------------------------------------------
		while (true)
		{
			if (vecMarkFH.size() == 0)
				break;

			OpenMesh::FaceHandle fhTemp = vecMarkFH[0];

			for (auto ff_it = refineMeshNew.ff_iter(fhTemp); ff_it != refineMeshNew.ff_end(fhTemp); ff_it++)
			{
				if (!refineMeshNew.property(FPropTriFlag, *ff_it))
				{
					refineMeshNew.property(FPropTriFlag, *ff_it) = true;
					refineMeshNew.property(FPropTriMark, *ff_it) = nMark;
				}
			}

			vecMarkFH.erase(vecMarkFH.begin());

		}
		
		nMark++;

	}
	//外层循环结束------------------------------------------------------------------------------
```



## 效率：

​		经过测试，用此算法，遍历一个有四千多万（40, 000, 000多）个面的数据模型，只需要耗费时间约`6s`，比我自己先前写的一个思路，耗时效率要高得多



//+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

## 更新：

<font color=#D0087E size=4 face="幼圆">**更新时间：** 2019-7-9 17:35:43</font>

<font color=#D0087E size=4 face="幼圆">**更新内容：** </font>增加有分割后的多个模型导出的代码分享

将下面的`mesh`替换成分割之后的pMesh,分割成为多个的时候，建议使用`Vector<pMesh> v`来存储，然后使用1行里面的代码替换掉`mesh`为`v[i]`即可；

**提示：** 因为上面代码已经将模型做了区分，然后使用for循环，直接按照标记所属于的，直接输出；亦可以即可将各自的点、面、半边、纹理等属性赋值给一个新的`pMesh`即可；

```c
if (!OpenMesh::IO::write_mesh(mesh, "output.off"))
    std::cerr << "Cannot write mesh to file 'output.off'" << std::endl;
```



------



**本博文同步到csdn博客：** [OpenMesh模型分割：区域增长实现](https://blog.csdn.net/qq_33154343/article/details/93505922#comments)