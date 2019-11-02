---
title: 三十分钟学会Git（写的挺好的一篇）
date: 2019-7-30 22:46:23
toc: true
categories: 
 - [学习 - git]
 - [学习 - 编码规范，辅助技巧]
tags: 
 - git
 - 版本控制
---



**简介：**  三十分钟学会Git（写的挺好的一篇）

<!-- more -->

[TOC]

<br>

**编程环境：**  `win10 x64 专业版 1803`  操作系统版本：`17134.285` 

<br>

## <font color=#D0087E  face="幼圆">重要提示：</font>

- 若遇`csdn`的博文的排版异常，图片无法加载，无法替换显示，则会删除该异常部分（删减版）
- 无法预览的视频，会替换为链接；若学习资源分享失效，请评论区留言或留下邮箱
- <font color=#D0087E  size=4 face="幼圆">**请点击<font color=#FE7207  size=4 face="幼圆">本文末的同步链接</font>，在 [github.io](https://touwoyimuli.github.io/) 博客上查看更好的100%效果体验**</font> 

<br>

## 转载原文：

> 第一步：首先我们了解下下面几个概念（Git本质上是保存文件的工具）：

1. Git保存的文件对应4个状态:

     【Untracked】：没有加入Git管理的文件；

     【Unmodified】：已经提交文件（即已经存在于Git仓库中文件）；

     【Modified】：已经存在于Git仓库的文件，后面做了修改但是没有提交；

     【Staged】：存在于Git暂存区，待提交的文件

​    

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730225608.png"/>


​    

-  **这4个状态分别被放在3个区域中**
     1.【工作目录（Working Directory）】：PC上的一个目录，存放从 Git 仓库的压缩数据库中提取出来的文件，存放的文件状态Unmodified、Modified、Untracked
     2.【暂存区域（Staging Area）】：保存了下次提交的文件列表信息，文件状态：Staged
     3.【Git仓库目录（Local Repository）】：Git 用来保存项目的元数据和对象数据库的地方(存在于工作目录下的隐藏文件夹[.git]，备份这个文件夹即可备份整个仓库)，文件状态：Unmodified

- 上面3个区域在Git中被抽象成3棵树对象

     Working Directory：记录整个工作目录的文件树结构

     Index：预期的下一次提交的快照

     HEAD：上一次提交的快照，下一次提交的父结点

​    <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730225706.png"/>


​    

1. SHA-1

- Git的每一次commit对会产生一个SHA-1值（一个40个字符的字符串），这个SHA-1可以理解为这次commit的别名。
     通常我们使用SHA-1时无需全部字符，只需要前几个字符（大于4个并且没有歧义）来表示某一个commit版本。
- 显示简短、无歧义的SHA-1(默认是7个字符)

```cpp
git log --abbrev-commit --pretty=oneline
```

- 查看某个commit，下面三个命令是等价的：

```cpp
$ git show 1c002dd4b536e7479fe34593e72e6c6c1819e53b
$ git show 1c002dd4b536e7479f
$ git show 1c002d
```

- 获取master分支现在指向的那个SHA-1

```cpp
git rev-parse master
904dc9362ece4c8659af08d3568d39f68d9687d6
```

