---
title: 数据增强
description: 数据增强，可以 扩充数据集、提升模型泛化性。
author: yu
date: 2025-08-01 17:26:10 +0800
categories: [AI, Deep Learning]
tags: [AI, Deep Learning]
---


## 简介

数据增强（Data Augmentation，DA），又称数据增广、数据扩增。

**数据增强**是一种通过算法扩展训练数据的技术，主要用于解决机器学习中的小样本问题。其核心原理是利用现有数据生成相似样本，从而扩大数据集规模，提升模型训练效果。

**数据增强**是一种在机器学习（尤其是深度学习）中广泛使用的技术，**通过对原始训练数据进行一系列随机变换，生成新的、多样化的训练样本**，从而在不实际收集新数据的前提下扩大训练集的规模和多样性。其核心目标是**提升模型的泛化能力、减少过拟合**，并增强模型对真实世界中各种变化的鲁棒性。

## 工具支持

- 图像：<br>
TensorFlow（`tf.keras.layers.RandomFlip`）、
PyTorch（`torchvision.transforms`）、
`Albumentations`（高性能增强库）

- 文本：<br>
`nlpaug`、`TextAttack`

- 音频：<br>
`Librosa`、`torchaudio.transforms`

- 时间序列：<br>
`Tsaug`

