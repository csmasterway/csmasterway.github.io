---
layout: post
title: Linux文件打包与解压缩
categories: [Linux]
description: Linux
keywords: Linux
---

# 文件打包与解压缩

在讲 Linux 上的压缩工具之前，有必要先了解一下常见常用的压缩包文件格式。在 Windows 上最常见的不外乎这两种 `*.zip`，`*.7z` 后缀的压缩文件。而在 Linux 上面常见的格式除了以上两种外，还有 `.rar`，`*.gz`，`*.xz`，`*.bz2`，`*.tar`，`*.tar.gz`，`*.tar.xz`，`*.tar.bz2`，简单介绍如下：

| 文件后缀名 | 说明                           |
| ---------- | ------------------------------ |
| `*.zip`    | zip 程序打包压缩的文件         |
| `*.rar`    | rar 程序压缩的文件             |
| `*.7z`     | 7zip 程序压缩的文件            |
| `*.tar`    | tar 程序打包，未压缩的文件     |
| `*.gz`     | gzip 程序（GNU zip）压缩的文件 |
| `*.xz`     | xz 程序压缩的文件              |
| `*.bz2`    | bzip2 程序压缩的文件           |
| `*.tar.gz` | tar 打包，gzip 程序压缩的文件  |
| `*.tar.xz` | tar 打包，xz 程序压缩的文件    |
| `*tar.bz2` | tar 打包，bzip2 程序压缩的文件 |
| `*.tar.7z` | tar 打包，7z 程序压缩的文件    |

讲了这么多种压缩文件，这么多个命令，不过我们一般只需要掌握几个命令即可，包括 `zip`，`tar`。下面会依次介绍这几个命令及对应的解压命令。

## 使用ZIP打包程序

- 使用 zip 打包文件夹，注意输入完整的参数和路径：

```bash
cd /home/shiyanlou
zip -r -q -o shiyanlou.zip /home/shiyanlou/Desktop
du -h shiyanlou.zip
file shiyanlou.zip
```

上面命令将目录 `/home/shiyanlou/Desktop` 打包成一个文件，并查看了打包后文件的大小和类型。第一行命令中，`-r` 参数表示递归打包包含子目录的全部内容，`-q` 参数表示为安静模式，即不向屏幕输出信息，`-o`，表示输出文件，需在其后紧跟打包输出文件名。后面使用 `du` 命令查看打包后文件的大小（后面会具体说明该命令）。

- 设置压缩级别为 9 和 1（9 最大，1 最小），重新打包：

```bash
zip -r -9 -q -o shiyanlou_9.zip /home/shiyanlou/Desktop -x ~/*.zip
zip -r -1 -q -o shiyanlou_1.zip /home/shiyanlou/Desktop -x ~/*.zip
```

这里添加了一个参数用于设置压缩级别 `-[1-9]`，1 表示最快压缩但体积大，9 表示体积最小但耗时最久。最后那个 `-x` 是为了排除我们上一次创建的 zip 文件，否则又会被打包进这一次的压缩文件中，**注意：这里只能使用绝对路径，否则不起作用**。

我们再用 `du` 命令分别查看默认压缩级别、最低、最高压缩级别及未压缩的文件的大小：

```bash
du -h -d 0 *.zip ~ | sort
```

通过 man 手册可知：

- `-h`， --human-readable（顾名思义，你可以试试不加的情况）
- `-d`， --max-depth（所查看文件的深度）

