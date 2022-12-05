---
layout: titile
title: Git
date: 2018-12-05 17:32:36
tags: [Git,开发岗]
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
 <!--more--> 
## 工作区和暂存区
工作区就是你在电脑里能看到的目录
.git文件不是工作区是一个Git的版本库，版本库中存了很多东西，最重要的是stage或者index的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的指针HEAD
![jiegou](/Git/p1.png "图示")
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
放弃所有本地操作，直接合并到远程库最新的操作
git fetch --all ： 下载远程库的最新内容，不做和合并
git reset --hard origin/hexo ：重置后不保留暂存区和工作区
git pull origin hexo:hexo ：拉取
如果本地的想要保存修改 保存当前工作进度，会把暂存区和工作区的改动保存起来
git stash save "message" git stash list 
然后拉取git pull 之后弹出当前保存的进度
git stash pop
本地分支与远程库建立连接
git branch -u origin/master master
查看本地分支与远程分支对应的链接情况
git branch -vv
**git push <远程主机名> <本地分支名>:<远程分支名>
git pull <远程主机名> <远程分支名>:<本地分支名>**
将远程库的更新合并到本地库中，--rebase的作用是取消本地库中的commit，并把他们接到更新后的版本库中
git pull --rebase origin hexo

# 如何在别人的开源项目中提交自己的Pull Request ?

先在本地创建一个空文件夹,里面准备放克隆过来的代码. --> 我在本地Downloads文件夹下创建了一个名为 gitMessagekit 的文件夹.
在"终端"中通过cd命令进入到gitMessagekit文件夹下(将"自己电脑的用户名"换成你自己的电脑的用户名). --> cd /Users/自己电脑的用户名/Downloads/gitMessagekit
在"终端"输入克隆命令 git clone 开源项目源代码的url. --> git clone https://github.com/MessageKit/MessageKit.git
进入到克隆所在的文件夹. --> cd /Users/自己电脑的用户名/Downloads/gitMessagekit/MessageKit
用查看命令查看一下开源项目都有多少个分支. --> git branch -a
找到自己要切换的分支,准备切换分支,在这里我要切换到3.0.0-beta分支. --> git checkout remotes/origin/3.0.0-beta
基于远程分支新建本地分支(3.0.0-beta),2条命令. --> git branch 3.0.0-beta git checkout 3.0.0-beta
打开/Users/自己电脑的用户名/Downloads/gitMessagekit/MessageKit该路径下的代码,对代码进行修改.
添加修改. --> git add 你修改的文件
提交修改. --> git commit -m "fix 某某问题"
去自己的git仓库,准备fork一下开源项目MessageKit到自己的仓库(repository)中.
即将关联自己fork过的项目. --> git remote add upstream git@github.com:xxjldh/MessageKit.git
推送本地的分支(3.0.0-beta)到自己fork过的仓库中,2条命令. --> git fetch origin git merge origin/3.0.0-beta
在即将提交时出现这样一个错误,git@github.com: Permission denied (publickey).解决办法(https://www.jianshu.com/p/f22d02c7d943)
最后push自己的分支到自己fork过的仓库中. --> git push upstream 3.0.0-beta
在开源项目https://github.com/MessageKit/MessageKit.git的pull request中添加自己刚修改过的文件, 点"comment pull request"即可.