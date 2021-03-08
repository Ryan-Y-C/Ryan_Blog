# 数据库浅谈


<!--more-->
# 数据库浅谈

数据库提供结构化数据的持久化存储

索引保证数据查询的速度

事务原子性保证数据不丢失
- 多件事情要么不发生，要么都发生
## 数据库实战

Schema：设计自己的第一个数据库
- 数据库的类型与SQL语句
- 行与列
- 数据的外键
JDBC简介
- Java Database Connection
- 通过连接数据库字符串，读取数据库信息
H2简介
使用JDBC存储数据

### DDL-SQL

create table 建表语句

drop table 删表语句

alter table 修表语句

### 基本SQL

insert into 增 
- 例：insert into user(name ,password,tel , avatar,created, updated_at) values('zhangsan','1111','111111','http:asd.jpg',now(),now())
delete from 删
- 例：delete from user where id=2;
- 物理删除，数据从磁盘删除掉
- 逻辑删除。数据还在磁盘， 
- 删除id为2的用户
```sql
update user set status=0 UPDATED_AT=now() where id=2
```


update 改
alter table user add status tinyint not null default 1
### select 查

select *

select count(*)count(1)

select max /min/avg

select limit 分页

select order by 排序

select is null/is not null

create table order(id,name,user,update,update_at,created_at)

select * from user limit <从第几个元素查找>,<最多返回多少个元素>  =》分页

group by ：按照一个列分组
```sql
select ADDRESS,count(*) as count from user group by ADDRESS 
```

join 将多个表联合起来 
Lift join 外连接
inner join 内连接
```sql
select "ORDER".id ,"ORDER".USER_ID,"ORDER".GOODS_ID,GOODS.NAME
    from "ORDER"
        inner join GOODS
            on "ORDER".GOODS_ID = GOODS.id

```

distinct:去掉重复得元素

```sql
select * from user where 
    id in 
(
    select USER_ID
    from "ORDER" where GOODS = 1 
)

```



### 数据库表的设计原则

每个实体一张表（用户/商品）

- 每个实体都有一个主键 
- 按照业务需要创建索引

每个关系用一张表联系


SQL语句不区分大小写

数据区分大小写 

命名风格是 下划线区分两个单词

数据库中字符串是单引号的

数据库中的注释是 - -

分号分隔多个SQL语句

### 使用JDBC连接数据库

JDBC连接
- 连接串
- 用户名
- 密码

Statement

PrepareStatement 防止SQL注入

ResultSet
## Sql注入
没验证传入的参数，使攻击者使用sql语句进行攻击
JDBC防范入侵
