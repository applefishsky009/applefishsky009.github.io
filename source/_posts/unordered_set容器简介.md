---
title: unordered_set简介
date: 2016-04-19 19:42:25
category: STL
tags: [unordered_set,unordered_multiset]

---

先上彩蛋：
1. [cplusplus](http://www.cplusplus.com/)
2. [cppreference](http://en.cppreference.com/w/)
3. [stackoverflow](http://stackoverflow.com/)
`#include <unordered_set>`头文件内定义了两个无序hash容器，`unordered_set`和`unordered_multiset`。

---

## unordered_set简介

容器属性：
1. Associative:通过key而不是绝对位置来引用;
2. Unordered:无序，即通过hash表来组织数据，以支持通过key的快速访问；
3. Set:键值就是值本身；
4. Unique keys:这是一个一一映射
5. Allocator-aware:使用分配器`allocator`来动态存储。

特征补充：
1. `unordered_set简介`访问个体的速度比`set`更快，但是子集元素的范围迭代效率更低。

---

## unordered_set简介

1. [Word Ladder II](https://github.com/applefishsky009/LeetCode/blob/master/126%20-%20Word%20Ladder%20II/126%20-%20Word%20Ladder%20II.cpp)；

### insert()
因为key的唯一性，只有在没有这个key的情况下才能插入成功`_pair<iterator,bool> insert ( const value_type& val );`;

### find()
```C++
iterator find ( const key_type& k );
const_iterator find ( const key_type& k ) const;
```
若找到该元素，返回的指针指向该元素，没找到返回的指针会指向超尾即`unordered_set::end`。

### erase()
```C++
by position (1)	iterator erase ( const_iterator position );
by key (2)	size_type erase ( const key_type& k );
range (3)	iterator erase ( const_iterator first, const_iterator last );
```
擦除指定值，容器重排。

### count()
`size_type count(const Key& keyval) const;`返回unordered_set中指定键对应的元素个数，由于其唯一性，只能返回1(存在)或0(不存在)。

