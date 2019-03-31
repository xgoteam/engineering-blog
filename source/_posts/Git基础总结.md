---
title: Git基础总结
categories:
  - Git
tags:
  - Git
date: 2019-03-31 20:04:20
---

## 菜鸟都知道 GIT ?

git 可以理解为是一个备份器，修改一次代码，就做个备份，这是git 最基本的功能。

git 还是多人开发时的重要版本管理工具，成员写好模块只需要推送简单的push一下 ,其他成员就可以通过服务器获取到推送。

## git 的特点

    方便多人协同开发
    方便版本控制

## git 构成
    工作区：添加，删除，修改代码的操作
    本地仓库：暂存区，存储修改的记录，查看状态
             仓库区，储存更改记录，查看历史记录
    远程仓库: 提交本地代码到远程服务器
             拉取远程代码到本地

## git 本地仓库操作
    git init - 在本地创建一个仓库，这个仓库可以添加东西。
    git status - 查看目前本地状态
    git add .  - 添加了所有修改过的文件到暂存区（代码颜色会红色变绿色）
    git commit -m "说明" - 将暂存区的文件添加到本地仓库。
    git config user.name "哈哈哈" 
    git config user.email "xgo_great.guo@xgo.one"
    git commit -am "版本描述" - 添加和提交操作一期完成
    git reflog - 能查看所有记录（包括删除记录）

## git 版本回退
    git log - 查看提交日志
    git reset --hard 版本号 回退到指定版本号
    git reset --hard HEAD^ 回退到上一个版本

## git 版本对比
    git diff HEAD -- login.py - 对比版本库与工作区
    git diff HEAD HEAD^ -- login.py - 对比版本库


## git 远程仓库pull和push    
    首先需要在远程仓库创建一个项目
    git clone https 克隆远程服务器的代码
    git push 推送本地仓库的代码到远程服务器
    git pull 拉取代码（建议在每次开发中都要拉取一下，保证自己的代码是最新的）

## git 代码冲突
容易冲突的操作方式：
* 多个人同时操作了同一个文件
* 一个人一直写不提交
* 修改之前不更新最新代码
* 提交之前不更新最新代码
* 擅自修改同事代码

减少冲突的操作方式
* 先pull在修改,修改完立即commit和push
* 确保自己正在修改的文件是最新版本的
* 各自开发各自的模块

如何解决
* 保留需要的代码，其他与代码无关的符号全部删除。
* 与开发人员沟通，及时解决。

## git 分支

    git branch - 查看当前的分支
    git checkout -b dev - 创建并且切换到分支dev
    git push -u origin dev - 将本地分支推送到远程
    git checkout master - 切换到master



