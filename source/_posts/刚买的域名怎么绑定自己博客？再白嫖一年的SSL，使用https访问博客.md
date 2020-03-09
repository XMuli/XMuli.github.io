---
title: 刚买的域名怎么绑定自己博客？再白嫖一年的SSL，使用https访问博客
date: 2020-03-08 03:51:27
toc: true
categories: 
 - [学习 - hexo]
 - [学习 - 网络]
---



　　**简  述：**　本篇只是说说怎么将刚买的域名绑定到自己的博客； 又如何白嫖一年的 SSL 证书，给你博客网站访问去掉*不安全* 访问标签，使用 https 访问博客网址。

<!-- more -->

[TOC]

### 背景铺垫：

使用 Hexo + githubPage 搭建的博客，想要绑定域名 + 开启 https 访问。

将博客绑定域名，启用 SSL 证书，升级为 https 访问，其具体操作步骤如下：

1. 购买域名
2. 绑定博客地址和域名（设置域名解析）
3. 启用白嫖 SSL 证书，设置 https 方式。
4. 配置 github 博客仓库，勾选中 `Enforce HTTPS` 功能。

<br>

### 服务商购买域名：

选择一个服务商，这里以阿里云的万网为例：[wanwang.aliyun.com](https://wanwang.aliyun.com/domain/) ；在这里购买你所想要的域名（极少部分不能够备案，极其不推荐购买）。选好之后，按照流程，创建实名模板，绑定联系邮箱，付钱下单，然后到 `控制中心-域名服务中去`。 选择你购买的域名下的右侧 `解析` 设置。

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_20200308_012701.png" width="70%"/>

<br>

### 设置域名解析：

以我自己的为例：

顶级域名: xmuli.tech
博客地址: https://touwoyimuli.github.io



打开终端，ping 一下博客的地址，得到博客服务器的地址：`185.199.111.153`；

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_20200308_013049.png" width="60%"/>



然后将域名解析的设置为如下：

选择 A 类，将子域名解析到对应的 ip 地址的服务器。

<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_20200308_013736.png" width="100%"/>



这里的 TTL 的含义是，修改一行配置之后，这个解析要十分钟才能够生效。在 10 分钟后，再次在浏览器里面打开此网址。会显示 github 的 404 页面；此时说明解析已经生效，但是还有一步骤没有设置。



进入到本地电脑的 blog 的文件夹，在 source 文件夹下创建 CNAME 文件，在里面写下你的域名；前面不要加 http ，https ，www 或者最后面还有一个 / 这些符号。

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_20200308_014715.png" width="70%"/>



在执行如下熟悉的命令，将 blog 文件夹的内容生效且上传到站点：

```bash
hexo clean   //清除旧的缓存数据
hexo g -d    //配置和同步上传到 github 仓库
```



在此打开访问网址， 会发现成功的打开你的博客网址了（第一次解析会有点慢）。

<br>

### 启用 SSL，设置 https 访问：

此时会发现，该网址被标记为不安全，原因是访问的全称
是 http://xmuli.tech ，
非是 https://xmuli.tech 这个。

查询了一下，githubPage 是支持 https 方式，但是不能够直接上传 SSL 证书。被限制住了。

于是乎你的网址就被标记为不安全；作为文艺青年，这怎么能够容忍，wtf！！！

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_Snip20200307_204719.png" width="70%"/>



必须给它安排了，此时的将 http 升级为 https 的解决方式如下：

- 借用 CloudFlare CDN 提供商的服务
- 使用 SSL 证书（白嫖一年）
- 自己价格不菲购买证书，部署到机器，机器部署`nginx`，`stunnel`等代理



作为一个在家蹲了快两个月，躲避疫情的~~穷且屌丝的~~工程师，我倾向于第二条，这是无意之中开启的新世界大门；

钱不是问题，关键是对于咳咳，技术的兴趣的折腾。



打开阿里云控制台的 `基本信息`，选择  `开启 SSL 证书`，

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_Snip20200307_210418.png" width="60%"/>

 

然后点击购买 "GeoTrust" 的证书，默认是第一个。。。。。。为何藏起如此之深！！！

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_Snip20200307_210507.png" width="70%"/>



作为一口气买了 10 年的域名，因为~~钱不是问题，~~懒惰管理，选择购买年限最长的 SSL 证书，省去一年年的续费，准备 付款下单一气呵成，等等，，这个有点超出了，，，（数了数费用的小数点，这玩意怎么比域名还贵，还是续费方式的。不能够买断）。不好思思， 我再去看看其他方式加上小绿锁。

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_Snip20200307_210541.png" width="50%"/>



就在这个页面，发现有 `个人版的证书 ` ，且免费，果断下单，完成次笔交易。此刻双方都表示合作很愉快

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_Snip20200307_210717.png" width="50%"/>



接下来就是配置证书，点击证书的右侧的 `证书申请` ，再填写一下，等待大约 10 分钟，会看到上面那张图里面 ，配置域名解析的图，里面多出一行 txt 的配置，看到她出来，就属于配置成功。<img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_Snip20200307_211124.png" width="100%"/>

<br>

### 开启 github blog 仓库的HTTPS：

还差最后一脚，进入到 github 的这个对应的博客的仓库，点击 `setitng`，往下翻网页， 找到 `GitHub Pages ` 这个标签，勾选中 `Enforce HTTPS` 这一栏。然后再次在浏览器里面输入网址，会发现 显示为安全的访问。

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_20200308_023426.png" width="50%"/>

 

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_Snip20200307_202022.png" width="50%"/>



再次点击网页浏览，会发现为安全的访问，小锁的标记也有了，且无论是输入 http， 域名，旧的 github.io 方式，都会被强制跳转为 https 链接方式浏览。开心。

 <img src="https://raw.githubusercontent.com/touwoyimuli/FigureBed/img/2020/mark_20200308_024317.png" width="70%"/>

<br>

### 一年之后：

不过在 SSL 一年后到期，可以再次购买此证书一年；或者尝试使用 [CloudFlare](https://www.cloudflare.com/) 来解决（方式参考文末链接），亦或是那个时候，你已经有个很多个证书。毕竟事情总是在发展和变化的。另外，还白嫖了一篇配置文章，还图文并茂，点个赞留言再留如何？下次一定也可以啊！

<br>

**参考：**

[为GitHub Pages自定义域名并添加SSL-开启https强制](https://tzhou2018.github.io/2018/04/%E4%B8%BAGitHub-Pages%E8%87%AA%E5%AE%9A%E4%B9%89%E5%9F%9F%E5%90%8D%E5%B9%B6%E6%B7%BB%E5%8A%A0SSL-%E5%BC%80%E5%90%AFHTTPS%E5%BC%BA%E5%88%B6/) 

[为你的hexo博客配置个性域名](https://juejin.im/post/5a308ae551882540f363879a) 