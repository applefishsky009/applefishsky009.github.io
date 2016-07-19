---
title: unordered_map容器简介
date: 2016-06-01 14:21:28
category: STL
tags: [unordered_map,unordered_multimap]

---

先上彩蛋：
1. [cplusplus](http://www.cplusplus.com/)
2. [cppreference](http://en.cppreference.com/w/)
3. [stackoverflow](http://stackoverflow.com/)
`#include<unordered_map>`头文件内定义了两个无序的hash容器`unordered_map`和`unordered_multimap`

---

## unordered_map简介

容器属性：
1. Associative:通过key而不是绝对位置来引用；
2. Unordered:无序，即通过hash表来组织数据，以支持通过key的快速访问；
3. Map:将key与mapped value映射，通过key访问mapped value；
4. Unique keys:这是一个一一映射
5. Allocator-aware:使用分配器`allocator`来动态存储。

特征补充：
1. `unordered_map`访问个体的速度比`map`更快，但是子集元素的范围迭代效率更低。
2. <font color = red>`value_type:pair<const key_type,mapped_type>`</font>

心得：
1. 他底层数据结构是散列表，因此搜索操作具有常数级的平均时间复杂度,[和map对比](http://ask.todgo.com/detail/6006173bc12b.html)；
2. `map`的查找效率比`unordered_map`稳定，其插入，删除，搜索的复杂度都是O(logn)，对于频繁的插入删除，优先使用`map`；
3. 如果仅需要在插入完毕后排序一次，可以考虑用`unordered_map`，最后sort那个arary。

---

## unordered_map使用

1. [Clone Graph][1]
2. [Permutations II](https://github.com/applefishsky009/LeetCode/blob/master/47%20-%20Permutations%20II/47%20-%20Permutations%20II.cpp)
	+ 随着sums的增长，在最后一步find()方法的复杂度n!增长，耗时太久;
	+ 在每一步中可以去掉相同的步，可以极大优化算法;
	+ 用hash_map统计出现的次数速度更快，于是用字符次数对来代替原数组(重复字符越多速度提升越明显)。
3. [Longest Consecutive Sequence](https://github.com/applefishsky009/LeetCode/blob/master/128%20-%20Longest%20Consecutive%20Sequence/128%20-%20Longest%20Consecutive%20Sequence.cpp)
	+ O(n)的时间复杂度应该联想到Hash Table；
	+ 和其他线性容器不同，他的删除操作很容易实现。
4. [Two Sum](https://github.com/applefishsky009/LeetCode/blob/master/1%20-%20Two%20Sum/1%20-%20Two%20Sum.cpp)
	+ 使用`unordered_map`，常数级搜索；
	+ 考虑{3,2,4，(3)},6；因此不能对半筛选，因此过滤相同下标。

[1]:https://github.com/applefishsky009/LeetCode/blob/master/133%20-%20Clone%20Graph/133%20-%20Clone%20Graph(BFS).cpp

### operator[]
```C++
mapped_type& operator[] ( const key_type& k );
mapped_type& operator[] ( key_type&& k );
```
运算符`[]`的重载，如果已有key，返回val;**如果没有key,插入这个key**,返回val的引用(因此可以直接赋值)。注意使用operator[]和find()具有不同意义。

### insert()
之前提到他的`value_type`是`pair<const key_type,mapped_type>`，因此使用insert()方法要用`make_pair()`方法来构建对象，返回对象也是指针和bool的pair。
`pair<iterator,bool> insert ( const value_type& val );`

### find()
```C++
iterator find ( const key_type& k );
const_iterator find ( const key_type& k ) const;
```
返回指向key的指针，如果没有则返回超尾unordered_map::end。他的两个主要用途可以使用`count()`方法(是否有Key)和`operator[]`(访问val)来代替。

### erase()
```C++
iterator erase ( const_iterator position );	//by position (1)	
size_type erase ( const key_type& k );	//by key (2)	
iterator erase ( const_iterator first, const_iterator last );	/range (3)
```
Hash Table的删除操作很容易实现(因为其数据结构不是线性的)，因此相对于线性结构赋值区分不同，删除更方便。

---

以后一一补充。