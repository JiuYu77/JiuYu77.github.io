---
title: 杀死进程（终止进程）
description: kill、pkill、killall。
author: yu
date: 2025-05-31 14:51:34 +0800
categories: [Linux, 命令]
tags: [Linux, command]
---

**杀死指定 `PID`的进程**
```shell
# 杀死进程，PID是进程标识符
kill PID
# 强制杀死进程，向想要杀死的进程发送信号9（即SIGKILL信号）
kill -9 pid
```

**杀死指定名字的所有进程**（根据进程名杀死进程）
```shell
# pkill 根据进程名称中的“关键字”去杀进程
pkill -9 php
# killall
killall -9 python
```

**一键杀掉某用户所有进程**
```shell
pkill -u 用户名
killall -u 用户名
```

