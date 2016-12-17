---
title: VSCode配置Gcc环境
date: 2016-07-30 15:04:17
category: VSCode
tags: VSCode

---

之前编程需要用Vs2015(c++11/14)零散的解读过STL源码，后来受到启发，想要系统的剖析STL源码，深入了解泛型编程，于是以侯捷的《STL源码剖析》为指引，开始我的STL源码剖析之旅。

---

## STL版本

STL目前一共有五个版本：
1. HP STL，所有STL实现版本的始祖；
2. PJ STL, Visual C++ 采用，可读性极低(命名极不讲究)；
3. RW STL, C++Builder 采用，可读性不错，源码中夹杂特殊常量；
4. STLport, 以SGI STL 为蓝本的高度可移植性版本，可以将STLport移植到Visual C++ 和 C++Builder；
5. SGI STL，GCC 采用，可读性很高，建议剖析这个版本。

---

## 在windows下使用SGI STL

首先gdb调试(工程目录和gdb安装目录都是)不支持中文目录，包括空格，需要调试尤其注意这一点。总之，Vscode+gcc比codeblock好用太多了。。。
### CodeBlock
1. [codeblocks](http://www.codeblocks.org/downloads/26),下载最新版mingw版本;
2. [Have g++ follow the C++11 ISO C++ language standard [-std=c++11]](http://www.cnblogs.com/abcdea/archive/2013/09/13/Sublime.html)；
3. 界面不太友好，调试器极度不友好!!!，但是他源码追踪是很正确稳定的。

### VsCode+GCC

1. 先参照这个[Windows下VSCode编译调试c/c++](http://blog.csdn.net/c_duoduo/article/details/51615381),这是[官方指南](https://code.visualstudio.com/docs/languages/cpp)，安装cpptools,SGITools;
	+ 安装参考[下载安装cgywin](http://www.programarts.com/cfree_ch/doc/help/UsingCF/CompilerSupport/Cygwin/Cygwin1.htm)，即从中国镜像地址`http://www.cygwin.cn`下载；
	+ 建议安装`make`包，在编译大项目是需要自己写`makefile`。
2. 安装编译、调试环境时有些问题，一般在项目的`.vscode`中放入三个文件(即上述过程中的配置文件,我的配置在后边给出，参数依然需要follow the C++11 ISO C++ language standard)，参考来源：[Compiling C++11 with g++](http://stackoverflow.com/questions/10363646/compiling-c11-with-g)， [How do I enable C++11 in gcc?](http://stackoverflow.com/questions/16886591/how-do-i-enable-c11-in-gcc);
3. 一定要注意安装gcc时，用VsCode调试项目时，<font color=red>路径中不能有空格</font>，否则gdb调试器不能定位这个路径；
4. 界面很友好，但是，源码追踪比较坑，建议直接在文件夹中自己找源码。

需要注意的是，使用不同的编译器要包含正确的头文件，VS中的一些习惯很可能在GCC中失败，比如`#include <cstdlib>`和`#include <cmath>`中的`abs()`方法是不同的。

launch.json

```json
{ 
	"version": "0.2.0", 
	"configurations": [ 
		{ 
			"name": "C++ Launch (GDB)",
			"type": "cppdbg",
			"request": "launch",
			"targetArchitecture": "x86",
			"program": "${file}.exe",
			"miDebuggerPath":"D:\\Cygwin64\\bin\\gdb.exe",
			"args": ["blackkitty", "1221", "# #"], 
			"stopAtEntry": false,
			"cwd": "${workspaceRoot}",
			"externalConsole": true,
			"preLaunchTask": "g++"
		} 
	] 
}
```

tasks.json

```json
{
	"version": "0.1.0", 
	"command": "g++", 
	"args": ["-g","-std=c++11","${file}","-o","${file}.exe"],
	"problemMatcher": { 
		"owner": "cpp", 
		"fileLocation": ["relative", "${workspaceRoot}"], 
		"pattern": { 
			"regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$", 
			"file": 1, 
			"line": 2, 
			"column": 3, 
			"severity": 4, 
			"message": 5 
		} 
	} 
}
```

c_cpp_properties	-	定位STL源文件位置

```json
{
    "configurations": [
        {
            "name": "Mac",
            "includePath": ["D:/Cygwin64/lib/gcc/x86_64-w64-mingw32/4.9.2/include"],
            "browse" : {
                "limitSymbolsToIncludedHeaders" : true,
                "databaseFilename" : ""
            }
        },
        {
            "name": "Linux",
            "includePath": ["D:/Cygwin64/lib/gcc/x86_64-w64-mingw32/4.9.2/include"],
            "browse" : {
                "limitSymbolsToIncludedHeaders" : true,
                "databaseFilename" : ""
            }
        },
        {
            "name": "Win32",
            "includePath": ["D:/Cygwin64/lib/gcc/x86_64-w64-mingw32/4.9.2/include"],
            "browse" : {
                "limitSymbolsToIncludedHeaders" : true,
                "databaseFilename" : ""
            }
        }
    ],
    "clang_format" : {
        "style" : "file",
        "fallback-style" : "LLVM",
        "sort-includes" : false
    }
}
```

在上述文件修改你的gcc include目录，各项的含义第一条中的超链接有。

