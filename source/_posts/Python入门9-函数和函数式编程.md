---
title: Python入门9-函数和函数式编程
date: 2017-01-06 11:09:07
category: Python
tags: Python

---

## 函数基础

### 简介
1. python的过程就是函数，因此解释器会隐式返回默认值`None`。即如果保存一个没有返回值的函数的返回值，该值就为`None`。
2. Python里的函数可以返回一个值或者对象。由于元组语法上不一定需要圆括号，会让人以为可以返回多个对象，实际上是一个元组。
	+ 元组的保存方式也有三种，整个元组变量，单一变量，带圆括号的单一变量。

### 调用
1. 函数操作符`()`相当于类的实例化。
2. 允许参数缺失或者不按顺序，解释器能通过给出的关键字来匹配参数的值。关键字参数结合默认参数可以跳过缺失参数。
3. 默认参数，给参数赋予默认值，和C++中一样。
4. 使用`*`和`**`指定元组和字典作为非关键字参数和关键字参数组。完整语法为(所有参数都是可选的)：
	`func(positional_args,keyword_args,*tuple_grp_nonkw_args,**dict_grp_kw_args)`

### 创建
1. `def`关键字，函数的名字，参数集合。建议添加文档字串和函数体。
2. 定义和声明有区别的语言，往往是因为函数的定义可能和其声明放在不同的文件中。Python将二者视为一体，函数由声明的标题和随后的定义体组成。
3. 在a函数定义中的b函数调用不会出现前向引用问题，除非在a函数调用的时候b函数依然没有被定义。
4. 句点属性标识，意味着不同的命名空间。函数声明中不能访问属性，因为此时函数体还没有被创建。定义后通过<font color=red>函数名</font>(而不是实例名)和句点属性标识来访问修改属性值。
5. 内部/内嵌函数：整个函数体在外部函数的作用域，或者lambda表达式。
	+ 如果内部函数定义包含了外部函数内定义的对象的引用，内部函数会成为**闭包**(传递了作用域)。
