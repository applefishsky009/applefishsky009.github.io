---
title: Python入门11-类的高级特性
date: 2017-01-17 10:07:53
category: Python
tags: Python

---

## 通用特性

1. 类和类型统一，所有Python内建转换函数都是工厂函数。类名以及工厂函数可以创建类的新对象，还可以作为基类去子类化类型，还可以用于`instance()`内建函数。	
2. 注意`instance(obj,OBJ)`内建函数不是严格比较的，如果obj是一个给定类型的实例或其子类的实例，都会返回`TRUE`。
3. 严格比较使用`is`操作符。

---

## __slots__类属性

[Usage of __slots__?](http://stackoverflow.com/questions/472000/usage-of-slots)
1. 字典对实例很重要，`__dict__`属性跟踪所有实例属性。使用`inst.foo`和`inst.__dict__['foo']`访问属性是一样的。
2. 字典会占用大量内存，如果有一个属性数量很少的类，但有很多实例，基于内存上的考虑，可以使用`__slots__`属性来代替`__dict__`属性。
3. `__slots__`属性是一个类变量，由一序列型对象组成，由所有合法标识构成的实例属性的集合来表示，可以是列表，元组，可迭代对象，甚至在实例属性唯一时他可以是字符串。
4. `__slots__`属性是“类型安全”的，不允许用户动态增加实例属性。带`__slots__`属性的类定义不会存在`__dict__`了。
5. `__slots__`属性主要有两个优势，节约内存，访问属性速度更快。

---

## __getattribute__()特殊方法

1. 回忆`__getattr__()`特殊方法，当属性不能在实例或他的类或他的祖先类中的`__dict__`找到时，调用这个方法(在授权中，调用这个方法后调用`getattr()`内建函数来实现授权)。
2. 使用一个函数来执行每一个函数的访问，不光是属性不能找到的情况。如果类同时定义了`__getattribute__()`及`__getattr__()`方法，除非明确从`__getattribute__()`调用`__getattr__()`或者引发`AttributeError`(`__getattr__()`捕获这个异常并执行)，否则后者不会被调用。
3. 如果在`__getattribute__()`中不知何故再次调用了`__getattribute__()`，会引起无穷递归调用。因此总应该先调用祖先类的同名方法。

---

## 描述符

### 简介
描述符是表示对象属性的一个代理。
1. 描述符可以是任何类，至少实现了`__get__()`,`__set__()`,`__del__()`三个方法中的一个，这三个方法充当描述符协议的作用。实现方法不同具有不同的读写权限。
2. 非数据描述符实现`__get__()`方法，是只读数据，也叫做方法描述符。数据描述符具有读写权限，实现了`__set__()`和`__get__()`方法。
3. 如果想为一个属性写个代理，必须把他作为一个类的属性，让这个代理来完成所有的工作。考虑到上一节中每个属性实例都会调用的特殊方法`__getattribute__()`，他是描述符的核心。
4. 如果`super()`被调用了，它会沿着`obj.__class__.__mro__`紧接着的继承树来查找属性。

```Python
def __get__(self, obj, type=None) => value
类X的实例x  x.foo    equals    type(x).__dict__['foo'].__get__(x,type(x))
类X        X.foo    equals    X.__dict__['foo'].__get__(None,X)
类X的子类Y  super(Y,obj).foo   X.__dict__['foo'].__get__(obj,X)

def __set__(self, obj, val) => None
def __delete__(self, obj) => None
```
### 优先级别
1. 类属性 > 数据描述符 > 实例属性 > 非数据描述符 > 默认为`__getattr__()`。
2. 非数据描述符的目的只是当实例属性值不存在时，提供的一个值而已。当没有找到非数据描述符，`__getattribute__()`将会抛出一个`AttributeError`异常，接着会调用`__getattr__()`作为最后一个操作。
3. 优先级高的可以隐藏优先级低的属性。
4. 函数是非数据描述符，实例属性有更高的优先级，可以遮蔽任何一个非数据描述符。只要把(另)一个(非数据描述符)对象赋给实例(属性)就行了。
5. 静态方法、类方法、属性，甚至所有的函数都是描述符，描述符会根据函数的类型确定如何“封装”这个函数和函数被绑定的对象，然后返回调用对象。

---

## 属性和property内建函数

[使用@property](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143186781871161bc8d6497004764b398401a401d4cce000)
[property](http://www.howsoftworks.net/python/function/property.html)
1. 可以使用`property()`方法来处理实例属性的获取(`x.getter`)、赋值(`x.setter`)、删除(`x.deleter`)操作，在操作中可以对属性做一些合法值判断等功能。
2. `property()`方法是他所在类被创建时调用的，实际上是将函数作为参数传递进去，这些方法(其实就是函数)是非绑定的。
 
---

## 元类(metaclass)

[使用元类](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319106919344c4ef8b1e04c48778bb45796e0335839000)
[深刻理解Python中的元类(metaclass)](http://blog.jobbole.com/21351/)
[What is a metaclass in Python?](http://stackoverflow.com/questions/100003/what-is-a-metaclass-in-python)
1. 元类是类的类，即由元类创建类，由类创建实例，关键是元类中的`__new__()`方法和`type()`内建函数(唯一可以创建类的东西)。
2. 元类一般用于创建类，解释器首先会查找类的关键字参数`metaclass`，如果没有传入，他会继续查找父类的关键字参数`metaclass`，直到`object`，`type(object)`即`type`类型，可以看到一个令人震惊的事实，`object`是`type`创建的。
3. 元类用于改变类的默认行为和创建方式。

运行以下代码：
```Python
# -*- coding:utf-8 -*-

from time import ctime

print('1')
print('2')

class MetaA(type):
    
    def __init__(cls,name,bases,attrd):
        super(MetaA,cls).__init__(name,bases,attrd)
        print('4')

print('3')

class C(object,metaclass=MetaA):
    
    def __init__(self):
        print('6')

print('5')
c = C()
print('7')
```

输出为
```Python
1
2
3
4
5
6
7
```

---

