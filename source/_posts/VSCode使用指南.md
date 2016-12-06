---
title: VSCode使用指南
date: 2016-11-09 14:13:31
category: VSCode
tags: VSCode

---

## 快捷键修改

[Key Bindings](https://code.visualstudio.com/docs/customization/keybindings)
由于经常使用**打开文件夹**功能，文件 —> 首选项 -> 键盘快捷方式，然后在`keybindings.json`中添加：
```JSON
[
    { "key": "ctrl+q",                "command": "workbench.action.files.openFolder" }
]
```

注：`command id`不知如何对应，这一条是根据打开文件的设置修改测试而来。

[VSCode KeyBindings.pdf](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

---

## Ctrl + shift + P模式(主命令框)

调出命令列表，查看所有的命令/快捷键;`ctrl + P`模式和他可以切换(`>`和`BackSpace`)
1. **alt**
	+ 显示菜单栏，平时可以在这个模式下输入`view: toggle Mebu Bar`来切换正常状态下菜单栏是隐藏的还是显示的;

---

## Ctrl + P模式

1. 输入`>`回到主命令框`ctrl + shift + p`模式；
2. 直接输入文件名打开文件(可以用`ctrl + o`打开文件)；
3. `?`显示当前可执行操作；
4. `:`跳转到行数；
5. `ctrl + shift + m`显示errors或warnings(`!`模式不能用？);
6. `@`查找变量(全局)或函数`ctrl + shift + o`进入。
7. `@:`查找函数或者属性。
8. `#`查找符号；
	+ 比`ctrl + F`更细化。

---

## 编辑器和窗口管理

### 同时打开多个窗口
1. **ctrl + shift + N**
	+ 打开新的窗口，注意markdown打开新窗口的快捷键也是这个(虽然没有卵用)，chrome打开新窗口的快捷键是`ctrl + N`,`ctrl + shift + N`可以打开新窗口并进入隐身模式。
2. **ctrl + shift + W**
	+ 关闭当前窗口。
3. **ctrl + alt + up/down**
	+ 屏幕向上/向下。 

### 同时打开多个编辑器
1. ** ctrl + N**
	+ 新建文件。
2. ** ctrl + tab, alt + left, alt + right**
	+ 打开文件之间的切换(`ctrl + tab`)。
3. ** ctrl + \**
	+ 切出一个新的编辑器(最多三个)，也可以`ctrl` + 点击Explore里边的文件名。
4. ** ctrl + k** + **left/right**
	+ 编辑器换位置。
5. ** ctrl + `**
	+ 打开集成终端，比如`make clean`就可以在这里输入，`cls`指令可以清理屏幕。


---

## 代码编辑

### 格式调整
1. **ctrl + \**
	+ 拆分编辑器。`ctrl + 1`,`ctrl + 2`,`ctrl + 3`左中右三个编辑器的快捷键。
2. **alt + shift + F**
	+ 代码段自动对齐！！！
3. **ctrl + /**
	+ 注释光标所在行。
4. **alt + up/down**
	+ 将光标所在的代码上移或者下移一行。
5. **shift + alt + up/down**
	+ 向上向下复制一行。
6. **ctrl + enter**
	+ 在当前行下方插入一行。
7. **ctrl + shift + enter**
	+ 在当前行上方插入一行。
8. **ctrl + shift + [/]**
	+ 打开/折叠代码块。
9. **ctrl + [/]**
	+ 代码行缩进(注意很方便的一点就是光标不需要在这一行首)。
10. **ctrl + c/v**
	+ 如果不选中，默认复制或者粘贴一整行。

### 光标相关
1. **home/end**
	+ 移动到行首/行尾。
2. **ctrl + home/end**
	+ 移动到文件开头/结尾。
3. **ctrl + i**
	+ 选中当前行。
4. **shift + home/end**\
	+ 选中从光标处到行首/行尾。
5. **ctrl + delete**
	+ 删除光标右侧所有的字(以单词为单位)。
6. **ctrl + shift + l**
	+ 同时选中所有匹配的( = **ctrl + F2**)。
7. **ctrl + u**
	+ 回退上一个光标操作。
8. **ctrl + d**
	+ 下一个匹配的也被选中。
9. **ctrl + shift + left/right**
	+ 光标的左右扩展。
10. **alt + click**
	+ 添加多个光标，连续选择多处一起修改。

### 重构代码
1. **F5**
	+ 调试。
2. **F9**
	+ 加断点。
3. **F10**
	+ 逐行运行。
4. **F11**
	+ 进入该行的函数内部调试(step into)。
5. **F12**
	+ 跳转到定义处。
6. **alt + F12**
	+ 预览定义而不是跳转。

### 查找和替换
1. **ctrl + F/H**
	+ 查找/替换。

### 显示相关
1. **F11**
	+ 全屏。
2. **ctrl + =/-**
	+ 视距放大/缩小(Zoom In/Out)。
3. **ctrl + b**
	+ 侧边栏显/隐。
4. **ctrl + shift + e/f/g/d/x**
	+ 侧边栏五大功能，资源管理器/搜索/git/调试/扩展。
5. **ctrl + shift + v**
	+ 预览`markdown`。
6. **ctrl + shift + u**
	+ 显示输出窗口。


参考资料：
[Vscode中一些方便的快捷键](http://www.jianshu.com/p/1b7b8760504c)
[学会用好 Visual Studio Code](https://nshen.net/article/2015-11-20/vscode/)

---

## 皮肤预留

在**ctrl + shift + p**模式下输入`theme`选择文件图标主题韩式编译器颜色主题，然后上下移动光标可以预览。

---

## 强大的自定义用户代码块

在 文件 -> 首选项 -> 用户代码片段 -> 选择语言，配置弹出的`.json`可以自定义用户代码块块，比如我的配置的一部分：
```JSON
	//add Ryl module
	"include RylModule": {
		"prefix": "#include",
		"body": [
				"#include <E:\\RylModule\\show.h>\n",	//单斜杠转义，比如\n
				"$1"	//$id(id = 1,2,...)表示插入代码块后可快速编辑的位置，可用tab切换
		],
		"description": "RylModule"
	}
```

参考资料：[Visual Studio Code自定义用户代码块](http://www.jianshu.com/p/85e707cc5c5c)

---