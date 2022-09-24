---
title: Spring Cloud 概述
date: 2022-06-16 09:40:52
summary: Spring Cloud 概述
categories: Spring Cloud
tags:
- Spring Cloud   

---
## Spring Cloud 概述


流量接入层


业务网关

Spring Cloud  

多级缓存

流量网关

Nginx 集群如何配置？

网络带宽 

Nginx + Lua



### 负载均衡

硬件级别


软件级别

LVS + KeepAlive


### 服务注册与发现

#### Eureka

注册

1. 互相注册，互相交换信息，可以不需要强一致性。  只需要向一个 Eureka 注册 

2. 互相独立 ， 需要像多个 Eureka 注册 


发现

1. 需要与多台 Eureka 发现


2. 只需要与相关的 Eureka 发现


宁可保留健康的和不健康的，也不盲目注销任何健康的服务。



### 负载均衡




如何处理大用户？  比如： 访问某个明星的主页



#### Ribbon

ZoneAvoidanceRule 区域权衡策略

BestAvailableRule 最低并发策略

RoundRobining 轮询策略



### 降级

#### Hystrix


限流、熔断



线程池，线程用完了，怎么办？


### 服务治理


### 线程隔离与信号量隔离

Spring Cloud 


线程隔离
1.异常隔离

信号量隔离 - 单线程的问题
1.响应时间超短


### 网关

### 流量网关




### 业务网关

- 代理、隧道 方式，分发服务

隧道 - 还是需要中转

![隧道](/medias/Server/1655870270906.png)

好处 - 可以在中转处处理业务逻辑


![路由](/medias/Server/1655871585119.png)


网关可以做缓存



### Tomcat 

每次请求都是重量级的，需要 https 连接， 3次握手。 



LVS











