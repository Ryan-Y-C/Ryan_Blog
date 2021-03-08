# 注解


<!--more-->

# 注解

## 什么是注解

Class
- Class是Java类的说明书
- 通过反射或阅读说明书创建类的实例

Annotation注解就是说明书(Class文件上携带信息)的一段信息/文本/标记
- 可以携带参数
- 可以在运行时被阅读

## 注解的写法
新建一个类的时候选择注解

元注解(可以在放在注解上面的注解)
- @Retention
    - 当前注解编译后保留等级（选项）
- @Target
    - 限制当前注解在什么位置可以使用(类/方法/属性) 
- @Documented
- @Inherited
- @Repeatable

## 注解属性
基本数据类型+String+类及其他们的数组

默认值

名为value的属性

## JDK的自带注解
@Deprecated //废弃的方法

@Override //子类覆盖父类方法

@SuppressWarning //忽略检查

@FunctionalInterface //函数式标记接口

## ByteBuddy
```xml
<dependency>
            <groupId>net.bytebuddy</groupId>
            <artifactId>byte-buddy</artifactId>
            <version>1.10.1</version>
            <scope>compile</scope>
</dependency>

```
## 基于注解的缓存装饰器

[基于注解的缓存装饰器](https://github.com/hcsp/annotation-based-cache-decorator/pull/88)

