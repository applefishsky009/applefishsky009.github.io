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

## push_back()

平时使用`push_back()`方法觉得非常简单，但是有一些值得注意的地方：

### 迭代器有效性
如果在`push_back()`时候有内存分配行为(reallocation)，他所有的迭代器都会失效，这是因为容器可能不在原先的内容处而被拷贝到另一处。 因此可以在`push_back()`之前调用`reverse()`方法。
```C++
void push_back(const value_type& _Val)
	{	// insert element at end
	if (_Inside(_STD addressof(_Val)))
		{	// push back an element
		size_type _Idx = _STD addressof(_Val) - _Unfancy(this->_Myfirst());
		if (this->_Mylast() == this->_Myend())
			_Reserve(1);
		_Orphan_range(this->_Mylast(), this->_Mylast());
		this->_Getal().construct(_Unfancy(this->_Mylast()),this->_Myfirst()[_Idx]);
		++this->_Mylast();
		}
	else
		{	// push back a non-element
		if (this->_Mylast() == this->_Myend())
			_Reserve(1);
		_Orphan_range(this->_Mylast(), this->_Mylast());
		this->_Getal().construct(_Unfancy(this->_Mylast()),_Val);
		++this->_Mylast();
		}
	}
```

---

## sort()用于vector;

在`algorithm>`头文件中提供了`sort`的两个重载函数,查看源码可发现是用**快排**实现的。

| 表达式	| **返回类型**	| 说明	|
| :---:	| :---:	| :---:	|
| `sort(p,q)`	| `void`	| 对[p,q)升序排序		|
| `sort(p,q,cmp)`	| `void`	| 对[p,q)使用`cmp`方法排序，`cmp`	|

```C++
	bool comp(const int &a,const int &b)
	{
   		return a>b;
	}
```

1. 右闭左开区间，一般来讲`p = a.begin()`;`q = a.end()`完成了对容器的排序；
2. 升序排序直接用第一个方法，系统默认`a<b`返回真，因此是升序；
3. 降序排序需要自定义`cmp`方法,方法如下，只需要将默认值改为`a>b`；
4. 第二种表达式的主要作用是对一些自己构造的数据结构可以自定义排序方法，可以这么写：
```C++
sort(nums.begin(), nums.end(), [](int a, int b) {return a < b;});
```

例子：
1. [Merge Intervals](https://github.com/applefishsky009/LeetCode/blob/master/56%20-%20Merge%20Intervals/56%20-%20Merge%20Intervals.cpp)

---

以上参考了[这里](http://www.cnblogs.com/cj695/p/3863142.html "这里")


