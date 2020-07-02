# 2.1 introduction
SCTP是流控制传输协议,是个比较新的协议
# 2.6 TCP连接建立和终止
## 建立连接:
1. 服务端必须准备就绪接受一个连接进来, 通常通过`socket, bind, listen`来实现, 也叫作被动打开
2. 客户端通过调用`connect`发出一个主动打开, 客户端TCP发送一个"同步"(SYN)段, 告诉服务器客户端要发送数据的初始序号. 通常SYN不会伴随这数据发送;只是包含了IP header, TCP header, 还可能包含TCP选项
3. 服务器必须承认(ACK)客户端的SYN并且服务器必须也发送自己的包含即将通过该连接发送的数据初始序列号的SYN. 服务器发送的SYN和ACK在同一个段中.
4. 客户端必须承认服务器的SYN
这种方式需要交换的包最少3个,因此称为三次握手

## TCP选项
每个SYN可以包含TCP选项, 通常有以下几种:
- MSS: maximum segment size. 发送端TCP使用接收端的MSS作为它发送的最大段大小
- window scale: TCP可以广播到其他TCP的最大窗口是65535, 因为