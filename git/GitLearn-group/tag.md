## 标签
tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

### 新建一个标签

```bash
$ git tag <tagname>
```

命令`git tag <tagname>`用于新建一个标签，默认为HEAD，也可以指定一个commit id。

### 指定标签信息

you can use like this: `git tag -a <tagname> -m "say something..."`

```bash
$ git tag -a <tagname> -m <description> <branchname> or commit_id
```


### PGP签名标签

you can use like this: `git tag -s <tagname> -m "say something..."`

```bash
$ git tag -s <tagname> -m <description> <branchname> or commit_id
```

他有简化版本

>比方说要对`add merge`这次提交打标签，它对应的commit id是`f52c633`，敲入命令：
>
>> $ git tag v0.9 f52c633

### 查看所有标签

```bash
$ git tag
```

标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

```bash
$ git show v0.9
commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:56:54 2018 +0800

    add merge

diff --git a/readme.txt b/readme.txt
...
```

### 推送一个本地标签

```bash
$ git push origin <tagname>
```

### 推送全部未推送过的本地标签

```bash
$ git push origin --tags
```

### 删除一个本地标签

```bash
$ git tag -d <tagname>
```

### 删除一个远程标签

```bash
$ git push origin :refs/tags/<tagname>
```
