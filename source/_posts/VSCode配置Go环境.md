---
title: VSCode配置Go环境
date: 2016-12-16 11:20:12
category: VSCode
tags: VSCode

---

安装VSCode以及VSCode中Go扩展，git的过程省略

---

## 配置git环境

安装Go插件需要git指令因此首先要配置git环境,由于之前使用的是github桌面版，要在cmd下使用还需要配置环境变量。
1. gitshell使用powershell启动是由于gitshell的原因，并不是powershell可以使用git指令；
2. 因此在将VSCode shell改成powershell之后会无法识别git指令；
3. 将git的bin目录添加在环境变量PATH中，但github安装时是不能选择安装目录的，一般情况下是安装在`C:\Users\***\AppData\Local\GitHub`目录下，然后有些英文不知道什么意思的文件夹，进去之后只看有没有bin目录。有的话就说明进对了。注意不一定是一级目录，也不一定只有一个bin目录，比如我有两个bin目录`PortableGit_d7effa1a4a322478cd29c826b52a0c118ad3db11\mingw32\bin`和`PortableGit_d7effa1a4a322478cd29c826b52a0c118ad3db11\usr\bin`,其中前一个是有效的。
4. 如果安装的是git for windows，可以选择安装目录，这个问题比较容易解决。

参考资料：[GitHub cmd下环境变量设置](http://blog.csdn.net/bugmeout/article/details/21335609)

--

## 插件配置

### 插件配置选项
Preferrence->WorkSpaces Settings中设置插件：
```JSON
{
    "files.autoSave": "onFocusChange",
    "go.buildOnSave": true,
    "go.lintOnSave": true,
    "go.vetOnSave": true,
    "go.buildFlags": [],
    "go.lintFlags": [],
    "go.vetFlags": [],
    "go.useCodeSnippetsOnFunctionSuggest": false,
    "go.formatOnSave": false,
    "go.formatTool": "goreturns",
    "go.goroot": "D:\\Go",
    "go.gopath": "D:\\GoWorks"
}
```

### 安装这些插件
这些工具默认的安装方式如下：
```
go get -u -v github.com/nsf/gocode
go get -u -v github.com/rogpeppe/godef
go get -u -v github.com/golang/lint/golint
go get -u -v github.com/lukehoban/go-outline
go get -u -v sourcegraph.com/sqs/goreturns
go get -u -v golang.org/x/tools/cmd/gorename
go get -u -v github.com/tpng/gopkgs
go get -u -v github.com/newhook/go-symbols
go get -u -v golang.org/x/tools/cmd/guru
go get -u -v github.com/lukehoban/go-find-references
go get -v -u github.com/peterh/liner github.com/derekparker/delve/cmd/dlv	//调试工具
```
最终目的是在gopath中bin文件夹生成一些exe文件，如果以上命令运行成功，会自动编译生成，但我运行时有5个安装失败，因此在网上找到已经生成好的exe包。

不过<font color=red>最终我还是通过命令安装成功了</font>，个人觉得终端提示的所有错误都可以无视，因为我开了两个VSCode换着一直输入指令，然后突然就全都好了，个人猜测是长城的原因。终端提示有以下几种：
1. is not using a known version control system.
2. git remote error.
3. timeout 这是正确原因，由于长城的存在导致的。

参考资料:
<font color=red>特别重要</font>[Go for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=lukehoban.Go)
[indows下visual studio code搭建golang开发环境](http://www.cnblogs.com/JerryNo1/p/5412864.html)
[HOW TO SETUP VISUAL STUDIO CODE TO DEBUG GOLANG WITH DELVE](https://duosoftware.com/blog/how-to-setup-visual-studio-code-to-debug-golang-with-delve/)

## launch和settings

以下是simple项目的配置，复杂项目的配置以后慢慢研究。

launch.json
```JSON
{
    "version": "0.2.0",
    "configurations": [
        
        {
            "name": "Launch",
            "type": "go",
            "request": "launch",
            "mode": "debug",
            "remotePath": "",
            "port": 2345,
            "host": "127.0.0.1",
            "program": "${workspaceRoot}",
            "env": {},
            "args": [],
            "showLog": true
        }
    ]
}
```

settings.json
```JSON
{
    "files.autoSave": "onFocusChange",
    "go.buildOnSave": true,
    "go.lintOnSave": true,
    "go.vetOnSave": true,
    "go.buildFlags": [],
    "go.lintFlags": [],
    "go.vetFlags": [],
    "go.useCodeSnippetsOnFunctionSuggest": false,
    "go.formatOnSave": false,
    "go.formatTool": "goreturns",
    "go.goroot": "D:\\Go",
    "go.gopath": "D:\\GoWorks"
}
```

---

