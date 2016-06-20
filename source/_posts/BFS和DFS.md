---
title: BFS和DFS
date: 2016-04-21 15:28:51
category: 数据结构与算法
tags: [BFS,DFS]

---

持续更新,该贴记录一些BFS和DFS在使用过程中的心得体会，以加深对BFS与DFS的理解。

---

## BFS(广度优先搜索)

BFS需要借助一个**队列**来记录遍历的"层数"；对每个节点的邻接节点咬判断是否入队(入队就是该节点的下一层)，如果需要记录步长，要将`pair`模板压入队列；队列为空，结束搜寻。
1. [Word Ladder](https://github.com/applefishsky009/LeetCode/blob/master/127%20-%20World%20Ladder/127%20-%20World%20Ladder.cpp)
2. [Word Ladder II](https://github.com/applefishsky009/LeetCode/blob/master/126%20-%20Word%20Ladder%20II/126%20-%20Word%20Ladder%20II.cpp)
3. [Surrounded Regions](https://github.com/applefishsky009/LeetCode/blob/master/130%20-%20Surrounded%20Regions/130%20-%20Surrounded%20Regions.cpp)
4. [Binary Tree Level Order Traversal II](https://github.com/applefishsky009/LeetCode/blob/master/107%20-%20Binary%20Tree%20Level%20Order%20Traversal%20II/107%20-%20Binary%20Tree%20Level%20Order%20Traversal%20II.cpp)
5. [Binary Tree Zigzag Level Order Traversal](https://github.com/applefishsky009/LeetCode/blob/master/103%20-%20Binary%20Tree%20Zigzag%20Level%20Order%20Traversal/103%20-%20Binary%20Tree%20Zigzag%20Level%20Order%20Traversal.cpp)
	+ `vector`当`stack`来使用。

---

## DFS(深度优先搜索)

DFS需要用递归或者借助栈来**记录**走过的路径；每遍历完这条分支(所有可能性)，便要**回溯**到上一层(撤销操作)；在递归之前可以记录深度。
有四点需要特别说明：**递归出口(DFS深度控制)**，**递归入口(在DFS中何时调用DFS)**，**撤销操作(同一层继续找递归入口)**，**递归出口深度控制需要传递深度参数**。
1. [Palindrome Partitioning](https://github.com/applefishsky009/LeetCode/blob/master/131%20-%20Palindrome%20Partitioning/131%20-%20Palindrome%20Partitioning.cpp)
	+ 使用递归的DFS
2. [Word Ladder II](https://github.com/applefishsky009/LeetCode/blob/master/126%20-%20Word%20Ladder%20II/126%20-%20Word%20Ladder%20II.cpp)
	+ 使用栈的DFS；
3. [Unique Paths](https://github.com/applefishsky009/LeetCode/blob/master/62%20-%20Unique%20Paths/62%20-%20Unique%20Paths.cpp)
	+ 二分DFS与DP；
4. [Unique Paths II](https://github.com/applefishsky009/LeetCode/blob/master/63%20-%20Unique%20Paths%20II/63%20-%20Unique%20Paths%20II.cpp)
	+ 带判断的二分递归与DP；
5. [N-Queens](https://github.com/applefishsky009/LeetCode/blob/master/51%20-%20N-Queens/51%20-%20N-Queens.cpp)
	+ 使用递归的DFS，递归出口(DFS深度控制)，递归入口(在DFS中何时调用DFS)，以及回溯时撤销操作(同一层继续找递归入口)；
	+ 判断是否在对角线上：行列差相等。
6. [Permutations](https://github.com/applefishsky009/LeetCode/blob/master/46%20-%20Permutations/46%20-%20Permutations.cpp)
	+ 按位置遍历，注意出口，入口，往前走一步(覆盖所有可能性)，回溯。
7. [Permutations II](https://github.com/applefishsky009/LeetCode/blob/master/47%20-%20Permutations%20II/47%20-%20Permutations%20II.cpp)
	+ 随着sums的增长，在最后一步find()方法的复杂度n!增长，耗时太久;
	+ 在每一步中可以去掉相同的步，可以极大优化算法;
	+ 用hash_map统计出现的次数速度更快，于是用字符次数对来代替原数组(重复字符越多速度提升越明显)。
8. [N-Queens II](https://github.com/applefishsky009/LeetCode/blob/master/52%20-%20N-Queens%20II/52%20-%20N-Queens%20II.cpp)
	+ 使用`vector<int> C`来记录每一层/行(i)的Q在哪一列(j)。