---
path: /gitusage
date: 2020-04-25T10:16:20.693Z
title: Git使用教程
description: 结合实际工作的需求，介绍 Git 的基本使用
---
# Git简单使用

- [Git简单使用](#git%e7%ae%80%e5%8d%95%e4%bd%bf%e7%94%a8)
  - [背景](#%e8%83%8c%e6%99%af)
  - [初次使用](#%e5%88%9d%e6%ac%a1%e4%bd%bf%e7%94%a8)
    - [安装](#%e5%ae%89%e8%a3%85)
    - [配置](#%e9%85%8d%e7%bd%ae)
  - [日常操作](#%e6%97%a5%e5%b8%b8%e6%93%8d%e4%bd%9c)
    - [创建仓库](#%e5%88%9b%e5%bb%ba%e4%bb%93%e5%ba%93)
    - [从远程库拉取最新代码](#%e4%bb%8e%e8%bf%9c%e7%a8%8b%e5%ba%93%e6%8b%89%e5%8f%96%e6%9c%80%e6%96%b0%e4%bb%a3%e7%a0%81)
    - [提交代码](#%e6%8f%90%e4%ba%a4%e4%bb%a3%e7%a0%81)
    - [撤回、回退](#%e6%92%a4%e5%9b%9e%e5%9b%9e%e9%80%80)
    - [分支操作](#%e5%88%86%e6%94%af%e6%93%8d%e4%bd%9c)
    - [切换分支](#%e5%88%87%e6%8d%a2%e5%88%86%e6%94%af)
  - [VSCode工具](#vscode%e5%b7%a5%e5%85%b7)
    - [自带的源代码管理](#%e8%87%aa%e5%b8%a6%e7%9a%84%e6%ba%90%e4%bb%a3%e7%a0%81%e7%ae%a1%e7%90%86)
    - [GitLens](#gitlens)

## 背景

+ 本次分享将：结合实际工作的需求，介绍 Git 的基本使用
+ 本次分享不会：介绍 Git 的高级用法，深入到 Git 的每个细节和内部机制
+ 注意：本次分享中 Git 的用法，仅仅能代表我目前对这个工具的认识，这和 Git 的最佳实践可能有所出入，请勿将其当做标准

## 初次使用

+ 说明：本小节虽然面向 Git 的初次使用者，但是**并不代表**本节内容对有一定 Git 使用经验的开发者**没有意义**。

### 安装

### 配置

#### 配置用户信息

+ 全局配置

```
设置全局配置
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
查看全局配置
$ git config --global --list
```

+ 项目配置

```
设置项目配置
$ git config user.name "Your Name"
$ git config user.email "email@example.com"
查看当前配置及配置文件路径
$ git config --list --show-origin
```

#### 配置用户凭证

##### 用账号密码进行认证

+ 正常情况下，第一次`clone`项目，控制台会要求输入用户名、密码
+ Tips：公司 GitLab 使用LDAP模式，用户名密码即`itcode`和`password`

+ 异常情况下（比如密码被修改），会出现远程服务器的用户凭证和本地系统保存的凭证不一致的情况，如：
    ```
    $ git clone https://newgitlab.digitalchina.com/internet-of-things-division/sino-trans-pudong-web-mock.git
    $ Cloning into 'sino-trans-pudong-web-mock'...
    $ remote: HTTP Basic: Access denied
    $ fatal: Authentication failed for 'https://newgitlab.digitalchina.com/internet-of-things-division/sino-trans-pudong-web-mock.git/'
    ```
    这种情况有两种解决方式：
    + 治标：重置凭证存储模式为默认值，每一次连接都询问用户名和密码
        ```
        $ git config --system --unset credential.helper
        ```
    + 治本：重置后将凭证存储模式改为 store 模式，只需输入一次用户名和密码即可将凭证存储在本地磁盘，凭证存储路径默认为 ~/.my-credentials
        ```
        $ git config --system --unset credential.helper
        $ git config --global credential.helper 'store --file ~/.my-credentials'
        ```

##### 用 SSH Key 进行认证

1. 生成 SSH Key

    ```
    进入 Git Bash
    $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    生成的 SSH Key 会被保存到 ~/.ssh/id_rsa.pub
    ```
2. 拷贝 SSH Key 的内容
3. 进入 Github 用户设置，创建 SSH key
4. 在 Title 输入框为 SSH Key 取一个名字，在 Key 输入框粘贴内容
5. 用 SSH Key 的方式`git clone`一下试试

## 日常操作

### 创建仓库

#### 从本地推送

```
首先，在 Github 上创建一个空的仓库，然后
$ git init
$ git add .
$ git commit -m "initial commit"
$ git remote add origin https://github.com/[your-username]/[your-repo-name].git
将两边master分支联系起来
$ git push -u origin master
```

#### 从远程库克隆

```
首先，在 Github 上创建一个仓库，然后
HTTPS 方式
$ git clone https://github.com/[your-username]/[your-repo-name].git
SSH Key 方式
$ git clone git@github.com:[your-username]/[your-repo-name].git
```

### 从远程库拉取最新代码

#### 配置远程库

```
添加
$ git remote add [originname] [repo]
查看
$ git remote -v
```

#### 拉取代码

```
拉取远程库指定分支到当前分支并合并
$ git pull origin [branchname]
拉取指定分支到当前分支
$ git fetch origin [branchname]
```

### 提交代码

#### 提交对当前分支的修改（不需要合并）

```
$ git add .
$ git commit -am "xxx"
在 push 前要先 pull
$ git pull origin [curent-branchname]
$ git push origin [curent-branchname]
```

#### 提交修改到dev/master分支（需要合并）

```
在项目根目录
$ git add .
$ git commit -am "xxx"
$ git pull origin [target-branchname]
如果出现
$ Auto-merging readme.txt
$ CONFLICT (content): Merge conflict in readme.txt
$ Automatic merge failed; fix conflicts and then commit the result.
需要到指定文件出手动解决冲突，上例为：readme.txt
解决冲突后再次提交
$ git add readme.txt
$ git commit -m "conflict fixed"
$ git push origin [curent-branchname]
```

#### add 和 commit 区别

+ add：在做功能的过程中存档，方便回退
+ commit：功能做完了，提交修改
+ 最明显区别：`add`没有版本号，而`commit`有

假设：  
前端老大要求：每个页面一个分支，每个PR只能新增一个commit  
做页面的流程：1. 创建页面 2. 写一个表格  
前端程序猿做一个页面的过程：  
1. 创建页面
2. `git add .`
3. 写表格
4. 正准备`add`，发现表格组件用错了，又不想手动撤回修改  
5. `git checkout .`
6. 重写表格
7. `git add .`
8. `git commit -am "页面写完了"`
9. `git pull origin dev`
10. `git push origin xxx`
11. 提PR，实现只新增一个commit

### 撤回、回退

#### 撤回工作区的修改

```
将工作区的内容回退到上一次 add 后的状态
$ git checkout .
```

#### 撤回暂存区的修改

```
撤回指定文件的修改到工作区
$ git reset [filename]
撤回当前目录下的修改到工作区
$ git reset .
```

#### 版本回退

```
查看版本记录
$ git log

查看版本记录（一个提交一行）
$ git log --oneline

查看版本记录（画图，常用于查看分支合并记录）
$ git log --graph

查看版本记录（只看最近10次日志）
$ git log -10

回退到HEAD的第一个父提交，修改保留到工作区
$ git reset HEAD^

回退到HEAD的第一个父提交，修改会从磁盘上撤销
$ git reset --hard HEAD^

将回退的版本与远程仓库同步
$ git push origin [branchname] --force
```

### 分支操作

#### 创建、查看、删除

```
查看
$ git branch --list

创建
$ git checkout -b [branchname]

删除
$ git branch -d [branchname]

强制删除（有未合并到远程的提交时使用）
$ git branch -D [branchname]
$ git branch -d --force [branchname]
```

### 切换分支

```
切换前要保证工作区是干净的
$ git checkout [branchname]
```

#### 合并分支

##### 发一个PR

1. 把目标分支`pull`到当前分支
2. 处理冲突
3. `add`、`commit`、`push`
4. 进入 Github ，点击 Compare & pull request 
5. 确定要合并的分支并填写注释
6. Create pull request

## VSCode工具

### 自带的源代码管理

+ 以项目为单位管理工作空间
+ 以文件为单位进行提交、撤回
+ 查看文件未提交内容的 diff

### GitLens

+ 以文件为单位比较版本 diff
