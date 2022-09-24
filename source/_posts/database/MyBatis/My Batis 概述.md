---
title: My Batis 概述
date: 2022-06-01 09:40:52
summary: My Batis 概述
categories: Spring MVC
tags:
- My Batis   

---
## My Batis 概述



- JDBC 操作数据库的接口

- DBUtils 封装了对JDBC的操作

- Hibernate ORM映射,对象映射关系，实体类对应表，  SSH， Spring-Structs-Hibernate。 
  1.完全封装，导致无法使用数据的一些功能；2.缓存问题 - ；3.耦合度过高-不好找bug

- JDBCTemplate 

My Batis

更简洁，方便 ？


SqlSessionFactory

- XML 方式创建

- 注解


- SqlSessionFactoryBuilder  这个类可以被实例化、使用和丢弃，一旦创建了 SqlSessionFactory，就不再需要它了。 因此 SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域（也就是局部方法变量）。 你可以重用 SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory 实例，但最好还是不要一直保留着它，以保证所有的 XML 解析资源可以被释放给更重要的事情。

- SqlSessionFactory SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例。 使用 SqlSessionFactory 的最佳实践是在应用运行期间不要重复创建多次，多次重建 SqlSessionFactory 被视为一种代码“坏习惯”。因此 SqlSessionFactory 的最佳作用域是应用作用域。 有很多方法可以做到，最简单的就是使用单例模式或者静态单例模式。

- SqlSession 每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。 绝对不能将 SqlSession 实例的引用放在一个类的静态域，甚至一个类的实例变量也不行。 也绝不能将 SqlSession 实例的引用放在任何类型的托管作用域中，比如 Servlet 框架中的 HttpSession。 如果你现在正在使用一种 Web 框架，考虑将 SqlSession 放在一个和 HTTP 请求相似的作用域中。 换句话说，每次收到 HTTP 请求，就可以打开一个 SqlSession，返回一个响应后，就关闭它。

MyBatis中存在两种缓存，即一级缓存和二级缓存

- 一级缓存也叫本地缓存，在MyBatis中，一级缓存是在会话(SqlSession)层面实现的，这就说明一级缓存作用范围只能在同一个SqlSession中，跨SqlSession是无效的。
  默认开启

- 二级缓存的作用域是namespace，也就是作用范围是同一个命名空间. 一般指的是 跨SqlSession
  默认开启
设置连接最大数 

各种缓存

MemoryCache 缓存 -> redis

![MyBatis 一级缓存](/medias/MySQL/MyBatis/1654142989.jpg)


![MyBatis 二级缓存](/medias/MySQL/MyBatis/1654143124.jpg)



只要是缓存，必然存在缓存通用的问题，目标在于如何进行取舍。  没有哪一个框架说，能够彻底解决问题。






My Batis Plus 只是对 My Batis 的扩展。










