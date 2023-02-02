---
layout: post
title: FTP服务器搭建
categories: [Blog, FTP, vsftpd]
description: FTP server set up
keywords: FTP, vsftpd
---

## 系统环境 Centos 7.2

## 1.安装vsftpd

```
1.使用yum下载vsftpd
yum install -y vsftpd

2.启动vsftpd服务并且设置开机自启动
systemctl start vsftpd
systemctl enable vsftpd 
如果启动服务失败，查看是否有进程占用端口号20-21，若有进程占用端口号，kill -9 [pid]终止进程
```

## 2.配置vsftpd

vsftpd安装后默认开启匿名FTP匿名功能，使用匿名FTP，用户无需输入用户名和密码即可登录FTP服务器，但是没有权限修改或者上传文件，用户如果试图使用Linux系统中的账号登录服务器，会被vsftpd拒绝，但是可以在vsftpd里面配置用户名和登录密码登录ftp服务器。

```
1.在系统中创建用户用于管理ftp服务器
useradd ftpadmin
2.为ftpadmin设置密码
passwd ftpadmin
3.将用户的家目录作为ftp服务器的根目录，若是用其他账户创建的文件夹作为ftp服务器的根目录，还需要修改文件目录的所有者
chown -R ftpadmin:ftpadmin /file_path/
4.修改vsftpd.conf配置文件
打开文件 vi /etc/vsftpd/vsftpd.conf
可根据实际需求选择将FTP配置为主动模式或者被动模式。如果客户端无公网ip地址，要通过公网IP地址访问FTP服务器时，需要将FTP服务器配置为被动模式。

主动模式需要配置的参数如下：
#设置以下参数，不允许匿名登录FTP服务器，允许本地用户登录FTP服务器，并指定FTP本地用户使用的文件目录。
anonymous_enable=NO              #不允许匿名登录FTP服务器
local_enable=YES                 #允许本地用户登录FTP服务器
local_root=/var/ftp/work01       #FTP本地用户使用的文件目录

#设置以下参数，限制用户只能访问自身的主目录。
chroot_local_user=YES                      #所有用户都被限制在其主目录
chroot_list_enable=YES                     #启用例外用户名单
chroot_list_file=/etc/vsftpd/chroot_list   #例外用户名单
allow_writeable_chroot=YES                 


被动模式除了需要配置主动模式所需的所有参数外，还需要配置的参数如下：
#设置以下参数，配置FTP支持被动模式。并指定FTP服务器的公网IP地址，以及可供访问的端口范围，端口范围请根据实际环境进行设置。
listen=YES                    
listen_ipv6=NO                
pasv_address=xx.xx.xx.xx      #FTP服务器的公网IP地址
           
pasv_min_port=3000            #被动模式下的最小端口
pasv_max_port=3100            #被动模式下的最大端口

5.创建例外用户名单,“chroot_list”文件是限制在主目录下的例外用户名单。如果需要设置某个用户不受只可以访问其主目录的限制，可将对应的用户名写入该文件。如果没有例外也必须要有“chroot_list”文件，内容可为空。
touch /etc/vsftpd/chroot_list

6.重启vsftpd服务
systemctl start vsftpd

7.设置放行端口：主动模式的20-21端口以及被动模式下的端口范围，可以再服务器安全组入方向设置，也可以通过命令行设置。
iptables -I INPUT -p tcp --dport 80 -j ACCEPT
```

| FTP模式  | 方向   | 协议 | 端口                                                         | 源地址    |
| -------- | ------ | ---- | ------------------------------------------------------------ | --------- |
| 主动模式 | 入方向 | TCP  | 21端口（命令端口）、20端口（数据端口）                       | 0.0.0.0/0 |
| 被动模式 | 入方向 | TCP  | 21端口和配置文件“/etc/vsftpd/vsftpd.conf”中参数“pasv_min_port”和“pasv_max_port”之间的所有端口。 | 0.0.0.0/0 |
