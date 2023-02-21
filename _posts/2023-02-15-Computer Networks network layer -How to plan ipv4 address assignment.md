---
layout: post
title: 计算机网络网络层如何规划IPv4地址分配
categories: [计算机网络]
description: 网络层
keywords: computer networks network layer 
---

## 如何规划IPv4地址分配

+ 定长子网掩码划分：划分出的每一个子网都使用相同的子网掩码，容易造成地址浪费

  ![image-20230215195413508](https://wendaocsmaster.github.io/images/blog/image-20230215195413508.png)

  + 网络1有6个主机接口，1个路由器接口，再加上主机号全0的网络地址和主机号全1的广播地址，共需要9个IP地址

  + 网络2有25个主机接口，1个路由器接口，再加上主机号全0的网络地址和主机号全1的广播地址，共需要28个IP地址

  + 网络3有12个主机接口，1个路由器接口，再加上主机号全0的网络地址和主机号全1的广播地址，共需要15个IP地址

  + 网络1有10个主机接口，1个路由器接口，再加上主机号全0的网络地址和主机号全1的广播地址，共需要13个IP地址

  + 网络5有两个路由器接口，再加上主机号全0的网络地址和主机号全1的广播地址，共需要4个IP地址

  + C类网络218.75.230.0共有256个地址，使用定长子网掩码划分，则每个网络最少分配32个地址，从该C类网络地址的主机号部分借用3个bit表示主机号，将该C类网络地址划分为8个子网，所以子网掩码为255.255.255.224，划分后的地址细节如下表，只需要从子网1-8中任选5个划分给图中的网络即可。

    | **子网** | **网络地址**       | **广播地址**       | **该子网上的可分配地址**          |
    | -------- | ------------------ | ------------------ | --------------------------------- |
    | **1**    | **218.75.230.0**   | **218.75.230.31**  | **218.75.230.1~218.75.230.30**    |
    | **2**    | **218.75.230.32**  | **218.75.230.63**  | **218.75.230.33~218.75.230.62**   |
    | **3**    | **218.75.230.64**  | **218.75.230.95**  | **218.75.230.65~218.75.230.94**   |
    | **4**    | **218.75.230.96**  | **218.75.230.127** | **218.75.230.97~218.75.230.126**  |
    | **5**    | **218.75.230.128** | **218.75.230.159** | **218.75.230.129~218.75.230.158** |
    | **6**    | **218.75.230.160** | **218.75.230.191** | **218.75.230.161~218.75.230.190** |
    | **7**    | **218.75.230.192** | **218.75.230.223** | **218.75.230.193~218.75.230.222** |
    | **8**    | **218.75.230.224** | **218.75.230.255** | **218.75.230.225~218.75.230.254** |

  + 显然，网络5本来只需要4个地址即可，但是却分配了32个地址，造成了地址资源的浪费。

+ 不定长子网掩码划分：划分出的每一个子网可以使用不同的子网掩码，尽量减少地址资源的浪费

  ![image-20230215195418590](https://wendaocsmaster.github.io/images/blog/image-20230215195418590.png)

  ​		如上图，网络1需要9个IP地址，那么划分给它16个IP地址就足够，网络2需要28个地址，那么划分给它32个地址就足够，网络3需要15个地址，那么划分给它16个就足够，网络4需要13个地址，那么划分给它16个地址就足够，网络5需要4个地址，划分给它4个地址正好。从218.75.230.0/24地址块中取出5个地址块/30, /28, /28, /28, /27分别划分给网络5，网络1，网络3，网络4和网络2。

  ![image-20230215195923981](https://wendaocsmaster.github.io/images/blog/image-20230215195923981.png)

  ​		**在地址块中选取子块有有原则，只能选取主机号部分是块大小的整数倍作为地址起点，一般先为大的子块选取，如果先为小的子块选取有可能造成地址资源的浪费。**如下图中5个网络中只有1个网络需要4个地址，其余网络需要的块大小最小为16，当首先为网络5选取一个大小为4的地址块后，从218.75.230.4-218.75.230.16之间的地址无法分配给其他网络，造成了地址资源的浪费。当然如果图中还有3个需要4个地址的网络，则不会造成地址资源浪费。

  ​		如果在218.75.230.0/24地址块中首先为网络选取地址块，则给网络5分配的地址块为218.75.230.0/30，然后其余4个网络中需求最小的地址块大小为16，可以从主机号借用4位作为子网号，从218.75.230.0/30，即218.75.230.<font color =red>0000</font> 0000中选取大小为16的地址块，如果借用的4位子网号为0000，那么会包括先前为网络5选取的地址块，此时子网号的最小取值为0001，对应的点分十进制为218.75.230.16，子网掩码为255.255.255.240，斜线记法为218.75.230.16/28，那么就造成了218.75.230.4-218.75.230.16之间的网络地址浪费。

  ![image-20230215200352828](https://wendaocsmaster.github.io/images/blog/image-20230215200352828.png)

## 参考资料

[深入浅出计算机网络（微课视频版）](http://www.tup.tsinghua.edu.cn/booksCenter/book_09342101.html)

![img](https://wendaocsmaster.github.io/images/blog/093421-01.jpg)