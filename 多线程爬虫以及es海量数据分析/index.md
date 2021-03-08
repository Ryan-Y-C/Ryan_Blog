# 多线程爬虫以及ES海量数据分析


<!--more-->
# 多线程爬虫以及ES海量数据分析

### 做项目原则

- 使用标准化、业界公认的模式和流程
    - 代码仓库中不要有多余的文件
- 几乎没有本地依赖，使用者能毫无障碍的运行
- 提交PR时候应该有相应的的信息   
- 小步快跑
 - 尽量分成小的模块进行提交
 - 越小的变更越容易修正问题
 
### 多线程爬虫以及ES海量数据分析项目的原则

- 使用GitHub+主干/分支模型进行开发
    - 禁止直接push master
    - 所有变更通过PR进行
- 自动化代码质量检查+测试
    - Checkstyle/SpotBugs
    - 最基本的自动化测试覆盖
- 一切工作自动化
- 规范化提交流程

### 创建项目骨架

- 新建GitHub仓库
- 建立新项目
    - mvn archetype（创建项目骨架）
        - 通过mvn archetype:generate命令生成项目骨架
    - IDEA - new
        - 通过IDEA创建项目骨架
- .gitignore
- README
- 配置基本的代码质量检查检查插件
    - 越早代价越低

### 项目的设计流程

#### 自顶向下
- 多人协作
- 多模块
    - 各模块间职责明确，界限清晰
    - 基本的文档
    -基本的接口
- 小步提交

#### 自底向上
- 先实现功能
- 将公用代码抽离出来
- 通过重构实现模块化、接口化
### 创建CI
 创建测试类（冒烟测试）
    - 用注解方式（@Test）创建test方法

引入测试类相关maven依赖

```xml
<dependencys>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-api</artifactId>
    <version>5.6.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.6.0</version>
    <scope>test</scope>
</dependency>
</dependencys>

```
```xml
<plugins>
    <plugin>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.22.1</version>
        <configuration>
            <argLine>-Dfile.encoding=UTF-8</argLine>
        </configuration>
    </plugin>
        <plugin>
            <artifactId>maven-checkstyle-plugin</artifactId>
            <version>3.1.0</version>
            <configuration>
                <configLocation>${basedir}/.circleci/checkstyle.xml</configLocation>
                <includeTestSourceDirectory>true</includeTestSourceDirectory>
                <enableRulesSummary>false</enableRulesSummary>
            </configuration>
            <executions>
                <execution>
                    <id>compile</id>
                    <phase>compile</phase>
                    <goals>
                        <goal>check</goal>
                    </goals>
                </execution>
            </executions>
            <dependencies>
                <dependency>
                    <groupId>com.puppycrawl.tools</groupId>
                    <artifactId>checkstyle</artifactId>
                    <version>8.29</version>
                </dependency>
            </dependencies>
        </plugin>
</plugins>
```
- 如果不小心提交了代码使用git reset HEAD~1返回上一次提交前的操作
    - reset->重启
    - commit 
```
git reset HEAD~1
```
如果push到master上，使用git revert，使用git long 查看提交的id后 使用 git revert id 撤回提交

如果不小心commit并且push了，如果在主干上则删除多余的提交文件，如果在自己的分支上则已同样的方式并且force push。


#### 创建分支

通过git checkout -b 分支名称 创建分支 

#### 查看git状态

git status

### 算法

#### 深度优先

#### 广度优先

优先访问一个层次内容

常用队列实现

JDK的队列实现

#### 程序流程

从池中拿出一个链接，如果是没有处理过并且是想要处理的链接就进行处理，把新链接放入池中，如果是新闻链接就存储内容。将处理完的链接删除

Arraylist从尾部删除元素更有效率

### 处理页面流程

1. 通过httpClient发送http请求并获取http响应.
2.通过jsoup获取html内容
通过Jsoup.parse(html);解析html
~~~xml
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.13.1</version>
</dependency>
~~~
#### 抓取新闻中出现的问题
1. 重复抓取 "https://sina.cn"
- 因为没有吧链接池中移除已经处理的链接
2. 重复抓取“https://sina.cn/?reload=sina”
- 因为处理完成后没有将处理完成的链接加入处理完成的链接池
3.弹出新浪的登陆页
- 因为没有设置user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.113 Safari/537.
4 因为链接头为//开始的所以需要加上https
#### 重构代码

