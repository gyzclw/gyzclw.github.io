layout: pages
title: ssh——key 使用
date: 2015-12-10 10:14:32
tags: [linux 技术]
  
 
---

## ssh 连接服务器

使用该命令将密钥上传服务器
> cat ~/.ssh/id_rsa.pub | ssh root@45.55.1.163 "mkdir -p ~/.ssh && cat   ~/.ssh/authorized_keys"
