---
title: 深入了解string类
date: 2016-07-01 14:58:51
category: C++的类
tags: string

---

之前由于编程过程中经常使用，在[string类与cctype](http://rylcode.cn/2016/04/21/string%E4%B8%8Ecctype/)等博客中简单介绍过一些`string`相关的方法，迭代器，算法的用法。这篇主要是以一个类的角度深入系统地管理`string`类知识。可在[cplus上的string](http://www.cplusplus.com/reference/string/)查阅各种方法用法。

---

## 模板具体化

`string`是模板`basic_string<char>`的一个typedef，`basic_string`有四个具体化，都有`typedef`名称：
```C++
template<class charT, class traits = char_traits<charT>, class Allocator = allocator<charT> >
basic_string{...};

typedef basic_string<char> string;
typedef basic_string<wchar_t> wstring;
typedef basic_string<char16_t> u16string;
typedef basic_string<char32_t> u32string;
```
1. `charT`表示选定字符类型；
2. `traits`描述选定字符类型的特定情况(如值比较)；
3. `Allocator`类管理内村分配，每种字符类类型，都有预定义(默认)的`allocator`模板具体化，使用`new`和`delete`。

---

## 构造函数

```C++
default (1)	string();
copy (2)	string (const string& str);
substring (3)	string (const string& str, size_t pos, size_t len = npos);
from c-string (4)	string (const char* s);
from buffer (5)	string (const char* s, size_t n);
fill (6)	string (size_t n, char c);
range (7)	template <class InputIterator>	//指针模板
  			string  (InputIterator first, InputIterator last);
initializer list (8)	string (initializer_list<char> il);
move (9)	string (string&& str) noexcept;
```
大部分很容易理解，特别说明的几个：
1. (5)中从s指针开始位置开始构造n个字符，即使超过了NBTS(以空字符结束的字符串)；
2. (8)是为了普遍化列表初始化语法，其使用意义并不大；
3. (9)-移动构造函数与(2)的区别是不将`string`视为`const`，主要用来在一些情况下优化性能。

---

## 输入

`string`对象有两种输入方式(`cin.getline()`和`cin.get()`是C风格的输入方式)：
```C++
string s;
cin>>s;	//read a word
getline(cin,s);	//read a line, discard \n, 接受第三个参数作为分界符，默认\n
```
`getline()`的`string`版本是一个独立的函数而不是`istream`类的方法，他可以自动调整目标对象大小使之刚好存储，他受到两个条件限制：
1. `string`对象最大允许长度(`string::npos`)，通常是`unsigned int`最大值；
2. 内存量大小。

他在以下情况下停止读取：
1. 到达文件尾(EOF)，输入流`ios_base::iostate`设置为`ios::eof`(只有`ios::good`才能读取，`cin.clear()`可纠正)；
2. 分界字符(默认为`\n`)；
3. 读取字符数最大允许值`string::npos`(注意他的类型为`size_t`)，输入流`ios_base::iostate`设置为`ios::fail`

---

## 使用字符串

### 比较
主要是重载了关系运算符。

### 搜索
以`find()`方法为例：
```C++
string (1)	size_t find (const string& str, size_t pos = 0) const noexcept;
c-string (2)	size_t find (const char* s, size_t pos = 0) const;
buffer (3)	size_t find (const char* s, size_t pos, size_type n) const;
character (4)	size_t find (char c, size_t pos = 0) const noexcept;
```
注意(3)中n表示限制了子串的长度。下列其他的方法原型类似：
1. `rfind()`找最后一次出现的位置；
2. `find_first_of()`找参数中**任何**字符第一次出现的位置；
3. `find_last_of`找参数中**任何**字符最后一次出现的位置’
4. `find_first_not_of`找字符串中第一个不在参数串中的字符位置；
5. `find_last_not_of`找字符串中最后一个不在参数串中的字符位置。

---

## 其他功能
列出功能提纲，如有需要可以扩充上一目录中：
1. 删除(erase)；
2. 替换(replace)；
3. 插入(insert)；
4. 提取(substring)；
5. 复制(copy)；
6. 交换(swap)；

---

## 重载的运算符

1. `+`；
2. `=`；
3. `+=`;
4. `[]`;
5. `>>`;
6. `>>`;
7. 关系运算符。