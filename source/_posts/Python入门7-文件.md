---
title: Python入门7-文件
date: 2016-12-24 10:51:18
category: Python
tags: Python

---

首先介绍一个keyword:`with`，Python引入了`with`关键字来自动帮助我们调用`close()`方法。另外`as`关键字可以作为shorthands。
其次介绍一个内建函数`help()`：
1. `help(object)` 显示帮助信息，一般用于交互环境查询帮助信息。
2. 参数`object`是对象。
	+ 内建(工厂)函数直接输入函数名即可，例如`help(help)`;
	+ 对象的方法，首先实例化一个对象(基本数据类型可以用关键字来代替)，然后通过对象和方法名来调用帮助。
	+ 在很多文档中方法或属性不全面或者版本误差，可以这样来查看最全面的帮助文档。例如打开一个文件f，`help(f)`。

---

# Functions

## open()
`open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True)`
返回文件对象，如果是文本文件，返回TextIOWrapper。如果是二进制文件，读时返回BufferedReader，写时返回BufferedWriter。访问模式：`r/w/a/r+/w+/a+` `rb/wb/ab/rb+/wb+/ab+`
参考资料：
[io — Core tools for working with streams](https://docs.python.org/3/library/io.html)
Zeal
[廖雪峰的博客](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431917715991ef1ebc19d15a4afdace1169a464eecc2000)
[Python3 File 方法](http://www.w3cschool.cn/python3/python3-file-methods.html)

## Iteration
打开文件之后，文件迭代(一行一行访问)很简单，使用`for each in f`比`for each in f.readline()`更高效。

---

# Methods

1. file.read([size]) 从文件读取指定的字节数，如果未给定或为负则读取所有。
2. file.readline([size]) 读取整行，包括 "\n" 字符。
3. file.readlines([size int]) 读取所有行并返回列表，若给定size int > 0，返回总和<font color=red>大约</font>为size int字节的行, 实际读取值可能比size int较大, 因为需要填充缓冲区。
	+ 注意read相关方法都不会删除行结束符，这个工作要通过`str.strip()`(默认情况下删除空白字符，`\t\ \n`)来完成。
4. file.write(str) 将字符串写入文件，没有返回值。
5. file.writelines(sequence) 向文件写入一个序列字符串列表，如果需要换行则要自己加入每行的换行符。
6. file.seek(offset, whence) 设置文件当前位置。
7. file.tell() 返回文件当前位置。
8. file.next() 返回文件下一行。
9. file.truncate([size]) 截取文件，截取的字节通过size指定，默认为当前文件位置。
	+ 注意这个和`file.seek()`配合时，截断后返回文件开头。
10. file.flush() 刷新文件内部缓冲，直接把内部缓冲区的数据立刻写入文件, 而不是被动的等待输出缓冲区写入。
11. file.close() 关闭文件。关闭后文件不能再进行读写操作。
12. file.fileno() 返回一个整型的文件描述符(file descriptor FD 整型), 可以用在如os模块的read方法等一些底层操作上。
13. file.isatty() 如果文件连接到一个终端设备返回 True，否则返回 False。

# Attributes

1. file.closed 表示文件已经被关闭，否则为`false`。
2. file.encoding 返回文件所使用的编码。
3. file.mode 文件打开时所使用的模式。
4. file.name 文件名。
5. file.error
6. file.newlines
7. file.buffer

---

# Standard File

## sys

Python中通过sys模块来访问文件句柄。
1. `print()`通常输出到`sys.stdout`。
2. `input()`通常从`sys.stdin`接受输入。
3. `sys.*`是文件，所以必须自己处理好换行符。
sys模块有argv属性，`sys.argv`返回一个列表，列表长度`len(sys.argv)`即参数个数(C中的argc，即argument count)，列表本身就是参数向量(C中的argv，即argument vector)。
1. `sys.argv`是命令行参数列表。
2. `len(sys.argv)`是命令行参数个数。

## os
Python中通过这个模块访问文件系统。

## os.path
完成一些针对路径名的操作。

## pickle/cPickle
保存Python对象到文件中，而不需要转换成字符串。两个主要函数`dump()`，`load()`。

---
