---
title: Git 基础总结
categories:
  - Git
tags:
  - Git
date: 2019-03-31 20:04:20
author: 郭俊东
---

### 菜鸟都知道 Git？

Git 是一个分布式版本管理系统，修改一次代码并提交，就会产生一个版本，这是 Git 最基本的功能。

Git 还是多人开发时的重要版本管理工具，成员写好模块只需要推送简单的进行提交并 push 一下，其他成员就可以通过服务器获取到最新的代码。

### git 的特点

* 方便多人协同开发
* 分布式，每一个副本都保留完成的仓库信息
* 速度快，除提取和推送外基本都是本地操作
* 方便版本控制

### git 构成

工作区：就是你在电脑里能看到的目录。
暂存区：英文叫stage, 或index。一般存放在 ".git目录下" 下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）
版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。


### git 本地仓库操作

* git init //将所在文件夹初始化为 Git 仓库
* git status //查看目前本地状态
* git add .  //添加了所有修改过的文件到暂存区（代码颜色会红色变绿色）
* git add path/to/file //添加指定文件到暂存区
* git commit -m "说明" //将暂存区的文件添加到本地仓库。
* git config user.name "哈哈哈" //设置本仓库的用户名
* git config user.email "xgo_great.guo@xgo.one" //设置本仓库的 email
* git commit -am "版本描述" //添加所有文件到暂存区并提交
* git reflog //查看所有操作记录

### git 版本回退

* git log //查看提交日志
* git reset --hard 版本号 //回退到指定版本号
* git reset --hard HEAD^ //回退到上一个版本（注意！！！如果 HEAD 为合并产生的提交，要留意第一父提交和第二父提交问题，详见 https://imciel.com/2016/09/09/git-parent/）

### git 版本对比

* git diff HEAD -- login.py //对比版本库与工作区中某文件的修改
* git diff HEAD HEAD^ -- login.py //对比两个版本间某个文件修改


### git 远程仓库 pull 和 push    

* 首先需要在远程仓库创建一个项目
* git clone 仓库地址 // 克隆远程服务器的代码
* git push origin branch // 推送本地仓库的代码到远程服务器的 branch 分支
* git pull origin //拉取远程 branch 分支的代码（建议在修改代码前都要拉取一下，保证自己的代码是最新的）

### git 代码冲突

容易冲突的操作方式：

* 多个人同时操作了同一个文件
* 一个人一直写不提交
* 修改之前不更新最新代码
* 擅自修改同事代码

减少冲突的操作方式

* 先 pull 在修改，修改完立即 commit 和 push
* 确保自己正在修改的文件是最新版本的
* 各自开发各自的模块

如何解决

* 保留需要的代码，其他与代码无关的符号全部删除。
* 与开发人员沟通，及时解决。

### git 分支

* git branch //查看当前的分支
* git checkout -b dev //创建并且切换到分支dev
* git push -u origin dev //将本地分支推送到远程
* git checkout master //切换到 master



