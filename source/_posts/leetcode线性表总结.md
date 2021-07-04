---
title: leetcode线性表总结
date: 2021-03-27 15:20:16
tags: [leetcode,数据结构,机试]
---

## 经常使用的STL和数据结构



## 经典题型－－分类总结

### hash_map相关

#### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

> 给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
>
> 采用O(n)的解决方案：
>
> 思路1: set-遍历nums，对于每一个nums[i]，对外进行扩张，...num[i-1]...num[i+1]...查看是否都在数组中，直到不能扩张为止，
>
> **对于匹配的过程，暴力的方法是 O(n) 遍历数组去看是否存在这个数，但其实更高效的方法是用一个哈希表unordered_map存储数组中数，这样查看一个数是否存在即能优化至 O(1)的时间复杂度。**
>
> **同时考虑到同一连续序列向两边扩张的结果是相同的，所以我们增加一个访问标记，去重**
>
> unordered_map<int,bool> flags . flags.find(k)!＝flags.end()表明在该集合中，　flags[k]==true表明它属于某个连续的子序列中，不用要再扩张
>
> ​	时间复杂度：O(n),　空间复杂度：O(n)
>
> 
>
> 思路2：本身查找的过程类似于聚类，采用并查集，find,union..利用unordered_map<int,int> map用来存储
>
> (参考leetcode sheet)

#### [594.最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

和谐数组是指一个数组里元素的最大值和最小值之间的差别 **正好是 `1`** 

> 利用map存储时，按照key有序排列，map<int,int> count，　count[k]表示k在原数组中出现的次数[注意map只能利用key下标或者迭代器遍历]
>
> 如果pre_it->first == later->first-1,则可以构成和谐子序列，计算长度



### 二分法查找

####  [153.寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

####  [154.寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

####  [33.搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

####  [81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

> 刷题心得，这类题目特点:在每个分段序列中都是有序的，前面的最小值大于(等于)后半的最小值。
>
> 实际套路题，利用二分，只需分析清楚　在每种比较条件下，应该走哪个分支　[注意下标的开闭区间]





### 排序(归并思路)

#### [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)





## 常见套路/模板

### 位运算



### 单调栈





## 其他知识点

格雷码生成方式

排列数和组合数的生成算法