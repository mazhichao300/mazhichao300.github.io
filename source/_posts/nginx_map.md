---
layout: post
title:  "Nginx Map"
date:   2017-06-27 17:58:28 +0800
categories: [Web]
tags: [nginx]
---

> 项目中一个接口需要跨域支持，允许一部分的域名进行调用，这里配合nginx map进行允许跨域处理。

# map
map为一个变量设置的映射表，在http段中进行配置。

## 语法
映射表由两列组成，匹配模式和对应的值。在 map 块里的参数指定了源变量值和结果值的对应关系。匹配模式可以是一个简单的字符串或者正则表达式，使用正则表达式要用('~')。
一个正则表达式如果以 “~” 开头，表示这个正则表达式对大小写敏感。以 “~*”开头，表示这个正则表达式对大小写不敏感。

```
map $var1 $var2 { 
    default '';
    x y;
}
```

## 跨域支持实例
```
map $http_origin $corsHost {
    default 0;

    "~https://www.baidu.com"   $http_origin;
    "~.xx.com"   $http_origin;
    "~.yy.com"   $http_origin;
}

server {
    listen 80;
    ...

    location ~ ^/api {
        root xxx;
        fastcgi_pass xxx;
        add_header      Access-Control-Allow-Origin $corsHost;
        add_header      Access-Control-Allow-Credentials true;
    }
}
```

