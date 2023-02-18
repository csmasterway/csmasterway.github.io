---
layout: post
title: 计算机网络网络层路由选择协议RIP
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer 
---

## 路由选择协议RIP

1. RIP距离：RIP要求自治系统AS内的每一个路由器都要维护一个它自己到自制系统内其他每个路由器的距离记录。这一组距离叫做距离向量。

   + RIP使用跳数为单位来度量到达其他网络的距离。

   + RIP将路由器到直连网络的距离定义为1，将路由器到非直连网络的距离了定义为经过的路由器的个数加1。

   + RIP允许一条路径上包含的路由器的个数最大为15，当距离等于16时候表示目的网络不可达。

     ![image-20230216151508526](E:\myblog\csmasterway.github.io\images\blog\image-20230216151508526.png)

2. RIP认为跳数少的路径就是最短路由。当到达同一目的网络存在多条距离相等的路由时候，可以进行等价负载均衡，将通信量均衡地分布到多条等价的路径上。

   ![image-20230216152429609](E:\myblog\csmasterway.github.io\images\blog\image-20230216152429609.png)

3. RIP协议存在局限性，仅仅使用跳数来进行衡量最有路径，到达目的网络的路径是RIP距离最小的，但是也有可能是带宽最小的。如下图，RIP会认为从R1到达R5的最优路径是R1:arrow_right:R4:arrow_right:R5，但是从结合带宽来看，该路径显然不是最优路径，经过该路径从R1到达R5有可能耗时更多。

   ![image-20230216152108661](E:\myblog\csmasterway.github.io\images\blog\image-20230216152108661.png)

4. RIP仅和相邻的路由器交换各自的路由表信息，周期性进行交换例如每隔30s，为了加快网络拓扑发生变化时RIP的收敛速度，可将周期性交换路由表信息改为触发式更新，当网络拓扑发生变化时，立即向相邻路由器报告变化后的路由信息。

5. RIP的基本工作过程

   + 路由器开始工作时候，只知道自己到直连网络的RIP距离为1

   + 每个路由器仅和相邻的路由器周期性交换更新路由信息

   + 若干次交换和更新后，每个路由器就知道到达本本自治系统内各网络的最短距离和下一跳路由器，RIP收敛。

     ![image-20230216153623504](E:\myblog\csmasterway.github.io\images\blog\image-20230216153623504.png)

     如上图所示，初始时刻RIP只知道自己直连网络的RIP距离为1，经过周期性地和相邻路由器交换路由表信息后最终达到RIP收敛，自治系统内的每个路由器都知道到达自治系统内的其他网络的最短距离和路径。

6. RIP距离向量算法的具体过程

   + 给相邻路由器发送封装有自己所知路由信息的RIP更新报文

     

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
