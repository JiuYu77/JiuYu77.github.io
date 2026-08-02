---
title: 【Jekyll】命令
description: jekyll 命令，build。
author: yu
date: 2026-08-02 14:13:49 +0800
categories: [Website, Jekyll]
tags: [Jekyll]
---

## 命令

 Build your site（构建你的网站）：
```bash
jekyll build
```

一个jekyll网站项目，既要部署到GitHub，又想将构建的静态网站（如构建目录`_site`）部署到自己的服务器。
部署到自己服务期时，若设置的浏览器访问地址带有前缀，如：http://ip/前缀、http://域名/前缀，
而又不想修改项目的`_config.yml`文件中的`baseurl`，因为修改后，部署github可能会出问题。
这时，可以通过`--baseurl`选项：
```shell
jekyll build --baseurl "your baseurl"
# 示例：jekyll build --baseurl "/jiuyu77"
```

