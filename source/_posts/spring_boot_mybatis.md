---
layout: post
title:  "spring boot mybatis"
date:   2017-05-08 11:50:23 +0800
categories: [JAVA]
tags: [springboot,mybatis]
---

>本文介绍如何整合spring boot与Mybatis

# Spring Boot
运行起spring boot比较容易，要注意的是入口类要放到所有其他类的父目录。

# 整合Mybatis
- [官方文档](http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)
- [github例子](https://github.com/mybatis/spring-boot-starter/tree/master/mybatis-spring-boot-samples/mybatis-spring-boot-sample-xml)

# 打成war包，供部署到独立的tomcat
spring boot默认是打成一个集成tomcat的jar包，spring boot同样提供了打包war的方式。
## 修改打包方式
```xml
pom.xml中修改打包方式
<packaging>war</packaging>
```
## 移除嵌入式tomcat依赖
```xml
pom.xml dependencies下添加
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
```
## 修改入口类-继承SpringBootServletInitializer并重写configure方法
```java
@SpringBootApplication
public class Application extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }
}
```
至此，可以通过mvn clean package导出war包。同样，也可以使用嵌入式tomcat进行调试。

# 碰到的问题
## 找不到主程序
class文件未生成，重新编译即可
## archive for required library tomcat-embed cannot be read
原因是Jar包损坏，将.m2下的相应目录删除，重新下载即解决。
