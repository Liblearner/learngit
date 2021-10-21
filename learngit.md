[TOC]

## 零.写在前面

git是一个分布式资源管理系统软件，可以方便的记录文档的改动等等。但是其只能追踪文本文档的改动，而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。Microsoft的Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的，前面我们举的例子只是为了演示，如果要真正使用版本控制系统，就要以纯文本方式编写文件。
## 一.本地创建版本库 
### 1. 创建空目录


在git bash这一交互式输入中输入以下命令，$符号是输入提示符
```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```
其中learngit是所创建的库的名字。
pwd可以查看库的路径，如果没有发现，是因为它是默认隐藏的，用==ls-ah==命令可见。
尽量不要在路径中包含中文。

### 2. 建立git对这个库的管理。

通过`git init`命令
```
$ git init
Initialized empty Git repository in C:/Users/DELL/name/.git/
```
### 3.将文件添加到版本库
tips：最好不要用记事本写。
写好之后保存在库之下
之后写入代码：

```
git add readme.md#第一步，把文件添加到仓库
git commmit -m"wrote a readme file"#-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。
```

出现了错误：
```
$ git commit -m"写了一个新文件readme"
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got 'DELL@DESKTOP-QABPPTE.(none)')

DELL@DESKTOP-QABPPTE MINGW32 ~/name (master)
$ ^C

DELL@DESKTOP-QABPPTE MINGW32 ~/name (master)

```
出现的原因：创建git文件夹的时候信息不完善所以解决方法是当出现这个上述提示后 接着补充
你在命令行中执行
git config --global user.email "你的邮箱"
git config --global user.name "你的名字"
（注意 “ 前面是有空格的）
输入完后再接着执行git commit 即可成功!
### 4. 搬运工：可能出现的错误：
Q：输入git add readme.txt，得到错误：fatal: not a git repository (or any of the parent directories)。

A：Git命令必须在Git仓库目录内执行（git init除外），在仓库目录外执行是没有意义的。

Q：输入git add readme.txt，得到错误fatal: pathspec 'readme.txt' did not match any files。

A：添加某个文件时，该文件必须在当前目录下存在，用ls或者dir命令查看当前目录的文件，看看文件是否存在，或者是否写错了文件名。
### 5.修改文本文档

打开文档之后进行修改。
运行`git status`命令查看结果
```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.md

no changes added to commit (use "git add" and/or "git commit -a")
```
该命令可以时刻让我们掌握仓库当前的状态，上面的命令输出告诉我们，readme.md被修改过了，但还没有准备提交的修改。

可以用`git diff`这个命令具体看看修改了什么内容：
```
$ git diff
diff --git a/readme.md b/readme.md
index 850cd31..7069c9f 100644
--- a/readme.md
+++ b/readme.md
@@ -1,3 +1,3 @@
-git is a version control system.
+git is a distributed version control system.

 git is a free software.
\ No newline at end of file
```
其中的-和+分别表示这删去和新加入的内容。

提交修改和提交新文件是一样的两步，全部代码见下：
```
DELL@DESKTOP-QABPPTE MINGW32 ~/name (master)
$ git add readme.md

DELL@DESKTOP-QABPPTE MINGW32 ~/name (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   readme.md


DELL@DESKTOP-QABPPTE MINGW32 ~/name (master)
$ git commit -m"add distributed"
[master ef05a3b] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)

DELL@DESKTOP-QABPPTE MINGW32 ~/name (master)
$ git status
On branch master
nothing to commit, working tree clean

DELL@DESKTOP-QABPPTE MINGW32 ~/name (master)
$
```
## 二.版本回退
类似于RPG游戏的存档与读档，Git中每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失
一个查看历史记录的命令是：`git log`
```
$ git log
commit decf7bcc703438ebb6a15ad86646070afa167f3b (HEAD -> master)
Author: name is <my name>
Date:   Tue Oct 19 20:41:01 2021 +0800

    appedn GRL

commit ef05a3bc4bf82ae2dc6a569a52b3dc2fc46cce0a
Author: name is <my name>
Date:   Tue Oct 19 20:36:31 2021 +0800

    add distributed

commit 8b5924b10f63c7405562d7e9ee7b9eff6d2874f7
Author: name is <my name>
Date:   Tue Oct 19 20:23:18 2021 +0800

    wrote a new file
```
可以把输出信息调整到一行以内：`git log --pretty=oneline`
```
$ git log --pretty=oneline
decf7bcc703438ebb6a15ad86646070afa167f3b (HEAD -> master) appedn GRL
ef05a3bc4bf82ae2dc6a569a52b3dc2fc46cce0a add distributed
8b5924b10f63c7405562d7e9ee7b9eff6d2874f7 wrote a new file
```
git中对于版本的表示：
HEAD        当前版本
HEAD^      上一个版本
HEAD^^    再上一个版本
HEAD~n    往前n个版本

