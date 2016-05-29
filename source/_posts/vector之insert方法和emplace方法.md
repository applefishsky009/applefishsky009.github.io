---
title: vector之insert方法和emplace方法
date: 2016-04-13 20:39:29
category: STL
tags: [vector,insert(),emplace()]

---



## a.insert();

模板类提供了三个函数重载,这是一种**拷贝**插入方法：

| 表达式	| 返回类型	| 说明	|
| :---:	| :---------: | :---:	|
| `a.insert(p,t)`	| 迭代器	| 指向原本指向的元素,将t插入到p前面	|
| `a.insert(p, n,t)` | `void`	| 将n个t插入到p前面	|
| `a.insert(p,i,j)`	| `void`	|将区间[i,j)插入到p的前面，注意是左闭右开区间，j可以是超尾	|

---

## a.emplace();

新标准引入的`emplace_front`,`emplace`,`emplace_back`这些操作是构造而不是拷贝元素。当插入一个对象时，将会比`insert`少拷贝构造，析构的步骤。

---

### 注：
1. 调用`push`或`insert`成员函数，将元素类型对象传递进去，这些对象被拷贝到容器中；
2. `emplace()`在容器中构造元素，因此效率更高；
3. 注意`a.insert(p,t)`的源码提供了一个重载，若t是普通类型，则调用`a.emplace(p，t)`，若t是`const`类型，则调用`a.insert(p,t)`。
