---
title: 有趣的switch
date: 2016-07-26 09:48:41
category: C++基础
tags: switch

---

遇到一个有趣的题目，以下代码会输出什么？
```C++
int n = 'c';
switch (n++) {
default:cout << "error" << endl;
case 'a':case 'A':case 'B':cout << "good" << endl;
case 'c':case 'C':cout << "pass" << endl;
case 'd':case 'D':cout << "warn" << endl;break;
case 'k':cout << "hello" << endl;
}
```
答案是
```C++
pass
warn
```
下面说明switch是如何工作以帮助使用，并合理猜测其底层原理。

---

## switch

将`switch`简单视为`if...else...`的替代品是错误的，他忽略了`break`语句在`switch`中的作用。简单来说，编译时`switch`的`case`会有一个比较表(这个表的顺序不重要)，重要的是每一个比较值都有一个跳转地址(所以这个表也叫跳转表，是一个`case`值与地址的映射)，这个跳转地址是按语句顺序来的，比如在上边的程序中：
```C++
	"aAB"	 		跳转到 cout << "good" << endl;地址并向下执行；
	"cC"  			跳转到 cout << "pass" << endl;地址并向下执行；
	"dD"  			跳转到 cout << "warn" << endl;地址并向下执行；
	"k"				跳转到 cout << "hello" << endl;地址并向下执行；
default，即不在表中	跳转到 cout << "error" << endl;地址并向下执行；
遇到break;			跳转到 switch{}代码块的下一句并向下执行，上述代码中是n++；
```
因此尤为注意的是：
1. 每一个`switch`只在跳转表中查找一次(可能有n次比较)，然后提供一个程序入口，之后与`switch...case...`无关；
2. 有n个`case`语句最坏情况下需要n次比较；
3. 上述代码中的`n++`实际上执行在`switch(){}`代码块之后。