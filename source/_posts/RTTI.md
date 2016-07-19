---
title: RTTI
date: 2016-06-29 11:18:41
category: C++基础
tags: [RTTI,类型转换运算符]

---

## RTTI

RTTI(Runtime Type Identification)，是运行阶段类型识别。
在以下三种情况下需要在运行阶段知道类型(指针指向的对象)：
1. 调用类方法的正确版本(通过虚方法已经解决)；
2. **调用派生类对象独有的方法**(这需要将基类指针转化为派生类指针)；
3. 调试需要跟踪对象类型。

以下三种方法支持RTTI:
1. `dynamic_cast`运算符将使用一个指向基类(expr)的指针来生成指向派生类的指针(target type)(<font color=red>这个指针指向的对象继承层次必须低于要转化的层次</font>)，否则返回`nullptr`；
2. `typeid`返回指出对象类型的值；
3. `typed_info`结构存储特定类型信息；

### dynamic_cast运算符
```C++
dynamic_cast<target-type> (expr)
```
他解决这样一个问题：expr(对象指针或引用)是否可以转化为target-type的指针或引用。如果可以，返回目标指针/引用，否则返回`nullptr`。[cplusplus-Forum](http://www.cplusplus.com/forum/general/33626/)明确指出**被铸造的指针/引用应该是expr类类型或其派生类类型**。

应尽可能使用虚函数，只在必要时使用RTTI。以下是指针和引用的使用例子：
指针用法：
```C++
Object *p = new Object();
DerivedObject *q = nullptr;
if (q = dynamic_cast<DerivedObject *>(p)){
	q->uniqueFunc();
}
```
引用用法：
```C++
try{	//因为没有与空指针对应的引用类型
	Object test();
	Object &p = test;
	DerivedObject &q = dynamic_cast<DerivedObject &>(p)
}
catch (bad_cast &){	//<typeinfo>中定义
	...
}
```

### typeid运算符和type_info类
`typeid`运算符接受类名或结果为对象的表达式，返回一个`type_info`对象的引用,重载`==`,`!=`，可用来判断两个类型是否一致。
```C++
Object test();
Object *p = new Object();
if (typeid(test) == typeid(*p)){
	...
}
```
值得注意的是如果在`if_else`语句中使用了`tpyeid`，则考虑使用`dynamic_cast`。

---

## 类型转换运算符
向上类型转换：派生类向基类转化。
向下类型转换：基类向派生类转化。
根据目的提供四个运算符：
1. dynamic_cast;
2. const_cast;
3. static_cast;
4. reinterpret_cast。

```C++
xxxxx_cast<target-type> (expr)
```

### dynamic_cast
对指向对象的指针来说，他支持向上转化(也就是说他在运行阶段会检查转换是否安全)，如果不安全(不是向上转化)返回`nullptr`，如果不是多态，编译出错。

### const_cast
改变值为`const`或者`vilatile`，例如：
```C++
int a = 5;
const int *b = &a;
//*b = 6;	//invalid because const
int *c = const_cast<int *>(b);
*c = 9;
```
注意他修改一个指向值的指针，但修改`const`值得结果是不确定的。仅当指向的值不是`const`时才可行。

### static_cast
[using static_cast to downcast](http://www.cplusplus.com/forum/general/79249/),他在运行阶段不会检查转化是否合理(假设是合理的)，因此可以向下转化(这一般情况下是不允许的，因为不安全)，因此`static_cast`更快一些，相比`dynamic_cast`来说(需要多态)，更像显式的强制类型转化，因此数值指针转化都用他。
使用心得：
1. 因为不安全，因此一定在100%确认转化是合理的才能使用`static_cast`；
2. 在不确认转换是否合理时，使用`dynamic_cast`。

### reinterpret
[When to use reinterpret_cast](http://www.cplusplus.com/forum/general/47849/)他执行一些危险的类型转化，如“改变编译器解释数据的方式”
1. 他不允许删除`const`；
2. 可以将指针类型转换为足以存储指针表示的整型，但不能将指针转换为更小的整型或浮点型；
2. 转换为一种类型到**完全不同的类型**而不改变位；
3. 不能将函数指针转化为数据指针。

[static_cast和reinterpret_cast的区别](http://baike.baidu.com/item/reinterpret_cast),要谨慎使用`reinterpret_cast`(按字节**重新解释**)