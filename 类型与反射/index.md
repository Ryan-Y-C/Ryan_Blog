# 类型与反射


<!--more-->

# 类型与反射
## java的类与Class

RTTI(Run-Time TypeIdentification) 运行时类型识别

一个Class对象就是一个类的说明书

- JVM根据这个说明书创建这个类的实例
- 静态变量的本质

object类的getClass方法可以提供该对象的具体的类

复习(本质时调用getClass后知道自己的类型)
- instanceof
- 强制类型转换 

## Class对象的生命周期

在第一次使用时被加载

      ||     链接         ||
加载->||验证->准备->解析->||初始化     =>类加载过程

查看类加载过程命令 verbose:class

过滤命令grep

## Class与ClassLoader

Classloader负责从外部系统中加载一个类
- 这个类对应的Java文件并不需要存在
- 这个类（字节码）并不一定需要存在
    - 文件的本质是字节流，所以可以网络加载字节码

Classloader的双亲委派加载模型

java语言规范与java虚拟机规范
- Java Language Specification JLS
- Java virtual Machine Specification 
- 这种分离提供了在JVM上运行其他语言的可能

## 反射与Spring反射

反射
- 动态创建对象
- 动态调用方法
- 动态获得属性

