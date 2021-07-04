---
title: Broadcasting
date: 2020-08-24 23:43:31
tags: [pytorch, 大创]
categories: [深度学习,pytorch]
---

### 1.Broadcasting的基本准则

![2020-08-25_00-03](Broadcasting/2020-08-25_00-03.png)

例子

![2020-08-24_23-52](Broadcasting/2020-08-24_23-52.png)

### 2.Broadcasting引入的背景

	1. 实际计算需求：可以允许不同shape(但又满足某种标准)的tensor可以进行计算
 	2. 同时这种原则满足数学上的运算，又无需手动操作，又不会消耗很多的内存（对比repeat)

