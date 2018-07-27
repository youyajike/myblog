title: spring boot 给spring开发带来了什么呢?
author: 郑肖峰
tags:
  - spring boot
categories:
  - 技术
date: 2018-07-21 15:55:00
---
springboot是由spring团队最新推出的项目。目的是为了简化spring程序的配置和部署，让开发者将精力更多的花在业务上面。


### 一、它如何简化spring程序的开发？

- 简化***依赖***：用starter对依赖进行抽象
- 简化***启动***：内嵌容器，独立运行
- 简化***配置***：自动化配置，默认配置可支持程序运行

<img src="http://www.yiibai.com/uploads/images/201701/03/557150107_51249.jpg" style="text-align:left;display:inline-block;"/>


### 二、使用Starter简化POM依赖配置

每个springboot程序都要继承`spring-boot-starter-parent`，如下

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
springboot是通过stater来对依赖进行抽象的，比如要开发web项目，我们在pom中引用`spring-boot-starter-web`，那么开发web需要的依赖会被自动引入进来

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
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.0.3.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```

这时候，我们运行

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
[INFO] \- org.springframework.boot:spring-boot-starter-web:jar:2.0.3.RELEASE:compile
[INFO]    +- org.springframework.boot:spring-boot-starter:jar:2.0.3.RELEASE:compile
[INFO]    |  +- org.springframework.boot:spring-boot:jar:2.0.3.RELEASE:compile
[INFO]    |  +- org.springframework.boot:spring-boot-autoconfigure:jar:2.0.3.RELEASE:compile
[INFO]    |  +- org.springframework.boot:spring-boot-starter-logging:jar:2.0.3.RELEASE:compile
[INFO]    |  |  +- ch.qos.logback:logback-classic:jar:1.2.3:compile
[INFO]    |  |  |  +- ch.qos.logback:logback-core:jar:1.2.3:compile
[INFO]    |  |  |  \- org.slf4j:slf4j-api:jar:1.7.25:compile
[INFO]    |  |  +- org.apache.logging.log4j:log4j-to-slf4j:jar:2.10.0:compile
[INFO]    |  |  |  \- org.apache.logging.log4j:log4j-api:jar:2.10.0:compile
[INFO]    |  |  \- org.slf4j:jul-to-slf4j:jar:1.7.25:compile
[INFO]    |  +- javax.annotation:javax.annotation-api:jar:1.3.2:compile
[INFO]    |  +- org.springframework:spring-core:jar:5.0.7.RELEASE:compile
[INFO]    |  |  \- org.springframework:spring-jcl:jar:5.0.7.RELEASE:compile
[INFO]    |  \- org.yaml:snakeyaml:jar:1.19:runtime
[INFO]    +- org.springframework.boot:spring-boot-starter-json:jar:2.0.3.RELEASE:compile
[INFO]    |  +- com.fasterxml.jackson.core:jackson-databind:jar:2.9.6:compile
[INFO]    |  |  +- com.fasterxml.jackson.core:jackson-annotations:jar:2.9.0:compile
[INFO]    |  |  \- com.fasterxml.jackson.core:jackson-core:jar:2.9.6:compile
[INFO]    |  +- com.fasterxml.jackson.datatype:jackson-datatype-jdk8:jar:2.9.6:compile
[INFO]    |  +- com.fasterxml.jackson.datatype:jackson-datatype-jsr310:jar:2.9.6:compile
[INFO]    |  \- com.fasterxml.jackson.module:jackson-module-parameter-names:jar:2.9.6:compile
[INFO]    +- org.springframework.boot:spring-boot-starter-tomcat:jar:2.0.3.RELEASE:compile
[INFO]    |  +- org.apache.tomcat.embed:tomcat-embed-core:jar:8.5.31:compile
[INFO]    |  +- org.apache.tomcat.embed:tomcat-embed-el:jar:8.5.31:compile
[INFO]    |  \- org.apache.tomcat.embed:tomcat-embed-websocket:jar:8.5.31:compile
[INFO]    +- org.hibernate.validator:hibernate-validator:jar:6.0.10.Final:compile
[INFO]    |  +- javax.validation:validation-api:jar:2.0.1.Final:compile
[INFO]    |  +- org.jboss.logging:jboss-logging:jar:3.3.2.Final:compile
[INFO]    |  \- com.fasterxml:classmate:jar:1.3.4:compile
[INFO]    +- org.springframework:spring-web:jar:5.0.7.RELEASE:compile
[INFO]    |  \- org.springframework:spring-beans:jar:5.0.7.RELEASE:compile
[INFO]    \- org.springframework:spring-webmvc:jar:5.0.7.RELEASE:compile
[INFO]       +- org.springframework:spring-aop:jar:5.0.7.RELEASE:compile
[INFO]       +- org.springframework:spring-context:jar:5.0.7.RELEASE:compile
[INFO]       \- org.springframework:spring-expression:jar:5.0.7.RELEASE:compile
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.330 s
[INFO] Finished at: 2018-07-24T14:13:35+08:00
[INFO] Final Memory: 20M/228M
[INFO] ------------------------------------------------------------------------
```

