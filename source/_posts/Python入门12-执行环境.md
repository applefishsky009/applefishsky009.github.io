---
title: Python入门12-执行环境
date: 2017-01-17 18:10:32
category: Python
tags: Python

---

## 可调用对象
函数，方法，类，类的实例。

### 函数
1. 内建函数(BIF:Built-in Function)，C/C++所写，可以用`dir()`列出所有内建函数的属性。
	+ `type(BIF) -> type 'builtin_funciotn_or_method'`，但不用于工厂函数，工厂函数会返回类型`type`。
2. 用户自定义函数(UDF:User-Defind Function)，Python所写，用户自定义函数是函数类型的。
	+ `type(UDF) -> <type 'function'>`。
3. `lambda`表达式，创建匿名函数，通过别名来引用，享有和UDF相同的属性。

### 方法
1. 内建方法(BIM:built-in Method)，只有内建类型(BIT:built-in type)有内建方法，通过内建对象来访问内建方法。
	+ BIF和BIM有相同的属性，不同的是BIM的`__self__`属性指向一个Python对象，而BIF指向`None`。
2. 用户定义的方法(UDM:User-Defined Method)，与类对象关联/非绑定，通过类的实例来调用(绑定的)。
	+ 访问对象本身会揭示你正在引用一个绑定方法还是非绑定方法。

### 类
1. 利用类的可调用性来创建实例。

### 类的实例
1. 类有一个特别方法`__call__()`，该方法允许程序员创建可调用的实例。默认情况下，这个方法是没有被实现的，即大部分实例是不可调用的。

---

## 代码对象
函数对象仅是代码对象的包装，方法则是给函数对象的包装。
1. `callable(object)`判断一个对象是否可调用。
2. `compile(source,filename,mode,flag=0,dont_inherit=False,optimize=-1)`编译source为code或AST对象 code可以通过调用`exec()`和`eval()`执行。
	+ source可以为字符串或AST对象，第二个参数是代码所有文件，通常设置为`None`。
3. `eval(expression,globals=None,locals=None)`执行一个表达式，可以是字符串或内建函数`compile()`创建的预编译代码对象。
4. `exec(obj)`执行代码对象或字符串形式的Python代码。
	+ 注意使用这个执行文件操作时，依然要`tell()`，`seek()`，`close()`，`getsize()`。
5. `input(prompt)`输出提示符，读取用户输入。把输入作为Python对象来求值并返回表达式的结果。
6. 模块导入：`__name__`系统变量可以在运行时检测该模块是被导入还是被直接执行。
	+ 如果模块是被导入，`__name__`的值为模块名字。
	+ 如果模块是被直接执行，`__name__`的值是`__main__`。
	+ 利用这个特性在主程序中书写测试代码。
7. 使用命令行从工作目录直接调用脚本。(如果是标准库的一部分会比较复杂)
8. `os.system()`通常不会和产生输出的命令一起使用，他通过退出状态显示成功或失败而不是通过输入和/或输出通信。通常的约定是利用退出状态，0表示成功，非0表示其他类型的错误。
9. `os.popen()`建立一个指向程序的单向链接，然后像访问文件一样访问程序。
10. `os.fork(), os.exec*(), os.wait*()`，进程控制函数，各种`os.exec*()`函数接受加载到新进程的参数列表。
	+ 子进程返回的pid是0，父进程返回的pid是子进程的进程号。
11. `os.spawn*()`在新进程中执行命令。
12. `subprocess`模块，允许生成新进程，连接到输入/输出/错误管道，并获取返回码。此模块旨在替换多个旧模块和功能。

---

## 终止执行的方法
1. `sys.exit()` 调用时，引发`systemExit`异常，这是唯一不看做错误的异常，如果没有给出状态参数，默认为0。
2. `os._exit()` 不执行任何清理便立即退出Python，不应该在一般应用中使用，因为他是平台相关的。
3. `os.kill(pid,sig)` 发给pid你想要发送的信号sig。 

---
