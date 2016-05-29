---
title: cctype与进制数转化
date: 2016-04-21 09:16:59
category: C++基础
tags: [进制转化,cctype]

---

## 进制转化

1. 控制符dec，hex，oct用于设置整数显式的进制数，分别对应十进制，16进制，八进制，语法：`cout<<dec;`,`cout<<hex;`,`cout<<oct`。
2. 头文件`cstdlib`中的`char *_itoa_s(int value,char string,int radix)`可以设置任一进制的输出。
	+ 参数一：要转换的数据；
	+ 参数二：存放结果的字符串地址；
	+ 参数三：进制数；
	+ 返回值：指向结果字符串的指针。

---

## cctype

主要是`cctype`头文件中的字符函数在编程过程中可以带来很多便利，常用的总结如下：
1. `isalpha()`,`isdigit()`,`isalnum()`可以用于判断是字符、数字、字母或数字。返回`true`或`false`。
2. `islower()`,`isupper()`,`isprint()`可以用于判断是小写字母、大写字符、可显示字符。返回`true`或`false`。[可显示字符](https://zh.wikipedia.org/wiki/ASCII#.E5.8F.AF.E6.98.BE.E7.A4.BA.E5.AD.97.E7.AC.A6)从32到126一共95个。
3. `tolower()`,`toupper()`用于大小写字母的转换，如果不需要转换，字符不变。
4. 另外一些不常用的字符函数,`isgraph()`(除空格之外的打印字符),`ispunct()`(标点符号),`isspace()`(标准空白字符),`iscntrl()`(控制字符),`isxdigit()`(16进制,即1-9,a-f,A-F)。

---

