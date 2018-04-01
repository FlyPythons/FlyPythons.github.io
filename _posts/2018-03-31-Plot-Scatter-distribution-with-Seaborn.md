---
layout:     post
title:      "Seaborn绘制scatter分布图"
subtitle:   ""
date:       2018-03-31
author:     "FlyPythons"
header-img: ""
catalog: true
tags:
    - Visualization
    - Seaborn
    - Distribution
---

# Seaborn绘制scatter分布图

# 1. 准备数据


```python
%matplotlib inline

import seaborn as sns
from matplotlib import pyplot as plt
import numpy as np

x = np.random.randn(5000)
y = np.random.randn(5000)
```

# 2. 开始绘图


```python
with sns.axes_style("darkgrid"):  # backgournd
    g = sns.JointGrid(x,y)
    g.plot_joint(plt.scatter, c="b", s=20, linewidth=0, marker="o", alpha=0.5)  # plot scatter
    g.plot_marginals(sns.distplot, kde=False, color="b")  # plot distribution
    g.set_axis_labels("GC %", "Sequence depth (X)", size=14)  # add label
```


![png](https://github.com/FlyPythons/flypythons.github.io/raw/master/img/2018-03-31-dist-1.png)


* 来个等高线


```python
with sns.axes_style("darkgrid"):  # backgournd
    g = sns.JointGrid(x,y)
    g.plot_joint(plt.scatter, c="b", s=20, linewidth=0, marker="o", alpha=0.5)  # plot scatter
    g.plot_joint(sns.kdeplot, shade=True, shade_lowest=False, alpha=0.9, cmap=plt.cm.Blues)  # plot kde
    g.plot_marginals(sns.distplot, kde=False, color="b")  # plot distribution
    g.set_axis_labels("GC %", "Sequence depth (X)", size=14)  # add label
```


![png](https://github.com/FlyPythons/flypythons.github.io/raw/master/img/2018-03-31-dist-2.png)


* 蜂窝？


```python
with sns.axes_style("darkgrid"):  # backgournd
    g = sns.jointplot(x,y, kind="hex", gridsize=30, alpha=1, cmap=plt.cm.Blues, color="blue",marginal_kws={"kde":False}, stat_func=None)
    g.set_axis_labels("GC %", "Sequence depth (X)", size=14)  # add label
```


![png](https://github.com/FlyPythons/flypythons.github.io/raw/master/img/2018-03-31-dist-3.png)


* 还是正方形


```python
with sns.axes_style("darkgrid"):  # backgournd
    g = sns.JointGrid(x,y)
    #g.plot_joint(plt.scatter, c="b", s=10, linewidth=1, marker="o", alpha=1)  # plot scatter
    g.plot_joint(plt.hist2d,bins=30, alpha=1, cmap=plt.cm.Blues)  # plot kde
    g.plot_marginals(sns.distplot, kde=False, color="b")  # plot distribution
    g.set_axis_labels("GC %", "Sequence depth (X)", size=14)  # add label
```


![png](https://github.com/FlyPythons/flypythons.github.io/raw/master/img/2018-03-31-dist-4.png)


* 做个回归分析


```python
x1 = np.random.randint(1, 100 ,1000)
y1 = 2*x1 + 10
noise = np.random.randn(1000)*20
y1 = y1 + noise

with sns.axes_style("darkgrid"):  # backgournd
    g = sns.jointplot(x1,y1, kind="reg",  color="blue",marginal_kws={"kde":False,}, joint_kws={ "scatter_kws": {"s": 20, "alpha":0.5}} )
    g.set_axis_labels("GC %", "Sequence depth (X)", size=14)  # add label
```


![png](https://github.com/FlyPythons/flypythons.github.io/raw/master/img/2018-03-31-dist-5.png)

## 3. 下载.ipynb
[Download Jupyter notebook .ipynb](https://github.com/FlyPythons/flypythons.github.io/raw/master/_downloads/2018-03-31-Plot-Scatter-distribution-with-Seaborn.ipynb)