1. 将复杂的逻辑提取成短小的方法。
2. 使用stream使代码更加简洁。

#### Maven生命周期与插件配置

自动检查工具设置。

- spotbugs(findbugs)
    - 引入maven依赖
```xml
<plugin>
  <groupId>com.github.spotbugs</groupId>
  <artifactId>spotbugs-maven-plugin</artifactId>
  <version>4.0.0</version>
  <dependencies>
    <!-- overwrite dependency on spotbugs if you want to specify the version of spotbugs -->
    <dependency>
      <groupId>com.github.spotbugs</groupId>
      <artifactId>spotbugs</artifactId>
      <version>4.0.4</version>
    </dependency>
  </dependencies>
</plugin>
```
- 使用mvn spotbugs:check命令
- 使用mvn spotbugs:gui命令以图形化方式提示错误

- 如何阅读和使用官方文档
- maven生命周期（maven lifecycle）
- 通过插件(plugin)绑定到某个maven阶段进行检查

#### 数据持久化
创建数据表
- links_to_be_processed
- links_already_processed
- news
    - news表属性
        - id 
        - title
        - content
        - url
        - created_at
        - modified_at
```h2
create table news
(
    id          bigint primary key AUTO_INCREMENT,
    title       text,
    content     text,
    url         varchar(200),
    created_at  timestamp,
    modified_at timestamp
)
```
JDBC操作
- 获取加载驱动DriverManger并创建连接connection
- 通过连接并创建预处理语句
- 执行查询并获取结果

```
String url="jdbc:h2:file:J:\\Crawler\\news";
String sql="select link from links_to_be_processed";
Connection connection = DriverManager.getConnection(url);
PreparedStatement statement = connection.prepareStatement(sql);
ResultSet resultSet = statement.executeQuery();
```
#### 添加断点续存
- 使用数据库实时的存储数据
- 使用URLDecoder.decode进行字符转码。
```
String href = URLDecoder.decode(aTag.attr("href"), "UTF-8");~
```
#### 自动化数据库构建工具Flyway

Flyway 可以对数据库结构进行版本控制

添加Flyway依赖
```xml
<plugin>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-maven-plugin</artifactId>
    <version>6.4.4</version>
    <configuration>
    <url>jdbc:h2:file:J:\Crawler\news</url>
    <user>ryan</user>
    <password>123</password>
    </configuration>
</plugin>
```
创建文件夹目录并创建sql文件
```
 my-project
   src
     main
       resources
         db
            migration
                R__My_view.sql
                U1.1__Fix_indexes.sql
                U2__Add a new table.sql
                V1__Initial_version.sql
                V1.1__Fix_indexes.sql
                V2__Add a new table.sql
```
运行mvn flyway：migrate

#### ORM(Object Relation Mapping)

ORM框架MyBatis
- 添加MyBatisMaven依赖
```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.5</version>
</dependency>
```
创建MyBatis 配置数据库连接配置文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
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
配置数据库配置文件操作
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
使用SqlSession对数据进行增删改查
```
try (SqlSession session = sqlSessionFactory.openSession()) {
  Blog blog = (Blog) session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);
}
```
#### 使用docker容器中的MySql

安装好docker后json配置加速镜像
```json
{
  "registry-mirrors": [
    "https://hub-mirror.c.163.com",
    "https://mirror.baidubce.com"
  ]
}
```
docker教程
```
https://yeasy.gitbook.io/docker_practice/
```
下载Mysql docker镜像
```
 docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```
使用jdbc链接mysql

-v参数使数据持久化（将数据存储到物理机的硬盘中）-v参数将docker容器中的数据映射到主机上

-p 参数将容器的端口映射到主机中

创建MySQL数据库
```
docker run --name mysql -e MYSQL_ROOT_PASSWORD=123 -p 3305:3306 -v J:\Crawler\mysql-data:/var/lib/mysql -d mysql:5.7.27 --lower_case_table_names=1
```
将MySql数据库修改成UT8编码
```mysql
ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
```
并设置jdbc写入数据库为utf-8格式
```
jdbc:mysql://localhost:3305/news?useUnicode=true;characterEncoding=UTF-8
```
