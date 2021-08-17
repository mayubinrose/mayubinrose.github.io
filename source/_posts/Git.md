---
layout: titile
title: Git
date: 2018-12-05 17:32:36
tags: Git
categories: Git
---
# 什么是Git
目前世界上最先进的分布式版本控制系统
# 创建版本库
版本库就是一个仓库，里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪
将当前目录变为仓库：git init
将文件添加到暂存区：git add 文件名 [可选：另一个文件名]
将暂存区提交到仓库：git commit –m "描述"
 <!--more--> 
# 时光机穿梭
掌握仓库当前的状态：git status  查看某个具体文件的改变：git diff wenjian.txt
## 版本回退
git log:显示最近到最远的提交日志(回到过去)
git中HEAD表示当前版本，上一个版本是HEAD^
git reset --hard HEAD^：回退到上一个版本
git reset --hard 1094a^：回到某一个版本
git reflog：记录每一次的命令(回到未来)
## 工作区和暂存区
工作区就是你在电脑里能看到的目录
.git文件不是工作区是一个Git的版本库，版本库中存了很多东西，最重要的是stage或者index的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的指针HEAD
![jiegou](/Git/p1.png "图示")
## 撤销修改
git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
&emsp;&emsp;一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。总之，就是让这个文件回到最近一次git commit或git add时的状态。
git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区
git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。
## 删除文件
git rm：用于删除一个文件(暂存区)
# 远程仓库
ssh-keygen -t rsa -C "1084930331@qq.com"：申请密钥
## 添加远程库
git remote add origin git@github.com:michaelliao/learngit.git添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。
git push -u origin master把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
之后就用git push origin master推送最新的修改
一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：
## 从远程库克隆
git clone git@github.com:michaelliao/gitskills.git
# 分支管理
## 创建和合并分支
![jiegou](/Git/p2.png)
git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：git branch dev  git checkout dev
git branch命令查看当前分支，一般回到master分支上然后进行merge合并
git merge命令用于合并指定分支到当前分支。
git branch -d dev：删除分支
## 解决冲突
解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
用git log --graph命令可以看到分支合并图。
## 分支管理策略
在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：
一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
另一种方式是用git stash pop，恢复的同时把stash内容也删了：
## Feature分支
开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除
## 多人协作
当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin
git remote：查看远程库的信息加上-v显示更详细的信息
git push origin master：把master推送给远程库origin
git pull把最新的提交从origin/dev抓下来
# 标签管理
## 创建标签
先切换到要打标签的分支上，然后git tag <name>就可以了，默认打在最新提交的commit上
git tag <name> f52c633：对具体的id打标签
git tag：查看标签
git show <tag_name>：显示标签信息
## 操作标签
删除标签：git tag -d <tagname>
删除远程标签：git push origin :refs/tags/<tagname>
推送某个标签到远程：git push origin <tagname>