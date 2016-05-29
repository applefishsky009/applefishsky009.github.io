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

---

## DFS(深度优先搜索)

DFS需要用递归或者借助栈来**记录**走过的路径；每遍历完这条分支，便要**回溯**到上一层；在递归之前可以记录深度。
1. [Palindrome Partitioning](https://github.com/applefishsky009/LeetCode/blob/master/131%20-%20Palindrome%20Partitioning/131%20-%20Palindrome%20Partitioning.cpp)
2. [Word Ladder II](https://github.com/applefishsky009/LeetCode/blob/master/126%20-%20Word%20Ladder%20II/126%20-%20Word%20Ladder%20II.cpp)