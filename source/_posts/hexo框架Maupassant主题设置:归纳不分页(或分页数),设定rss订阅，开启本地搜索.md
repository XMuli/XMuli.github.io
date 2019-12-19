---
title: hexo框架Maupassant主题设置:归纳不分页(或分页数),设定rss订阅，开启本地搜索
date: 2019-11-28 00:19:27
toc: true
categories: 
 - [学习 - hexo]
---

**简  述：**  这是之前，将博客迁移过来的时候，一些个人个性化的操作📒设置，在此存一下，用来以后自己备查；在`hexo`中，使用的`Maupassant`主题，主要记录📝是以下的几个操作：

- 主题设置归纳不分页（或者指定分页数）
- 设置`rss`订阅
- 开启本地搜索， 关闭`google`和`baidu`搜索

<!-- more -->

> <font color=#D0087E  size=4 face="幼圆">**本篇的[csdn](https://blog.csdn.net/qq_33154343)/[github.io](https://touwoyimuli.github.io/)同步博文:** </font>  [hexo框架Maupassant主题设置:归纳不分页(或分页数),设定rss订阅，开启本地搜索](https://blog.csdn.net/qq_33154343/article/details/103285926)

<br>

[TOC]

## `hexo`中`Maupassant`主题设置归纳不分页

### 方法一（设置归纳不分页，或归纳分页数）：

**如下是我在（2019-11-03 时候搭建的`hexo`框架时候，有的效果）；**

查看了一下相关的配置文件`hexo/_config.yml`在2019-11-01下载更新的一份， 会比2019-06-08下载初始化的`hexo/_config.yml`要多几行文件，其中隔一段多几行;



- **实现不分页方法：**

将`hexo/_config.yml`的这一段代码， 默认的10 修改为0， 即便是不翻页

**[设置归档，若是为0， 则不用翻页]**

```bash
# Pagination 
## Set per_page to 0 to disable pagination
per_page: 0
pagination_dir: page
```

<br>

### 方法二（设置归纳不分页，或归纳分页数）：

参考： [Hexo 博客 Maupassant 主题下归档页显示的高级设置](https://www.nickyam.com/tech/hexo-maupassant-archives-show-all-in-one-page.html)  

**如下是我在（2019-06-08 时候搭建的`hexo`框架时候，有的效果）；**

先执行下面三个命令：

```bash
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-tag --save
```

再在`hexo`的`_config.yml`里面配置文件, 新增加如下片段：

```bash
# 设置首页分页之前默认就有，这里就不额外加了
# index_generator:
#   per_page: 7
archive_generator:
  per_page: 0  #值为0表示不分页，按需填写
  yearly: true  #是否按年生成归档
  monthly: false  #为了加快生成速度，按月归档就不要了
# 依葫芦画瓢，我们也可以对分类展示页、标签页等进行改造，以期与首页独立控制分页条数
tag_generator:
  per_page: 40  #值为0表示不分页，按需填写
category_generator:
  per_page: 40  #值为0表示不分页，按需填写
```

最后效果会遮蔽如下(自带的)这端：

```bash
# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page
```

<br>

## `hexo`中`Maupassant`主题设置rss订阅

**本身就是自带支持 RSS 订阅的:**

1. 安装`hexo-generator-feed`

    ```bash
    cnpm install hexo-generator-feed --save
    ```

2. 修改 `hexo` 配置

    ```yaml
    #RSS订阅
    plugin:
    - hexo-generator-feed
    #Feed Atom
    feed:
      type: atom
      path: atom.xml
      limit: 20
    ```

    其中，feed 配置是可选项

3. 修改主题配置

    ```yaml
    - page: rss
        directory: atom.xml
        icon: fa-rss
    ```

<br>

## 开启本地搜索， 关闭google和baidu搜索:

执行命令：

```bash
npm install hexo-generator-search --save
```

 然后在**themes/maupassant/_config.yml**文件，把下面几行的数值的参数修改为如下：                  

```bash
google_search: false ## Use Google search, true/false.
baidu_search: false ## Use Baidu search, true/false.

self_search: true ## Use a jQuery-based local search engine, true/false.
```

