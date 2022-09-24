---
title: JUC
summary: JUC同步锁
categories: 并发编程
date: 2021-19-07 09:25:00
tags:
- 并发
- JUC
---
## JUC同步锁 

### ReentrantLock

> synchronized 也是可重入锁,自动上锁与解锁
> ReentrantLock 手动上锁与手动解锁
> synchronized一旦尝试获取锁就会一直等待直到获取到锁
> 指定时间内尝试获取锁 lock.tryLock(5,TimeUnit.SECONDS)
```java
    static Lock lock = new ReentrantLock();

    public static void write0(Lock lock){
        lock.lock();
        // do something!
        lock.unlock();
    }


    void m1(){

        boolean locked = false;
        try {
            lock.lock();
            for (int i = 0; i < 3; i ++){
                TimeUnit.SECONDS.sleep(1);
                System.out.println("m1 ... = " + locked);
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }

    synchronized void m2() throws InterruptedException {
        for (int i = 0; i < 3; i ++){
            TimeUnit.SECONDS.sleep(1);
            System.out.println();
        }
    }


    public synchronized static void write1(Lock lock){
        // do something!
    }

    public static void main(String[] args) throws InterruptedException {
        // synchronized 也是可重入锁,自动上锁与解锁
        // ReentrantLock 手动上锁与手动解锁；
        // 尝试获取锁 lock.tryLock(5,TimeUnit.SECONDS)  synchronized一旦尝试获取锁就会一直等待直到获取到锁

        lock.tryLock(5,TimeUnit.SECONDS);

        // 组合1  lock.lock();    lock.unlock();
        lock.lock();

        lock.unlock();

        // 组合2  lock.lockInterruptibly();    lock.unlock();
        lock.lockInterruptibly(); // t.interrupt 可以打断锁

        lock.unlock();

        // 公平锁  检查队列，队列为空直接执行，队列不为空，等待。
        Lock lock = new ReentrantLock(true);

    }
```

### CyclicBarrier
> 累计线程

```java
        CyclicBarrier barrier0 = new CyclicBarrier(20);

        CyclicBarrier barrier1 = new CyclicBarrier(20,() -> System.out.println("enough!"));

        CyclicBarrier barrier2 = new CyclicBarrier(20, new Runnable() {
            @Override
            public void run() {
                System.out.println("enough!");
            }
        });

        for (int i = 0; i < 100; i ++){
            new Thread(()->{
                try {
                    barrier1.await();
                    System.out.println(Thread.currentThread().getName());
                }catch (Exception e){
                    e.printStackTrace();
                }
            }).start();
        }
```
### CountDownLatch
> latch.countDown(); 计数
```java
        Thread[] threads = new Thread[100];
        CountDownLatch latch = new CountDownLatch(threads.length);

        for (int i = 0; i < threads.length; i ++){
            threads[i] = new Thread(()->{
                int result = 0;
                for (int j = 0; j < 10000;j ++){
                    result += j;
                }
                latch.countDown();
            });
        }

        for (Thread t : threads){
            t.start();
        }

        try {
            // 直到 latch.countDown() 到 0; 99...98...97...96.......0
            latch.await();
            // latch.countDown() 到 0
            System.out.println("zero");
        }catch (Exception e){
            e.printStackTrace();
        }
        System.out.println("end");

```

### ReadWriteLock
> 共享锁  读锁
> 读线程，大家都可以读。
> 排他锁  写锁
> 写线程，其他线程不能操作。
```java
    static Lock lock = new ReentrantLock();
    private static int value;

    static ReadWriteLock readWriteLock = new ReentrantReadWriteLock();
    static Lock readLock = readWriteLock.readLock();
    static Lock writeLock = readWriteLock.writeLock();

    public static void read(Lock lock){
        try {
            lock.lock();
            Thread.sleep(1000);
            System.out.println("read end");
        }catch (Exception e){
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public static void write(Lock lock, int val){
        try {
            lock.lock();
            Thread.sleep(1000);
            value = val;
            System.out.println("write end");
        }catch (Exception e){
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public static void main(String[] args) throws InterruptedException {
        // 共享锁  读锁
        // 排他锁  写锁
        Runnable r0 = () -> read(lock);
        Runnable w0 = () -> write(lock, (int) (Math.random() * 10));

        for (int i = 0; i < 10; i ++){
            new Thread(r0).start();
        }
        for (int i = 0; i < 3; i ++){
            new Thread(w0).start();
        }
        
        // read 可以并发
        Runnable r1 = () -> read(readLock);
        Runnable w1 = () -> write(writeLock, (int) (Math.random() * 10));
        for (int i = 0; i < 10; i ++){
            new Thread(r1).start();
        }
        for (int i = 0; i < 3; i ++){
            new Thread(w1).start();
        }
    }
```

