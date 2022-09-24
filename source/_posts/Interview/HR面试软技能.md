---
title: HR面试软技能
date: 2022-05-22 10:24:52
summary: 会话管理 概述
categories: 会话管理 概述
tags:


---
## HR面试软技能


介绍自己

- 专业能力

- 经验 - 行业

- 个性的积极部分，分享技术，提升团队能力

- 解决问题的能力。追求技术

5年工作经验，从基层技术开发做到学校、企业的项目架构负责人，做过创业技术合伙人。负责团队人才培养,帮助团队人员负责解决技术问题，同时帮助团队人员提升个人能力与未来的发展方向。

负责学校导师的多个项目的0到1的搭建。


为何换工作？

- 创业公司的技术负责人

- 毕业去大公司发展，找挑战。
  
- 导师退休，项目少。

1W5

2W5


期望薪资：30K

前端

- 多年的前端开发经验，熟悉前端开发

后端

- 熟悉掌握JavaSE 和 JavaEE 相关知识，多年一线研发经验,具备良好的编码能力，并熟练应用设计模式;

- Go语言开发，具有重构Go项目的经验;

- 多年一线开发经验，熟练使用SSM等java主流框架，熟悉其原理，并看过核心源码。

- 熟悉JVM,多线程，GC回收，性能调优经验；

- 熟练使用MYSQL,NOSQL等原理，MYSQL调优；

- 分布式原理 zookeeper,redis,kafka,分布式事务；

- Elastic Search

- Tomcat,Nginx服务器；

- 熟悉Linux命令及其开发，了解Linux IO；

- 了解云原生、devOps,熟悉Docker,K8s.

- 熟悉BIO,IO及Netty, RPC服务器框架。

- 熟悉数据结构，ACM校队；

- 熟悉Git、Maven等项目管理及构建工具，熟悉微服务中基于Jenkins的CI/CD,熟悉容器化服务构建及运维，熟练使用grafana + Prometheus

- 多年的开发经验；

- 精通数据结构。。。

精通redis,MYSQL,Kafka,Zookeeper.


至少是熟悉，了解就不要写了。

技术栈先宽后深，技术栈尽量多，精通。




- 项目重构

- 新项目的沟通，业务谈判

带团队，0到1


个人概况

掌握的技能

工作经历 - 倒叙

项目经验？



回答技巧

总 当前问题回答的是哪里具体的点

分 以1，2，3，4.。。。分点描述。(突出一些技术名词，核心概念，接口，类，关键方法)

### spring面试题


- 谈谈对 Spring IOC 的理解，原理与实现

控制反转：

理论思想，原来的对象是由使用者进行控制，有了spring后，可以把整个对象交给spring来进行管理。
DI:依赖注入，把对应的属性的值注入到具体的对象中，@Autowired, popolateBean 完成属性值的注入。

容器：存储对象，使用map结构存储，在spring中一般存在三级缓存。
singletonObject存放完整的bean，整个bean的生命周期，从创建到使用到销毁的过程都是由容器进行管理。

三级缓存？

map ？

1.一般聊IOC容器的时候要涉及到容器的创建过程，beanFactory,DefaultListableBeanFactory,
向Bean工厂中设置一些参数(BeanPostProcess,Aware接口的子类)等等属性

2.加载解析Bean对象，准备要创建的Bean对象的自定义对象BeanDefinition(XML或注解的解析过程)

3.beanFactoryPostProcess的处理，此处是扩展点。PlaceHolderConfigurationSupport,ConfigurationClassProcessor

4.BeanPostProcessor的注册功能，方便后续对Bean对象完成具体的扩展功能

5.通过反射的方式将BeanDefinition对象实例化成具体的bean对象

6.bean对象的初始化过程(填充属性，调用aware子类的方法，调用BeanPostProcessor的前置方法，调用init-method方法，
调用BeanPostProcessor的后置处理方法方法)

7.生成完整的bean对象，通过getBean方法直接获取

8.销毁过程

这是我对IOC的整体理解，包含一些处理过程，您觉得有没什么问题？可以指点一下。


具体的细节我记不太清楚，但是spring的bean都是通过反射的方式生成的，同时其中包含了很多的扩展点，

比如最常用的对BeanFactory的扩展，对Bean的扩展，我们在公司对这方面的使用比较多的，

除此之外，IOC中最核心的耶就是填充具体bean的属性和生命周期。


- 谈一下 spring IOC 的底层实现

反射，工厂，设计模式，关键的几个方法

createBeanFactory ,getBean ,doGetBean ,createBean ,doCreateBean ,createBeanInstance ,populateBean

1.先通过createBeanFactory创建出一个Bean工厂(DeafultListBeanFactory)

2.开始循环创建对象，因为容器中的bean默认都是单例的，所以优先通过getBean,doGetBean从容器中查找

找不到的话，通过createBean,doCreateBean方法，以反射的方式创建对象，一般情况下使用的是无参的构造方法(getDeclaredConstructor,newInstance)

