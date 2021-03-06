---
title: STL算法库
date: 2016-07-28 10:01:48
category: STL
tags: Algorithm

---

这里主要是指`<algorithm>`和`<numeric>`头文件。
STL算法库包含一些处理容器的非成员函数，查阅[参考文档](http://www.cplusplus.com/reference/algorithm/)得到详细信息。这些算法函数设计，思路主要是两个通用部分：
1. 使用模板来提供泛型；
2. 使用迭代器来提供访问容器中的数据的通用表示。

这些容器设计使不同类型的容器之间有明显的联系，比如：
1. 不同容器之间的`copy()`;
2. 不同容器之间的`==`。

使用STL时应尽可能减少要编写的代码，STL算法是经过仔细选择的，并且是内联的。

---

## 算法组分类

1. 非修改式序列操作(Non-modifying sequence operations);
2. 修改式序列操作(Modifying sequence operations);
3. 排序和相关操作(Sorting,Partitions,Merge等);
4. 通用数字计算(<numeric>)

---

## 就地算法和复制算法

就地算法(in-place algorithm)就地完成工作(如`sort()`)，复制算法(copy algirithm)创建拷贝(如`copy()`)。有的可以以两种方式工作(如`transform()`，他的输出迭代器可以指向输入区间)

STL约定：
1. 复制版本的名称以`_copy`结尾，他仅仅是只读只写，因此输入输出迭代器足够了(如`replace_copy()`)；
2. 就地版本，同时需要读写数据，因此至少需要正向迭代器(如`replace()`)；
3. `_if`变体：根据 将函数应用于容器元素得到的结果 来执行操作(如`replace_if()`)。

---

## STL与string类兼容

`string`类不是STL组件，但是他设计时考虑了STL，包含`begin()`,`end()`,`rbegin()`,`rend()`等成员，因此可以使用STL接口；
得到字符串的所有组合(两个方法都体现兼容性)：
1. `next_permutation()`得到下一种排列方式；
2. `sort()`得到最初的排列方式，二者组合即能得到所有组合。

---

## 函数和容器方法

STL方法和STL函数对比而言，方法是更好的选择，因为方法是针对特定容器设计的，一般使用可能的最低需求。
`remove()`方法和函数：
1. `remove()`方法删除容器中指定值的元素；
2. `renmove()`函数调整 指定值所在结点值 为 下一个非指定值的值 ，并返回新的超尾；因此重要的一点是容器大小不变，之后`erase()`新超尾和原超尾区间才能达到`remove()`方法的效果。