---
title: 再解KMP
date: 2017-04-06 10:21:03
category: 数据结构与算法
tags: Algorithm

---

之前在[BP、KMP、改进的KMP](http://rylcode.cn/2016/05/03/BP%E3%80%81KMP%E3%80%81%E6%94%B9%E8%BF%9B%E7%9A%84KMP/)博文中介绍了计算KMP的方式以及程序理解，经过一段时间的沉淀，现在从算法角度和编程两个角度结合其他算法探求沉思KMP算法的本质。

---

## 算法角度

结合DP思维，记录了前后缀匹配长度。
DP的思想体现首先在于<font color=red>**无后效性**</font>，`next[j]`代表`j`位以前字符串中真前后缀的最大公共元素长度，即不包含`j`自身。`j`位以前的记录和`j`位以后无关，这就是无后效性。但注意在之前博文中改进的KMP算法使用到了`j`位的字符比较。
其次体现在状态转移上，`j`位的记录仅由`j-1`位和当前字符和前缀指针是否匹配来共同决定，这就引出了编程使用双指针fail pointer的决策实现。

---

## 编程角度

fail pointer问题。在编程实现中，可以用失配指针来解决，双指针一个指向真前缀，失配则回溯，一个指向真后缀，由前一状态和当前是否匹配来共同决定。
值得注意的是，在`nextTemp[j]`的计算中，如果`i`的值代表了后缀指针，那么`nextTemp[i-1]`的值就代表了前缀指针，

```C++
vector<int> next(l.size(), 0);
for (int i = 1; i < l.size(); i++)
{
    int j = next[i - 1];    // 前一状态，同时这个值也是相同的真前缀长度
    while (j > 0 && l[i] != l[j])   //失配
        j = next[j - 1];    //  回溯，因为next[j]记录了匹配位置
    next[i] = (j += l[i] == l[j]);//回溯后再判断是否匹配
}
```

在[一幅图让你彻底理解KMP算法](http://www.rudy-yuan.net/archives/182/)这个博文中对回溯的过程解释的比较清楚，即在回溯到的子字符串中，其实也经过了前缀后缀的匹配过程，因此需要多次回溯，回溯的跳出条件为回溯到底或者完全匹配为止。因此从`nextTemp[j]`到`next[j]`到`nextVal[j]`的算法优化过程如下：

`nextTemp[j]`
```C++
vector<int> next(l.size(), 0);
for (int i = 1; i < l.size(); i++)
{
    int j = next[i - 1];
    while (j > 0 && l[i] != l[j])
        j = next[j - 1];
    next[i] = (j += l[i] == l[j]);
}
```

`next[j]`，在这里做了两件事情，将i,j初始值减一，j作为前缀指针进行了更强有力的控制。
```C++
int i = 0, j = -1;
next[0] = -1; 
while(i < l.size() - 1)
{
	while(j >= 0 && l[i] != l[j])
		j = next[j];
	j++;
	i++;
	next[i] = j;
}
```
再写`next[j]`，这里做了两件事情，去掉内层循环仅有外层循环控制，转换判断逻辑。
```C++
int i = 0, j = -1;
next[0] = -1; 
while (i < s2.size() - 1)
{
	if (j == -1 || s2[i]==s2[j])
	{
		i++;
		j++;
		next[i] = j;
	}
	else
		j = next[j];
}
```
优化至`nextVal[j]`
```C++
while (i < needle.size() - 1) //注意i少一个值
{
	if (j == -1 || needle[i] == needle[j]) //前一次失配 或 当前匹配，真前后缀长度加1
	{
		j++;
		i++;
		if (needle[i] != needle[j])
			nextVal[i] = j;
		else
			nextVal[i] = nextVal[j];
	}
	else
		j = nextVal[j];
}
```

---

参考题目：
[214. Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/#/description)
[28. Implement strStr()](https://leetcode.com/problems/implement-strstr/)