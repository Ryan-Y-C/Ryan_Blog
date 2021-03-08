# Java程序的部署


<!--more-->
# Java程序的部署
分布式
- 1、负载均衡
- 2、容灾（单点故障）
- 数据一致性
    - 多台设备使用统一个数据库-单一数据源
- HTTP是无状态的

### 部署

监听端口

响应HTTP请求

进行预定义的业务逻辑处理

部署的版本需要不停的进行更新（分布式更新）

### 分布和部署要解决的问题
环境问题
- 开发环境（测试环境）
- 预发布环境（预生产环境）
- 生产环境（正式环境）

环境兼容问题
- 硬件/软件
- 数据库等
### 发布和部署Java程序
要解决的问题
- 编写的代码
- 所依赖的第三方库
- 你所依赖的特殊环境配置（数据库/缓存等）
- 稳定性
- 升级和回滚

## 1：使用Maven exec plugin
[exec官方文档] https://www.mojohaus.org/exec-maven-plugin/
导入maven依赖
```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>3.0.0</version>
    <configuration>
        <executable>java</executable>
        <arguments>
            <argument>-classpath</argument>
                        <!-- automatically creates the classpath using all project dependencies,
                             also adding the project build directory -->
            <classpath/>
            <argument>com.example.Main</argument>
        </arguments>
    </configuration>
</plugin>
```

运行时加-x参数可以查看详细日志

自动将所有的传递性依赖加入


优点：简单

缺点：不适用于自动化的场景

## 2：jar包
运行mvn命令创造jar包
```
mvn package
```
使用 java -jar \[jar包名字] 启动程序
```
java -classpath com.github.Main
```
解决jar包文件不完整问题
[相关blog] https://www.cnblogs.com/thinking-better/p/7827368.html
```xml
<plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>1.5.3.RELEASE</version>
                <configuration>
                    <mainClass>com.github.hcsp.Application</mainClass>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
```
设置程序端口
```
java -jar -Dserver.port=8081 target\spring-aop-redis-mysql-0.0.1.jar
```
jar包
- 编译后的代码打包
可以直接运行的jar包
- jar包与Manifest

优点：简单可靠

缺点：依赖于JVM环境

## Docker
创建Dockerfile
```
FROM java:openjdk-8u111-alpine

RUN mkdir /app

WORKDIR /app

COPY target/spring-aop-redis-mysql-0.0.1.jar /app

EXPOSE 8080

CMD ["java","-jar","spring-aop-redis-mysql-0.0.1.jar"]
```

运行 //docker build .//创建镜像

创建application.properties配置文件
```
# 请勿改变数据库名、端口、用户名及密码
spring.datasource.url=jdbc:mysql://localhost:3306/mall?characterEncoding=utf-8
spring.datasource.username=root
spring.datasource.password=123456
# 请勿改变Redis端口号
spring.redis.host=localhost
spring.redis.port=6379

spring.aop.proxy-target-class=true
mybatis.config-location=classpath:db/mybatis/config.xml

```
运行容器
```
$ docker run -p 8082:8080 -v J:/lean/spring-aop-redis-mysql/application.properties:/app/config/application.properties a7dad994b5b2
```
Docker运行Java程序

Docker运行数据库

Docker运行Redis

Docker运行 

### Nginx
创建nginx容器
```
$ docker run --restart=always -v J:/lean/spring-aop-redis-mysql/deploy/nginx.conf:/etc/nginx/nginx.conf:ro -p 80:80 -d nginx
```
[nginx官网] http://nginx.org/en/docs/http/load_balancing.html
```
http {
    upstream myapp1 {
        server srv1.example.com;
        server srv2.example.com;
        server srv3.example.com;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://myapp1;
        }
    }
}
```
### 查看咱用80端口的进程

```
netstat -aon | findstr "80"
```
查看对应进程的程序
```
tasklist | findstr "4"
```
删除对应进程
```
taskkill /pid 4 /F
```
查看占用80端口的服务
```
netsh http show servicestate
```
