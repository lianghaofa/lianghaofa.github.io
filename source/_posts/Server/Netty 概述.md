---
title: Netty 概述
date: 2022-05-21 09:40:52
summary: Tomcat 概述
categories: 服务器
tags:
- Netty   

---
## Netty 概述

Netty 是一个异步事件驱动的网络应用 框架，快速开发可维护的高性能协议服务器和客户端。

Netty 是一个NIO客户端服务器框架。

支持多种协议。

![Netty - 架构](/medias/Server/components.png)


Netty is a NIO client server framework which enables quick and easy development of network applications such as protocol servers and clients. 

It greatly simplifies and streamlines network programming such as TCP and UDP socket server.

'Quick and easy' doesn't mean that a resulting application will suffer from a maintainability or a performance issue. Netty has been designed carefully with the experiences earned from the implementation of a lot of protocols such as FTP, SMTP, HTTP, and various binary and text-based legacy protocols. As a result, Netty has succeeded to find a way to achieve ease of development, performance, stability, and flexibility without a compromise.


互联网三高 : 高可用，高性能，高并发

访问量大

业务多变

响应事件快

### NIO

1. Channel(接口)、 Buffer(抽象类)、Selector(抽象类)

抽象类 能够有具体实现。

### 传统 IO

InputStream 读取方式:一个字节一个字节读

OutputStream 写方式:一个字节一个字节写

InPutStream 与 OutPutStream 调用 OS 提供的接口。


![InputStream / OutputStream ](/medias/Server/1653358379.png)

**直接读取磁盘数据 ？？？**


写数据 - 写 OS 内存，让 OS 直接 写磁盘。

读数据 - 读 OS 内存，等待 OS 读取完磁盘。

BufferedInputStream ， 系统提前向缓冲区写数据， BufferedInputStream 读取缓冲区的数据。

一旦缓冲区数据被读完了，  调用 InputStream 写数据。

引入缓存区
![BufferedInputStream / BufferedOnputStream](/medias/Server/1653360282.png)


BufferedInputStream 读取缓冲区数据，



普通IO模型
输入对象，输出对象。面向字节数据，字节流。

![普通IO模型](/medias/Server/1653354779.png)







### NIO模型

传输数据不再创建输入对象与输出对象。    构建Channel通道，将数据抽象为Buffer，在Channel中传输Buffer。
![NIO模型](/medias/Server/1653354834.png)


Channel - JVM 与 OS 沟通 通道

Buffer -  Channel 以 Buffer 传输数据包 


创建对象，会有对象的引用，复制对象的引用，存放对象的引用，为何需要复制？ 为了invokespecial,astore需要消耗也能用？
创建对象时，需要创建一片空间，


直接内存与堆内存

- 堆内存 - java的堆内存
- 直接内存 - Java内存中的缓冲区
  
![直接内存与堆内存](/medias/Server/1653361975.png)

创建对象，会有对象的引用，复制对象的引用，存放对象的引用，为何需要复制？ 为了invokespecial,astore需要消耗也能用？
创建对象时，需要创建一片空间，

![Channel](/medias/Server/1653443486.png)


ChannelPipeline

在InboundHandler 与 OutboundHandler 中进行事件处理，例如 Http 编码与解码, 统计连接信息等

 
ByteBuffer 满了才能读取 ？ 

每次 get都需要check 边界

Netty 只需要检查一次。


GC 是在堆内存不足时运行， 如何回收堆外内存？ 

java9 - 可指定， 统计使用直接内存的大小。  java 回收堆外内存时静态同步的，对高并发环境下效率非常低。



Netty 操作自身定义的 ByteBuffer 会有counter统计，当counter为0，Netty cleaner 将进行释放内存。 

- Memory Pool 因为释放内存代价太大，释放内存的放进 Memory Pool。 不会遵循 java 定义的 直接内存大小。


Netty 有不同级别的内存溢出检查器。


Netty 默认都是使用直接内存


Memory Pool 如何有合适大小的内存，直接使用。

如果没有合适大小的内存 - Memory Pool 如何处理？    Jemalloc - 论文  https://zhuanlan.zhihu.com/p/48957114




一个Channel 绑定到 一个 IO-Thread， 绑定关系不改变。  



Netty 4 每次调用 write 不会进入到 socket ，需要调用 flush 才写入 socket。


JNI


### 零拷贝


零拷贝 需要上下文

DDD 

DCI(Data Context Interaction)

- 不同的环境有不同的实现


MVC  

M - model

V - view

C - controller 

NIO

直接写OS内存，不需要切换


HeapByteBuffer


DirectByteBuffer


Reactor 设计模型

01 : Netty 3


### 直接内存管理

直接内存管理 - JVM的finalizer 或 cleaner 实现。 JVM 难以回收 直接内存

JIT 


游戏服务器需要自定义编码与解码方式。


![Netty - 主从](/medias/Server/1654574549716.jpg)


Boss Group 负责监听

Work Group 负责处理接收数据



channel  bytebuffer selector


bytebuffer -> bytebuf [pool]

bytebuf 堆内堆外


pool 与 unpooled 的区别

pool 都池里获取，unpooled每次重新new



客户端


事件驱动，

注册(需要监听的端口)


Netty pipeline，  注册的事件(端口接收到的事件，(读取操作))


自己定义读取方式，编码，加密等等。















