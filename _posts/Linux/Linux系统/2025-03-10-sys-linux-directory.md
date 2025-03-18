---
title: Linux目录
description: 目录结构。
author: yu
date: 2025-03-10 12:16:00 +0800
categories: [Linux, Linux系统-基础]
tags: [Linux]
mermaid: true
---

## Linux的目录结构

Linux操作系统使用`单一目录树`结构，整个操纵系统只有一个`根目录`。根目录用`正斜线`（/）表示，目录之间也是通过正斜线分隔（如：/etc/systemd/）。

各个分区分别被挂载到目录树的某个目录中，通过通过访问挂载点目录，即可实现对这些分区的访问。

> 在Linux系统中，根目录是文件系统的最顶层，是所有其他目录和文件的起点，所有的目录、文件和设备都在根目录之下。
{: .prompt-info }

```mermaid
graph LR
	A[ / ]-->B1["/"boot]
	A-->B2[ /dev ]
	A-->B3[ /etc ]
	A-->B4[ /home ]
	A-->B5[ /media ]
	A-->B6[ /mnt ]
	A-->B7[ /opt ]
	A-->B8[ /proc ]
	A-->B9[ /root ]
	A-->B10[ /run ]
	style B11 stroke-dasharray: 10, 5
	A-->B11[ /bin ]
	B11-.->C1[ /usr/bin ]
	style B12 stroke-dasharray: 10, 5
	A-->B12[ /sbin ]
	B12-.->C2[ /usr/sbin ]
	style B13 stroke-dasharray: 10, 5
	A-->B13[ /lib ]
	B13-.->C3[ /usr/lib ]
	style B14 stroke-dasharray: 10, 5
	A-->B14[ /lib64 ]
	B14-.->C4[ /usr/lib64 ]
	A-->B15[ /usr ]
	A-->B16[ /srv ]
	A-->B17[ /sys ]
	A-->B18[ /tmp ]
	A-->B19[ /var ]
```

## 根目录 及 系统目录

- **`/`**：根目录。
- **`/boot`**：存放用于系统启动的内核文件 和 引导装载程序文件。
- **`/dev`**：存放设备文件。
- **`/etc`**：存放系统配置文件，如网络配置、设备配置文件等。
- **`/home`**：存放各个用户的主目录。其中的子目录是每个用户的主目录（家目录），名称为各用户的用户名。
- **`/media`**：U盘、光盘、软盘等设备的默认挂载点。
- **`/mnt`**：为某些设备提供的默认挂载点。
- **`/opt`**
- **`/proc`**
- **`/root`**
- **`/run`**
- **`/bin`**
- **`/sbin`**
- **`/lib`**
- **`/lib64`**
- **`/usr`**
- **`/srv`**
- **`/sys`**
- **`/tmp`**
- **`/var`**

## Linux的路径

Linux路径（目录之间）通过`正斜线`（/）分隔，如：/var/log/。
**绝对路径**：由`根目录`开始的文件名或目录名，例如**/etc/profile**。
**相对路径**：开头不是根目录的路径，例如**../../home/user1/**。

## Linux的特殊目录

| 目录 | 说明 |
|:---:|:----:|
| /       | 根目录 |
| .       | 当前目录 |
| ..      | 当前目录的上一级目录 |
| ~       | 当前用户的主目录 |
| ~用户名 | 特定用户的主目录；ls ~jyu |
| -       | 上一次的工作目录；cd - |

## 日志目录

[在Linux中，日志文件通常存储在哪些目录？](https://www.cnblogs.com/huangjiabobk/p/18172420)
* /var
   1. **/var/log/**
   2. **/var/spool/**
* **/proc/sys/log/**
* /home/ 或 /opt/ 或 其他用户目录
  - 对于特定于用户或特定于应用程序的日志，它们可能会被存储在这些目录下。
* **/etc/**
  - 虽然`/etc/`目录主要用于存放系统配置文件，但某些日志，也可能存放在这里。这些文件通常也被视为系统日志的一部分，因此它们通常也会在`/var/log/`下有一个符号链接或备份。

