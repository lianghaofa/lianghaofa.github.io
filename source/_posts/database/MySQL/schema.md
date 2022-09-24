---
title: schema
date: 2022-05-04 20:45:52
summary: 数据类型优化
categories: 数据库
tags:
- MySQL
---
## MySQL 查询优化

system > const > ref >  range > index > all
### all
select * from

### index
select id from

### range
select id from between 10 and 100

### index_subquery 利用索引关联子查询
select * from emp where emp.job in (select job from t_job) 

### unique_subquery 
select * from emp e where e.deptno in (select distinct deptno from t_job)

### ref_or_null
select * from emp e where e.mgr is null or e.mgr = 100

### ref


### const
select * from emp where id = 100




