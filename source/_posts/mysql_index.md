---
layout: post
title:  "Mysql索引实现原理"
date:   2017-04-21 11:50:23 +0800
categories: [mysql]
tags: [索引]
---

> 本文说明mysql索引的实现原理以及索引使用

## 索引是什么
如果你要从一本书中找一段内容，有两种方式：

1. 一页一页的翻，直至找到内容；
2. 先通过`目录`定位到相应章节，再去查找；

正常情况下，第二种方式是可以明显加快查找速度的。在这里，`目录`就可以理解为索引。

## 索引实现原理
Mysql的索引是存储引擎实现的，MyIsam、InnoDB是采用BTree数据结构实现的索引。  
[**BTree介绍**](https://zh.wikipedia.org/wiki/B%E6%A0%91)

## 索引分类
### 唯一索引
`unique key`：索引值必须唯一。主键也是唯一索引的一种，关键字是`primary key`。

### 全文索引
InnoDB不支持，Myisam支持，一般在`char`、`varchar`或`text`列上创建。

```sql
Create table 表名
(id int not null primary anto_increment,
title varchar(100),
FULLTEXT(title)
)type=myisam
```

### 单列索引与多列索引
索引可以是单列索引也可以是多列索引(复合索引)，多列索引的`最左前缀原则`：在sql where子句中列的顺序要保持和多索引的一致或以多列索引顺序出现，只要出现非顺序出现、断层都无法利用到多列索引或者只能用到多列索引的部分列。

例如, `index(a, b, c)`:

1. `where a=1`可以命中索引的a字段
2. `where a=1 and b=2`可以命中索引的a, b字段
3. `where a=1 and b=2 and c=3`可以命中索引的全部字段
4. `where a=1 and c=3`只能命中a字段
5. `where b=2`等非以a字段开头的查询都无法命中索引

### explain
`explain`可以用来提供sql语句执行信息，可以配合`select delete insert replace update`使用。

```sql
mysql> explain select * from shopinfo_for_proto where  userid=22681834 and zdid=85570921;
+----+-------------+--------------------+------+---------------+--------+---------+-------+------+-------------+
| id | select_type | table              | type | possible_keys | key    | key_len | ref   | rows | Extra       |
+----+-------------+--------------------+------+---------------+--------+---------+-------+------+-------------+
|  1 | SIMPLE      | shopinfo_for_proto | ref  | userid        | userid | 8       | const |   16 | Using where |
+----+-------------+--------------------+------+---------------+--------+---------+-------+------+-------------+
```

从中可以分析出是否命中了缓存，`key`表示实际命中的缓存，`key_len`表示命中的缓存大小，可以用来分析命中了多列索引中的几列。

`explain`输出的各字段的[详细解释见官网](https://dev.mysql.com/doc/refman/5.7/en/explain-output.html)