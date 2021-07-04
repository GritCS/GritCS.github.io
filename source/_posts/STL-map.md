---
title: STL-map
date: 2021-02-24 10:14:27
tags: [数据结构,stl]
---

# STL中map的基本用法

## pair

map中存储的基本数据类型是pair,首先我们简单介绍一下pair的基本结构

pair 定义在<map.h\>中，

**pair**是有两个变量的结构体，变量的默认权限是public，都可以访问，只是first设置为const，不能修改键值

### 底层实现

#### 结构定义

```c++
template <class T1,class T2>
   struct pair{
       typedef T1 first_type;
       typedef T2 second_type;
       
       T1 first;
       T2 second;
       
       pair(): first(T1()) ,second(T2()){}
       
       pair(const T1& a,const T2& b): first(a) ,second(b) { }
     #ifdef	__STL_MEMBER_TEMPLATES
       template<class U1,class U2>
       pair(const pair<U1,U2>& p) :dirst(p.first),second(p.second){}
       #endif  
   }

```

#### 重载

```c++
template <class T1, class T2>
inline bool operator==(const pair<T1, T2>& x, const pair<T1, T2>& y) { 
  return x.first == y.first && x.second == y.second; 
}


template <class T1, class T2>
inline bool operator<(const pair<T1, T2>& x, const pair<T1, T2>& y) { 
  return x.first < y.first || (!(y.first < x.first) && x.second < y.second); 
}
```

#### 成员函数 make_pair

```c++
template<class T1, classT2>
inline pair<T1,T2> make_pair(const T1& x,const T2& y){
		return pair<T1,T2>(x,y);
}
```

## map

map存储的基本数据类型是pair,数据结构是RB-tree，可以理解为关联数组。可以使用键作为下标来获得相应的值。关联的本质在于元素的值与某个特定的键相关联。

map元素的键值是唯一的，multimap允许重复元素

map的insert必须是以pair为存储结构，当然也可以直接使用make_pair构造一个临时pair

### 构造函数

```c++
/*
map c / multimap c; //产生一个空的map或者multimap，不包含任何因素
map c(op) / multimap c(op); //以op为排序准则，产生一个空的map
map c1(c2) / multimap c1(c2) ; //产生某个map或者multimap对象的副本，所有元素均被复制

map c(beg,end) /multimap c(beg,end); //以区间(beg,end)内的元素产生一个新的map/multimap

map c(beg,end,op) / multimap c(beg,end,op) //以区间(beg,end)内的元素产生一个新的map/multimap，排序准则为op
   */

map<int,double>  m1;
map<int,double,greater<int>> m2; //降序排列
map<int,double,less<int>> m3; //升序排列

//采用插入方式，增加元素
//对应迭代器的声明
map<int ,double>::iterator itm;
```

### 增加元素

```c++
    //方式1：提前先定义好pair
	pair<string, int> p;
	p.first = "zero", p.second = 0;
	m.insert(p); 
    
    //方式2：使用make_pair方式创建临时pair对象
	m.insert(make_pair("one", 1));
	
////[key] 自动添加，参考符号重载
	m["two"]=3;
	
	std::cout<<m["three"]; //输出0,默认值
```



### 基本属性获取

```c++
class map {
public:
	...
public:
    // 实际调用的是RB-tree的key_comp函数
  key_compare key_comp() const { return t.key_comp(); }
    // value_comp实际返回的是一个仿函数value_compare
  value_compare value_comp() const { return value_compare(t.key_comp()); }
    // 以下的begin, end等操作都是调用的是RB-tree的接口
    
   //迭代器
  iterator begin() { return t.begin(); }
  const_iterator begin() const { return t.begin(); }
    
  iterator end() { return t.end(); }
  const_iterator end() const { return t.end(); }
    
   
  reverse_iterator rbegin() { return t.rbegin(); }
  const_reverse_iterator rbegin() const { return t.rbegin(); }
  reverse_iterator rend() { return t.rend(); }
  const_reverse_iterator rend() const { return t.rend(); }
    
    //判断是否为空
  bool empty() const { return t.empty(); }
    
    //计算尺寸
  size_type size() const { return t.size(); }
  size_type max_size() const { return t.max_size(); }
    
    // 交换, 调用RB-tree的swap, 实际只交换count
  void swap(map<Key k, T t, Compare, Alloc>& x) { t.swap(x.t); }
    ...
};
template <class Key, class T, class Compare, class Alloc>
inline void swap(map<Key, T, Compare, Alloc>& x, 
                 map<Key, T, Compare, Alloc>& y) {
  x.swap(y);
}
```



