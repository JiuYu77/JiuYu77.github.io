---
title: 【Golang】环境变量
description: Go环境会参考其环境变量进行配置，如GOROOT、GOPATH、GOPROXY。
author: yu
date: 2025-09-10 11:58:20 +0800
categories: [Golang, 基础]
tags: [Go, Golang]
image:
  path: /common/posts/Golang/Golang02.jpg
  alt: Golang
---

## 查看Go环境变量

查看Go环境变量具体信息，运行以下命令：
```shell
go help environment
```
该命令会输出Go支持的环境变量，及其说明。

查看环境变量的值，请运行以下命令：
```shell
go env [环境变量名1 var2 ...]
```
如：
```shell
go env  # 查看所有go环境变量的值
go env GOROOT
go env GOROOT GOPATH
```

## 重要Go环境变量

### GOROOT

### GOPATH

### GOPROXY


## 设置Go环境变量

### 方法1

该方法是在Linux配置文件中设置，如`/etc/profile`、`/etc/profile.d/`目录下的脚本文件、`~/.profile`等。

**推荐：**
- go的`bin`目录配置到Linux环境变量`PATH`中。
- `GOPROXY`变量可以配置到Linux配置文件（同一台机器多个版本的go可能都需要设置代理），也可以通过`方法2`设置。
- 其余环境变量通过`方法2`配置。

### 方法2

通过go命令设置go环境变量，如此`多个版本`的go环境变量可以相互独立：
```shell
go env -w var=value
```
如：
```shell
go env -w GOPATH=/home/nk/install/go/gopath
```

