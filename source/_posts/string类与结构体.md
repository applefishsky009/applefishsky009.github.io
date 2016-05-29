---
title: string类与结构体
date: 2016-04-21 10:36:21
category: C++基础
tags: [string.find(),结构体]

---

## string类

### string运算符
`string`类的运算符重载在头文件`string`里,如`+`,`<<`。注意`"a"+"b" = "ab"`,`'a'+'b' = 195`,前者是字符串拼接，后者是字符常量相加。但是`cout<<"a"+"b";`这个语句是错误的，必须至少声明两个`string`类型。

### string.find()
`string`类的`find()`方法，可以用于找子串，返回子串在原串出现的下标。[这里](http://www.cnblogs.com/web100/archive/2012/12/02/cpp-string-find-npos.html)和[这里](http://www.cplusplus.com/reference/string/string/find/)有详细解释，使用时注意以下三点：
+ 接受三个参数，第一个是子串，第二个是开始寻找的下标，第三个参数是匹配字串的字符数。可以用于找全部的匹配子串；
+ 如果没有找到，返回值是`string::npos`，他是一个很大的正数；
+ 返回值是`size_t`，一般可用`auto`来代替。

以下代码可以输出全部的匹配位置：
```
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

函数声明如下：`_Myt substr(size_type _Off = 0, size_type _Count = npos) const`，返回从指定位置(_Off)开始的长度为(_Count)的子字符串。

---

## 结构体

1. 基本项，可以列表化，提倡外部结构声明，可以使用赋值运算符,列表初始化不允许缩窄转换。
2. 结构体对准：
	+ 结构体首地址能被其最宽基本类型成员的大小所整除；
	+ 结构体每个成员相对于结构体首地址的偏移量(offset)都是成员大小整数倍；
	+ 结构体的总大小是结构体最宽基本类型成员大小的整数倍。
3.  与此相关还有栈对准，某些编译器(x64?)按大小对准，`char`位于栈底，`double`位于栈顶排列。找不到相关资料了，需要深入了解。

示例：
```
//Definition for a binary tree node.
struct TreeNode {
	int val;
	TreeNode *left;
	TreeNode *right;
	TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
 ```