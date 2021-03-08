---
title: "Stream"
date: 2017-08-07T19:47:35+08:00
lastmod: 2017-08-07T19:47:35+08:00
draft: true
description: ""
show_in_homepage: true
show_description: false
license: ''

tags: [Stream]
categories: [Stream]

featured_image: ''
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
math: false
---

<!--more-->
# java 8 Stream
### 创建流
- 1、Collection.stream()
- 2、Stream.of
- 3、String.chars()
    - 生成字符流
- 4、IntSteam.range()

### Steam终结操作
- 返回非"Stream"的操作
- 一个流只能被消费一次
- forEach
- count/max/min
- findFirst/FindAny
- anyMatch/noneMatch
- collect

options（函数式）：当作返回值进行函数式操作

### Collector与Collectors

### 并发流（parallelStream）
