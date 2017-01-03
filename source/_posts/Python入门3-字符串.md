---
title: Python入门3-字符串
date: 2016-12-13 09:45:58
category: Python
tags: Python

---

序列篇:字符串(str)，列表(list)，元组(tuple)，range
在python中list,tuple,range是序列容器，而str单独作为文本序列类型，因此很多特性都不尽相同。建议多看官方文档。

参考： [序列类型](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range)

---

## 操作符

### 成员关系操作符
`in/not in`，判断一个元素是否属于某一个序列。

### 连接操作符
连接操作符，`+`，但其效率并不是最高。
1. 对字符串来说，不如在列表或可迭代对象中调用`join`方法来把所有的内容连接在一起节约内存。
2. 对列表来说，可以用`extend`方法来把两个或者多个列表对象合并

### 重复操作符
`*`，返回一个序列的多份拷贝。

### 切片操作符
`[],[:][::]`,参数为头(2)+超尾(1)+步长(3)
1. 序列索引范围：`-len <= index <= len - 1 `,越界会引发`IndexError`异常；
2. 切片的返回值是原类型；
3. `None`可以作切片的索引，返回原字符串，但是Python3中`range()`的返回类型是`<class 'range'>`,可以转换后连接。

每次砍掉一个字符串：
```Python
s = 'abcde'
i = -1
for i in [None] + list(range(-1, -len(s), -1)):
    print(s[:i])
```

---

## Funtions

迭代的概念本身是从序列，迭代器，或者其他支持迭代操作的对象中泛化而来的。因此序列本身就包含了迭代的概念。

### 类型转化
各种序列的相互转化：
1. `str()`一般在一个对象需要打印对象的信息输出时特别有用，但通常对字符串应用`list()`和`tuple()`得不到我们想要的结果；
2. `list()`和`tuple()`在列表和元组互换时非常有效。

### 可操作函数
iter表示可迭代对象,seq表示序列。
1. len(seq) 返回seq长度;
2. max(iter, key = None),key是传给sort()的回调函数;
	+ max(arg0, arg1, ..., key = None)
3. min()用法同max
4. seq.index(x, start, end) 返回[start， end)区间x元素首次出现的位置。
5. seq.count() 返回x在seq中出现的次数。
6. str是不可变文本序列，可以通过切片来得到他的转置序列`str1 = str[-1:-len(str)-1:-1]`。

---

# str

字符串是不可变类型的，就是说改变一个元素需要创建一个新的字符串。

## 基本操作
1. 创建和赋值 不区分`'',""，`，他是直接量或者标量，不包含其他Python类型，考虑到他不可变以及不能单独改变字符串的一个字符或者一个子串。这些内容都是相互呼应的。
2. 访问字符串的值 索引方式来访问
3. 改变字符串 变量赋值，可以使用切片和连接符来得到
4. 删除字符串 `del`，一般情况下并不需要显式删除字符串。

## 操作符
1. 标准类型 `=`,`<`,`>`,`!=`
2. 序列操作符切片 头，超尾， 步长
3. 成员操作符 `in/not in`判断字符或者子串是否在另一个字符串中
4. 运行时字符串连接(`+`)
5. 编译时字符串连接(在源码中用`''`或者`""`)
6. 普通字符串转为unicode字符串，连接处理中如果有一个字符串是unicode字符串，Python会进行转化`u'abc'`

## 只适用于字符串的操作符
1. 格式化操作符(`%`) `'we are at %d%%' % 100`，另外，`repr()`和`str()`函数来辅助调试。
	+ 格式化操作符后边的可以是列表，元组等，可以在输出时避免迭代，尤其好用。
2. 原始字符串操作(`r/R`) 让程序员不必过多关注如何转义，编译器会完成这个工作。
	+ `r/R`必须紧靠在第一个引号之前；
3. Unicode字符串操作(`u/U`)
	+ Unicode操作符必须出现在原始字符串操作符之前。

## Funtions
1. len()
2. max(),min()
3. enumerate()
4. zip()
	+ 注意返回类型是zip，转化成list可视化
	+ 可以使用解析符号`*`unzip，`x,y=zip(*c)`，返回值x,y是tuple类型。

## Methods
共44个str相关方法，详见[How Soft Works.net Logo - str](http://www.howsoftworks.net/python.api/builtins/str_capitalize.html)。常见方法总结：
1. str.find() 返回子字符串在字符串中第一次出现的位置；如没找到返回-1。
2. str.format() 执行字符串格式化操作，替换字段使用{}分割。
	+ str.format_map()的替换字段可能是字典[Python 的内置字符串方法](https://segmentfault.com/a/1190000004598007)
3. str.isalnum() 判断字符串是否至少有一个字符，并且所有字符都是数字或者字符。
	+ 判断字符系列，共12个
4. str.join() 使用str连接多个数据，可以是字典，列表，元组。
5. str.split() 拆分字符串，返回一个列表。
6. str.translate() 根据映射关系，将字符串中的每个字符转换成另外一个字符。
	+ 和str.maketrans()配合，返回值是一个字典
7. str.upper()/str.lower() 字符串大小写转换。
8. str.zfill() 字符串左边填充0，不会截断字符串。

## 转义字符
1. 和其他语言一样，反斜杠转义
2. 三引号允许字符串跨行，字符串中可以包含换行符，制表符，以及其他特殊字符。
	+ 这个特性可以让程序员从特殊字符串的转义工作中解放出来(比如繁琐的html或者sql语句中)。
3. 字符串不可变，修改实际上都是新建字符串。

## Unicode
1. ASCII字符只能表示233个字符，而Unicode可以表示超过9w个。
2. 编码与解码 encode()/decode()
3. Python3中str()和chr()已经是Unicode(默认utf-8编码)，没有unicode()内建函数。