---
title: Fork解析
date: 2016-11-10 10:19:54
category: 操作系统
tags: 进程

---

## 进程

测试：[Fork测试](https://github.com/applefishsky009/OperationSystem/blob/master/1-Fork/fork.cpp)

参考unix环境高级编程，主要是测试fork的机制，其中两个函数和一个宏值得注意：

### `pid_t pidTmp = fork();`
创建一个进程(vfork才保证子进程先运行)，id是pidTmp，子进程的返回值是0，父进程的返回值是新建子进程的id，<font color=red>因为父进程无法获得所有子进程的id。</font>

### `pid = waitpid(pidTmp,&status,0);`
请注意不要在`waitpid`函数之前判断`pidTmp > 0`来 在父进程中等待子进程完成。如果指定的进程或进程组不存在，或者参数`pidTmp`指定的进程不是调用进程的子进程，或者<font color = red>不存在子进程</font>，该函数会出错，`status`为0，返回值pid为-1(出错)。
参考资料：
[僵尸进程的产生和避免,以及wait，waitpid的使用](http://www.programgo.com/article/85414485023/)
[LINUX编程基础之进程等待（WAIT()函数）](http://blog.csdn.net/mybelief321/article/details/9066359)

### WEXITSTATUS(status)
如果直接输出`status`可以看到其与`exit()`的返回码并不相同，这是由于`status`主要有三部分组成：
1. bits 0-6 为 termination signal;
2. bits 7 为 core dump flags;
3. bits 8-15 为 exit status;

|		|		|		|		|		|
|  :-:	| :-:	| :-:	| :-:	| :-:	|
| status | 0 0 0 0 0 0 0 0 |        0           | 0 0 0 0 0 0 0      |
| bits   |     8 - 15      |        7           |     0 - 6          |
| mean   |   exit status   | coredump generated | termination signal |

因此从`status`转化到返回码就需要一个宏`WEXITSTATUS(status)`。

参考资料：
[关于 Linux 进程结束状态](http://cs-cjl.com/2015/11_18_linux_process_exit_status)

---

## 循环中的fork机制

参考资料：[一个fork的面试题](http://coolshell.cn/articles/7965.html)
测试：[Fork in Loop测试](https://github.com/applefishsky009/OperationSystem/blob/master/1-Fork/fork2.cpp)

参考资料讲得非常好。除了进程示意图，还有一点非常重要，<font color=red>标准输出是行缓冲，所以遇到`\n`的时候会刷出缓冲区</font>，如果不想见到`\n`也可以使用[std::flush](http://www.cplusplus.com/reference/ostream/flush-free/?kw=flush)将数据刷出缓冲区，另外，一个进程退出时也会将数据刷出缓冲区。

根据以上规则,另外已知`fork()`能拷贝缓冲区，很容易就推出(假设循环次数为2,也就是说应该输出6个字符)：
```C++
......
if (pid > 0) cout << "B" << endl;	//主进程
else cout << "A" ;
......
输出：
AAAB	//第一个子进程A保留在缓冲区被fork，到达第二次循环就会这么输出
B	//主进程的B
AB	//主进程在第二次循环中的输出

......
if (pid > 0) cout << "B";	//主进程
else cout << "A";
...
cout << endl;
//cout << flush;
......
输出：
AA
AB
BA
BB
//AAABBABB	
//在第一次循环中都没有输出，fork在子进程第二次循环

......
if (pid > 0) cout << "B" << flush;// << endl	//主进程
else cout << "A" << flush;// << endl;
...
cout << endl;
//cout << flush;
......
标准输出：
AABBAB
//A
//A
//B
//B
//A
//B
//理论上的标准输出。
```

因此可以得到结论：
1. 如果不是需要用`fork()`拷贝缓冲区的特性，最好在`fork()`之前`flush`(刷出缓冲区)，其标准输出是\\( 2^1+....+2^{loopCount} \\)
2. 如果没有`flush`没有`endl(即\n)`，他的输出次数是\\( 2^{loopCount}*loopCount \\)
3. 如果子进程和父进程的输出缓冲规则不一致，请逐步分析。
4. 如果没有刷出缓存(`flush`或`\n`),但只输出了6个(理论上是8个),考虑到断点设置导致的后先调用的父进程的缓冲区没有刷出，因此会缺少。
$$ $$

---