---
title: 类的this指针
date: 2016-04-12 09:08:44
category: C++的类
tags: this指针

---

## C++类的this指针


如图，假设有一个类`Stock`，他有一个`private：int val`。他还有一个方法`const Stock& Stock::compare(const Stock &classIn) const`;这个方法要实现这样的功能：对于两个类`Stock`的对象a和b，比较a的`val`和b的`val`，返回`val`大的对象(a或者b)，可能性的写法如下：`a.compare(b)`。由于在类的方法定义中，还没有具体对象(a)。那么有这样一个问题，在方法`compare`中，如何返回以后才初始化的对象本身(也就是a)？
![为什么使用this指针](http://i.imgur.com/qChOATG.png)

1. 一般来说，所有类方法都将`this`指针设置为调用它的**地址**。(因此返回对象使用**`*this`**)
2. `compare`方法返回类型是*指针意味着返回的是调用对象本身，而不是其副本*。
3. `compare`方法的最后一个`const`表示该方法不会修改隐式访问对象(即调用该方法的对象本身)，这在之前的博客中提到过。
4. 形参列表中的`const`表示该函数不会修改被显示访问的对象(即图中的s)。
5. 由于该函数返回了两个`const`对象之一的引用，因此**返回类型也应该是`const`引用**。

![this的实例](http://i.imgur.com/CzuJ4fG.png)