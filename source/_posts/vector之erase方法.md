---
title: vector之erase方法
date: 2016-04-14 10:52:25
category: STL
tags: [vector,erase()]

---

## a.erase();

模板类提供了两个函数重载：

| 表达式	| **返回类型**	| 说明	|
| :---:	| :---:	| :---:	|
| `a.erase(p)`	| **迭代器**	| 删除p指向的元素	|
| `a.erase(p,q)`	| **迭代器**	| 删除区间[p,q)中的元素	|
**思考**:
1.	在之前内存之中讨论过，`vector`是保证内存连续的，那么`erase`之后他是如何保证内存连续的？答案是让该元素之后的所有元素前移补充“空位”。这一点在`erase()`方法的代码中可以看到传入的p的形参是`const`类型的，也就是说p指向的地址并不发生变化，只不过是元素前移了。测试如下：
![STL的迭代器](http://i.imgur.com/TZxRRb6.png)
2. 注意到`erase`返回的都是迭代器，由1中的分析可知，删除p指向的元素之后，他返回的迭代器的**地址不变**，值是删除之后可用的下一个元素，因此**给人感觉是p指向了下一个元素**。那么在这里有一个值得注意的问题，如下：
``` 
	vector<int>::iterator p = b.begin();
	for (;p!=b.end();p++)
	{
		if (*p == 2)
			b.erase(p);
	}
```
这是错误的，`b.erase(p)`没有迭代器来承接p的返回值，它的返回值被释放了。然后执行p++很显然会出现野指针。但是如果修改为`p = b.erase(p)`也是不正确的。迭代器在进行删除的这一个循环里会`++`两次（`erase`可以当做`++`一次）。但如果在`if`语句中执行一次`p--`，这是正确的。即`p = --b.erase(p);`
1. 不能用`p-- = b.erase(p);`，因为`p--`是表达式，不能为左值；
2. 不能用`p = b.erase(p--);`，也是因为`p--`是一个表达式，强调计算结果，不能作为左值，也不能取址。

 **迭代器的循环使用`while`可以降低错误率。**

---

为简单，分析第一个表达式的源码如下：
```C++
iterator erase(const_iterator _Where)
{	// erase element at where
	if (_VICONT(_Where) != this
		|| _VIPTR(_Where) < this->_Myfirst
		|| this->_Mylast <= _VIPTR(_Where))
		_DEBUG_ERROR("vector erase iterator outside range");
	_Move(_VIPTR(_Where) + 1, this->_Mylast, _VIPTR(_Where));
	_Destroy(this->_Mylast - 1, this->_Mylast);
	_Orphan_range(_VIPTR(_Where), this->_Mylast);
	--this->_Mylast;
	return (_Make_iter(_Where));
}
void _Orphan_range(pointer _First, pointer _Last) const
		{	// orphan iterators within specified (inclusive) range
		_Lockit _Lock(_LOCK_DEBUG);
		const_iterator **_Pnext = (const_iterator **)this->_Getpfirst();
		if (_Pnext != 0)
			{	// test an iterator
			while (*_Pnext != 0)
				if ((*_Pnext)->_Ptr < _First || _Last < (*_Pnext)->_Ptr)
					_Pnext = (const_iterator **)(*_Pnext)->_Getpnext();
				else
					{	// orphan the iterator
					(*_Pnext)->_Clrcont();	//WTF!!!!!!!!!!!
					*_Pnext = *(const_iterator **)(*_Pnext)->_Getpnext();
					}
			}
		}
```
可以看到他做了哪些工作,首先元素前移，然后毁掉最后一个对象(`capacity`不变)，之后<font color=red>_Orphan_range将这个位置以及之后的孤儿迭代器失效了！WTF!!!</font>(也许是基于安全考虑),接下来指向正确的尾部元素，最后<font color=red>总算有点良心</font>，返回值是`erase()`元素之后的下一个元素(也认为是位置不变)，即返回指向原地址的迭代器。

## 迭代器的有效性

```C++
vector<int>::iterator t;	//便于说明此处不初始化
vector<int>::iterator k = result.begin();
vector<int>::iterator p = result.begin() + 1;
vector<int>::iterator q = result.begin() + 2;
t = result.erase(p);
```
看这个句法，以此为例，在[Cplusplus](http://www.cplusplus.com/reference/vector/vector/erase/)上提到这一点，：
>Iterators, pointers and references pointing to position (or first) and beyond are invalidated, with all iterators, pointers and references to elements before position (or first) are guaranteed to keep referring to the same elements they were referring to before the call.

大意是指向p以及之后的孤儿迭代器都不可用了(p和q)，但是指向p之前的孤儿迭代器以及`erase()`方法返回的孤儿迭代器都是可用的(k和t)。

因此在使用多个迭代器的时候一定要注意迭代器是否有效(特别是涉及`erase()`和`insert()`方法)，这种情况下最好使用`index`下标作为循环而不是孤儿迭代器。

1. [Merge Intervals](https://github.com/applefishsky009/LeetCode/blob/master/56%20-%20Merge%20Intervals/56%20-%20Merge%20Intervals.cpp)
	+ 注意Two Pointers的用法

