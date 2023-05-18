---
layout: post
title: Download the softwares fluently in the Linux system
categories: [Linux]
description: Linux
keywords: Linux
---

## Preconditions

- Some softwares such as v-2-r-a-y , x-r-a-y and so on, generally we can find these softwares on the GitHub site without proxy.
-  A Proxy server.

## Steps

- Set up the profile for the proxy client, to decrease the cost of studying, we can generate the profile by using the Q-2-v-r-a-y which is a GUI client.

- Use the following command to start it

  ```
  path/to/x-r-a-y -c /path/to/profile/conf.json
  ```

- Configure the environment variables for the terminal, add following lines to the file '~/.bashrc'

  ~~~
  export all_proxy=socks5://127.0.0.1:port
  # http access
  export https_proxy=http://127.0.0.1:port
  # https access
  export http_proxy=http://127.0.0.1:port
  ~~~

## Test

We can test if the client works correctly by using the following command.

~~~
curl cip.cc
~~~

If the return result is not your real IP address, it works correctly and then you can download the softwares from any international sites.

## Security measure when we use domestic cloud services

To avoid any risks, when we don't need the proxy, we should shutdown the client instantly, and delete the software from the cloud server. However, it is troublesome if we use it frequently, so we can package files as an encrypted file.

~~~
# encrypt
tar -czvf - /path/to/folder | openssl enc -aes-256-cbc -salt -out backup.tar.gz.enc
# decrypt
openssl enc -d -aes-256-cbc -in backup.tar.gz.enc | tar xzvf -
~~~