6. 装饰器：以`@`开头，需要返回<font color=red>函数对象</font>，实际上是把要装饰的函数参数传递给装饰器。其返回的函数对象是一个包装了的函数。并将其重新赋值为原来的标识符，并永久失去对原始函数对象的访问。
	+ [PYTHON修饰器的函数式编程](http://coolshell.cn/articles/11265.html)
	+ 下边是类式的装饰器。

```Python
class myDecorator(object):
 
    def __init__(self, fn):		#装饰时调用
        print("inside myDecorator.__init__()")
        self.fn = fn
 
    def __call__(self):		#调用时调用
        self.fn()
        print("inside myDecorator.__call__()")
 
@myDecorator
def aFunction():
    print("inside aFunction()")

print("Finished decorating aFunction()")
 
aFunction()
```
输出：
```Python
inside myDecorator.__init__()
Finished decorating aFunction()
inside aFunction()
inside myDecorator.__call__()
```

### 函数传递
1. 所有对象都是通过引用传递的，函数也不例外。因此可以将函数作为参数传入其他函数来调用。函数内部通过函数操作符`()`来调用函数。

### 参数传递
1. 按照被调用函数中定义的顺序来准确传递参数(就像C++一样)
2. 默认参数：和C++一样，在函数声明中使用赋值运算符。
	+ 所有的必需的参数都必须在默认参数之前。否则编译器无法确定匹配方式。
	+ 使用关键字参数可以不按顺序提供参数，也可以用于跳过缺失参数。 
	+ 默认参数<font color=red>一定要是不可变对象</font>，否则不同的实例会操作同一个对象，实例之间会有数据污染。参考[Python函数参数默认值的陷阱和原理深究](http://cenalulu.github.io/python/default-mutable-arguments/)。

### 可变长度参数
1. 函数调用时，形参将值赋给函数声明中相对应的局部变量。剩下的非关键字参数按顺序插入到一个元组中便于访问，关键字参数按顺序插入到一个字典中便于访问。
2. 使用星号操作符`*`和双星号操作符`**`之后标识的形参将参数传递给函数。
	+ `**`是被重载的，因为他也可以表达幂运算。
	+ 函数声明中所有的形参必须位于非正式的参数之前。
	+ 关键字字典必须是最后一个参数且非关键字参数在他之前。
3. 如果在函数调用之外创建传入的字典和元祖，函数内部最终接受的元组和字典是被调函数的超集。

---

## 函数式编程

### lambda
1. 这种语句的目的是由于性能的原因，在调用时绕过函数的栈分配。
2. 默认参数和可变参数是允许的。

### 内建函数
1. `filter(function, iterable)`->`function`重构为`labmda`->`filter`重构为列表解析式(`filter`相当于列表解析中的`if`)->重构为嵌套的列表解析。
2. `map(function, iterable, ...)`映射函数->`function`重构为`labmda`->(在处理单个序列时)重构为列表解析(`for...in...`迭代循环)。
	+ `...`表示多个序列传入`function`。
	+ `map`函数在`function`为`None`时可以将不相关的序列归并在一起，相当于`zip`。
	+ `map`独特的优势在于处理多个序列的映射。
3. `functools.reduce(function, iterable,initializer)`将列表元素减少为单一的值，如果给定初始化器，那么一开始的迭代会用初始化器和一个序列的元素进行，接着会正常进行。
	+ [functools.reduce(function, iterable[, initializer]](https://docs.python.org/3/library/functools.html)可以看到代码逻辑。
	+ `reduce((labmda:x,y:x+y),range(5))` = `((((0 + 1) + 2) + 3) + 4)`

### 偏函数
PFA:[Partial Function Apply](https://docs.python.org/3/library/functools.html)
1. `functools.partial(func, *args, **keywords)`创建PFA。
2. 参数顺序传入`func`，可以使用关键字参数且其总是出现在形参之后。

---

## 变量作用域

### 全局变量和局部变量
1. 全局变量的一个特征是除非被删除掉，否则他们会存活到脚本运行结束。
2. Python会先从局部作用域开始搜索，如果在局部作用域内没有找到，那么会在全局作用域或者内建作用域中找到这个变量，否则就会抛出`NameError`异常(LGB原则)。
3. 由2的规则可知，通过创建一个局部变量来隐藏全局变量是有可能的。
4. `if`,`try`,`for`不会产生新的作用域，`def`,`class`,`lambda`等才会产生。

### global语句
1. 在局部作用域中通过`global`语句来明确引用一个已命名的全局变量。
2. 如果不明确引用，只要不定义同名变量局部编译器依然会搜索到全局变量。

### 闭包
1. 如果在一个函数内，对外部作用域(但不是全局作用域)的变量进行引用，那么内部函数就被认为是闭包。这是函数式编程中一个很重要的概念。
2. 闭包仅仅是带了额外特征的函数---另外的作用域。

### lambda
1. `lambda`表达式定义了新的作用域。

### 变量作用域和名称空间
1. 从函数内部，局部作用域包围了局部名称空间，第一个搜寻名字的地方。如果名字存在的话，那么将跳过检查全局作用域。

---

## 递归与生成器

### 递归
1. 函数内部包含对函数自身的调用，和C++一样。

### 生成器
#### 生成器的动机
1. 函数可以在迭代中以某种方式生成下一个值并且返回和`next()`调用一样。
2. 协同程序：函数可以暂停或者挂起，并从程序离开的地方继续或者开始。
3. 挂起返回出中间值并多次继续的协同程序被成为生成器。

#### 生成器的定义
1. 函数中使用`yield`关键字，当到达一个真正的返回或者函数结束没有更多的值返回，会抛出一个`StopIteration`异常。
2. 获得和保存一个生成器对象之后，使用`next(obj)`或者`obj.__next__()`来生成下一个值。
3. 生成器不停的挂起或者继续，其状态是保留的，那么当在生成的过程中如何动态调整生成的值？这就需要介绍`send()`函数。
	+ `yield val`这个表达式是有返回值的，`next(obj)`其实相当于`send(None)`，默认情况下返回的是`None`，可以通过`send(val)`将值传入生成器。用参数接收表达式的值。
	+ `send(val)`让生成器继续，将表达式的值赋给变量(必须要有接受输入的参数)，然后调用`next(obj)`。

输入：
```Python
def counter(start_at = 0):
	count = start_at
	while True:
		val = (yield count)
		if val is not None:
			count = val
		else:
			count += 1

cnt = counter(5)
next(cnt)
next(cnt)
cnt.send(9)
next(cnt)
cnt.close()
```
输出：
```Python
5
6
9
10
```

---