---
title: set容器简介
date: 2016-05-27 10:14:47
category: STL
tags: C++

---

先上彩蛋：
1. [cplusplus](http://www.cplusplus.com/)
2. [cppreference](http://en.cppreference.com/w/)
3. [stackoverflow](http://stackoverflow.com/)
`#include<set>`头文件内定义了两个有序的hash容器`set`和`multiset`

---

## set简介

容器属性：
1. Associative:通过key而不是绝对位置来引用;
2. Ordered:有序;
3. Set:键值就是值本身；
4. Unique keys:这是一个一一映射
5. Allocator-aware:使用分配器`allocator`来动态存储。

特征补充：
1. 容器中的元素都是不可修改的(都是`const`类型)，但是可以`insert()`，`remove()`；
2. 容器通过二叉搜索树实现。

---

## set使用

1. [Subsets II(Recursion)][1]

[1]:https://github.com/applefishsky009/LeetCode/blob/master/90%20-%20Subsets%20II/90%20-%20Subsets%20II(Recursion).cpp

### insert()
```C++
pair<iterator,bool> insert (const value_type& val);
pair<iterator,bool> insert (value_type&& val);
```
`set`中没有则插入，返回一个`pair`对象，`pair.first()`指向插入的元素或set中已有的该元素，`pair.second()`指示该元素是已有的还是新插入的。

### 彩蛋
1. `#include <algorithm>`中[copy将set转vector](http://stackoverflow.com/questions/5034211/c-copy-set-to-vector)，当然也可以用`vector`的构造函数(iterator类型一致)。

---

暂没有用到其他方法，待补充
