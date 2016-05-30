---
title: Python爬虫抓取知乎图片
date: 2016-05-30 16:58:08
category: Python爬虫
tags: [知乎图片爬虫]

---

初学Python，数据结构感觉很简单，语法什么也太过于枯燥无味。就从爬虫做起，使用Python3.5版本，在Pycharm中编写，用自带的IDE调试(这个真好用啊)，花了两天时间终于做好了自己的Python爬虫，先抓取某个用户一个回答的所有图片，然后抓取某一知乎用户的所有图片，代码见[Github](https://github.com/applefishsky009/PythonReptile)，感觉新世界的大门在缓缓开启啊，^.^~

另外，由于只学习几天就急着做爬虫玩，导致Python基本功不行，代码风格还是乱七八槽的，估计以后也懒得修改，主要关注内容原理吧。

---

## 工具

首先介绍使用的三个模块，在实现中节省不少功夫，不知道的同学可以去搜，这里主要分析如何实现功能以及为什么这么使用，不会介绍模块。
1. bs4，`from bs4 import BeautifulSoup`，模块文档在这里[doc](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)，这是一个非常好用的网页解析工具。简单来说，将抓回来的页面解析成`html`格式以便于查找定位。
2. shutil，`import shutil`导入这个模块主要是使用文件夹操作，主要是因为`mkdir`使用时如果目录中已有想创建的文件夹，会返回错误，我用shutil删除重建，保证程序的稳定性。
3. linecache，`import linecache`导入这个模块读取本地txt文件中的信息，存储了两行，第一行是要爬取的**主页**,第二行是保存图片的文件夹。注意`linecache.getline()`方法会读取回车，去掉就行。

### bs4
```python
def url_open(url): #打开页面]
    try:
        with urllib.request.urlopen(url,timeout=30) as response:
            html = response.read()
        return html
    except Exception as e:
        print(e)
        return url_open(url)

anwser_page = url_open(table_tail)
soup = BeautifulSoup(anwser_page,'html.parser')
soup = BeautifulSoup(one_page, 'html.parser')
```
简单强调一点，bs4的`find()`和`find_all`方法，第一个返回一个bs4.Tag类，第二个返回的是这个类的`Set`,因此`find_all`之后一定要在循环中对`Set`的每一个成员才能使用`find()`方法。
大部分页面不登录就能访问，个别页面需要登录才能访问，因此需要判断`soup.find()`返回类型是不是`None`，可以登录才能爬取图片。

### shutil
```python
if os.path.isdir(folder):
	shutil.rmtree(folder)
	os.mkdir(folder)
	print('删除已有目录，创建新目录')
else:
	os.mkdir(folder)
	print('没有指定目录，创建新目录')
```

### linecache
```python
person = linecache.getline(r'E:\Python\pic_answer_page\input.txt',1).split('\n')[0]
folder = linecache.getline(r'E:\Python\pic_answer_page\input.txt',2).split('\n')[0]
```
由于`linecache.getline()`方法会读取回车，因此需要分割字符串去掉`\n`。

---

## 简易

这里只爬取了知乎一个回答页面的图片作为测试，要抓取的人和保存的文件夹都内置了，可以右键编辑`down_wshpic`函数的默认参数`folder`和这个函数下的`url`(注意一定要是某一个回答的页面)
这个简短程序主要的BUG有两个：
1. `url_open(url)`没有采取`try...catch`模块，遇见这个BUG多跑几次就行了，在进阶的程序中也做了修改，可以直接替换函数。
2. 抓取的知乎页面提示需要登录才能访问，这个问题本不该出现，下次制作爬虫再解决
代码在这里[抓取单个页面](https://github.com/applefishsky009/PythonReptile/blob/master/pic_page/pic_page.py)

---

## 进阶

找到一个你感兴趣的人，主页是这样的形式`https://www.zhihu.com/people/xxxx`，为了熟悉定位过程，程序会自动定位回答页面，解析回答页面数，解析所有回答的链接，爬取图片。
代码在这里[抓取某位用户的所有图片](https://github.com/applefishsky009/PythonReptile/blob/master/pic_people/pic_people.py)
主页信息和要保存图片的路径信息都在txt中保存，第一行是主页信息，第二行是保存文件的路径，形式见[这里](https://github.com/applefishsky009/PythonReptile/blob/master/pic_people/input.txt)

---

## 卡死的爬虫

1. 可以看到在进阶中和简易的爬虫中的`url_open(url)`是不一样的，这是由于网络响应不全或者服务器不响应导致的(注意如果服务器不响应，`except`后边要用`pass`)，设置一个响应超时的参数(30s)，如果还未抓取`urlopen()`会抛出一个异常，因此采用`try...except`来抓取异常，并重新请求响应(因为知乎服务器大部分情况下，我们认为是响应不全)。如果用`pass`这个页面就被跳过了。如下：
![超时重连](http://i.imgur.com/tKh1O4Z.png)
2. 在爬取的过程中还有一个问题，大部分页面是不需要的登陆就可以访问的，偶尔有极个别的人登陆之后才能访问，这些页面，爬取不到，直接跳过，以后的制作的爬虫会发送登录信息。
![抓取错误](http://i.imgur.com/3eNTvO5.png)
3. 还有这个错误。。。原因未知，猜测是我的网挂了一下，挂着蓝灯不太稳定导致的
![BadGate](http://i.imgur.com/LTGQx7W.png)
---

吐槽一下，有的人278个回答，只有250个图片，有的人142个回答，爬了1629个图片。。。这真是太可怕了。。
一共爬取9个人获得3231张图片。

写在最后，喜欢哪个知乎上的帅哥妹子，快去收藏他/她的图库吧。。。

下一步计划，
1. 登陆网站
2. 使用正则
3. 打印PDF