### Semaphore
> 最多只能 n个线程 同时执行。

```java
        // 最多只能 2个线程 同时执行
        Semaphore semaphore0 = new Semaphore(2);
        // 进入队列等待，最多只能 2个线程 同时执行
        Semaphore semaphore1 = new Semaphore(2,true);

        new Thread(()->{
            try {
                // blocking until one is available
                semaphore0.acquire();
                System.out.println("T1 running");
                Thread.sleep(200);
                System.out.println("T1 running");

            }catch (Exception e){

            }finally {
                semaphore0.release();
            }
        });
```

### Phaser
> 分阶段

![Phaser](https://isblog.oss-cn-guangzhou.aliyuncs.com/Phaser.jpg)
```java
public class Test_Threads {

    static ProcessPhaser phaser = new ProcessPhaser();

    static void threadSleep(int milli) {
        try {
            TimeUnit.MICROSECONDS.sleep(milli);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {

        // 最开始阶段的人数
        phaser.bulkRegister(6);

        for (int i = 1; i <= 5; i ++){
            new Thread(new Person("旅游团成员 " + i)).start();
        }

        new Thread(new Person("导游")).start();
    }

    static class ProcessPhaser extends Phaser {
        
        @Override
        protected boolean onAdvance(int phase, int registeredParties) {
            switch (phase) {
                case 0:
                    System.out.println("所有人都到齐了！" + registeredParties);
                    return false;
                case 1:
                    System.out.println("所有人观光结束！" + registeredParties);
                    return false;
                case 2:
                    System.out.println("所有人都离开了！" + registeredParties);
                    return true;
                default:
                    return true;
            }
        }
    }

    static class Person implements Runnable {

        String name;

        public Person(String name) {
            this.name = name;
        }

        public void arrive() {
            threadSleep((int) (Math.random() * 1000));
            System.out.println(name + " gather");
            // 到达，并等待
            phaser.arriveAndAwaitAdvance();
        }

        public void sighSeeing() {
            threadSleep((int) (Math.random() * 1000));
            System.out.println(name + " sighseeing");

            phaser.arriveAndAwaitAdvance();
        }

        public void leave() {
            threadSleep((int) (Math.random() * 1000));
            System.out.println(name + " gather");
            phaser.arriveAndAwaitAdvance();
        }
        
        @Override
        public void run() {
            arrive();
            sighSeeing();
            leave();
        }
    }
}
```
### Exchanger
> 线程间交换数据
![Exchanger](https://isblog.oss-cn-guangzhou.aliyuncs.com/exchanger.jpg)
```java
        new Thread(()->{
            String s = "t1";
            try {
                s = exchanger.exchange(s);
            }catch (Exception e){
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " " + s);
        }, "t1").start();

        new Thread(()->{
            String s = "t2";
            try {
                s = exchanger.exchange(s);
            }catch (Exception e){
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " " + s);
        }, "t2").start();
```

### LongAdder
![LongAdder](https://isblog.oss-cn-guangzhou.aliyuncs.com/longAdress.jpg)

### LockSupport

{% video <iframe width="560" height="315" src="https://isblog.oss-cn-guangzhou.aliyuncs.com/JAVA_THREADS/%E6%96%B0%E8%A7%86%E9%A2%91.mp4" frameborder="0" gesture="media" allow="encrypted-media" allowfullscreen></iframe> %}

```java
        // 1.park 阻塞线程
        // 2.unpark 唤醒线程
        Thread t = new Thread(()->{
            for (int i = 0; i < 8; i ++){
                System.out.println(i);
                if (i == 4){
                    // 阻塞 t 线程
                    LockSupport.park();
                }
                // t 线程 阻塞 1秒
                try {
                    TimeUnit.SECONDS.sleep(1);
                }catch (Exception e){
                    e.printStackTrace();
                }
            }
        });

        t.start();
        System.out.println("t线程开启");
        try {
            TimeUnit.SECONDS.sleep(8);
        }catch (Exception e){
            e.printStackTrace();
        }
        System.out.println("t线程开启 6秒之后");
        // 恢复t线程运行
        LockSupport.unpark(t);
```
