## 分支

> **Git鼓励大量使用分支**

### 常用分支命令

```bash
查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>
```

### 分支策略

> 创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

在实际开发中，我们应该按照几个基本原则进行分支管理：

1. master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活

2. 在dev分支上进行开发，也就是说，dev分支是不稳定的，到版本发布时，再把dev分支合并到master上，在master分支发布版本

3. 你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

4. 如果有中间版本，比如测试版，预发布版，按照优先级和流程，从dev递归合并到master上。

   ![git-br-policy](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

5. 合并分支时，加上`--no-ff`参数就可以用普通模式合并，表示禁用`Fast forward`，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

   > -m "merge with no-ff"  合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去

   ```
   $ git merge --no-ff -m "merge with no-ff" dev
   Merge made by the 'recursive' strategy.
    readme.txt | 1 +
    1 file changed, 1 insertion(+)
   ```

### 创建分支

```bash
$ git branch <branchname>
创建dev分支，然后切换到dev分支：

$ git checkout -b dev
Switched to a new branch 'dev'
git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：

$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

### 查看分支

all branch

```bash
$ git branch
```

### 切换分支

```bash
$ git checkout <branchname>
```

### 创建+切换分支
```bash
$ git checkout -b <branchname>
```

### 合并某分支到当前分支

```bash
$ git merge <branchname>
用带参数的git log也可以看到分支的合并情况：
$ git log --graph --pretty=oneline --abbrev-commit
```

### 删除分支

```bash
$ git branch -d <branchname>
```

### 强制删除

```bash
$ git branch -D <branchname>
```

### 查看分支合并图

**当Git无法自动合并分支时，就必须首先解决冲突。** 解决冲突后，才可以继续操作。

```bash
$ git log --graph
```

### 查看分支树(简版)

为什么要敲这么长的命令呢？因为树很长，而简版一眼可以看穿架构

```bash
git log --graph --pretty=oneline --abbrev-commit
```

### 普通模式合并分支:禁用n`o-ff`

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。  

合并分支时，加上`--no-ff`参数就可以用普通模式合并，能看出来曾经做过合并，包含作者和时间戳等信息，而fast forward合并就看不出来曾经做过合并。

```bash
$ git merge --no-ff -m "description" <branchname>
```

> **在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支**

### 保存工作现场

```bash
$ git stash
```

### 查看工作现场列表

```bash
$ git stash list
```

### 恢复工作现场

但是恢复后，stash内容并不删除，你需要手动删除

```bash
git stash apply
```

### 删除工作现场

```bash
git stash drop
```

### 恢复后自动删除工作现场

```bash
git stash pop
```

### 恢复指定的工作现场

```bash
git stash apply <stash version>
```

### 丢弃一个没有合并过的分支

```bash
$ git branch -D <branchname>
```

### 查看远程库信息

```bash
$ git remote -v//详细地信息
$ git remote 
```

### 在本地创建和远程分支对应的分支

建议本地和远程分支的名称保持一致

```bash
$ git checkout -b branch-name origin/branch-name，
```


### 建立本地分支和远程分支的关联

```bash
$ git branch --set-upstream branch-name origin/branch-name；



设置为跟踪来自origin 的远程分支branch-name。
使用 --track 或 --set-upstream-to 分支 origin/branch-name  

$ git branch --track branch-name origin/branch-name;
```

### 从本地推送分支

如果推送失败，先用`git pull`抓取远程的新提交；

```bash
$ git push origin branch-name
```


### 从远程抓取分支

如果有冲突，要先处理冲突。

```bash
$ git pull
```



##### 把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

`git rebase`

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。