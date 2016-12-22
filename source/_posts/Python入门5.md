---
title: Python入门5
date: 2016-12-19 09:49:03
category: Python
tags: Python

---

映射类型：字典(dict)
集合类型：集合(set) 可变集合/不可变集合

---

# 字典

## dict

### Assignment
1. 赋值运算符`=`和花括号`{}`，键值之间的对应用冒号`:`。
2. 工厂方法dict(),键值之间可以用赋值运算符`=`。
3. 使用内建方法dict.fromkeys(seq,value),创建一个默认的字典，字典中的元素具有相同的值。

### Access
1. 键值访问 dict[key]返回value
2. 迭代器访问 for key in dict

### Update
1. 方括号和赋值运算符新建/更新字典，dict[key] = newValue;
	+ 如果没有key,则新建，如果有则更新；
2. 使用内建方法dict.update(dict)将整个字典的内容添加到另一个字典。

### Delete
1. del dict[key] 删除键为key的条目。
2. dict.clear() 删除dict中所有的条目。
3. dict.pop(key) 删除并返回键为key的条目的值。

### Operator
1. 赋值运算符`=`，Python3中取消了关系运算符`>`,`<`，等相关的运算符，但是`==`依然可用。
2. Python3中取消了`cmp()`内建函数。
3. 字典的键查找操作符`[]`，给某元素赋值或者查询字典中某元素的值。
4. 键成员关系操作`in/not in`。

## Built-in Functions
1. type() 返回对象的类型或创建一个新的类型对象。
2. str() 得到对象的str()版本。
3. len() 返回对象长度或者集合个数。
4. dict() 创建字典。
	+ 如果参数是可迭代的，必须成对出现才能创建字典，zip或者列表，列表解析等；
	+ 如果输入参数是一个映射对象，此函数会返回该映射对象的浅拷贝版本，但是效率没有dict.copy()高。
5. hash() 得到对象的哈希值。
	+ 一般用来判断对象是否是可哈希的，如果可哈希返回哈希值；
	+ 如果不可哈希，返回错误。

## builtins
1. dict.clear() 删除dictionary中的所有键值对，字典变为空。
2. dict.copy() 浅拷贝dictionary。
3. dict.fromkeys(iterater, value=None) 新建一个dictionary，键由可迭代对象组成，所有的值都是value。
4. dict.get(key, d) 返回dictionary中键为key的值，如果不存在key，则返回d(默认为None)，这可以判断键key是否存在。
5. dict.items() 返回dictionary中所有键值对，类型为dict_items，可以使用迭代器访问。
6. dict.keys() 返回dictionary中所有的键集合，类型为dict_keys，可以使用迭代器访问。
7. dict.values() 返回dictionary中所有的值集合，类型为dict_values，可以使用迭代器访问。
8. dict.pop(key) 删除dictionary中指定key，并返回key的值。如果没有key，会抛出KeyError。
9. dict.popitem() 删除dictionary中key-value对，类型是tuple()，如果dict是空，抛出异常。
10. dict.setdefault(k,d) 如果dictionary中不存在k,设置dict[k] = d,并返回d；如果k存在，则返回原有键k的值。
11. dict.update(dict/iterable) 使用一个字典或者可迭代容器(比如zip)更新字典(添加内容)。

## Warning
1. 不允许一个键对应多个值，以最后一次的赋值/更新为准。
2. 键必须是可哈希的
	+ 所有的不可变类型都是可哈希的，因为字典中的键用来计算数据存储位置，如果可变，存取数据就变得不可靠了。
	+ 数字和字符串都可以作为键，但对元组来说，元组只包括数字和字符串这样的不可变参数，才能作为字典中有效的值。

---

# 集合

分为可变集合和不可变集合，可变集合可以添加删除元素，不可哈希。不可变集合刚好相反。

## Set

### Assignment
只能通过工厂方法来创建，集合具有无序性，确定性，互异性。
1. set() 可变集合。
2. frozenset() 不可变集合。

### Access
1. `in/not in`。

### Update
1. set.add() 往set中添加一个元素。
2. set.remove() 从set中删除一个元素，这个操作和set.add()相对应。
3. set.update(set) 往set中添加一个set的所有元素。
4. set `-=` set() 从set中删去一个set的所有元素，这个操作与set.update()相对应。

### Delete
1. `del set`清除出当前的名称空间，如果引用计数为0，会被垃圾回收。

### Operator
注意`+`不是集合类型操作符，最接近`+`的逻辑是或(OR`|`)。
1. `in/not in` 成员关系操作符。
2.  `==/!=` 等价/不等价。
3.  `</<=,>/>=` 严格子集/非严格子集(`<=`相当于set.issubset())，严格超集/非严格超集(`>=`相当于set.issuperset())。
4.  `|` 联合，相当于OR,等价方法set.union()。
5.  `&` 交集，相当于ADD，等价方法set.intersection()。
6.  `-` 差补/相对补集，等价方法set.difference()。
7.  `^` 对称差分,即异或，等价方法set.symmetric_difference()。

以下仅适用于可变集合：
1. `|=` 从已存在的集合中添加多个成员，此方法和update()等价。
2. `&=` 保留(交集更新)，保留与其他集合的共有成员，此方法和intersection_update()等价。
3. `-=` 差更新，返回除去右集合后剩余元素组成的集合，此方法和difference_update()等价。
4. `^=` 差分更新，返回仅是左集合或者仅是右集合的元素组成的集合。

## Built-in Functions
内建函数。
1. len() 返回集合基数。
2. set()/frozenset() 生成可变/不可变集合。
	+ 默认生成空集合；
	+ 如果有一个参数，该参数必须是可迭代的。

## builtins
内建方法。
1. set.add() 往set中添加一个元素。
2. set.clear() 从set中移除所有元素。
3. set.copy() 返回set的浅拷贝。
4. set.difference() 返回两个或多个set中不同的元素组成的set，差分。
5. set.difference_update() 从当前set中移除其他set中所有元素。
6. set.discard() 从set中删除一个元素，如果该元素存在。无论该元素存在与否，返回值都是None。
7. set.intersection() 返回两个或多个set的交集。
8. set.intersection_update() 更新当前set，只保留所有set中都存在的元素、
9. set.isdisjoint() 判断两个set有没有交集。
10. set.issubset()/set.issuperset() 判断是否非严格子集/非严格超集。 

---