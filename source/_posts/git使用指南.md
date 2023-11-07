---
title: git使用指南
top: false
cover: false
author: DULULU oO
date: 2023-04-18 11:09:41
password:
summary:
tags: git
categories: 面试
---

[git命令大全](https://blog.csdn.net/bjbz_cxy/article/details/116703787?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168178388816800182174092%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168178388816800182174092&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-116703787-null-null.142^v84^control_2,239^v2^insert_chatgpt&utm_term=git%E5%91%BD%E4%BB%A4&spm=1018.2226.3001.4187)
### 配置与初始化
初次使用git需要设置你的用户名以及邮箱，这将作为当前机器git的标识，如果你用它来下载远程仓库一些需要登录权限的仓库会要求登录，git默认使用配置邮箱以及用户名登入，但会要求你手动输入密码
用户名设置
```bash
git config --global user.name "你的用户名"
```

邮箱设置
```bash
git config --global user.email "你的邮箱"
```
使用git init初始化当前仓库，可以用ls-ah查看.git的隐藏目录

### 新建文件

> add: 将文件添加到缓存区
 commit: 提交到本地仓库

git中有工作区与缓存区的概念：
**工作区**：工作区就是你当前的工作目录
**缓存区**：这里存放了你使用git add命令提交的文件描述信息，它位于.git目录下的index文件中
这些文件中存储了我们一些提交的缓存数据，git会解析它们，HEAD文件就是指向当前的仓库。最后使用git commit提交时git会提交到当前仓库中，当前的工作区也就成为了最新一次提交的仓库版本。

例如： 在当前目录下创建一个新的文件 test.txt
```linux
touch test.txt
git add test.txt
```
这个时候还不算添加到了本地仓库，我们还需要使用git commit命令为其添加修改的描述信息

注意在使用git commit时我们只需要简单描述一下我们做了什么，不要像写注释那样写一大堆，不然将来在回滚代码或者查看历史版本时，很难审阅。

我们需要使用-m命令来简写描述我们的信息，如果不使用-m，会调用终端的注释编辑器让你输入描述信息，但是不建议使用

```linux
git commit -m "add new file \"test.c\""
```

可以用git -amend改写上一次提交的信息，会进入vim
```linux
git commit --amend
```

如果要将所有的改动文件添加到缓存区:git add --all

### 删除文件
rm:删除
```linux
git rm test.txt
```

删除后恢复（仅限 git rm删除后恢复，因为git rm可以将文件先放入缓存区）
1. 重置缓存区操作
```linux
git reset
```
2. 取消操作
```linux
git checkout test.txt
```

### 查看历史提交日志、回滚代码仓库
查看日志：
git log

可以查看到历史版本

使用以下命令进行回滚
```linux
git reset --hard 要回滚的id
```

如果只要回滚当前仓库指向的版本，可以直接
```linux
git reset --hard HEAD^
```
^代表上一个版本的意思，HEAD代表当前仓库的指向，当前HEAD指向master，就代表回滚到master上一次提交的版本

```linux
git reset --hard HEAD~3
```
后面的~3，代表以当前版本为基数，回滚多少次。HEAD~3代表回滚master前三个版本'

查看单个文件的可回滚版本：
```linux
git log test.txt
```
通用可以使用git reset 回滚
### 查看提交后的文件是否有改动

```linux
git status
```

### 撤销修改并回到上一次修改的状态

> checkout:通常用于切换分支仓库

当我们在工作中修改了一个文件，猛然间发现内容好像改的不对，想重新修改，这个时又不知道自己改了什么代码，想撤销修改，有一个最简单的方法，就是git checkout -- file，注意中间要有“--”，checkout这个命令是切换分支的功能，关于它我们后面在细说，你现在只需要知道这个命令加上“--”可以用来将文件切换到最近一次的状态

注意这个恢复只能恢复到上一次提交的状态，如你刚提交了这个文件到仓库，随后你修改了它，那么使用这个命令只会回到刚刚提交后的那个状态里，不能回到你还没有提交，但修改的状态中。

这个功能不能一直迭代恢复，如果恢复到了修改前的版本，你想再次回滚回滚到修改前在之前的版本是不行的。
```linux
git checkout --test.txt
```

### 创建分支

git checkoout -b 可以创建分支
```linux
git checkout -b newbranch
```

创建了新分支newbranch
可以用git branch查看当前属于什么分支，也就是head的指向
```linux
git branch
```

git checkout -b等价于
```linux
git branch newbranch
git checkout newbranch
```
**git branch 如果后面跟着名字则会创建分支，但不会切换**
**git checkout 后面如果是分支名称则切换过去**

### 切换分支

```linux
git checkout master
```
切换回master分支

### 合并分支
git merge

例如要合并master和newbranch，可以切换回master，在master下执行合并
1. 切换
```linux
git checkout master
```
2. 合并
```linux
git merge newbranch
```

这里需要说一点，如果你在任何分支下创建文件，没有提交到仓库，那么它在所有仓库都是可见的，比如你在分支dev中创建了一个文件，没有使用git add和git commit提交，此时你切换到master，这个文件依旧存在的，因为你创建的文件在工作目录中，你切换仓库时git只会更新跟仓库有关的文件，无关的文件依然存放在工作区

### 删除分支

git删除本地分支：git branch -D

git branch -D 分支名
git删除远程分支：git push origin --delete

注意这里的远程分支名不需要加origin，输入分支名就可以了

git push origin --delete 远程分支名

### 本地仓库关联远程仓库

1. git remote add origin
例如
```linux
git remote add origin git@github.com:beiszhihao/test.git
```

2. git push推送到远程
```linux
git push -u origin master
```
push：将本地仓库与远程仓库合并

-u：将本地仓库分支与远程仓库分支一起合并，就是说将master的分支也提交上去，这样你就可以在远程仓库上看到你在本地仓库的master中创建了多少分支，不加这个参数只将当前的master与远程的合并，没有分支的历史记录，也不能切换分支

origin：远程仓库的意思，如果这个仓库是远程的那么必须使用这个选项

master：提交本地matser分支仓库

### git拉取远程仓库到本地，拉取分支，切换远程分支

git clone拉到本地
```linux
git clone git@github.com:xxx.git
```

git clone -b 分支名： 拉取知道哪个分支
```linux
git clone master git@github.com:xxx.git
```

### git提交本地的到远程git add, git commit, git push

```linux
git status
git add --all
git commit -m "add xxx"
git push origin master
```

### git fetch拉取全部分支

也可以git fetch xxx拉取指定分支