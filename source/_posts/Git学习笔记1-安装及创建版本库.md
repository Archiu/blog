---
title: Git学习笔记1-安装及创建版本库
date: 2017-08-01 15:26:15
tags: 
- Git
---
教程来自于廖雪峰的[Git教程](https://liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

## 安装
在windows下使用linux的工具，需要Cygwin这样的模拟环境。这里使用某高人打包好的模拟环境和GIt。
[国内镜像](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)
安装后打开Git-->git bash 就说明安装成功啦。

安装后需要在 git bash中进一步设置：

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

<!-- more -->

## 创建版本库
* 首先找一个合适的地方创建新目录：
```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

* 通过 `git init` 命令将这个目录变成Git可以管理的仓库：
```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```
## 将文件添加到版本库
包括Git的所有的版本控制系统，其实只能跟踪文本文件的改动。而ms的word文档是二进制格式，所以不能用git跟踪。
编码建议使用UTF-8。
**最好不要用Windows的记事本编辑文本。**建议使用notepad++。


首先用编辑器编辑如下文本：
> 
Git is a version control system.
Git is free software.

一定要放在`learngit`目录下。

* 第一步，用命令`git add`告诉Git，将文件添加到仓库：
```
$ git add readme.txt
```

* 第二步，用命令`git commit`告诉Git，将文件提交到仓库：
```
$ git commit -m "wrote a readme file"
[master (root-commit) cb926e7] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

-m后面是对此次提交的说明。

