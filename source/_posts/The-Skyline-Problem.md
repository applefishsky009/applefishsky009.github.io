---
title: The Skyline Problem
date: 2017-04-14 09:54:01
category: 数据结构与算法
tags: Algorithm

---

[218. The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/#/description)这个问题非常有意思，与[SJF(短作业优先算法)](https://github.com/applefishsky009/FunnyIssues/blob/master/6-SJF/6-SJF.cpp)极为相似，使用`priority_queue`配合`pair`来解题非常方便。由此延伸到`utility`和`tuple`头文件的理解与关联。

---

## 解题 

关于这个题的理解[The skyline problem](https://briangordon.github.io/2014/08/the-skyline-problem.html)阐述的较为清楚。解题的重点在于高权值在存活时间内覆盖低权值的“活动”，而所要求的`keyPoint`就是每当权值发生变化时的横坐标-权值对。

### 普通解法
在简单的解法中，每有一个活动开始，更新它存活时间内的权值最大值，由此就得到所谓天际线，得到`keyPoint`。但在这个解法中，时间复杂度和空间复杂度都与这个活动可能的持续总时间相关，而与活动个数`n`没有直接关系，也就是说，极可能出现`kn`的时间复杂度，`k`值非常大，`k`的空间复杂度，这里的`k`甚至会引起`bad_alloc`。

普通解法代码，仅供理解。
```C++
class Solution
{
  public:
    vector<pair<int, int>> getSkyline(const vector<vector<int>> &buildings)
    {
        vector<pair<int, int>> keyPoint;
        if (!buildings.size())
            return keyPoint;
        const int n = buildings[buildings.size() - 1][1];
        vector<int> segTree(n + 2, 0);
        for (auto x : buildings)
            for (int i = x[0]; i < x[1]; i++)
                segTree[i] = max(x[2], segTree[i]);
        for (int i = 1; i < n + 2; i++)
            if (segTree[i] != segTree[i - 1])
                keyPoint.push_back(make_pair(i, segTree[i]));
        return keyPoint;
    }
};
```


### 优先级队列
之前提到解题的重点在于高权值在权值时间内覆盖低权值的“活动”，显然，一个以权值为`key`的优先级队列可以满足需要，同时需要携带`Y`值信息来判断活动是否结束，由于活动按照`X`值排好序，那么沿`X`轴进行扫描(只在活动开始或者结束时检查)，可能的操作有：
1. 在当前时间点开始的所有活动入队列，检查权值是否发生变化，如果变化则产生一个关键点。
2. 随着`X`的扫描，所有超过存活时间的活动出队列，同时检查权值是否发生变化，如果变化则产生一个关键点。

```C++
class Solution
{
  public:
    vector<pair<int, int>> getSkyline(vector<vector<int>> &buildings)
    {
        vector<pair<int, int>> res;
        int cur = 0, cur_X = 0, cur_H = -1, len = buildings.size();
        priority_queue<pair<int, int>> liveBlg;
        while (cur < len || !liveBlg.empty())
        {                                     
            cur_X = liveBlg.empty() ? buildings[cur][0] : liveBlg.top().second;

            if (cur >= len || buildings[cur][0] > cur_X)
            { 
                while (!liveBlg.empty() && (liveBlg.top().second <= cur_X))
                    liveBlg.pop();
            }
            else
            {
                cur_X = buildings[cur][0];
                while (cur < len && buildings[cur][0] == cur_X)
                {
                    liveBlg.push(make_pair(buildings[cur][2], buildings[cur][1]));
                    cur++;
                }
            }
            cur_H = liveBlg.empty() ? 0 : liveBlg.top().first;
            if (res.empty() || (res.back().second != cur_H))
                res.push_back(make_pair(cur_X, cur_H));
        }
        return res;
    }
};
```
[【STL学习】优先级队列Priority Queue详解与C++编程实现](http://blog.csdn.net/xiajun07061225/article/details/8556786)
> 优先级队列内部的元素并不是按照添加的顺序排列，而是自动依照元素的**权值**排列。权值最高者排在最前面。缺省情况下，优先级队列利用一个<font color=red>**大顶堆**</font>完成。

[优先队列](https://zh.wikipedia.org/wiki/%E5%84%AA%E5%85%88%E4%BD%87%E5%88%97)
> 标准模板库（STL）以及1998年的C++标准确定优先队列是标准模板库的容器适配器模板。其实现了一个需要三个参数的最大优先队列：比较函数（缺省情况是小于函数less<T>）、存储数据所用的容器类型（缺省情况是向量vector<T>）以及指向序列开始和结束位置的两个迭代器。和标准模板库中其他的真实容器不同，优先队列不允许使用其元素类型的迭代器，而必须使用优先队列抽象数据类型的迭代器。标准模板库还实现了支持随机访问数据容器的优先队列--二叉最大堆。Boost C++库也在其中实现了堆结构。


---

## 从utillity到tuple

在优先级队列中发现了一个很有意思的现象，没有重写`pair`类型的比较函数就能构造一个默认的`grater`优先级队列，这是如何进行的？

### utility
[utility (C++)](https://en.wikipedia.org/wiki/Utility_(C%2B%2B))头文件包括两个关键部分：
1. `rel_ops`(relational operators)是一个命名空间，它包含一组模板，这些模板定义了同类型的关系运算符(`！=`,`>`,`<=`,`>=`)的默认行为。这些默认行为是基于用户定义的`==`,`<`两个行为的(也就是说这两个关系<font color=red>必须被定义</font>)。
2. `pair`是一个容器模板，他可以包含任意类型的两个成员对象。通过(`first`和`second`来访问成员)，值得注意的是，头文件定义了他<font color=red>**所有的**</font>关系运算符。从这里[relational operators (pair)](http://www.cplusplus.com/reference/utility/pair/operators/)也能看到。
	+ `pair`使用默认的构造函数对数据成员进行值初始化。
	+ `pair`的数据成员是`public`的，两个成员啊分别命名为`first`和`second`。
	+ 新标准下，我们可以对返回值进行列表初始化，就像`python`中那样，但旧的标准中必须显式构造返回值。另外还可以用`mnake_pair`来生成`pair`对象。

这就就容易解释`pair`在优先级队列中并不需要重写比较函数这一现象。另外，在[pair](http://www.cplusplus.com/reference/utility/pair/)中可以看到以下描述，而`tuple`这个结构不能不让人想到`Python`中的`tuple`。
> Pairs are a particular case of tuple.

### tuple
上节提到`pair`是`tuple`的特例，其实他是类似`pair`的模板，
1. `tuple`的默认构造函数会对每个成员进行值初始化。
2. `tuple`的成员都是未命名的，可以使用`get<i>(t)`来访问成员。
3. 可以使用辅助类模板`tuple_size<tupleType>::value`和`tuple_element<i,tupleType>::type`来查询`tuple`成员的数量和类型。`tupleType`可以通过`typedef decltype(t) tupleType`来推断。
4. 同类型的`tuple`才可以进行比较。
5. `tuple`常用来从一个函数返回多个值。



---

一个简单的tips:
关于出栈顺序的题目，以1,2,3,4,5,6元素为例，如果当前位出现了6,之后小于6的元素肯定是<font color=red>**逆序**</font>，从思维上来说，不必使用DFS的思路来找正确答案，类似于一个DP问题，当前当前位是否合理与他之后的元素顺序有关。

