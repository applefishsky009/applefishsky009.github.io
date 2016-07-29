---
title: 其他非STL库
date: 2016-07-29 14:57:52
category: 非STL库
tags: [valarray,array,initializer_list]

---



---

## valarray和array

1. `vector`模板是容器类和算法系统的一部分，他支持面向容器的操作；
2. `valarray`模板是面向数值计算的，之前简单介绍过，比如重载基本运算符，定义数学方法等；
3. `array`为代替内置数组设计，他通过更好更安全的接口，让数组更紧凑，<font color=red>效率更高</font>。

注意：
1. `valarray`的接口更简单，但是性能并不更高，而且他没有`begin()`,`end()`方法，凡是他有`begin(val)`,`end(val)`函数，他还有二值化值域，扩展的下标访问等功能。
	+ `valarray<bool> vbool = numbers > 9`;
	+ `a[slice(1,4,3)]`没有计算功能，需要转化为`valarray`:`valarray<int> b(a[slice(1,4,3)])`;
2. `array`提供了多个STL方法，但是他固定长度(因此改变长度的方法他都没有)；

---

## initializer_list

C++11新增，使得STL容器能够使用初始化列表语法，容器类会包含一个`initializer_list<T>`作为参数的构造函数。
1. 如果类有接受`initializer_list`作为参数的构造函数，则使用`{}`语法将调用该构造函数；
2. 初始化列表构造函数不能进行隐式的缩窄转化；
3. 包含头文件`<initializer_list>`构造`initializer_list`对象，这个模板类包括`begin()`,`end()`,`size()`等成员函数，还重载了赋值运算符`=`；
4. `initializer_list`迭代器类型为`const`。