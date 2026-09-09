---
title: DeepSeek Harness
description: DeepSeek Harness
author: yu
date: 2026-09-09 23:25:32 +0800
categories: [AI, Agent]
tags: [AI, reasonix]
---

## 官方

DeepSeek Harness，DS Harness

**官网**：[https://www.deepseek.com/harness](https://www.deepseek.com/harness)

**GitHub**：[https://github.com/deepseek-ai/deepseek-harness](https://github.com/deepseek-ai/deepseek-harness)

## 安装

### 一键使用

```shell
npx @deepseek-ai/dsh web
```

### 源码安装

```shell
git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
pnpm install
pnpm run build
pnpm dsh web
```

项目更新后，我们执行git pull后，若构建（build）失败，可尝试以下命令：
```shell
pnpm clean
```
然后，再进行build：
```shell
pnpm install
pnpm run build
pnpm dsh web
```

## 修改存储目录

DeepSeek Harness 默认数据存储目录：
- Windows：`C:\Users\用户名\.dsh\`
- Linux / MacOS：`~/.dsh/`

修改存储目录，配置`DSH_HOME`环境变量即可，如`D:\app_data\dsh\`。

## 自定义模型输入

DeekSeek Harness 支持自定义模型，即配置非deepseek官网的模型，通过API Key进行模型调用。

但配置好的模型，默认只支持文本输入，即使模型本身可以接受图片。

但DS Harness界面上，没有提供设置模型支持的输入模态的方式。

我们需要手动修改DS Harness配置文件`settings.yaml`。

DeepSeek Harness 目前支持`text`、`image`两种输入格式。

默认情况下，配置文件中，不存在输入格式配置：

![](common/posts/deepseek-harness/ds-harness-01.png)

`defaultInput: [text, image]`，相当于全局配置：

![](common/posts/deepseek-harness/ds-harness-02.png)

`input: [text, image]`，则可以单独控制每个模型的输出支持：

![](common/posts/deepseek-harness/ds-harness-03.png)

defaultInput 和 input 同时出现时，input优先级更高：
![](common/posts/deepseek-harness/ds-harness-04.png)
