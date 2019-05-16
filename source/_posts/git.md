---
title: git知识汇总
date: 2018/9/12 14:05:00
tags:
- git
categories: git
---
### 创建版本库
------
通过`git init`命令把当前目录变成Git可以管理的仓库。

把一个文件放到Git仓库只需要两步：
1. 通过`git add <filename.filenameExtension>`命令告诉Git，把文件添加到仓库。

   (可以使用`git add *` 将全部文件添加到仓库。)<!-- more -->

2. 通过`git commit -m "<commit content>"`命令告诉Git，把文件提交到仓库。

   (-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。)

### git基础操作
------
#### 版本回退
通过`git reflog`命令，可以输出记录你的每一次命令，如`add`、`commit`等等。

通过`git log`命令显示从最近到最远的提交日志，每条日志包括`commit id`，`Author`，`Date`和`commit content`信息。

(可以通过加上`--pretty=oneline`参数，只输出`commit id`和`commit content`，来简化输出信息。)

`commit id`是一个SHA1计算出来的一个非常大的数字，用十六进制表示。

在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写成HEAD~100。

通过`git reset --hard HEAD^`命令，可以把当前版本回退到上一个版本。

通过`git reset --hard <commit id>`命令，可以把当前版本回退到指定版本。

#### 工作区和暂存区的概念
Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

工作区(Working Directory)，就是在电脑里能看到的目录，比如我的`test`文件夹就是一个工作区。

版本库(Repository)，工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

之前我们把文件往Git版本库里添加的时候，是分两步执行的：

1. 通过`git add`命令把文件添加进去，实际上就是把文件修改添加到暂存区。

2. 通过`git commit`命令提交更改，实际上就是把暂存区的所有内容提交到当前分支。

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的(working tree clean)。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。

#### 管理修改
每次修改，如果不用`git add`到暂存区，通过`git commit -m "<commit content>"`命令提交，修改那就不会加入到`commit`中。

但是如果通过`git commit * -m "<commit content>"`命令会直接提交工作区的内容到当前分支。

#### 撤销修改
通过`git checkout -- file`命令可以把`file`文件在工作区的修改全部撤销。

这里有两种情况：
1. `file`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态。
2. `file`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。

#### 删除文件
通过`git rm file`命令删掉文件，并且通过`git commit -m ""`命令提交到版本库，之后版本库中该文件已经被删除。

通过`git checkout -- file`命令可以把误删的文件恢复到最新版本(版本库里的版本)。

### 远程仓库
------
#### 添加远程库
要关联一个远程库，使用命令
`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

#### 从远程库克隆
要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

### 分支管理
------
#### 创建与合并分支
查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

删除远程库分支：`git push origin :<name>` (分支名前的冒号代表删除)

#### 冲突解决
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。

#### 分支管理策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

#### Bug分支

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再通过`git stash pop`命令，回到工作现场。

#### Feature分支

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`命令强行删除。

#### 多人协作

##### 查看远程库

当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

要查看远程库的信息，用`git remote`，或者，用`git remote -v`显示更详细的信息。

##### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：`git push origin master`。

如果要推送其他分支，比如dev，就改成：`git push origin dev`。

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

* master分支是主分支，因此要时刻与远程同步；

* dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

* bug分支只用于在本地修复bug，就没必要推到远程了；

* feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

##### 抓取分支

多人协作时，大家都会往远程的master和dev分支上推送各自的修改。假如你的小伙伴已经向origin/dev分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送。
```
$ git push origin dev
To github.com:xxx/xxx.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:xxx/xxx.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
结果推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送。
```
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```
git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：
```
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```
再pull：
```
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```
这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push。
##### 小结
查看远程库信息，使用`git remote -v`；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

#### Rebase 变基

多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后push的童鞋不得不先pull，在本地合并，然后才能push成功。每次合并再push后，分支变的很乱。

针对上述问题，可以通过`git rebase`命令，把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

最后，通过push操作把本地分支推送到远程：`git push origin master`。

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。注意的使用情形和原则：只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作。因为rebase会改变提交历史记录，这会影响到别人使用这一远程仓库。

### 标签管理
------
#### 创建标签
命令`git tag <tagname> [<commit id>]`用于新建一个标签，默认为HEAD，也可以指定一个commit id；

命令`git tag -a <tagname> -m "blablabla..." [<commit id>]`可以指定标签信息；

命令`git tag`可以查看所有标签。标签不是按时间顺序列出，而是按字母排序的;

命令`git show <tagname>`可以查看标签信息。

`注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。`
#### 管理标签
命令`git push origin <tagname>`可以推送一个本地标签；

命令`git push origin --tags`可以推送全部未推送过的本地标签；

命令`git tag -d <tagname>`可以删除一个本地标签；

命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。