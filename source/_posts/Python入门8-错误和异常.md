---
title: Python入门8-错误和异常
date: 2017-01-04 14:24:53
category: Python
tags: Python

---

错误是语法和逻辑上的，当Python检测到一个错误时，解释器就会指出当前流已经无法继续执行下去了，这时候就出现了异常。

---

## Python中的异常

[Exception hierarchy](https://docs.python.org/3/library/exceptions.html)，所有的标准/内建异常都是从根异常派生的。目前的Python3有4个直接从BaseException派生的异常子类：
1. SystemExit
2. KeyboardInterrupt
3. GeneratorExit
4. Exception

其他所有的内建异常都是Exception的子类。

---

## 检测和处理异常

### try...except...
1. 使用<font color=red>`as`</font>关键字将错误捕获为异常参数。
2. 为用户可能遇到的返回错误写文档。比如显式的返回None或者负数(如果可以表征无效值)，来规范代码。
3. 带有多个`except`的`try`语句，匹配不同的异常来执行对应的动作。
4. 处理多个异常的`except`语句，可以在一个`except`语句中处理多个异常，但使用`as`捕获异常参数时，被赋值的是所触发的特定异常。
5. 不指定任何异常，可以捕获所有异常。
6. 不要捕获并忽略(`pass`)所有错误，可以捕获并忽略特定错误。
7. 异常参数是导致异常的代码的诊断信息的类实例。

### try...finally...
1. 这个语句不用来捕捉异常，无论`try`是否有异常被触发，`finally`代码段都会被执行。
2. 当`try`范围中产生一个异常时，会立即跳转到`finally`语句段。当`finally`中的所有代码都执行完毕后，会继续向上一层引发异常。
3. 如果`finally`中的代码引发了另一个由于`return`,`break`,`continue`语法而终止，原来的异常将丢失且无法重新引发。

### try...except...finally...
1. 无论异常是否发生，是否捕捉都会执行的一段代码。

### try...except...else...
1. 在`try`范围内没有异常被检测到时，执行`else`子句。

### try...except...else...finally...
1. 无论选择什么语法，至少需要一个`except`，`else`和`finally`都是可选的。

所有的异常处理语法：
```Python
try:
	doSomething
except Exception1:
	doSomethingForE1
except (Exception2,Exception3,Exception4):
	doSomethingForE2||E3||E4
except Exception5 as e5:
	doSomethingForE5
except (Exception6,Exception7,Exception8) as e6||e7||e8
	doSomethingForE6||E7||E8
	note that e6||e7||e8 is only the one Exception that cause error.
except:
	catch all the other exceptions
else:

```

---

## 上下文管理

`with`语句的目的在于从流程图中把`try`,`except`,`finally`关键字和资源分配释放相关代码统统去掉。
支持上下文管理协议的成员：
1. file
2. decimal.Context
3. thread.LockType
4. threading.Lock
5. threading.RLock
6. threading.Condition
7. threading.Semaphore
8. threading.BoundedSemaphore

file是最常见且最易于演示的(通常见到`with`用于打开文件)。

---

## 触发异常

异常除了由解释器引发，还可以由程序员明确触发：`raise`语句。
`raise SomeException args traceback`
1. `SomeException`:触发异常的名字。
2. `args`:异常参数，可以是一个单独对象，或者是一个元组。
3. `traceback`:跟踪记录对象，用于重新引发异常。

---

## 断言
`assert`和C++中用法基本相同，只不过不需要括号需要空格分隔关键字和表达式。

---

## sys模块和异常
`sys`模块中的`exc._info()`函数返回一个三元组来提供关于异常更多的消息。
1. `exc_type`:异常类；
2. `exc_value`:异常类的实例；
3. `exc_traceback`:跟踪记录对象。

--- 