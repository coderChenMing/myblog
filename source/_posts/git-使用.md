---
title: git 使用
date: 2019-03-09 20:09:46
updated: 2019-03-09 21:11:44
tags: git 
categories: version_control
---
   ## 管理分支
    1、查看分支
        $ git branch
        * master
        *标识的是你当前所在的分支。
    2.查看远程分支
        git branch -r
    3、查看所有分支
        git branch -a
   ## 本地创建新的分支
    git branch [branch name]

   ## 切换到新的分支
    git checkout [branch name]
   ## 创建+切换分支
    git checkout -b [branch name]
    相当于以下两步操作：
    git branch [branch name]
    git checkout [branch name]
   ## 将新分支推送到github
    git push origin [branch name]
   ## 删除本地分支
    git branch -d [branch name]
   ## 删除github远程分支
    git push origin :[branch name]
    分支名前的冒号代表删除。
