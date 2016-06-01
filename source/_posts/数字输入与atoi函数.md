---
title: 数字输入与atoi函数
date: 2016-05-22 16:14:08
category: C++基础
tags: [输入,atoi]

---

这篇博客来源于编程中的数字输入检查，最终选择`cingetline()`加`atoi()`函数进行输入检查转换。

---

## 输入

### cin的学问
首先分析`cin`,`cin.get()`,`cin.getline()`:
1. `cin<<`会忽略**有效字符前的**空格，换行符，制表符(开始有效输入之后非法就退出了)；
2. `cin.get()`读取每一个字符；
3. `cin.getline()`读取一行到字符串中，并把`\n`替换为`\0`存储。

测试中发现更多的问题：
1. cin.getline(字符指针(char*),字符个数N(int),结束符(char));其只读取N-1个字符，因为最后一个字符要补`\0`,如果<font color=red>输入超过N-1</font>,会将状态位设置：`ios_base::failbit`(表示轻微错误，可以挽回。查看`cin.getline()`源码很明显)，注意剩下的字符依然在输入队列中。
2. `cin.getline()`超过N-1输入状态位设置：`ios_base::failbit`(可修复的)，这时候不能使`cin<<`和`cin.get()`和`cin.getline()`，此时输入队列有值。应该用`cin.clear()`设置状态位`ios_base::goodbit`才能继续读取。
3. [cin.sync()](https://www.zhihu.com/question/40160488)亲自测试`cin.sync()`并不是如网上所说的清空缓冲区(相信英语是对的...),`cin.ignore()`才是。

在编写主界面时，经常会碰到如下语句：
```
int index = 0;
switch(index){
	case 1:
	case 2:
	......
	default：
}
```
上述语句没有对`index`变量进行输入检查，显然不安全，那么如何进行输入检查？

### 一般的错误处理机制
```
int index;
while (!cin<<idex) {
	cin.clear();
	while(cin.get()!='\n')
		continue;
	cout<<"请再次输入"<<endl;
}
```
这里会解决错误输入的问题,但是有一个新的问题，如输入`5a`，`cin<<`只会让`index=5`，`a`留在输入队列中，会影响下一次的输入。

### atoi()引发的思考

原型：`int atoi(const char *nptr);`;
头文件：`#include <cstdlib>`;
[LeetCode][1]
[1]:https://github.com/applefishsky009/LeetCode/blob/master/8%20-%20String%20to%20Integer%20(atoi)/8%20-%20String%20to%20Integer%20(atoi).cpp
这里有一个非常重要的概念，不管是数字输入还是字符串转数字，都是**遇到有效输入开始读取，直到碰到无效输入退出**
也就是说其实`cin<<`就是对输入缓存进行了一个atoi()。区别就是在`cin<<`退出之后同行中的非法输入(如果有)还是留在输入队列中，影响后续读入。
因此可以使用`cingetline()`加`atoi()`函数进行输入检查转换是安全的。代码如下：
```C++
int k = 0;
char s1[20];	//注意分配空间和,很久没用char[]，
cin.getline(s1,20);
k = atoi(s1);
```
之后大于19的部分可以正常用`getchar()`读取，`clear()`之后可以用cin方法读取，但是之前的输入依然在队列中，如果不想要之前的部分，要使用`ignore()`方法。