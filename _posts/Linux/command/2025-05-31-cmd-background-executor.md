---
title: 后台执行程序
description: 程序后台运行，使用&、nohup。
author: yu
date: 2025-05-31 15:02:53 +0800
categories: [Linux, 命令]
tags: [Linux, command]
---

`0` 标准输入、`1` 标注输出、`2` 标注错误输出。

## 只使用&

退出Terminal（shell）后，程序会终止（停止）运行。

```shell
程序 &
程序 >/dev/null &  # `>` 是 `1>` 的简写，默认 将 1标准输出 重定向到 黑洞文件/dev/null
程序 >/dev/null 2>&1 &

示例：后台执行ls命令，并将ls命令的 标准输出 和 标准错误 重定向到当前目录下的test.txt文件；`若test.txt文件不存在，则自动创建`。
ls >test.txt 2>&1 &
```

## nohup 加 &

退出Terminal（shell），也不会终止（停止）程序运行。

```bash
# 标准输出，重定向到：黑洞文件/dev/null
nohup 程序 1> /dev/null &

# “2>&1”是一种优化写法，表示把'标准错误的输出'也输出到'标准输出'所指定的文件中。
nohup 程序 1>/dev/null 2>&1 &
```

