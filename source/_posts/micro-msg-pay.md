---
title: H5微信支付
date: 2016-07-28 19:21:30
tags:
---

>我们的M站接入了微信支付，虽然不是我开发的，但是过程中参与了部分的方案调研，这里把我知道的记录下来。

#### 针对微信内部浏览器和微信外部浏览器，调用微信支付的技术方案是不同的。
1. 微信外部浏览器
方案没参与，不清楚。应该是打开某个特殊的URL调起微信
2. 微信内部浏览器
此时不能使用方案1，因为已经是在微信内部了，不能再调起微信。这时可以走微信内部公众号的方式，用openid调用微信支付接口，用户，公众号的对应关系为一个Openid。现在的问题就是如何获取openid了。微信提供了oauth授权接口，只需要openid的话，还可以做到静默授权，用户无感知。授权后拿到token,然后在回调里面用token即可取openid。

-----
#### 参考文档
[微信支付官方文档](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419316505&token=&lang=zh_CN)
