## 远程仓库

### 创建SSH Key

第1步：在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```bash
$ ssh-keygen -t rsa -C "youremail@example.com"

Generating public/private rsa key pair.  # 生成密钥对 
Enter file in which to save the key (/root/.ssh/id_rsa):  # 保存路径
Enter passphrase (empty for no passphrase):   # 密码，默认空
Enter same passphrase again:   # 重复密码
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
92:41:73:6d:ba:03:bf:36:f8:ab:a2:90:0c:9c:a1:85 youremail@example.com
The key's randomart image is:
+--[ RSA 2048]----+
|      o ..       |
| .   . o  o      |
|E..   .  o       |
|o.o   .o.        |
|oo    ooS.       |
|o.     .+        |
|o.     . o       |
| .  . . +        |
|  .. ..+oo       |
+-----------------+

```

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

![github-addkey-1](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0)

点“Add Key”，你就应该看到已经添加的Key：

![github-addkey-2](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849083502905a4caa2dc6984acd8e39aa5ae5ad6c83000/0)

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。

### 关联远程仓库

你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

> 首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：
>
> ![github-create-repo-1](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849084639042e9b7d8d927140dba47c13e76fe5f0d6000/0)
>
> 在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：
>
> ![github-create-repo-2](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849084720379a3eae576b9f417da2add578c8612a2e000/0)
>
> 目前，在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

```bash
git remote add origin git@github.com:git_username/repository_name.git
```

添加后，远程库的名字就是origin，Git默认的叫法

### 取消关联远程库

```bash
git remote remove origin
```

### 查看远程库

```bash
git remote
```

### 查看远程库详细信息

如果没有相关权限，则看不到相关地址信息  

例如没有推送权限，则看不到push地址

```bash
git remote -v
```

### 推送到远程仓库

```bash
$ git push origin <branch-name>
```

`-u` 表示第一次推送master分支的所有内容,不过建议先clone再push，尽量避免此方法

```bash
$ git push -u origin <branch-name>
```

日常提交只需要使用`git push`即可

从现在起，只要本地作了提交，就可以通过命令：

```bash
$ git push origin master
```

bash把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

### 从远程克隆

```bash
$ git clone https://github.com/usern/repositoryname.git
```

日常获取只需要使用`git pull`即可

GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。