3 进行对象的属性填充populateBean

4.进行其他的初始化操作，initializingBean


- 描述一下bean的生命周期


![bean的生命周期](/medias/Spring/1661170850312.png)

对图中的内容进行适当扩展描述

1.实例化bean，反射的方式生成对象

2.填充bean的属性，populateBean()循环依赖问题(三级缓存)

3.调用aware接口相关的方法，invokeAwareMethod() 完成BeanName,BeanFactory,BeanClassLoader对象的属性设置

4.调用BeanPostProcess中的前置处理方法，使用较多的有(ApplicationContextPostProcess,设置ApplicationContextEnvironment,ResourceLoader,EmbedValueResolver等对象)

5.调用initMethod方法，invokeinitmethod...判断是否实现了initializingBean接口，如果有，调用afterPropertiesSet方法，否则不调用。

6.调用BeanPostProcess的后置处理方法:spring的Aop就是在此处实现的，AbstractAutoProxyCreator

7.注册Destruction相关的回调接口，钩子函数？

8.获取到完整的对象，可以通过getBean的方法来获取对象

9.销毁流程，1判断是否实现了DispoableBean接口，2调用destoryMethod方法

- Spring 是如何解决循环依赖的问题的？


三级缓存，提前暴露对象，AOP

总：什么是循环依赖问题，A依赖B，B依赖A

分：先说Bean的创建过程，实例化与初始化


创建，实例化，初始化
![循环依赖](/medias/Spring/1661173712514.png)



1.先创建A对象，实例化A对象，此时A对象的B属性为null

2.初始化A对象，从容器中查找B对象，如果找到，直接赋值。不存在循环依赖问题。

找不到，创建B对象，实例化B对象，初始化的发现A已经存在了，对B对象中的A属性进行赋值。

spring 把未完成初始化的对象允许提前被赋值。解决问题的核心在于实例化与初始化分开操作，这是解决循环依赖的关键。

当所有的对象都完成初始化和初始化操作完成后，再把完整对象放到容器中

- 完成实例化，但未完成初始化   二级缓存

- 完成初始化，并完成初始化  一级缓存

使用不同的map进行存储，此时就有了一级缓存与二级缓存

 为什么还需要三级缓存 ？ 三级缓存value类型是ObjectFactory是一个函数式接口，存在的意义是保证在整个容器的运行过程中同名Bean对象只能有一个版本的对象。

如果一个对象需要被代理，或者说需要生成代理对象，那么要不要优先生成一个普通对象？ 要

普通对象和代理对象不能同时出现在容器中，因此当一个对象需要被代理，就要使用代理对象覆盖掉之前的普通对象。

实际的调用过程中，是没有办法确定什么时候对象被使用。所有就要求某个对象被调用的时候，优先判断此对象是否需要被代理，
类似于一种回调机制的实现，因此传入lambda表达式的时候，可以通过lambda表达式来执行对象的覆盖过程，getEarlyBeanReference()

 因此，所有的Bean对象在创建的时候优先放到三级缓存中，在后续的使用过程中，如果需要被代理则返回代理对象，如果不需要被代理，
则直接返回普通对象

- 缓存的放置时间和删除时间

三级缓存createBeanInstance 之后，addSingletonFactory

二级缓存 第一次从三级缓存确认对象是代理对象还是普通对象的时候，同时对应的三级缓存 getSingleton

一级缓存，生成完整对象之后放到一级缓存，删除对应的二三级缓存 addSingleton


- BeanFactory 与 FactoryBean 有什么区别

相同点：都是创建Bean对象

不同点：使用BeanFactory创建对象的时候，必须要遵循严格的生命周期流程，太复杂了。如果想要简单的自定义某个对象的创建，
同时创建完成的对象想交给spring来管理，那么就需要实现 FactoryBean 接口。isSingleton,getObjectType 获取对象的类型
getObject自定义创建对象的过程(new,反射，动态代理)

- Spring中用到的设计模式

单例模式：bean默认都是单例

原型模式：指定作用域为prototype

工厂模式：BeanFactory

模板方法：PostProcessBeanFactory,OnRefresh,InitPropertyValue

策略模式：XMLBeanDefinitionReader，PropertiesBeanDefinitionReader

观察者模式：Listener,event,multicast

适配器模式：Adapter

装饰者模式：BeanWrapper

责任链模式：使用aop的时候会先生成一个拦截器

代理模式：动态代理

委托者模式：delegate

- Spring的AOP的底层实现原理

动态代理

AOP是IOC的一个扩展功能，现有的IOC，再有AOP，只是在IOC的整个流程中新增的一个扩展点而已。BeanPostProcessor

总：AOP概念，应用场景，动态代理

分：Bean的创建过程中有一个步骤可以对Bean进行扩展实现，AOP本身就是一个扩展功能，所以在BeanPostProcessor的后置处理方法中进行实现

1.代理对象的创建过程(advice,切面，切点)

2.通过jdk，或者cglib的方式来生成代理对象

