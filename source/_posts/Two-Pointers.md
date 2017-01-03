---
title: Two Pointers
date: 2016-07-21 16:08:38
category: 数据结构与算法
tags: Algorithm

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
5. [Remove Element](https://github.com/applefishsky009/LeetCode/blob/master/27%20-%20Remove%20Element/27%20-%20Remove%20Element.cpp)
	+ 模拟`remove()`函数。
6. [Linked List Cycle](https://github.com/applefishsky009/LeetCode/blob/master/141%20-%20Linked%20List%20Cycle/141%20-%20Linked%20List%20Cycle.cpp)
	+ 快慢指针相遇，说明链表有环。
7. [Linked List Cycle II](https://github.com/applefishsky009/LeetCode/blob/master/142%20-%20Linked%20List%20Cycle%20II/142%20-%20Linked%20List%20Cycle%20II.cpp)
	+ 设快慢指针相遇时慢指针走了\\( s \\)步，环长为\\( r \\)，那么对快指针：\\( 2s = s+nr \\)；即有关系\\( s = nr \\)；(注意相遇时慢指针一定没有走完链表一次);
	+ 设链表长度为\\( L \\),链表头到环结点的距离为\\( x \\),环结点到快慢指针相遇结点的距离为\\( a \\),那么\\( s(x+a) = nr((n-1)*r+L-x) \\);
	+ 即有关系：\\( x = (n-1)r+(L-x-a) \\),\\( L-x-a \\)在链表上代表相遇结点到环结点的距离！
	+ 记住结论：快慢指针相遇时，两个慢指针，一个从链表头出发，一个从相遇点出发，他们相遇在环入口。


$$$$