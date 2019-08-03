####1 搭建数据库

~~~mysql
CREATE TABLE `userinfo` (
  `id` int(20) NOT NULL AUTO_INCREMENT,
  `username` varchar(20) DEFAULT NULL,
  `password` varchar(20) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8
~~~

####2 创建maven工程

#### 3 在pom文件中导入spring boot需要的jar包

~~~xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.hu</groupId>
  <artifactId>SpringBootPro</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  <!-- spring boot jar -->
  <dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.9.RELEASE</version>
		<type>pom</type>
	</dependency>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
		<version>1.5.9.RELEASE</version>
	</dependency>
	<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <version>1.5.9.RELEASE</version>
        <scope>test</scope>
    </dependency>
  </dependencies>
  <build/>
</project>
~~~

#### 4 增加*application.properties* 属性文件

在*src/main/resources*下新建文件： application.properties

~~~properties
server.port=8010
server.contextPath=/springboot
~~~

#### 5 增加启动类

*MainService.java*：

~~~java
package com.hu.server;
 
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
 
@SpringBootApplication
public class MainServer {
	public static void main(String[] args) {
		SpringApplication.run(MainServer.class, args);
	}
}
~~~

#### 6 启动测试

运行*MainService.java*

右键→ *run as/ debug as → java application*

#### 7 编写第一个测试接口

① 将*MainService.java*申明当前*Controller*

② 添加测试地址

③ 启动*main*方法，前端访问

~~~java
package com.hu.server;
 
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
@Controller
@SpringBootApplication
public class MainServer {
	public static void main(String[] args) {
		SpringApplication.run(MainServer.class, args);
	}
	
	@RequestMapping(value = "/hello")
	public @ResponseBody String hello(){
		return "Hello Boot";
	}
}
~~~

#### 8 整合日志框架

① 导入日志*jar*包*pom.xml*下增加

~~~xml
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-logging</artifactId>
		<version>1.5.9.RELEASE</version>
	</dependency>
~~~

② 增加*logback.xml*配置文件，放到*resource*目录下

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!--定义日志文件的存储地址 勿在 LogBack 中使用相对路径-->
    <property name="LOG_HOME" value="C:\Users" />
    <!-- 不用彩色控制台输出 -->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--<!–格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符–>-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
    </appender>
    <!-- 按照每天生成日志文件 -->
    <appender name="DAYINFO"  class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名-->
     <FileNamePattern>${LOG_HOME}/TestSpringBoot_info.log.%d{yyyy-MM-dd}.log</FileNamePattern>
            <!--日志文件保留天数-->
            <MaxHistory>30</MaxHistory>
        </rollingPolicy>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>info</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
        <!--日志文件最大的大小-->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>10MB</MaxFileSize>
        </triggeringPolicy>
    </appender>
 
    <appender name="DAYERROR"  class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名-->
 <FileNamePattern>${LOG_HOME}/TestSpringBoot_error.log.%d{yyyy-MM-dd}.log</FileNamePattern>
            <!--日志文件保留天数-->
            <MaxHistory>30</MaxHistory>
        </rollingPolicy>
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>error</level>
            <onMatch>ACCEPT</onMatch>
            <onMismatch>DENY</onMismatch>
        </filter>
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
        <!--日志文件最大的大小-->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <MaxFileSize>10MB</MaxFileSize>
        </triggeringPolicy>
    </appender>
 
    <!-- 日志输出级别 -->
    <root level="INFO">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="DAYERROR" />
        <appender-ref ref="DAYINFO" />
    </root>
</configuration>
~~~

搬运地址：[spring boot项目搭建示例](https://blog.csdn.net/huxiangen/article/details/80911192 "spring boot项目搭建示例")

<https://blog.csdn.net/huxiangen/article/details/80911192>

