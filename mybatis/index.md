# MyBatis


<!--more-->
# MyBatis - ORM框架

## ORM(Object Relationship Mapping)

对象关系映射
- 自动完成对象到数据库的映射
Association
- 自动装配对象

## MyBatis
[MyBatis入门] https://mybatis.org/mybatis-3/zh/getting-started.html

1、引入Maven依赖
```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.5</version>
</dependency>
```
2、创建MyBatis文件夹及其配置文件
- 文件路径：main/resources/db/mybatis/config.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
    <!-- dataSource链接池-->
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
      <!--driver 设置数据库驱动 -->
        <property name="driver" value="${driver}"/>
        <!-- 连接串 -->
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```
 
3、创建SqlSessionFactory对象
```
String resource = "db/mybatis/config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

4、通过接口和注解调用Mybatis进行数据库操作

```xml
<!-- 在配置文件中 进行配置后使用注解 class中填入接口全限定类名 或者别名 -->
<mapper class="my.package.com.MyClass"></mapper>
```

```java
interface UserMapper{
    @Select("select * from user")
    List<User> getUsers();
}
```

```
String resource = "db/mybatis/config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
try(SqlSession session = sqlSessionFactory.openSession()){
    UserMapper mapper = session.getMapper(UserMapper.Class)
}
```
原理：
- 使用代理模式通过反射和注解创建了一个UserMapper实现
## 配置日志
[配置MyBatis日志]https://mybatis.org/mybatis-3/zh/logging.html
```xml
  <settings>
    <setting name="logImpl" value="LOG4J"/>
  </settings>
```
配置日志Maven依赖
```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>

```

创建log4j.properties文件
```properties
# 全局日志配置 rootlogger日志等级 DEBUG
log4j.rootLogger=ERROR, stdout
# MyBatis 日志配置
log4j.logger.org.mybatis.example.BlogMapper=TRACE
# 控制台输出
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

首先配置日志框架，提高问题排查效率

配置数据源

Mapper:接口由MyBatis动态代理(注解方式)
- 优点：方便
- 缺点：SQL复杂的时候不方便

Mapper:用xml编写复杂的SQL
- 优点：可以方便使用MyBatis的强大功能
- 缺点：SQL和代码分离

创建Mapper.xml文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```
调用Mapper.xml中的sql语句
```
String resource = "db/mybatis/config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
try(SqlSession session = sqlSessionFactory.openSession()){
    UserMapper mapper = session.slectList(配置文件路径和sql语句名字)
}
```
MyBatis参数

parameterType
- 参数的#{}和${}
    - _#{}以sql方式替换参数 防止sql注入 
    - ${}直接替换对应的参数
- 参数是按照JavaBean约定读取的

resultType
- typeAlias
    - $区分内部类的分割符
    - 设置别名（类似类的引用）
- 写参照是按照JavaBean约定的

-- #{id}查找传入对象对应为id的属性（该对象遵循JavaBean约定）可以用Map取代临时的对象
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```
Association

Map与key = 属性，value=值

## MyBatis动态SQL

通过传入的参数动态决定执行的sql语句
```xml
<select id="findActiveBlogWithTitleLike"
     resultType="Blog">
  SELECT * FROM BLOG
  WHERE state = ‘ACTIVE’
  <if test="title != null">
    AND title like #{title}
  </if>
</select>
```
foreach语句
```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")">
        #{item}
  </foreach>
</select>
```




