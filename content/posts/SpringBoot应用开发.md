---
title: "SpringBoot应用开发"
date: 2017-10-04T16:26:39+08:00
lastmod: 2017-10-04T16:26:39+08:00
draft: true
description: ""
show_in_homepage: true
show_description: false
license: ''

tags: []
categories: []

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true
math: false
---

<!--more-->

[SpringBoot官网教程] https://spring.io/guides/gs/spring-boot
### 创建SpringBoot应用方式
- 看官方文档创建
    - 创建pom.xml文件
    - 创建web应用文件HelloController
    - 创建应用类Application                            
- clone
- IDEA Initializer
- mvn overType

### 设置maven镜像
[镜像文档]https://developer.aliyun.com/article/78124
1、将镜像写入./.m2/setting.xml对所有项目生效

2、在pom.xml中写入
```xml
<repositories>  
        <repository>  
            <id>alimaven</id>  
            <name>aliyun maven</name>  
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
            <releases>  
                <enabled>true</enabled>  
            </releases>  
            <snapshots>  
                <enabled>false</enabled>  
            </snapshots>  
        </repository>  
</repositories> 
```
### 声明依赖
在字段上声明依赖注解
- @Resource
- @AutoWired

在构造器上声明注解
- Inject
```xml
<!-- https://mvnrepository.com/artifact/javax.inject/javax.inject -->
<dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
</dependency>
```
### 配置bean对象
[相关博客] https://www.itread01.com/content/1549220976.html
1、xml方式
在src/main/java/resources目录下创建app-config.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/mvc
            http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!-- 测试引用xml -->
    <bean id="orderservice" class="hello.service.OrderService"/>
    <bean id="userservice" class="hello.service.UserService"/>
</beans>
```
创建该类实现依赖注入
```java
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.ImportResource;

@Configuration
    public class ConfigClass {
}
```
2、注解方式
```java
package hello.config;

import hello.service.UserService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
@Configuration
public class JavaConfiguration {
    @Bean
    public UserService userService(){
        return new UserService();
    }
}
```

### docker 方式创建mysql

```
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=springboot -v J:/springboot/mysql:/var/lib/mysql -d -p 3306:3306 mysql
```

### 创建springboot mybatis依赖
[springboot mybatis] http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/
```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.3</version>
</dependency>
```

创建mybatis 数据库操作接口
```java
import hello.service.User;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

@Mapper
public interface UserMapper {
    @Select("SELECT * FROM USER WHERE id = #{id}")
    User findUserById(@Param("id") Integer id);
}
```

调用响应方法（findUserById）获取数据

[创建springboot DataSource] https://howtodoinjava.com/spring-boot2/datasource-configuration

创建application.properties配置文件

引入mysql 驱动 maven依赖

### 登录页面
[Spring安全保护] https://spring.io/guides/gs/securing-web/
依然spring鉴权框架依赖
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

Spring安全框架会拦截所有的请求
```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
	@Override
	protected void configure(HttpSecurity http) throws Exception {
	    http.csrf().disable();//关闭跨站请求伪造防护，才能访问auth之下的请求
		http
			.authorizeRequests()
				.antMatchers("/", "/auth/**").permitAll()//设置不去拦截的请求
				.anyRequest().authenticated()
				.and()
			.formLogin()
				.loginPage("/login")
				.permitAll()
				.and()
			.logout()
				.permitAll();
	}

	@Bean
	@Override
	public UserDetailsService userDetailsService() {
		UserDetails user =
			 User.withDefaultPasswordEncoder()
				.username("user")
				.password("password")
				.roles("USER")
				.build();

		return new InMemoryUserDetailsManager(user);
	}
}
```
- 鉴权 Authentication 是不是创造某个东西的人
- 验权 Authorization 是否有权力访问

鉴别用户名和密码
```
UserDetails userDetails = userDetailsService.loadUserByUsername(username);
        UsernamePasswordAuthenticationToken token =
                new UsernamePasswordAuthenticationToken(
                        userDetails,
                        password,
                        userDetails.getAuthorities());
```

添加鉴权管理器 AuthenticationManager

添加Spring加密算法
```
public BCryptPasswordEncoder bCryptPasswordEncoder(){
    return new BCryptPasswordEncoder();
}
```
mybatis 设置驼峰形式
```
mybatis.configuration.map-underscore-to-camel-case=true
```
[spring docs] https://docs.spring.io/spring-security/site/apidocs/org/springframework/security/core/context/SecurityContextHolder.html
注销登录 SecurityContextHolder.clearContext();

使用注解@JsonIgnore忽略序列话字段

### 自动化测试与持续集成
1、单元测试
- 黑盒测试
- 单个服务测试

2、集成测试
- 测试对外接口功能

3、冒烟测试

4、回归测试
- 发现问题解决之前的问题
#### 使用junit4/junit5/TestNG测试库
[junit5 官方文档] https://junit.org/junit5/docs/current/user-guide/
```xml
<build>
    <plugins>
            <!--单元测试 -->
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.22.2</version>
        </plugin>
            <!--继承测试 -->
        <plugin>
            <artifactId>maven-failsafe-plugin</artifactId>
            <version>2.22.2</version>
        </plugin>
    </plugins>
</build>

<dependencies>
    <!-- ... -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.3.2</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <version>5.3.2</version>
        <scope>test</scope>
    </dependency>
    <!-- ... -->
</dependencies>

```

test阶段只针对一个具体的类

跳转测试快捷键ctrl+shift+t

[冒烟测试框架] https://site.mockito.org/

相关依赖和注解
```xml
<!-- https://mvnrepository.com/artifact/org.mockito/mockito-junit-jupiter -->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-junit-jupiter</artifactId>
    <version>2.17.0</version>
    <scope>test</scope>
</dependency>
```
```java
@ExtendWith(MockitoExtension.class)
public class ExampleTest {

    @Mock
    private List list;

    @Test
    public void shouldDoSomething() {
        list.add(100);
    }
}
```
#### 集成测试
引入Spring测试包
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
</dependency>
```

### 部署持续集成CT Jenkins/Travis-ci/circleci Appveyor

docker pull jenkins/jenkins

```xml
<plugin>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.22.2</version>
    <!-- 排除集成测试部分 -->
    <configuration>
        <excludes>
            <exclude>**/*IntegrationTest</exclude>
        </excludes>
    </configuration>
</plugin>
<plugin>
    <artifactId>maven-failsafe-plugin</artifactId>
    <version>2.22.2</version>
    <!-- 将集成测试包含进去 -->
    <configuration>
        <includes>
            <include>**/*IntegrationTest</include>
        </includes>
    </configuration>
</plugin>
```

查看依赖关系 mvn dependency:tree

maven wrapper
```
mvn -N io.takari:maven:0.7.7:wrapper
```

给特定阶段添加插件 
[maven exec plugin] https://www.mojohaus.org/exec-maven-plugin/


####  TDD模式开发博客模块
先写一个必定会失败的测试

进入docker容器:docker exec it 容器名称 sh 

包含docker容器的Jenkins：weweave/jenkins-lts-docker

```
docker run -it -p8081:8080 -v `pwd`/jenkins-data:/var/jenkins_home -v `pwd`/jenkins-m2:/var/jenkins_home/.m2 -v /var/run/docker.sock:/var/run/docker.sock weweave/jenkins-lts-docker
```

设置文件权限

chmod 777 jenkins-data

chmod 777 jenkins-m2

docker registry

[创建docker私服] https://docs.docker.com/registry/

[demo] https://github.com/blindpirate/xiedaimala-springboot


