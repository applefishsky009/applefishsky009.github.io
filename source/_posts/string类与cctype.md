---
title: string类与cctype
date: 2016-04-21 10:36:21
category: STL
tags: C++

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
5. [Multiply Strings](https://github.com/applefishsky009/LeetCode/blob/master/43%20-%20Multiply%20Strings/43%20-%20Multiply%20Strings.cpp)
	+ 任意大小非负数字乘积计算(`string`类)，按结果逐位计算；
	+ 注意避免头插和`find_first_not_of("0")`可以去掉头部的'0'；
	+ 保证num1，num2访问不越界。
6. [Wildcard Matching](https://github.com/applefishsky009/LeetCode/blob/master/44%20-%20Wildcard%20Matching/44%20-%20Wildcard%20Matching.cpp)
	+ 利用字符串和字符串指针来回溯；
	+ 正则匹配的思路。
7. [Longest Common Prefix](https://github.com/applefishsky009/LeetCode/blob/master/14%20-%20Longest%20Common%20Prefix/14%20-%20Longest%20Common%20Prefix.cpp)
	+ 参考[Null-terminated string](https://en.wikipedia.org/wiki/Null-terminated_string)
8. [Valid Number](https://github.com/applefishsky009/LeetCode/blob/master/65%20-%20Valid%20Number/65%20-%20Valid%20Number.cpp)
	+ 这种题看似简单，但是坑巨多无比;
	+ 完成' '的strip功能;
	+ '.'小数点的判断；
	+ 'e'指数，底数可以是小数，但是指数只能是整数；
	+ '+'，'-'的有效性；
	+ 最后，神一般的有限状态机解法，不知道说什么好了。
9. [Roman to Integer](https://github.com/applefishsky009/LeetCode/blob/master/13%20-%20Roman%20to%20Integer/13%20-%20Roman%20to%20Integer.cpp)
	+ 除数留余，避免`substr()`方法，使用`Hash Table`让提升效率。
10. [CountandSay](https://github.com/applefishsky009/LeetCode/blob/master/38%20-%20Count%20and%20Say/38%20-%20Count%20and%20Say.cpp)
	+ dp思路，一层一层找。
11. [ZigZag Conversion](https://github.com/applefishsky009/LeetCode/blob/master/6%20-%20ZigZag%20Conversion/6%20-%20ZigZag%20Conversion.cpp)
	+ 分类，注意找对称元素。
12. [Group Anagrams](https://github.com/applefishsky009/LeetCode/blob/master/49%20-%20Group%20Anagrams/49%20-%20Group%20Anagrams.cpp)
	+ 这个题与ip消息队列，LRU cache有相似的地方，键值映射一个队列/链表。
13. [Simplify Path](https://github.com/applefishsky009/LeetCode/blob/master/71%20-%20Simplify%20Path/71%20-%20Simplify%20Path.cpp)
	+ 这个题和处理`,`分割的数字输入类似，使用`find`分类讨论。
14. [Text Justification](https://github.com/applefishsky009/LeetCode/blob/master/68%20-%20Text%20Justification/68%20-%20Text%20Justification.cpp)
	+ 除了最后一行，字符串按位数均匀分布(两个字符串之间至少有一个空格)，注意如果只有一个单词要补充足够的空格；
	+ 最后一行单词左排列就行(依然至少一个空格)，要补充足够的空格。

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

## 完整的strcpy函数

```C++
char * strcpy( char *strDest, const char *strSrc ) {
	assert( (strDest != NULL) && (strSrc != NULL) );
	char *address = strDest; 
	while( (*strDest++ = * strSrc++) != ‘\0’ ); 
 	return address;
}
```

---

## cctype

主要是`cctype`头文件中的字符函数在编程过程中可以带来很多便利，常用的总结如下：
1. `isalpha()`,`isdigit()`,`isalnum()`可以用于判断是字符、数字、字母或数字。返回`true`或`false`。
2. `islower()`,`isupper()`,`isprint()`可以用于判断是小写字母、大写字符、可显示字符。返回`true`或`false`。[可显示字符](https://zh.wikipedia.org/wiki/ASCII#.E5.8F.AF.E6.98.BE.E7.A4.BA.E5.AD.97.E7.AC.A6)从32到126一共95个。
3. `tolower()`,`toupper()`用于大小写字母的转换，如果不需要转换，字符不变。
4. 另外一些不常用的字符函数,`isgraph()`(除空格之外的打印字符),`ispunct()`(标点符号),`isspace()`(标准空白字符),`iscntrl()`(控制字符),`isxdigit()`(16进制,即1-9,a-f,A-F)。
5. 尤为注意的是除了这些判断函数，`<cctype>`中还包括两个转化函数(^.^主要是可以作为函数对象，很方便)

---