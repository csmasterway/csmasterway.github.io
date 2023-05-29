---
layout: post
title: 一次完整的计算机上电启动过程
categories: [操作系统]
description: OS
keywords: Operating system 
---

## 一次完整的计算机上电启动过程

### 真实计算机

1. ROM stage：计算机上电后CPU的PC寄存器的值被设置为ROM的物理地址，并且运行ROM内的软件（一般叫做固件 firmware），对CPU进行一些初始化工作，将后续需要的bootloader的代码和数据加载到物理内存中，这一阶段直接在ROM中运行。
2. RAM stage：检测并初始化CPU、以及主板等，这一阶段在RAM上运行。
3. BootLoader stage：在内存中找到bootloader并执行bootloader的指令，完成对CPU的一些初始化工作，将操作系统镜像从硬盘加载到物理内存中，随后跳转将计算机的控制权转交给操作系统。这一阶段计算机的控制权属于bootloader
4. OS stage：这一阶段由操作系统控制计算机。

综上计算机的一次完整的启动过程伴随着计算机控制权的转移

ROM->BOOTLOADER->OS

### QEMU

>QEMU中ROM和SBI功能简单，没有能力负责加载bootloader和OS镜像，他们是在QEMU启动之前就被加载到QEMU的内存中的

+ QEMU的内存空间布局如下,QEMU接电后将必要文件加载到内存中，包括bootloader和内核镜像，将bootloader放到QEMU物理内存地址`0x8000 0000`处，内核镜像文件放在`0x8020 0000`处

    > 内核镜像文件与可执行文件是不一样的，可执行文件包括了一些文件符号信息，而内核镜像是纯二进制指令，内核镜像由可执行文件进一步处理得到
    
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



