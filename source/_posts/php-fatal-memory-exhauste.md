---
title: PHP Fatal error Allowed memory size exhauste
date: 2017-03-17 10:33:07
categories: [PHP]
tags: [PHP]
---

> 有时运行PHP会报错误`Fatal error: Allowed memory size of 134217728 bytes exhauste`，直接原因是PHP所占用的内存超过了设定大小

## 解决办法
### 修改内存限制
- 取消限制`ini_set("memory_limit","-1");`
- 根据需要设定上限`ini_set("memory_limit","2048M");`

### 通过优化程序以减少内存占用
- 变量使用后，及时`unset()`等
