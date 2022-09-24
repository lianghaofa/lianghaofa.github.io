---
title: Spring MVC 概述
date: 2022-05-21 09:40:52
summary: Spring MVC 概述
categories: Spring MVC
tags:
- Spring MVC   

---
## Spring MVC 概述

Spring MVC 是 Spring 框架的一个模块


用户请求服务 -> 服务器 开启一个 servelt 进行处理 -> 处理完生成HTML页面返回给用户


处理完生成HTML页面返回给用户

- 直接在 servelt 中写 HTML 代码，然后返回前端
  
- servelt 处理后，把业务返回给 jsp， jsp处理相关的渲染，生成 HTML 返回前端

- servelt 处理后 ，把业务返回 thymeleaf 处理， 生成 HTML 返回前端



继承 Tomcat 相关的 Servelt， 来处理浏览器的请求。


Spring MVC 对 Servelt 的封装，让配置更简单。


![Spring MVC 处理方式](/medias/Spring/MVC/1654057064.png)



DispatcherServlet  相当于  web.xml


DispatcherServlet 统一管理 

![Spring MVC 处理流程](/medias/Spring/MVC/1654134421.png)



![Spring MVC 处理流程](/medias/Spring/MVC/1654134539.png)












