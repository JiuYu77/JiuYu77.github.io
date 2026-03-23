---
title: 【Python】数据分析与处理库
description: pandas、numpy、jieba、gensim。
author: yu
date: 2026-03-23 15:40:00 +0800
categories: [Python, 库]
tags: [Python, library]
---


## pandas

Pandas是基于Numpy的专业数据分析工具, 可以灵活高效的处理各种数据集。它提供了的数据结构有DataFrame和Series等，可以简单的把DataFrame理解为Excel里面的一张表，而Series就是表中的某一列

pip安装命令：
```bash
pip install pandas
```
导包示例：
```python
import pandas as pd
```

## numpy

NumPy是Python中科学计算的基础包。它是一个Python库，提供多维数组对象，各种派生对象（如掩码数组和矩阵），以及用于数组快速操作的各种API，有包括数学、逻辑、形状操作、排序、选择、输入输出、离散傅立叶变换、基本线性代数，基本统计运算和随机模拟等等。 NumPy包的核心是 ndarray 对象。它封装了python原生的同数据类型的 n 维数组，为了保证其性能优良，其中有许多操作都是代码在本地进行编译后执行的。

pip安装命令：
```shell
pip install numpy
```
导包示例：
```python
import numpy as np
```

## jieba

pip安装命令：
```bash
pip install jieba
```

导包示例：
```python
import jieba.analyse as analyse
import jieba
```

## gensim

pip安装命令：
```bash
pip install gensim
```

导包示例：
```python
from gensim import corpora, models, similarities
import gensim
```

