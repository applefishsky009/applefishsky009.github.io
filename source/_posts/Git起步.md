---
title: Git简明指南
date: 2016-06-07 12:47:05
category: Git
tags: Git

---

## Git本地版本库

本地版本库推送到远程版本库需要经过这样的过程：
本地文件->缓存池->HEAD->remote

### git init
将本地文件夹初始化为版本库，使用下列指令创建并进入文件夹(简单的window操作。。。)
```
mkdir folder
cd folder
```

### git add file <-> git rm file
将本地文件加入到缓存池中<->从缓存池删除。**不论文件是新建的还是旧的，只要你想commit，就需要add进缓存池**
1. `git rm file`,将缓存池中有，本地文件夹没有，的文件从缓存池删除。
2. `git rm -f file`,**缓存池**中有，本地文件夹也有，这个指令可以一并删除。
3. 注意如果还没有add到缓存池,直接`rm file`就行。

### git commit -m "summary"
将缓存池的改动提交到HEAD中,push到远程分支之后,summay就是文件名和时间中间现实的东西，一般新建的仓库中的read.md的summary都是Initial commit。

### git commit --ammend
修改最近一次commit的summary。

### git rebase -i ID
变基(将基置于ID)提交，可用于重写提交历史，或者合并提交。输入后会弹出文本文档。
1. 更改文本文档中提交历史的顺序，可以重写提交历史。使用这一条指令和`git commit --ammend`可以修改某一次commit的summary。
2. pick改为squash会将这次提交合并到前一次提交。
3. 修改完成之后`git rebase --continue`

### git status
显示你的本地文件夹，缓存池，HEAD中不一致的地方。

### git log
查看版本库的提交历史，按q退出。
1. `git log -n`显示版本库提交的历史，默认参数n=all。
2. `git log --stat -n`显示版本库历史的文件变更统计。
3. `git log -g`我主要用他在切换分支时查看两个版本之间的提交。

### cat file
文件列表中的文件或标准输入转标准输出，简单来讲，在屏幕上打印文件内容。
1. 常用`cat .git/config`查看配置文件

### git diff
1. `git diff`显示本地版本库修改前后的变化。
2. `gir diff --cached`显示缓存池add前后的变化。
3. `git diff ID1 ID2`显示两次commit的变化(ID2-ID1)，如果需要查看最新一次提交的变更，ID2是最新一次的ID，ID1是之前的ID。

### git show
1. `git show ID`显示某次commit的详细信息。
2. `git show-branch --more=10`显示当前开发分支的开发summary。

### git branch
1. `git branch`列出本地分支;
2. `git branch -r`列出远程分支;
3. `git branch -a`列出本地分支和远程分支;
4. `git branch name1`创建名字为`name1`的本地分支(不进行分支切换);
5. `git branch -d name1`删除分支，详情自行查询。

### git mv file1 file2
重命名一个文件夹(更改到缓存池)，这是下列指令的组合：
```
mv file1 file2
git rm file1
git add file2
```

### git clone
clone一个版本库
1. `clone folder1 folder2`，将folder1版本库clone并重命名为folder2；
2. `clone url`clone在url上的远程版本库到本地。

### git config
1. `git config paramname val`修改本仓库的配置
2. `git config --global paraname val`修改全局配置(我的.config文件在C：/users/username/.gitconfig)

---


## Git远程版本库

### git remote add origin url
这为你的git本地仓库创建远程连接。
1. origin是约定的远端库的名字(当然可以用其他的);url在github上创建的仓库可以clone到。
2. 可以使用cat .git/config来查看。

### git push
1. `git push origin master`，将HEAD提交到远端库origin的master分支；
2. `git push -u origin master`，指定默认远端库和分支。执行之后，以后的提交都可以简写为`git push`。
3. `git push -f`将本地内容强行提交(force)到远端(本地有冲突),比如说修改commit的summary。

### git pull
git pull = git fetch + git merge
1. `git fetch`将远端库的HEAD,fetch到本地；
2. `git merge`将本地与HEAD合并，有冲突时需要先解决冲突，注意如果本地有远端库没有的文件，需要手动删除。

### git reset --hard HEAD~3/ID
这一指令可以快速的版本切换(用git log查询ID，变更HEAD的头指针)
1. 变更之后使用`git log -g`查看，可以来回切换。

