---
layout: post
title: Linux用户管理
categories: [Linux]
description: Linux
keywords: Linux
---

# 用户管理

## 查看用户

~~~shell
who am i
# 或者
who mom lisks
~~~

![3-2.1-1](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pngdocument-uid735639labid3timestamp1531731170296.png)

输出的第一列表示打开当前伪终端的用户的用户名（要查看当前登录用户的用户名，去掉空格直接使用 `whoami` 即可），第二列的 `pts/0` 中 `pts` 表示伪终端，所谓伪是相对于 `/dev/tty` 设备而言的，终端本质上是对应着 Linux 上的 /dev/tty 设备，Linux 的多用户登录就是通过不同的 /dev/tty 设备完成的，Linux 默认提供了 6 个纯命令行界面的 “terminal”（准确的说这里应该是 6 个 virtual consoles）来让用户登录。在物理机系统上你可以通过使用`[Ctrl]`+`[Alt]`+`[F1]～[F6]`进行切换，当切换到其中一个终端后想要切换回图形面,可以按下`[Ctrl]`+`[Alt]`+`[F7]`来完成。

还有一点需要注意的是，在某些环境中 `who am i` 和 `who mom likes` 命令不会输出任何内容，这是因为当前使用的 Shell 不是登录式 Shell（login shell），没有用户与 `who` 的 stdin 相关联，因此不会输出任何内容。登录 Shell 是指用户使用自己的 user ID 登录交互式 shell 的第一个进程，判断是不是登录 Shell 可以执行 `echo $0` 命令，如果返回 zsh、/bin/zsh、/bin/bash 这种格式，说明是非登录式 Shell（non-login shell）；如果返回 -zsh、-bash 则说明是登录式 Shell，这时你执行 `who am i` 就会有输出。

例如在本地的 Ubuntu 系统上输入这个命令就不会有提示。

![图片描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pnguid871732-20200302-1583140204433.png)

`who` 命令其它常用参数

| 参数 | 说明                       |
| ---- | -------------------------- |
| `-a` | 打印能打印的全部           |
| `-d` | 打印死掉的进程             |
| `-m` | 同`am i`，`mom likes`      |
| `-q` | 打印当前登录用户数及用户名 |
| `-u` | 打印当前登录用户登录信息   |
| `-r` | 打印运行等级               |

## 创建用户

在 Linux 系统里， `root` 账户拥有整个系统至高无上的权限，比如新建和添加用户。

>root 权限，系统权限的一种，与 SYSTEM 权限可以理解成一个概念，但高于 Administrator 权限，root 是 Linux 和 UNIX 系统中的超级管理员用户帐户，该帐户拥有整个系统至高无上的权力，所有对象他都可以操作，所以很多黑客在入侵系统的时候，都要把权限提升到 root 权限，这个操作等同于在 Windows 下就是将新建的非法帐户添加到 Administrators 用户组。更比如安卓操作系统中（基于 Linux 内核）获得 root 权限之后就意味着已经获得了手机的最高权限，这时候你可以对手机中的任何文件（包括系统文件）执行所有增、删、改、查的操作。

大部分 Linux 系统在安装时都会建议用户新建一个用户而不是直接使用 root 用户进行登录，当然也有直接使用 root 登录的例如 Kali（基于 Debian 的 Linux 发行版，集成大量工具软件，主要用于数字取证的操作系统）。一般我们登录系统时都是以普通账户的身份登录的，要创建用户需要 root 权限，这里就要用到 `sudo` 这个命令了。不过使用这个命令有两个大前提，一是你要知道当前登录用户的密码，二是当前用户必须在 `sudo` 用户组。shiyanlou 用户也属于 sudo 用户组（稍后会介绍如何查看和添加用户组）。

大部分 Linux 系统在安装时都会建议用户新建一个用户而不是直接使用 root 用户进行登录，当然也有直接使用 root 登录的例如 Kali（基于 Debian 的 Linux 发行版，集成大量工具软件，主要用于数字取证的操作系统）。一般我们登录系统时都是以普通账户的身份登录的，要创建用户需要 root 权限，这里就要用到 `sudo` 这个命令了。不过使用这个命令有两个大前提，一是你要知道当前登录用户的密码，二是当前用户必须在 `sudo` 用户组。shiyanlou 用户也属于 sudo 用户组（稍后会介绍如何查看和添加用户组）。

现在我们新建一个叫 `lilei` 的用户：

```bash
sudo adduser lilei
```

实验楼的环境目前设置为 shiyanlou 用户执行 sudo 不需要输入密码，通常此处需要按照提示输入 shiyanlou 密码（**Linux 下密码输入是不显示任何内容的，shiyanlou 用户密码可以在右侧环境信息里查看，请勿自行设置密码**）。然后是给 lilei 用户设置密码，后面的选项的一些内容你可以选择直接回车使用默认值。

![3-2.2-1](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pngdocument-uid735639labid3timestamp1531731216215.png)

这个命令不但可以添加用户到系统，同时也会默认为新用户在 /home 目录下创建一个工作目录：

```bash
ls /home
```

![图片描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pnguid871732-20200302-1583141675216.png)

现在你已经创建好一个用户，并且你可以使用你创建的用户登录了，使用如下命令切换登录用户：

```bash
su -l lilei
```

输入刚刚设置的 lilei 的密码，然后输入如下命令并查看输出：

```bash
who am i
whoami
pwd
```

![图片描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pnguid871732-20200302-1583142076115.png)

