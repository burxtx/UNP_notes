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
一个input操作分为两个阶段:
1. 等待数据准备好
2. 从内核复制到app进程

对于一个socket上的输入操作, 第一步包含了等待网络上的数据抵达. 当包到达后, 它先被copy到内核缓冲区. 第二步开始, 从内核缓冲区copy到app缓冲区
## 阻塞I/O模型
## 非阻塞I/O模型
## I/O多路复用
## 信号驱动的I/O(SIGIO)
## 异步I/O(posix aio_函数)

# 6.3 select function
