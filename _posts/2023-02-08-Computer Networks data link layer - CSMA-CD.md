---
layout: post
title: 计算机网络数据链路层有线共享式以太网封装格式
categories: [计算机网络]
description: 如何实现数据链路层封装
keywords: computer networks data link layer 
---

## 网络适配器和MAC地址

1. 网络适配器：俗称网卡，集成了数据链路层和物理层的功能，即网络接口层

   网卡与外部以太网之间的通信，一般是通过传输媒体以<font color = red>串行传输</font>进行的

   网卡和CPU以及存储器之间的数据传输方式是并行传输，网卡和外部以太网之间的传输方式一般是串行传输，因此主机在与网布网络通信时，网卡要根据收发数据进行串并转换和并串转换

2. MAC地址:

   + 点对点链路无需使用MAC地址
   + 当众多主机在同一广播信道上时，要想实现两个主机之间的通信，每必须使用MAC地址来区分各主机
   + MAC地址存储在网卡的EEPROM中
   + 同一台主机可以有多块网卡，就会有多个MAC地址，严格来说MAC地址是对网络上各接口的唯一标识，而不是对网络上各设备的唯一标识

3. MAC地址分类

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)
