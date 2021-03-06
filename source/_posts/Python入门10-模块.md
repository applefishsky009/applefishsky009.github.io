---
title: Python入门10-模块
date: 2017-01-09 19:38:23
category: Python
tags: Python

---

## 模块基础

1. 自我包含并且有组织的代码片段就是模块。把其他模块中属性附件到你的模块中的操作叫做导入。
2. 模块按照逻辑来组织Python代码方法，文件是物理层组织模块的方法。
	+ 一个文件被看作一个独立模块，一个模块也被看作是一个文件。
	+ 模块的文件名就是模块的名字加上扩展名.py。
3. 一个名称空间就是一个从名称到对象的关系集合映射。所以每个模块都定义了自己的唯一的名称空间。
	+ 即使属性之间有名称冲突，但他们的完整授权名称——据点属性标识指定了各自的名称空间(防止名称冲突)。
4. 默认搜索路径是在编译时或者安装时指定的。他可以在一个或者两个地方修改。路径模块`import sys`。
	+ `sys.path`包含每个独立路径的列表，那么显然`sys.path.append()`可以添加搜索的路径，`sys.path.insert(pos, val)`可以指定插入位置。
	+ `sys.modules`可以找到当前导入了哪些模块以及他们来自哪里，返回值是一个字典，模块名是`key`，对应物理地址是`val`。
5. 名称空间是名称到对象的映射，改变一个名字叫做重新绑定，删除一个名字叫做解除绑定。
	+ 局部名称空间，全局名称空间，内建名称空间(`__buildins__`模块中的名字)，从前往后搜索名称，从后往前加载。
	+ `globals()`和`locals()`内建函数得到全局/当前符号表字典。可以用于判断某一名字属于哪个名称空间。
6. `__name__`系统变量可以在运行时检测该模块是被导入还是被直接执行。
	+ 如果模块是被导入，`__name__`的值为模块名字。
	+ 如果模块是被直接执行，`__name__`的值是`__main__`。
	+ 利用这个特性在主程序中书写测试代码。
7. 明白名称查询的规则，就很容易理解遮蔽效应。名称查询首先从局部名称空间开始，然后查找全局名称空间，然后在内建名称空间里查找。
8. 可以在任何需要防止数据的地方获得一个名称空间，比如可以在任何时候给函数添加属性。 

---

## 模块导入

1. 建议按顺序导入模块，Python标准库模块->Python第三方模块库->应用程序自定义模块。
2. 可以在模块中导入指定属性。即吧指定名称导入当前作用域。
3. 多行导入，使用逗号分隔属性，使用`\`来自动换行，建议使用Python标准分组机制(圆括号)来创建更合理的多行导入语句。
4. 在`import`语句之后使用`as`在导入的同时指定局部绑定名称。
5. 一个模块只能被加载一次，加载只在第一次导入时发生。
	+ 限制使用`from module import *`，因为会污染名称空间。
	+ 在交互解释器或者目标模块属性非常多时可以全部导入。
6. 属性名称可能冲突时，建议使用`import`和完整的标识符名称(句点属性标识)。
7. `__future__`导入新的特性。
8. 警告框架，警告用户不要使用一个即将改变或不支持的操作，包含几个部分：
	+ API：调用API发布警告，使用warnings模块。
	+ 一些警告异常类的集合：Warning，UserWarning，DeprecationWarning,SyntaxWarning和RunningWarning等。
	+ 警告过滤器：不仅仅收集关于警告的信息(如信号，警告原因等)，控制是否忽略警告，是否显示警告(可以自定义格式)，或者转化为错误生成一个异常等。
9. 从zip文件中导入模块。
10. 编写可调用的`import`类来重新实现整个导入机制，需要实现查找器和载入器。
	+ `import`语句调用`__import()__`函数完成工作，提供这个函数是为了让有特殊需求的用户覆盖他，实现自定义的导入算法。

---

## 包和其他特性

### 包
1. 包是一个有层次的文件目录结构，他定义了一个由模块和子包组成的Python应用程序执行环境。
2. 可以通过`最顶层的包.子包.模块`来导入；也可以通过`from 最顶层的包 import 子包`然后使用 属性/点 操作符向下引用子包树；也可以`from 最顶层的包.子包 import 模块`来导入模块；甚至可以沿着子包的树状结构导入子包中模块的名称空间`from 最顶层的包.子包.子包中模块 import something`。
3. 绝对导入是指导入的包通过Python路径(`sys.path`或者`PYTHONPATH`)，`.`表示导入同目录下的模块，`..`表示导入不同目录下的模块(他们都包含在同一个最顶层的包中)。
	+ [使用相对路径名导入包中子模块](http://python3-cookbook.readthedocs.io/zh_CN/latest/c10/p03_import_submodules_by_relative_names.html)

### 其他特性
1. 自动载入的模块：内建模块，在Python2.x中命名是`__builtin__`，在Python3.x中命名是`builtins`。而不论是2还是3中，`__builtins__`都是对内建模块的引用。不同的是，在主模块`__main()__`中，其是对内建模块本身的引用；而在非`__main()__`中，其仅仅是对`__buildin__/builtins.__dict__`的引用，他的类型是字典。
	+ [Python中的内建模块](http://www.52ij.com/jishu/665.html)
2. 阻止属性导入：如果不想让某个模块被`from module import *`导入，可以在模块中给不想导入的属性名称加下划线(`_`，例如`bar`->`_bar`)，但是加上下划线之后怎么导入模块？可以导入整个模块(`import foo`)或者显式导入这个属性(`import foo._bar`)。
3. 不区分大小写的导入，必须指定一个`PYTHONCASEOK`的变量。
4. 源代码编译，[关于Python脚本开头两行的：#!/usr/bin/python和# -*- coding: utf-8 -*-的作用 – 指定文件编码类型](http://www.crifan.com/python_head_meaning_for_usr_bin_python_coding_utf-8/)
5. 导入循环，循环引用的问题。删除导入语句；或者将导入语句移到最后；或者将导入语句放在函数内部，只在调用函数的时候导入这个模块。
6. 模块执行：通过命令行，shell，execfile()，模块导入，解释器的`-m`选项等。

---