---
layout: post
title:  "PHP发送HTML格式邮件"
date:   2017-03-17 12:51:23 +0800
categories: [PHP]
tags: [email,PHP]
---


> 经常会有定期发送邮件报表的需求，这里使用HTML写报表模板，然后填充数据后发送。

## 代码如下
```php
// $htmlTemplate为HTML模板
$content = str_replace($find, $replace, $htmlTemplate);

$to = 'xxx@baidu.com';
$subject = '数据日报-' . $day;
$header = "MIME-Version: 1.0\r\n"; //设置MIME版本
$header .= "Content-type: text/html; charset=utf-8\r\n"; //设置内容类型和字符集
$header .= "Content-Transfer-Encoding: 8bit\r\n";
$header .= "From: xx <xx@baidu.com>\r\n"; //设置发件人
$header .= "Cc: xx@mailexample.com/r/n"; // 设置抄送
mail($to, $subject, $content, $header);
```
