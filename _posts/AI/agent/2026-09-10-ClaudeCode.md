---
title: Claude Code
description: Claude Code
author: yu
date: 2026-09-10 00:28:05 +0800
categories: [AI, Agent]
tags: [AI, Claude Code]
---

## 官方

官网：[https://claude.com/product/claude-code](https://claude.com/product/claude-code)

文档：[https://code.claude.com/docs/en/overview](https://code.claude.com/docs/en/overview)，文档可以切换不同语言。

## 问题

（1）API Error: 400 Invalid schema for function 'Artifact'

对应的 GitHub Issues：[https://github.com/farion1231/cc-switch/issues/7236](https://github.com/farion1231/cc-switch/issues/7236)

方法1：修改`settings.json`文件，增加：
```json
{
  "enableArtifact": false
}
```

方法2：启动claude，使用 `/config`命令，将 Artifacts true 改为 Artifacts false，该方法同样会写入settings.json，只是不用手动修改文件了。
