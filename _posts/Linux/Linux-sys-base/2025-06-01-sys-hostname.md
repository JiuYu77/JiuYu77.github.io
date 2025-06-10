---
title: Linux主机名
description: Linux主机名，修改主机名，hostname，hostnamectl。
author: yu
date: 2025-06-01 17:35:38 +0800
categories: [Linux, Linux系统-基础]
tags: [Linux]
---


## 查看主机名

```shell
hostname
```

## 临时修改主机名

```shell
sudo hostname 新主机名
```
重启计算机后，更改失效。

## 永久修改主机名

方法1：修改配置文件`/etc/hostname`

```shell
sudo vim /etc/hostname
```

方法2：`hostnamectl`命令

```shell
sudo hostnamectl set-hostname <new hostname>
```
这条命令会删除 `/etc/hostname` 文件中的主机名，然后替换为新的主机名。

**重启生效**。

