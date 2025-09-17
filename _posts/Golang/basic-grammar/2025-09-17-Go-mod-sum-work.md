---
title: 【Golang】go.mod、go.sum、go.work
description: go.mod、go.sum、go.work文件，go mod、go work命令。
author: yu
date: 2025-09-17 16:13:53 +0800
categories: [Golang, 基础]
tags: [Go, Golang]
image:
  path: /common/posts/Golang/Golang02.jpg
  alt: Golang
---


## go mod

Go项目由 go模块（`Go module`）和 go包（`Go package`）构成：
- 一个 Go项目 可以包含 一个或多个 Go模块
- 一个 Go模块 可以包含 一个或多个 Go包
- 一个 Go包 可包含 一个或多个 go文件（扩展名为.go）

每个模块都有一个`go.mod`文件，用于记录依赖项（追踪依赖）。

`go mod`命令用于管理模块及其依赖。

### go mod init

- `go mod init`命令用于初始化模块，并新建`go.mod`文件，实际这创建了一个以当前目录为根的新模块。
- 执行该命令须确保**不存在** go.mod 文件。

初始化一个模块，请执行以下命令：
```shell
go mod init [module-path]
```
对于 `GOPATH` 之外的模块，那么 模块路径（module path）是必须的。示例：
```shell
go mod init example.com/hello
```
生成的 go.mod 文件内容，包括 模块路径 和 你使用的go版本：
>module example.com/hello

>go 1.25.1


### go mod tidy

- `go mod tidy` 可以向 `go.mod`文件 添加构建当前模块的包和依赖项**所需的任何缺失模块**，并下载缺失模块。
- 删除不再使用的依赖（模块），在 `go.mod`文件中的记录。
- 向`go.sum`文件**添加**缺失模块的验证信息，用于对模块进行身份验证。
- **删除**不再使用的模块（依赖项），在`go.sum`文件中的对应条目。

通过以下命令，查看更多用法：
```shell
go mod help tidy
```

### go mod edit

`go mod edit`命令用来编辑`go.mod`文件。

更多内容请查看帮助信息：
```shell
go mod help edit
```

## go.sum

`go.sum`文件用于记录 `go.mod`文件中追踪的**依赖（模块）的验证信息**，这些信息用于对模块进行身份验证。

go.sum文件不需要手动编辑，它由Go工具自动生成和维护。

## go work

多模块工作区（multi-module workspaces），即一个文件夹（项目）包含多个模块（module）。

`go work`机制用于管理工作空间（工作区）：
- 工作区由`go.work`文件指定，一个工作区拥有一个 go.work文件。
- `go.work`文件使用`use`指令指定（记录）了一组模块目录，这些模块被go命令用作构建和相关操作的**根模块**，可以在其他模块中导入并使用。
- `go work`命令提供对工作空间的操作。


### go work init

初始化工作区：
```shell
go work init [moddirs]
```
该命令会新建 `go.work`文件。

`moddirs`是可选参数，工作区中的模块的路径，可以是多个，以空格分隔。
如果省略，则`go.work`文件中只有一行go版本，如：
>go 1.25.1

若给定了模块，如`go work init ./hello`，则`go.work`文件：
>go 1.25.1

>use ./hello

### go work use

`go work use`指令用于向`go.work`文件添加指定模块目录。

go.work文件中，use指定的模块可以被当前工作区的其他模块调用（导入并使用）。

```shell
go work use [-r] [moddirs]
```
- **moddirs**：向`go.work`文件添加的目录，可以是多个，以空格分隔。
- **-r**：可选参数，递归搜索参数目录中的模块。

示例：
```shell
go work init
go work use ./hello ./test
```
go.work文件：
>go 1.25.1
>
>use (
>        ./1
>        ./2
>)


