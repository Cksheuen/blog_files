---
title: GIT&GitHub
---
# GIT&&GitHub

标签（空格分隔）： 未分类

---
# GIT
## Git命令
### Git常用命令
|命令名称|作用|
|---|---|
|git config --global user.name 用户名|设置用户签名|
|git config --global user.email 邮箱|设置用户签名|
|git init|初始化本地库|
|git status|查看本地库状态|
|git add 文件名|添加到暂存区|
|git commit -m "日志信息" 文件名|提交到本地库|
|git reflog|查看历史记录|
|git reset --hard 版本号|版本穿梭|


## Git分支
### 分支的操作
|命令名称|作用|
|---|---|
|git branch 分支名|创建分支|
|git branch -v|查看分支
|git checkout 分支名|切换分支|
|git merge 分支名|把指定的分支合并到当前分支上|

## Idea 集成Git
在.gitconfig 文件中引用
```
[core]
    excludesfile = C:/Users/Cksheuen/git.ignore
```
---
# GitHub

## 创建远程库
```
git remote -v ——查看别名
git remote add 别名 链接 ——创建别名
```
## 代码推送 Push
```
git push 别名/链接 分支
```
## 代码拉取 Pull
```
git pull 别名/链接 分支
```
## 代码克隆 Clone
```
git clone 别名/链接
```
## SSH免密登录
```
```
## Idea集成GitHub

---
# Gitee 码云

## 码云创建远程库
## Idea集成Gitee码云
## 码云连接GitHub 进行代码的复制和迁移

---
# GitLab
## GitLab服务器的搭建和部署
## Idea集成GitLab