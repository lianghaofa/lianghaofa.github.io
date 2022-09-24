---
title: class文件格式
summary: class文件格式解析
categories: class
date: 2022-04-26 09:25:00
tags:
- class
---

## class文件格式
### 编译 java文件

编译前
```java
    public class NullClass {
    
    }
```
编译后  **生成默认构造方法**
```java
    public class NullClass {
        public NullClass() {
        }
    }
```


16进制

![16进制格式](https://isblog.oss-cn-guangzhou.aliyuncs.com/classHEX.png)


CA FF BA BE 


只要符合class文件格式，jvm就能执行。


### 静态（static）方法















