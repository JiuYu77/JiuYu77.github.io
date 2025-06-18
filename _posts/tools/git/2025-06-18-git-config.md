---
title: Git 配置
description: 安装git后，需要配置一下；查看配置。
author: yu
date: 2025-06-18 17:00:00 +0800
categories: [Blogging, git]
tags: [Git]
---

## 查看配置

查看局部配置（在仓库目录下）：
```shell
git config list
git config --list
```

查看全局配置：
```shell
git config --global --list
git config --list  # 位于仓库目录外
```

## 配置用户和邮箱

### 全局配置

配置用户名
```shell
git config --global user.name  'your_name'
```

配置邮箱
```shell
git config --global user.email 'your_email'
```

### 局部配置

进行局部配置时，需要处于 **git仓库 所在目录**。

配置用户名
```shell
git config user.name  'your_name'
```

配置邮箱
```shell
git config user.email 'your_email'
```

