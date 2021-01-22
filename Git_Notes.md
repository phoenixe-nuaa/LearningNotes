# Git

## 简介

Git是一种分布式版本控制系统。

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
- `git add . `：将local改动加入暂存区
- `git commit -m` ：将暂存区的内容提交到local repo
- `git status`：查看
- `git push`：将改动提交到remote repo

### 其他命令

- `git init`：初始化一个repo
- `git mv [file-original] [file-renamed]`：修改文件名称，并放入暂存区。
- `git branch`：列出local所有branch
- `git branch -r`：列出所有remote branch
- `git branch -a`：列出所有local和remote branch
- `git branch [branch-name]`：新建一个branch，但仍留在当前分支
- `git checkout -b [branch]`：新建一个branch，并切换到该branch