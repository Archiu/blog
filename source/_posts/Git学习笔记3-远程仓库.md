---
title: Git学习笔记3-远程仓库
date: 2017-08-01 16:00:57
tags:
- Git
---
由于本地Git和Github仓库之间的传输是通过SSH加密的，所以需要一点设置。
** 1. 创建SSH key： **
在用户主目录下找到.ssh目录，以及看看目录中有没有`id_rsa`,`id_rsa.pub`这两个文件，如果有，进入下一步。
如果没有，打开Git Bash，执行：
`$ ssh-keygen -t rsa -C "youremail@example.com"`
一路回车，如果成功了话可以在ssh目录中找到`id_rsa`和`id_rsa.pub`。其中 `id_rsa`是私钥，不要轻易泄露,`id_rsa.pub`是公钥，可以公开给别人。

<!-- more -->

** 2. 登陆Github，打开Setting-->SSH key： **
点击New SSH key
![Markdown](http://i2.tiimg.com/1949/ebcc34235381f54b.jpg)

Title随意填，Key填入刚才生成的`id_rsa.pub` 。
（在Windows环境中可以`clip < ~/.ssh/id_rsa.pub`复制到剪贴板）
这样就完成了基本设置。
 
***

## 添加远程库
* `git remote add origin git@github.com:YourGithubName/YourRepositoryName.git `
将本地库和远程仓库关联起来。

* `git push -u origin master`
将本地库的内容推送到远程仓库，实际是把当前`master`分支推送到远程。
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

* **SSH警告**
第一次`clone`或者`push`命令链接Github时，会出现一个警告：

```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)
```

这个警告只会出现一次，以后就不会出现了。

***

## 从远程库clone##
新建一个远程库，新建时勾选下面的选项：
![Markdown](http://i2.tiimg.com/1949/24f45789e349e233.jpg)


 接着用命令
`$ git clone git@github.com:YourGithubName/NewRepositiryName.git`
就可以将远程库克隆到本地了。
