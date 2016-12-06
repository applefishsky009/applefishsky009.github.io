---
title: 编写MakeFile
date: 2016-11-26 15:24:15
category: VSCode
tags: [MakeFile,VSCode]

---

## 代码并非越灵活越好

[把代码写得太灵活不好吗？](http://www.zhihu.com/question/52951851),总结起来大概有以下几个方面的考量：
1. 更高的弹性可能增加复杂性，开发/维护成本，代码体积，性能开销等。
2. 使用复杂性的增加，比如泛型`ListNode`或`TreeNode`时需要使用`<>`携带类型，但平常使用的接口是用得不到的。
3. 好的代码应该体现在架构设计上，具体到代码片段，反而是简单直白的好。
4. KISS - Keep IT Simple,Stupid.
5. 没有需求不要加功能。

---

## 为什么需要MakeFile?

在之前的VSCode介绍中已经配置过使用`gdb`编译简单短小的c++程序，但是在实际的项目中会有非常多的.h和.cpp文件相互配合，这时候直接通过g++编译可执行文件就会比较复杂，而`make`这个强大的项目构建工具就能完美的解决这个问题，帮助我们构建和组织项目代码。

参考资料：[在Linux中使用VS Code编译调试C++项目](http://www.cnblogs.com/zhxilin/p/5881080.html)

可以根据参考资料，将所有的g++指令写在`MakeFile`中，这样适用于大项目或者小项目。

---

## 如何编写MakeFile?

第一节中的参考资料做出了简单的解释，基本语法：
```make
target : prerequisites
	command	#前面必须是tab
```
`target`可以是：
1. 目标文件(Object File) :
	+ [What does an object file contain?](http://stackoverflow.com/questions/3045603/what-does-an-object-file-contain)
	+ 即编译，根据需要的.cpp生成.o文件；
2. 可执行文件:
	+ 即链接，链接所有的.o文件生成.exe文件；
3. 标签：
	+ 动作名字。在`make`后指出整个动作的名字即可执行该命令。

举例：
```make
build : main.o init.o
	g++ -std=c++0x -o build main.o init.o	
main.o :
	g++ -std=c++0x -g -c main.cpp 
init.o : init.cpp init.h
	g++ -std=c++0x -g -c init.cpp	
clean :
	rm main.o init.o build
```