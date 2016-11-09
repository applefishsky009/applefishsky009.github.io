---
title: VS Code使用指南
date: 2016-11-09 14:13:31
category: VSCode
tags: VSCode

---

## VSCode的配置

参见[VsCode+GCC](http://rylcode.cn/2016/07/30/windows%E4%B8%8B%E9%85%8D%E7%BD%AESGI-STL/)

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
