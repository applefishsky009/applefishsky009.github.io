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
```
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
```
可以看到他做了哪些工作,首先元素前移，然后毁掉最后一个对象(`capacity`不变)，之后<font color=red>_Orphan_range发生了什么？</font>,接下来指针指向正确的位置，最后返回指向原地址的迭代器。

