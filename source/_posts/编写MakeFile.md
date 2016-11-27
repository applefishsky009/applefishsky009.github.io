---
title: 编写MakeFile
date: 2016-11-26 15:24:15
category: VSCode
tags: [MakeFile,VSCode]

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