你发现了区别了吗？这就是上一小节我们讲到的 `who am i` 和 `whoami` 命令的区别。

退出当前用户跟退出终端一样，可以使用 `exit` 命令或者使用快捷键 `Ctrl+D`。

![图片描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pnguid871732-20200302-1583142261959.png)

## 用户组

在 Linux 里面每个用户都有一个归属（用户组），用户组简单地理解就是一组用户的集合，它们共享一些资源和权限，同时拥有私有资源，就跟家的形式差不多，你的兄弟姐妹（不同的用户）属于同一个家（用户组），你们可以共同拥有这个家（共享资源），爸妈对待你们都一样（共享权限），你偶尔写写日记，其他人未经允许不能查看（私有资源和权限）。当然一个用户是可以属于多个用户组的，正如你既属于家庭，又属于学校或公司。

在 Linux 里面如何知道自己属于哪些用户组呢？

#### 方法一：使用 groups 命令

```bash
groups shiyanlou
```

![此处输入图片的描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pngdocument-uid13labid3timestamp1454035714557.jpeg)

其中冒号之前表示用户，后面表示该用户所属的用户组。这里可以看到 shiyanlou 用户属于 shiyanlou 用户组，每次新建用户如果不指定用户组的话，默认会自动创建一个与用户名相同的用户组（差不多就相当于家长的意思）。

默认情况下在 sudo 用户组里的可以使用 sudo 命令获得 root 权限。shiyanlou 用户也可以使用 sudo 命令，为什么这里没有显示在 sudo 用户组里呢？可以查看下 `/etc/sudoers.d/shiyanlou` 文件，我们在 `/etc/sudoers.d` 目录下创建了这个文件，从而给 shiyanlou 用户赋予了 sudo 权限：

![此处输入图片的描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pngdocument-uid13labid3timestamp1454035855554.jpeg)

#### 方法二：查看 `/etc/group` 文件

```bash
cat /etc/group | sort
```

这里 `cat` 命令用于读取指定文件的内容并打印到终端输出，后面会详细讲它的使用。 `| sort` 表示将读取的文本进行一个字典排序再输出，然后你将看到如下一堆输出，你可以在最下面看到 shiyanlou 的用户组信息：

![3-2.3-3](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pngdocument-uid735639labid3timestamp1531731335264.png)

没找到？没关系，你可以使用 `grep` 命令过滤掉一些你不想看到的结果：

```bash
cat /etc/group | grep -E "shiyanlou"
```

![此处输入图片的描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pngdocument-uid13labid3timestamp1454035698068.jpeg)

##### `/etc/group` 文件格式说明

/etc/group 的内容包括用户组（Group）、用户组口令、GID（组 ID） 及该用户组所包含的用户（User），每个用户组一条记录。格式如下：

> group_name:password:GID:user_list

你看到上面的 password 字段为一个 `x`，并不是说密码就是它，只是表示密码不可见而已。

这里需要注意，如果用户的 GID 等于用户组的 GID，那么最后一个字段 `user_list` 就是空的，这里的 GID 是指用户默认所在组的 GID，可以使用 `id` 命令查看。比如 shiyanlou 用户，在 `/etc/group` 中的 shiyanlou 用户组后面是不会显示的。lilei 用户，在 `/etc/group` 中的 lilei 用户组后面是不会显示的。

#### 将其它用户加入 sudo 用户组

默认情况下新创建的用户是不具有 root 权限的，也不在 sudo 用户组，可以让其加入 sudo 用户组从而获取 root 权限：

```bash
# 注意 Linux 上输入密码是不会显示的
su -l lilei
sudo ls
```

![图片描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pnguid871732-20200302-1583145040679.png)

会提示 lilei 不在 sudoers 文件中，意思就是 lilei 不在 sudo 用户组中，至于 sudoers 文件（/etc/sudoers）你现在最好不要动它，操作不慎会导致比较麻烦的后果。

使用 `usermod` 命令可以为用户添加用户组，同样使用该命令你必需有 root 权限，你可以直接使用 root 用户为其它用户添加用户组，或者用其它已经在 sudo 用户组的用户使用 sudo 命令获取权限来执行该命令。

这里我用 shiyanlou 用户执行 sudo 命令将 lilei 添加到 sudo 用户组，让它也可以使用 sudo 命令获得 root 权限，首先我们切换回 shiyanlou 用户。

```bash
su - shiyanlou
```

当然也可以通过 `sudo passwd shiyanlou` 进行设置，或者你直接关闭当前终端打开一个新的终端。

```bash
groups lilei
sudo usermod -G sudo lilei
groups lilei
```

![图片描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pnguid871732-20200302-1583145514469.png)

然后你再切换回 lilei 用户，现在就可以使用 sudo 获取 root 权限了。

![图片描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pnguid871732-20200302-1583145591124.png)

删除用户是很简单的事：

```bash
sudo deluser lilei --remove-home
```

![3-2.4-1](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pngdocument-uid735639labid3timestamp1531731417990.png)

使用 `--remove-home` 参数在删除用户时候会一并将该用户的工作目录一并删除。如果不使用那么系统会自动在 /home 目录为该用户保留工作目录。

![图片描述](https://wendaocsmaster.github.io/images/blog/image-20230225103214468.pnguid871732-20200302-1583146790310.png)

删除用户组可以使用 `groupdel` 命令，倘若该群组中仍包括某些用户，则必须先删除这些用户后，才能删除群组。

## Reference

https://www.lanqiao.cn/courses/1/learning/?id=3&compatibility=true
