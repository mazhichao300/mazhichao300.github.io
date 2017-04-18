---
layout: post
title:  "Maven使用"
date:   2017-04-18 21:50:23 +0800
categories: [JAVA]
tags: [java,maven]
---

>本篇文章用来记录使用Maven过程中的一些问题和总结。

### Maven安装


### 解决依赖下载慢的问题
下载慢是由于国内访问官方站点网络不稳定导致，使用阿里云mirror镜像可解决此问题，配置mirror:

```xml
<mirrors>
	<mirror>
		<id>alimaven</id>
		<name>aliyun maven</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
		<mirrorOf>central</mirrorOf>        
	</mirror>
</mirrors>
```

### 配置私服
配置repository即可

### 深度理解`mirror`,`repository`关系
**repository**是仓库，里面存放的是各种jar包和maven插件，可以分为remote库和本地库。

**mirror**则是仓库的镜像，比如maven官方仓库A，阿里云提供了一个仓库B，和A完全一致，就称为A的镜像。

