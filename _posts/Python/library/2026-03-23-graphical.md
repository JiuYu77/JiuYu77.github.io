---
title: 【Python】图形化
description: PyQt5、matplotlib、pyecharts。
author: yu
date: 2026-03-23 15:54:07 +0800
categories: [Python, 库]
tags: [Python, library]
---

## PyQt5

PyQt是Python语言的GUI（Graphical User Interface，简称 GUI，又称图形用户接口）编程解决方案之一。可以用来代替Python内置的Tkinter。其它替代者还有PyGTK、wxPython等，与Qt一样，PyQt是一个自由软件。

pip安装命令：
```shell
pip install PyQt5
```
导包示例：
```python
from PyQt5 import QtCore, QtGui, QtWidgets
```

## matplotlib

Matplotlib 是一个非常强大的 Python 绘图库，我们可以使用它将很多数据通过各种静态、动态、交互式图表的形式直观的呈现出来，比如：线图、散点图、条形图、饼图、3D 图形、图形动画等等，并且提供多样化的输出格式。Matplotlib 通常与 NumPy 和 Pandas 等库一起使用， 形成一个强大的科学计算环境，有助于我们通过 Python 学习数据科学、机器学习。
需要注意的是，matplotlib 的依赖库包括 numpy、cycler、Pillow、packaging、pyparsing、python-dateutil、fonttools、contourpy、kiwisolver，需要预先安装，不过只要连网后都会自动下载安装。

pip安装命令：
```bash
pip install matplotlib
```
导包示例：
```python
import matplotlib as mp
import matplotlib.pyplot as plt
```

## pyecharts

pyecharts库是一个用于生成 Echarts 图表的类库。
Echarts是一个由百度开源的商业级数据图表，它是一个纯JavaScript的图表库，可以为用户提供直观生动，可交互，可高度个性化定制的数据可视化图表，赋予了用户对数据进行挖掘整合的能力。

pip安装命令：
```shell
pip install pyecharts
```
导包示例：
```python
from pyecharts.charts import Line
from pyecharts.charts import Map
from pyecharts.charts import Bar, Timeline
from pyecharts.options import TitleOpts, LegendOpts
```

