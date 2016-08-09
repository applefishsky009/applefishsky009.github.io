---
title: Two Pointers
date: 2016-07-21 16:08:38
category: 数据结构与算法
tags: Two Pointers

---

## 关于Two Pointers

一直以来致力于总结算法，方法。正如某些算法书作者所说，这个工作确实不好做，按数据结构显得大而空(精细的方法优化必然是很有针对性的)，而按照方法显得好很多，但是感觉必然总有一些问题徘徊于体系之外(或者说自成体系)，LeetCode上的Tags其实做的很好,一般来说是，数据结构+方法(技巧)。除去基本的数据结构，这些方法/技巧很具有启发意义。

遇到很多巧妙地利用`Two Pointers`的题目，感觉无法按照经典分类，这里使其自成体系，这绝对不过分，题目以后会慢慢填充，一般来说其具有以下几个特点：
1. 针对数组或链表
2. 针对数组一般是夹逼，begin指针，back指针。
3. 针对链表一般是记录位置，需要回溯。

举几个必要的例子，找单链表倒数第k个节点，水的最大容积，有序数组移除重复元素，三色排序，合并有序数组等。

---

## Two Pointer的实例
1. [3Sum](https://github.com/applefishsky009/LeetCode/blob/master/15%20-%203Sum/15%20-%203Sum.cpp)
	+ 先排序，注意跳过重复值，且遍历后再跳以便于第三个指针可以跳到前两个指针的重复值(如果有的话)；
2. [Remove Nth Node From End of List](https://github.com/applefishsky009/LeetCode/blob/master/19%20-%20Remove%20Nth%20Node%20From%20End%20of%20List/19%20-%20Remove%20Nth%20Node%20From%20End%20of%20List.cpp)
	+ 使用头结点不必对第一个逻辑结点单独处理。
3. [3Sum Closest](https://github.com/applefishsky009/LeetCode/blob/master/16%20-%203Sum%20Closest/16%20-%203Sum%20Closest.cpp)
	+ 夹逼的精髓是两侧逼近，如水的容积，正数组和为k的最长长度；
	+ 注意迭代器方法的使用可以简化编程。
4. [4Sum](https://github.com/applefishsky009/LeetCode/blob/master/18%20-%204Sum/18%20-%204Sum.cpp)
	+ 固定两个位置，两个指针夹逼；
	+ 注意提前减枝，去掉相同解。