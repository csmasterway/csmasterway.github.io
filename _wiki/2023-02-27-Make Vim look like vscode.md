---
layout: post
title: Make Vim look like vscode
categories: [Linux]
description: Linux
keywords: Linux
---

## Configuration steps

1. Use the [proxy](https://wendaocsmaster.github.io/wiki/2023-02-27-Download the softwares fluently in the Linux system/) correctly.

2. Download **nvim.appimage**

3. Run

   ```
   chmod u+x nvim.appimage && ./nvim.appimage
   ```

   - If your system does not have FUSE you can

     extract the appimage

     ```
     ./nvim.appimage --appimage-extract
     ./squashfs-root/usr/bin/nvim
     ```

4. Configure global launch

   ~~~sh
   ln -s /path/to/nvim.appimage /usr/bin/nvim
   ~~~

   Of course, we can realize this trait by adding it to the environment variables.

5.  Download [Jarvim](https://github.com/glepnir/jarvim/releases) which can generate a module   vim configuration like **VIM PRO!**

   ~~~sh
   curl -fLo  install.sh https://raw.githubusercontent.com/glepnir/jarvim/master/install.sh
   sh install.sh
   ./jarvim -g
   ~~~



## ERROR

The following error is possible.

~~~
Error detected while processing /root/.config/nvim/init.vim:
line 1:
E121: Undefined variable: execute
Press ENTER or type command to continue
~~~

To solve the problem, we can modify the file  init.vim according to the following content.

~~~
:execute 'source ' . fnamemodify(expand('<sfile>'), ':h') . '/core/core.vim'
~~~

## Reference

[Releases · neovim/neovim (github.com)](https://github.com/neovim/neovim/releases)

https://github.com/glepnir/jarvim

https://www.bilibili.com/video/BV1mh411d7qj/?spm_id_from=333.999.0.0&vd_source=de4eb17f4c05b07e2288f16c09f1f513

[ThinkVim: A dark powerd nvim config (gitee.com)](https://toscode.gitee.com/modao233/ThinkVim)
