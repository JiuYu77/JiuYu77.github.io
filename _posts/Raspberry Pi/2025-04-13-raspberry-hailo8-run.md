---
title: 树莓派5 加 Hailo-8 运行PyTorch模型
description: 本文讲述了如何在 树莓派5平台 利用 Hailo-8加速器，运行PyTorch深度学习模型。
author: yu
date: 2025-04-13 20:11:52 +0800
categories: [Blogging, 树莓派]
tags: [Raspberry Pi, apt, Hailo-8]
image:
  path: https://assets.raspberrypi.com/static/8f6eca535dbb23a21eb41a748050e3a0/33b96/16gb.webp
  alt: Raspberry Pi 5
---

## 前言

我们无法通过Hailo-8直接运行PyTorch模型，因此要在Hailo硬件上运行PyTorch模型，通常需要经过以下步骤：
1. 模型训练与导出
   - 在PyTorch中训练您的模型
   - 将训练好的PyTorch模型导出为ONNX格式（.onnx文件）
2. 模型转换
   - 将ONNX模型转为Hailo可执行的格式(.hef)
   - 这个步骤通常通过Hailo的Model Zoo或Dataflow Compiler完成
3. 部署运行
   - 将生成的.hef文件部署到Hailo设备上
   - 使用Hailo的运行时库在设备上执行推理

详细步骤请见下文。

##  导出PyTorch模型为ONNX

请参考：[PyTorch模型导出为ONNX格式](https://jiuyu77.github.io/posts/PyTorch-onnx-export/)

## 使用Hailo工具链转换


## 在Hailo设备上运行

