---
title: map容器简介
date: 2016-05-16 12:42:19
category: STL
tags: C++

---

先上彩蛋：
1. [cplusplus](http://www.cplusplus.com/)
2. [cppreference](http://en.cppreference.com/w/)
3. [stackoverflow](http://stackoverflow.com/)
`#include <map>`中定义了两个hash容器，`map`和`multimap`，他将键对象和值对象进行关联，值对象可以是新的`map`，因此可以形成多级映射。

---

## multimap简介

容器属性：
1. Associative:通过key而不是绝对位置来引用；
2. Ordered:有序；
3. Map:将key与mapped value映射，通过key访问mapped value；
4. Multiple equivalent keys:这是一个一对多映射
5. Allocator-aware:使用分配器`allocator`来动态存储。

特征补充：
1. <font color = red>`value_type:pair<const key_type,mapped_type>`</font>
2. multimap多重映照容器:容器的数据结构采用红黑树进行管理；
3. multimap特性以及用法与map完全相同，唯一的差别在于: 允许重复键值的元素插入容器(每一个都是用一个**链表**来链接的);

---

## multimap使用
1. [Word Ladder II](https://github.com/applefishsky009/LeetCode/blob/master/126%20-%20Word%20Ladder%20II/126%20-%20Word%20Ladder%20II.cpp)；

### insert()
```C++
single element (1)	iterator insert (const value_type& val);
with hint (2)	iterator insert (const_iterator position, const value_type& val);
range (3)	void insert (InputIterator first, InputIterator last);
initializer list (4)	void insert (initializer_list<value_type> il);
```
有序插入


### equal_range()
```C++
pair<const_iterator,const_iterator> equal_range (const key_type& k) const;
pair<iterator,iterator>             equal_range (const key_type& k);
```
寻找某个键值的所有对象，返回pair对象(两个指针,左闭右开,将右开的指针称为超尾)。如果没有找到，这两个指针都会指向超尾。


