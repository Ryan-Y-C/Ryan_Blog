# Spring


<!--more-->
# Spring IoC容器原理与手写简单实现
## Spring 容器的原理
### Spring
java世界应用的事实标准

Spring容器-一个IoC容器
- 自动化管理对象的容器

Spring MVC-基于Spring和Servlet的Web应用框架

Spring Boot - 集成度和自动化程度更高

### Spring 容器的核心概念
Bean
- 容器中的最小工作单元，通常为一个Java对象
BeanFactory/ApplicationContext
- 容器本身对应的Java对象
依赖注入（DI）Dependence Injection 
- 容器负责注入所有的依赖
控制反转（IoC）Inverse of Control
- 用户将控制权交给了容器

### 简单实现Spring
定义Bean

加载Bean的定义

实例化Bean

查找依赖，实现自动化注入

### Spring的实现

在XML里面定义Bean

BeanDefinition的载入和解析

Bean的实例化和依赖注入

对外提供服务

[简单实现ioc容器]https://github.com/hcsp/simple-ioc-container/pull/81
