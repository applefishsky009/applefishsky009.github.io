---
title: Binary Search的陷阱
date: 2016-05-20 17:03:16
category: 数据结构与算法
tags: Algorithm

---

先上[二分搜索算法wiki](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%88%86%E6%90%9C%E7%B4%A2%E7%AE%97%E6%B3%95)，再上[二分查找写正确的方法](http://www.cppblog.com/converse/archive/2009/09/21/96893.aspx)。简单说说二分搜索：

主要用于<font color=red>有序数组</font>的遍历，时间复杂度O(log(n))，比之顺序遍历的时间复杂度O(n)更优，空间复杂度为O(1)。如用在插入排序中。

---

## 三点原则

1. 为了防止溢出折半时应该这么写`mid = start + (end - start) / 2;`；
2. 因为大部分情况不是大于就是小于，因此一般在最后检测相等(如下例，检测条件过于复杂，因此放在开始检测相等少写一个检测条件，写全条件容易出错，一般用于debug)。
3. 传入的一定是左闭右闭区间，因此递归入口为`low <= high`，由原则2可知递归出口判断写在入口之后。
4. <font color = red>2016.09.13修改</font>：受到STL启发，传入头和超尾，即左闭右开区间更合适。

---

## 陷阱

二分法，顾名思义，就是将区间一分为二传入递归函数，但是注意区间有开闭之分，因此在过程中一定要注意，
1. 不建议传入左开右开区间，增加判断次数，但是这样写极为简单，不得已可以使用，如例子中的`Solution`;
2. 传入左闭右闭区间，递归入口一定是`low <= high`,不能少等号;
3. 如例子中的`Solution1`所警示，在另外有区间判断的条件时注意`int`向下取整的特性，可能会有`low == mid; `需要特殊分析。

---

## 递归二分

1. [Search in Rotated Sorted Array](https://github.com/applefishsky009/LeetCode/blob/master/33%20-%20Search%20in%20Rotated%20Sorted%20Array/33%20-%20Search%20in%20Rotated%20Sorted%20Array.cpp),[Search in Rotated Sorted Array(2)][4]
2. [Search in Rotated Sorted Array II](https://github.com/applefishsky009/LeetCode/blob/master/81%20-%20Search%20in%20Rotated%20Sorted%20Array%20II/81%20-%20Search%20in%20Rotated%20Sorted%20Array%20II.cpp)
3. [Search for a Range](https://github.com/applefishsky009/LeetCode/blob/master/34%20-%20Search%20for%20a%20Range/34%20-%20Search%20for%20a%20Range.cpp)
4. [Search Insert Position](https://github.com/applefishsky009/LeetCode/blob/master/35%20-%20Search%20Insert%20Position/Search%20Insert%20Position.cpp)
5. [Pow(x, n)][1]
6. [Sqrt(float x)][2]
7. [Sqrt(int x)][3]
8. [Search a 2D Matrix](https://github.com/applefishsky009/LeetCode/blob/master/74%20-%20Search%20a%202D%20Matrix/74%20-%20Search%20a%202D%20Matrix.cpp)
9. [Median of Two Sorted Arrays](https://github.com/applefishsky009/LeetCode/blob/master/4%20-%20Median%20of%20Two%20Sorted%20Arrays/4%20-%20Median%20of%20Two%20Sorted%20Arrays.cpp)
	+ 大小数组个数和大于k，保证两数组左侧个数和是k;
	+ 保证二分搜索时小数组不越界，这样小数组删完了可以直接取大数组;
	+ 每次递归删掉二分的一般继续找，直到找到为止。
10. [Divide Two Integers](https://github.com/applefishsky009/LeetCode/blob/master/29%20-%20Divide%20Two%20Integers/29%20-%20Divide%20Two%20Integers.cpp)
	+ 倍数逐差法来优化普通的逐差，不知道编译器是否是这么完成的除法和余操作?
	+ 注意特殊边界的处理，特别是数据不对称性导致的非法值。

[1]:https://github.com/applefishsky009/LeetCode/blob/master/50%20-%20Pow(x%2C%20n)/50%20-%20Pow(x%2C%20n)%20.cpp
[2]:https://github.com/applefishsky009/LeetCode/blob/master/69%20-%20Sqrt(x)/69%20-%20Sqrt(float%20x).cpp
[3]:https://github.com/applefishsky009/LeetCode/blob/master/69%20-%20Sqrt(x)/69%20-%20Sqrt(int%20x).cpp
[4]:https://github.com/applefishsky009/LeetCode/blob/master/33%20-%20Search%20in%20Rotated%20Sorted%20Array/33%20-%20Search%20in%20Rotated%20Sorted%20Array(2).cpp

---

## 使用while的二分查找

不使用递归，使用while，传入左闭右闭(有人会写传入左闭右开)
1. [Search a 2D Matrix](https://github.com/applefishsky009/LeetCode/blob/master/74%20-%20Search%20a%202D%20Matrix/74%20-%20Search%20a%202D%20Matrix.cpp)