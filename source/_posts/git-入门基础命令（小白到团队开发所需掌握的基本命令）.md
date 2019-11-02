---
title: '`git`入门基础命令（小白到团队开发所需掌握的基本命令）'
date: 2019-07-30 22:24:08
toc: true
categories: 
 - [学习 - git]
 - [学习 - 编码规范，辅助技巧]
tags: 
 - git
 - 版本控制
---

**简介：**  `git`学习：适合新手，开始使用`git`进行团队合作开发，需要掌握的基础`git`命令，不做知识点的工具书。

<!-- more -->

[TOC]

## 本博文的简述or解决问题？

​		本文可以做到掌握如下内容，掌握基础的`git`命令即可（达到能够在团队*[或商业化公司中]*，和队友一起协同开发即可）。更高深的技巧请移步`git`高深语法区。

​       `git`学习：适合新手，开始使用`git`进行团队合作开发，需要掌握的基础`git`命令，不做知识点的工具书。

<br>

## 开发平台环境：

**编程环境：**  `win10 x64 专业版 1803`  操作系统版本：`17134.285` 

**编程软件：**  `visual studio 2015`， `Qt Creator 4.8.2 (Enterprise)`， `Qt 5.9.8`

<br>

## <font color=#D0087E  face="幼圆">重要提示：</font>

<font color=#70AD47 size=4 face="幼圆">推荐点击文末的同步博客链接，查看本文，获得100%的浏览体验效果</font>

- 若遇csdn的博文的排版异常，图片无法加载，无法替换显示，则会删除异常部分（文末为无删减版）
- 无法预览的视频，则会替换为其链接；若学习资源分享失效，请评论区留言或留下邮箱
- <font color=#D0087E  size=4 face="幼圆">**请点击<font color=#FE7207  size=4 face="幼圆">本文末的同步链接</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好的100%效果体验**</font> 

<br>

## 背景：

