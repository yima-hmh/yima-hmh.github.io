---
title: selenium爬取动态网页
description: 爬到用时方恨少
tags:
  - python
  - 爬虫
categories:
  - - python
    - 爬虫
abbrlink: e506
date: 2023-06-30 15:12:09
---
### 概述

1. 什么是动态网页?
使用异步加载。现在动态网页的数量大于静态网页的数量
2. 静态网页的爬虫
scrapy，beautiful-soup，bs4
3. 为什么用selenium？
    a. selenium是网页浏览器的模拟器，通常用来做网页测试-八爪鱼
    b. 优点：
    ⅰ. 简单直接：直接模拟用户的行为，用户加载网页获取网页源码
    ⅱ. 直接和scrapy结合（middlewire中间件）
    ⅲ. 支持多种浏览器驱动器web-driver：phantomjs（无头，指不用打开，直接访问代码），chorme（le，firefox）
    c. 用来做爬虫的缺点：
    ⅰ. 容易被识别
    ⅱ. 容易崩溃，网速，网站的稳定性都会崩掉
    ⅲ. 不易做成可执行程序（打包成exe可执行文件，可以直接打开，不用对方安装python或者安装python的各种依赖库。使用pyinstaller）
    如何使用
1. 安装浏览器和驱动器以及对应版本的驱动器
2. pip安装selenium
3. 打开/关闭浏览器

