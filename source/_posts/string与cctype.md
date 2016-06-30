---
title: string与cctype
date: 2016-04-21 10:36:21
category: C++的类
tags: [string,cctype]

---

## string类

1. [Add Binary](https://github.com/applefishsky009/LeetCode/blob/master/67%20-%20Add%20Binary/67%20-%20Add%20Binary.cpp)
	+ 不同长度链表或字符串运算，注意`?`,`:`用得非常巧妙；
2. [Evaluate Reverse Polish Notation](https://github.com/applefishsky009/LeetCode/blob/master/150%20-%20Evaluate%20Reverse%20Polish%20Notation/150%20-%20Evaluate%20Reverse%20Polish%20Notation.cpp)
	+ `string::sizetype`是无符号，`string::npos`是-1的强制类型转换，不同类型值不同；
	+ `stringtoint:string::stoi()`。
3. [Longest Substring Without Repeating Characters](https://github.com/applefishsky009/LeetCode/blob/master/3%20-%20Longest%20Substring%20Without%20Repeating%20Characters/3%20-%20Longest%20Substring%20Without%20Repeating%20Characters.cpp)
	+ 从上一个重复字符的下一个字符计数。
4. [Regular Expression Matching](https://github.com/applefishsky009/LeetCode/blob/master/10%20-%20Regular%20Expression%20Matching/10%20-%20Regular%20Expression%20Matching.cpp)
	+ `*`匹配0个或者多个前个字符 - 重点；
	+ 注意当前匹配条件`*p == *s || (*p == '.' && *s != '\0')`，'.'唯一不能匹配'\0'。

### string运算符
`string`类的运算符重载在头文件`string`里,如`+`,`<<`。注意`"a"+"b" = "ab"`,`'a'+'b' = 195`,前者是字符串拼接，后者是字符常量相加。但是`cout<<"a"+"b";`这个语句是错误的，必须至少声明两个`string`类型。

### string.find()
`string`类的`find()`方法，可以用于找子串，返回子串在原串出现的下标。[这里](http://www.cnblogs.com/web100/archive/2012/12/02/cpp-string-find-npos.html)和[这里](http://www.cplusplus.com/reference/string/string/find/)有详细解释，使用时注意以下三点：
+ 接受三个参数，第一个是子串，第二个是开始寻找的下标，第三个参数是匹配字串的字符数。可以用于找全部的匹配子串；
+ 如果没有找到，返回值是`string::npos`，他是一个很大的正数(-1强制转化为无符号整型)；
+ 返回值是`size_type`，一般用`auto`来代替，这是一个无符号数据，因为对于不同的数据类型,`string::npos`是不同；如果不是`string::npos`，返回值和`int`是没有区别的。

以下代码可以输出全部的匹配位置：
```C++
string s1 = "abcdbcgbcdbjjkklbcdbcdbcdghjbcd";
string s2 = "bcd";
auto k  = s1.find(s2);
while (k != string::npos)
{
	cout<<k<<endl;
	k = s1.find(s2,k+1);	
}
```

### string.substr()
函数声明如下：`_Myt substr(size_type _Off = 0, size_type _Count = npos) const`，返回从指定位置(_Off)开始的长度为(_Count)的子字符串，注意缺省值取尽可能多的字符。


### string::stoi()
字符串转整型stringtoint:
```C++
int stoi (const string&  str, size_t* idx = 0, int base = 10);
int stoi (const wstring& str, size_t* idx = 0, int base = 10);
```
对比而言`atoi()`功能类似，但其是C-stringtoint。
```C++
int atoi (const char * str);
```

---

## cctype

主要是`cctype`头文件中的字符函数在编程过程中可以带来很多便利，常用的总结如下：
1. `isalpha()`,`isdigit()`,`isalnum()`可以用于判断是字符、数字、字母或数字。返回`true`或`false`。
2. `islower()`,`isupper()`,`isprint()`可以用于判断是小写字母、大写字符、可显示字符。返回`true`或`false`。[可显示字符](https://zh.wikipedia.org/wiki/ASCII#.E5.8F.AF.E6.98.BE.E7.A4.BA.E5.AD.97.E7.AC.A6)从32到126一共95个。
3. `tolower()`,`toupper()`用于大小写字母的转换，如果不需要转换，字符不变。
4. 另外一些不常用的字符函数,`isgraph()`(除空格之外的打印字符),`ispunct()`(标点符号),`isspace()`(标准空白字符),`iscntrl()`(控制字符),`isxdigit()`(16进制,即1-9,a-f,A-F)。

---