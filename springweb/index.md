# SpringWeb


<!--more-->
# Spring Web应用

##从零开始一个Spring应用

pom.xml
- [创建pom.xml]https://spring.io/guides/gs/spring-boot/
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.3.2.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>spring-boot</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>spring-boot</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

src/main/java/hello/Application.java

src/main/java/hello/HelloController.java

## Web应用的本质

处理HTTP请求
- 从HTTP请求中提取query string(查询字符串)
- 从HTTP请求中提取payload（负载）body中的参数
返回HTTP响应
- status code
- HTTP response header
- HTTP response body
    - JSON
    - HTML
    
### HTTP GET请求
Query String
- ?param1=value1&param2=value2

通常用来传递非敏感信息

使用@RequsetParam进行接受

### RESTful API（设计API的约定）

使用HTTP动词来代表动作
- GET：获取资源
- POST:新建资源
- PUT：更新资源
- DELETE：删除资源

使用URL（名词）来代表资源
- 资源里没有动词
- 使用复数来代表资源列表

### @RestController
使用RESTful风格的参数
- 使用@PathVariable进行参数提取
### PostMapping
处理POST请求
从HTTP POST请求提取body

### json转class文件工具
idea工具：GsonFormat

### 生成HTTP请求
直接生成HttpServletResponse对象
- 原始、简单
直接返回HTML字符串

返回对象，并自动格式化成JSON

- 常用

- @ResponseBody

模板引擎渲染
- JSP/Velocity/Freemarker
### servlet-转换器

将字节流转换成java对象

将java对象转换成字节流

### 周边生态系统
HTTPS

分布式部署

扩展功能

- 数据库
- Redis缓存
- 信息队列
- RPC(Dubbo/Spring Cloud)
- 微服务化


