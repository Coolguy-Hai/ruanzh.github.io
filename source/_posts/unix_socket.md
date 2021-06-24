---
title: 【网络编程】Unix Socket
date: 2021-06-23 13:47:11
tags: 网络编程
---

UNIX Domain SOCKET 是在Socket架构上发展起来的用于同一台主机的进程间通讯（IPC）。它不需要经过网络协议栈，不需要打包拆包、计算校验和、维护序列号应答等。只是将应用层数据从一个进程拷贝到另一个进程。UNIX Domain SOCKET有SOKCET_DGRAM和SOCKET_STREAM两种模式，类似于UDP和TCP，但是面向消息的UNIX socket也是可靠的，消息既不会丢失也不会顺序错乱。

 UNIX Domain Socket是全双工的，API接口语义丰富，相比其它IPC机制有明显的优越性，目前已成为使用最广泛的IPC机制，比如X Window服务器和GUI程序之间就是通过UNIX Domain Socket通讯的。

使用UNIX Domain Socket的过程和网络socket十分相似，也要先调用socket()创建一个socket文件描述符，address family指定为AF_UNIX，type可以选择SOCK_DGRAM或SOCK_STREAM，protocol参数仍然指定为0即可。

UNIX Domain Socket与网络socket编程最明显的不同在于地址格式不同，用结构体sockaddr_un表示，网络编程的socket地址是IP地址加端口号，而UNIX Domain Socket的地址是一个socket类型的文件在文件系统中的路径，这个socket文件由bind()调用创建，如果调用bind()时该文件已存在，则bind()错误返回。

参考：[UNIX SOCKET简介](https://blog.csdn.net/zhangkun2609/article/details/84188465)