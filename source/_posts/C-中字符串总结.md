---
title: C++中字符串总结
date: 2021-02-01 14:57:37
tags: [ c++]
---

## c++中string，字符指针，字符数组之间的区别

### 常量

c/c++中设置有常量：在程序运行过程中，其值不能改变的量称为常量

常量分为不同的类型：整型常量(3)，浮点型常量(3.12)，字符型常量('a')，字符串常量("abc")

常量一般有两种表现形式：

- 直接常量：直接以值的形式表示的常量称之为直接常量
- 符号常量：用标识符命名的常量称为符号常量，也就是在直接常量上再取一个名字，方便程序后续维护．习惯用大写字母和下划线来命名
  - 两种定义方式
    - const 类型　符号常量名字＝常量值
    - #define 符号常量名　常量值

### 字符指针和字符数组的区别

c/c++中**每个字符串都以字符'/0'作为结尾**．为了节省内存，c/c++把常量字符串放到单独的一个内存区域，当几个指针赋值给相同的常量字符串时，它们实际会指向相同的内存地址．但是用常量内存初始化数组时，情况却有所不同．

```c++
char str1[] = "hello world";
char str2[] = "hello world";
//str1!=str2
//c++会为str1,str2分配两个长度为12个字节的空间，并把"hello world"的内容分别复制到数组中．
//str1,str2是两个不同的字符数组，所以初始地址不同


char* str3 = "hello world";
char* str4 = "helo world";
//str3==str4
```



参考资料

＜剑指offer

< http://c.biancheng.net/view/2236.html