+ CPU的程序计数器PC会被初始化为`0x1000` 由固化在 [QEMU模拟的计算机内存](https://github.com/LearningOS/qemu/blob/386b2a5767f7642521cd07930c681ec8a6057e60/hw/riscv/virt.c#L59)中的[一小段汇编程序](https://github.com/LearningOS/qemu/blob/386b2a5767f7642521cd07930c681ec8a6057e60/hw/riscv/virt.c#L536)初始化并跳转执行bootloader，其起始地址为`0x8000 0000`

  以下代码就是被写入ROM中的程序

  ~~~C
   uint32_t reset_vec[10] = {
          0x00000297,                  /* 1:  auipc  t0, %pcrel_hi(fw_dyn) */
          0x02828613,                  /*     addi   a2, t0, %pcrel_lo(1b) */
          0xf1402573,                  /*     csrr   a0, mhartid  */
          0,
          0,
          0x00028067,                  /*     jr     t0 */
          start_addr,                  /* start: .dword */
          start_addr_hi32,
          fdt_load_addr,               /* fdt_laddr: .dword */
          0x00000000,
                                       /* fw_dyn: */
      };
      if (riscv_is_32bit(harts)) {
          reset_vec[3] = 0x0202a583;   /*     lw     a1, 32(t0) */
          reset_vec[4] = 0x0182a283;   /*     lw     t0, 24(t0) */
      } else {
          reset_vec[3] = 0x0202b583;   /*     ld     a1, 32(t0) */
          reset_vec[4] = 0x0182b283;   /*     ld     t0, 24(t0) */
      }
  
  ~~~

  

+ bootloader的bin文件放置在进入地址`0x8000 0000`后面对的是bootloader的第一条指令，对计算机进行一些初始化工作，然后跳转到内核镜像地址`0x8020 0000`

![image-20230303200618017](https://wendaocsmaster.github.io/images/blog/image-20230303200618017.png)

## 调试启动过程

启动QEMU并加载SBI以及内核镜像

~~~
qemu-system-riscv64 \
    -machine virt \
    -nographic \
    -bios ../bootloader/rustsbi-qemu.bin \
    -device loader,file=target/riscv64gc-unknown-none-elf/release/os.bin,addr=0x80200000 \
    -s -S
~~~

在另一个终端中输入

~~~、
riscv64-unknown-elf-gdb \
    -ex 'file target/riscv64gc-unknown-none-elf/release/os' \
    -ex 'set arch riscv:rv64' \
    -ex 'target remote localhost:1234'
~~~

对指令进行反汇编,可以看到QEMU的固件中共有5条指令,当数据为 0 的时候则会被反汇编为 `unimp` 指令。

```asm
(gdb) x/10i $pc
=> 0x1000:      auipc   t0,0x0
   0x1004:      addi    a2,t0,40
   0x1008:      csrr    a0,mhartid
   0x100c:      ld      a1,32(t0)
   0x1010:      ld      t0,24(t0)
   0x1014:      jr      t0
   0x1018:      unimp
   0x101a:      0x8000
   0x101c:      unimp
   0x101e:      unimp

```

+ `AUIPC t0,0x0`

  Add Upper Immediate PC: rd = pc + imm[31:12]

  ![image-20230305084618770](https://wendaocsmaster.github.io/images/blog/image-20230305084618770.png)

  对当前PC的值和立即数0x0值得高20位相加后保存到寄存器`t0`,可以看到立寄存器的值为4096

  ~~~asm
  (gdb) si
  0x0000000000001004 in ?? ()
  (gdb) p $t0
  $1 = 4096
  ~~~

+ `ADDI a2,t0,40`

  ADD Immeiate: rd = rs1 + imm[11:0]

  ![image-20230305085957941](https://wendaocsmaster.github.io/images/blog/image-20230305085957941.png)

  将t0与40相加符号扩展的12位立即数加到寄存器a2

  ~~~
  (gdb) si   
  0x0000000000001008 in ?? ()
  (gdb) p $a2
  $4 = 4136
  ~~~

+ `csrr a0,mhartid`

  Control State Register Read: a0 = mhartid

  ![image-20230305092025012](https://wendaocsmaster.github.io/images/blog/image-20230305092025012.png)

  ![image-20230305091940893](https://wendaocsmaster.github.io/images/blog/image-20230305091940893.png)

  从mhartid中读出经过0扩展后写入a0

  ~~~
  0x000000000000100c in ?? ()
  (gdb) p $a0
  ~~~

+ `ld a1,32(t0)`

  从t0+32的内存中加载4B数据到a1,即从`0x1000 + 32 = 0x1020`中读取连续四字节内容存入a1寄存器，这里需要注意riscv是小端架构，因此实际存入a1的数据是`0x1023 0x1022 0x1021 0x1020`中的数据

  使用gdb查看这些位置的数据，以及寄存器a1中的数据

  ~~~
  (gdb) x/4bx 0x1020
  0x1020: 0x00    0x00    0x00    0x87
  (gdb) x/1bx 0x1023
  0x1023: 0x87
  (gdb) x/1wx 0x1020
  0x1020: 0x87000000
  (gdb) p/x $a1
  $6 = 0x87000000
  ~~~

+ `ld t0,24(t0)`

  从t0+32的内存中加载4B数据到t0，即从`0x1000 + 24 = 0x1018`中读取连续四字节内容存入a1寄存器，这里需要注意riscv是小端架构，因此实际存入a1的数据是`0x101b 0x101a 0x1019 0x1018`中的数据

  使用gdb查看这些位置的数据，以及寄存器t0中的数据

  ~~~
  (gdb) x/4bx 0x1018    
  0x1018: 0x00    0x00    0x00    0x80
  (gdb) x/1bx 0x101b
  0x101b: 0x80
  (gdb) x/1wx 0x1018
  0x1018: 0x80000000
  (gdb) p/x $t0
  $14 = 0x80000000
  ~~~

+ `jr t0`

  跳转到t0中的地址即`0x8000 0000`进入bootloader

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
