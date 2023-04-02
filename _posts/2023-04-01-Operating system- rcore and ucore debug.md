---
layout: post
title: rCore和uCore的可视化debug
categories: [操作系统]
description: OS
keywords: Operating system 
---

# 前言

uCore和rCore的调试工作通常使用gdb进行调试，但是通常情况下使用gdb调试对于不同源文件之间的切换以及各个函数的搜索等个人感觉不太友好，而在vscode中可以解决这些痛点，但是根据uCore官方给出的Makefile文件中仅仅提供了在终端中使用gdb进行调试的方式，使用命令`make debug`，结合vscode对源文件阅读搜索等方面的友好以及gdb的调试功能实现可视化快捷调试

# vscode中结合gdb实现uCore可视化debug

主要设计到两方面的修改

## vscode gdb启动项配置文件

~~~json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "cppdbg",
            "request": "launch",
            "name": "Attach to gdbserver",
            //内核文件
            "program": "${workspaceFolder}/build/kernel",
            "miDebuggerServerAddress": "localhost:1234", 
            //指定远程调试端口，qemu启动后通过在1234端口监听等待debugger连接
            "miDebuggerPath": "/usr/bin/riscv64-unknown-elf-gdb",//gdb路径
            "cwd": "${workspaceRoot}", 
            //vscode的环境变量和workspaceFolder的功能一样，但是在vscode的后续版本中推荐使用				workspaceRoot
        }
    ]
}
~~~

## Makefile

新增命令`make gdbsever`将QEMU启动，然后F5开启调试

![image-20230402202218511](https://wendaocsmaster.github.io/images/blog/image-20230402202218511.png)

## gdb结合dashboard

感觉这种方式比单纯的gdb加tui的方式要舒服一点，甚至可能比vscode加gdb更方便舒服点，可以同时显示汇编码和源码，甚至在查看内存中的内容和寄存器方面更有优势，但是对于环境有一定的要求

+ 安装调试工具链https://github.com/sifive/freedom-tools，修改Makefile文件使用`*-gdb-py`代替原来的`*gdb`

+ 安装`gdb-dashboard`,及其用法https://github.com/cyrus-and/gdb-dashboard

  正常显示应该如下图所示，但是可能我的环境有问题，连续遇到了3个问题:cry:最后 一个是未知错误 :cry:

  ### First blood

  > make debug
  >
  > 启动后提示/usr/local/riscv64/python/lib/python3.7/lib-dynload/math.cpython-37m-x86_64-linux-gnu.so: undefined symbol: PyFloat_Type
  >
  > >动态链接库有问题，重新下载python3.7.0进行源码编译生成新的动态链接文件
  > >
  > >~~~sh
  > >wget https://mirrors.huaweicloud.com/python/3.7.0/Python-3.7.0.tgz
  > >tar xzvf Python-3.7.0.tgz   
  > >cd Python-3.7.0/                                    
  > >./configure --enable-shared 
  > >make
  > >cp math.cpython-37m-x86_64-linux-gnu.so /usr/local/riscv64/python/lib/python3.7/lib-dynload/
  > >~~~

  ### Double kill

  >make debug
  >
  >ImportError: libpython3.7m.so.1.0: cannot open shared object file: No such file or directory
  >
  >没有动态链接文件，发现缺少环境变量和和文件，刚刚重新编译的3.7.0中是有这个文件的
  >
  >~~~sh
  > export LD_LIBRARY_PATH="/usr/local/riscv64/python/lib:/usr/local/riscv64/python/lib:$LD_LIBRARY_PATH"
  > #写入.bashrc中或者仅在中端中执行作为临时环境变量
  > cp libpython3.7m.so.1.0 /usr/local/riscv64/python/lib
  >~~~

  ### Extermation

  >make debug
  >
  >make: *** [Makefile:117: debug] Segmentation fault (core dumped)，启动之后闪退
  >
  >:cry::cry::cry:
  >
  >Suspend to solve this problem

  ![img](https://i.v2ex.co/4mzn264Y.png)

  ### gdb调试其他C程序

  ![image-20230402215627836](https://wendaocsmaster.github.io/images/blog/image-20230402215627836.png)

## vscode中结合gdb实现rCore可视化debug

### 方案1[第零章：实验环境配置 - Open-Source-OS-Training-Camp-2022 文档 ](https://learningos.github.io/rust-based-os-comp2023/0setup-devel-env.html)

修改Makefile文件

打开对应实验目录下的 `Makefile` 文件，例如在开发os8，那么就是 `os8/Makefile` 文件，在其结尾添加如下几行：

```
dbg: build
   qemu-system-riscv64 -machine virt -nographic -bios $(BOOTLOADER) -device loader,file=$(KERNEL_BIN),addr=$(KERNEL_ENTRY_PA) -drive file=$(FS_IMG),if=none,format=raw,id=x0 -device virtio-blk-device,drive=x0,bus=virtio-mmio-bus.0 -s -S
```

```
make dbg
```

之后，是照常的编译流程，编译完成后，Terminal会卡在启动qemu后的状态，此时，在VSCode中按下F5键开启调试

在新版本的rCore给出的Makefile文件中debug有make gdbserver和make gdbclient和上述原理类似

### 方案2[支持Rust语言的源代码级内核调试工具](https://github.com/chenzhiy2001/code-debug#安装与使用)

[方案作者的视频演示](https://www.bilibili.com/video/BV1gW4y1x7ys/?spm_id_from=333.999.0.0&vd_source=ab418999e896fd33cc8eefbeab063d7f)

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
