---
title: 【ssh】openssh-server 配置文件
description: ssh，openssh-server，sshd_config，ssh服务端配置文件，IPv4，IPv6。
author: yu
date: 2025-10-22 18:23:46 +0800
categories: [Blogging, ssh]
tags: [SSH]
---


## 前言

`ssh` 分为 服务端（`openssh-server`）与 客户端（`openssh-client`）。

本文 介绍了 **openssh-server** 配置文件 相关内容。

以Linux为例，openssh-server配置文件为`/etc/ssh/sshd_config`。

## sshd_config 文件

安装openssh-server后，`/etc/ssh/sshd_cinfig`文件中各配置项的值，即为**默认值**，
不论前面有没有 注释符号`#`。

**修改配置后，需重启ssh。**重启 sshd 服务，使配置生效：
```shell
sudo systemctl restart ssh
# 或
sudo systemctl restart sshd
# 或
sudo service ssh restart
```

### 网络配置选项

**#Port 22**，ssh连接的端口号，默认22：
```plaintext
# 设置为其他port，需重启ssh服务
Port 16123
```

**#AddressFamily any**
- 功能：指定 SSH 服务器监听的地址族：
- 选项：
  - `any`: 同时监听IPv4 和 IPv6
  - `inet`: 仅监听 IPv4
  - `inet6`: 仅监听 IPv6

**#ListenAddress 0.0.0.0**
- 功能：指定 SSH 服务器监听的 IPv4 地址
- `0.0.0.0`：监听所有可用的 IPv4 地址

**#ListenAddress ::**
- 功能：指定 SSH 服务器监听的 IPv6 地址
- `::`：监听所有可用的 IPv6 地址

**监听所有可用的 IPv4 和 IPv6 地址：**
```shell
AddressFamily any      # 同时支持 IPv4 和 IPv6
ListenAddress 0.0.0.0  # 监听所有 IPv4
ListenAddress ::       # 监听所有 IPv6
# 以上，与默认配置等效
#AddressFamily any      # 同时支持 IPv4 和 IPv6
#ListenAddress 0.0.0.0  # 监听所有 IPv4
#ListenAddress ::       # 监听所有 IPv6
```

**只监听IPv4**
```shell
AddressFamily inet # 或 AddressFamily any
ListenAddress 0.0.0.0  # 只监听IPv4
#ListenAddress ::       # 注释掉IPv6
```

**只监听IPv6**
```shell
AddressFamily inet6 # 或 AddressFamily any
#ListenAddress 0.0.0.0  # 注释掉 IPv4
ListenAddress ::       # 只监听 IPv6
```

### 安全相关配置选项

**#PermitRootLogin prohibit-password**
- 功能：控制 root 用户如何通过 SSH 登录
- 选项：
  - `prohibit-password`：允许 root 登录，但**禁止使用密码认证**（只能使用密钥认证）
  - `yes`：允许 root 使用密码和密钥登录
  - `no`：完全禁止 root 登录
  - `without-password`：同 `prohibit-password`（别名）
  - `forced-commands-only`：仅允许使用密钥认证且执行特定命令

是否允许 `root` 用户登录：
```plaintext
# 允许 root 用户登录
PermitRootLogin yes
```

### X11转发

**#X11Forwarding yes**
- 功能：控制是否允许 X11 图形界面转发
- 选项：
  - `yes`：启用 X11 转发
  - `no`：禁用 X11 转发

开启`X11`转发：
```plaintext
X11Forwarding yes
```


