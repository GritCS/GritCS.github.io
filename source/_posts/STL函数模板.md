---
title: STL函数模板
date: 2021-03-16 20:26:45
tags: [stl ]
---

# STL常见函数模板

## 查找

**利用二分查找的方法在一个排好序的数组中进行查找**

### ‘upper_bound' && 'lower_bound'

```c++
upper_bound(begin,end,num);
/*
对于从小到大的排序数组，upper_bound()从数组的begin位置到end-1位置二分查找第一个大于num的数字，如果找到，则返回该数组的迭代器;反之，返回end迭代器


对于从大到小的排序数组，upper_bound()从数组的begin位置到end-1位置二分查找第一个小于num的数字，如果找到，则返回该数组的迭代器;反之，返回end迭代器

*/

lower_bound(begin,end,num);
/*
对于从小到大的排序数组，upper_bound()从数组的begin位置到end-1位置二分查找第一个大于等于num的数字，如果找到，则返回该数组的迭代器;反之，返回end迭代器


对于从大到小的排序数组，upper_bound()从数组的begin位置到end-1位置二分查找第一个小于等于num的数字，如果找到，则返回该数组的迭代器;反之，返回end迭代器

*/

```

应用：

leetcode: Remove Duplicates from Sorted Array

## 删除

### 'unique()'

```c++
unique(); //用于去除相邻的重复元素（只保留一个），使用前需要对数组进行排序.
//实际上只是把重复元素放到数组的后方
//返回，去重后最后一个元素的地址
```

