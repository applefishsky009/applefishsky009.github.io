---
title: map容器简介
date: 2016-05-16 12:42:19
category: STL
tags: [map,multimap]

---

先上彩蛋：
1. [cplusplus](http://www.cplusplus.com/)
2. [cppreference](http://en.cppreference.com/w/)
3. [stackoverflow](http://stackoverflow.com/)
`#include <map>`中定义了两个hash容器，`map`和`multimap`，他将键对象和值对象进行关联，值对象可以是新的`map`，因此可以形成多级映射。

---

## multimap简介

multimap是一种Hash Table。首先使用`multimap`必须使用宏语句`#include <map>`。MSDN上对multimap的解释已经比较清楚[multimap基础](http://blog.csdn.net/chenyujing1234/article/details/8193172)，[multimap与map，unorderedmap的对比](http://blog.csdn.net/xz_rabbit/article/details/43907311)
主要有以下几点：
1. multimap多重映照容器:容器的数据结构采用红黑树进行管理(还没有深入理解)；
2. multimap的所有元素都是pair:第一元素为键值(key),不能修改;第二元素为实值(value),可被修改 
3. multimap特性以及用法与map完全相同，唯一的差别在于: 允许重复键值的元素插入容器(每一个都是用一个**链表**来链接的);
4. `unordered_multimap`(目前还没有用到过)的无序存储特点，这是其与`multimap`最大的区别。
5. 参考[Word Ladder II](https://github.com/applefishsky009/LeetCode/blob/master/126%20-%20Word%20Ladder%20II/126%20-%20Word%20Ladder%20II.cpp)；

---

## multimap的使用

1. 初始化:`multimap<string, string> father;`，第一个是key类型，第二个是映照类型；
2. 插入数据:`father.insert(make_pair(string1, string2);`，[`pair`与`make_pair`介绍](http://www.cnblogs.com/Nimeux/archive/2010/10/05/1844191.html)。
3. 寻找某个键值:`pair<multimap<string, string>::iterator, multimap<string, string>::iterator> pos = father.equal_range(string1)`;`equal_range(string1);`注意其返回的是`pair`对象，`first`和`second`都是迭代器类型，他返回键值为`string1`的左指针和超尾(右)指针(最后一个键值为`string1`的下一个指针)，源码如下：

```
typedef pair<iterator, iterator> _Pairii;

_Pairii equal_range(const key_type& _Keyval)
{	// find range equivalent to _Keyval in mutable tree
	return (_Eqrange(_Keyval));
}
``` 


