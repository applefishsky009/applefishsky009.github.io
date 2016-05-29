---
title: Git命令
date: 2016-04-23 14:27:10
category: Git
tags: Git

---

初学git，记录当时的一些解决方法。

1. [删除不想要的git历史](https://www.zhihu.com/question/22132675)`git rebase -i SHA`如果之后有版本，会有conflict，根据提示`git rebase --abort`会放弃修改，`git rebase --skip`会跳过这个(放弃冲突的提交,也就是说之后的提交都没了)`git rebase --continue`应该会保留之后的提交,暂时没有进行测试。
2. [删除版本库中的提交](https://segmentfault.com/q/1010000000115900)注意，他将HEAD指向某一个commit，之后的commit和文件都被擦除了，删除前做好备份……
3. 博客中常用的命令：hexo clean(删除一些没有用的缓存，比如删掉的tag等);hexo generate(生成一个commit?);hexo deploy(提交到远程)
4. 使用过程中碰到[Warning: Permanently added 'github.com,192.30.252.120' (RSA) to the list of known hosts](http://stackoverflow.com/questions/9299651/git-says-warning-permanently-added-to-the-list-of-known-hosts).这个问题在linux下很好解决,但我用windows for github，困扰好久，方法如下：在C:/user/###(你的用户名)/.ssh/新建config文件(无后缀),添加UserKnownHostsFile ~/.ssh/known_hosts,下一次访问你还会看到，但是之后(可能要多几次，我的三次才可以，因为在这个文件夹下添加了三个ip)就没有了。
5. windows下换行符[warning: LF will be replaced by CRLF](http://www.luckyonecn.com/blog/git-auto-crlf-problem/)，不知道为什么我的`git config --global autocrlf false`没有用，因此直接在仓库中将config文件，core中修改autocrlf = false,没有则添加。