# git配置

## git在win上安装后的初次配置
### 1.创建公钥
打开Git Bash，输入如下指令即可创建公钥
>ssh-keygen -t rsa -C "yushan2025@gmail.com"

`中间需要ENTER确认的地方直接ENTER默认即可，公钥默认存放/c/Users/even/.ssh/id_rsa`
```java
//创建公钥
even@DESKTOP-even MINGW64 /e/gitgit
$ ssh-keygen -t rsa -C "yushan2025@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/even/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/even/.ssh/id_rsa
Your public key has been saved in /c/Users/even/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:KngFw9jF4bmKsM01Zv6xKizWMHHZrvO0qO1KfGJlDlU yushan2025@gmail.com
The key's randomart image is:
+---[RSA 3072]----+
|     Eo.         |
|   +.o..         |
|  ..B o          |
| ..o + .         |
|..oo* o S        |
|.B=B = .         |
|.*Oo*.o          |
|+.*=oo.o         |
|.++=++o          |
+----[SHA256]-----+
```

公钥信息位于`C:\Users\even\.ssh\id_rsa.pub`中
### 2.添加公钥
>登录github
>
>->Setting
>
>->SSH and GPG keys
>
>->New SSH key
>
>->`key栏中填入公钥信息`
>
>->Add SSH key
>
至此添加完成，需测试ssh key是否成功。
>ssh -T git@github.com

`如果出现You’ve successfully authenticated, but GitHub does not provide shell access 。表示已成功连上github。`
```java
//测试ssh key
even@DESKTOP-even MINGW64 ~/.ssh
$ ssh git@github.com
The authenticity of host 'github.com (13.250.177.223)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,13.250.177.223' (RSA) to the list of known hosts.
PTY allocation request failed on channel 0
Hi dendyikbc! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.

```
### 3.配置本地信息
>git config --global user.name "dendyikbc"
>
>git config --global user.email yushan2025@gmail.com

查看本地配置信息
>git config --list

```java
//配置本地信息
even@DESKTOP-even MINGW64 /e/gitgit
$ git config --global user.name "dendyikbc"

even@DESKTOP-even MINGW64 /e/gitgit
$ git config --global user.email yushan2025@gmail.com

```

```java
//查看本地配置信息
even@DESKTOP-even MINGW64 /e/gitgit
$ git config --list
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=openssl
http.sslcainfo=D:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
core.autocrlf=true
core.fscache=true
core.symlinks=false
core.editor="C:\\Program Files (x86)\\Notepad++\\notepad++.exe" -multiInst -notabbar -nosession -noPlugin
credential.helper=manager
gui.recentrepo=E:/gitgit/testgit
user.name=dendyikbc
user.email=yushan2025@gmail.com

```


## git使用指令
### 1.从GitHub上down工程到本地
>->进入本地文件夹   `cd e/gitgit`
>
>->右键git bash here
>
>->执行git clone url命令等待结束即可

```java
//执行git clone url命令示意
even@DESKTOP-even MINGW64 /e/gitgit
$ git clone https://github.com/dendyikbc/dendyikbc.github.io
Cloning into 'dendyikbc.github.io'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), 952 bytes | 4.00 KiB/s, done.


```

### 2.提交本地文件到GitHub
>->进入本地需要push的工程文件夹   `cd e/gitgit/工程文件夹`
>
>->右键git bash here
>
>->push指令
```java
git add --all
git commit -m "1st commit:add xxx model"
git push -u origin master
```


一个标准顺序
 - 首先获取远程仓库链接，如：https://github.com/xxx/MyDemo.git
 - 进入本地需要push到github的项目，“cd”进入根目录。
 - 执行git init命令，初始化本地仓库，会创建一个.git的隐藏文件夹。
 
 - 执行git add .命令，将目录添加入索引
 - 执行 git commit -m "日志"
 - 执行 git remote add origin github远程仓库的链接;
 - 执行git pull origin master从远程仓库获取更新，在2.9.2之后的版本还需要加上--allow-unrelated-histories，否则会pull失败。
 - 执行 git push -u origin master等待结束，提交成功；


 其他
 ```java
//进行push前先将远程仓库pull到本地仓库
$ git pull origin master    #git pull --rebase origin master
$ git push -u origin master
 
//强制push本地仓库到远程 (这种情况不会进行merge, 强制push后远程文件可能会丢失 不建议使用此方法)
$ git push -u origin master -f
 
//避开解决冲突, 将本地文件暂时提交到远程新建的分支中
$ git branch [name]
# 创建完branch后, 再进行push
$ git push -u origin [name] 


 ```

- [all-about-git](https://gitee.com/all-about-git)