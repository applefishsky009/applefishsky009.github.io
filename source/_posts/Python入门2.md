---
title: Python入门2
date: 2016-12-10 10:14:09
category: Python
tags: Python

---

数字篇

---

## 更新
他的更新与删除在Python对象介绍中已经说过，注意只缓存<font color=red>简单整型</font>。

---

## 类型
1. 布尔型
2. 整型(类似于C中长整型)
3. 浮点(类似于C中双精度浮点型)
4. 复数,属性如下：
	+ num.real 返回实部
	+ num.imag 返回虚部
	+ num.conjugate 返回共轭复数

---

## 操作符

### 类型转换
1. 如果有一个操作数是复数，另一个操作数被转换为复数；
2. 否则，如果有一个操作数是浮点型，另一个操作数被转化为浮点型；
3. 否则，如果一个操作数是长整型，则另一个操作数被转化为长整型；
4. 否则，两者必然普通整型，无需类型转化。

### 标准类型操作符
`==`,`>=`,`<=`,`<`,`>`,`!=`,and,or

### 算术操作符
`+`,`-`,`*`,`/`,`%`,`//`,`**`
1. 取余运算的本质：x/y的余数 = x - x//y*y

### 整型的位操作符
`~`,`<<`,`>>`,`&`,`^`,`|`

---

## 内建函数和函数工厂

### 转换工厂函数
1. bool(obj)
2. int(obj,base = 10),(Python3中取消了函数工厂long()，因为逐渐不区分整型和长整型)、
3. float(obj)
4. complex(str) or complex(real, imag = 0.0)

### 功能函数
1. abs() 返回绝对值
2. coerce(num1, num2) 转换为同一类型并返回元组
3. divmod(num1, num2) 除法-取余运算的结合，返回一个元组(num1/num2, num1%num2)
4. pow(num1, num2, mod = 1) 幂函数，提供余数时候效率比`**`高
5. round(flt,ndig = 0) 返回最接近原数的整型(浮点型)，可以设置小数点位数
	+ int()直接截去小数部分，返回整型
	+ math.floor()，得到的是最接近原数但小于原数的整型

### 仅用于整型的函数
#### 进制转化
1. 八进制oct()，返回一个对应值的字符串对象
2. 十进制hex()

#### ASCII转换函数 
1. 返回字符串chr()
2. 返回整型值ord()

---

## 其他数字类型

### 布尔型
1. 布尔型是整型的子类，但不能再被继承而生成他的子类
	+ `True + 100 = 101`

### 十进制浮点型
由于C中二进制浮点型的精度问题，Python提供了十进制浮点型的包
```Python
from decimal import Decimal
```

---

## 数学相关模块

1. decimal
2. array
3. math/cmath
4. operator
5. random
	+ randint() 左闭右闭区间
	+ randrange() range()函数一样，可以传递起始值(默认为0)，步长(默认为1)，超尾(不可缺少)。
	+ uniform() 几乎和randint一样，只不过返回的是浮点型
	+ random() 类似于uniform()，只不过下线恒等于0.0，上限恒等于1.0
	+ choice() 随机返回给定序列的一个元素
```Python
from random import randint
...
```

[TypeError: 'module' object is not callable](http://stackoverflow.com/questions/4534438/typeerror-module-object-is-not-callable)

