---
title: Git学习笔记2-时光机穿梭
date: 2017-08-01 15:52:01
tags:
- Git
---


* `git status`可以掌控仓库的状态
* `git diff`可以常看具体文件的修改
***
## 版本回退
* `git reset`在不同版本之间穿梭（`--hard`参数 + HEAD/commit id（部分即可））
* `git log`查看版本历史 ，以回到过去版本（`--pretty=oneline`显示单行）
* `git reflog`查看命令历史，以回到未来版本

<!-- more -->
### 查看历史记录
* `git log`命令显示最近到最远的提交日志

```
$ git log
commit 3628164fb26d48395383f8f31179f24e0882e1e0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Tue Aug 20 15:11:49 2013 +0800

    append GPL

commit ea34578d5496d7dd233c827ed32a8cd576c5ee85
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Tue Aug 20 14:53:12 2013 +0800

    add distributed
```

* 加上`--pretty=oneline`可以只显示单行

```
$ git log --pretty=oneline
3628164fb26d48395383f8f31179f24e0882e1e0 append GPL
ea34578d5496d7dd233c827ed32a8cd576c5ee85 add distributed
```
### 版本回退
在Git中，`HEAD`表示当前版本，`HEAD^`表示上个版本，`HEAD^^`表示上上个版本，`HEAD~100`表示往上100个版本。

* `git reset`命令可以回到之前的版本

```
$ git reset --hard HEAD^
HEAD is now at ea34578 add distributed
```
执行该命令回到过去后，再次用`git log`查看会发现最新的版本已经消失了。想回到最新的版本的话，如果命令窗口没有关掉，可以往上找到`append GPL`的commit id`3628164...`。
```
$ git reset --hard 3628164
HEAD is now at 3628164 append GPL
```
这样就回到了最新版本，commit id 不用全部输入。

* `git refog`记录了你的每一次的命令。
如果回到过去版本后，关掉了命令行窗口，想要回到新版本，可以如下找到对应的commit id：

```
$ git reflog
ea34578 HEAD@{0}: reset: moving to HEAD^
3628164 HEAD@{1}: commit: append GPL
ea34578 HEAD@{2}: commit: add distributed
```

***
## 工作区和暂存区
**工作区（Working Direction）**

即电脑中能看到的目录，如前面所创建的`learngit`文件夹就是一个工作区。

**版本库（repository）**
工作区中有一个隐藏的目录`.git`，这个不属于工作区，而是Git的版本库。
Git的版本库中有一个叫stage的暂存区，还有Git自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

当我们将文件添加到版本库，实际上是：
1. `git add` -->将文件修改添加到暂存区
2. `git commit`-->将暂存区的所有内容提交到当前分支

用图片理解一下：
![Markdown](http://i2.tiimg.com/1949/a8eb431a4e90ee7d.png)

 我们修改readme.txt，并增加一个LICENSE文本文件。然后使用两次`git add`添加这两个文件的修改。
 暂存区就变成这样了。 
![Markdown](http://i2.tiimg.com/1949/98776774c16a7ace.png)

 然后执行`git commit`将暂存区所有修改提交到分支。
![Markdown](http://i2.tiimg.com/1949/b821e3f33ba5f60f.png)

 
***
## 管理修改
Git跟踪管理的是修改，而非文件。
可以进行如下实验:
第一次修改文本-->`git add`-->第二次修改文本-->`git commit`
可以发现只有第一次的修改被提交了。这是因为只有第一次的修改被`git add`放入暂存区，所以`git commit`只会提交第一次的修改。

***
## 撤销修改
* 场景一：改乱了工作区某个内容，想直接丢弃工作区的修改时，使用命令`git checkout -- file`。
* 场景二：改乱了工作区的某个内容并添加到了暂存区，想丢弃修改，分两步，先用命令`git reset HEAD file`回到场景一，第二步按场景一操作。
* 场景三：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

***
## 删除文件
在工作区删除某个文件后，工作区和版本库就不一致了，有两个选择：
* 彻底从版本库删除文件：①`git rm file` ②`git commit`
* 从版本库中恢复文件：`git checkout -- file`

`git checkout`其实是用版本库的版本替换工作区的版本。