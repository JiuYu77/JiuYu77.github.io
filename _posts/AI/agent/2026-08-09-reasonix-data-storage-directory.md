---
title: 修改 reasonix 数据存储目录
description: reasonix 支持通过环境变量，修改数据存储位置。
author: yu
date: 2026-08-09 18:26:56 +0800
categories: [AI, Agent]
tags: [AI, reasonix]
---

`reasonix`的配置、使用过程产生的信息等数据，默认存储在：

- 对于windows系统，codex数据默认存储到`C:\Users\用户名\.reasonix`目录，即`%USERPROFILE%\.reasonix`。
- 对于Linux、Mac系统，codex数据默认存储到`~/.reasonix`目录。

可通过以下步骤修改存储位置（迁移数据）：
1. 如果正在使用，则退出reasonix。
1. 确定数据迁移位置（新目录），如`D:/path/to/reasonix-data`。
1. 设置`REASONIX_HOME`环境变量。
1. 将数据从 原目录，如 C:/Users/nk/.reasonix， 复制到 新目录。
1. 打开reasonix，检查数据迁移是否成功。
1. 若迁移成功，则可以删除原目录。
