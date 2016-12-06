---
title: STL简介
date: 2016-07-24 15:12:45
category: STL
tags: 

---

STL提供一组表示容器、迭代器、函数对象和算法的模板。
1. 容器存储若干个同质的值；
2. 算法完成特定任务；
3. 迭代器是广义指针，用来遍历容器对象；
4. 函数对象是类似于函数的对象，可以是类对象或函数指针。

注意STL不是面向对象的编程，而是泛型编程。

---

## vector

之前介绍，使用都比较多了，简单列举一些：
1. 重载`[]`，使用数组表示方来访问元素。
2. `vector`模板开头如下，省略模板第二个参数的值时容器模板默认使用`allocator<T>`类，这个类使用`new`和`delete`。

```C++
template <class T,class Allocator = allocator<T>>
	class vector{...}
```

---

## 矢量操作

STL容器的基本方法：
1. `size()`，返回元素数目；
2. `swap()`，交换两个容器内容(可以看作交换名字，`a.swap(b)`)，与`iterator`的`swap()`方法是不同的；
3. `begin()`，得到指向容器第一个元素的迭代器；
4. `end()`，得到指向容器超尾的迭代器。

迭代器，是一个广义指针，即一个指针或一个可对其执行类似指针操作的对象(包括解除引用`operator*()`和递增`operator++()`等)
1. `for_each(nums.begin(),nums.end(),XXXX)`可以避免显式使用迭代器变量,`XXX`代表对每个元素操作的函数；
2. `random_shuffle(nums.begin(),nums.end())`，随机排列区间元素；
3. `sort(nums.begin(),nums.end(),XXX)`，排序，`XXX`是指向要使用的函数的指针(函数对象)，可定制排序方式。
	+ 全排序中`a<b`，`a>b`都不成立，必然是`a=b`；
	+ 完整弱排序只是说明二者等价(只在比较/排序的项上相同)，不一定相同。

---

## 基于范围的for循环

需要注意传值还是传引用。
```C++
for (double x : prices)	//缺点：对数组没有下标索引
	cout << x << endl;

for_each(nums.begin(), nums.end(), show)
void show (const double &x){	//广义指针操作，依然没有下标索引，值不可变，但是例外，可查看参考文档
	cout << x << endl;
}

for(auto &x : nums) change(x);	//值可变，依然没有索引

```

