---
title: Linux目录
description: 目录结构、文件权限。
author: yu
date: 2025-03-10 12:16:00 +0800
categories: [Linux, Linux系统-基础]
tags: [Linux]
---


## Linux目录

$$
\begin{equation}
  \sum_{n=1}^\infty 1/n^2 = \frac{\pi^2}{6}
  \label{eq:series}
\end{equation}
$$

We can reference the equation as \eqref{eq:series}.

When $a \ne 0$, there are two solutions to $ax^2 + bx + c = 0$ and they are

$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a} $$


```mermaid
 gantt
  title  Adding GANTT diagram functionality to mermaid
  apple :a, 2017-07-20, 1w
  banana :crit, b, 2017-07-23, 1d
  cherry :active, c, after b a, 1d
```

### 目录结构

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


| 目录 | 说明 |
|:----:|:----:|
| /     | 根目录 |
| /boot |2""|
| /dev ||
| /etc ||
| /home ||
| /media ||
| /mnt ||
| /opt ||
| /proc ||
| /root ||
| /run ||
| /bin ||
| /sbin ||
| /lib ||
| /lib64 ||
| /usr ||
| /srv ||
| /sys ||
| /tmp ||
| /var | 2|

### 日志目录
[在Linux中，日志文件通常存储在哪些目录？](https://www.cnblogs.com/huangjiabobk/p/18172420)
* /var
   1. **/var/log/**
   2. **/var/spool/**
* **/proc/sys/log/**
* /home/ 或 /opt/ 或 其他用户目录
  - 对于特定于用户或特定于应用程序的日志，它们可能会被存储在这些目录下。
* **/etc/**
  - 虽然`/etc/`目录主要用于存放系统配置文件，但某些日志，也可能存放在这里。这些文件通常也被视为系统日志的一部分，因此它们通常也会在`/var/log/`下有一个符号链接或备份。

