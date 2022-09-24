---
title: Linux IO 概述
date: 2021-06-03 09:40:52
summary: Linux IO 概述
categories: Linux IO
tags:
- IO  

---
## Linux IO 概述

### VFS - 虚拟文件系统

树状

/ 
  /bin
  /boot
  /dev
  /etc
  ...


df - 查看 文件 挂载在哪个位置(哪块磁盘,哪个分区...)

同一目录下的文件，不一定在同一分区，设置可能不在同一磁盘。

文件类型
- | 普通文件(可执行，图像，文本)
- d : 目录
- b : 块设备
- l : 链接， 类似window 软链接
- c : 字符设备
- s : socket
- p : pipeline
- eventpoll  
 
fd - 文件描述符


seek - 偏移量 

Inode  
- ID 一样，磁盘只有一个文件，删除后文件不一定不在，count 为0 才删除， 
- ID 一个ID一个文件



pagecache 默认 4k

app1  app2  app1修改了文件后，标记成脏，立即flush，交由操作系统(可以配置相关参数)什么时候flush。  


TCP
- 面向连接 - 三次握手后，内核级(客户端，服务端都开辟)开辟资源，
  
- 可靠的传输协议 ack 确认 
  
Socket
- 四元组(cip,cport,sip,sport) 只要四元组唯一，建立多少连接都可以  (内核)

- 一个四元组分配一个文件描述符(fd)给用户态的进程


Socket Buffer 满了，会有数据丢失 。 不处理新来的请求，丢失新来的数据。
Socket Buffer 可以指定为多少个连接开辟缓存，超过了个数，只能等待。等待时间过长，连接就会失效。

可以配置 tcpip 的相关参数

tcpip - keepalive
tcpip 双方建立了连接后，每隔一段时间就是发送 keepalive 包，确认时候还存在。不存在的话可以释放相关的资源。


socket pipeline

head -1 test.txt  // 文件第一行

tail -1 test.txt  // 文件最后一行

文件第8行

head -8 test.txt | tail -1 test.txt


pipeline 开启了子进程进行处理，进程隔离，子进程无法操作父进程的任何内容。


java创建新线程，调用内核的clone()，一个新的进程

clone() 是对 fork 的包装。

建立新的socket，返回一个文件描述符fd3

bind(fd3,8090)  - 建立相关的绑定信息

listen(fd3)

每建立一个连接,根据四元组，得到相关的文件描述符fd3。然后 accept(fd3)， 返回一个新的文件描述符fd5

此时，文件描述符fd5就负责此连接的通信。可以建立新的线程专门负责此通信，处理相关的业务。

每建立一个连接，返回一个新的文件描述符fd给客户端处理相关的业务

#### BIO

```java
    public static void main(String[] args) throws IOException {

        ServerSocket server = null;
        try {
            server = new ServerSocket();
            server.bind(new InetSocketAddress(9090), 1);
            server.setReceiveBufferSize(4096);
            server.setSoTimeout(4000);
        }catch (Exception e){
            e.printStackTrace();
        }

        System.out.println("绑定端口");
        
        try {
            while (true){
                // 阻塞 ,等待新的连接进来
                Socket client = server.accept();
                // 读取数据会阻塞，创建新的线程来读取数据
                new Thread(new Runnable() {
                    Socket ss;
                    public Runnable setSS(Socket s){
                        ss = s;
                        return this;
                    }
                    @Override
                    public void run() {
                        try {
                            InputStream in = ss.getInputStream();
                            BufferedReader reader = new BufferedReader(new InputStreamReader(in));
                            while (true){
                                // 阻塞
                                System.out.println(reader.readLine());
                            }
                        }catch (Exception e){
                            e.printStackTrace();
                        }
                    }
                }.setSS(client)).start();
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
```

BIO 的弊端

- 服务器端不断地查询请求，阻塞等待连接。
  
- BIO 每次接收到新的请求后(阻塞式监听请求)，BIO会新建线程去阻塞读取此请求的数据。请求量大，会导致BIO出现大量的线程。