学习`linux`之前，还是先学习一番`git`，之前一点`git`基础，之前只是使用 [github for desktop ](https://desktop.github.com/) 的可视化工具来进行，git的使用，期间同时使用一下 [git bash](https://www.git-scm.com/downloads) ，但是一直没有系统的学习的git，可能是柑橘初次接触`git`那会，感觉晦涩难懂吧，毕竟有着名言：

> 大佬们创照带界面UI，就是方便提升效率，那还为什么不使用呢？

为了学习(`zhuangbi`)一下`linux`，和应该多接触版本控控制工具，且网上推崇的`git`工具，老实说，我是真的心之向往，（`github` 限制文件`100M` 很难受）。

<br>

## 写此之前注意：

**不要为了写博文而只成为了网络知识点的搬运工，感动了自己，却忘记了根本是掌握**

<br>

## 学习命令之前的预备：

https://www.jianshu.com/p/cd1430161149?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation 

<br>

## git config：

- 配置`git`环境，告诉电脑，你是谁（哪一个git账号）？

```cpp
git config --global user.email "touwoyimuli@gmail.com"
git config --global user.name "touwoyimuli"
```

**一：区别**

参数--global含义：git config、git config --global、git config --system之前有何区别？

1.执行`git config `，会打开该项目所属的配置文件（作用域最小，值针对当前项目有效）。

2.执行`git config --global`，会打开`C:\Users\yuanyi\.gitconfig`下的配置文件（作用域中等，为登陆这台计算机的用户）。

3.执行`git config --system`，会打开`D:\Program Files\Git\etc\gitconfig`（作用域最大，整台计算机，不管登陆那个帐号，不管哪个项目）。

**二：优先级**

有没有想过，如果三种配置里面都设置了某个参数，那么最后生效的是哪种呢？它们之前的优先级为（由高到低）：`git config` > `git config --global` > `git config --system`。

也就是作用域范围越广的优先级越低，相信这个不难理解。

<br>

## git init：

- 初始化（该项目），将该项目（可看成文件夹）设置为带有`g`it管理的仓库

<br>

## git status：

- 判断该工程的文件的状态

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730215844.png"/>

需要了解的git的几个区域的工作：

【Untracked】：   没有加入Git管理的文件；
【Unmodified】：已经提交文件（即已经存在于Git仓库中文件）；
【Modified】：     已经存在于Git仓库的文件，后面做了修改但是没有提交；
【Staged】：         存在于Git暂存区，待提交的文件

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730215922.png"/>

<br>

## git add:

- 将文件夹项目，提交到暂存区（`staged`）

高级用法：

```cpp
git add -p //分块add
git add -u //只提交修改的文件，未追踪的不管
```

执行之后，会有如下：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730214219.png"/>

我的linux里面没有没有`s 参数` 自定义手动划分区域，不知道为什么？

```cpp
[y,n,q,a,d,/,K,j,J,g,e,?]
y - stage this hunk 
n - do not stage this hunk 
q - quit; do not stage this hunk nor any of the remaining ones 
a - stage this hunk and all later hunks in the file 
d - do not stage this hunk nor any of the later hunks in the file 
g - select a hunk to go to 
/ - search for a hunk matching the given regex 
j - leave this hunk undecided, see next undecided hunk 
J - leave this hunk undecided, see next hunk 
k - leave this hunk undecided, see previous undecided hunk 
K - leave this hunk undecided, see previous hunk 
s - split the current hunk into smaller hunks 
e - manually edit the current hunk 
? - print help
```

上面的翻译如下：

```cpp
y - 存储这个hunk 
n - 不存储这个hunk 
q - 离开，不存储这个hunk和其他hunk 
a - 存储这个hunk和这个文件后面的hunk 
d - 不存储这个hunk和这个文件后面的hunk 
g - 选择一个hunk 
/ - 通过正则查找hunk 
j - 不确定是否存储这个hunk，看下一个不确定的hunk 
J - 不确定是否存储这个hunk，看下一个hunk 
k - 不确定是否存储这个hunk，看上一个不确定的hunk 
K -不确定是否存储这个hunk，看上一个hunk 
s - 把当前的hunk分成更小的hunks 
e - 手动编辑当前的hunk 
? - 输出帮助信息
```

<br>

## git log:

- 查看时间线：所有的`commit`提交

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730220016.png"/>

<br>

## git commit:

- 提交项目

```cpp
git commit "项目简介一句话"  //不推荐
git commit           //会自动跳到一个单独的文件，写完整的提交信息   //实际工作中
```

<br>

## git rm:

```cpp
git rm --cached //重暂存区移除，文件还在
git rm -f       //直接删除文件，并且重git仓库移除
```

<br>

## git diff:

```cpp
git diff master    //查看当前分支做了哪些改动
git diff --cached  //查看已经staged（不记得回看上面的那个区图）文件改动
```

<br>

## git show:

- 显示某一个项目的具体信息

```
git show
```

<br>

## git branch：

- 一般用于分支的操作，比如创建分支，查看分支等等

```cpp
git branch       //列出本地已经存在的分支，并且在当前分支的前面用"*"标记
git branch -r    //查看远程版本库分支列表
git branch -a    //查看所有分支列表，包括本地和远程
git branch dev   //创建名为dev的分支，创建分支时需要是最新的环境，创建分支但依然停留在当前分支
```

<br>

## git checkout:

- 操作分支

```cpp
git checkout master     //将分支切换到master
git checkout -b master  //如果分支存在则只切换分支，若不存在则创建并切换到master分支，repo start是对git checkout -b这个命令的封装，将所有仓库的分支都切换到master，master是分支名，
```

<br>

## git rebase：

 [廖雪峰的*git*教程 - *Rebase* - 廖雪峰的官方网站 ](https://www.baidu.com/link?url=G9aDyNNdYNN0EkHcDhYXKNkTH18WrrNVF999_GBt8Ug8xhw6GbGk_AkeRCqd5nO2B_-L0fTaFBiyrE5kP6eRYS321z4pKOEjwR3doMzCwrq&wd=&eqid=a666ce3b0070882b000000045d403223) 

<br>

## git merge：

[*git*在工作中的正确使用方式---*git merge*篇 - nrsc - CSDN博客](https://www.baidu.com/link?url=uA3_4yNe4yHTvSkh7duH_lmK4L_XE_c_QZZ2W5QqiOX1_Q1GrEue6VPuKQbSw4JR9ZPUFR3kIGNbQNIY2Ro5-_-63YEpemsP3olaXZ2XuQO&ck=11700.5.0.0.0.246.176.0&shh=www.baidu.com&sht=baidu&wd=&eqid=de4c72f700760aac000000045d4032d8) 

<br>

## 冲突解决：

- 手动解决
- git mergtool

<br>

**//+++++++++++++++++++++++++++分割，建议歇一会，再开始看下面的++++++++++++++++++++++++**

<br>

## git stash：

- 备份当前仓库状态

<br>

## git reset:

```cpp
git reset          //仓库文件不变，git提交信息被更改
git reset --hard   //仓库文件强制回滚到commit
```

<br>

## git revert:

- git reset的作用是修改HEAD的位置，即将HEAD指向的位置改变为之前存在的某个版本，如下图所示，假设我们要回退到版本一：

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730222216.png"/>

 [Git恢复之前版本的两种方法reset、revert（图文详解）](https://blog.csdn.net/yxlshk/article/details/79944535) 

<br>

## git bisect:

- 用来查找哪一次代码提交引入了错误。
- 二分查找，排错

 [*git* *bisect* 命令教程 - 阮一峰的网络日志](https://www.baidu.com/link?url=lqeenJf_8WgBO2JKsMpH1ca7K5TZEGF5Jt7OaZTX2-O5DxnxdgJBMkeOXe_x3vFoLYjNMuqXdtQcwFE8RnM-B_&wd=&eqid=9ca323ee0032aca9000000045d4034d0) 

<br>

## git tag：

- tag是git版本库的一个标记，指向某个commit的指针。

 [*Git*中*tag*标签的使用 - 等待化茧成蝶的专栏 - CSDN博客](https://www.baidu.com/link?url=kri4FpJf7XZN2teqfO78VKbjiIGVh7dacRXa_Ax4CyN--npYWUBPL3yQmH3gjYTCTk5icT6ruBEdAXEegWe1n6KV-1rGhCJqx2F_L9ng-R7&wd=&eqid=d21b29ff0039f5be000000045d403549) 

<br>

## git clean：

- 删库到跑路，被老板抓住，打断腿，是一个工作几个月可赚60W的命令

```cpp
git clean -fd
```

<br>

## git cherry-pick：

- 看做“局部合并”
- 可以理解为”挑拣”提交，它会获取某一个分支的单笔提交，并作为一个新的提交引入到你当前分支上。 当我们需要在本地合入其他分支的提交时，如果我们不想对整个分支进行合并，而是只想将某一次提交合入到本地当前分支上

<br>

## git submodule：

- 当一个项目需要包含其他支持项目源码时使用的功能，作用是两个项目是独立的，且主项目可以使用另一个支持项目。

<br>

## git reflog：

- 可以查看所有分支的所有操作记录（包括已经被删除的 `commit` 记录和 `reset` 的操作）

    例如执行 `git reset --hard HEAD~1`，退回到上一个版本，用`git log`则是看不出来被删除的`commitid`，用`git reflog`则可以看到被删除的`commitid`，我们就可以买后悔药，恢复到被删除的那个版本。

    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730220217.png"/>

    <br>

## git ignore：

- 来忽略某些文件的提交

**规则  作用**

```cpp
 /mtk    过滤整个文件夹
 *.zip   过滤所有.zip文件
 /mtk/do.c   过滤某个具体文件
 !/mtk/one.txt   追踪（不过滤）某个具体文件
 注意：如果你创建.gitignore文件之前就push了某一文件，那么即使你在.gitignore文件中写入过滤该文件的规则，该规则也不会起作用，git仍然会对该文件进行版本管理。
```

**配置语法**

```cpp
 以斜杠“/”开头表示目录；
 以星号“*”通配多个字符；
 以问号“?”通配单个字符
 以方括号“[]”包含单个字符的匹配列表；
 以叹号“!”表示不忽略(跟踪)匹配到的文件或目录。
 注意： git 对于 .gitignore配置文件是按行从上到下进行规则匹配的
```

<br>

## 参考博文：

因为有着热心网友的无私分享，故不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 

 [git 官方book 英文](https://git-scm.com/doc) （推荐）

 [git book 中文](http://gitbook.liuhui998.com/index.html) （推荐）

 [Git详解及 github与gitlab使用](https://www.cnblogs.com/clsn/p/7929958.html) （推荐，系列）

 [直接git config和带--global、--system的区别](https://www.daixiaorui.com/read/240.html) 

 [Git合并单个文件和[y,n,q,a,d,/,K,j,J,g,e,?]](https://blog.csdn.net/diehuang3426/article/details/82588028) 

 [2.7 Git 基础 - Git 别名](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-Git-%E5%88%AB%E5%90%8D) 

 [Git branch && Git checkout常见用法](https://www.cnblogs.com/qianqiannian/p/6011404.html) 

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730220747.png"/>

<br>

## 本篇同步博文：

<font color=#FE7207  size=4 face="幼圆">**本博文同步到github.io博客：**</font> [`git`入门基础命令（小白到团队开发所需掌握的基本命令）](https://blog.csdn.net/qq_33154343/article/details/97825712) 