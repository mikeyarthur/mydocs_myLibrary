# git_stash
> 暂时保存本地修改到堆栈上，修复问题后恢复本地修改

[TOC]

## 1. [git stash详解](https://blog.csdn.net/stone_yw/article/details/80795669)

## 2. 将所有未提交的修改（工作区和暂存区）保存至堆栈中，用于后续恢复当前工作目录
```shell
# 1. 能够将所有未提交的修改（工作区和暂存区）保存至堆栈中，用于后续恢复当前工作目录
git status
git stash

git status
# git stash 操作过后，分支就clean了，文件/文件修改内容都放到堆栈，也就是除了git之外没有其他方法知道修改内容
# On branch master
# nothing to commit, working tree clean

# 2. 等同于git stash，区别是可以加一些注释
git stash save "test1"
# 修改内容放到堆栈标签为 "test1"
# stash@{0}: On master: test1

```

## 3. 查看当前stash中的内容列表
```shell
# 如果有标签，请注意标签指向
git stash list

```

## 4. 查看堆栈中最新保存的stash和当前目录的差异
```shell
# 1. 目录的差异
git stash show

# 2. 内容的差异
git stash show -p
```

## 5. 把堆栈上的修改恢复到工作区
```shell
# 1. 将当前stash中的内容弹出，并应用到当前分支对应的工作目录上。
# 该命令将堆栈中最近保存的内容删除（栈是先进后出）
# 顺序执行git stash save “test1”和git stash save “test2”命令
git stash list
git stash pop
git stash list
# 此时注意到stash堆栈标签已经pop掉 "test2"

# 2. 适用于多个分支的情况
# 不同于git stash pop，该命令不会将内容从堆栈中删除，也就说该命令能够将堆栈的内容多次应用到工作目录中
git stash list
git stash apply
git stash list
# 此时仍然保留stash堆栈标签 "test1"
```

## 6. 删除
```shell
# 1. 删除指定标签
# 从堆栈中移除某个指定的stash
git stash list
git stash drop + "标签"
git stash list

# 2. 清除所有标签
# 清除堆栈中的所有 内容
git stash list
git stash clear
git stash list
```
