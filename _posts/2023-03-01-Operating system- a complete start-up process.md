---
layout: post
title: 一次完整的计算机上电启动过程
categories: [操作系统]
description: OS
keywords: Operating system 
---

## 一次完整的计算机上电启动过程

1. 计算机上电后CPU的PC寄存器被设置为计算机内部只读寄存器ROM的物理地址，随后CPU开始运行ROM中的软件，对CPU进行初始化，并加载操作系统引导程序bootloader，随后跳转到bootloader的程序中
2. bootloader同样对CPU进行一些初始化工作，将操作系统镜像加载到物理内存中，随后跳转到适当地址将计算机控制权交给操作系统

综上计算机的一次完整的启动过程伴随着计算机控制权的转移

ROM->BOOTLOADER->OS

以QEMU和最简单的LibOS为例:QEMU将

![LibOS总体结构](https://wendaocsmaster.github.io/images/blog/lib-os-detail.png)

QEMU的内存空间布局如下

~~~C
static const struct MemmapEntry {
    hwaddr base;
    hwaddr size;
} virt_memmap[] = {
    [VIRT_DEBUG] =       {        0x0,         0x100 },
    [VIRT_MROM] =        {     0x1000,       0x11000 },//启动 ROM
    [VIRT_TEST] =        {   0x100000,        0x1000 },
    [VIRT_RTC] =         {   0x101000,        0x1000 },
    [VIRT_CLINT] =       {  0x2000000,       0x10000 },
    [VIRT_PLIC] =        {  0xc000000,     0x4000000 },
    [VIRT_UART0] =       { 0x10000000,         0x100 },//UART 串口
    [VIRT_VIRTIO] =      { 0x10001000,        0x1000 },
    [VIRT_FLASH] =       { 0x20000000,     0x4000000 },
    [VIRT_DRAM] =        { 0x80000000,           0x0 },//设备内存
    [VIRT_PCIE_MMIO] =   { 0x40000000,    0x40000000 },
    [VIRT_PCIE_PIO] =    { 0x03000000,    0x00010000 },
    [VIRT_PCIE_ECAM] =   { 0x30000000,    0x10000000 },
};
~~~



+ 物理内存的起始物理地址为`0x8000 0000`, 在计算机接电后，CPU的程序计数器PC会被初始化为`0x1000` 由固化在 [QEMU模拟的计算机内存](https://github.com/LearningOS/qemu/blob/386b2a5767f7642521cd07930c681ec8a6057e60/hw/riscv/virt.c#L59)中的[一小段汇编程序](https://github.com/LearningOS/qemu/blob/386b2a5767f7642521cd07930c681ec8a6057e60/hw/riscv/virt.c#L536)初始化并跳转执行bootloader，其起始地址为`0x8000 0000`

  以下代码就是被写入ROM中的程序

  ~~~C
  uint32_t reset_vec[8] = {
          0x00000297,                  /* 1:  auipc  t0, %pcrel_hi(dtb) */
          0x02028593,                  /*     addi   a1, t0, %pcrel_lo(1b) */
          0xf1402573,                  /*     csrr   a0, mhartid  */
  #if defined(TARGET_RISCV32)
          0x0182a283,                  /*     lw     t0, 24(t0) */
  #elif defined(TARGET_RISCV64)
          0x0182b283,                  /*     ld     t0, 24(t0) */
  #endif
          0x00028067,                  /*     jr     t0 */
          0x00000000,
          start_addr,                  /* start: .dword */
          0x00000000,
                                       /* dtb: */
      };
  ~~~

  

+ bootloader的bin文件放置在进入地址`0x8000 0000`后面对的是bootloader的第一条指令，对计算机进行一些初始化工作

![image-20230303200618017](https://wendaocsmaster.github.io/images/blog/image-20230303200618017.png)

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