![图片描述](https://wendaocsmaster.github.io/images/blog/uid600404-20190428-1556438181236.jpeg)

这样一目了然，理论上来说默认压缩级别应该是最高的，但是由于文件不大，这里的差异不明显（几乎看不出差别），不过你在环境中操作之后看到的压缩文件大小可能跟图上的有些不同，因为系统在使用过程中，会随时生成一些缓存文件在当前用户的家目录中，这对于我们学习命令使用来说，是无关紧要的，可以忽略这些不同。

- 创建加密 zip 包

使用 `-e` 参数可以创建加密压缩包：

```bash
zip -r -e -o shiyanlou_encryption.zip /home/shiyanlou/Desktop
```

**注意：** 关于 `zip` 命令，因为 Windows 系统与 Linux/Unix 在文本文件格式上的一些兼容问题，比如换行符（为不可见字符），在 Windows 为 CR+LF（Carriage-Return+Line-Feed：回车加换行），而在 Linux/Unix 上为 LF（换行），所以如果在不加处理的情况下，在 Linux 上编辑的文本，在 Windows 系统上打开可能看起来是没有换行的。如果你想让你在 Linux 创建的 zip 压缩文件在 Windows 上解压后没有任何问题，那么你还需要对命令做一些修改：

```bash
zip -r -l -o shiyanlou.zip /home/shiyanlou/Desktop
```

需要加上 `-l` 参数将 `LF` 转换为 `CR+LF` 来达到以上目的。

## 使用UNZIP解压

将 `shiyanlou.zip` 解压到当前目录：

```bash
unzip shiyanlou.zip
```

使用安静模式，将文件解压到指定目录：

```bash
unzip -q shiyanlou.zip -d ziptest
```

上述指定目录不存在，将会自动创建。如果你不想解压只想查看压缩包的内容你可以使用 `-l` 参数：

```bash
unzip -l shiyanlou.zip
```

**注意：** 使用 unzip 解压文件时我们同样应该注意兼容问题，不过这里我们关心的不再是上面的问题，而是中文编码的问题，通常 Windows 系统上面创建的压缩文件，如果有有包含中文的文档或以中文作为文件名的文件时默认会采用 GBK 或其它编码，而 Linux 上面默认使用的是 UTF-8 编码，如果不加任何处理，直接解压的话可能会出现中文乱码的问题（有时候它会自动帮你处理），为了解决这个问题，我们可以在解压时指定编码类型。

使用 `-O`（英文字母，大写 o）参数指定编码类型：

```bash
unzip -O GBK 中文压缩文件.zip
```

## 使用tar打包工具

在 Linux 上面更常用的是 `tar` 工具，tar 原本只是一个打包工具，只是同时还是实现了对 7z、gzip、xz、bzip2 等工具的支持，这些压缩工具本身只能实现对文件或目录（单独压缩目录中的文件）的压缩，没有实现对文件的打包压缩，所以我们也无需再单独去学习其他几个工具，tar 的解压和压缩都是同一个命令，只需参数不同，使用比较方便。

下面先掌握 `tar` 命令一些基本的使用方式，即不进行压缩只是进行打包（创建归档文件）和解包的操作。

- 创建一个 tar 包：

```bash
cd /home/shiyanlou
tar -P -cf shiyanlou.tar /home/shiyanlou/Desktop
```

上面命令中，`-P` 保留绝对路径符，`-c` 表示创建一个 tar 包文件，`-f` 用于指定创建的文件名，注意文件名必须紧跟在 `-f` 参数之后，比如不能写成 `tar -fc shiyanlou.tar`，可以写成 `tar -f shiyanlou.tar -c ~`。你还可以加上 `-v` 参数以可视的的方式输出打包的文件。

- 解包一个文件（`-x` 参数）到指定路径的**已存在**目录（`-C` 参数）：

```bash
mkdir tardir
tar -xf shiyanlou.tar -C tardir
```

- 只查看不解包文件 `-t` 参数：

```bash
tar -tf shiyanlou.tar
```

- 保留文件属性和跟随链接（符号链接或软链接），有时候我们使用 tar 备份文件当你在其他主机还原时希望保留文件的属性（`-p` 参数）和备份链接指向的源文件而不是链接本身（`-h` 参数）：

```bash
tar -cphf etc.tar /etc
```

对于创建不同的压缩格式的文件，对于 tar 来说是相当简单的，需要的只是换一个参数，这里我们就以使用 `gzip` 工具创建 `*.tar.gz` 文件为例来说明。

- 我们只需要在创建 tar 文件的基础上添加 `-z` 参数，使用 `gzip` 来压缩文件：

```bash
tar -czf shiyanlou.tar.gz /home/shiyanlou/Desktop
```

- 解压 `*.tar.gz` 文件：

```bash
tar -xzf shiyanlou.tar.gz
```

![此处输入图片的描述](https://wendaocsmaster.github.io/images/blog/document-uid735639labid61timestamp1532339561961.png)

现在我们要使用其它的压缩工具创建或解压相应文件只需要更改一个参数即可：

| 压缩文件格式 | 参数 |
| ------------ | ---- |
| `*.tar.gz`   | `-z` |
| `*.tar.xz`   | `-J` |
| `*tar.bz2`   | `-j` |

> tar 命令的参数很多，不过常用的就是上述这些，需要了解更多你可以查看 man 手册获取帮助。

说了这么多，其实平常使用的参数并没有那么复杂，只需要记住常用的组合就可以了。 常用命令：

- zip：
- 打包 ：zip something.zip something （目录请加 -r 参数）
- 解包：unzip something.zip
- 指定路径：-d 参数
- tar：
- 打包：tar -cf something.tar something
- 解包：tar -xf something.tar
- 指定路径：-C 参数

## Reference

https://www.lanqiao.cn/courses/1/learning/?id=3&compatibility=true
