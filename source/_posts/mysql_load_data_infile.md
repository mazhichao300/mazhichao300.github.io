---
layout: post
title:  "Mysql load data infile"
date:   2016-12-06 19:51:28 +0800
categories: [Web]
tags: [mysql]
---
>背景：兄弟团队每天定时提供一份全量文件，我们需要将数据更新到DB中。

## 最开始的方案是
逐条数据到DB中判断，有则更新，无则插入，同时对变动过的记录更新时间戳，最后将表中此次未更新的记录全部删掉。但是因数据量较大（300W条），hhvm执行到一定程度就会内存溢出，然后停止运行。

## load data infile
Mysql提供了load data infile，可以快速将文件中的数据导入表中，设置unique key索引并指定replace即可做到有则替换，无则插入。详细用法参照[LOAD DATA INFILE](http://blog.csdn.net/zhao1234567890123456/article/details/41054557)

## load data infile中文乱码解决办法
[load data infile 中文乱码解决](http://lijunjie.iteye.com/blog/258494)
