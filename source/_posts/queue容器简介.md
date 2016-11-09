---
title: queue容器简介
date: 2016-09-07 11:17:37
category: STL
tags: [queue,priority_queue]

---

## queue

这个是简单的FIFO，不需要多说。

---

## priority_queue

他支持的操作与`queue`相同，唯一的区别在于最大/最小的元素被移动到队首,甚至可以自定义其比较的方式，这是通过对`<`的运算符重载来实现的；比如最常用的结构体排序方式。
详细用法参考[cplusplus - priority_queue](http://www.cplusplus.com/reference/queue/priority_queue/),主要是构造函数，最常用的模式(只提供模板参数)：
```C++
template <class T, class Container = vector<T>,
  class Compare = less<typename Container::value_type> > class priority_queue;
```
1. `class`:元素类型
2. `Container`:容器类型
3. `Compare`:二元谓词，可以用STL提供，预定义的关系函数符。可以是函数指针或者函数对象，默认是`less<T>`，在`functional`头文件里,可以查看[functional](http://www.cplusplus.com/reference/functional/)

结构体`less`函数符的关系运算符`<`重写(因为是调用`<`的)：
```C++
struct node{
	int a;
	int b;
	node(int ina,int inb):a(ina),b(inb){};
	bool operator < (const node &tmp) const{
	return b > tmp.b;
    }
};
```

举例：
1. [最短时间作业SJF - GitHub](https://github.com/applefishsky009/FunnyIssues/blob/master/7%20-%20SJF/6%20-%20SJF.cpp)





