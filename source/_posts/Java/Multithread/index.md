---
title: 多线程
summary: 程序 进程 线程
categories: 多线程
tags:
- 线程
- java
---

如何预防死锁？

- 死锁的四个条件



活锁 - 线程一致重复执行某个相同的操作，并且一致失败重试



Java中的wait和sleep的区别与联系？

区别

sleep针对的是线程

wait是Obejct超类的方法

wait与锁相关的操作，会释放锁。而sleep是针对线程的，不会释放锁。


联系

wait是Obejct的方法，而线程是程序执行的基本单位，必然会涉及到线程。

两者都能中断与唤醒。中断与唤醒？


### 进程

### 线程

### 程序

QQ.EXE -> loaded in memory
多次开启QQ.EXE，一个QQ对应一个进程。可以限制一个程序一个进程。 资源分配的基本单位。 
程序开启进程后，启动一个主线程执行任务。 执行的基本单位-线程
![线程公式](https://user-images.githubusercontent.com/24481784/163958247-839490b4-a44d-4add-895a-7c3750a41406.png)

CPU密集型-线程数=CPU核数 + 1

IO密集型-线程数=CPU利用率尽可能高，内存占用率尽可能高




### Create threads in java

1.implements Runnable 接口

2.implements Callable 接口

3.extends Thread

4.FutureTask

5.Executors.newCachedThreadPool()

但最终都还是Runnable接口的run方法


``` java
static class MyThreads extends Thread {
    @Override 
    public void run() { 
        System.out.println("MyThreads"); 
    } 
}

static class MyRun implements Runnable{
    @Override
    public void run() {
        System.out.println("MyRun");
    }
}

static class MyCall implements Callable<String> {
    @Override
    public String call() throws Exception {
        System.out.println("MyCall");
        return "success";
    }
}

public static void main(String[] args) throws ExecutionException, InterruptedException {
    // 0
    new MyThreads().start();
    // 1
    new Thread(new MyRun()).start();
    // 2
    new Thread(()->{
        System.out.println("lam");
    }).start();
    
    // 3   FutureTask    Future Callable
    FutureTask<String> task = new FutureTask<String>(new MyCall());
    Thread t = new Thread(task);
    t.start();
    // success
    System.out.println("FutureTask<String> = " + task.get());

    // 4
    ExecutorService service = Executors.newCachedThreadPool();
    service.execute(()->{
        System.out.println("ExecutorService");
    });
    Future<String> f = service.submit(new MyCall());
    String s = f.get();
    // success
    System.out.println(s);
    service.shutdown();
}

```
**本质都是 new Thread()**
### Java的线程状态
NEW – a newly created thread that has not yet started the execution

RUNNABLE – either running or ready for execution but it's waiting for resource allocation

BLOCKED – waiting to acquire a monitor lock to enter or re-enter a synchronized block/method

WAITING – waiting for some other thread to perform a particular action without any time limit

TIMED_WAITING – waiting for some other thread to perform a specific action for a specified period

TERMINATED – has completed its execution
![java种6种线程状态](https://user-images.githubusercontent.com/24481784/163965686-74d57c46-c591-49c0-ba51-dc57d321ac77.jpg)
### How to stop a thread.
``` java
Thread t = new Thread(()->{
        while (true){
            System.out.println("go on");
        }
    });

    t.start();
    // not recommend.   it will release everything including lock,data inconsistent...it will influence others.
    t.stop();
    
    // not recommend.   it will not release everything ...it will cause dead lock ,data inconsistent  ?
    t.suspend();
    t.resume();


//  set a flag
//  volatile
private static volatile boolean running = true;

public static void main(String[] args) throws ExecutionException, InterruptedException {

    Thread t = new Thread(()->{
        while (running){

            System.out.println("go on");
        }
    });
    t.start();
    // but you don't when it will stop.
    running = false;
}
//  interrupt
public static void main(String[] args) throws ExecutionException, InterruptedException {

    Thread t = new Thread(()->{
        while (!Thread.interrupted()){

            System.out.println("go on");
        }
    });
    t.start();
    // but you don't when it will stop.
    t.interrupt();
}
```
