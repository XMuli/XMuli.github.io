---
title: 在MacOS10.14.6安装最新gdb8.3的详细教程，含可能遇到的所有坑(网上最新教程)
date: 2020-03-10 21:57:08
toc: true
categories: 
 - [学习 - MacOS]
---



　　**简  述：**　在 mac 使用 gdb 调试程序时候，会遇到报错如下，本文就是专门解决这个问题的。

```bash
(gdb) run
Starting program: /Users/muli/project/github/linuxExample/06_gdb/mainApp 
Note: this version of macOS has System Integrity Protection.
Because `startup-with-shell' is enabled, gdb has worked around this by
caching a copy of your shell.  The shell used by "run" is now:
    /Users/muli/Library/Caches/gdb/bin/zsh
Unable to find Mach task port for process-id 20050: (os/kern) failure (0x5).
 (please check gdb is codesigned - see taskgated(8))
```



在 MacOS 10.14.6 的系统下，发现安装 gdb 8.3 （当前最新） 的坑不是一般的多，那真真的多。鉴于网上的教程都是过时的，且按照某一篇， 你是不可能顺利运行安装成功的。所以浪费我昨天一个晚上解决这个问题（我的时间也很宝贵的），今天又花费一晚上时间，完成这篇图文并茂的的安装教程。作为一个日后的记录和给后来者一个方便。



也**是目前网上最新最详细的在 mac 安装 gdb 的教程** ；其中大概思路：

- 创建整证书，证书授权
- 关闭 SIP 安全防护，重启系统；
- sudo 运行 gdb 调试；
- kill 卡死进程， 再次重新 gdb 调试；
- 成功



<!-- more -->

[TOC]

### 笔记本系统环境：

　　**💻：**  `MacOS 10.14.6 ` 

<br>

### 查看是否安装 gdb:

- 执行 `brew search gdb` ，搜索 brew 仓库：

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185237.png" width="70%"/>

<br>

### 命令 brew 安装 gdb：

- 执行 `brew install gdb` 使用 brew 工具安装 gdb （默认最新），等待安装完毕；

- 执行 `which gdb` 查看安装的路径为 `/usr/local/bin/gdb` 

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185446.png" width="70%"/>

- 执行 `gdb -v` 查看的 gdb 安装版本，版本为 `8.3`

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185458.png" width="80%"/>

<br>

### 创建系统证书：

- 打开 **钥匙串访问** 

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185515.png" width="20%"/>

  

- 左上角进入 `钥匙串访问 - 证书助理 - 创建证书`

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185639.png" width="50%"/>

  

- 创建证书， 名称随意，如 gdb_codesigned ，其中选择为 **自签名根证书** ，**代码签名** ，还有✅ **让我覆盖这些默认值** 

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185816.png" width="50%"/>

  

- 后面一路**点击下一步，不用做任何修改** ，贴出来中间的过程图

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185829.png" width="50%"/>

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185835.png" width="50%"/>

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185840.png" width="50%"/>

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185844.png" width="50%"/>

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185849.png" width="50%"/>

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185854.png" width="50%"/>

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185858.png" width="50%"/>

   

- 一直到这步骤，进行修改，选择 "**系**统"， 

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185915.png" width="50%"/>

  

- 重新再来一次， 证书创建成功

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_191146.png" width="50%"/>

<br>

### 修改证书：

- **双击** 证书打开， 勾选使用 **始终信任** ，然后关闭此此窗口，会自动保存修改

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_191412.png" width="70%"/>

<br>

### 对证书授权：

- 执行 `codesign -fs gdb_codesigned /usr/local/bin/gdb` 命令，**给证书授权**；在执行 `echo "set startup-with-shell off" >> ~/.gdbinit` ， **关闭 MacOS 系统的 SIP 安全验证** ；设置完这咯爱那个步骤后， **要重启电脑** ，使得配置生效。

  ```bash
  codesign -fs gdb_codesigned /usr/local/bin/gdb
  echo "set startup-with-shell off" >> ~/.gdbinit
  ```

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_211131.png" width="80%"/>

<br>

### 验证 gdb 证书创建，关闭 SIP 成功：

- 使用一个小的 c++ 项目测试下，[下载地址](https://github.com/touwoyimuli/linuxExample/tree/master/05_makefile)，执行 `g++-9 *.cpp -o mainApp -g`  生成可调试的**可执行程序** mainApp  ，然后运行 `sudo gdb mainApp`

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_192435.png" width="70%"/>

<br>

--- -



### 期间可能会遇到的奇怪的问题？

#### 遇到证书创建失败？

- 若是失败，看到下图提示：

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_185938.png" width="50%"/>



- 解决方法：则重头创建一次系统证书，最后这一步选择 “**登录**” ，编绘创建成功。

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/image-20200310191044729.png" width="50%"/>

  

- 且证书创建成功后，需要手动将其从登录区域， 移动到系统区域

 <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_191315.png" width="70%"/>



<br>

#### 遇到 `Unable to find Mach task port for process-id 2358: (os/kern) failure (0x5).`：

- 出现如下如下代码：  `Unable to find Mach task port for process-id 2358: (os/kern) failure (0x5).` `(please check gdb is codesigned - see taskgated(8))` 

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_212227.png" width="80%"/>



- 解决方法：按下 `control + z` **退出终端** ，**使用 sudo 权限** ，运行命令 `sudo gdb mainApp` 调试

<br>

#### 遇到 `[New Thread 0x1303 of process 971]` 卡死:

- 出现如下代码 `[New Thread 0x1303 of process 971]` 代码，被进程被卡死（**通常第一次会遇到这个问题）** 。

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_192247.png" width="80%"/>

<br>

- 解决方法：按下 `control + z` **退出终端**，执行 `killall 2415` 杀死该进程；然后再次运行 `sudo gdb mainApp` 调试， 按下 `r` 调试。

   <img src="https://cdn.jsdelivr.net/gh/touwoyimuli/xmuliPic@pic/2020/20200310_192130.png" width="70%"/>

<br>

### 一些必须注意点:

- 执行命令时候，证书生效，有的带 -f 参数；
- 命令 `codesign -fs gdb_codesigned /usr/local/bin/gdb` 时候，`gdb_codesigned` 要换成你的证书名称
- 有的证书博客证书的起名称为 gdb_cert， 但是命令 和终端截图为 gdb-cert； 注意短杠
- 另外一个是 dgb 最好需要带绝对路径，避免找不到；
- **看完这篇教程，一定要点赞**

<br>

**参考博客：**

[在macOS10.14上使用GDB的教程](https://zhuanlan.zhihu.com/p/68398728)

[macbook创建自签名根证书失败,怎么办？](https://www.zhihu.com/question/264381471)

[mac book上安装和使用gdb](https://blog.csdn.net/zg_hover/article/details/82453862)

[Tips:如何优雅的使用GDB调试Go](https://studygolang.com/articles/26715?fr=sidebar)

[解决GDB在Mac下不能调试的问题](https://segmentfault.com/a/1190000004136351)

[MAC OSX系统使用gdb编译程序时的报错处理](https://blog.csdn.net/angus_monroe/article/details/78515468)

[macOS High Sierra下无法使用gdb的解决办法](https://juejin.im/post/5acdc6bef265da239a6029f4)

