# 字符串原理


<!--more-->

# Java字符串原理

不可变的字符串

字符串是最重要的引用类型

## 字符串的不可变性

如何保证字符串的不可变性
- String类是final类型的，因此是不可继承的
- 字符串是字符的容器，String =char[]
- 字符串数组是final的所有不能改变地址指向
为什么字符串是不可变的
- 安全：线程安全 ，存储安全
    - 线程安全
        - 因为字符串是不可变的所以所有线程只能读取字符串而不能更改，所以是线程安全的
    - 存储安全
        - hashCode:将对象映射成一个数字
        - hashCode值的约定：
            - 数值不可变性，决定了String对象不可变性
            - 两个hashCode相等的对象，是一个对象。
- 缺点每次修改的时候都需要重复创建新的对象

StringBuffer和StringBuilder
- StringBuilder 不保证线程安全 可变字符序列，速度快
- StringBuffer 线程安全的可变字符序列，相对速度慢

HashCode存储：将当前字符串的HashCode缓存起来

## 字符串与编码 
Unicode（占用四个字节）
- code point(码点）
byte order mark(头部不可见字符-字节顺序标记)

BMP(基本多元平面16进制的码点)

编码
- utf-8
    - 变长的编码方案 
- utf-16-java程序内部的存储UniCode的方法
    - 十进制转十六进制：常用的是2个字节，不常用是4个字节


解码
