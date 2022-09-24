---
title: Reference
summary: 四大引用
categories: JVM
date: 2022-04-26 09:25:00
tags:
- 引用
- java
---

## Reference

### finalize()方法
> Called by the garbage collector on an object when garbage collection determines that there are no more references to the object.
> 明确为null，GC才会回收

**垃圾回收可能调用此方法**
``` java
static class Test_Gc{

        // 垃圾回收可能调用此方法
        @Override
        protected void finalize() throws Throwable {
            System.out.println("finalize ");
        }
    }
```
**finalize()方法被调用**
``` java
    public static void main(String[] args) throws IOException {

        // 强引用
        Test_Gc test_gc = new Test_Gc();
        // 断开引用，test_gc 指向null
        test_gc = null;

        // 调用GC回收
        System.gc();

        // 阻塞主线程，防止程序停止。
        System.in.read();
    }
```

**finalize()方法没被调用**
``` java
    public static void main(String[] args) {
        WeakReference<Test_GC> t = new WeakReference<>(new Test_GC());

        System.out.println(t.get());

        // 垃圾回收
        System.gc();

        System.out.println(t.get());

    }
``` 
### 强引用



``` java
    public static void main(String[] args) throws IOException {

        // 强引用
        Test_Gc test_gc = new Test_Gc();
        // 断开引用，test_gc 指向null
        test_gc = null;

        // 调用GC回收
        System.gc();

        // 阻塞主线程，防止程序停止。
        System.in.read();
    } 
```


### 软引用


设置堆内存
**-Xms20M -Xmx20M**

``` java
    public static void main(String[] args) throws IOException {

        // 软引用，内存够用，不回收。  内存不够用，回收。
        // 10 M
        SoftReference<byte[]> m = new SoftReference<>(new byte[1024 * 1024 * 10]);
        System.out.println(m.get());
        System.gc();
        try {
            Thread.sleep(500);
        }catch (Exception e){
            e.printStackTrace();
        }
        // 软引用，内存够用，不回收
        System.out.println(m.get());
        
        byte[] b = new byte[1024 * 1024 * 10];
        // 内存不够用，回收。
        System.out.println(m.get());
        // 阻塞主线程，防止程序停止。
        //System.in.read();
    }
```
软引用缓存图片等耗内存的资源。


### 弱引用
GC回收直接回收弱引用

``` java
    public static void main(String[] args) {
        WeakReference<Test_GC> t = new WeakReference<>(new Test_GC());

        System.out.println(t.get());

        // 垃圾回收
        System.gc();

        System.out.println(t.get());

        ThreadLocal<Test_GC> tl = new ThreadLocal<>();
        tl.set(new Test_GC());

        tl.remove();
    }
```

``` java
public void remove() {
      ThreadLocal.ThreadLocalMap m = getMap(Thread.currentThread());
         if (m != null)
            m.remove(this);
}
```

![弱引用](https://isblog.oss-cn-guangzhou.aliyuncs.com/Weak.jpg)

### 虚引用


![虚引用](https://isblog.oss-cn-guangzhou.aliyuncs.com/JVM/PhantomReference.jpg)

private static final List<Object> LIST = new LinkedList<>();
private static final ReferenceQueue<Test_GC> QUEUE = new ReferenceQueue<>();

``` java
    public static void main(String[] args) {

        PhantomReference<Test_GC> phantomReference = new PhantomReference<>(new Test_GC(), QUEUE);

        new Thread(()->{
            while (true){
                LIST.add(new byte[1024 * 1024]);
                try {
                    Thread.sleep(1000);
                }catch (Exception e){
                    e.printStackTrace();
                    Thread.currentThread().interrupt();
                }
                System.out.println(phantomReference.get());
            }
        }).start();

        new Thread(()->{
            while (true){
                Reference<? extends Test_GC> poll = QUEUE.poll();
                if (poll != null){
                    System.out.println("虚引用被回收 " + poll);
                }

            }
        }).start();

    }
```







