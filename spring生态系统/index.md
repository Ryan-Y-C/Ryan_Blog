# Spring生态系统


<!--more-->
# Spring生态系统
## Bean配置与Spring MyBatis实战

0、在Spring容器中引入Bean

1、 Spring+MyBatis（H2/MySQL/Postgres）

2、模板引擎（后端渲染HTML）

3、前后端分离和后端渲染

Controller 做HTTP相关业务

Service 做业务代码

Dao 做数据库相关业务
### 在resources/static 中的html页面可以在浏览器中获取

### 模板引擎freemarker 
[freemarker Springboot starter]http://zetcode.com/springboot/freemarker/
[freemarker API]https://freemarker.apache.org/docs/ref_directive_list.html
[SpringWeb]https://github.com/Ryan-Y-C/SpringDemo
# 动态代理、AOP与Spring

## 动态代理与AOP

### AOP(Aspect-Oriented-Programming) 面向切面编程
- 相对于OOP（面向对象编程）
- AOP是面向切面编程，关注一个统一的切面
- AOP和Spring不是同一个东西

### AOP使用场景

需要统一处理的场景
- 日志
- 缓存
- 鉴权
OOP使用装饰器模式实现
- 装饰器模式 Decorator pattern 
- 动态的为一个对象增加功能，但是不能改变其中结构
- 本质是一个“包装”

装饰器模式一般会声明一个接口，新功能继承这个接口在新的类中添加新功能

### AOP的两种实现方式：JDK动态代理与字节码生成
1、JDK动态代理

- 优点：方便，不需要依赖任何第三方库

- 去点：只适用于接口

必须实现JDK的InvocationHandler接口，必须实现invoke方法，该方法为拦截方法。当调用代理类时候就会调用该方法

```
// 创建代理对象，并获取代理的对象实体的类加载器->拦截代理的接口->处理该对象的代理类
//拦截已有方法的实现，实现AOP
DataService ds= (DataService)Proxy.newProxyInstance(service.getClass().getClassLoader，new Class[]{DataService.Class },new LogProxy())
```

2、CGLIB/ByteBuddy字节码生成

- 优点：强大，不受接口限制

- 缺点：需要引用额外的第三方类库
- 不能增强final类和方法和private方法

引入CGLIB库
```xml
<!-- https://mvnrepository.com/artifact/cglib/cglib -->
<dependency>
    <groupId>cglib</groupId>
    <artifactId>cglib</artifactId>
    <version>2.2.2</version>
</dependency>
```

实现CGLIBD的MethodInterceptor接口，实现该拦截方法
```
DataServiceImpl service = new DataServiceImpl();
Enhancer enhancer =new Enhancer();
//设置实体方法DataServiceImpl为需要代理的实体类
enhancer.setSuperclass(DataServiceImpl.class)
//设置触发拦截时需要调用的类（需要执行拦截方法的类）
enhancer.setCallBack(new LogInterceptpor(sservice)) 
//新创建的类 类似于实体类的子类
DataServiceImpl enhancedService = (DataServiceImpl) enhancer.create();

```
### SpringBoot中使用AOP

#### AOP与Spring
在Spring中使用AOP实现Redis缓存

@Aspect声明切面
- @Before
    - 在方法执行前处理
- After
    - 在方法结束后进行处理
- Around
[SpringAOP]https://developer.ibm.com/zh/technologies/spring/articles/j-spring-boot-aop-web-log-processing-and-distributed-locking/
[Spring官方文档]https://docs.spring.io/spring/docs/4.3.15.RELEASE/spring-framework-reference/html/aop.html
[中文Blog]http://blog.didispace.com/springbootaoplog/
引入SpringBootAOP依赖
```xml
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

配置CGLIB切换CGLIB动态代理
```properties
spring.aop.proxy-target-class=true
```
```
//传达要拦截的方法即含有Cache注解的方法
@Around("@annotation(SpringWeb.anno.Cache)")

//获取被拦截的方法
MethodSignature signature=(MethodSignature)joinPoint.getSignature();

//joinPoint.proceed()执行被拦截的方法

```

### Redis
广泛使用的内存缓存

常见的数据结构
    String/List/Set/ ZSet
Redis
- 完全基于内存
- 优秀的数据结构设计
- 单一线程，避免上下文切换
- 事件驱动，非阻塞
- 因为redis基于内存 所以多个JVM可以基于内存共享资源

 使用docker安装redis

[官方文档]https://hub.docker.com/_/redis

[docker关联redis]https://marcus116.blogspot.com/2019/02/how-to-run-redis-in-docker.html

```
docker pull redis
```
```
docker run -p 6379:6379 -d redis
```
配置Spring redis端口
```properties
spring.redis.host=localhost
spring.redis.port=6379
```
```xml
<dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

将java对象传递给redis,java对象（User等）实现Serializable接口实现可序列化

创建docker mysql
```
 docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=mall -p 3306:3306 -d mysql:8.0.18 --lower_case_table_names=1
```

[Spring AOP Redis Mysql实战]https://github.com/hcsp/spring-aop-redis-mysql/pull/60