#### NIO

NIO 是单线程的。

NIO 的问题

- accept() 虽然是非阻塞，但需要系统调用

- 需要对已经建立好的连接进行遍历，查询时是否有数据。


```java
    public static void main(String[] args) throws IOException, InterruptedException {
        LinkedList<SocketChannel> clients = new LinkedList<>();


        ServerSocketChannel ss = ServerSocketChannel.open();
        ss.bind(new InetSocketAddress(9090));
        ss.configureBlocking(false);

        while (true){

            Thread.sleep(1000);

            // 非阻塞, client 为 null， 没有连接
            SocketChannel client = ss.accept();

            if (client == null){
                System.out.println("client is null");
            }else {
                // 非阻塞， 连接后读取数据
                client.configureBlocking(false);
                int port = client.socket().getPort();
                clients.add(client);
            }

            // 开辟堆外内存
            ByteBuffer buffer = ByteBuffer.allocateDirect(4096);

            // 遍历已经建立好连接的客户端有没数据进来

            for (SocketChannel c : clients){
                int num = c.read(buffer);
                if (num > 0){
                    buffer.flip();
                    byte[] aa = new byte[buffer.limit()];
                    buffer.get(aa);

                    String b = new String(aa);
                    System.out.println(c.socket().getPort() + " : " + b);
                    buffer.clear();
                }

            }
        }
    }
```

#### 多路复用

用户态的进程需要只要是否有数据到达，必须要切换到内核态来查询才知道。

遍历调用行为 与 单次

多路复用 - 返回有数据的IO的状态-，用户进程对有状态的IO，切换到内核读取。



IO 

- 同步  app 自己 R/W

- 异步  内核完成 R/W，把数据放到 app 的缓存 ，目前Linux 不支持

- 阻塞  等待有数据再读取

- 非阻塞  轮询访问


同步阻塞 - app 自己 R/W， 阻塞等待数据的到来。 BIO

同步非阻塞 - app 自己 R/W，轮询访问，访问到有数据的就读取数据，没有数据就返回，等下一次轮询。  NIO

异步非阻塞 - window 有具体实现，Linux 目前没有实现。

多路复用的实现 - Linux 同步非阻塞

##### select

int select(int maxfd, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);

- maxfds：是一个整数值，是指集合中所有文件描述符的范围。在linux系统中，select的默认最大值为1024。
- readfds：readfs是一个容器，里面可以容纳多个文件描述符，把需要监视的描述符放入这个集合中，当有文件描述符可读时，select就会返回一个大于0的值，表示有文件可读；
- writefds：和readfs类似，表示有一个可写的文件描述符集合，当有文件可写时，select就会返回一个大于0的值，表示有文件可写；
- exceptfds：用来监视文件错误异常文件。
- timeout


select(fds)

recv(fd)


##### poll 


int poll(struct pollfd fds[], nfds_t nfds, int timeout)；

- fds：用于存放需要检测其状态的Socket描述符；每当调用这个函数之后，系统不会清空这个数组；对于 socket连接比较多的情况下，在一定程度上可以提高处理的效率；这一点与select()函数不同，调用select()函数之后，select() 函数会清空它所检测的socket描述符集合，导致每次调用select()之前都必须把socket描述符重新加入到待检测的集合中；因 此，select()函数适合于只检测一个socket描述符的情况，而poll()函数适合于大量socket描述符的情况；

- nfds：nfds_t类型的参数，用于标记数组fds中的结构体元素的总数量；

- timeout：是poll函数调用阻塞的时间，单位：毫秒；


NIO SELECT POLL 都要遍历所有的IO，询问状态。

- NIO 的遍历的每一次都需要从用户态切换到内核态的切换

- SELECT POLL 的遍历放到的内核态，减少了用户态与内核态的切换，只触发了一次系统调用。
  用户需要把一堆的fds传递给内核，内核进行遍历，修改状态，返回用户。问题？ 用户调用 SELECT， 传递fds后，如何知道我哪些fds有数据了。  什么时候去访问fds。 如何协调工作？ 我读了fd1，内核怎么知道我读取了？

