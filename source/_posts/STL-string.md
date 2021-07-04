---
title: STL-string
date: 2021-04-09 14:30:32
tags: [stl,string,字符串处理]
---

使用时，需要`#include<string>`

## string定义和初始化

```c++
string str;
string str = "abcd";
```

## string中内容的访问

```c++
//1.直接通过下标访问
string str = "abcd";
str[0];

//2.通过迭代器访问
#include<stdio.h>
#include<string>
using namespace std;
int main(){
    string str ="sadas";
    for(string::iterator it = str.begin();it!=str.end();it++){
        cout<<*it<<" ";
    }
}
//和vector一样，string的迭代器支持加减整数操作
```

## string常用的函数

#### 运算符

```c++
//operator+=，支持两个string直接拼接
string str1 = "abc",str2 = "xsf",str3;
str3 = str1+str2; // str3 = "abcxsf";
str1 += str2; //str1="abcxsf"


//compare operator == ，!= ,< , <= ,>,>= 比较大小,比较规则是字典序
string str1 = "aa" , str2 = "aaa" ,str3 = "abc" ,str4 = "xyz";
bool b1 = str1<str2; //true
bool b2 = str3>str2;//true
```

#### 增删改查

```c++
length()/size() //返回字符串长度
 str.substr(int pos , int len); //返回从pos下标开始，长度为len的子串，时间复杂度为O(len)


//==========增===========
insert(int pos,string); //在pos号下，插入字符串string
insert(iterator it , iterator str , iterator end);//将[str,end)之间的字符串 插入到it所指的位置之前

string str = "safs" , str2="134";
str.insert(3,str2); //str = "saf123s"
string str3 = "abc";
str2.insert(str2.begin()+1,str3.begin(),str3.end());//str2 = "1abc34";

//===========删=O(N)==================
str.erase(iterator it); //用于删除单个元素
str.erase(iterator first, iterator last); //用于删除[first,last)区间
str.erase(int pos , length);//
string str = "dasdfas

    
 str.clear();//清空字符串

//=======find=======
str.find(str2); //当str2是str的子串时，返回其在str中第一次出现的位置，如果str2不是str子串，返回-1

//===替换
str.replace(int pos, int len , str2);//把str从pos下标开始的，长度为len的子串替换为str2(str2的长度可以超过len,)
str.replace(iterator it1, iterator it2,str2);
    
```



## 相关库函数

```c++
bool  isalnum(char c); //用于检测一个字符是否是字母或者十进制数字

bool isalpha(char c);//用于检测一个字符

bool isdigit(char c);//用于检测一个字符是否是十进制数

    #include <stdio.h>
    #include <ctype.h>
    int main ()
    {
        int i = 0, n = 0;
        char str[] = "*ab%c123_ABC-.";
        while(str[i])
        {
            if( isalnum(str[i]) ) n++;
            i++;
        }
        printf("There are %d characters in str is alphanumeric.\n", n);
        return 0;
    }

///There are 9 characters in str is alphanumeric.
```

### 常用操作

#### 大小写字母转换([transform](https://blog.csdn.net/lihaidong1991/article/details/96098070?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-0&spm=1001.2101.3001.4242))

```c++
//===大小写字母转换===transform
/*
template <class InputIterator, class OutputIterator, class UnaryOperation>
  OutputIterator transform (InputIterator first1, InputIterator last1,
                            OutputIterator result, UnaryOperation op);
    
template <class InputIterator1, class InputIterator2,
          class OutputIterator, class BinaryOperation>
  OutputIterator transform (InputIterator1 first1, InputIterator1 last1,
                            InputIterator2 first2, OutputIterator result,
                            BinaryOperation binary_op);
*/
#include <algorithm>
#include <string>
	string strsrc("Hello, World!");
	transform(strsrc.begin(), strsrc.end(), strsrc.begin(), to_upper);	// 转换为大写
 
	string strsrc("Hello, World!");
	transform(strsrc.begin(), strsrc.end(), strsrct.begin(), to_lower); // 转换为小写
```

版权声明：本文为CSDN博主「ningto.com」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/tujiaw/article/details/6976424