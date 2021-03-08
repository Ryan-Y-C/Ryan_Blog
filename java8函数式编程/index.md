# Java8函数式编程


<!--more-->

# Java 8 函数式编程

### 什么是函数式编程

函数式：将一个对象映射成另一个对象

jdk工具类：predicate接口

- 实现predicate接口，传入一个对象映射成（返回）boolean值。

- 即使不实现predicate接口（满足predicate条件会自动转换成predicate实现），只要方法满足接受一个参照返回一个boolean值就可以使用函数式编程和lambda表达式。

- 使用方法引用减少代码量 类名::静态方法名 （将静态方法转换成函数接口）
    - 方法引用可以通过静态方法名解释该操作的行为。

- 实现predicate 对象—>boolean
    - 1、lambda表达式
    - 2、静态方法
    - 3、实例方法 默认参数是（object this）返回boolean

lambda表达式 （参数）-> 操作语句
- 代码简洁，但是不易理解。
- 超过两行不建议使用lambda表达式

### java函数接口

创建函数接口：任何只包含一个抽象方法的接口都可以被自动转换为函数接口
jdk函数接口：
- Consumer<T>：处理（消耗）某个对象。

- BiConsumer<T,U>:同时消耗两个对象。

- Function<T,R>：将一个类型T映射（变换）成另一个类型R。

- BiFunction<T,U,R> :将两个对象映射成一个对象。

- Supplier<T>:T get（）生产对象。

- BooleanSupplier：创建boolean值

### 函数式编程Comparator

使用Comparator创建TreeSet排序规则

数据丢失问题，Set认为重复数据会去除重复数据。


