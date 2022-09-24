---
title: ThreadLocal
summary: ThreadLocal
categories: 线程
tags:
- 线程
- java
---

## ThreadLocal
### 每个线程操作自己的本地内存


> 线程操作共享变量
```java
    public static void main(String[] args) {
        p.name = "zhangsan";
        // 创建一个线程，输出 Person 的 name
        new Thread(() -> {
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (Exception e) {
                e.printStackTrace();
            }
            System.out.println(p.name);

        }).start();

        // 创建一个线程，修改 Person 的 name
        new Thread(() -> {
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (Exception e) {
                e.printStackTrace();
            }
            p.name = "lisi";
        }).start();
        System.out.println(p.name + " main");
    }
```

> 每个线程操作自己的本地内存
```java
    static ThreadLocal<Person> threadLocal = new ThreadLocal<>();

    public static void main(String[] args) {


        Thread t1 = new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(3);
            }catch (Exception e){
                e.printStackTrace();
            }
            System.out.println("t1 " + threadLocal.get());

        });
        t1.start();

        Thread t2  = new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(1);
            }catch (Exception e){
                e.printStackTrace();
            }
            threadLocal.set(new Person("mike"));
            System.out.println(threadLocal.get().name);
        });
        t2.start();
        System.out.println("main " + threadLocal.get());

    }
```

### ThreadLocal底层
```java
        // 当前线程的 map
        public void set(T value) {
            Thread t = Thread.currentThread();
            ThreadLocalMap map = getMap(t);
            if (map != null)
                // 当前线程的map 存在
                // 往map存值  当前线程对象  ， value
                map.set(this, value);
            else
                // 当前线程的map 不存在， 创建当前线程的map。 并往map 存值
                createMap(t, value);
        }
```













