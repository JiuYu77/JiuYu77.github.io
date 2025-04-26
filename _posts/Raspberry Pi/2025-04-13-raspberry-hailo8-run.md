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
   - 这个步骤通常通过Hailo的 `Dataflow Compiler` 或 Model Zoo完成
3. 部署运行
   - 将生成的.hef文件部署到Hailo设备上
   - 使用Hailo的运行时库在设备上执行推理

详细步骤请见下文。

##  导出PyTorch模型为ONNX

请参考：[PyTorch模型导出为ONNX格式](https://jiuyu77.github.io/posts/PyTorch-onnx-export/)

## onnx转为hef

### 创建虚拟环境（可选）

为了避免与系统环境冲突，建议创建一个虚拟环境来运行Hailo相关工具，以下两种方式选择一个即可。

1. 使用Python自带的 `venv` 模块创建虚拟环境：
```bash
python3 -m venv hailo_env  # 创建虚拟环境
source hailo_env/bin/activate  # 激活虚拟环境
deactivate  # 退出虚拟环境
```

2. 使用Anaconda创建虚拟环境：
```bash
conda create -n hailo_env python=3.10  # 创建虚拟环境
conda activate hailo_env  # 激活虚拟环境
conda deactivate  # 退出虚拟环境
```

### 安装Hailo DFC

为了将自定义模型编译为 `.hef` 模型，需要安装 **Hailo Dataflow Compiler**（DFC） 工具。登录 Hailo 的网站 <a href="https://hailo.ai/developer-zone/software-downloads" target="_blank">https://hailo.ai/developer-zone/software-downloads</a>，找到对应 Python 版本的 `.whl` 文件，并下载。


Latest release（最新版本）、Archive（存档）。

![](/common/posts/hailo/hailo-Dataflow-Compiler.png)

安装 graphviz 库：
```bash
sudo apt install libgraphviz-dev
```

安装 Hailo Dataflow Compiler Python包：
```bash
pip install hailo_dataflow_compiler-3.31.0-py3-none-linux_x86_64.whl
```

卸载 Hailo Dataflow Compiler Python包：
```bash
# 找到 Hailo Dataflow Compiler的包名
pip list  # 查看已安装的包
pip list | grep hailo  # 查找包含 hailo 的包

# 卸载 Hailo Dataflow Compiler包
pip uninstall hailo-dataflow-compiler
```


### onnx转为hef

#### onnx转为hef

har 输入形状：[N, C] 或 [N, H, W, C]

hef

#### 可能遇到的问题

1. **DNN library is not found.**  
Python查看 tensorflow 信息：
```python
import tensorflow as tf
build = tf.sysconfig.get_build_info()
print(build['cuda_version'])   # 查看 tensorflow 使用的cuda版本，如11.8
print(build['cudnn_version'])  # 查看 tensorflow 使用的cudnn版本，如8
```
若没有cuda，则需要安装，官网下载安装 或 命令行apt安装，不需要与tensorflow 使用的cuda版本 严格一致。  
若没有cudnn，则需要安装，官网下载安装 或 命令行apt安装，以cudnn8为例：
```bash
sudo apt install libcudnn8 libcudnn8-dev
```

2. Could not load library **libcublasLt.so.12**. Error: **libcublasLt.so.12**: cannot open shared object file: No such file or directory  
已安装的 Hailo Dataflow Compiler 版本，需要的 `libcublas` 库版本不满足。解决方法：重新安装 cuda 或 Hailo Dataflow Compiler，升高/降低 版本。
```shell
apt search libcublas  # 查看cublas版本
sudo apt install libcublas-dev-12-8
```

## 在Hailo设备上部署运行

