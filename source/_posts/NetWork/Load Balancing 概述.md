---
title: Load Balancing 概述
date: 2022-07-18 09:25:00
author: Darren Leung
hide: true
cover: false
coverImg: /images/1.jpg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
mathjax: false
summary: 计算机网络，keepalive技术
tags:
---


## keepalive


最前端如何配置 ？ 如何处理单点故障


lvs 单点故障问题

VIP 如何对应多台服务器。  主备 主主 

主备模式

方向性 - 备机怎么知道主机宕机了？ 备机定时轮询主机。 主机广播给备机

效率性 - 

Real Server 故障问题

keepalive 应用程序

- 主机 keepalive 应用程序 监听本机上的服务，向备机通告自己还活着。

- 备机 keepalive 应用程序 监听本机上的服务，监听主机的状态。

- 对后端Server进行健康检查









域名解析处？ 

一个域名对多个IP， 多个IP作为最前端的负载均衡。


- 应用层的负载均衡，Nginx，

- 传输控制层的负载均衡，不需要握手，只负责转发，不需要应用层。



传输控制层的负载均衡负责分发流量，分发到下一跳，应用层的负载均衡处理握手请求，然后交由具体的服务器处理业务。



NAT 模型 - 不能知道转发到客户端

![NAT 模型](/medias/NetWork/1658155197501.png)


![DR 模型](/medias/NetWork/1658157110793.png)





