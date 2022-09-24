---
title: Spring 概述
date: 2022-05-21 09:40:52
summary: Spring 概述
categories: Spring
tags:
- Spring   

---
## Spring 概述


课程03 ： java 零基础后端

java 框架，21 done

### IOC  控制反转




DI dependency injection. 


控制管理对象，容器中生成一大堆对象。

直接 new ，正向

反向， 容器帮忙生成。


目的，解耦 ？ 

- 对象的创建，容器启动的时候就默认创建了。 默认单例，容器启动时创建。

- 赋值属性，必须要有 get, set 方法。

- 作用域， singleton - 默认容器启动时创建, prototype - 每次获取都新创建,request - 每个请求创建一个对象, session - 每个session创建一个对象，默认每个session 30分钟



创建bean对象的方式

- 反射

- 工厂模式



### AOP  面向切面


AOP 类似于断点的插入，在某个方法或某类方法插入某些代码。


AOP 面向切面编程

#### jdk 的实现

```java

public interface Calculator {

    public Integer add(Integer i, Integer j) throws NoSuchMethodError;

    public Integer sub(Integer i, Integer j) throws NoSuchMethodError;

    public Integer mul(Integer i, Integer j) throws NoSuchMethodError;

    public Integer div(Integer i, Integer j) throws NoSuchMethodError;
}

public class MyCalculator implements Calculator {

    @Override
    public Integer add(Integer i, Integer j) throws NoSuchMethodError {
        return i + j;
    }

    @Override
    public Integer sub(Integer i, Integer j) throws NoSuchMethodError {
        return i - j;
    }

    @Override
    public Integer mul(Integer i, Integer j) throws NoSuchMethodError {
        return i * j;
    }

    @Override
    public Integer div(Integer i, Integer j) throws NoSuchMethodError {
        return i / j;
    }
}

public class CalculatorProxy {

    public static Calculator getCalculator(final Calculator calculator){

        // 获取被代理对象的类加载器
        ClassLoader loader = calculator.getClass().getClassLoader();

        // 被代理对象的所有接口
        Class<?>[] interfaces = calculator.getClass().getInterfaces();

        // 用来执行被代理类需要执行的方法
        InvocationHandler handler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                // 开始调用被代理类的方法
                
                //  如果 method.getName() 是某种类型，就进行某种操作
                Object result = null;
                try {
                    LogUtils.start(method, args);
                    result = method.invoke(calculator, args);
                    LogUtils.end(method, args);
                }catch (Exception e){
                    LogUtils.exception(method, args);
                }finally {
                    LogUtils.fin(method, args);
                }
                return result;
            }
        };

        Object o = Proxy.newProxyInstance(loader, interfaces, handler);

        return (Calculator) o;
    }

}

public class LogUtils {


    public  static void start(Method method, Object ... objects){
        System.out.println(method.getName() + " - start - parameters" + Arrays.asList(objects));
    }

    public  static void end(Method method, Object ... objects){
        System.out.println(method.getName() + " - end - parameters" + Arrays.asList(objects));
    }

    public  static void exception(Method method, Object ... objects){
        System.out.println(method.getName() + " - exception - parameters" + Arrays.asList(objects));
    }

    public  static void fin(Method method, Object ... objects){
        System.out.println(method.getName() + " - fin - parameters" + Arrays.asList(objects));
    }
    
}


    
public class Test {

    public static void main(String[] args) {

        
        // jdk,reflect包下的功能  传入某个类，这个类实现某个接口

        Calculator calculator = CalculatorProxy.getCalculator(new MyCalculator());

        calculator.add(1, 2);

        // cglib 无需实现特定的接口

        // spring 使用两种动态代理的方法，  jdk,reflect包 和 cglib
        
    }

}
```

java.lang.reflect 动态代理的实现

- JVM有不同的ClassLoader，需要找到你想要执行的方法，必须先要对应的 ClassLoader 和 类。

- 获取对应的 ClassLoader
ClassLoader loader = calculator.getClass().getClassLoader();
  
- 获取对应的类 Class
 Class<?> cl = getProxyClass0(loader, intfs);
  
遍历所有的接口

生成代理类，字节码文件，加载到内存，然后创建对象。


生成代理类实现了对应的接口，且toString, equal, hashCode


找到 ClassLoader 后，需要找指定的方法。JDK interfaces ，为什么 ？ 必须要 实现某些接口，否则无法调用

回凋方法，执行对应的方法前后，需要执行某些业务。


Proxy.newProxyInstance(loader, interfaces, handler)

handler 监听方法的执行。