版本回退的命令`git reset`
```
$ git reset --hard HEAD^
HEAD is now at ef05a3b add distributed
```

读取文本的内容`cat readme.md`
```
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```
并且修改之后曾经的新版本已经不存在记录。如果要找回，可以找到想要回去的版本号，比如：
```$ git reset --hard 8b5924
HEAD is now at 8b5924b wrote a new file
```
版本号可以在上面找到，只要提供前几位可以被git识别就可以了。
Git提供了一个命令git reflog用来记录你的每一次命令：
```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```
终于舒了口气，从输出可知，append GPL的commit id是1094adb，现在，你又可以乘坐时光机回到未来了。
如果命令行窗口被关掉了，那么你想要找回到原来的版本就一定要提前调用该命令来保存版本号。
综上所述，HEAD表示和版本号就是指向某一个版本的指针。
## 三.本地库与github远程库关联
### 1.登录并且创建github账号和远程库
第一次接触到这样的操作，包括更改安全权限，hosts等等。这也算是第一次翻墙了，干这个事情还是挺有意思的。我本来以为在修改安全权限中遇到的一些细节问题没有办法搜到，后来抱着试一试的心态竟然真的搜到了，惊喜万分，搜索引擎竟然可以细致到这个程度。
把用到的操作记录一下：
C/windows/system32/drivers/hosts   修改dns
cmd  用 idconfig/flushdns 更新域名

顺便在这里记录一下账户的名字：
Liblearner
### 2.创建新的库
这一部分不看教程也可以，库的名字需要和本地库同名，其他保持默认设置即可。github还给出了和本地库建立连接的操作。

但是遇到了一个非常严重的问题，当第二天再次登录的时候在bash中找不到之前库的有关信息了。
建立连接：

```
DELL@DESKTOP-QABPPTE MINGW32 ~/learngit (master)
$ git remote add origin git@github.com:Liblearner/learngit.git
```
再接下来可以把本地库的内容推送到远程库上。
```
$ git push -u origin master
The authenticity of host 'github.com (52.69.186.44)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 214 bytes | 214.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:Liblearner/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```
从今往后可以用命令`git push origin master`这个命令的最新修改推送至github。

![image-20211021165610446](image-20211021165610446.png)

这就是上传文件之后的结果啦。

### 3.删除库
`git remote rm origin`
此命令是解除了本地库和远程库的连接，要真正删除远程库要在github'网站上操作。
操作的合集：
```
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；

关联一个远程库时必须给远程库指定一个名字，origin是默认习惯命名；

关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
```

## 四.从github的库中提取代码

## 五.提交（push）与获取代码（pull）
### 1.从远程库克隆，从而建立本地库

建立的新库的名字是：gitskills
勾选 Initialize this repository with a README，github会自动创建一个README文件。
用命令`git clone`建立本地库
```
$ git clone git@github.com:Liblearner/gitskills.git
Cloning into 'gitskills'...
warning: You appear to have cloned an empty repository.
```
github支持多种协议的地址连接。目前使用这种就好啦，详细教程可以以后探索。
### 2.如何获取代码
这部分的内容应该就是说要从别人的库里粘贴代码下载到本地？从CSDN上找到了教程，搜索后在别人的库里找到地址之后同样使用命令`git clone`即可
```
$ git clone git@github.com:getlantern/download.git
Cloning into 'download'...
remote: Enumerating objects: 94, done.
remote: Total 94 (delta 0), reused 0 (delta 0), pack-reused 94
Receiving objects: 100% (94/94), 24.43 KiB | 201.00 KiB/s, done.
Resolving deltas: 100% (29/29), done.

```
这样就把代码拷到本地啦（虽然第一次资料是乱找的）。

由于时间原因，优先完成其他任务啦，下面的内容之后再学。

## 六.创建分支和合并分支
## 七.如何多人协作