### 重载

#### `==`,`<`

```c++
//大小比较，取决于值的大小
class map {
public:
	...
public:
  map(const map<Key, T, Compare, Alloc>& x) : t(x.t) {}
  map<Key, T, Compare, Alloc>& operator=(const map<Key, T, Compare, Alloc>& x)
  {
    t = x.t;
    return *this; 
  }
    ...
};
template <class Key, class T, class Compare, class Alloc>
inline bool operator==(const map<Key, T, Compare, Alloc>& x, 
                       const map<Key, T, Compare, Alloc>& y) {
  return x.t == y.t;
}

template <class Key, class T, class Compare, class Alloc>
inline bool operator<(const map<Key, T, Compare, Alloc>& x, 
                      const map<Key, T, Compare, Alloc>& y) {
  return x.t < y.t;
}
```

#### `[]`

```c++
class map {
public:
	...
public:
    // 1.  insert(value_type(k, T()) : 查找是否存在该键值, 如果存在则返回该pair, 不存在这重新构造一该键值并且值为空
	// 2.  *((insert(value_type(k, T()))).first) : pair的第一个元素表示指向该元素的迭代器, 第二个元素指的是(false与true)是否存在,  first 便是取出该迭代器而 * 取出pair.
	// 3.  (*((insert(value_type(k, T()))).first)).second : 取出pair结构中的second保存的数据
  T& operator[](const key_type& k) {
    return ( *(  (  insert(value_type(k, T()))   ).first)  ).second;
  }
    ...
};
```



### 常用操作

```c++
//swap() ,交换两个键值的内容
std::cout<<mm["1"]<<" "<<mm["2"]<<std::endl;
std::swap(mm["1"],mm["2"]); 
std::cout<<mm["1"]<<" "<<mm["2"];

//erase
iterator erase（iterator it);//通过一个条目对象删除

iterator erase（iterator first，iterator last）//删除一个范围

size_type erase(const Key&key);//通过关键字删除

clear()//就相当于enumMap.erase(enumMap.begin(),enumMap.end());



//count(const Key& key)
size_type count(const Key& key )  const;//函数功能是返回元素在map/multimap中出现的次数。对于map型容器，返回值只有 0/1
    

//find,其功能是返回指向key的迭代器。如果键值key对应的元素不存在，在函数返回迭代器end()
iterator find(const Key& key);
const_iterator find(const Key& key ) const;
    


```

### 补充1:

在STL中，map,set，multimap，multiset底层都是用红黑树实现，都是有序的

相关操作的复杂度

```
插入: O(logN)
查看:O(logN)
删除:O(logN)
```

hash_map, hash_set, hash_multimap, and hash_multiset
 上述四种容器采用哈希表实现，不同操作的时间复杂度为：

```
插入:O(1)，最坏情况O(N)
 查看:O(1)，最坏情况O(N)
 删除:O(1)，最坏情况O(N)。
```

unordered_map 和 hash_map的内部结构都是用哈希表来实现，但是在C++11中，hash_map已经被舍弃了，最好建议使用unordered_map

更详细的相关区别： https://blog.csdn.net/chen134225/article/details/83106569

参考资料：

https://github.com/GritCS/STL/blob/master/30%20map.md

