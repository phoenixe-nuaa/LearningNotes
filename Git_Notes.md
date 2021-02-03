[toc]

# [Git笔记](https://juejin.cn/post/6844904199189184525)

## 简介

Git是一种分布式版本控制系统。可以有效地进行版本控制和并行开发。

![img](https://user-gold-cdn.xitu.io/2020/3/26/171169bf8360cf09?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Git GUI是图形化界面。

Git CMD是和windows风格类似的命令窗口。

Git Bash是和Linux环境相似的命令窗口。

## 配置

### 设置用户名和邮箱

全局设置，以后使用的每一次提交包含以下信息。

```
git config --global user.name "xxx"
git config --global user.email "xxx@xx.com"
```

如果不加`--global`则表示为特定项目配置的信息。

可以通过以下命令查看设置：

```
git config --global --list
```

## 常用Git命令

### 日常使用

- `git clone`：复制remote repo到本地
- `git add`：将local改动加入暂存区
- `git commit` ：将暂存区的内容提交到local repo
- `git push`：将改动提交到remote repo

### 其他命令

- `git init`：初始化一个repo
- `git mv [file-original] [file-renamed]`：修改文件名称，并放入暂存区。
- `git branch`：列出local所有branch
- `git branch -r`：列出所有remote branch
- `git branch -a`：列出所有local和remote branch
- `git branch [branch-name]`：新建一个branch，但仍留在当前分支
- `git checkout -b [branch]`：新建一个branch，并切换到该branch

## 工作区、缓存区、版本库小结

- 工作区 --- 文件目录因为你的开发工作对文件进行修改所以叫做工作区
- 缓存区 --- 没有提交前也就是没有完成完成工作结果代码保存的地方
- 版本库 --- 每一个工作结果的时间带会被按照不同的提交记录保存起来

![img](https://user-gold-cdn.xitu.io/2020/6/25/172ea48acc9e10d9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

# 本地管理

## 创建本地库

创建库的过程其实就是将一个普通的文件夹升级为一个具备版本控制能力的文件夹：

```
# 创建文件夹
mkdir hello-git

# 改变工作目录到hello-git
cd hello-git

# 初始化Git相当将文件夹升级为仓库
git init

# 查看文件tree
tree -a
```

## 添加文件到版本库

### 添加文件

添加完成后发现这个时候文件状态变为`changes to be committed`，用`git status`查看：

```
# 添加文件追踪
git add READE.md

# 查看local repo状态
git status
```

### 添加多个文件追踪

```
# 添加多个文件追踪
git add .
```

### 取消追踪

```
# 取消追踪
git rm --cached README.md
```

### 忽略文件.gitignore

如果希望在使用 add .的时候忽略某一个文件，可以在目录下创建一个 .gitignore文件。

## 提交代码commit

当已经完成了某一阶段的代码开发，比如完成了某一个代码功能(features)或者修改了某一个代码缺陷(fix)，就可以创建一次代码提交。

```
git commit -m 'XXX'
```

## 保存临时工作成果stash

- `git stash`：保存临时工作成果
- `git stash pop`：读取

## 放弃修改

- `git restore .`：放弃修改

## 回退到上一次提交

我们可以回退到上一次提交记录：

```
# 参数-a 是先添加到缓冲区再提交的意思
git commit -am 'XXX 01'

# 检查提交记录
git log
git log –-oneline # 简短日志
git reflog # 操作记录 包括回退记录也会被显示

# 只是版本回退 不更新工作区
git reset HEAD^

# 不但版本回退 也会更新工作区（文件目录）的文件到上一个版本
git reset --hard HEAD^
```

# 分支管理

管理并行开发的利器。

## 创建分支

默认情况下我们处于master分支，我们首先要做的就是切分一个新的分支，比如开发一个叫做FunA的功能点：

```
# 分支A
# 创建分支并切换分支到funA
git checkout -b 'funA'

# 完成功能
echo 'FunA XXXXXXX' >> README.md

# 提交功能
git commit -am 'funA add'

# 切换master分支
git checkout master

# 合并将开发分支合并到主分支
git merge funA

# 可以利用-d合并的同时删除分支
git merge -d funA
```

## 分支的查看、删除

```
# 查看
git branch 

# 查看 - a 包括远程分支
git branch -a

# 删除
git branch -D <分支名称>
```

## 冲突解决

假设两个分支都针对同一行代码进行修改，就会造成冲突，需要人工确定那一个分支得到保留。

```
# 撤销merge
git merge --abort
# 查看merge过程
git log --graph --pretty=oneline --abbrev-commit
```

## 清洗提交历史squash

最终提交的时候想要隐藏细节，可以采用这样的办法压缩多次commit，只用完一次完成：

```
# 切换master分支
# 使用squash方式提交 只合并不commit
git merge --squash funA
# 创建分支并切换分支到funA
git commit -am 'funA update'
```

## rebase

# 标签管理

标签管理就是给自己的代码打上版本标记。

```
# 将最新提交打标签
git tag v1.0

# 将指定commit打标签
git tag v0.9 4ab025

# 查看打标签
git tag 

# 查看与某标签之间的差距
git show v0.9
```

# 远程仓库GitHub

## 添加远程分支

## 查看

```
git remote -v
```

## push

```
git push
```

## fetch

```
git fetch
```

## pull

```
# == fetch + merge
git pull
```

