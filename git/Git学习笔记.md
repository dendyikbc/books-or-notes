##　Git学习笔记

>  dave

[git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)

> **Git是目前世界上最先进的分布式版本控制系统(廖雪峰)**
>
> 2002年，BitMover公司出于人道主义精神，授权Linux社区免费使用BitKeeper这个版本控制系统
>
> 2005年，开发Samba的Andrew试图破解BitKeeper的协议（这么干的其实也不只他一个），被BitMover公司发现了，于是BitMover公司怒了，要收回Linux社区的免费使用权
>
> > 于是，Linus花了两周时间自己用C写了一个分布式版本控制系统，这就是Git！
> >
> > 一个月之内，Linux系统的源码已经由Git管理了！
>
> 2008年，GitHub网站上线了，它为开源项目免费提供Git存储，无数开源项目开始迁移至GitHub，包括jQuery，PHP，Ruby等等
>
> 如果不是当年BitMover公司威胁Linux社区，可能现在我们就没有免费而超级好用的Git了。
>
> 以前有个软件也叫GIT（GNU Interactive Tools），结果Git就只能叫`git-core`了。由于Git名气实在太大，后来就把GNU Interactive Tools改成`gnuit`，`git-core`正式改为`git`。

### 1.集中式vs分布式

> **集中式版本控制系统，版本库是集中存放在中央服务器的**，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。
>
> > **必须联网**
> >
> > 免费的SVN修正了CVS的一些稳定性问题，是目前用得最多的集中式版本库控制系统

![central-repo](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384860735706fd4c70aa2ce24b45a8ade85109b0222b000/0)



> 分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。
>
> > **安全性要高很多**
>
> > 实际使用分布式版本控制系统的时候，其实很少在两人之间的电脑上推送版本库的修改，因为可能你们俩不在一个局域网内，两台电脑互相访问不了，也可能今天你的同事病了，他的电脑压根没有开机。因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

![distributed-repo](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013848607465969378d7e6d5e6452d8161cf472f835523000/0)

### 2.创建版本库

>版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

### 3.把文件添加到版本库

> **所有的版本控制系统，其实只能跟踪文本文件的改动**，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而**图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化**，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

> 不幸的是，Microsoft的Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的，前面我们举的例子只是为了演示，如果要真正使用版本控制系统，就要以纯文本方式编写文件。
>
> 因为文本是有编码的，比如中文有常用的GBK编码，日文有Shift_JIS编码，如果没有历史遗留问题，强烈建议使用标准的UTF-8编码，所有语言使用同一种编码，既没有冲突，又被所有平台所支持。

> Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。

言归正传，现在我们编写一个`readme.txt`文件，内容如下：

```
Git is a version control system.
Git is free software.
```

一定要放到`learngit`目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。

和把大象放到冰箱需要3步相比，把一个文件放到Git仓库只需要两步。

第一步，用命令`git add`告诉Git，把文件添加到仓库：

```
$ git add readme.txt
```

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

嫌麻烦不想输入`-m "xxx"`行不行？确实有办法可以这么干，但是强烈不建议你这么干，因为输入说明对自己对别人阅读都很重要。

> #### git commit 不输入 -m 以及后面的内容会进入vim模式
>
> ##### vi & vim 有两种工作模式：
>
> - （1） 命令模式：接受、执行 vi & vim 操作命令的模式，打开文件后的默认模式；
>
> - （2） 编辑模式：对打开的文件内容进行 增、删、改 操作的模式； 在编辑模式下按下 ESC 键，回退到命令模式。
>
>   ##### 创建、打开文件：$ vi [filename]
>
> - （1）使用 vi 加 文件路径（或文件名）的模式打开文件，如果文件存在则打开现有文件，如果文件不存在则新建文件，并在终端最下面一行显示打开的是一个新文件。
>
> - （2）键盘输入字母 “i”或“Insert”键进入最常用的插入编辑模式。
>
>   ##### 3、保存文件：
>
> - （1）在插入编辑模式下编辑文件。
>
> - （2）按下 “ESC” 键，退出编辑模式，切换到命令模式。
>
> - （3）在命令模式下键入"ZZ"或者":wq"保存修改并且退出 vi

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

### 4.时光机穿梭

`git status`

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。

`git diff`

```
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

`git diff`顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个`distributed`单词。

#### 版本回退

`git log`查看版本提交日志

```
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

`git log`命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是`append GPL`，上一次是`add distributed`，最早的一次是`wrote a readme file`。

`git log --pretty=oneline`简洁一点的查看

```
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

需要友情提示的是，你看到的一大串类似`1094adb...`的是`commit id`（版本号），和SVN不一样，Git的`commit id`不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且你看到的`commit id`和我的肯定不一样，以你自己的为准。为什么`commit id`需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。

每提交一个新版本，实际上Git就会把它们自动串成一条时间线。

> **Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。**

`git reset`回退版本命令

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

>回退以后，相当于回到过去，那如何回到未来某版本
>
>只要上面的命令行窗口还没有被关掉（有第一次 'git log' 的输出内容），你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：
>
>```
>$ git reset --hard 1094a
>HEAD is now at 83b0afe append GPL
>```
>
>版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向回退版本，然后顺便把工作区的文件更新了。

>回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？
>
>在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。

Git提供了一个命令`git reflog`用来记录你的每一次命令：

```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

#### 工作区和暂存区

>工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。
>
>Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。



![git-repo](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

> 上图很有可能有错
>
> 需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
>
> git diff 比较的是工作区文件与暂存区文件的区别（上次git add 的内容）
> git diff --cached 比较的是暂存区的文件与仓库分支（版本库）里（上次git commit 后的内容）的区别
>
> git diff --staged
>
> `git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本（最后一次commit）的区别

[Git：关于git diff 是和谁对比的问题](https://blog.csdn.net/JackLiu16/article/details/80463004)

#### 撤销修改——工作区与暂存区

>`git checkout -- readme.txt`
>
>把`readme.txt`文件在**工作区**的修改全部撤销，这里有两种情况：
>
>一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
>
>一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
>
>总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。
>
>命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：
>
>`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。
>
>`git reset HEAD <file>`
>
>可以把**暂存区**的修改撤销掉（unstage），重新放回工作区：
>
>1. 已修改工作区的内容，但还没有add到暂存区 git checkout -- readme.txt 回到历史版本
>2. 已add到暂存区，但还没有commit git reset HEAD readme.txt 回到1
>3. 已add到暂存区，并做修改，但还没有commit git checkout -- readme.txt 回到2

#### 删除文件

从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

### 5.远程仓库

本地Git仓库和GitHub仓库之间的传输是通过SSH加密的

后面参考github GitLearn-master