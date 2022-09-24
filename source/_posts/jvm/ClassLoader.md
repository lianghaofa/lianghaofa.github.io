---
title: 类加载与初始化
summary: class文件格式解析
categories: JVM
date: 2022-04-26 09:25:00
tags:
- java
---

## 类加载与初始化


![类加载与初始化](/medias/Java/JVM/1663640770496.png)

加载，链接(验证、准备、解析)、初始化、使用、卸载

```java
    static {
        System.out.println(123);
    }
    public static void main(String[] args) {
        System.out.println(1234);
    }
```
static {

}代码块在初始化时执行。

### compile
**混合模式**
解释器 + 热点代码编译
起始阶段采用解释执行

**热点代码检测**
 多次被调用的方法 (方法计数器)
 多次被调用的循环 (循环计数器，检测循环执行频率)
 进行编译

-Xmixed 混合模式
-Xint   解释模式，启动快，执行慢。
-Xcomp  编译模式，启动慢，执行快。


### Loading 

```java
    public static void main(String[] args) {
        // Bootstrap.getParent() == null
        // Extension.getParent() == null
        // App.getParent() == Extension
        // CustomClassLoader.getParent() == App

        // App
        System.out.println(NullClass.class.getClassLoader());
        // null
        System.out.println(NullClass.class.getClassLoader().getClass().getClassLoader());
        // Ext
        System.out.println(NullClass.class.getClassLoader().getParent());
        // null
        System.out.println(NullClass.class.getClassLoader().getParent().getParent());
    }
```

**自定义类加载器**
```java
public class MyClassLoader extends ClassLoader {
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        File f = new File("D:/test/","myclass");
        try {
            FileInputStream fis = new FileInputStream(f);
            ByteArrayOutputStream bs = new ByteArrayOutputStream();
            int b = 0;
            while ( (b = fis.read()) != 0){
                bs.write(b);
            }
            byte[] bytes = bs.toByteArray();
            bs.close();
            fis.close();
            return defineClass(name, bytes, 0, bytes.length);
        }catch (Exception e){
            e.printStackTrace();
        }
        return super.findClass(name);
    }
}
```



### Linking

#### Verification

验证文件是否符合JVM规范

#### Preparation

静态成员变量赋默认值


#### Resolution

将类、方法、属性等符号引用解析为直接引用

常量池中的各种符号引用解析为指针、偏移量等内存地址的直接引用






### Initialzing



调用类初始化代码。

Bootstrap ClassLoader
加载路径 sun.boot.class.path
   

ExtensionClassLoader
加载路径 java.ext.dirs 

AppClassLoader 
加载路径  java.class.path   




















