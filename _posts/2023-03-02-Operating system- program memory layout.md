---
layout: post
title: 程序内存布局
categories: [操作系统]
description: OS
keywords: Operating system 
---

## 程序内存布局

![../_images/MemoryLayout.png](https://wendaocsmaster.github.io/images/blog/MemoryLayout.png)

+ .text：代码段
+ .rodata：只读的数据 静态内存分配
+ .data：可修改的全局变量 静态内存分配
+ .bss：未初始化的全局变量 block started by symbol 静态内存分配

> data是已经初始化的数据，所以是要在二进制文件中占用空间的，而bss段因为都是未初始化的数据所以加载的时候由内核或者加载器将其初始化为0，本身在二进制文件中并不占用空间

+ heap：动态分配内存的数据，向高地址增长 典型使用方式就是C中的malloc和free
+ stack：局部变量数据，向低地址增长

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
