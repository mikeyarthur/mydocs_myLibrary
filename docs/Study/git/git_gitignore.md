# git_gitignore

[TOC]

## 1. 基本配置
### 1.1. 参考[# Git中使用.gitignore忽略文件的推送](https://blog.csdn.net/lk142500/article/details/82869018)

### 1.2. 配置
```shell
# 添加要忽略的文件或目录，每行表示一个忽略规则。
echo .idea/ >> .gitignore
notepad++/vim 打开 .gitignore 修改

cat .gitignore
target/
*.iml
.idea/

```

### 1.3. 配置生效
```shell
# 1. untracked状态的文件被自动 git add 忽略
git config core.excludesfile .gitignore

# 2. 检查配置生效情况

cat .git/config
# 或者
git config --list --show-origin

# 3. 检查忽略文件生效情况
git status
git ls-files
```

### 1.4. 忽略规则
在 .gitignore 文件中，每一行的忽略规则的语法如下：

1. 空格不匹配任意文件，可作为分隔符，可用反斜杠转义
2. # 开头的文件标识注释，可以使用反斜杠进行转义
3. ! 开头的模式标识否定，该文件将会再次被包含，如果排除了该文件的父级目录，则使用 ! 也不会再次被包含。可以使用反斜杠进行转义
4. / 结束的模式只匹配文件夹以及在该文件夹路径下的内容，但是不匹配该文件
5. / 开始的模式匹配项目跟目录
6. 如果一个模式不包含斜杠，则它匹配相对于当前 .gitignore 文件路径的内容，如果该模式不在 .gitignore 文件中，则相对于项目根目录
7. ** 匹配多级目录，可在开始，中间，结束
8. ? 通用匹配单个字符
9. [] 通用匹配单个字符列表


## 2. 关于换行符
### 2.1. 参考[Git忽略文件和操作系统尾部换行符问题](https://blog.csdn.net/moamao_jishuyuan/article/details/118994511)
### 2.2. 操作
```shell
git config --global core.autocrlf false
# false 的传参意味着不做任何动作
1) true:             x -> LF -> CRLF
2) input:            x -> LF -> LF
3) false:            x -> x -> x

```


