---
title: Spring Boot 概述
date: 2022-06-16 09:40:52
summary: Spring Boot 概述
categories: Spring Boot
tags:
- Spring Boot   

---
## Spring Boot 概述

spring 的启动点 ？ 

减少 spring 的配置


WebMvcProperties.class - web文件配置相关属性，css,js

封装 spring mvc

ResourceProperties.class - 资源文件配置相关属性


Web应用程序的类型 - 默认 Servelet

Reactive

Null

Servelet


SpringFactoriesInstance 


通过反射进行实例化。

Spring Boot 初始化步骤

1.配置resourceloader

2.判断当前应用程序的类型

3.获取初始化器的实例对象

4.获取监听类的实例对象

5.找到当前应用程序的主类，开始执行run()。

6.装配 命令行参数

7.准备应用程序运行的环境



加载实例都是通过反射进行实例化。

反射比直接new对象更复杂，效率更低，为何还需要反射？ 反射能够在运行的时候再生成对应类的实例，经常在框架中使用，因为框架只提供大的流程，具体操作的行为需要用户指定。


run()


1.设置启动时间，应用上下文，异常报告，awt(java-ui)

2.SpringApplicationRunListener   ApplicationStartingEvent  11个监听器

3. listeners.starting()   各种初始化 验证,jackson,字符集  



自动装配



@Configuration 

解析 @Configuration  注解的类

ConfigurationClassParser  














