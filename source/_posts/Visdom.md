---
title: Visdom
date: 2020-09-03 11:14:27
tags: [pytorch, 大创]
categories: [深度学习 , pytorch]
---

## visdom

### 简介

相比于tensorboard会更高效，而且展示图片时，不需要转换数据类型到`.cpu.numpy()`

**使用 visdom之前，必须先启动 visdom server**

```python
#在终端输入命令
python -m visdom.server
```

初始化visdom对象

```python
from visdom import Visdom
viz = Visdom()
```

### API

参考博客

https://www.pytorchtutorial.com/using-visdom-for-visualization-in-pytorch/#i-2