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
9. [Interleaving String](https://github.com/applefishsky009/LeetCode/tree/master/97%20-%20Interleaving%20String)
	+ DFS传递步数，使用两个指针判断，但是会超时(在release版本下复杂样本测试时间超过30s)。
10. [Combinations](https://github.com/applefishsky009/LeetCode/blob/master/77%20-%20Combinations/77%20-%20Combinations.cpp)
	+ start-标识当前步, cur-path中的元素数量(可以用path.size()代替);
	+ 这题自己写的时候走了不少弯路，想用`unordered_set<int>`标识当前可走的步数，用`set<int>`对path排序，用`unodered_set<vector<int>>`来防止重复，但`unodered_set<vector<int>>`需要重写hasher，比较复杂，因此用了对`vector<vector<int>>`用了find方法，这在大样本下难免会超时；
	+ 从上可以看出其**深度参数**的重要性，参数左边是已经走过的步，参数右边就是所有可能走的步数；
	+ 另外注意Backtracking是在当前步走完，递归，就要立即回溯。
11. [Restore IP Addresses](https://github.com/applefishsky009/LeetCode/blob/master/93%20-%20Restore%20IP%20Addresses/93%20-%20Restore%20IP%20Addresses.cpp)
	+ 每一层的选择不一定这么简单，就像这个问题，尤其注意剪枝；
	+ 每一层的遍历尽量用`for`，不符合条件的剪掉就行。当前层剪不掉的可以在下一层入口剪掉，例如例子中余下字符数过多过少的问题。
12. [Same Tree](https://github.com/applefishsky009/LeetCode/blob/master/100%20-%20Same%20Tree/100%20-%20Same%20Tree.cpp)
	+ 及其简单的一个DFS。
13. [Letter Combinations of a Phone Number](https://github.com/applefishsky009/LeetCode/blob/master/17%20-%20Letter%20Combinations%20of%20a%20Phone%20Number/17%20-%20Letter%20Combinations%20of%20a%20Phone%20Number.cpp)
	+ 递归版本比较简单，掌握四要素：1.递归出口；2.传递当前步数；3.当前步数可能值循环；4.回溯(因为是按值传递path)
	+ 非递归版本比较复杂，按内存复制。
14. [Combination Sum](https://github.com/applefishsky009/LeetCode/blob/master/39%20-%20Combination%20Sum/39%20-%20Combination%20Sum.cpp)
	+ 保证输入排序；
	+ 提前减枝提升性能。
15. [Symmetric Tree](https://github.com/applefishsky009/LeetCode/blob/master/101%20-%20Symmetric%20Tree/101%20-%20Symmetric%20Tree.cpp)
	+ 利用重载来改变接口为自己想要的；
	+ 不能用中序遍历判断vector是否对称来做，以1233N2N(层次顺序)为例。
16. [Combination Sum II](https://github.com/applefishsky009/LeetCode/blob/master/40%20-%20Combination%20Sum%20II/40%20-%20Combination%20Sum%20II.cpp)
	+ 严格按照层dfs，每层使用一次dfs，不要调用第二次；
	+ 对数组来说，未决定的所有剩余数字都可作为本层输入。
17. [Generate Parentheses](https://github.com/applefishsky009/LeetCode/blob/master/22%20-%20Generate%20Parentheses/22%20-%20Generate%20Parentheses.cpp)
18. [Word Search](https://github.com/applefishsky009/LeetCode/blob/master/79%20-%20Word%20Search/79%20-%20Word%20Search.cpp)
	+ DFS解答，注意模拟`Hash Table`记录已经遍历的路径防止重复使用元素。