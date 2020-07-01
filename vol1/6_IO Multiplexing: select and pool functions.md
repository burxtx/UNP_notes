# 6.1 introduction
-- 0. TCP 客户端同时处理两个输入时:一个标准输入和一个TCP socket, 会有一个问题, 当客户端在调用fgets时被阻塞 server进程被kill掉. 服务端TCP正确的发送了一个FIN到客户端, 但是客户端进程此时读标准输入被阻塞, 将不会收到EOF.
1. IO多路复用: 需要kernel提供这样一种能力, 当一个或多个I/O条件准备好后通知用户app. 
2. 使用场景:
    - 当客户端处理多个描述符时(通常是中断输入和一个网络socket)
    - 客户端同时处理多个sokets时
    - TCP server同时处理监听和连接中的sockets
    - server同时处理TCP和UDP请求
    - server同时处理多个服务或者多个协议
    > IO多路复用不止局限于网络编程, 一些复杂应用也需要用到这种技术
# 6.2 I/O models
一个input操作通常分为两个阶段:
1. 等待数据准备好
2. 从内核复制数据到进程

对于一个socket上的输入操作, 第一步通常包含等待网络上的数据抵达. 当包到达后, 它先被copy到内核缓冲区. 第二步开始, 从内核缓冲区copy到app缓冲区
## 阻塞I/O模型
最普遍的I/O模型是阻塞模型, 所有sockets是默认阻塞的.  
我们使用UDP数据报来解释这一模型, 用UDP的原因是, "准备好读的数据"这一概念很简单, 要么完整数据报接收到, 要么没有. 如果在TCP条件下就复杂一些, 因为额外的变量例如socket的low-water mark发挥作用.
![blockio](../img/6_block_io.png)
## 非阻塞I/O模型
当我们将socket设置成非阻塞的, 意思是告诉kernel"当IO操作请求不使进程进入休眠状态而无法完成时, 不要将进程置为休眠状态, 但是要返回错误"
![nonblockingio](../img/6_nonblocking_io.png)
当一个应用循环调用recvfrom时, 这种方式叫做轮询, 应用持续轮询kernel来判断操作是否就绪, 这会很耗费CPU时间片
## I/O多路复用
![io multiplexing](../img/6_IO_Multiplexing.png)
## 信号驱动的I/O(SIGIO)
![signal_driven_io](../img/6_signal_driven_io.png)
## 异步I/O(posix aio_函数)
![aio](../img/6_aio.png)

# 6.3 select function
