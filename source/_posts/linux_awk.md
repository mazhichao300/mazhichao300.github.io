---
layout: post
title:  Linux awk命令
date:   2017-07-12 08:52:33 +0800
categories: [linux]
tags: [awk]
---
> awk是一个强大的文本分析工具，简单来说awk就是把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分再进行各种分析处理。

> awk有3个不同版本: awk、nawk和gawk，未作特别说明，一般指gawk，gawk 是 AWK 的 GNU 版本。

> awk其名称得自于它的创始人 Alfred Aho 、Peter Weinberger 和 Brian Kernighan 姓氏的首个字母。实际上 AWK 的确拥有自己的语言： AWK 程序设计语言 ， 三位创建者已将它正式定义为“样式扫描和处理语言”。它允许您创建简短的程序，这些程序读取输入文件、为数据排序、处理数据、对输入执行计算以及生成报表，还有无数其他的功能。


## 使用方法
```shell
awk '{pattern + action}' {filenames}
```

pattern 表示 AWK 在数据中查找的内容，而 action 是在找到匹配内容时所执行的一系列命令。花括号（{}）不需要在程序中始终出现，但它们用于根据特定的模式对一系列指令进行分组。 pattern就是要表示的正则表达式，用斜杠括起来。


## 实例
```shell
$ cat /etc/passwd | awk -F ':' '{print $1}'
nobody
root
daemon
```
awk工作流程：读入有'\n'换行符分割的一条记录，然后将记录按指定的域分隔符划分域，填充域，$0则表示所有域,$1表示第一个域,$n表示第n个域。默认域分隔符是"空白键" 或 "[tab]键"，所以$1就是用户名。

```shell
#文件number.txt中有5列（以tab分割），统计第三列之和
$ awk 'BEGIN {sum=0} {sum+=$3} END {print sum}' number.txt
71188417507
```

awk工作流程：先执行BEGIN， 然后读入每行，按tab分割后执行`action {sum+=$3}`， 最后执行END将结果打印出来。

```shell
$ awk  '/1425977238827502/' wrong.txt
67205476    1425977238827502    20209908    3752441 86800893918921209
```
匹配模式，搜索wrong.txt中包含1425977238827502的行，没有action，输出整行。


```shell
$ awk  '/1425977238827502/ {print $1}' wrong.txt
67205476
```
匹配模式，搜索wrong.txt中包含1425977238827502的行，按照action输出每一列。


----------------

## 参考
- [The GNU Awk User’s Guide](http://www.gnu.org/software/gawk/manual/gawk.html)
- [linux awk命令详解](http://www.cnblogs.com/ggjucheng/archive/2013/01/13/2858470.html)