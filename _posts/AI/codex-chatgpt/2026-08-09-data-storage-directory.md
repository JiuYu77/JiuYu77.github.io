---
title: 修改 Codex 数据存储目录
description: 对于windows系统，codex数据默认存储到c:\User\用户名\.codex目录。
author: yu
date: 2026-08-09 18:07:58 +0800
categories: [AI, Codex]
tags: [AI, Codex]
---

Codex的配置、使用过程产生的信息等数据，默认存储在：

- 对于windows系统，codex数据默认存储到`C:\User\用户名\.codex`目录，即`%USERPROFILE%\.codex`。
- 对于Linux、Mac系统，codex数据默认存储到`~/.codex`目录。

尤其对于windows用户来说，若遇到C盘空间不足等原因，可能需要迁移Codex的数据，并修改存储路径。

## Codex App 存储目录修改

对于Windows系统，目前无法通过设置`CODEX_HOME` 或 `CODEX_DIR`环境变量，改变`Codex App`的存储路径。

但可以通过创建`符号链接`实现修改存储目录：
```cmd
mklink /D "%USERPROFILE%\.codex" "D:\path\to\.codex"
```

删除符号链接，但不删除数据：
```cmd
rd "%USERPROFILE%\.codex"
```

