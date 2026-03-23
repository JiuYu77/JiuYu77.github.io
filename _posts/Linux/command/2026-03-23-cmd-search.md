---
title: 【Linux命令】查找
description: find、locate、whereis、which。
author: yu
date: 2026-03-23 13:01:57 +0800
categories: [Linux, 命令]
tags: [Linux, command]
---

## find

find命令支持`正则表达式`，使用时要注意目录的**访问权限**。

在当前文件夹查找：
```bash
find 要查找的目录 或 文件
find . <要查找的 目录 / 文件>
```
在指定文件夹查找：
```bash
find 目录 <要查找的 目录 或 文件>
```
查找存在的文件，却显示“没有那个文件或目录”，加上`-name`选项
```
find -name 要查找的目录 或 文件
find 目录 -name 要查找的目录 或 文件

示例：
    find ~/ -name .vimrc
    find ~/ -name **.vim**
```
不区分大小写：
```bash
find 目录 -iname <目录 / 文件>
```
查找文件，限制大小：
```bash
find <路径> -name <文件名/目录> -size +<大小限制>
# 示例：find /home/user -name "example.txt" -size +1M
```

## locate

## whereis

Linux whereis命令用于查找文件。该指令会在特定目录中查找符合条件的文件。
这些文件应属于原始代码、二进制文件，或是帮助文件。
该指令只能用于查找二进制文件、源代码文件和man手册页，一般文件的定位需使用locate命令。

## which

Linux which命令用于查找文件。which指令会在环境变量`PATH`设置的目录里查找符合条件的文件。
