---
title: vector简介及非STL函数用法-持续更新
date: 2016-04-12 14:54:50
category: STL
tags: [vector,sort()]

---

## vector

`vector`模板类是最简单的序列类型，除非其他类型的特殊有点能够满足程序的要求，否则应该默认使用这种类型。以后将记录一些在编程过程中常用的`vector`方法作为笔记以便不时复习，必要时会分析源码。

常见的方法有：
`.insert()`，`a.emplace()`，`.resize()`，`.reverse()`，`.begin()`，`a.end()`，`a.rbegin()`，
`a.rend()`，`a.size()`，`a.swap(b)`，`a.empty()`，`a.front()`，`a.back()`，`a.clear()`，
`a.push_back(t)`，`a.popback(t)`，`a[n]`，`a.at(n)`。

有`vector<int> a`;`vector<int> b`;即a,b是`vector<int>`的对象。`vector<int> ::iterator p`;p是指向`vector<int>`的迭代器。i、j、q均和p一样是指向`vector<int>`的迭代器。

---

## sort()用于vector;

在`algorithm>`头文件中提供了`sort`的两个重载函数,查看源码可发现是用**快排**实现的。

| 表达式	| **返回类型**	| 说明	|
| :---:	| :---:	| :---:	|
| `sort(p,q)`	| `void`	| 对[p,q)升序排序		|
| `sort(p,q,cmp)`	| `void`	| 对[p,q)使用`cmp`方法排序，`cmp`	|

1. 右闭左开区间，一般来讲`p = a.begin()`;`q = a.end()`完成了对容器的排序。
2. 升序排序直接用第一个方法，系统默认`a<b`返回真，因此是升序。
3. 降序排序需要自定义`cmp`方法,方法如下，只需要将默认值改为`a>b`。

```
	bool comp(const int &a,const int &b)
	{
   		return a>b;
	}
```

---

以上参考了[这里](http://www.cnblogs.com/cj695/p/3863142.html "这里")


