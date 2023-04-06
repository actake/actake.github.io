---
title: git必知必会-基础知识
date: 2021-03-08 22:23:24
tags: [git]
---

## Git 是什么
一个强大的版本控制软件
### [官方文档](https://git-scm.com/book/zh/v2)
### [交互式学习站点-learngitbranching](https://learngitbranching.js.org/)

## 基础概念
- 四种状态：
  * 未跟踪：还没有参与版本控制的状态
  * 已修改：表示修改了文件，但还没保存到数据库中
  * 已暂存：表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中
  * 已提交：表示数据已经安全地保存在本地数据库中
  ![](四种状态.png)
- 四个区域
  * Git 仓库：是 Git 用来保存项目的元数据和对象数据库的地方
  * 工作目录：是对项目的某个版本独立提取出来的内容
  * 暂存区域：保存了下次将要提交的文件列表信息
  * 远程仓库

<!-- more -->

## 工作流程
![](工作流程.png)

## 常用命令
### git clone
Q：clone 失败或者是速度慢
A：可以尝试使用
```cmd
git config --global http.postBuffer 524288000
```

### git fetch
通常是先**获取**再拉取，比较稳，好像没有碰到过什么问题

### git pull
Q：拉取失败
A：通常是本地文件冲突导致，在拉取前 commit 或者 stash 一下（尽量操作前都这么做）

### git add
提交到暂存区

### git commit
将暂存区的改动提交到本地的版本库
**tips: amend 很好用**
```cmd
git commit --amend
```

### git push
将本地修改推送到远程仓库
**tips: 尽量不用 --force 参数**

### git checkout
检出，用于切换分支或者提交。

### git branch
分支操作，创建、查看等

### git merge
分支合并。
> 扩展：分支合并策略，[点我查看文档](https://www.jianshu.com/p/58a166f24c81)

### git rebase
变基，用的不多，[点我查看文档](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA)

### git reset
重置|回退|撤销，有三种模式：
```cmd
git reset --soft & :: 软合并，会保存所有本地改动
git reset --mixed & ::  混合合并，保持工作副本并重置索引(即取消暂存)，默认使用该模式
git reset --hard & :: 强行合并，丢弃所有改动的工作副本
```

### git stash
储藏，挺经常用的
```cmd
git stash
git stash list & :: 查看 stash 列表
git stash pop [--index] & :: 将最新的 stash 恢复到工作区，恢复后会删除当前 stash
git stash apply [--index] & :: 和 pop 类似，区别是恢复后不会删除当前 stash
```

### git reflog
查看操作记录，基本是误操作后使用
比如，在进行一个 amend 操作后，发现提交信息错误，想回退。这时候，就可以使用 git reflog 查看 amend 操作前的哈希，然后使用 git reset 进行回退了

### git cherry-pick
遴选，用于将指定的提交应用于其他分支
```cmd
git cherry-pick <commitHash> & :: 可以有多个 hash，越往左越早
```

## Git 配置
### 配置文本编辑器
配置用于编辑提交以及标签信息的编辑器，默认使用 vim
```cmd
git config --global core.editor "code --wait"
git config --global core.editor "vim" & :: 重置为默认编辑器
```
### 配置大小写敏感
主要在 push 时可能会碰到这个问题，开启大小写敏感
```cmd
git config core.ignorecase false
```

### 配置别名
可以通过 git 的别名配置来简化命令的输入，有两种方式
- 通过命令行设置：
```cmd
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```
- 将下面的内容复制到：用户文件夹/.gitconfig
```
[alias]
  # cr = code review
  cr = !sh -c 'git push origin HEAD:refs/for/$1' -
  ci = commit
  ciamend = commit --amend -m ''
  patch = !sh -c 'git ciamend && git cr $1' -
  st = status
  br = branch
  co = checkout
```


### hooks
总的来说，git 的钩子分为三类：
- 提交工作流钩子
- 电子邮件工作流钩子
- 其它钩子

比较常用的应该是**提交工作流钩子**，如```pre-commit```，通常会结合[husky](https://github.com/typicode/husky)进行配置。

## 图形化客户端
GIt 的 客户端很多，tortoiseGit、 sourceTree、 vscode 的 git graph 插件等等，选一款适合自己的就好了，个人推荐 ```sourceTree```
