---
title: SSH实现无密码远程登录
description: ssh生成密钥（公钥/私钥）、实现无密码SSH远程连接。
author: yu
date: 2025-04-18 21:47:32 +0800
categories: [Blogging, ssh]
tags: [SSH]
---

## 生成SSH密钥对

[生成密钥对](https://jiuyu77.github.io/posts/ssh-key/#%E7%94%9F%E6%88%90ssh%E5%AF%86%E9%92%A5%E5%AF%B9)

## 上传

将公钥(public key)上传到服务器（远程计算机）。

```shell
scp id_rsa.pub 用户名@hostname:~/.ssh/id_rsa2.pub
```
注意：服务器是否已存在同名文件id_rsa.pub，若存在可以将上传的公钥文件重命名，如上 **~/.ssh/id_rsa2.pub**。

## 追加

登录到服务器，将上传到服务器的本地公钥，追加到服务器的`authorized_keys`文件：
```shell
cd ~/.ssh
cat id_rsa2.pub >> authorized_keys
```
