---
title: i++与++i的区别
date: 2016-05-06 14:22:46
category: C++的类
tags: 自增减运算符重载

---

一直以来对`++i`与`i++`，知道前者效率更高，但是不知道为什么。有人说与寄存器有关，实际上寄存器都执行了一次加法，是一样的。看了[这里的博客](http://falldog7.blogspot.jp/2007/10/programmer-i-i.html)才恍然大悟，原来是其实现的机制不同。

`++i`先加再用，其对i类型(假设为INT)的++运算符重载如下：
```
//++i
INT operator ++(){
	this->_value++;
	return *this;
}
```

`i++`先用再加，创建临时变量保存原有值用来返回，后调用++i语句，重载如下：
``` 
//i++
INT operator ++(int t){
	INT temp(_value);//!!! 必須create出一個temp的變數!!!
	this->_value++;
	return temp;
}

```
在这个函数中参数n是没有任何意义的，它的存在只是为了区分是前置形式还是后置形式。

可以看到`i++`所必须付出的代价，就是多create出一个temp的变量，以及temp变量的的constructor()。

