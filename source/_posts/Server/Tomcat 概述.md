---
title: Tomcat 概述
date: 2022-04-25 19:40:52
summary: Tomcat 概述
categories: 服务器
tags:
- Tomcat   

---
## Tomcat 概述

Tomcat 是一个实现了部分J2EE规范的服务器，用于承载Servet。

Jakarta EE(Eclipse基金会维护) 是 J2EE 的延续。

Tomcat9及之前实现J2EE规范，Tomcat9之后是Jakarta EE规范。


动态服务器 - 动态的生成HTML页面
 
静态服务器 - 直接返回HTML页面

CATALINA_HOME -  Tomcat的核心 - 只读：jar包，bin目录二进制文件

CATALINA_BASE - 配置文件的目录，可以配置一台机器多个实例。 - 可变

只读：jar包，bin目录二进制文件，可变：配置，多个Tomcat实例


/bin - Tomcat执行脚本目录 

/conf - Tomcat配置文件

/logs - Tomcat执行时的LOG文件

/webapps - Tomcat的主要Web发布目录

/work - jsp最终要转变为servlet,而servlet是一个java类，类是需要编译处理的，work目录保存jsp生成的servlet文件。


![Tomcat 架构](/medias/Server/1653058477.png)

Server - 启动与关闭服务器

Service - 由一个或多个连接器Connector组成

Connector - 接收外部的连接

Engine - 管理 Host

Host - 管理1个或多个 Context(web 应用)

Context - 管理1个或多个 Wrapper

Wrapper - 管理1个或多个 Servlet

Servlet - 处理请求的类


默认配置  ${catalina.base}/conf/web.xml

Context - 每个web应用都有自己的 web.xml ，覆盖  web.xml

WEB-INF/web.xml

![Tomcat 架构](/medias/Server/1653058477.png)
1653060452.png

浏览器 ->  Apache/Nginx（负载均衡） -> Tomcat

Apache -> AJP(专门用于 Apache 与 Tomcat 连接) / (Http/Https) -> Tomcat

Nginx -> Http/Https 连接 Tomcat

浏览器   Http/Https -> Tomcat


JNDI , 操作

Listener

Host 组件

Cluster - 集群组件，共享 session



Context 组件

- Context.Manager -  session 管理

- web.Filter -> 决定哪个 Servlet 处理

- Context.Loader -> 每个Context(服务器)都有自己的加载器，以免加载的类发生冲突。



每个组件都有Listener, 监听每个组件的 生命周期 


JSP解析成Servlet


![执行流程](/medias/Server/1655822263873.png)






