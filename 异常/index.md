# 异常


<!--more-->

## 1 Java的异常体系简介

在return语句之外，为方法提供另外的一种出口

IOException通常代表“预期之内的异常”

## 2 try/catch/finally
如果没有try,异常将击穿所有栈帧

catch可以将一个异常抓住

finally执行清理工作

JDK7+:try-with-resources
## 3 throw/throws

throw 抛出一个异常
- 可以被丢出来的异常和错误

throws只是一个声明

### Java的异常体系
Throwable-可以被抛出的东西（有毒）
- Exception-checked exception(受检异常，有毒)
   - 意料之中的异常,IOException
    - RuntimeException(运行时异常，无毒)
- Error (错误，有毒)
    - 代表一种不正常的情况，内存溢出
catch的级联与合并
- 处理异常
## 异常的栈轨迹
### Throwable
栈轨迹Stacktrace(排查问题最重要的信息)
异常链（Caused by）
## 异常抛出的原则
1 能用if/else处理的，不使用异常
2 尽早抛出异常
3 异常要准确、带有详细信息
4 抛出异常也比悄悄的执行错误的逻辑强多
### 异常的处理原则
本方法是否有责任处理这个异常？
- 不要处理不归自己管的异常
本方法是否有能力处理这个异常？
- 如果自己无法处理，就抛出
如非必要，不要忽略异常
### JDK常见异常
NullPointerException(空指针异常)

ClassNotFoundException(不存在类的异常)/NoClassDeFoundError

IllegalStateException（不正常的状态）

IllegalArgumentException（不正常的参数）

IllegalAccessException（非法访问）

ClassCastException
