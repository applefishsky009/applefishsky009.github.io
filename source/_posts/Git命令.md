---
title: Git命令
date: 2016-04-23 14:27:10
category: Git
tags: Git

---

初学git，记录当时的一些解决方法。

1. [删除不想要的git历史](https://www.zhihu.com/question/22132675)`git rebase -i SHA`如果之后有版本，会有conflict，根据提示`git rebase --abort`会放弃修改，`git rebase --skip`会跳过这个(放弃冲突的提交,也就是说之后的提交都没了)`git rebase --continue`应该会保留之后的提交,暂时没有进行测试。
2. [删除版本库中的提交](https://segmentfault.com/q/1010000000115900)注意，他将HEAD指向某一个commit，之后的commit和文件都被擦除了，删除前做好备份……
3. 博客中常用的命令：hexo clean(删除一些没有用的缓存，比如删掉的tag等);hexo generate(生成一个commit?);hexo deploy(提交到远程)；合并hexo generate和hexo deploy：hexo d -g；
	+ hexo s -p 5000；可以改变server运行的端口号。
4. 使用过程中碰到[Warning: Permanently added 'github.com,192.30.252.120' (RSA) to the list of known hosts](http://stackoverflow.com/questions/9299651/git-says-warning-permanently-added-to-the-list-of-known-hosts).这个问题在linux下很好解决,但我用windows for github，困扰好久，方法如下：在C:/user/###(你的用户名)/.ssh/新建config文件(无后缀),添加UserKnownHostsFile ~/.ssh/known_hosts,下一次访问你还会看到，但是之后(可能要多几次，我的三次才可以，因为在这个文件夹下添加了三个ip)就没有了。
5. windows下换行符[warning: LF will be replaced by CRLF](http://www.luckyonecn.com/blog/git-auto-crlf-problem/)，不知道为什么我的`git config --global autocrlf false`没有用，因此直接在仓库中将config文件，core中修改autocrlf = false,没有则添加。
6. 从零配置网上教程到处都是，不必多说，主要解释换电脑或者重装系统的配置：
	+ 安装Nodejs(这是一个框架)
	+ 安装hexo -> npm install hexo-cli -g(这是hexo指令)
	+ 将保存的hexo目录(博客文件夹)拷贝过来，并使用git for windows add仓库(否则使用命令行配置key)
	+ 然后进入Hexo目录重新配置hexo模块 npm install(局部hexo指令)
	+ 最后建议在GUI同步一下(否则本地和remote的commit记录不同步)
	+ [参考资料](http://stackoverflow.com/questions/9023672/nodejs-how-to-resolve-cannot-find-module-error)
	+ 为hexo安装math指令：npm install hexo-math --save,(也有可能并不需要,npm以及全部安装了)
	+ 注意如果使用行内数学公式，该文章必须至少使用一行行间公式，不然行内公式会失效(如果不需要，请在文章最后添加`$$$$`)。

---

## 命令行新建repository的步骤

1. `mkdir repositoryName` -> `cd repositoryName`;
2. `git init`，同时在远程新建仓库得到链接;
3. `touch .gitignore` -> `vim .gitignore`并设置ignore文件
	+ [vim简易使用指南](http://blog.csdn.net/jackalfly/article/details/7546878)
	+ 将 .gitignore 文件和 .gitattributes文件 另存，每次新建仓库拷贝即可。
	+ [Git之忽略文件(ignore file)](http://blog.csdn.net/benkaoya/article/details/7932370)
4. 标准commit：`git add .gitignore` -> `git commit -m ".gitignore"`;
	`-m`即message，界面中的summary。
5. 远程链接`git remote add origin <URL>`;
	+ `origin`指定你`push`到哪个remote,如果远程没有，参考[Git 的origin和master分析 ](http://blog.csdn.net/abo8888882006/article/details/12375091)
6. 设置默认上传流`git push -u origin master`;
	+ 默认推送到远程`master`分支，如果没有则新建；
7. 设置完毕，以后的文件上传使用三个指令就可以完成：
	+ `git add fileName`；
	+ `git commit -m "summary"`;
	+ `git push`；
	+ 结合 .gitignore 文件设置上传规则。
	+ 可以`cd folder`单独上传文件设置不同的message，但是github上文件的message是最近commit的message。