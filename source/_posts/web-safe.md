---
title: 常见的安全漏洞简介
date: 2016-08-22 20:07:07
categories: [Web]
tags: [网络安全]
---
# 安全漏洞分类
作用于服务端的漏洞，作用于客户端的漏洞

# 常见的服务端漏洞
## 不好的设计
```php
基于机制不可知来隐藏敏感信息是不安全的

<?php
// 用可预测的COOKIE值判断管理员
if ('admin' == $_COOKIE['role']) {
    echo 'Welcom, admin';
    echo 'You can do whatever you want';
}

// 基于MD5生成TOKEN
$name = $_POST['name'];
$passwd = $_POST['passwd'];

// hello hacker, we use double md5, crack it if you can
// http://www.cmd5.com/
$passwd = md5(md5($passwd));

register($name, $passwd);
```
## 非法文件上传  
### 形成漏洞的前提：  

- 上传的文件能够被Web容器解释执行。所以文件上传后所在的目录要是Web容器所覆盖到的路径。
- 用户能够从Web上访问这个文件。如果文件上传了，但用户无法通过Web访问，或者无法得到Web容器解释这个脚本，那么也不能称之为漏洞。  

### 错误的做法：  
- 对于上传的文件不做限制   
- 允许上传的文件改名，对改的名字不做限制  

### 正确的做法：
- 白名单限制上传文件的类型  
- 抛弃原始文件名，按规则生成新的文件名
- 只提供上传功能：不要允许改名
- 上传的文件单独存放，取消执行权限

## sql注入
就是通过把SQL命令插入到Web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。

```php
// http://10.94.43.36:8112/sqlinject.php?username=&password=%27%20or%20%27%27=%27

<?php
$username = $_GET['username'];
$password = $_GET['password'];

$conn = mysql_connect('127.0.0.1', 'root', '') or die("connect failed". mysql_error());
mysql_select_db('user', $conn);

$sql = "select name,password from student where 
    name='$username' and password='$password'";

$ret = mysql_query($sql, $conn);

$result = [];
while ($row = mysql_fetch_array($ret)) {
    $result[] = $row;
}

$data = [
   'sql' => $sql,
   'data' => $result,
];

exit(json_encode($data));
```
## 命令执行
不可信的用户输入跨越边界进入SHELL

```php
// http://10.94.43.36:8112/rce.php?cmd=whoami
// system函数
$cmd = $_GET['system'];

if (!empty($cmd)) {
    system($cmd);
}

// http://10.94.43.36:8112/rce.php?eval=echo%20%27helloword%27;
// eval函数: eval() 函数把字符串按照 PHP 代码来计算。

$cmd = $_GET['eval'];

if (!empty($cmd)) {
    eval($cmd);
}

```

## 内网代理 
不可信的用户输入跨越边界进入网络访问或文件系统

## 信息泄露
报错信息（sql, stacktrace）  
物理路径泄露
用户信息泄露

本身不会带来很严重问题，但是可能会帮助黑客更快的攻克你的系统

# 常见的客户端漏洞
## xss
什么是XSS？跨站脚本攻击(Cross Site Scripting)
反射型
持久型
DOM型

```php
// http://cp01-rdqa-dev094.cp01.baidu.com:8112/xsstest.php?username=%3Cscript%3Ealert(document.cookie)%3C/script%3E  

header('X-XSS-Protection:0');
header('Content-Security-Policy:0');

$username = $_GET['username'];

echo 'Welcom ' . $username;
```
## csrf
CSRF（Cross-site request forgery跨站请求伪造，也被称为“One Click Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。尽管听起来像跨站脚本（XSS），但它与XSS非常不同，并且攻击方式几乎相左。XSS利用站点内的信任用户，而CSRF则通过伪装来自受信任用户的请求来利用受信任的网站。与XSS攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比XSS更具危险性。

```php
// http://cp01-rdqa-dev094.cp01.baidu.com:8112/csrftest.php

header('X-XSS-Protection:0');
header('Content-Security-Policy:0');

$username = $_GET['username'];

$test = '<img src="https://passport.baidu.com/passport/?logout"/>';

echo $test;
```

## Json Hijack
