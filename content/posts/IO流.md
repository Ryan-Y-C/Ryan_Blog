---
title: "IO流"
date: 2017-02-17T18:10:15+08:00
lastmod: 2017-02-17T18:10:15+08:00
draft: true
description: "IO流"
show_in_homepage: true
show_description: false
license: ''

tags: ['学习笔记']
categories: ['学习笔记']

featured_image: ''
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
math: false
---

<!--more-->
# IO流
## 1 计算机多级体系原理简介

### File与IO
一切文件的本质
- 一段字节流：
    - 文本文件
    - 二进制文件
- 每个程序负责解释文件中的字节流
## 2 InputStream/OutputStream
抽象的输入输出操作
- 从文件中读取字节流
- 从网络里读取字节流

## 3 Java中的File 
- File并不代表一个“文件”，它只代表一个路径。
- 抽象的“文件”：文件或者文件夹。
File的常见方法   
- 绝对路径和相对路径
- 读/写文件
- NIO的Path
    - 旧版本的File
## 4   BufferedReader/ Writer
    -
在内存中创建好一次写入
## 5 工具
FileUtils
IOUtils
