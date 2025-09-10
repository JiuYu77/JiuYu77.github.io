---
title: 【Golang】 安装
description: 学习或使用Go之前，需要安装Go。
author: yu
date: 2025-09-10 09:29:00 +0800
categories: [Golang, 基础]
tags: [Go, Golang]
image:
  path: /common/posts/Golang/Golang.jpg
  alt: Golang
---


## 前言

`Golang` 简称 `Go`，由Google公司研发，是一种快速、静态类型、编译型语言，看起来像动态类型、解释型语言。
Go语言并发机制（concurrency mechanisms）强大，可轻松实现高并发，并具有**垃圾回收**（GC，garbage collection）机制 和 **运行时反射能力**（power of run-time reflection）。

Go官网：<a href="https://go.dev/" target="_blank">https://go.dev</a>

Google还提供了一个中国大陆地区特供版Go官网：<a href="https://golang.google.cn/" target="_blank">https://golang.google.cn</a>，可直接访问。

## 安装Go

参考官方教程：<a href="https://go.dev/doc/install" target="_blank">https://go.dev/doc/install</a>或
<a href="https://golang.google.cn/doc/install" target="_blank">https://golang.google.cn/doc/install</a>
该安装教程开头部分，还给出了安装`多个版本`、`源码编译安装` Go的方法。

官网教程中，`Linux`系统下将Go安装（解压）到了`/usr/local/go`，我们也可以解压到`其他`自己想用的目录（如 ~/install/go），然后配置`PATH` 环境变量。

Mac系统应该也可以，由于没有Mac电脑，所以没有尝试。

