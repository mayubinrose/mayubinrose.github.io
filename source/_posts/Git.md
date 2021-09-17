---
layout: titile
title: Git
date: 2018-12-05 17:32:36
tags: Git
top: 98
categories: 开发岗
---
# 什么是Git
目前世界上最先进的分布式版本控制系统
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
 <!--more--> 
# 实际操作
## 分支操作
创建分支
git branch dev
切换分支
git checkout dev
创建并切换
git checkout -b dev
分支上修改完全后
添加到暂存区
git add .
合并到当前分支
git commit -m ""
先切换到master
git checkout master
将dev分支的工作成果合并到master上
git merge dev
删除分支
git branch -d dev
## 将本地代码远程连接到git上
本地新建一个空文件夹
git init
然后将代码复制进来
git add .
git commit -m "fisrt"
将本地仓库和远程连接
git remote add origin git@...
为了实现push操作需要生成key
ssh-keygen -t rsa -C "邮箱号" -f  ~/.ssh/newname
将生成的各个私钥文件添加到引擎中
ssh-add ~/.ssh/newname
如果报错
ssh-agent bash
在~/.ssh目录下找到config没有的话 touch config
Host git@github.com:mayubinrose/Graduation.git
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/graduation-rsa
Host git@github.com:mayubinrose/mayubinrose.github.io.git
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
最后在仓库的setting处设置key添加即可
如果push出错
git pull --rebase origin master
然后push即可
# 本地修改拉取远程仓库代码
--hard 参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交
git reset --hard
然后拉取
git pull origin hexo:hexo 
如果本地的想要保存修改 保存当前工作进度，会把暂存区和工作区的改动保存起来
git stash save "message" git stash list 
然后拉取git pull 之后弹出当前保存的进度
git stash pop
本地分支与远程库建立连接
git branch -u origin/master master