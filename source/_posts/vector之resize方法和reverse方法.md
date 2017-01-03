---
title: vector之resize方法和reverse方法
date: 2016-04-15 14:39:13
category: STL
tags: C++

---

## a.resize()和a.reserve();

1. 首先介绍容器的两个属性`capacity`和`size`。`capacity`存储区的大小；`size`容器的大小。
2. `reserve()`是预分配存储区的大小，预分配存储区，但存储区不一定有容器对象。
3. `resize()`是改变容器大小，容器中一定有容器对象。

---

### a.reserve();
看源码：

```
void reserve(size_type _Count)
{	// determine new minimum length of allocated storage
	if (capacity() < _Count)
	{// something to do, check and reallocate
		if (max_size() < _Count)
			Xlen();
		_Reallocate(_Count);
	}
}
```
1. 没有`else`，说明若当前的`capacity`大于传入的值，**`capacity`是不会减小的**；
2. 里层的if是错误检测机制；
3. 验证`vector`的`reallocate`原理，实际上每次新的`capacity`是之前的1.5倍。因此在**循环之前一定要`reserve`保证效率**。

---

### a.resize();
模板类提供了两个函数重载：

| 表达式	| **返回类型**	| 说明	|
| :---:	| :---:	| :---:	|
| `a.resize(n)`	| void	| 容器大小设为n	|
| `a.resize(n,t)`	| void	| 容器大小设为n，必要时用t填充	|

#### a.resize(n)
```
void resize(size_type _Newsize)
{	// determine new length, padding as needed
	if (_Newsize < size())
		erase(begin() + _Newsize, end());
	else if (size() < _Newsize)
	{	// pad as needed
		_Alty _Alval(this->_Getal());
		_Reserve(_Newsize - size());
		_TRY_BEGIN
		_Uninitialized_default_fill_n(this->_Mylast, _Newsize - size(),_Alval);
		_CATCH_ALL
		_Tidy();
		_RERAISE;
		_CATCH_END
		this->_Mylast += _Newsize - size();
	}
}
```

1. 若容器新大小小于现在的大小，毁掉多余的对象；
2. 新大小大于现在的大小：
	+ 注意`_Reserve`和`.reverse`是两个不同的方法，一个比较`capacity`，一个比较`_Unused_capacity`；
	+ 检查空间，不够则`reverse`；
	+ 填充未初始化的对象
	+ 修改尾指针

####a.resize(n,t)
```
void resize(size_type _Newsize, const value_type& _Val)
{	// determine new length, padding with _Val elements as needed
	if (_Newsize < size())
		erase(begin() + _Newsize, end());
	else if (size() < _Newsize)
		_Insert_n(end(), _Newsize - size(), _Val);
}
```

1. 若容器新大小小于现在的大小，毁掉多余的对象；
2. 新大小大于现在的大小，直接执行`insert`;

---