---
title: 【Gin】response - 响应json
description: 服务端返回json格式数据。
author: yu
date: 2025-12-01 13:47:50 +0800
categories: [Golang, 框架]
tags: [Go, Golang, Gin]
image:
  path: /common/posts/Golang/Golang.jpg
  alt: Golang
---


在使用 Gin 框架开发 Web 应用时，常常需要向客户端返回 JSON 格式的数据。Gin 提供了非常简便的方法来实现这一点。

## c.JSON()

Gin 提供了 `c.JSON()` 方法，可以轻松地将数据以 JSON 格式返回给客户端。该方法接受两个参数：HTTP 状态码和要返回的数据。

```go
func(c *gin.Context) {
  c.JSON(http.StatusOK, gin.H{
    "code": 0,
    "msg": "Hello, Gin!",
    "data": nil,
  })
}
```
在上面的示例中，`gin.H` 是一个快捷方式，用于创建一个 `map[string]interface{}` 类型的对象，方便我们构建 JSON 响应。

## 示例代码

下面是一个完整的示例，展示了如何使用 Gin 框架返回 JSON 响应：

```go
package main
import (
  "net/http"
  "github.com/gin-gonic/gin"
)
func main() {
  r := gin.Default()

  r.GET("/json", func(c *gin.Context) {
    c.JSON(http.StatusOK, gin.H{
      "code": 0,
      "msg": "Hello, Gin!",
      "data": nil,
    })
  })

  r.Run(":8080") // 启动服务器，监听端口8080
}
```

在这个示例中，当客户端发送 GET 请求到 `/json` 路径时，服务器会返回一个 JSON 响应，包含状态码、消息和数据字段。
运行该程序，访问 [`http://localhost:8080/json`](http://localhost:8080/json)，你将看到如下 JSON 响应：
```json
{
  "code": 0,
  "msg": "Hello, Gin!",
  "data": null
}
```
通过这种方式，Gin 框架使得返回 JSON 响应变得非常简单和高效，极大地提升了开发效率。

## 封装响应

为了统一响应格式，可以封装一个响应函数，`resp/resp.go`文件：
```go
package resp
import (
  "net/http"
  "github.com/gin-gonic/gin"
)

var codeMap = map[int]string{
  0:    "成功:)",
  1000: "服务错误", // 默认错误
  1001: "权限错误",
  1002: "角色错误",
}

func GetMsg(code int) string {
  msg, ok := codeMap[code]
  if !ok {
    msg = codeMap[1000]
  }
  return msg
}

type Response struct {
  Code int         `json:"code"`
  Msg  string      `json:"msg"`
  Data any         `json:"data"`
}

func respond(c *gin.Context, code int, msg string, data any) {
  c.JSON(http.StatusOK, Response{
    Code: code,
    Msg:  msg,
    Data: data,
  })
}

func OK(c *gin.Context, msg string, data any) {
  respond(c, 0, msg, data)
}
func OKWithData(c *gin.Context, data any) {
  OK(c, GetMsg(0), data)
}
func OKWithMsg(c *gin.Context, msg string) {
  OK(c, msg, gin.H{})
}

func Fail(c *gin.Context, code int, msg string, data any) {
  respond(c, code, msg, data)
}
func FailWithMsg(c *gin.Context, msg string) {
  Fail(c, 1001, msg, gin.H{})
}
func FailWithCode(c *gin.Context, code int) {
  msg := GetMsg(code)
  Fail(c, code, msg, gin.H{})
}
```

通过封装响应函数，可以在项目中统一管理响应格式，提升代码的可维护性和一致性，简洁方便。

使用示例：
```go
package main

import (
  "net/http"
  "github.com/gin-gonic/gin"
  "your-project/resp"
)

func main() {
  r := gin.Default()

  r.GET("/json", func(c *gin.Context) {
    resp.OKWithMsg(c, "Hello, Gin!")
  })

  r.GET("/users", func(c *gin.Context) {
		resp.OKWithData(c, gin.H{
			"name": "秦始皇",
      "age":  22,
		})
	})

  r.POST("/users", func(c *gin.Context) {
		resp.FailWithMsg(c, "参数错误")
	})

  r.Run(":8080") // 启动服务器，监听端口8080
}
```
通过这种方式，可以更方便地管理和返回 JSON 响应，提升代码的可读性和维护性。
