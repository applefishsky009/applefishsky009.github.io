---
title: VSCode配置Python环境
date: 2017-01-19 17:11:03
category: VSCode
tags: VSCode

---

## VSCode与插件
1. VSCode安装省略。
2. 插件选择`Python`，作者是Don Jayamanne，下载量最多的那个。

---

## Python环境
安装环境时注意重启VSCode，powershell，计算机。
1. 安装Python，注意安装的时候[Add python.exe to Path](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001374738150500472fd5785c194ebea336061163a8a974000)，[一招搞定windows安装路径配置](https://www.zhihu.com/question/22621185)，如果安装的时候没有选择，手动添加`Python.exe`到环境变量即可。
2. 配好环境变量在cmd中输入`python`，可以看到版本信息。因为我用的是powershell，在cmd中可以看到版本信息而在powershell中看不到，解决方案是[Running Python in powershell?](http://stackoverflow.com/questions/19676403/running-python-in-powershell)
3. 参数配置：

launch.json
```JSON
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python",
            "type": "python",
            "request": "launch",
            "stopOnEntry": true,
            "program": "${file}",
            "debugOptions": [
                "WaitOnAbnormalExit",
                "WaitOnNormalExit",
                "RedirectOutput"
            ]
        },
        {
            "name": "Python Console App",
            "type": "python",
            "request": "launch",
            "stopOnEntry": true,
            "program": "${file}",
            "externalConsole": true,
            "debugOptions": [
                "WaitOnAbnormalExit",
                "WaitOnNormalExit"
            ]
        },
        {
            "name": "Django",
            "type": "python",
            "request": "launch",
            "stopOnEntry": true,
            "program": "${workspaceRoot}/manage.py",
            "args": [
                "runserver",
                "--noreload"
            ],
            "debugOptions": [
                "WaitOnAbnormalExit",
                "WaitOnNormalExit",
                "RedirectOutput",
                "DjangoDebugging"
            ]
        },
        {
            "name": "Watson",
            "type": "python",
            "request": "launch",
            "stopOnEntry": true,
            "program": "${workspaceRoot}/console.py",
            "args": [
                "dev",
                "runserver",
                "--noreload=True"
            ],
            "debugOptions": [
                "WaitOnAbnormalExit",
                "WaitOnNormalExit",
                "RedirectOutput"
            ]
        }
    ]
}
```
tasks.json
```JSON
{
    "version": "0.1.0",
    "command": "python",
    "isShellCommand": true,
    "args": ["${file}"],
    "showOutput": "always",
    "options": {
        "env": {
            "PYTHONIOENCODING": "UTF-8"
        }
    }
}
```

---

## Run Build Task 中文乱码 BUG
安装好之后F5运行，在dubug窗口显示的输出格式并不正确，按`ctrl + shift + B`运行(tasks指令)，安装好其他两个插件之后`Configure Task Runner`，即写好tasks配置文件，运行后可能出现乱码。
解决方案：
[VSCode Python 配置](https://segmentfault.com/a/1190000005986197)
我采用在tasks.json中配置options参数来解决。

---

## task配置
详细信息查看官网。
[Integrate with External Tools via Tasks](http://code.visualstudio.com/docs/editor/tasks)

> The final command line is constructed as follows:

> If suppressTaskName is true, the command line is <font color=red>command 'global args' 'task args'</font>.
> If suppressTaskName is false, it is <font color=red>command 'global args' taskName 'task args'</font>.

---
