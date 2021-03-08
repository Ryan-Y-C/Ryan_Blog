# 多线程


<!--more-->

### 1 多线程与并发原理

- Java 的执行模型是同步/阻塞（block）的
- 默认情况下只有一个线程
    - 处理问题非常自然
    - 但具有严重的性能问题
### 2 Thread
-  Java中只有这么一种东西代表线程
- start方法才能并发执行
- 没多开一个线程，就多一个执行流
- 方法栈（局部变量）是线程私有的
- 静态变量/类变量是被所有线程共享的
### 3 多线程带来的性能的提升
对于IO密集型应用极其有用
- 网络IO（通常包括数据库）
- 文件IO
对于密集型应用稍有折扣

性能提升上线，单核CPU100

- 为什么需要多线程？
- 多线程带来了什么问题，如何避免？
- 线程的属性、状态、生命周期详解
- 什么是ThreadLocal？
- 为什么需要线程池？
- 线程池的构造函数中的参数都是什么含义？

线程安全
- 原子性
- 共享变量
- 默认的实现几乎都是线程不安全的

线程不安全的表现
数据错误
- i++
- if-then-do

hashMap线程不安全原因：“https://blog.csdn.net/luxia_24/article/details/52344367”//
“https://mp.weixin.qq.com/s?src=3&timestamp=1587039264&ver=1&signature=6oFg-q-hiaeFxUciDdYlmAEh5vUmrF5V-wlMFqRLzDV*FYd6yL4pdWokFybdsFj0NoxuZuOIgnY1Mwv-3FLVI2V61gJruVhB1aRnaZIBGp5Vz9Pbt9JTIh-BWFSS4HY9Yclh-xi8nJ8eCan6LeDn8gv6M2w0cCoMauf-XnmdqOg=”
hashMap 死循环问题

死锁例子：
- 有四个上锁的门A1、B1、A2、B2,A1中放着B1，A2中放着B2。钥匙一可以打开A1,A2,钥匙二可以打开B1,B2。
- 小明拿着钥匙一打开了A1的门,必须使用钥匙二打开B1的门才可以出去，而钥匙二小红打开了B2的门，需要钥匙一才可以打开A2的门。
- 等待对方归还钥匙才能出去。
死锁原因：
- 多个线程相互持有对方资源，促使单独一方没有获取全部资源使程序无法继续执行下去。

死锁问题排查
- 1、找到死锁进程ID
    - jps：列出Java进程命令。
- 2、jstack 进程ID。
    - 打印当前进程所有信息
- 3、 看方法栈（stack）停留的位置。看Java的提示。
多线程的经典问题：哲学家用餐问题
synchronized同步
预防死锁产生的原则：
- 释放资源
- 所有的线程都按照相同的顺序所得资源的锁

### 线程安全
实现线程安全的基本手段
- 不可变类
    - Interger/String/...
- synchronized 同步块
- 同步块是什么东西？
    - synchronized指定一个对象把这个对象当成锁
    - static synchronized 方法
        - 把class对象当成锁
        - 等价于下面代码
```
    - 实例的synchronized方法把该实例当成实例锁
synchronized(this){

} 
```    
- Collection.synchronized（线程同步工具方法）
 所有的集合线程都不是安全的，比如 list map link hashMap treeSet
 实现线程安全的基本手段
 - JUC工具包
    - AtomicInteger/..（原子级）
        - i++不是原子的
        - cup从内存中取i
        - +1
        - 把结果写回内存
    - ConcurrentHashMap(同步HashMap)
        - 任何使用HashMap有线程安全问题的地方，都可以使用ConcurrentHashMap替换即可。
    - ReentrantLock（可重入锁）
        - 一个地方加锁，在另一个地方解锁
### Object类里的线程方法
线程的历史
- Java从一开始就把线程作为语言的特性，提供语言级的支持
为什么java中所有的对象都可以成为锁？
- Object.wait()/notify()/notifyAll()方法

### 多线程的经典问题_生产者/消费者模型

使用三种方法来解决它
- wait()/notify/notifyAll
- Lock/Condition
- BlockingQueue

### 线程池与Callable/Future
什么是线程池
Executors(线程)
- 线程是昂贵的（Java线程模型的缺陷）
    - Java线程与系统中的线程进行绑定
- 线程池是预先定义好的若干个线程
- Java中的线程池
Callable/Future
- 类比Runnable,Callable可以返回值，抛出异常
- Future代表一个“未来才会返回的结果”
