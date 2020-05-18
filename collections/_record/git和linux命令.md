---
layout: post
title: git和linux命令
date: 2020-04-28
---
## tree
https://zhuanlan.zhihu.com/p/90726468

## Git
https://mp.weixin.qq.com/s/WsmF5DrMQAoXKBhx4GoK1A
https://learngitbranching.js.org/?locale=zh_CN
* `git clone git@github.com:nohosts/nohost.git` 克隆远程仓库的内容到本地
* `git pull origin master` 获取远程分支master并merge到当前分支
* `git branch -a`查看全部分支（远程+本地）
* `git checkout -b bugFix`新建bugFix，并切换到到此分支。（如果分支已存在则去掉-b即可）
* `git status` 查看当前~~~~版本状态（是否修改）
* `git add .` 增加当前子目录~~~~下所有文件更改至暂存区
* `git commit -m 'xxx'` 提交暂存区的修改至本地的版本库, 修改备注为xxx
* `git push` 将本地版本推送到远程分支
* `git tag v1.0 dfb02e6e4f2f7b573337763e5c0013802e392818` 增加v1.0的tag到某个提交上
* `git merge testBranch` 合并testBranch分支至当前分支`
* `git stash` 暂存本地的当前修改，将本地代码重置为HEAD状态。（如果需要取出修改，命令后加一个pop即可）
* `git log` 显示提交日志（如果想每个提交信息显示在一行，可以加上--pretty=oneline）
* `git show dfb02e6e4f2f7b573337763e5c0013802e392818`显示某个提交的详细内容
* `git reset --hard HEAD` 将当前版本重置为HEAD

注意这两个命令的区别：
```
git pull = fetch + merge
git pull --rebase = fetch + rebase
 ```

## linux
https://www.runoob.com/linux/linux-system-contents.html
https://www.liaoxuefeng.com/wiki/896043488029600/896827951938304