3.在执行方法调用的时候，会生成字节码文件中，直接会找到DynamicAdvisoredInterceptor类中的intercept方法，方法开始执行

4.按照之前定义好的通知来生成拦截器链

5.从拦截器链中依次获取每一个通知开始进行执行，在执行过程中，为了方便找到下一个通知是哪个，
会有一个InvocationInterceptor的对象，找的时候是从-1的位置依次开始查找并且执行的。

- Spring的事务是如何回滚的？

Spring的事务是如何实现的？

总：Spring的事务是由AOP来实现的，首先要生成具体的代理对象，然后按照AOP的整套流程来执行具体的操作逻辑，
正常情况下要通过通知来完成核心功能，但是在AOP中事务不是通过通知来实现的，而是通过一个TransactionInterceptor来实现的，
然后调用invoke来实现具体的逻辑。

分：1先准备工作，解析各个方法上事务相关的属性，根据具体的属性来判断是否开启新事务
2当需要开启的时候，获取数据库连接，关闭自动提交功能，开启事务
3执行具体的SQL逻辑操作
4在操作过程中，如果执行失败，那么通过 completeTransactionAfterThrowing 来看事务的回滚操作，回滚的具体逻辑是通过doRollBack方法来实现的，
实现的时候也是要先获取连接对象，通过连接对象来回滚
5如果执行过程中，没有任何意外情况发生，那么通过 commitTransactionAfterRunning 来完成事务的提交操作，提交的具体逻辑是通过doCommit方法来实现的，
实现的时候也是需要获取连接，通过连接对象来提交
6当事务执行完毕之后需要清除相关事务信息 cleanupTransactionInfo

具体细节 TransactionInfo 与 TransactionStatus 对象

- Spring事务的传播

7种传播特性

Required,Requires_new,Nested,Support,Never,Mand

某一个事务嵌套另一个事务的时候，怎么处理？

A方法调用B方法，AB方法都有事务，并且传播特性不同，那么A如果有异常，B怎么办，B如果有异常，A怎么办？

总：事务的传播特性指的是不同方法的嵌套调用过程中，事务应该如何进行处理，是用同一个事务还是不同的事务，当出现异常的时候会回滚还是提交，
两个方法之间的影响，在日常工作中，使用比较多的是Required,Requires_new,Nested.


分
1先说事务的不同分类，可以分成三类，支持当前事务，不支持当前事务，嵌套事务

2如果外层方法是Required，内层方法是 Requires_new,Nested

3如果外层方法是Requires_new，内层方法是 Required，Requires_new,Nested

4如果外层方法是Nested，内层方法是 Required，Requires_new,Nested

核心处理逻辑，判断内外方法是否是同一事务，如果是同一事务，异常同一在外层方法处理，如果不是，内层方法可能影响到外层方法，但是外层方法不会影响到内层方法的。(但是个别情况不同，nested)

Spring的隔离级别就是数据库的隔离级别

- Redis的应用场景

5大Value类型；

基本是缓存，不会用于存储；

- redis是单线程还是多线程？

1.无论什么版本，工作线程就是一个；
2. 6.x高版本出现了IO多线程。
3.IO模型，取数据，发数据交给IO线程，工作线程负责计算
   
客户端被读取的顺序不能被保障，只能保障同一连接或sock里的执行顺序

- redis存在线程安全的问题吗？为什么？

线程安全？ 单线程？ 不能能保证不同连接的执行顺序，只能保障同一连接的顺序，同一连接下是否线程安全，只能取决客户处理的逻辑
   
- 缓存穿透，详细描述

单次

- 缓存击穿

多次

- 缓存雪崩

有效请求，

Canal binlog

不允许时间差

mysql主从同步使用binlog，Canal 模拟binlog，使 redis 与 mysql 同步。

允许有时间差

MQ

- 简述一下redis主从不一致的问题

1.redis默认是弱一致性，异步的同步
2.锁不能用(单实例/分片集群/redlock) ==> redission
3.在配置中提供了必须有多少个client连接同步，可以配置同步因子，趋向于强一致性

- redis 持久化原理


异步后台进程完成持久化

fork + copyofwrite

1.RDB,AOF,主从同步也算持久化
2.高版本开启AOF，AOF是可以通知执行日志得到全部内存数据的方法，但是追求性能；
2.1体积变大，重复无效指令->重写，后台线程把内存的kv生成指令写个新的AOF
2.2 4.x新增的模式， 把内存的kv生成指令写个新的AOF 变为 RDB 快照

- 为什么使用setnx?

1.原子性操作
2.如果要做分布式锁，就要set k v nx ex (不存在，过期时间，避免死锁)


Linux 与 IO

- 云原生

CI/CD 

如何检测 nginx 高并发？ 连接数

java基础相关面试题

- HashMap 为什么要使用红黑树？

Array + 链表(红黑树) 。链表长度大于8，改为红黑树。长度为6退化成链表？


JUC高并发类
ConcurrentHashMap "锁分段"机制
