---
title: Git学习笔记4-分支管理
date: 2017-08-01 20:48:41
tags:
- Git
---
# 创建与合并分支

>`git branch`:查看分支
  `git branch <name>`:创建分支
  `git checkout <name>`:切换到分支
  `git checkout -b <name>`:创建并切换到分支
  `git merge <name>`:合并该分支到当前分支（此时为快速合并`Fast forward`）
  `git branch -d <name>`:删除该分支

<!-- more -->

***
# 解决冲突
当两条分支对同一个文本进行了不同的修改，并试图合并时，Git无法自动合并的，称为冲突(conflict)。
例如当前在`master`分支，试图`git merge dev`合并`dev`分支时，会返回一个冲突。打开冲突的文本可以发现出现了下面的内容：

```
<<<<<<< HEAD<HEAD中冲突的内容> 
=======<dev中冲突的内容> 
>>>>>>> dev
```

此时手动编辑文本中的内容，去掉这些标记，然后像往常一样添加提交，最后删掉不要的分支即可。

用命令`$ git log --graph --pretty=oneline --abbrev-commit`可以看到分支合并的情况。

***

# 分支管理策略
## 禁用`Fast forward`模式
通常在合并分支时，如果可能，Git会使用`Fast forward`模式，但在这种模式下删除分之后，会丢失分支信息。
如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

在合并时使用`--no-ff`参数禁用`Fast forward`模式：

`$ git merge --no-ff -m "merge with no-ff" dev` 

由于在merge时生产了一个新的commit，所以在这里引用了 `-m`参数。
合并后用`git log`查看分支，发现分支信息保存了下来：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   6138341 merge with no-ff
|\
| * 5582ddb add merge
|/
*   189265d conflict fixed
```

让我们删除dev分支后再创建dev分支，然后用`Fast forward `模式合并一次试一试。
合并后查看分支历史，可以发现看不到分支信息：

```
$ git log --graph --pretty=oneline --abbrev-commit
* 8bda56b test ff
*   6138341 merge with no-ff
```

## 分支策略
实际开发中团队合作的分支管理：
![Markdown](http://i1.ciimg.com/1949/c8ac77d9155c40f5.png)

***

# bug分支
在Git中，每个bug都可以通过一个临时分支来修复，修复后合并分支，再删除临时分支。

* “储存”当前分支的工作现场：` git stash`命令可以保存工作现场，等以后恢复现场后再工作。

```
 $ git stash 
 Saved working directory and index state WIP on dev: 6224937 add merge 
 HEAD is now at 6224937 add merge
```

* 确定在哪个分支修复bug，如`master`分支，就从该分支创建临时分支，修复bug后提交并合并，然后删除临时分支。

* 回到工作的分支，可以用`git stash list`查看存储的工作现场。 

```
 $ git stash list 
 stash@{0}: WIP on dev: 6224937 add merge
```

此时想恢复工作现场有两种方法：
①用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除
②用`git stash pop`，恢复的同时把stash内容也删了

***
# Feature分支
开发一个新feature，最好新建一个分支。
如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

***

# 多人协作
> 
查看远程库的信息：`git remote`或者`git remote -v`(后者更详细)
推送分支：`git push origin master`
推送其他分支：`git push origin <name>`

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？
* `master`分支是主分支，因此要时刻与远程同步。
* `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步。
* bug分支只用于在本地修复bug，就没必要推到远程了。
* feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

