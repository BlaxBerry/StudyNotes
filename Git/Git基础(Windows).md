# Git基础入门

[黑马Git/ Github基础教程](https://www.bilibili.com/video/BV1Mk4y1y7jR?p=4)

Git是个版本管理控制系统(VCS)

可以在任何时间点，将文档的状态作为更新记录保存起来；

也可在任何时间点，将更新过的记录恢复回来



## Git 安装

[Git官网下载](https://git-scm.com/downloads)

### 打开Git鼠标右键   Git Bash Here  

Git的操作窗口其实就是个命令行程序

***

### 检查当前Git版本

```git
$ git --version
git version 2.30.0.windows.2
```

***

### 清屏操作窗口

```git
$ clear
```





## Git 工作流程

### 工作目录 ==> 暂存区  ==> Git仓库

| 工作目录                  | 暂存区               | Git仓库      |
| ------------------------- | -------------------- | ------------ |
| 被Git管理的项目目录文件夹 | 临时存放被修改的文件 | 存放提交记录 |

在当前工作的目录打开Git，

先把要提交的文件从 **工作目录 **提交到 **暂存区**，

然后再从 **暂存区** 提交到 **Git仓库**





## 配置信息

只需要**第一次Git使用前配置**即可。

使用Git提交前要告诉Git提交人的信息，

方便日后多人协作出问题时查找当事人

### 配置提交人姓名

```bash
$ git config --global user.name 名字

$ git config --global user.name BlaxBerry
```

***

### 配置提交人邮箱

```bash
$ git config --global user.email 邮箱地址

$ git config --global user.email chenjiaxu333@gmail.com
```

***

### 查看Git配置信息

```bash
$ git config --list

返回下列：
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=openssl
http.sslcainfo=C:/IT/Git/mingw64/ssl/certs/ca-bundle.crt
core.autocrlf=true
core.fscache=true
core.symlinks=false
pull.rebase=false
credential.helper=manager-core
credential.https://dev.azure.com.usehttppath=true
init.defaultbranch=master
user.name=BlaxBerry
user.email=chenjiaxu333@gmail.com

有配置的提交人姓名和邮箱地址，说明没有问题
```

***

#### 修改配置信息

若想修改配置信息，重新执行一遍上述命令，**覆盖之前的配置**

```bash
$ git config --global user.name 新的名字
$ git config --global user.email 新的邮箱地址
$ git connfig --list
```





## Git 提交步骤

### 初始化Git仓库

```bash
$ git init
```

即在工作目录下初始化新建一个仓库

```bash
$ git init
Initialized empty Git repository in C:/IT/IT  笔记/Git GitHub/.git/
说明在该路径下已经初始化新增了一个Git仓库
```

然后该工作目录下会新建一个隐藏的文件夹 **.git**

***

### 查看文件状态

```bash
$ git status
```

文件状态只有两种，**被管理的文件** 和 **未被管理的文件**

可获知有哪些文件尚未被Git跟踪管理，即还没被提交到缓存区中

```bash
$ git status

On branch master
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git.md
nothing added to commit but untracked files present (use "git add" to track)

可知当前有被提交管理的文件，文件名为 Git.md
```

***

### 向暂存区提交文件

```bash
$ git add 文件名
```

把文件提交到缓存区，让Git管理给文件

```bash
$ git add Git.mad
$ git status

On branch master
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   Git.md
        
说明有新文件被提交到了缓存区，被Git追踪管理， 文件是 Git.md

   
$ git status

On branch master
nothing to commit, working tree clean
说明此时工作区和暂存区没有需要提交的文件
```

***

### 向Git仓库提交文件

```bash
$ git commit -m 提交说明
```

把文件从缓存区提交到Git仓库

每次提交时**必须写提交说明**

```bash
$ git commit -m 文件Git.md第一次提交

[master (root-commit) 1d69d2c] 文件Git.md第一次提交
 1 file changed, 185 insertions(+)
 create mode 100644 Git.md

#说明文件向Git仓库提交成功
#并告诉这次有一个问价变化
```

***

### 查看提交历史记录

```bash
$ git log
```

```bash
$ git log

commit be062c44cd7fe716336899ada3e00f2ba5164e98 (HEAD -> master)
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 20:19:31 2021 +0900
    文件Git.md第二次提交
commit 1d69d2cc93f40b334a86acbc269a4b356944a3f5
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 20:15:57 2021 +0900
    文件Git.md第一次提交
```

如果提交记录过长，查看完后 可按 **q键** 退出查看

---

## Git 忽略清单

不想被Git管理的文件和文件夹

在工作目录下创建一个**.gitignore文件**

在 **.gitignore**文件中写上不想被Git管理的文件和文件夹的全名

添加到缓存区时，**.gitignore文件**也要被添加到缓存区

添加时Git会自动忽略那些忽略的文件和文件夹

---

## 全部添加暂存区

把工作目录下的文件和文件夹全部添加到暂存区

```bash
$ git add .
```





## 撤销

### 暂存区文件覆盖工作目录

```git
$ git checkout 文件名

$ git checkeout Git.md
```

***

### 删除暂存区中的文件

```git
$ git rm --cached 文件名
```

即删除提交到暂存区的文件，使该文件变为没有被Git跟踪管理的的状态

该命令执行完后，**暂存区中将不再存在该文件**

```git
$ git status

On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   Git.md

$ git rm --cached Git.md
rm 'Git.md'

$ git status

On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    Git.md
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Git.md
```

***

### Git仓库文件覆盖暂存区和工作目录

即把提交到Git仓库的文件的指定更新某次记录恢复出来，

并覆盖暂存区和当前工作目录，然后删除该次提交记录后的所有最近的记录

利用提交到Git仓库时的**ID**来选择指定的某次更新

```
$ git reset --hard 指定更新时的ID
即 commit 后的那一串
```

```git
$ git log

commit 4e4b7ce746e2da897e02fac7b963b2704c9725d0 (HEAD -> master)
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 22:03:33 2021 +0900
    文件Git.md第六次提交
commit 3ff82c2d20aea52579ec44b7fe8a363e15120659
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 21:54:36 2021 +0900
    文件Git.md第五次提交
commit 20fb922f2138473a5fc9722405ae5bb16089bc01
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 21:51:30 2021 +0900
    文件Git.md第四次提交
commit 2ce14dd76fea23a0aceded693a53d49e7c25241a
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 21:48:27 2021 +0900
    文件Git.md第三次提交
commit be062c44cd7fe716336899ada3e00f2ba5164e98
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 20:19:31 2021 +0900
    文件Git.md第二次提交
commit 1d69d2cc93f40b334a86acbc269a4b356944a3f5
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 20:15:57 2021 +0900
    文件Git.md第一次提交


$ git reset --hard 2ce14dd76fea23a0aceded693a53d49e7c25241a
HEAD is now at 4e4b7ce 文件Git.md第三次提交
如上，从Git仓库中恢复的是第三次提交的文件


$ git log


commit 2ce14dd76fea23a0aceded693a53d49e7c25241a
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 21:48:27 2021 +0900
    文件Git.md第三次提交
commit be062c44cd7fe716336899ada3e00f2ba5164e98
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 20:19:31 2021 +0900
    文件Git.md第二次提交
commit 1d69d2cc93f40b334a86acbc269a4b356944a3f5
Author: Chen <chenjiaxu333@gmail.com>
Date:   Mon Jan 18 20:15:57 2021 +0900
    文件Git.md第一次提交

恢复文件后，更新记录中在第三次提交后的所有提交记录被删除了
仅保留了第三次提交前的记录
```





## Git 分支

使用分支，**在不分支上做不同事情，避免相互影响**

比如，在一个分支线专门修复Bug，在另一个分支上专门开发新功能

### 主分支 > 开发分支 > 功能分支

***

#### 主分支   master

第一次向Git仓库提交更新时，会自动生成一个主分支

分支上每一个时间点就是更新的提交

***

#### 开发分支   develop

开发分支就是**主分支的一个副本**

基于主分支分离出来创建其他开发分支，以免影响开发主线

开发中一定要保证主分支的稳定性(代码的稳定性)主分支是直接可以向外界发布的，不能直接在上面开发

一般是在开发分支上开发，然后合并到主分支

***

#### 功能分支  feature

是基于开发分支创建的，作为**开发具体功能**的分支

开发中也要最大限度保证开发分支的稳定性

一般是在功能分支上开发功能，然后再合并到开发分支上

等开发分支的功能完成后，再把开发分支合并到主分支

***

还可有专门修复bug的分支

或者专门存放软件不同版本的分支，进行分别开发

***

***

### 分支命令

#### 查看分支

```bash
$ git branch

* 当前所在分支名
```

第一次向Git仓库提交过文件后就自动生成一个默认**主分支**

```bash
$ git commit -m "第一次提交"

[master (root-commit) d78c8f8] 第一次提交
 1 file changed, 12 insertions(+)
 create mode 100644 01.js

$ git branch
* master

#所在分支颜色会变绿，并且前面会多出一个 * 
```

***

#### 创建分支

```git
$ git branch 自定义分支名
```

基于当前所在分支上创建分支(创建副本)

```bash
USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub (master)
$ git branch
* master

USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub (master)
$ git branch develop

USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub (master)
$ git branch
  develop
* master


#基于master分支创建一个了develop开发分支
#并且当前所在mater分支上
```

***

#### 切换分支

```git
$ git checkout 分支名
```

```git
USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub (master)
$ git branch
  develop
* master

USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub (master)
$ git checkout develop
Switched to branch 'develop'

USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub (develop)
$ git branch
* develop
  master

如上从master主分支上切换到了develop开发分支上
```

**切换分支前，必须把当前所在分支的工作目录里的文件都提交到Git仓库，**

不然当前分支工作目录的文件，会影响到新切换过去的分支

如下：

假设master分支中有 01.js文件，并提交到了Git仓库，

develop分支因为是在master分支上创建的副本，所以也有 01.js 文件，

如果在develop中又新建了 02.js 文件，

但 **如果 02.js文件没提交到Git仓库的话，**(通过**git status**确认）

此时若从develop分支切换到master分支，

会发现  **master分支上居然出现了 develop分支上创建的02.js文件，**

（可在文件夹和VSCode的工作目录确认）

这样一来master分支的开发就受到了影响



但是如果在develop分支切换到master分支前，

**已经把当前develop分支的所有文件包括02.js文件提交到Git仓库了的话，**

再切换到master分支时，**master分支上是不会出现这个02.js文件，**

因为 02.js文件 是从develop分支上提交给Git仓库的，所以**仅在该分支develop分支上才能看到这个02.js文件，**

（可在文件夹和VSCode的工作目录确认）

从而实现了在不同分支的开发不会影响到其他分支



综上，切换分支时的严谨步骤：

1. 一定要先在被合并分支下查看文件状态
2. 若有没提交到Git仓库的文件，先提交

2. 然后再切换分支

```git
$ git status
$ git commit -m ****
$ git checkout 分支
```

***

#### 合并分支

若当前分支的工作已经完成，就可以把当前分支合并到其他分支中去

要在合并到的目标分支上执行合并代码，把被合并的分支合并过来

```bash
$ git merge 被合并的分支
```

如下：在master分支的角度上把develop分支合并过来

```bash
$ git merge develop

Updating 9a9e9a9..eab3b7f
Fast-forward
 develop-03.js | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 develop-03.js
 
 
实现了把develop分支上的文件合并到了master分支
```

综上，合并分支时的严谨步骤：

1. 一定要先在被合并分支下查看文件状态，

2. 然后再切换分支到要合并到的分支
3. 在目标分支内把要被合并的分支合并过来

```git
$ git status
$ git checkout 要合并到分支
$ git merge 被合并的分支 
```

分支被合并以后，还是依然存在的，

还可以切换回区继续开发补充，然后再合并分支

***

#### 删除分支

为了防止误删，只有被合并过的分支才可以被 **git branch -d** 删除

若想删除未被合并过的分支，可用**git branch -D **强制删除

```git
$ git branch -d 分支名
或
$ git branch -D 分支名
```

无法删除当前所在的分支，必须切换到其他分支后才能删除分支

```git
USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub/code (master)
$ git branch
  develop
* master

USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub/code (master)
$ git branch -d develop
Deleted branch develop (was eab3b7f).

USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub/code (master)
$ git branch
* master

如上，删除了develop分支
```

***

***

### 临时切换分支  暂时保存

上述，切换分支前必须把当前工作目录下的文件全部提交Git仓库后才行，

但在**临时切换分支**的场合，即当前分支的开发还未完成不能提交Git仓库，但又不得不切换分支的场合，

此时需要**暂时保存**当前分支上的所有修改并**储存到剪切板**，

```git
$ git stash
```

先把文件的改动保存到缓存区，然后把缓存区暂时保存到剪切板,

便可以得到一个干净的工作目录，然后就可以切换到其他分支了

```git 
USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub/code (develop02)
$ git add develop-03.js

USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub/code (develop02)
$ git status
On branch develop02
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   develop-03.js

USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub/code (develop02)
$ git stash
Saved working directory and index state WIP on develop02: eab3b7f develop-03.js从develop提交

USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub/code (develop02)
$ git status
On branch develop02
nothing to commit, working tree clean

如上，临时保存后git status，会提示工作目录没有更新需要提交Git仓库
此时再切换分支就不会影响到其他分支了
```

***

等再切换回该分支时，可以从剪切板中***再恢复***刚刚暂时保存的内容

```git
$ git stash pop
```

```git
USER@DESKTOP-46BRUEO MINGW64 /c/IT/IT  笔记/Git GitHub/code (develop02)
$ git stash pop
On branch develop02
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   develop-03.js

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (4004ec03701a5023dad313b7b4e2a92f55b0de6b)

```

保存的临时内容的**剪切板是独立于所有分支**，

在任何分支都可以恢复获得临时保存到剪切板的内容，

恢复后的临时内容将不再保存在剪切板中，

所以**恢复临时内容前一定要确认所在的分支**













# Github  远程仓库

[BlaxBerry首页](https://github.com/BlaxBerry)



如果只是一个人开发的话，仅本地仓库Git就够了

但多人开发的话，需要一个远程仓库共享代码，

Github也是一个公共远程仓库



## 多人开发流程

1. 开发者A在本地计算机中创建一个本地Git仓库

2. 开发者A在Github中创建一个远程仓库

3. 开发者A把本地仓库推送到远程仓库

4. 开发者B克隆Github的远程仓库到本地然后开发

5. 开发者B把本地仓库推送扫远程仓库

6. 开发者A把远程仓库中的最新内容拉到本地



## 推送到远程仓库

### 本地仓库推送到远程仓库

复制在Github初次创建远程仓库时提供来的http地址，然后给push提交

```bash
$ git push 远程仓库地址 分支名
```

```bash
$ git push https://github.com/BlaxBerry/jQuery---todoList.git master

Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 4 threads
Compressing objects: 100% (9/9), done.
Writing objects: 100% (9/9), 38.37 KiB | 3.20 MiB/s, done.
Total 9 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/BlaxBerry/jQuery---todoList.git
 * [new branch]      master -> master

```

***

### 远程仓库别名

远程仓库地址太长，第一次推送过后，每次发送时候还都要输入的话太麻烦

可以给远程仓库添加一个别名，添加仓库时候就可以使用别名而不是再全部输入地址了

``` github
$ git remote add 别名 远程仓库地址
```

一般是把远程仓库别名定位**origin**

``` git
$ git remote add 别名 远程仓库地址
```

```git
$ git remote add origin https://github.com/BlaxBerry/jQuery---todoList.git
$ git push origin master
```

---

### 记住推送过的远程仓库地址

推送更新到远程仓库时，使用参数 -u，让Git记住推送过的远程仓库地址，

下次再推送时候只用 git push即可推送到同一仓库

```git
$ git push -u 远程仓库地址 分支名
$ git push
```

```git
$ git push -u https://github.com/BlaxBerry/jQuery---todoList.git master
$ git push
```

---

---

## 克隆远程仓库

本地没有该仓库，从远端数据仓库克隆到本地

只会在开发者加入开发项目时第一次获取仓库时，会用到**clone**

```git
$ git clone 仓库地址
```

---

---

## 拉取最新的远程仓库

本地已有该仓库，开发中。

使用**pull** 会把远程仓库内容和本地的比较，从远端仓库拉取最新更新内容

```github
$ git pull 仓库地址 分支名
```

推送远程仓库时，如果本地仓库更新版本低于远程仓库，是无法提交的

需要先拉取最新的更新版本到本地修改后才能提交

---

---

## 解决冲突 

如果两个人同时修改了同一个地方，就会冲突

先提交的人会提交成功，后提交的人推送会报错





## GitHub的readme

在工作目录下创建一个**readme.md文件**

写好内容后也推送到远程仓库

在里面写的内容会显示在GitHub仓库的README中



# SSH协议免登录

使用http协议推送的话每次都需要登录GitHub账号，麻烦

通过密钥，密钥分为**公钥**和**私钥**， 就是两个文件

公钥在GitHub仓库中，私钥在本地（相当于锁和钥匙）

往远程仓库推送内容时，SSH协议判断公钥和私钥是否匹配

配对成功时推送更新成功，配对失败则推送失败

## 生成密钥

```github
$ ssh-keygen
```

使用默认值即可

```github

DESKTOP-46BRUEO% ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/chen/.ssh/id_rsa):
Created directory '/home/chen/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/chen/.ssh/id_rsa
Your public key has been saved in /home/chen/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:M4aZ1a1QfZM11tnqTMXKtbYYxgFKyh7bwC3ZDWURa0Q chen@DESKTOP-46BRUEO
The key's randomart image is:
+---[RSA 3072]----+
|         o+E+  *=|
|      o *o*.o.=.B|
|       Oo+.+oo.*.|
|      .=*... +=o |
|      +oS.. .++ .|
|       . o   .o. |
|                 |
|                 |
|                 |
+----[SHA256]-----+

```

---

## 密钥位置

生成的这一对密钥就是两个字符串文件，文件的位置是：

> Windows：用户user目录下 .ssh文件夹
>
> WSL Ubuntu：**\\wsl$\Ubuntu\home\chen\ .ssh**

生成两个文件是：

> 私钥：id_rsa
>
> 公钥：id_rsa.pub

---

## 添加公钥

首先用VScode打开公钥， 复制里面的字符串

到GitHub的设置中的SSH keys，添加公钥粘贴复制的字符串

---

## 使用SSH协议推送

和通过http协议推送本地仓库类似

```
$ git remote add origin_ssh git@github.com:BlaxBerry/todoList.git

$ git push origin_ssh master
```

因为GitHub上保存了公钥，自己的本地上保存了与之匹配的私钥

所以推送时会自动匹配，然后推送

 不会和http协议一样每次都要登录GitHub后才能推送





