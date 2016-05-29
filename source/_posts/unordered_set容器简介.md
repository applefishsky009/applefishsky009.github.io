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

首先使用`unordered_set`必须使用宏语句`#include <unordered_set>`。MSDN上有对[unordered_set](https://msdn.microsoft.com/zh-cn/library/bb982739.aspx)描述，CSDN上有博客对[unordered_set](http://blog.csdn.net/oabid/article/details/4562577)描述，这是一个哈希表。一般来说，一些简单功能可以当做STL里的容器来用：`insert()`、<font color=red>`find()`</font>、`erase()`、`size()`、`empty()`、`begin()`、`end()`。
1. 参考[Word Ladder II](https://github.com/applefishsky009/LeetCode/blob/master/126%20-%20Word%20Ladder%20II/126%20-%20Word%20Ladder%20II.cpp)；

---

## unordered_set(无序关联容器)与vector的异同

1. `_Pairib insert(const value_type& _Val);`;基本与`vector`用法相同，参数为要插入的值。
2. `iterator find(const key_type& _Keyval);`;若找到该元素，返回的指针指向该元素，没找到返回的指针会指向超尾即`end()`。与`size_type find(_Elem _Ch, size_type _Off = 0) const;`有区别，后者会返回下标(`size_type`可以看做一种足够大的`unsigned`类型来表示下标)，如果没找到，返回`string::npos`。而
3. `size_type erase(const key_type& _Keyval);`;擦除指定值，返回该指定值的位置。`iterator erase(const_iterator _Where);`擦除指针指向的值，容器重排，指针不变。
4. `size_type count(const Key& keyval) const;`返回unordered_set中指定键对应的元素个数，k-要查找的key值。

