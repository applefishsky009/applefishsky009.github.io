---
title: Python入门6-条件和循环
date: 2016-12-21 14:04:04
category: Python
tags: Python

---

# Condition

1. `if`子句
	+ 单一语句的代码块可以放在`if`同一行上，但建议使用合理的缩进。
2. `else`子句
	+ Python使用缩进在避免<font color=red>悬挂`else`</font>时非常有用，因为C/C++中else与最近的if搭配，这在他本想与内部if搭配时发生错误。不要忘记花括号可以避免这个问题。
3. `elif`子句，即`else if`语句
4. Conditional expression. Python很长一段时间内都没有条件表达式`C ? X : Y`，P2.5加入并确定语法为
	+ `X if C else Y`.

---

# Loop

## Conditional Loop(while)
`while`是条件循环语句，常用在计数循环(配合自增或自减语句)、无限循环中。

## Iterative Loop(for)
`for`是Python中最强大的循环结构，他是迭代循环语句，可用于列表解析和生成器表达式中，自动调用`next(iter)`工厂函数(Python2是迭代器方法)，最终捕获StopIterator异常并结束循环。
1. 可用于序列类型，包括字符串，列表以及元组等。
	+ 通过序列项迭代。
	+ 配合`range(len())`通过序列索引迭代，但显然效率不如序列迭代高，这是由迭代器的特性决定的。
	+ 使用索引和项迭代，这就是`enumerate()`的威力。
2. 用于迭代器类型，因为可以调用`next(iter)`函数，调用后返回下一条目所有条目迭代完毕后迭代器引发一个StopIteration异常告诉程序循环结束，这都是`for`语句内部发生的。
3. 内建函数`range()`可以将原本类似于`foreach`的语句变成更熟悉的语句。
	+ `range(start, end, step = 1)` step要求start。
	+ `range(end)`
	+ `range(start, end)`
	+ 注意不能将`range()`简略成一种语法`range(start = 0, end, step = 1)`，这是因为这个语法的两个参数调用版本有歧义。
4. 相关内建函数，`sorted()`(返回list),`zip()`(返回tuple指针)是序列相关，`enumerate()`,`reversed()`返回迭代器。

## break
用于`while`和`for`循环中打断列表的迭代。

## continue
终止当前循环并验证条件表达式(`while`)或验证是否有元素可以迭代(`for`)，验证成功的情况下开始下一次循环。

## pass
提供`pass`语句表示不做任何事，在Python中如果在需要有语句块的地方不写任何语句，会提示语法错误。`pass`也可以作为调试中的小技巧，标记你后来要完成的代码。

## else
Python中可以在`while`和`for`循环中使用`else`语句做循环后处理(post-processing)，相当于设置了一个完整执行循环的flag，即`break`语句也会跳过`else`块，因为循环并没有被完整执行。

---

# Iterator

迭代器从根本上来说就是一个有`next()`方法的对象，因此他能无缝支持序列对象。(next()方法迭代和StopIteration异常告诉外部调用者迭代结束)，内建函数`reversed()`,`enumerate()`,`any()`,`all()`都返回迭代器。
1. 序列 内建函数iter()和next()；
2. 字典 for迭代，方法缺省时迭代键；
3. 文件 for，行迭代。

不要在迭代不可变对象时改变值，比如字典的key，但是使用`dict.key()`方法是可以的，因为这个方法返回独立于字典的列表。一个实现了`__iter__()`和`__next__()`方法的类可以作为迭代器使用。

```Python
#for的工作原理
fetch = iter(seq)
while True:
	try:
		i = next(fetch)
	except StopIteration:
		break
	do_something_to(i)
```

---

# List Comprehensions

## lambda
定义匿名函数，这个语句作为function传入其它函数。
```Python
lambda x: doSomethingAboutX
```

## List Comprehensions
Python独有的语法，比lambda更具优势，两个表达式及例子：
```Python
[expr for iter_var in iterable]
[expr for iter_var in iterable if condexpr]

[(x + 1, y + 1) for x in range(3) for y in range(5)]
len([word for line in f for word in line.split()])
sum([len(word) for line in f for word in line.split()])
```

那么列表如何解析？
1. 对嵌套循环，for语句的顺序是与原顺序相同的；
2. 条件表达式的作用域是前一个语句(for或者其他表达式)；
3. 循环和条件之前的表达式是列表元素(核心部分)，如果还原回来这个语句应该在循环和条件的最内层。

---

# Generator Expression

列表解析的一个缺点就是要生成所有数据用于创建整个列表。生成器表达式是列表解析的一个扩展。它使用了对内存使用更友好的结构。几他不创建真正的列表，而是返回一个生成器，这个生成器每次计算出一个条目之后，把这个条目“产生(`yield`)出来”，使用“延迟计算”(lazy evaluation)。
```Python
(expr for iter_var in iterable if cond_expr)

((i, j) for i in range(3) for j in range(5))
return max([len(x.strip()) for x in open('ect/motd')])	#List Comprehensions
return max(len(x.strip()) for x in open('/ect/motd'))	#Generator Expression
```

---