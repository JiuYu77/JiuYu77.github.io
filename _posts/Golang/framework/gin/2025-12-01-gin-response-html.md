---
title: 【Gin】response - 响应html
description: 服务端返回html格式数据，服务端向客户端响应HTML。
author: yu
date: 2025-12-01 14:54:19 +0800
categories: [Golang, 框架]
tags: [Go, Golang, Gin]
image:
  path: /common/posts/Golang/Golang.jpg
  alt: Golang
---

在Gin框架中，服务端可以通过`c.HTML`方法向客户端响应HTML格式的数据。下面是一个简单的示例，展示如何使用Gin框架返回HTML页面。

在这个示例中，我们创建了一个Gin路由，当客户端访问`/html`路径时，服务器会返回一个包含`title` 和 `Title` 变量的HTML页面。

```go
package main

import (
  "github.com/gin-gonic/gin"
)

func main() {
  r := gin.Default()

  // 利用通配符，加载HTML模板文件
  r.LoadHTMLGlob("templates/*") // 加载templates文件夹中所有html文件

  r.GET("/html", func(c *gin.Context) {
    c.HTML(200, "index.html", gin.H{
      "title": "Gin HTML Response",
      "Title": "This is a Title.",
    })
  })

  r.Run(":8080") // 启动服务器，监听端口8080
}
```
确保你已经在项目中设置了HTML模板文件（例如`templates/index.html`），例如：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ .title }}</title>
</head>
<body>
    <h1>{{ .Title }}</h1>
</body>
</html>
```
