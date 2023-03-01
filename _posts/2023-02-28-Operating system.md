---
layout: post
title: 操作系统
categories: [操作系统]
description: OS
keywords: Operating system 
---

# 概述

1. 何为操作系统？

   目前对于操作系统并没有公认的非常精确的定义。大部分资料给出的定义如下：首先，操作系统是一种系统软件，能够管理硬件资源、控制程序运行、改善人机界面和为应用软件提供支持的一种系统软件。从这个意义上讲，目前的微信也应属于操作系统，例如，微信能够支持各种小程序，为这些小程序提供硬件资源的分配，能够控制各小程序的运行，同时各种UI设计改善了人接交互界面并且为这些小程序提供支持。

   对操作系统软件开发人员来讲，真正的操作系统是操作系统kernel，这是运行在CPU内核态的，而其他软件如微信等是运行在用户态的，从这个意义上讲微信不属于操作系统。

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301082600368.png" alt="image-20230301082600368" style="zoom:80%;" />

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301080341308.png" alt="image-20230301080341308" style="zoom:67%;" />

2. 不同阶段的操作系统要干什么？

   操作系统最直接的需求就是要管理硬件、支持软件应用程序。在单用户系统时代，使用纸带等进行程序的输入和输出，此时的操作系统就是装载器和程序库的集合，但是昂贵的CPU硬件设备大量时间处于等待过程中，利用率太低，为了解决CPU利用率太低的问题，提出了批处理系统，使用磁盘、磁带等存储设备进行程序的输入和输出，大大减少了CPU的等待时间，提高了CPU利用率，此时的操作系统就是装载器、程序控制器和输出处理器的集合。

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301080613424.png" alt="image-20230301080613424" style="zoom:18%;" />

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301080429833.png" alt="image-20230301080429833" style="zoom:25%;" />

   

   但是此时的操作系统对于各应用程序的支持仍然是各程序按照加载到内存中的顺序来执行。为了进一步提高系统的任务吞吐量和CPU利用率有了多道批处理系统，使得各应用程序能够交替使用CPU，此时的操作系统就是装载器、程序调度、内存管理和输出管理的集合。

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301081020636.png" alt="image-20230301081020636" style="zoom: 30%;" />

   为了满足用户和主机的交互以及多任务、多用户的支持提出了分时操作系统，主机以时间片为单位轮流分配给各用户或者终端使用，满足了用户和主机的交互、用户对主机的独占感知以及多用户共同使用同一主机时候的协调，UNIX。此时的操作系统就是装载器、程序调度、内存管理、中断处理等的集合。

   <img src="https://wendaocsmaster.github.io/images/blog/image-20230301082159602.png" alt="image-20230301082159602" style="zoom:30%;" />

3. 应用程序、操作系统、硬件设备

   随着硬件设备性能的提高，为了提高硬件设备的利用率要求操作系统能够在宏观上支持多个应用程序同时执行<font color = red>（并发）</font>，但是对于单CPU主机，各个应用程序又不可能在同一时刻使用CPU等设备<font color=red>（互斥共享）</font>，同时在微观上各应用程序是走走停停，完成的时间不能确定<font color = red>（异步）</font>，但是各应用程序感知到的是该程序“独占”用户主机，某些应用程序对于内存的要求可能远大于实际的物理内存，但是操作系统依然能够支持该程序的运行<font color = red>（虚拟）</font>。

虚拟化、并发、持久性——[Operating Systems: Three Easy Pieces (wisc.edu)](https://pages.cs.wisc.edu/~remzi/OSTEP/)

## 参考资料

[操作系统课在线幻灯片链接](https://www.yuque.com/xyong-9fuoz/qczol5/glemuu?)

[os-lectures2023: OS Lectures 2022 Spring ](https://learningos.github.io/os-lectures/)

[rust-based-os-comp2023: 2023开源操作系统训练营](https://learningos.github.io/rust-based-os-comp2022/)

[OS video](https://github.com/wendaocsmaster/rust-based-os-comp2023/blob/main/relatedinfo.md)

[lab-uCore-Tutorial-Guide-2023S 文档 (learningos.github.io)](https://learningos.github.io/uCore-Tutorial-Guide-2023S/)

[lab-rCore-Tutorial-Book-v3 3.6.0-alpha.1 文档 (rcore-os.cn)](http://rcore-os.cn/rCore-Tutorial-Book-v3/index.html)

[Introduction · GitBook (learningos.github.io)2020](https://learningos.github.io/ucore_os_webdocs/)

[Operating Systems: Three Easy Pieces (wisc.edu)](https://pages.cs.wisc.edu/~remzi/OSTEP/)

[Index of /~remzi/OSTEP/Chinese (wisc.edu)](https://pages.cs.wisc.edu/~remzi/OSTEP/Chinese/)

[Introduction  uCore Lab Documents (gitbooks.io)2015](https://objectkuan.gitbooks.io/ucore-docs/content/)

[操作系统-rCore-Tutorial-Book-v3 3.6.0-alpha.1 文档 (rcore-os.cn)](http://rcore-os.cn/rCore-Tutorial-Book-v3/chapter0/index.html)