- 祖先引用 
    - 引用的尾部加上一个 ^或者，表示为该引用的上一个提交HEAD 和 HEAD^ 是等价指向上一个提交；
    - 实例详解：[戳我](https://www.jianshu.com/p/abed5bb454dc) 

> 第二步：下面详解文件怎么在Git命令中运转



<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730225727.png"/>



------

- 【clone】：把远程仓库克隆到工作目录

```cpp
# 在目录下自动创建hello文件夹，并且克隆代码到hello文件夹
    $ git clone https://github.com/fanlehai/hello.git
    $ git clone git://github.com/fanlehai/hello.git
# 自定义文件夹克隆
    $ git clone https://github.com/fanlehai/hello.git hello-code
```

- 【init】：在本地目录创建仓库

```cpp
# 打开terminal，输入下面命令:
    $ mkdir hello
    $ cd hello
    $ git init
```

- 【add】：文件加入到暂存区(新增和修改文件用此命令) 
    - 【实例1】新增README.md，添加到暂存区，再提交到本地仓库

```cpp
    $ cd hello
    $ echo "# test" >> README.md
    $ git add README.md
    $ git commit -m "add README.md"
```

- 【实例2】：批量增加文件

```cpp
# 增加test文件夹下所有文件
$ git add test/*
# 增加test文件夹下所有后缀为.log的文件
$ git add test/*.log
```

- 【实例3】：对一个文件多处修改，进行分批提交：[戳我](https://www.jianshu.com/p/942f1fb3a182) 

- 【mv】：移动或重命名文件、目录、符号链接 
    - 【实例1】文件移入到新建文件夹并且提交

```cpp
*下面两个方法实现的效果完全相同
# 方法1：
      $ mkdir lib
      $ git mv hello.html lib
      $ git commit -m "Moved hello.html to lib"
# 方法2：
      $ mkdir lib
      $ mv hello.html lib
      $ git add lib/hello.html
      $ git rm hello.html
      $ git commit -m "Moved hello.html to lib"
```

- 【实例2】修改文件名

```cpp
# 修改文件名README.md--> README
      $ git mv README.md README
# 上面这句等下下面3句命令
      $ mv README.md README
      $ git rm README.md
      $ git add README
```

- 【rm】：删除文件 
    - 实例如下：

```cpp
1.删除文件
      $ git rm hello.html
      $ git commit -m "delete hello.html"
2.如果文件被修改过，而且被(git add)到暂存区,用-f强制删除
      $ git rm -f hello.html
3.把文件从暂存区中删除，但是保留在磁盘（变成Untracked状态）
     【使用场景】：当你忘记添加 .gitignore文件，不小心把一个很大的日志文件或一堆 .a
        这样的编译生成文件添加到暂存区时
      # a. 删除暂存区中的README文件
        $ git rm --cached README
      # b. 不小心test文件夹下所有文件加入暂存区，吧文件从暂存区移除，但不从硬盘上删除
        $ git add test/*
        $ git rm --cached test/*
4.批量删除
      #删除log目录下所有.log后缀的文件
      $ git rm log/*.log
      #删除所有文件名以~结尾的文件
      $ git rm *~
```

- reset：回滚版本、撤销暂存区文件 
    - 撤销暂存区文件

```cpp
# 撤销暂存区文件file.txt(其实就是把HEAD版本的file.txt文件恢复到暂存区)
      $ git reset HEAD file.txt
# 撤销暂存区文件(filename可能会跟分支名相同，所以用--来表示后面是路径或者文件)
      $ git reset HEAD -- filename
# 把指定版本(eb43bf)的文件恢复到暂存区
      $ git reset eb43bf file.txt
# 根据上个命令显示的commit对应的值回滚
      $ git reset --hard <commit>
# --hard表示删除工作区的所有在commit版本后的改动
```

- 对于版本提交撤销

```cpp
# 恢复工作区(同步缺少的文件，对于已经修改的文件只尝试把仓库中版本合并到本地，保留本地修改)
      $ git reset  HEAD
# 恢复工作区（把本地修改的文件全部从硬盘删除，同步成仓库中版本）
      $ git reset --hard HEAD
```

- 更多reset实例：[戳我](https://www.jianshu.com/p/77bc8781289e) 
- 【git reset --hard HEAD~】命令包含三个步骤：

```cpp
1.移动 HEAD 分支的指向上一个版本(HEAD~)
2.使索引看起来像现在的HEAD
3.使工作目录看起来像索引
##补充说明
      $ git reset --soft HEAD~ 这个命令执行上面1【图reset 2】
      $ git reset --mixed HEAD~这个命令执行上面1，2【图reset 3】
      $ git reset --hard HEAD~这个命令执行上面1，2，3【图reset 4】
【--mixed】是reset默认行为
      $ git reset HEAD~ == $ git reset --mixed HEAD~ 
```



<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730225819.png"/>

reset 1



<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730225843.png"/>

reset 2



<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730225901.png"/>

reset 3



<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730225940.png"/>

reset 4

- reset与revert区别：

```cpp
1.reset撤销指定版本后所有提交；
2.revert撤销指定版本的提交；
```

- reset与checkout区别：如下表格 

    - “REF” 表示该命令移动了 HEAD 指向的分支引用，而“HEAD” 则表示只移动了 HEAD 自身;

    - “WD Safe?” 表示命令是否影响工作区未提交文件（NO表示有影响）

    - git checkout HEAD 和 git reset --hard HEAD 

        - checkout不改变工作区已经修改的文件;reset --hard则不做检查就全面地替换工作目录；
        - checkout只移动 HEAD自身来指向另一个分支;reset会移动 HEAD和自身分支的指向；

    - git checkout -- file.txt等价于git reset --hard file.txt放弃对file.txt的所有修改，恢复成仓库中版本

        

        <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730230003.png"/>

        

> ||HEAD|Index|Workdir|WD Safe?|
>  |:--|:--|:--|:--|:--|
>  |**Commit Level**|||||
>  |reset --soft [commit]|REF|NO|NO|YES|
>  |reset [commit]|REF|YES|NO|YES|
>  |reset --hard [commit]|REF|YES|YES|**NO**|
>  |checkout [commit]|HEAD|YES|YES|YES|
>  |**File Level**|||||
>  |reset (commit) [file]|NO|YES|NO|YES|
>  |checkout (commit) [file]|NO|YES|YES|**NO**|

- 【revert】：撤销指定的提交版本
     git revert命令本质上就是一个逆向的 git cherry-pick操作。 它将你提交中的变更的以完全相反的方式的应用到一个新创建的提交中，本质上就是撤销或者倒转。

```cpp
# 撤销当前版本提交内容，这里会跳出编辑器让你修改commit内容
    $ git revert HEAD
# 撤销当前版本提交内容，直接撤销不会跳出编辑器
    $ git revert HEAD --no-edit
# 撤销指定版本,commit是提交版本的SHA-1，注意撤销老版本可能会引起文件冲突
    $ git revert <commit> --no-edit
```

- 【branch】：分支
    - 分支详解：[戳我](https://www.jianshu.com/p/893f159e28b0) 
- 【checkout】：切换分支或者恢复文件

```cpp
1.切换分支
    $ git checkout <branch name>
2.修复工作目录下的文件，使得跟仓库文件同步
    $ git checkout  <filename>
3.如果1中<branch name>和2中<filename>相同，调用上面命令会让git迷惑，
    所以对于文件同步用下面命令(撤销文件修改)：
    $ git checkout -- <filename>
4.新建并切换分支
    $ git checkout -b <branchname>
5.切换到旧的提交版本，commit是表示提交的id
    $ git checkout <commit>
6.删除分支：
    #先切换到其他分支，无法删除正在使用的分支
      $ git checkout master
    #开始删除本地分支
      $ git branch -d mybranch1
    #开始推送删除共享git服务器上分支
      $ git push origin :mybranch1
7.删除远程仓库的分支
      $ git push origin --delete serverfix
8.强行删除分支（当分支新增功能而且commit但是没有合并，然后现在不需要这个分支，就需要使用强制删除）
      $ git branch -D mybranch1
```

- 【commit】：提交文件到Local Repository

```cpp
1.提交说明放在命令行执行
    $ git add file.java
    $ git commit -m "add file.java"
2.启动文本编辑器以便输入本次提交的说明(默认会启用 shell 的环境变量 $EDITOR所指定的软件
    $ git commit
3.所有已经跟踪过的文件暂存起来一并提交,无需($git add file)命令 
    $ git commit -a -m 'added new benchmarks'
4.修改最后一次提交
    # 只修改最后一次提交信息，下面命令会跳出编辑器，修改内容关闭即可
      $ git commit --amend
    # 有些文件忘了提交、有些文件忘了删除，需要修改最后一次提交,这种情况会改变SHA-1的值
      $ git add forgotten_file
      $ git rm file
      $ git commit --amend
```

- 【diff】：查看任意两棵树的差异

```cpp
# 查看工作环境与暂存区的差异(即是未提交的文件)
    $ git diff
# 暂存区域与你最后提交之间的差异
    $ git diff --staged
# 比较两个提交记录的差异
    $ git diff master branchB
#检查空白错误（空格、tab等）
    $ git diff --check
# 查看合并后本分支变动了内容
    $ git diff --ours
# 查看被合并分支与合并后有哪些不同(-b去除空白)
    $ git diff --theirs -b
# 查看合并后，合并两个版本各自变动内容
    $ git diff --base -b
```

- 【merge】：合并分支 
    - 合并某分支(testing)到当前分支(master)： 
        1. 使用Fast-forward模式（Git默认模式）：

```cpp
$ git merge testing
$ git branch -d testing
2. 禁用Fast-forward模式（推荐使用此模式，记录所有分支合并情况）：
$ git merge --no-ff -m "merge with no-ff" testing
3. 两种模式的区别（左边使用Fast-forward，无法知道testing存在过）：
```



<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190730230022.png"/>



- 合并相关命令：

```cpp
# 合并时，如果有冲突，用自己分支代码取代冲突代码
       $ git merge -Xours branchname
# 合并有冲突，用待合并分支代码取代冲突代码
       $ git merge -Xtheirs branchname
#建议使用此方式，留下合并记录
       $ git merge --no-ff -m "merge with no-ff" <mybranch1>
       $ git merge <mybranch1>
#列出本地仓库，分支前面有* 表示是当前使用的分支
       $ git branch
# 列出分支，并且显示每个分支最后一次提交信息
       $ git branch -v
# 查看哪些分支已经合并到当前分支
      $ git branch --merged
# 查看所有包含未合并工作的分支
      $ git branch --no-merged
# 合并出现冲突，不想合并了，可以用下面命令撤销：
      $ git merge --abort
      或者
      $ git reset --hard HEAD
```

> 合并分支实例：[戳我](https://www.jianshu.com/p/5d34260824ee)

- 【rebase】：变基，合并分支的一种方式 
    - 实例：[戳我](https://www.jianshu.com/p/ce9fefaab751) 
- 【tag】：历史中的某一个提交打上标签，以示重要

```cpp
【轻量标签】很像一个不会改变的分支 - 它只是一个特定提交的引用。
【附注标签】是存储在 Git 数据库中的一个完整对象。
1.打轻量标签，只需要提供标签名
    $ git tag -m "Tag on new commit" mytag2
2.打附注标签，用-a
    $ git tag -a v1.4 -m 'my version 1.4'
```

> tag详解：[戳我](https://www.jianshu.com/p/95d142b7116d)

- 【rerere】：自动解决冲突

```cpp
# 定义：
一个隐藏的功能。 正如它的名字 “reuse recorded resolution” 所指，它
允许你让 Git 记住解决一个块冲突的方法，这样在下一次看到相同冲突
时，Git 可以为你自动地解决它。
# 使用场景：
1.如果你想要保证一个长期分支会干净地合并，但是又不想要一串中间的合并提交将
    rerere 功能打开后偶尔合并，解决冲突，然后返回到合并前。 如果你持续这样做，
    那么最终的合并会很容易，因为 rerere 可以为你自动做所有的事情。
2.可以将同样的策略用在维持一个变基的分支时，这样就不用每次解决同样的变基冲突了。 
    或者你将一个分支合并并修复了一堆冲突后想要用变基来替代合并 - 你可能并不想要再次解决相同的冲突。
3.当你偶尔将一堆正在改进的特性分支合并到一个可测试的分支时，就像 Git 项目自身经常做的。 
    如果测试失败，你可以倒回合并之前然后在去除导致测试失败的那个特性分支后重做合并，而不用再次重新解决所有的冲突。
# 启用 rerere功能方法：
1.全局开启(对所有仓库)：$ git config --global rerere.enabled true
2.单个仓库：仓库中创建 .git/rr-cache目录即可开启
```

- 变基实例：[戳我](https://www.jianshu.com/p/b0607f9ac7ed) 
- 交互式变基：[戳我](https://www.jianshu.com/p/71dcbb6ee847) 
- 【fetch】：从远程仓库抓取最新数据 
    - 当 git fetch命令从服务器上抓取本地没有的数据时，它并不会修改工作目录中的内容。 它只会获取数据然后让你自己合并。

```cpp
  #拉取远程仓库数据
      $ git fetch origin
  #如果origin有其他分支serverfix，那么分支的引用会被拉下来跟本地关联起来，
#但是serverfix分支的代码不会被同步到本地，需要同步使用下面命令：
      $ git checkout -b serverfix origin/serverfix
```

- 【pull】：拉取远程仓库最新代码，并尝试与本地进行合并

```cpp
#git pull 在大多数情况下它的含义是一个 git fetch紧接着一个 git merge 命令
    $ git pull
# 可以修改pull.rebase的默认配置
    $ git config --global pull.rebase true
```

- push：推送本地仓库文件到远程仓库

```cpp
#把master分支修改推送给远程仓库
    $ git push origin master
```

- 【remote】：管理远程仓库的属性

```cpp
# 查看远程仓库名称简写
    $ git remote
# 查看更多远程仓库origin的信息
    $ git remote show origin
# 查看远程仓库名称简写和URL
    $ git remote -v
    origin  https://github.com/schacon/ticgit (fetch)
    origin  https://github.com/schacon/ticgit (push)
# 添加一个新的远程 Git 仓库，同时指定一个你可以轻松引用的简写
    $ git remote add pb https://github.com/paulboone/ticgit
# 修改一个远程仓库的简写名：pb-->paul
    $ git remote rename pb paul
# 移除远程仓库paul
    $ git remote rm paul
```

- 【bundle】：对本地仓库进行打包 
    - 使用此命令的实例：

```cpp
    【1】在A机器上打包：
        $ cd R1
        $ git bundle create file.bundle master
        $ git tag -f lastR2bundle master   
  【2】把上面包文件file.bundle通过u盘拷贝到B机器上，解包
        $ git clone -b master /home/me/tmp/file.bundle R2
  【3】修改B机器配置
        cd R2
        nano .git/config
        [remote "origin"]
          url = /home/me/tmp/file.bundle
          fetch = refs/heads/*:refs/remotes/origin/*
  【4】此时A机器又修复了几个bug，再次打包
        $ cd R1
        $ git bundle create file.bundle lastR2bundle..master
        $ git tag -f lastR2bundle master
  【5】把A机器新的打包文件拷贝到B机器，覆盖上次file.bundle,然后运行命令更新
        $ cd R2
        $ git pull
```

- 打包整个分支

```cpp
  # 打包master分支
      $ git bundle create repo.bundle HEAD master
  # 解包，建仓库
      $ git clone repo.bundle repo
        Initialized empty Git repository in /private/tmp/bundle/repo/.git/
      $ cd repo
      $ git log --oneline
        9a466c5 second commit
        b1ec324 first commit
```

- 打包部分提交：

```cpp
  # 查看maste分支没有提交的版本
      $ git log --oneline master ^origin/master
  # 打包master分支9a466c5之后版本（不包括9a466c5）
      $ git bundle create commits.bundle master ^9a466c5
  # 确定打包文件是否合法正确（如果打包工具打包是缺少了一个提交）
      $ git bundle verify ../commits.bundle
  # 查看包的分支情况
      $ git bundle list-heads ../commits.bundle
        71b84daaf49abed142a373b6e5c59a22dc6560dc refs/heads/master
  # 导入包文件到other-master分支
      $ git fetch ../commits.bundle master:other-master
```

- 【log】 
    - 详情：[戳我](https://www.jianshu.com/p/bad1f70b6703) 
- 【status】

```cpp
# 查看当前仓库状态（比较详细）
    $ git status
# 查看当前仓库状态（简略）
    $ git status -s
    $ git status --short
      M README
      MM Rakefile
      A  lib/git.rb
      M  lib/simplegit.rb
      ?? LICENSE.txt
    ## ?? 表示未跟踪文件
    ## A  表示新添加到暂存区中的文件
    ## MM 第一个M表示修改过的文件而且放入到暂存区，第二个M表示
    ## 修改过的文件,但是没有添加到暂存区
```

- 【show】：查看Git中不同对象信息 (blobs, trees, tags and commits) 
    - 查看master最后一次commit(下面两个命令等价)

```cpp
git show master
git show 904dc9362ece4c8659af08d3568d39f68d9687d6
```

- 冲突产生时，用此命令生成冲突版本合并前的文件

```cpp
【关于冲突，显示冲突文件】
    # 如果产生冲突了，可以用下面方法恢复冲突版本对应文件
    # Stage 1 是它们共同的祖先版本
    # stage 2 是你的版本
    # stage 3 来自于 MERGE_HEAD，即你将要合并入的版本（“theirs”）
      $ git show :1:hello.rb > hello.common.rb
      $ git show :2:hello.rb > hello.ours.rb
      $ git show :3:hello.rb > hello.theirs.rb
    # :1:hello.rb只是查找那个 blob 对象 SHA-1 值的简写，
      通过下面命令可以查看到SHA-1，用SHA-1代替1
    $ git ls-files -u
      100755 ac51efdc3df4f4fd328d1a02ad05331d8e2c9111 1 hello.rb
      100755 36c06c8752c78d2aff89571132f3bf7841a7b5c3 2 hello.rb
      100755 e85207e04dfdd5eb0a1e9febbc67fd837c44a1cd 3 hello.rb
```

- 【blame】 : 查找文件每一行的提交信息(作者、时间等) 
    - 【-L】查看simplegit.rb的12-15每一行最后一次提交信息（下面^表示的行：是这些行第一次提交并且没有修改过）

```cpp
$ git blame -L 12,15 simplegit.rb
      ^4832fe2 (Scott Chacon  2008-03-15 10:31:28 -0700 12)  def show(tree = 'master')
      ^4832fe2 (Scott Chacon  2008-03-15 10:31:28 -0700 15)
      9f6560e4 (Scott Chacon  2008-03-17 21:52:20 -0700 16)  def log(tree = 'master')
      79eaf55d (Scott Chacon  2008-04-06 10:15:08 -0700 17)     command("git log #{tree}")
```

- 【-C】查看代码行最原始文件（当文件被拆分，使用此方法查看文件被拆分前的文件信息），

```cpp
#下面查看GITPackUpload.m的141-144行代码行的原始信息（-C参数表示查看原始出处）
$ git blame -C -L 141,144 GITPackUpload.m
      f344f58d GITServerHandler.m (Scott 2009-01-04 141)
      f344f58d GITServerHandler.m (Scott 2009-01-04 142) - (void) gatherObjectShasFromC
      f344f58d GITServerHandler.m (Scott 2009-01-04 143) {
      70befddd GITServerHandler.m (Scott 2009-03-22 144)         //NSLog(@"GATHER COMMI
```

- 【bisect】：对你的提交历史进行二分查找，找到有问题的版本 
    - 情景1：手动测试（查找发现当前版本有bug，想找出bug是在哪一个版本引入的）

```cpp
  # 启动
      $ git bisect start
  # 说明当前版本有问题
      $ git bisect bad
  # 设定v1.0版本是没有问题的
      $ git bisect good v1.0
  # 现在可以测试，
      1.没有问题输入:$ git bisect good
      2.有问题输入：$ git bisect bad
  # 继续上面测试这个过程，直到bisect找到第一个有问题版本
  # 重置bisect
      $ git bisect reset
```

- 情景2：自动测试，有个test-error.sh脚本，项目运行正确返回非0，项目运行错误返回0

```cpp
  # 设置bisect有问题版本HEAD和没有问题版本v1.0
      $ git bisect start HEAD v1.0
  # 开始自动运行测试
      $ git bisect run test-error.sh
```

- 【grep】：查找一个函数是在哪里调用或者定义，或者变更历史。

     优点：第一就是速度非常快，第二是你不仅仅可以可以搜索工作目录，还可以搜索任意的 Git 树。 

    - 【-n】参数来输出 Git 所找到的匹配行行号

```cpp
  $ git grep -n gmtime_r
      compat/gmtime.c:3:#undef gmtime_r
      compat/gmtime.c:8:      return git_gmtime_r(timep, &result);
      compat/gmtime.c:11:struct tm *git_gmtime_r(const time_t   *timep, struct tm *result)
      compat/gmtime.c:16:     ret = gmtime_r(timep, result);
      compat/mingw.c:606:struct tm *gmtime_r(const time_t *timep,   struct tm *result)
      compat/mingw.h:162:struct tm *gmtime_r(const time_t *timep, struct tm *result);
      date.c:429:             if (gmtime_r(&now, &now_tm))
      date.c:492:             if (gmtime_r(&time, tm)) {
      git-compat-util.h:721:struct tm *git_gmtime_r(const time_t *, struct tm *);
      git-compat-util.h:723:#define gmtime_r git_gmtime_r
```

- 【--count】输出搜索匹配的文件和匹配的个数

```cpp
  $ git grep --count gmtime_r
      compat/gmtime.c:4
      compat/mingw.c:1
      compat/mingw.h:1
      date.c:2
      git-compat-util.h:2
```

- 【-p】输出匹配的行是属于哪一个方法或者函数

```cpp
  $ git grep -p gmtime_r *.c
      date.c=static int match_multi_number(unsigned long num, char c, const char *date, char *end, struct tm *tm)
      date.c:         if (gmtime_r(&now, &now_tm))
      date.c=static int match_digit(const char *date, struct tm *tm, int   *offset, int *tm_gmt)
      date.c:         if (gmtime_r(&time, tm)) {
```

- 【--and】查看在旧版本 1.8.0 的 Git 代码库中定义了常量名包含 “LINK” 或者 “BUF_MAX” 这两个字符串所在的行

```cpp
  $ git grep --break --heading \
      -n -e '#define' --and \( -e LINK -e BUF_MAX \) v1.8.0
      v1.8.0:builtin/index-pack.c
        62:#define FLAG_LINK (1u<<20)
      v1.8.0:cache.h
        73:#define S_IFGITLINK  0160000
        74:#define S_ISGITLINK(m)       (((m) & S_IFMT) == S_IFGITLINK)
```

- 找到 ZLIB_BUF_MAX常量是什么时候引入的，我们可以使用 -S选项来显示新增和删除该字符串的提交。

```cpp
  $ git log -SZLIB_BUF_MAX --oneline
      e01503b zlib: allow feeding more than 4GB in one go
      ef49a7a zlib: zlib can only process 4GB at a time
```

- 查看 zlib.c文件中git_deflate_bound函数的每一次变更

```cpp
  $ git log -L :git_deflate_bound:zlib.c
```

- 【stash】：用于存储当前工作区域的环境，存储后用git clean工作区 
    - 【1】使用stash流程，git stash默认存储所有被跟踪的文件(除了Untracked文件)

```cpp
    #存储工作区（只存储暂存区的文件），并且清空工作区
      $ git stash
      $ git stash save
    #查看工作区，提示已经清理干净
      $ git status
      # On branch master
      nothing to commit, working directory clean
    #查看存储了些什么
      $ git stash list
      stash@{0}: WIP on master: 049d078 added the index file
      stash@{1}: WIP on master: c264051 Revert "added file_size"
      stash@{2}: WIP on master: 21d80a5 added number to log
    #恢复最近存储内容，不会恢复暂存区文件
      $ git stash apply
    #恢复最近一次存储内容，不会恢复暂存区文件，删除最近一次存储
      $ git stash pop
    #指定恢复内容：
      $ git stash apply stash@{2}
    #【--index】不经恢复工作区文件，还会恢复暂存区文件
      $ git stash apply --index
    #删除存储的内容,删除最近一次的存储
      $ git stash drop stash@{0}
    #清除所有存储
      $ git stash clear
```

- 【2】【--keep-index】: 只存储修改过并且没有提交到暂存区的文件（不存储未跟踪和暂存区文件）

```cpp
# (M  index.html)中的M表示在暂存区；（M lib/simplegit.rb）中的M表示文件被修改但是没有被提交到暂存区；
    $ git status -s
      M  index.html
       M lib/simplegit.rb
    $ git stash --keep-index
      Saved working directory and index state WIP on master: 1b65b17 added the index file
      HEAD is now at 1b65b17 added the index file
    $ git status -s
      M  index.html
```

- 【3】【--include-untracked 或 -u】存储所有已经跟踪的文件(Unmodified、Modified、Staged)和未跟踪的文件（Untracked），除了忽略的文件

```cpp
      $ git stash -u
```

- 【4】【--all】存储所有文件（Unmodified、Modified、Staged、Untracked、忽略文件）
- 【5】【--patch】不储藏所有修改过的文件，但是会交互式地提示哪些改动想要储藏、哪些改动需要保存在工作目录中

```cpp
      $ git stash --patch
        diff --git a/lib/simplegit.rb b/lib/simplegit.rb
        index 66d332e..8bb5674 100644
        --- a/lib/simplegit.rb
        +++ b/lib/simplegit.rb
        @@ -16,6 +16,10 @@ class SimpleGit
                 return `#{git_cmd} 2>&1`.chomp
               end
             end
        +
        +    def show(treeish = 'master')
        +      command("git show #{treeish}")
        +    end
         end
         test
        Stash this hunk [y,n,q,a,d,/,e,?]? y     ##这里确定是否删除##
        Saved working directory and index state WIP on master: 1b65b17 added the index file
```

- 【6】新建分支恢复储藏

```cpp
      【使用场景：】储藏了一些工作，然后继续在储藏的分支上工作，
                  如果此时恢复储藏可能会文件冲突，可以新建分支恢复储藏
          $ git stash branch testchanges
```

- 【clean】：清除工作目录中未被追踪的文件；git clean默认不会清除.gitiignore中忽略的文件

```cpp
# 【-d】表示删除为被追踪的文件夹以及文件夹内文件
# 【-n】表示先看下那些文件要被删除
    $ git clean -n -d
    Would remove build.TMP
    Would remove tmp/
# 【-x】是表示忽略.gitiignore的规则
    $ git clean -n -d -x
    Would remove build.TMP
    Would remove test.o
    Would remove tmp/
#【-i 或 “interactive”】 表示交互式清除，每个都会询问是否删除
    $ git clean -x -i
    Would remove the following items:
      build.TMP  test.o
    *** Commands ***
        1: clean                2: filter by pattern    3: select by numbers      4: ask each             5: quit
        6: help
    What now>
#【-f】 意味着 *强制* 或 “确定移除”，移除工作目录中所有未追踪的文件以及空的子目录
    $ git clean -f -d -x
```

- 【reflog】：记录了最近几个月 HEAD 和分支引用所指向的历史；引用日志只存在于本地仓库；克隆仓库时引用日志为空； 
    - 查看引用日志：

```cpp
$ git reflog
34713b HEAD@{0}: commit: fixed refs handling, added gc auto, updated
d921970 HEAD@{1}: merge phedders/rdocs: Merge made by recursive.
1c002dd HEAD@{2}: commit: added some blame and merge stuff
1c36188 HEAD@{3}: rebase -i (squash): updating HEAD
95df984 HEAD@{4}: commit: # This is a combination of two commits.
```

- 查看最近五次HEAD指向的提交

```cpp
$ git show HEAD@{5}
```

- 查看master分支昨天HEAD指向的提交

```cpp
$ git show master@{yesterday}
```

- 【submodule】：一个仓库包含另一个仓库 
    - 详见请见：[戳我](https://www.jianshu.com/p/491609b1c426)

作者：fanlehai

链接：

## 参考博文：

因为有着热心网友的无私分享，故不辞辛苦也将自己的项目或经验整理成博客的形式，也提供给一起大家学习探讨与交流 

[三十分钟学会Git](https://www.jianshu.com/p/cd1430161149?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation) 

![](https://raw.githubusercontent.com/touwoyimuli/FigureBed/master/img/20190719175818.png)

<br>

## 本篇同步博文：



<font color=#FE7207  size=4 face="幼圆">**本博文同步到github.io博客：**</font> [三十分钟学会Git（写的挺好的一篇）](https://blog.csdn.net/qq_33154343/article/details/97828995)