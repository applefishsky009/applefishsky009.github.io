---
title: BP、KMP、改进的KMP
date: 2016-05-03 14:57:41
category: 数据结构与算法
tags: Algorithm

---

在主串S中，自low位置开始，查找与模式串T相等的子串。如果这样的子串存在，返回其第一次出现的位置。本文详细介绍三种字符串匹配算法:
1. BF(Brute-Force)算法(暴力破解);
2. KMP算法;
3. 改进的KMP算法。

这三种算法的代码[在这里](https://github.com/applefishsky009/Interface/blob/master/BF%E5%92%8CKMP/BF%E5%92%8CKMP.cpp)。

---

## BF算法

设主串从i位开始与子串(从0开始)判断是否匹配;到子串的j位置，对应主串的i+j位置失配;将主串左移一位(`i++`),j回到0位继续匹配。

---

## KMP算法

### 为什么是next[j]
每次失配将i右移一位显然是低效的。**主观上来考虑，如果在失配之前子串有相等的真后缀，那么就可以右移更多的位。**考虑在i+j位失配时将主串左移k位(子串右移k位)，而不是一位。容易得到，这个k只与子串的性质有关。
我们使用next[j]来标识当j位失配时子串应向右移j-next[j]位。示例:

| `abcdefg`	|  0 1 2 3 4 5 6	|
| :---:		| :---:				|
| `next[j]`	| -1 0 0 0 0 0 0	|
| offset	|  1 1 2 3 4 5 6	|

| `abcabcabc`	|  0 1 2 3 4 5 6 7 8	|
| :---:			| :---:					|
| `next[j]`		| -1 0 0 0 1 2 3 4 5	|
| offset		|  1 1 2 3 3 3 3 3 3	|

### 如何求得next[j]
![这里](http://i.imgur.com/qc2eynB.png)
[这里]:next[j]
在计算公式中第二行指的是:j位**以前**字串中真前后缀的最大**公共**元素长度。真前缀、真后缀指的不包含串本身的子串。那么我们可以这样来计算j:
tempNext[j]表示j位及以前子串真前缀最大公共元素长度。将tempNext[j]右移一位，初值赋为-1,得到next[j]

| `xyxyyxxyx`	|  0 1 2 3 4 5 6 7 8	|
| :---:			| :---:					|
| `tempNext[j]`	|  0 0 1 2 0 1 1 2 3	|
| `next[j]`		| -1 0 0 1 2 0 1 1 2	|
| offset		|  1 1 2 2 2 5 5 6 6	|

tempNext[j]的计算:
```
//计算j位及之前真前缀以及真后缀的最大公共元素长度
void calTempNext(vector<int>&next)
{
	if (s2.size() == 1)
		return;
	next[0] = 0;
	int k = 0;//前缀指针,j就是后缀指针
	for (int j = 1;j < s2.size();j++)
	{
		while(k > 0 && s2[j] != s2[k])//k位失配，
			k = next[k-1];//k-1是可靠匹配，next[k-1]记录了上一个真后缀出现的地方
		if (s2[j] == s2[k])//匹配，k++,j++，next[j]赋值
			k++;
		next[j] = k;
	}
	return;
}
```
### 如何在编程中求得next[j]
如果使用上述计算过程，先计算tempNext[j]再计算next[j]，需要两次遍历。将tempNext[j]右移初值赋-1的过程可以直接融入程序中，使用一次遍历就可以得到next[j]，
```
//直接计算next(时间复杂度O(n))
void cal2Next(vector<int>&next)
{
	int j = -1;		//j,偏移指针
	int i = 0;		//i,next下标（实际上是要计算的next下标-1,因为是先加后赋值）
	next[0] = -1;
	while (i < s2.size()-1)
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
}
```

---

## 改进的KMP算法

next[j]值越小，模式匹配所需比较次数越少。next[j]的计算中先判断匹配，i,j自加再赋值。
1. 若自加之后失配，这时候i失配并不代表j失配，因此留给下次循环回溯后来判断。
2. 若自加之后匹配，说明i与j位置完全等效，i失配，j一定失配。，而朴素的KMP算法在失配之后要一次一次回溯。因此可以<font color=red>一次回溯到底</font>节约比较次数。

| `abcaabbabcaac`	|  0 1 2  3 4 5 6  7 8 9 10 11 12	|
| :---:				| :---:								|
| `next[j]`			| -1 0 0  0 1 1 2  0 1 2  3  4  5	|
| offset1			|  1 1 2  3 3 4 4  7 7 7  7  7  7	|
| `nextVal[j]`		| -1 0 0 -1 1 0 2 -1 0 0 -1  1  5	|
| offset2			|  1 1 2  4 3 5 4  8 8 9 11 10  7	|

[LeetCode代码][中括号很烦]
[中括号很烦]:https://github.com/applefishsky009/LeetCode/blob/master/28%20-%20Implement%20strStr()/28%20-%20Implement%20strStr().cpp