#### cglib 动态代理的实现



生成代理类，字节码文件，加载到内存，然后创建对象。



spring 的实现

切面 

拦截器


运行时切入。


AOP 的切入点

- Before advice 方法执行之前

- After returning advice  方法执行之后

- After throwing advice   返回结果之后

- After (finally) advice  出现异常的调用

- Around Advice - 监听 Before advice， After returning advice， After throwing advice， After (finally) advice 的执行 


正常执行流程

1.Around 前置 Advice - 2.Before advice - 3.Around 后置 Advice - 4.Around 返回 Advice - 5.After - 6.After returning advice 

异常执行流程

1.Around 前置 Advice - 2.Before advice - 3.Around 异常 Advice - 4.Around 返回 Advice - 5.After - 6.After returning advice


多个 Around Advice 可以指定执行顺序 Order(*) 



AOP 的应用场景

- 日志管理

- 权限认证

- 安全检查

- 事务控制


1.需要确定哪些文件需要扫描

2.切入点表达式 
- 精确匹配

#### 通配符 *  
  
- 一个或者多个字，例如匹配某个类下的所有方法
  
- 匹配任意类型的参数
  .
  

OOP 面向对象编程

spring 的底层实现


动态代理


### 编程式事务

try{
beginTransaction;
// SQL operate
commit;
}catch(){
rollback;
}finally{
close;
}

beginTransaction, commit, rollback

### 声明式事务

AOP 实现编程式事务

节省 try{

}catch(){

}

让AOP来实现


@Transactional(readonly= true,timeout=3000)

propagation 传播特性 - 不同事务之间的执行的关系

- propagation = Propagation.REQUIRED  Transactional( (Transactional1 、 Transactional2 、 Transactional3 ...) ) 形成一个大的事务，一个事务出现异常，整体回滚


- propagation = Propagation.REQUIRES_NEW  开启新的事务，挂起原有的事务  Transactional( (Transactional1 、 Transactional2 、 Transactional3 ...) )  
  挂起事务，一旦后来的的事务发生异常，挂起的事务将回滚(如果后来的事务 propagation = Propagation.REQUIRES_NEW，将不会回滚)。
  重点在于 propagation = Propagation.REQUIRES_NEW 默认把异常处理了，没有往外抛。 只要不是我内部事务出问题，我能顺利执行完，我的事务就不能回滚。
  

- propagation = Propagation.NESTED 嵌套  

**重点外层事务是否影响到内层事务，Propagation.NESTED 依赖外部，外部事务发生异常，内部事务需要回滚。 Propagation.REQUIRES_NEW 不依赖外部事务，外部事务发生异常，只要内部事务正常运行，当前的内部事务不会回滚** 


- propagation = Propagation.SUPPORTS 如果有事务，运行在事务中

- propagation = Propagation.NOT_SUPPORTED 有事务，也不会运行在事务中  。
  有事务，指的是  Transactional(  Transactional1(propagation = Propagation.NOT_SUPPORTED) , Transactional2(propagation = Propagation.NOT_SUPPORTED))
  Transactional 事务包括了Transactional1，Transactional2.   Transactional 去掉后指 Transactional1，没有在事务中

- propagation = Propagation.NEVER  外部不可有事务控制， 如果 Transactional(  Transactional1(propagation = Propagation.NEVER) ) 将会报错，propagation = Propagation.NOT_SUPPORTED不会报错。

- propagation = Propagation.MANDATORY  外部必须有事务控制， 与 propagation = Propagation.NEVER 相反。





isolation 隔离级别，4种隔离级别 。 参考 MySQL 数据库


- isolation = Isolation.DEFAULT 使用数据库默认的隔离级别

- isolation = Isolation.READ_COMMITTED  

- isolation = Isolation.READ_UNCOMMITTED

- isolation = Isolation.REPEATABLE_READ

- isolation = Isolation.SERIALIZABLE

timeout 超时时间 - 执行超过指定时间，回滚操作

readonly 只读事务， 不可写操作。 防止外部外部事件的修改，到底锁在哪个级别 ？   表，记录？

noRollbackfor ： 哪些类出现异常不会回滚。是指 Transactional 修饰的方法种出现了该类

noRollbackforClassName ： 跟 noRollbackfor 类似，只是配置方式不一样

rollbackfor  ： 哪些类出现异常会回滚

rollbackforClassName ： 跟 rollbackfor 类似，只是配置方式不一样


### 分布式事务

XA ？ 二阶段提交，三阶段提交



### spring 源码


1.读取配置文件

2. 创建 BeanFactory 



XML

注解

其他










