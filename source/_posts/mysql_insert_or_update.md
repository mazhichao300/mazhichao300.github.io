---
layout: post
title:  "insert or update"
date:   2016-12-06 19:51:28 +0800
categories: [Web]
tags: [mysql]
---
>实现记录有则更新，无则插入

## ON DUPLICATE KEY UPDATE
需要先设置UNIQUE KEY索引，然后使用ON DUPLICATE KEY UPDATE语句
```mysql
INSERT INTO table (a,b,c) VALUES (1,2,3),(4,5,6)
ON DUPLICATE KEY UPDATE c=3;
```

-----
## 参考文档
[ON DUPLICATE KEY UPDATE重复插入时更新](hhttp://lobert.iteye.com/blog/1604122)