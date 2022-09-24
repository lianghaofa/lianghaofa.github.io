---
title: ClassLoader 概述
summary: ClassLoader
categories: ClassLoader 概述
date: 2022-07-09 09:25:00
tags:

---
## ClassLoader 概述



### 类加载器和双亲委派机制


#### 引导类加载器

负责加载支撑JVM运行的位于JRE的lib目录下的核心类库，比如 rt.jar, charset.jar 等

#### 扩展类加载器

负责加载支撑JVM运行的位于JRE的lib目录下的ext扩展类库


#### 应用程序类加载器  AppClassLoader


负责加载ClassPath路径下的类包


#### 自定义加载器

负责加载用户自定义路径下的类包



跟路径无关？  只跟 com.tju.user 有关 ？

![Minor GC](/medias/Java/JVM/1657284496753.png)



### 打破双亲委派机制

Tomcat 打破双亲委派机制


不要委托父亲去加载

重新 loadClass









