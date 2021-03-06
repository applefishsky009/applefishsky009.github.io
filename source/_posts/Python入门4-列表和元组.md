---
title: Python入门4-列表和元组
date: 2016-12-14 15:45:47
category: Python
tags: Python

---

序列容器之列表和元组
列表，可变类型。
元组，不可变类型。

---

# 列表

## List

### Operator
1. `=`,`==`,`>`,`<`。列表的比较逻辑：两个列表元素分别比较，直到有一方的元素胜出。如果某两个元素相互之间不能比较会抛出一个TypeError。注意他的比较只是比较值，不会比较`id()`。
2. `in/not in` 成员关系操作
3. `+` 连接操作符，`extend()`方法可以代替连接操作符吧一个列表内容添加到另一个中去，而不是新建一个列表。
	+ `+=` 替换连接操作也支持；
	+ 注意连接操作符不能新建一个元素到列表，只能连接两个列表。
4. `*` 重复操作符

### Assignment
使用赋值运算符和方括号`[]`，或者内建的构造函数list(iterable)，可以从参数看到，传入可迭代对象，即可以从tuple,list,range,str创建list对象。

### Access
可以使用索引，或者切片操作符来访问列表值，并且类似于多维数组，可以在切片的结果上再次切片。

### Update
通过索引或者索引范围来更新列表，因为列表是可变的。

### List Comprehension
结合列表的方括弧和for循环，在逻辑上描述要创建的列表.
```Python
[i*2 for i in [8, -2, 5]] = [16, -4, 10]
[i for i in range[8] if i % 2 == 0] = [0, 2, 4, 6]
```

## Functions
1. len() 列表大小
2. max()/min() 最大最小值
3. sorted()/reversed() 字典序排序(排序后是另外一个对象)，反转(注意反转返回的是迭代器，需要list()转化，但这是另外一个对象了)
4. enumerate()/zip() 枚举，打包为元组
5. sum() 求和

## Methods
1. list.append() 附件一个对象到list
2. list.clear() 删除list中的所有元素
3. list.copy() 浅拷贝list
	+ [图解Python深拷贝和浅拷贝](http://www.cnblogs.com/wilber2013/p/4645353.html),简单来说，深拷贝会对每一个对象创造一个副本，而浅拷贝在能使用引用的时候绝对不用拷贝。
4. list.count() 指定参数在list中出现的次数
5. list.extend() 附加指定的元素到迭代器
6. list.index() 返回指定值在list中第一次出现的位置，如果不存在指定值则抛出错误
7. list.insert() 在list指定位置插入对象
8. list.pop() 删除并返回指定位置的对象，默认为最后位置，如果越界或者列表为空则抛出异常
9. list.remove() 从list中删除第一次出现的指定值，如果指定值不存在，抛出异常
10. list.reverse() 原地反转list中的元素
11. list.sort() 对列表排序(原地)


---

# 元组

## Tuple
元组和列表一个重要的区别：不可变类型，因此可以做一些列表不能做的事情，比如做字典的key.

### Assignment
使用赋值运算符和圆括号，值得注意的是<font color=red>只有一个元素时需要添加一个逗号`,`</font>，来区分工厂方法。
```Python
type((1)) = 'int'
type((1,)) = 'tuple'
```

### Acess
与列表切片操作一样，用方括号作为切片操作符，里边写上索引值或索引范围。

### Update
因为元组是不可变的，因此不能更新或者改变元组的值，字符串必须通过现有字符串片段再构造一个新的字符串的方式来解决，对元组也必须这样。

### Delete
因为元组不可变，删除一个单独的元素是不可能的，只有通过`del`来减少对象的引用计数。引用计数为0时会析构对象。Python中大部分时候并不需要显式析构。

### Operator
1. `=`,`*`,`+` 创建，重复和连接
2. `in/not in`,`[]`成员关系操作符，切片操作
3. `<`,`>`,`==` 运算符

## Functions
元组没有自己专用的内建函数，列表中描述的一些函数与其可变性有关，不适用于元祖。除此之外的一些适用于不可变迭代器的内建函数就可以用于元组。

## Methods
1. tuple.count() 指定参数在tuple中出现的次数
2. tuple.index() 指定参数在tuple中第一次出现的次数，如果没有出现抛出错误。

## 元组的不可变性

1. 不可变不完全是坏事，比如将数据传入一个不了解的API，不可变可以保证数据不被修改。而如果要修改函数返回的元组，可以用list()内建函数来转化。
2. 元组的不可变也没有那么可怕，通过连接，复制我们依然可以得到新的元组(尽管id不同了)，另外我们可以改变元组内部的一个可变对象来达到修改元组的目的(比如修改元组内部的列表元素)。
3. 所有的逗号分隔的没有明确符号定义的集合，默认类型都是元组，但最好是显式定义类型。

## list && tuple
1. 维护敏感数据->tuple
2. 管理动态数据集合->list

---

## Python浅拷贝和深拷贝

浅拷贝：
1. 完全切片操作
2. 利用工厂函数
3. 使用copy模块的copy函数
深拷贝：
1. 使用copy.decopy()函数

Waring:赋值运算符`=`并不会拷贝，这是类似于原引用的别名的东西(是同一个引用)。

1. 浅拷贝实际上是原对象的引用(新建的)，现在如果要改变其中的一个可变对象，那这两个对象访问的结果都会发生变化，如果这是一个联合账户(C++类中的静态对象)，那么这是期望的结果。但如果希望是相互独立的，这就导致了不期望的结果，此时需要深拷贝。
2. 但如果改变的对象是不可变对象(比如字符串)，由于这是不同的引用，这会导致在这个改变的操作中这个不可变对象被显式拷贝，因此改变前后的对象元素已经是不同的了。
3. 结合C++中的引用和类中的静态变量，以及Python中的引用计数机制，很容易理解。

---

参考资料：
1. [howsoftworks-builtins](http://www.howsoftworks.net/python.api/builtins/)
2. [howsoftworks-Builtin Function](http://www.howsoftworks.net/python/function/)

---