我们可以看到相关的依赖已经被引入了。
springboot官方已经提供了很多stater，每个stater针对一个具体的方面。例如下面

- `spring-boot-starter-websocket` websocket
- `spring-boot-starter-test` 对springboot应用的测试支持
- `spring-boot-starter-security` 安全控制
- `spring-boot-starter-redis` redis的访问
- `spring-boot-starter-logging` 日志服务
...


### 三、使用main方法来启动spring应用
springboot提供了run方法来启动spring应用。如下程序

``` java
package io.github.youyajike;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;

@EnableAutoConfiguration
public class Application {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }
}
```

因为我们已经在pom中使用了`spring-boot-starter-parent`，所以maven有一个run目标，我们可以执行

``` bash
 mvn spring-boot:run
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
[INFO] >>> spring-boot-maven-plugin:2.0.3.RELEASE:run (default-cli) > test-compile @ myboot >>>
[INFO] 
[INFO] --- maven-resources-plugin:3.0.1:resources (default-resources) @ myboot ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO] Copying 0 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.7.0:compile (default-compile) @ myboot ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-resources-plugin:3.0.1:testResources (default-testResources) @ myboot ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/shawn/Documents/localGitHub/study/java/myboot/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.7.0:testCompile (default-testCompile) @ myboot ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] <<< spring-boot-maven-plugin:2.0.3.RELEASE:run (default-cli) < test-compile @ myboot <<<
[INFO] 
[INFO] --- spring-boot-maven-plugin:2.0.3.RELEASE:run (default-cli) @ myboot ---

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.0.3.RELEASE)

2018-07-24 14:39:44.711  INFO 36532 --- [           main] io.github.youyajike.Application          : Starting Application on localhost with PID 36532 (/Users/shawn/Documents/localGitHub/study/java/myboot/target/classes started by shawn in /Users/shawn/Documents/localGitHub/study/java/myboot)
2018-07-24 14:39:44.716  INFO 36532 --- [           main] io.github.youyajike.Application          : No active profile set, falling back to default profiles: default
2018-07-24 14:39:44.879  INFO 36532 --- [           main] ConfigServletWebServerApplicationContext : Refreshing org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext@15b52143: startup date [Tue Jul 24 14:39:44 CST 2018]; root of context hierarchy
2018-07-24 14:39:46.523  INFO 36532 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2018-07-24 14:39:46.579  INFO 36532 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2018-07-24 14:39:46.579  INFO 36532 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/8.5.31
2018-07-24 14:39:46.601  INFO 36532 --- [ost-startStop-1] o.a.catalina.core.AprLifecycleListener   : The APR based Apache Tomcat Native library which allows optimal performance in production environments was not found on the java.library.path: [/Users/shawn/Library/Java/Extensions:/Library/Java/Extensions:/Network/Library/Java/Extensions:/System/Library/Java/Extensions:/usr/lib/java:.]
2018-07-24 14:39:46.718  INFO 36532 --- [ost-startStop-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2018-07-24 14:39:46.718  INFO 36532 --- [ost-startStop-1] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 1844 ms
2018-07-24 14:39:46.890  INFO 36532 --- [ost-startStop-1] o.s.b.w.servlet.ServletRegistrationBean  : Servlet dispatcherServlet mapped to [/]
2018-07-24 14:39:46.896  INFO 36532 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'characterEncodingFilter' to: [/*]
2018-07-24 14:39:46.897  INFO 36532 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'hiddenHttpMethodFilter' to: [/*]
2018-07-24 14:39:46.897  INFO 36532 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'httpPutFormContentFilter' to: [/*]
2018-07-24 14:39:46.897  INFO 36532 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'requestContextFilter' to: [/*]
2018-07-24 14:39:47.097  INFO 36532 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**/favicon.ico] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-07-24 14:39:47.418  INFO 36532 --- [           main] s.w.s.m.m.a.RequestMappingHandlerAdapter : Looking for @ControllerAdvice: org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext@15b52143: startup date [Tue Jul 24 14:39:44 CST 2018]; root of context hierarchy
2018-07-24 14:39:47.558  INFO 36532 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error]}" onto public org.springframework.http.ResponseEntity<java.util.Map<java.lang.String, java.lang.Object>> org.springframework.boot.autoconfigure.web.servlet.error.BasicErrorController.error(javax.servlet.http.HttpServletRequest)
2018-07-24 14:39:47.560  INFO 36532 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.servlet.error.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2018-07-24 14:39:47.595  INFO 36532 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-07-24 14:39:47.596  INFO 36532 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-07-24 14:39:47.848  INFO 36532 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2018-07-24 14:39:47.928  INFO 36532 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2018-07-24 14:39:47.937  INFO 36532 --- [           main] io.github.youyajike.Application          : Started Application in 3.686 seconds (JVM running for 7.595)
```