SELECT POLL 问题

- 重复传递fds

- 每次内核被遍历传递过来的fds

- SELECT POLL 只是完成了内核级的数据处理，仍需要用户进程切换到内核态处理数据。

##### epoll(Linux)  kqueue(unix)



int epoll_create(int size);

- 创建一个epoll的句柄。在使用完epoll后，必须调用close()关闭，否则可能导致fd被耗尽。

int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);

epoll的事件注册函数，它不同于select()是在监听事件时告诉内核要监听什么类型的事件，而是在这里先注册要监听的事件类型。

epfd - epoll_create()的返回值。

- 表示动作

- 需要监听的fd。

- 内核需要监听什么事

int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);

- 参数events是分配好的epoll_event结构体数组，epoll将会把发生的事件赋值到events数组中。

- maxevents告之内核这个events有多大，maxevents的值不能大于创建epoll_create()时的size，
  
- timeout 是超时时间（毫秒，0会立即返回，-1将不确定，也有说法说是永久阻塞）。

如果函数调用成功，返回对应I/O上已准备好的文件描述符数目，如返回0表示已超时。

1. APP 绑定端口号后，会得到一个文件描述符fd1

2. epoll_create 用户态转内核态，在内核开辟空间，得到一个文件描述符fd2   红黑树

3. epoll_ctl  添加，修改，删除 , 向红黑树添加，修改，删除节点，有事件完成，将会把事件添加到链表
 
4. epoll_wait 查询链表的事件，返回相关的文件描述符fd。仍需要 recv(fd) 获取数据


把链表放到共享内存 ？ 

epoll 的优化 ? epoll_wait 不需要切换状态 ？  共享内存 ？ 



#### java Selector

Selector 是 java 对多路复用器的封装



poll 

- socket(PF INET,SOCKET STREAM,IPPROTO IP) = 4

- fcnt(4,F SETFL,O RDWRIO NONBLOCK) = 0       server.configureBlocking(false);

- bind(4,(sa fmaily=AE INET, sin port = htons(9090)))     server.bind(new InetSocketAddress(port));

- listen(4,50)  

- poll({fd=5, events= POLLIN},{fd=4,events=POLLIN},2,-1) = 1     selector.select(1000) > 0

- accpet(4,7)  新的连接进来

- poll({fd=5, events= POLLIN},{fd=4,events=POLLIN},{fd=7,events=POLLIN} 3,-1}) =  1 (一个fd有事件)




epoll

- socket(PF INET,SOCKET STREAM,IPPROTO IP) = 4

- fcnt(4,F SETFL,O RDWRIO NONBLOCK) = 0       server.configureBlocking(false);

- bind(4,(sa fmaily=AE INET, sin port = htons(9090)))     server.bind(new InetSocketAddress(port));

- listen(4,50)

- epoll_create(256) = 7 (epfd)

- epoll_ctl(7, EPOLL_CTL_ADD, 4)

- epoll_wait(7, POLLIN )    selector.select(1000) > 0

- accpet(4,8)  新的连接进来  

- fcnt(4,F SETFL,O RDWRIO NONBLOCK) = 0  非阻塞

- epoll_ctl(7, EPOLL_CTL_ADD, 8)



一个线程专门负责处理进来的连接


开启更多的线程处理读，写事件。


解决方案

- M 个 selector， N 个 fds ，N / M ， 一个 selector 一个线程

- 部分线程只负责接收，均匀分发到 M 个 selector ， selector 再进行处理


主从



IO threads




![Netty - 主从](/medias/Server/1654574549716.jpg)


#### C10K
http://www.kegel.com/c10k.html


连接数的限制，内存大小，Linux 单个进程最大文件的打开个数 - 一个连接一个文件

基于 select() 的服务器

基于 dev / poll 的服务器

基于 epoll 服务器

基于 kqueue 服务器

基于 实时信号 的服务器


多路复用器





IO(四) epoll 演示


IO 快 丢数据？

NIO 慢 可靠安全？


RDB 







