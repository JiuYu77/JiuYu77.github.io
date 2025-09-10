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

Go环境变量官方文档：<a href="https://go.dev/ref/mod#environment-variables" target="_blank">https://go.dev/ref/mod#environment-variables</a>
或 <a href="https://golang.google.cn/ref/mod#environment-variables" target="_blank">https://golang.google.cn/ref/mod#environment-variables</a>

### GOROOT

`GOROOT`是go安装后的根目录，即它的作用是指定 Go 语言 SDK（软件开发工具包）的安装目录。

可执行程序`go`(或go.exe)和`gofmt`(或gofmt.exe)位于 **GOROOT/bin** 目录中。

GOROOT 通常在安装 Go 时会**自动配置**，一般不需要手动修改。

查看GOROOT值，可执行以下命令：
```shell
go env GOROOT
```

### GOPATH

`GOPATH`用于控制各种文件的存储位置，或者说GOPATH环境变量列出了查找Go代码的位置。

GOPATH指定的目录保存了**项目代码**、**第三方依赖**（模块/库）。

GOPATH变量是可能包含Go代码的`目录列表`：
- 在Unix上，该值是一个用冒号分隔的字符串。
- 在Windows上，该值是一个以分号分隔的字符串。

如果未设置GOPATH，则**默认**为用户主目录的go子目录，如**/home/nk-user/go**。

推荐通过以下命令修改GOPATH：
```shell
go env -w GOPATH=路径
```

### GOPROXY

`GOPROXY`是Go模块代理URL的列表，用逗号（,）或管道（\|）分隔。

当go命令查找有关模块的信息时，它会**按顺序**联系列表中的每个代理，直到收到成功的响应或终端错误。

当我们无法成功下载第三方依赖时，就需要设置代理。

中国用户可使用国内七牛云代理`https://goproxy.cn`，通过如下命令设置环境变量：
```shell
export GOPROXY=https://goproxy.cn
export GOPROXY=https://goproxy.cn,direct # 多个代理需要使用分隔符 , 或 |
```

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