使用普通的java main方法调用`SpringApplication`类，引导应用程序启动spring。我们上面配置了`spring-boot-starter-web`，所以spring会启动内嵌的tomcat容器,因此我们可以在日志里面看到tomcat启动并监听8080端口。

我们可以在pom.xml文件中加入以下配置
``` xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```
这样我们可以使用
``` bash
mvn package
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
[INFO] --- maven-resources-plugin:3.0.1:resources (default-resources) @ myboot ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO] Copying 0 resource
[INFO] 
[INFO] --- maven-compiler-plugin:3.7.0:compile (default-compile) @ myboot ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-resources-plugin:3.0.1:testResources (default-testResources) @ myboot ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/shawn/Documents/localGitHub/study/java/myboot/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.7.0:testCompile (default-testCompile) @ myboot ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-surefire-plugin:2.21.0:test (default-test) @ myboot ---
[INFO] No tests to run.
[INFO] 
[INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ myboot ---
[INFO] Building jar: /Users/shawn/Documents/localGitHub/study/java/myboot/target/myboot-1.0.0-SNAPSHOT.jar
[INFO] 
[INFO] --- spring-boot-maven-plugin:2.0.3.RELEASE:repackage (default) @ myboot ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.289 s
[INFO] Finished at: 2018-07-24T15:28:23+08:00
[INFO] Final Memory: 22M/271M
[INFO] ------------------------------------------------------------------------
```
来构建可执行jar包`myboot-1.0.0-SNAPSHOT.jar`。
我们使用命令
``` bash 
jar tvf myboot-1.0.0-SNAPSHOT.jar
     0 Tue Jul 24 15:28:22 CST 2018 META-INF/
   521 Tue Jul 24 15:28:22 CST 2018 META-INF/MANIFEST.MF
     0 Tue Jul 24 15:28:22 CST 2018 org/
     0 Tue Jul 24 15:28:22 CST 2018 org/springframework/
     0 Tue Jul 24 15:28:22 CST 2018 org/springframework/boot/
     0 Tue Jul 24 15:28:22 CST 2018 org/springframework/boot/loader/
     0 Tue Jul 24 15:28:22 CST 2018 org/springframework/boot/loader/data/
  2688 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/data/RandomAccessDataFile$DataInputStream.class
     0 Tue Jul 24 15:28:22 CST 2018 org/springframework/boot/loader/jar/
  3116 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/CentralDirectoryEndRecord.class
  3263 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/data/RandomAccessDataFile$FileAccess.class
  4624 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/CentralDirectoryParser.class
   282 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/data/RandomAccessDataFile$1.class
  1693 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/ZipInflaterInputStream.class
  4015 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/data/RandomAccessDataFile.class
 11509 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/Handler.class
   485 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/data/RandomAccessData.class
     0 Tue Jul 24 15:28:22 CST 2018 org/springframework/boot/loader/archive/
   437 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/Archive$EntryFilter.class
   616 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/Bytes.class
   945 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/Archive.class
  7336 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/JarFileArchive.class
  1953 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/PropertiesLauncher$PrefixMatchingArchiveFilter.class
  1484 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/PropertiesLauncher$ArchiveEntryFilter.class
   266 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/PropertiesLauncher$1.class
 19737 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/PropertiesLauncher.class
  4684 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/Launcher.class
  1502 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/MainMethodRunner.class
  3608 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/ExecutableArchiveLauncher.class
  1721 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/WarLauncher.class
  1585 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/JarLauncher.class
     0 Tue Jul 24 15:28:22 CST 2018 org/springframework/boot/loader/util/
  5203 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/util/SystemPropertyUtils.class
  1527 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/LaunchedURLClassLoader$UseFastConnectionExceptionsEnumeration.class
  5687 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/LaunchedURLClassLoader.class
   702 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarURLConnection$1.class
  1779 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/JarFileArchive$EntryIterator.class
  4306 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarURLConnection$JarEntryName.class
  1487 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/ExplodedArchive$FileEntryIterator$EntryComparator.class
  9852 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarURLConnection.class
  2062 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarFile$1.class
  3837 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/ExplodedArchive$FileEntryIterator.class
  1102 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/ExplodedArchive$FileEntry.class
   273 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/ExplodedArchive$1.class
  1233 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarFile$2.class
  1374 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarFile$JarFileType.class
  5243 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/ExplodedArchive.class
  1081 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/JarFileArchive$JarFileEntry.class
 14915 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarFile.class
  3414 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarEntry.class
   345 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/FileHeader.class
  3555 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/StringSequence.class
  4976 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/AsciiBytes.class
  1593 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarFileEntries$1.class
  1997 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarFileEntries$EntryIterator.class
 10728 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarFileEntries.class
   540 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/CentralDirectoryVisitor.class
   299 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/JarEntryFilter.class
  5267 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/jar/CentralDirectoryFileHeader.class
   302 Thu Jun 14 11:48:26 CST 2018 org/springframework/boot/loader/archive/Archive$Entry.class
     0 Tue Jul 24 15:28:22 CST 2018 BOOT-INF/
     0 Tue Jul 24 15:28:22 CST 2018 BOOT-INF/classes/
     0 Tue Jul 24 14:30:58 CST 2018 BOOT-INF/classes/io/
     0 Tue Jul 24 14:30:58 CST 2018 BOOT-INF/classes/io/github/
     0 Tue Jul 24 14:30:58 CST 2018 BOOT-INF/classes/io/github/youyajike/
     0 Tue Jul 24 15:28:22 CST 2018 META-INF/maven/
     0 Tue Jul 24 15:28:22 CST 2018 META-INF/maven/com.zhengxiaofeng/
     0 Tue Jul 24 15:28:22 CST 2018 META-INF/maven/com.zhengxiaofeng/myboot/
   777 Tue Jul 24 14:39:20 CST 2018 BOOT-INF/classes/io/github/youyajike/Application.class
  1038 Tue Jul 24 15:26:32 CST 2018 META-INF/maven/com.zhengxiaofeng/myboot/pom.xml
    98 Tue Jul 24 15:28:22 CST 2018 META-INF/maven/com.zhengxiaofeng/myboot/pom.properties
     0 Tue Jul 24 15:28:22 CST 2018 BOOT-INF/lib/
   588 Thu Jun 14 12:00:42 CST 2018 BOOT-INF/lib/spring-boot-starter-web-2.0.3.RELEASE.jar
   592 Thu Jun 14 11:46:48 CST 2018 BOOT-INF/lib/spring-boot-starter-2.0.3.RELEASE.jar
930680 Thu Jun 14 11:32:28 CST 2018 BOOT-INF/lib/spring-boot-2.0.3.RELEASE.jar
1162436 Thu Jun 14 11:40:30 CST 2018 BOOT-INF/lib/spring-boot-autoconfigure-2.0.3.RELEASE.jar
   613 Thu Jun 14 11:46:48 CST 2018 BOOT-INF/lib/spring-boot-starter-logging-2.0.3.RELEASE.jar
290339 Fri Mar 31 21:27:54 CST 2017 BOOT-INF/lib/logback-classic-1.2.3.jar
471901 Fri Mar 31 21:27:16 CST 2017 BOOT-INF/lib/logback-core-1.2.3.jar
 41203 Thu Mar 16 17:36:32 CST 2017 BOOT-INF/lib/slf4j-api-1.7.25.jar
 17519 Sun Nov 19 01:08:44 CST 2017 BOOT-INF/lib/log4j-to-slf4j-2.10.0.jar
255485 Sun Nov 19 00:48:58 CST 2017 BOOT-INF/lib/log4j-api-2.10.0.jar
  4596 Thu Mar 16 17:37:48 CST 2017 BOOT-INF/lib/jul-to-slf4j-1.7.25.jar
 26586 Wed Feb 21 15:54:16 CST 2018 BOOT-INF/lib/javax.annotation-api-1.3.2.jar
1226075 Tue Jun 12 14:42:16 CST 2018 BOOT-INF/lib/spring-core-5.0.7.RELEASE.jar
 21703 Tue Jun 12 14:42:04 CST 2018 BOOT-INF/lib/spring-jcl-5.0.7.RELEASE.jar
297518 Sat Oct 14 11:44:44 CST 2017 BOOT-INF/lib/snakeyaml-1.19.jar
   645 Thu Jun 14 12:00:40 CST 2018 BOOT-INF/lib/spring-boot-starter-json-2.0.3.RELEASE.jar
1349339 Tue Jun 12 01:14:32 CST 2018 BOOT-INF/lib/jackson-databind-2.9.6.jar
 66519 Sat Jul 29 20:53:26 CST 2017 BOOT-INF/lib/jackson-annotations-2.9.0.jar
323848 Mon Jun 11 17:53:30 CST 2018 BOOT-INF/lib/jackson-core-2.9.6.jar
 33395 Tue Jun 12 04:32:58 CST 2018 BOOT-INF/lib/jackson-datatype-jdk8-2.9.6.jar
 99987 Tue Jun 12 04:35:34 CST 2018 BOOT-INF/lib/jackson-datatype-jsr310-2.9.6.jar
  8645 Tue Jun 12 04:31:32 CST 2018 BOOT-INF/lib/jackson-module-parameter-names-2.9.6.jar
   591 Thu Jun 14 12:00:40 CST 2018 BOOT-INF/lib/spring-boot-starter-tomcat-2.0.3.RELEASE.jar
3115994 Fri Apr 27 21:24:52 CST 2018 BOOT-INF/lib/tomcat-embed-core-8.5.31.jar
240244 Fri Apr 27 21:24:54 CST 2018 BOOT-INF/lib/tomcat-embed-el-8.5.31.jar
256776 Fri Apr 27 21:24:54 CST 2018 BOOT-INF/lib/tomcat-embed-websocket-8.5.31.jar
1133563 Tue May 15 10:46:40 CST 2018 BOOT-INF/lib/hibernate-validator-6.0.10.Final.jar
 93107 Tue Dec 19 16:23:28 CST 2017 BOOT-INF/lib/validation-api-2.0.1.Final.jar
 66469 Wed Feb 14 13:23:28 CST 2018 BOOT-INF/lib/jboss-logging-3.3.2.Final.jar
 65100 Sat Sep 09 14:47:28 CST 2017 BOOT-INF/lib/classmate-1.3.4.jar
1254656 Tue Jun 12 14:43:24 CST 2018 BOOT-INF/lib/spring-web-5.0.7.RELEASE.jar
660545 Tue Jun 12 14:42:24 CST 2018 BOOT-INF/lib/spring-beans-5.0.7.RELEASE.jar
789866 Tue Jun 12 14:44:06 CST 2018 BOOT-INF/lib/spring-webmvc-5.0.7.RELEASE.jar
366340 Tue Jun 12 14:42:42 CST 2018 BOOT-INF/lib/spring-aop-5.0.7.RELEASE.jar
1090739 Tue Jun 12 14:42:54 CST 2018 BOOT-INF/lib/spring-context-5.0.7.RELEASE.jar
280032 Tue Jun 12 14:42:44 CST 2018 BOOT-INF/lib/spring-expression-5.0.7.RELEASE.jar
```
可以看到打的jar包里面已经包含了运行所需的依赖。
我们可以运行
``` bash
java -jar myboot-1.0.0-SNAPSHOT.jar

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.0.3.RELEASE)

2018-07-24 15:32:42.226  INFO 36769 --- [           main] io.github.youyajike.Application          : Starting Application v1.0.0-SNAPSHOT on localhost with PID 36769 (/Users/shawn/Documents/localGitHub/study/java/myboot/target/myboot-1.0.0-SNAPSHOT.jar started by shawn in /Users/shawn/Documents/localGitHub/study/java/myboot/target)
2018-07-24 15:32:42.231  INFO 36769 --- [           main] io.github.youyajike.Application          : No active profile set, falling back to default profiles: default
2018-07-24 15:32:42.331  INFO 36769 --- [           main] ConfigServletWebServerApplicationContext : Refreshing org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext@4141d797: startup date [Tue Jul 24 15:32:42 CST 2018]; root of context hierarchy
2018-07-24 15:32:44.508  INFO 36769 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2018-07-24 15:32:44.564  INFO 36769 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2018-07-24 15:32:44.565  INFO 36769 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/8.5.31
2018-07-24 15:32:44.592  INFO 36769 --- [ost-startStop-1] o.a.catalina.core.AprLifecycleListener   : The APR based Apache Tomcat Native library which allows optimal performance in production environments was not found on the java.library.path: [/Users/shawn/Library/Java/Extensions:/Library/Java/Extensions:/Network/Library/Java/Extensions:/System/Library/Java/Extensions:/usr/lib/java:.]
2018-07-24 15:32:44.781  INFO 36769 --- [ost-startStop-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2018-07-24 15:32:44.782  INFO 36769 --- [ost-startStop-1] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 2456 ms
2018-07-24 15:32:45.107  INFO 36769 --- [ost-startStop-1] o.s.b.w.servlet.ServletRegistrationBean  : Servlet dispatcherServlet mapped to [/]
2018-07-24 15:32:45.115  INFO 36769 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'characterEncodingFilter' to: [/*]
2018-07-24 15:32:45.117  INFO 36769 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'hiddenHttpMethodFilter' to: [/*]
2018-07-24 15:32:45.118  INFO 36769 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'httpPutFormContentFilter' to: [/*]
2018-07-24 15:32:45.119  INFO 36769 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'requestContextFilter' to: [/*]
2018-07-24 15:32:45.391  INFO 36769 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**/favicon.ico] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-07-24 15:32:45.875  INFO 36769 --- [           main] s.w.s.m.m.a.RequestMappingHandlerAdapter : Looking for @ControllerAdvice: org.springframework.boot.web.servlet.context.AnnotationConfigServletWebServerApplicationContext@4141d797: startup date [Tue Jul 24 15:32:42 CST 2018]; root of context hierarchy
2018-07-24 15:32:46.061  INFO 36769 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error]}" onto public org.springframework.http.ResponseEntity<java.util.Map<java.lang.String, java.lang.Object>> org.springframework.boot.autoconfigure.web.servlet.error.BasicErrorController.error(javax.servlet.http.HttpServletRequest)
2018-07-24 15:32:46.063  INFO 36769 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.servlet.error.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2018-07-24 15:32:46.122  INFO 36769 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-07-24 15:32:46.122  INFO 36769 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2018-07-24 15:32:46.405  INFO 36769 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2018-07-24 15:32:46.491  INFO 36769 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2018-07-24 15:32:46.498  INFO 36769 --- [           main] io.github.youyajike.Application          : Started Application in 4.949 seconds (JVM running for 5.547)
```
来启动springboot应用。
从上面的例子我们可以看到，通过springboot提供的支持，我们可以在本地用`main`方法来运行程序，也可以使用`mvn spring-boot:run`来运行程序，还可以使用`java -jar xxxx.jar`来运行程序，不论哪一种，都不需要依赖外部容器。从而使springboot程序的启动和部署得到简化。

### 四、spring boot基于预测的自动化配置
依据项目的`依赖`预测配置项并`自动设置缺省值`是springboot最强大的功能。此前，开发者需要显式的通过xml或者config类来配置应用参数。如果没有配置，系统会运行异常。
上面的例子我没有显式的配置任何属性，但spring boot可以顺利启动并初始化tomcat容器，指定监听8080端口。是不是很神奇？
要把尽自动化配置讲清楚，就要深入springboot的`启动机制`了，这将会在`下一篇文章`(敬请期待...)一起研究，本文不做展开讨论。

通过以上的改善，springboot提高了创建spring应用的效率。最快捷的方式只需要三步:创建包含starter依赖的pom，创建一个main方法包含SpringApplication.run的类，添加`@EnableAutoConfiguration`注解。然后就可以运行这个spring程序了。springboot将开发者从繁琐的配置中解放出来，这是它的意义所在。

---
<br/>
`谢谢阅读文章，我的微信是 zxflove08，如果需要交流可以加我微信`。