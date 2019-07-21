---

layout:     post
title:     Git基础
subtitle:   
date:       2019-07-14
author:     ctrlcoder
header-img: 
catalog: true
tags:
    - 开发工具
typora-root-url: ..
---

## 配置user信息

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

#### config的3个作用域：

- git config --local 只对某个仓库有效

- git config --global 对当前用户所有仓库有效

- git config --system 对系统所有登录的用户有效

##### 显示config的配置，加--list

- git config --list --local

- git config --list --global 

- git config --list --system 

  

## Git仓库

#### 1. 把已有的项目代码纳入Git管理

```shell
$ cd 目标 文件夹
$ git init
```

#### 2. 新建的项目直接用Git管理

```shell
$ git init your_project
$ cd your_project
```

#### 3.添加远程仓库

```shell
$ git remote add origin  https://github.com/ctrlcoder/git_test.git #orgin是别名
```

- git remote 查看远程库信息

  - 应该可以看到 origin的结果(clone的默认名)

  - 你也可以指定选项 -v，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL
  - git remote show [remote-name] ：查看某一个远程仓库的更多信息
  -  git remote rename  [remote-name]：去修改一个远程仓库的简写名
  - git remote rm  [remote-name] ：移除一个远程仓库
  - git ls-remote  来显式地获得远程引用的完整列表

#### 4.从远程库仓库克隆

```shell
$ git clone https://github.com/ctrlcoder/git_test.git
```

> GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。

![1563106270471](/img/assets_2019/1563106270471.png)

#### 5.基本操作

##### fetch

git fetch 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 

##### pull

 运行 git pull 通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

##### push

git push [remote-name] [branch name]

> 注意： 当你和其他人在同一时 间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。 你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。

## 基本操作

#### git status 

>  Git 有三种状态，你的文件可能处 于其中之一：已提交（committed）、已修改（modified）和已暂存（staged）。
>
> 请记住，你工作目录下的每一个文件都不外乎这两种状态：已跟踪或未跟踪。 已跟踪的文件是指那些被纳入了 版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已 放入暂存区。 工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记 录中，也没有放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。 

- Changes not staged for commit

- Changes to be committed
- nothing to commit, working tree clean

#### diff  

- git diff：比较工作区与暂存区

- git diff  --cached（ --staged）：比较暂存区与最新本地版本库

#### add添加到暂存区

- git add 文件名
- git add .

![1563084618981](/img/assets_2019/1563084618981.png)

#### git commit

- 给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤

- git commit -m'message'

  **类别(范围): 描述**，如chore(typo): fix comment typo

- 类别
  - feat：新功能（feature）
  - fix：修补bug
  - docs：文档（documentation）
  - style： 格式（不影响代码运行的变动）
  - refactor：重构（即不是新增功能，也不是修改bug的代码变动）
  - test：增加测试
  - chore：构建过程或辅助工具的变动

  

#### git log

- --oneline

- --all
- --graph
- --pretty=oneline

#### git rm

删除，相当于

```shell
$ mv README.md

$ git add README.md
```



#### git mv

重命名，相当于

```shell
$ mv README.md README 

$ git rm README.md 

$ git add README 
```



#### 撤销操作

- 你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作

```shell
$ git commit -m 'initial commit' 
$ git add forgotten_file 
$ git commit --amend 

#最终你只会有一个提交 - 第二次提交将代替第一次提交的结果
```

- 取消暂存的文件

```shell
$ git reset HEAD 文件名
```

- 撤销对文件的修改

  ```shell
  $ git checkout -- CONTRIBUTING.md文件名
  ```

  

## 分支管理

#### git branch  查看当前分支

- -a 查看所有分支

- -vv 要查看设置的所有跟踪分支

#### git checkout -b 分支名：创建分支并切换

相当于

```shell
$ git branch 分支名 #创建分支
$ git checkout 分支名 #切换分支
```

#### 合并分支

如，把dev分支的工作成果合并到master

```shell
$ git checkout master

$ git merge dev # 合并某分支到当前分支
```

#### 删除分支

```shell
$ git branch -d 分支名
```

#### 删除远程分支

```shell
$ git push origin --delete 分支名
```

#### 新建远程分支

> 如果你还没有 git 仓库，可以在 GitHub 等代码托管平台上创建一个空（不要自动生成 README.md）的 repository，然后将代码 push 到远端仓库。

```shell
$ git checkout -b 分支名 

$ git push orgin 分支名
```

#### 本地分支和远程分支

```shell
$ git checkout -b 分支名 origin/分支名 #远程分支存在，本地创建并关联

$ git push -u origin 分支名 #远程分支不存在，本地push上去并关联

$ git branch --set-upstream-to=origin/分支名 #远程分支必须存在，本地与远程关联 
```

## 解决冲突

### 变基