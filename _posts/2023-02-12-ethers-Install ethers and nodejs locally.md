---
layout: post
title: How to install ethers and nodejs locally
categories: [Blog, ethers, js]
description: start to learn smart contracts 
keywords:ethers
---

1. 安装[nodejs](https://nodejs.org/zh-cn/download/)

2. 安装ethers包5.4版本，最新版本修改了api

   ~~~
   npm install --save@5.4 ethers 
   ~~~

3. 创建js文件并初始化

   ~~~
   npm init -y
   ~~~

4.  在package.json中添加

   ~~~
   "type":"module"
   ~~~

   否则会出现import错误

