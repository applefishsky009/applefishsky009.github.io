---
title: 编程技巧说明
date: 2016-04-08 19:44:02
category: C++基础
tags: C++

---

## 判断两个值a,b是否相等

1. 若为整型，应该为`a==b`；
2. 若为浮点型，应该用`fabs(a-b)<1e-9`。(因为[计算机中浮点型是不准确的](http://0.30000000000000004.com/)，**与因数有关**)
3. 另外，对于`bool`型，应该用`if(a)`，`if(!b)`这样的形式来强调变量`bool`类型。

---

## 判断一个整数是否为奇数

1. `x%2 != 0`用来判断一个整数是否是奇数，**不能用`x%2 = 1`**，因为x可能是负数，余数就是-1。
2. `-3 = 2*-1+(-1)`，即`-3/2 = -1`;`-3%2= -1`;说明负奇数余数是-1。
3. if (num & 0x1)，判断奇数 WTF!!!

---

## char值做数组下标的强制类型转化

1. 应该强制转化为`uchar`，作为数组下标。不能直接转化为`uint`。
2. 因为高位扩充有两种，有符号数扩充，在高位补符号位；无符号数高位直接用0。例如`char c = -1`。c在计算机中的补码是`11111111`。`uint a = c`，那么`a = 4294967295`。因为c有符号，扩充后高位补1。`uint b = (uchar) c`。那么`b = 255`。因为`(uchar) c`是无符号数，高位用0扩充。
3. 二进制中有符号向无符号数的强制转化非常简单，将**符号位置为0**。比如-2，原码是`10000010`，反码是`11111101`，补码是`11111110`。将符号位置0，得到`01111110`，为254。即`char -2`强制转化成`uchar`值为254。
4. 参照维基百科和C++plusP44**圆图**可以更清楚了解更多数据储存与二进制运算。

---

## vector和string优先于动态数组的分配

性能上，`vector`保证内存（分配在堆）连续，一旦分配后，性能和原始数组相当；
用`new`必须`delete`，不然会`bug`，代码行数不够短；
多维数组定义方法：
1. `new/delete`:
```
int** array = new int*[row];
for(int i=0;i<row;i++)
	array[i] = new int [col];
```
2. `vector`:	
```
	vector<vector<int>> = array(row,vector<int>(col,0));
```

---

## 使用reverse来避免[不必要的重新分配](http://blog.csdn.net/bichenggui/article/details/4690175)

1. `vector`需要更多空间，以类似`realloc`的思想来增长大小。分配，回收，拷贝和析构，这些步骤都很昂贵。并且每次这些步骤发生时，所有指向`vector`或`string`中的迭代器、指针和引用都会失效。
2. 据博客中所说，vector重新分配时容量翻倍。我在VS2012，WIN32编译器下结果如下，容量是翻*1.5*倍的。因此在1000次`push_back`中导致了18次重新分配。![reverse()](http://i.imgur.com/ooqb6by.png)
3. 在**容器被构造之后**进行**`reserve`设置容器容量**可以避免不必要的重新分配。`a.reserve(1000)`即把a的容量设置为1000。

---

## 诊断宏

[assert.h](https://zh.wikipedia.org/wiki/Assert.h),`assert()`是一个诊断宏，用于动态辨识程序的逻辑错误条件，原型：
```C++
void assert(int expression);
```
1. 如果是非零值，不做任何操作；
2. 如果是零值，用宽字符打印诊断消息，然后调用`abort()`，诊断消息：
	+ 源文件名字；
	+ 源文件行号；
	+ 所在函数名；
	+ 求值结果为0的表达式。

---

## const在C++与ANSI C中的不同
[为什么const不能用于数组初始化](http://baike.baidu.com/subview/1065598/5048428.htm?fromTaglist=)，简单来说：
1. ANSI C中`const`定义了只读变量，C++中`const`是常量，而数组初始化要求参数只能是常量；
2. ANSI C中定义常量只能用枚举或宏。
3. 另外这样使用数组如果越界访问并存储，会按特定方式修改栈，造成安全隐患。
```C++
int main()
{
	const int n = 4;
	int a[n];
	a[0] = 1;a[1] = 2;a[2] = 3;a[3] = 4;a[4] = 5;
	for (int i = 0;i < n;++i)
		cout << a[i] << endl;
	cout << a[4] << endl;
	system("pause");
}
```
这会破坏栈中的环境变量表或其他设置(开始执行main()时压入栈的东西)，提示：
> Run-Time Check Failure #2 - Stack around the variable 'a' was corrupted.

如果使用vector,在越界访问时就会抛出一个错误，提示：
> vector subscript out of range.