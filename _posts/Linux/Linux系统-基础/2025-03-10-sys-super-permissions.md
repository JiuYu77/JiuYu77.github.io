---
title: Linux超级权限
description: 文件权限、文件操作。
author: yu
date: 2025-03-10 12:50:54 +0800
categories: [Linux, Linux系统-基础]
tags: [Linux]
---

## 超级权限

超级权限，是超级用户权限（root权限）的简称。

## su

**`su`** 命令用于切换当前用户。必须要有目标用户的“密码”才能切换过去，否则切换不成功。

```shell
格式：
su username   # 示例：su root

特殊用法：
# 以下两个命令作用都是切换到root用户，而不需要输入root
su
su -
```

## sudo

**`sudo`** 命令用于临时获得root权限。

使用sudo执行一个只有root用户才能执行的操作，但是“需要密码”，这里的密码不是root的密码，而是用户自己的密码。这里用户只是临时获得了root的权限，几分钟后，root权限自动消失，这样就解决了安全性问题。

```shell
sudo 要执行的命令
```

## sudoers
`/etc/sudoers` 文件

> Linux添加root权限：is not in the sudoers file解决方法
当我们使用sudo命令切换用户的时候可能会遇到提示以下错误：**用户名 is not in the sudoers file.**
**原因：**当前的用户没有加入到sudo的配置文件里

```shell
sudo cat /etc/sudoers
sudo vim /etc/sudoers

# 以下三行使用其中一个就行
用户名  ALL=(ALL:ALL) NOPASSWD: ALL
用户名  ALL=(ALL:ALL) ALL
用户名  ALL=(ALL) ALL
```
