# overview

[TOC]

## 1. 增
### 1.1. 仓库级别操作
#### 1. 初始化仓库
```shell
git init
```

#### 2. 从远程仓库获取到本地
```shell
# 1. 只有git地址
git clone git地址

# 2. 已经git clone，只想更新远程仓库内容
git fetch
```

#### 3. 从本地推送到远程仓库
```shell
# 1. 已提交暂存区，直接推送到远程仓库
git push

# 2. 未提交暂存区
git add readme.md
git commit -m "准备推送到远程"
git push 
```

### 1.2. 分支级别操作
#### 1. 新建分支
```shell
# 1. 仅新建分支
git branch -a
git branch new-branch
git branch -a

# 2. 新建并切换到新建的分支
git branch -a
git checkout -b new-branch
# 等价于 git branch new-branch && git checkout new-branch
git branch -a

```


### 1.3. 文件级别操作
#### 1. 文件增加到本地仓库暂存区
```shell
# https://blog.csdn.net/lian740930980/article/details/124697430
# 通过git ls-files,  git status,  git cat-file 查看objects 新增到本地仓库的文件信息
git ls-files
git add readme.md
git ls-files

```

#### 2. 文件增加到远程仓库
```shell
# 查看信息1，origin为远程仓库的别名
git remote -v
origin  https://github.com/xxx.git (fetch)
origin  https://github.com/xxx.git (push)

# 查看信息2，\*表示当前使用的分支
git branch -a
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main

# git push <远程主机名> <本地分支名>:<远程分支名>
# 如果本地分支名与远程分支名相同，则可以省略冒号
# git push <远程主机名> <本地分支名>

# push方法1，git地址
git push https://github.com/xxx.git main
# push方法2，别名
git push origin main
# push方法3，直接push
git push

```

## 2. 删
### 2.1. 仓库级别操作
#### 1. 删库
```shell
# 1. 删除本地仓库
# 2. 删除远程仓库
```

### 2.2. 分支级别操作
#### 1. 删除分支

### 2.3. 文件夹级别操作
#### 1. 从暂存区中删除文件夹
#### 2. 从暂存区中删除文件夹，并删除文件夹

### 2.4. 文件级别操作
#### 1. 从暂存区中删除文件
#### 2. 从暂存区中删除文件，并删除文件

## 3. 改
### 3.1. 仓库级别操作
#### 1. 配置
```shell
git config --list --show-origin
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
# 如果未配置，Git 会使用操作系统默认的文本编辑器
# 如果你想使用不同的文本编辑器，例如 Emacs
git config --global core.editor emacs
```

#### 2. 提交
```shell
git add readme.md
git commit -m "从暂存区提交到本地仓库"
```

#### 3. 回滚

### 3.2. 分支级别操作
#### 1. 切换分支
见 [[git_overview#1 新建分支]]

### 3.3. 文件级别操作
#### 1. 合并

#### 2. 回滚

#### 3. 放弃本地修改
```shell
# https://www.cnblogs.com/xiaoxi-jinchen/p/16008522.html
# 1. 未使用 git add 缓存代码
# 1.1. 不要忘记中间的 “--” ，不写就成了检出分支了
git checkout -- readme.md

# 1.2. 放弃所有的文件修改
git checkout .

# 2. 已经使用了 git add 缓存了代码
# 此命令用来清除 git  对于文件修改的缓存。相当于撤销 git add 命令所在的工作。在使用本命令后，本地的修改并不会消失。而是回到了 1. 未使用 git add 缓存代码 的状态
# 2.1. 放弃指定文件的缓存
git reset HEAD readme.md

# 2.2. 放弃所有的缓存
git reset HEAD .

# 2.3. 部分缓存(待验证)
git restore readme.md


# 3. 已经用 git commit  提交了代码
# 3.1. 回退到上一次commit的状态
git reset --hard HEAD^

# 3.2. 回退到任意版本
# git log 命令来查看git的提交历史。git log 的输出如下,之一这里可以看到第一行就是 commitid
git reset --hard  commitid

```


## 4. 查
### 4.1. 仓库级别操作
#### 1. 配置
```shell
# 1. 查看所有的配置以及它们所在的文件
git config --list --show-origin

# 2. 查看范围system/global/local
git config --list --show-scope

```

#### 2. 提交信息

#### 3. 分支信息

### 4.2. 分支级别操作
#### 1. 查分支列表

#### 2. 查分支信息

### 4.3. 文件级别操作
#### 1. 提交信息

#### 2. diff


## 5. 进阶操作

### 5.1. 配置
#### 5.1. [[git_gitignore]]
### 5.2. [[git_stash]] 暂时保存本地修改到堆栈上，修复问题后恢复本地修改
