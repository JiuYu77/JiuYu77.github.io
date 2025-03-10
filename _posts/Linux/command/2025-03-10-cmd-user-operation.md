---
title: 用户操作
description: 创建用户、删除用户、修改用户名...。
author: yu
date: 2025-03-10 12:04:39 +0800
categories: [Linux, 命令]
tags: [Linux, command]
---

## 添加用户

添加用户：创建用户。

### useradd

`useradd` 命令是内置的 Linux 命令，在任何 Linux 系统中都可用。然而，使用这种低级别的工具创建用户是比较繁琐的，因为默认情况下，它**不会创建主目录和用户密码**。
```shell
useradd new_user
```
### adduser

`adduser` 不是标准的 Linux 命令，它本质上是一个 perl 脚本，在后台调用 useradd 命令。这个高级实用程序在 Linux 中创建用户的时候效率更高，它**提供了创建主目录、设置密码以及其他一些参数的选项**。
```shell
adduser new_user
```
[原文链接](https://blog.csdn.net/yaxuan88521/article/details/127628775)

## 删除用户

### deluser

|        参数        |               说明              |
|:------------------:|:------------------------------:|
| -\-backup           |删除前备份用户家目录和邮件数据     |
| -\-backup-to        |设置指定目录进行备份              |
| -\-group            |删除用户组                       |
| -\-quiet            |静默执行模式                     |
| -\-help             |显示帮助信息                     |
| -\-remove-all-files |删除用户的所有文件                |
| -\-remove-home      |删除用户的主目录和邮件后台处理程序 |
| -\-version          |显示版本信息                     |

```shell
# 删除指定的用户
deluser 用户名
# remove the user from a group
deluser 用户名 组名
# 删除指定的用户及相关所有文件
deluser --remove-all-files 用户

# 删除组
deluser --group 组名
```

### userdel
> **userdel** 命令的全称是user delete，意思是删除用户。它是一个低级的系统管理工具，用于从系统中移除用户账户和相关的文件，如用户的主目录，邮件目录，密码文件，影子密码文件等。userdel命令的行为受到**/etc/login.defs**文件中的设置的影响，可以通过修改该文件来改变userdel命令的默认行为。

```shell
# 显示userdel命令的帮助信息
userdel -h

# 删除用户账户，并删除用户的主目录和邮件目录
sudo userdel -r 用户名
# 强制删除用户账户，并删除用户的主目录和邮件目录
sudo userdel -rf 用户名
```
[Linux userdel命令详解：如何删除用户账户和相关文件（附实例教程和注意事项）](https://bashcommandnotfound.cn/article/linux-userdel-command)

## 查看用户
[列出 Linux 系统上所有用户的 3 种方法](https://zhuanlan.zhihu.com/p/41161408)


## 修改用户名

usermod命令，修改用户的用户名。

- 将用户名修改为`新用户名`，并将家目录路径修改为/home/`新用户名`

```bash
sudo usermod -l 新用户名 -d /home/新用户名 旧用户名
```

-l选项用于指定新的用户名，-d选项，指定新的家目录路径。

- 修改用户名（只是修改了用户账号的属性，而不会修改用户的家目录名称）

```shell
sudo usermod -l 新用户名 旧用户名
```

## 修改用户密码

Linux系统中，passwd命令用于修改用户密码。

| 命令 | 说明 |
|:----:|:---:|
|passwd| 设置/修改 当前用户密码 |
|sudo passwd username | 设置/修改 指定用户密码；<br/>例如：sudo passwd root 设置/修改 root用户密码|
|sudo passwd su| 1. 设置root用户密码<br/>2. 修改root用户密码 |
