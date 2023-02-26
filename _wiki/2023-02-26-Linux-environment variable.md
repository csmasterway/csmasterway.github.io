---
layout: post
title: Linux环境变量
categories: [Linux]
description: Linux
keywords: Linux
---

## 环境变量

简单理解了变量的概念，就很容易理解环境变量了。环境变量的作用域比自定义变量的要大，如 Shell 的环境变量作用于自身和它的子进程。在所有的 UNIX 和类 UNIX 系统中，每个进程都有其各自的环境变量设置，且默认情况下，当一个进程被创建时，除了创建过程中明确指定的话，它将继承其父进程的绝大部分环境设置。Shell 程序也作为一个进程运行在操作系统之上，而我们在 Shell 中运行的大部分命令都将以 Shell 的子进程的方式运行。

![1](https://wendaocsmaster.github.io/images/blog/5-2.png)

通常我们会涉及到的变量类型有三种：

- 当前 Shell 进程私有用户自定义变量，如上面我们创建的 tmp 变量，只在当前 Shell 中有效。
- Shell 本身内建的变量。
- 从自定义变量导出的环境变量。

也有三个与上述三种环境变量相关的命令：`set`，`env`，`export`。这三个命令很相似，都是用于打印环境变量信息，区别在于涉及的变量范围不同。详见下表：

| 命 令    | 说 明                                                        |
| -------- | ------------------------------------------------------------ |
| `set`    | 显示当前 Shell 所有变量，包括其内建环境变量（与 Shell 外观等相关），用户自定义变量及导出的环境变量。 |
| `env`    | 显示与当前用户相关的环境变量，还可以让命令在指定环境中运行。 |
| `export` | 显示从 Shell 中导出成环境变量的变量，也能通过它将自定义变量导出为环境变量。 |

![1](https://wendaocsmaster.github.io/images/blog/5-3.png)

你可以更直观的使用 `vimdiff` 工具比较一下它们之间的差别：

```bash
temp=shiyanlou
export temp_env=shiyanlou
env|sort>env.txt
export|sort>export.txt
set|sort>set.txt
```

上述操作将命令输出通过管道 `|` 使用 `sort` 命令排序，再重定向到对象文本文件中。管道的概念后面我们会学到，现在你知道这是什么意思就行了。

```bash
vimdiff env.txt export.txt set.txt
```

使用 `vimdiff` 工具比较导出的几个文件的内容，退出 `vimdiff` 需要按下 Esc 后输入 `:q` 即可退出。

![1](https://wendaocsmaster.github.io/images/blog/document-uid735639labid60timestamp1532339264870.gif)

关于哪些变量是环境变量，可以简单地理解成在当前进程的子进程有效则为环境变量，否则不是（有些人也将所有变量统称为环境变量，只是以全局环境变量和局部环境变量进行区分，我们只要理解它们的实质区别即可）。我们这里用 `export` 命令来体会一下，先在 Shell 中设置一个变量 `temp=shiyanlou`，然后再新创建一个子 Shell 查看 `temp` 变量的值：

![此处输入图片的描述](https://wendaocsmaster.github.io/images/blog/document-uid735639labid60timestamp1532339293501.png)

**注意：为了与普通变量区分，通常我们习惯将环境变量名设为大写。**

#### 永久生效

但是问题来了，当你关机后，或者关闭当前的 shell 之后，环境变量就没了啊。怎么才能让环境变量永久生效呢？

按变量的生存周期来划分，Linux 变量可分为两类：

1. 永久的：需要修改配置文件，变量永久生效；
2. 临时的：使用 export 命令行声明即可，变量在关闭 shell 时失效。

这里介绍两个重要文件 `/etc/bashrc`（有的 Linux 没有这个文件） 和 `/etc/profile` ，它们分别存放的是 shell 变量和环境变量。还有要注意区别的是每个用户目录下的一个隐藏文件：

```bash
# .profile 可以用 ls -a 查看
cd /home/shiyanlou
ls -a
```

![图片描述](https://wendaocsmaster.github.io/images/blog/uid871732-20200303-1583220161661.png)

这个 .profile 只对当前用户永久生效。因为它保存在当前用户的 Home 目录下，当切换用户时，工作目录可能一并被切换到对应的目录中，这个文件就无法生效。而写在 `/etc/profile` 里面的是对所有用户永久生效，所以如果想要添加一个永久生效的环境变量，只需要打开 `/etc/profile`，在最后加上你想添加的环境变量就好啦。

## 命令的查找路径与顺序

你可能很早之前就有疑问，我们在 Shell 中输入一个命令，Shell 是怎么知道去哪找到这个命令然后执行的呢？这是通过环境变量 `PATH` 来进行搜索的，熟悉 Windows 的用户可能知道 Windows 中的也是有这么一个 PATH 环境变量。这个 `PATH` 里面就保存了 Shell 中执行的命令的搜索路径。

查看 `PATH` 环境变量的内容：

```bash
echo $PATH
```

默认情况下你会看到如下输出：

```bash
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
```

如果你还记得 Linux 目录结构那一节的内容，你就应该知道上面这些目录下放的是哪一类文件了。通常这一类目录下放的都是可执行文件，当我们在 Shell 中执行一个命令时，系统就会按照 PATH 中设定的路径按照顺序依次到目录中去查找，如果存在同名的命令，则执行先找到的那个。

> 下面我们将练习创建一个最简单的可执行 Shell 脚本和一个使用 C 语言创建的“ hello world ”程序，如果这两部分内容你之前没有学习过，那么你可以进行一个入门学习：

- [C 语言入门教程](https://www.lanqiao.cn/courses/57)
- [高级 Bash 脚本编程指南](https://www.lanqiao.cn/courses/5)
- [Linux Shell Scripting Tutorial (LSST) v2.0](http://bash.cyberciti.biz/guide/Main_Page)

创建一个 Shell 脚本文件，你可以使用 gedit，vim，sublime 等工具编辑。如果你是直接复制的话，建议使用 gedit 或者 sublime，否则可能导致代码缩进混乱。

```bash
cd /home/shiyanlou
touch hello_shell.sh
gedit hello_shell.sh
```

在脚本中添加如下内容，保存并退出。

**注意不要省掉第一行，这不是注释，有用户反映有语法错误，就是因为没有了第一行。**

```bash
#!/bin/bash

for ((i=0; i<10; i++));do
    echo "hello shell"
done

exit 0
```

为文件添加可执行权限，否则执行会报错没有权限：

```bash
chmod 755 hello_shell.sh
```

执行脚本：

```bash
cd /home/shiyanlou
./hello_shell.sh
```

创建一个 C 语言 `hello world` 程序：

```bash
cd /home/shiyanlou
gedit hello_world.c
```

输入如下内容，同样不能省略第一行。

```c
#include <stdio.h>

int main(void)
{
    printf("hello world!\n");
    return 0;
}
```

保存后使用 gcc 生成可执行文件：

```bash
gcc -o hello_world hello_world.c
```

**gcc 生成二进制文件默认具有可执行权限，不需要修改。**

在 `/home/shiyanlou` 家目录创建一个 `mybin` 目录，并将上述 `hello_shell.sh` 和 `hello_world` 文件移动到其中：

```bash
cd /home/shiyanlou
mkdir mybin
mv hello_shell.sh hello_world mybin/
```

现在你可以在 `mybin` 目录中分别运行你刚刚创建的两个程序：

```bash
cd mybin
./hello_shell.sh
./hello_world
```

![此处输入图片的描述](https://wendaocsmaster.github.io/images/blog/document-uid735639labid60timestamp1532339433567.png)

回到上一级目录，也就是 `shiyanlou` 家目录，当再想运行那两个程序时，会发现提示命令找不到，除非加上命令的完整路径，但那样很不方便，如何做到像使用系统命令一样执行自己创建的脚本文件或者程序呢？那就要将命令所在路径添加到 `PATH` 环境变量了。

## 添加自定义路径到“ PATH ”环境变量

在前面我们应该注意到 `PATH` 里面的路径是以 `:` 作为分割符的，所以我们可以这样添加自定义路径：

```bash
PATH=$PATH:/home/shiyanlou/mybin
```

**注意这里一定要使用绝对路径。**

现在你就可以在任意目录执行那两个命令了（注意需要去掉前面的 `./`）。你可能会意识到这样还并没有很好的解决问题，因为我给 PATH 环境变量追加了一个路径，它也只是在当前 Shell 有效，我一旦退出终端，再打开就会发现又失效了。有没有方法让添加的环境变量全局有效？或者每次启动 Shell 时自动执行上面添加自定义路径到 PATH 的命令？下面我们就来说说后一种方式——让它自动执行。

在每个用户的 home 目录中有一个 Shell 每次启动时会默认执行一个配置脚本，以初始化环境，包括添加一些用户自定义环境变量等等。实验楼的环境使用的 Shell 是 zsh，它的配置文件是 `.zshrc`，相应的如果使用的 Shell 是 Bash，则配置文件为 `.bashrc`。它们在 `etc` 下还都有一个或多个全局的配置文件，不过我们一般只修改用户目录下的配置文件。Shell 的种类有很多，可以使用 `cat /etc/shells` 命令查看当前系统已安装的 Shell。

![图片描述](https://wendaocsmaster.github.io/images/blog/uid871732-20200303-1583223324878.png)

我们可以简单地使用下面命令直接添加内容到 `.zshrc` 中：

```bash
echo "PATH=$PATH:/home/shiyanlou/mybin" >> .zshrc
```

**上述命令中 `>>` 表示将标准输出以追加的方式重定向到一个文件中，注意前面用到的 `>` 是以覆盖的方式重定向到一个文件中，使用的时候一定要注意分辨。在指定文件不存在的情况下都会创建新的文件。**

## 修改和删除已有变量

#### 变量修改

变量的修改有以下几种方式：

| 变量设置方式                   | 说明                                         |
| ------------------------------ | -------------------------------------------- |
| `${变量名#匹配字串}`           | 从头向后开始匹配，删除符合匹配字串的最短数据 |
| `${变量名##匹配字串}`          | 从头向后开始匹配，删除符合匹配字串的最长数据 |
| `${变量名%匹配字串}`           | 从尾向前开始匹配，删除符合匹配字串的最短数据 |
| `${变量名%%匹配字串}`          | 从尾向前开始匹配，删除符合匹配字串的最长数据 |
| `${变量名/旧的字串/新的字串}`  | 将符合旧字串的第一个字串替换为新的字串       |
| `${变量名//旧的字串/新的字串}` | 将符合旧字串的全部字串替换为新的字串         |

比如我们可以修改前面添加到 PATH 的环境变量，将添加的 mybin 目录从环境变量里删除。为了避免操作失误导致命令找不到，我们先将 PATH 赋值给一个新的自定义变量 mypath：

```bash
mypath=$PATH
echo $mypath
mypath=${mypath%/home/shiyanlou/mybin}
# 或使用通配符 * 表示任意多个任意字符
mypath=${mypath%*/mybin}
```

可以看到路径已经不存在了。

![图片描述](https://wendaocsmaster.github.io/images/blog/uid871732-20200303-1583224278229.png)

#### 变量删除

可以使用 `unset` 命令删除一个环境变量：

```bash
unset mypath
```

![图片描述](https://wendaocsmaster.github.io/images/blog/uid871732-20200303-1583224422523.png)

## 环境变量立即生效

前面我们在 Shell 中修改了一个配置脚本文件之后（比如 zsh 的配置文件 home 目录下的 `.zshrc`），每次都要退出终端重新打开甚至重启主机之后其才能生效，很是麻烦，我们可以使用 `source` 命令来让其立即生效，如：

```bash
cd /home/shiyanlou
source .zshrc
```

`source` 命令还有一个别名就是 `.`，上面的命令如果替换成 `.` 的方式就该是：

```bash
. ./.zshrc
```

![图片描述](https://wendaocsmaster.github.io/images/blog/uid871732-20200303-1583225164807.png)

在使用 `.` 的时候，需要注意与表示当前路径的那个点区分开。

注意第一个点后面有一个空格，而且后面的文件必须指定完整的绝对或相对路径名，`source` 则不需要。

## Reference

https://www.lanqiao.cn/courses/1/learning/?id=3&compatibility=true
