---
title: 树莓派5 使用 Hailo-8
description: 树莓派5，Hailo-8，NPU。
author: yu
date: 2025-04-09 22:01:48 +0800
categories: [Blogging, 树莓派]
tags: [Raspberry Pi, apt]
---

## 简介

Hailo-8，是由以色列公司 Hailo 开发的一款高性能边缘 AI 处理器(NPU，Neural network Processing Unit)，是一款AI加速器(AI Accelerator)，它专为低功耗、高效率的深度学习推理任务设计。

- `Hailo-8 AI Accelerator`，官网描述摘录（截至2025-04-09）：The `Hailo-8 edge AI processor`, featuring up to 26 tera-operations per second (TOPS), significantly outperforms all other edge processors. 
- `Hailo-8L Entry-Level AI Accelerator`，官网描述摘录（截至2025-04-09）：The `Hailo-8L Entry-Level AI Accelerator`, featuring up to 13 tera-operations per second (TOPS), is designed to support entry level products requiring limited AI capacity or lower performance.

[hailo-ai GitHub](https://github.com/hailo-ai)

[hailo.ai 官网入口](https://hailo.ai/)  
[Hailo-8L Entry-Level AI Accelerator 官网入口](https://hailo.ai/products/ai-accelerators/hailo-8l-ai-accelerator-for-ai-light-applications/)  
[Hailo-8 AI Accelerator 官网入口](https://hailo.ai/products/ai-accelerators/hailo-8-ai-accelerator/)

>CPU，Central Processing Unit，中央处理单元（Unit 可翻译为 器 或 单元）  
 GPU，Graphics Processing Unit，图形处理单元  
 NPU，Neural Processing Unit，神经（网络）处理单元  
{: .prompt-tip }


[**树莓派-hailo官方文档 AI Kit and AI HAT+ software**](https://www.raspberrypi.com/documentation/computers/ai.html)

本文`树莓派5`使用的系统：**`Raspberry Pi OS (64-bit)`** 。

是否修改了软件源：**否**。

## 硬件连接

1. 首先，确保您的Raspberry Pi运行最新的软件。运行以下命令进行更新：
```shell
sudo apt update && sudo apt full-upgrade
```
2. 接下来，确保您的树莓派固件(Raspberry Pi firmware)是最新的。
- 运行以下命令以查看您正在运行的固件：
```shell
sudo rpi-eeprom-update
```
- 运行以下命令将固件更新到最新版本：
```shell
sudo rpi-eeprom-update -a
```
- 重启`sudo reboot`
3. 断开树莓派电源。
4. 将 hailo-8 安装到树莓派上。

## 驱动&固件安装

```shell
sudo modprobe hailo_pci
```
