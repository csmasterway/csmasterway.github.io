---
layout: post
title: 操作系统的安全机制
categories: [操作系统]
description: OS
keywords: Operating system 
---

## 安全机制

操作系统为上层应用提供服务，如何防止恶意应用破坏其他应用甚至破坏操作系统？

权利：用户程序运行在在用户态只能执行非特权指令，操作系统运行在内核态，应用程序要得到操作系统的服务必须通过操作系统和应用程序之间的系统调用接口来实现。

内存空间上：应用程序运行在用户空间，OS运行在内核空间

## 隔离

通过隔离使得应用程序不会影响或破坏其他应用程序或者操作系统的正常运行以及信息泄露，但是隔离并不是相互之间没有关系的隔离，应用程序仍然需要OS的支持和服务，所以要要在共享某些资源的情况下实现隔离。隔离的一般方法如下：

+ 控制隔离：特权级机制，用户态和内核态
+ 数据隔离：用户地址空间和内核地址空间，每一个应用程序仅仅能够寻址自己的内存，没有许可不能访问不属于自己的内存
+ 时间隔离：中断处理，随时能打断正在执行的用户态APP，更重要的是能够打断一直占用CPU的APP
+ 对破坏隔离的处理：异常处理
+ 虚拟内存：OS完成虚拟内存到物理内存的映射，应用程序知道虚拟内存的地址而无法获取真实物理内存地址
+ CPU硬件中的特权模式：关键寄存器禁止应用程序访问
+ CPU硬件支持的不同的特权模式：x86和risc-v都有4种模式，以risc-v为例，有M、H、S、U模式，SBI运行在M mode、OS运行在S mode、APP运行在U mode，H mode用于提供虚拟化支持，risc-v的不同模式组合可以设计出适用于不同应用场景的CPU更加灵活

## 参考资料

[操作系统课在线幻灯片链接](https://www.yuque.com/xyong-9fuoz/qczol5/glemuu?)

[os-lectures2023: OS Lectures 2022 Spring ](https://learningos.github.io/os-lectures/)

[rust-based-os-comp2023: 2023开源操作系统训练营](https://learningos.github.io/rust-based-os-comp2022/)

[OS video](https://github.com/wendaocsmaster/rust-based-os-comp2023/blob/main/relatedinfo.md)

[lab-uCore-Tutorial-Guide-2023S 文档 (learningos.github.io)](https://learningos.github.io/uCore-Tutorial-Guide-2023S/)

[lab-rCore-Tutorial-Book-v3 3.6.0-alpha.1 文档 (rcore-os.cn)](http://rcore-os.cn/rCore-Tutorial-Book-v3/index.html)

[Introduction · GitBook (learningos.github.io)2020](https://learningos.github.io/ucore_os_webdocs/)

[ucore-os-docs](https://github.com/csmasterpath/ucore_os_docs)

[Operating Systems: Three Easy Pieces (wisc.edu)](https://pages.cs.wisc.edu/~remzi/OSTEP/)

[Index of /~remzi/OSTEP/Chinese (wisc.edu)](https://pages.cs.wisc.edu/~remzi/OSTEP/Chinese/)

[Introduction  uCore Lab Documents (gitbooks.io)2015](https://objectkuan.gitbooks.io/ucore-docs/content/)

[操作系统-rCore-Tutorial-Book-v3 3.6.0-alpha.1 文档 (rcore-os.cn)](http://rcore-os.cn/rCore-Tutorial-Book-v3/chapter0/index.html)
