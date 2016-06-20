---
title: valarray容器(数值处理)
date: 2016-06-10 15:40:05
category: STL
tags: valarray

---

`#include<valarray>`，头文件valarray支持了用于数值处理的valarray类,可以在[cplusplus](http://www.cplusplus.com/reference/valarray/)上查看其特性。在此做一个简单介绍即可。**最后尽量用它来代替vector**

---

## valarray简介

### 构造函数
常用的两个构造函数：
```
explicit valarray (size_t n);
valarray (const T& val, size_t n);
```

### 运算符重载
他实现了很多运算符的重载，除了最基本的`+`,`-`,`*`,`/`重载，基本所有算术运算符都重载了。

### 类方法
一些简单的`sum()`,`min()`,`max()`,`size()`,`swap()`等。

### 其他方法

他可以调用一些数学方法，`abs()`,`cos()`,`sin()`,`exp()`，`log()`等。