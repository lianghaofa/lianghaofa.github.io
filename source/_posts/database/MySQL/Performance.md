---
title: MySQL 性能检测 
date: 2022-05-04 19:45:52
summary: MySQL 性能检测
categories: 数据库
tags:
- MySQL
---
## MySQL 性能检测



[MySQL-官方文档](https://dev.mysql.com/doc/)


### 5.7 之后的版本

### Performance Schema

1. 实时检查
2. performance_schema 存储引擎
3. 不持久化
4. performance_schema 表


instruments：采集MySQL各种操作产生的时间信息

consumers：存储instruments采集的数据
### show processlist 查看连接的线程个数


## 5.7 之前的版本

### all 显示所有性能信息  show profile


### block io 显示io的操作次数 


### context witches 显示上下文切换次数，被动和主动

### IPC 显示发送和接受的消息数量

### Memory 

### page faults : 显示错误数量

###  source 显示源码中的函数名称与位置

### swaps 显示swap的次数

