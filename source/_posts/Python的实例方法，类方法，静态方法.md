---
title: Python的实例方法，类方法，静态方法
date: 2017-01-11 15:14:06
category: Python
tags: Python

---

## 静态方法和类方法

参考资料：
1. [python类的静态方法和类方法区别](http://www.jianshu.com/p/212b6fdb2c50)强调使用场景和继承。
2. [Python @classmethod and @staticmethod for beginner?](http://stackoverflow.com/questions/12179271/python-classmethod-and-staticmethod-for-beginner)最高赞答案强调了重载，次高赞答案强调了继承。
3. [Python 中的 classmethod 和 staticmethod 有什么具体用途](https://www.zhihu.com/question/20021164)最高赞答案强调了封装和使用场景。来自于[Difference between @staticmethod and @classmethod in Python](https://link.zhihu.com/?target=http%3A//www.pythoncentral.io/difference-between-staticmethod-and-classmethod-in-python/)

### 封装
1. 实例方法提供与类的实例对象进行交互的方法(针对实例特有的数据)，因此第一个参数必须传入实例对象，`一般习惯用self`。
2. 类方法提供与类本身(类也是对象)交互的方法()，我们可以在类外定义一个简单方法将类作为参数传入来交互。这个参数如果传入实例对象，需要通过实例对象的`__class__`属性来访问类，如果是类对象，可以直接访问。但是这样就会将类代码关系扩散到类定义的外部，违背了OOP封装的特性。因此采用`@classmethod`装饰器来创建类方法与类交互，注意要与类交互，因此类方法的第一个参数必须传入类对象。
3. 有一些与类有关系的功能但不需要类对象或者实例对象，这就需要静态对象，比如更改环境变量(控制一部分类功能)或者修改其他类的属性。用外部函数依然可以解决，但同样会扩散类内部代码，造成维护困难。因此采用`@staticmethod`装饰器，因为这些功能不需要类对象与实例对象，因此默认不需要传入任何参数。

代码参见参考资料3，但代码对齐有一些问题。

### 重载
1. 考虑C++中类构造函数的多样化可以通过函数重载(`overloading`)来实现，但Python中没有这个概念，如果只能有一个构造函数，对于不同的初始化参数，只能在类外做一些重复性的工作后传入唯一的构造函数来初始化。因此类方法`@classmethod`,通过给不同的类方法传入不同的初始化参数来实现多样化的初始化。
	+ 可重复利用(reusable)，针对构造函数外部重复性的处理。
	+ 封装(Encapsulation)，将类代码关系封装在类内。
	+ 继承(inherit)，类方法与类关联，因此继承后依然存在于子类，但其属于子类。
2. 静态方法与类方法非常相似，但其不需要强制参数，因此可以做一些与类控制相关的环境变量或其他类属性的判断或更改。

代码见参考资料2,3。

### 继承
如果子类继承父类方法，子类覆盖了父类的静态方法；
1. 子类实例继承父类的静态方法，调用该方法，还是调用父类的方法和类属性。其实可以认为是提供了一个子类访问父类属性的接口(就算子类重载了任何函数，这个静态函数的搜索也是从父类开始向上的，即屏蔽了子类的名称空间)。
2. 子类实例继承父类的类方法，调用该方法，调用子类的方法和类属性。可以通过继承来覆盖类方法。

代码参见资料1,2。

### 使用场景

1. 实例方法，只能被实例对象调用，第一个参数必须要传实例对象，一般习惯用`self`；
2. 类方法(在类中由`@classmethod`装饰)，可以被实例对象和类调用，第一个参数必须要传类，一般习惯用`cls`；
3. 静态方法(在类中由`@staticmethod`装饰)，可以被实例对象和类调用，参数没有要求。

```Python
class foo(object):
    
    def print_my1(self):    
        print('实例方法{}'.format(self.__class__))
        
    @classmethod
    def print_my2(cls): #所属的类名
        print('类方法{}'.format(cls.__name__))
        
    @staticmethod
    def print_my3():
        print('静态方法')

x = foo()
#实例初始化只有两个属性__dict__(包含可用的属性名-属性字典)和__class__(指出该实例属于哪一类)
#foo.print_my1() 类不能调用实例方法
foo.print_my2()
foo.print_my3()
x.print_my1()
x.print_my2()
x.print_my3()
```
输出：
```Python
#error
类方法foo
静态方法
实例方法<class '__main__.foo'>
类方法foo
静态方法
```

---
