title: 'spring boot '
author: 郑肖峰
tags:
  - spring boot
categories:
  - 技术
date: 2018-07-21 15:55:00
---
springboot是由spring团队最新推出的项目。目的是为了简化spring程序的配置和部署，让开发者将精力更多的花在业务上面。

### 它如何简化spring程序的开发？

- 简化***配置***：自动化配置，默认配置可支持程序运行
- 简化***启动***：内嵌容器，独立运行
- 简化***依赖管理***：提供parent POM，用starter来讲依赖抽象化

<img src="http://www.yiibai.com/uploads/images/201701/03/557150107_51249.jpg" style="text-align:left;display:inline-block;"/>

### 关于Stater

每个springboot程序都要集成`spring-boot-starter-parent`，如下
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.zhengxiaofeng</groupId>
    <artifactId>myboot</artifactId>
    <version>1.0.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.3.RELEASE</version>
    </parent>
</project>
```
`spring-boot-starter-parent`本身不提供依赖关系，但是可以设置默认的maven值和依赖管理。
我们运行 
``` bash
mvn dependency:tree

[WARNING] 
[WARNING] Some problems were encountered while building the effective settings
[WARNING] Unrecognised tag: 'release' (position: START_TAG seen ...</url>\n\t                <release>... @51:27)  @ /Users/shawn/Documents/tools/apache-maven-3.3.9/conf/settings.xml, line 51, column 27
[WARNING] Unrecognised tag: 'release' (position: START_TAG seen ...</url>\n\t                <release>... @50:27)  @ /Users/shawn/.m2/settings.xml, line 50, column 27
[WARNING] 
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building myboot 1.0.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-dependency-plugin:3.0.2:tree (default-cli) @ myboot ---
[INFO] com.zhengxiaofeng:myboot:jar:1.0.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.022 s
[INFO] Finished at: 2018-07-23T14:46:32+08:00
[INFO] Final Memory: 19M/220M
[INFO] ------------------------------------------------------------------------
```
可以看到里面并没有依赖树出现。
