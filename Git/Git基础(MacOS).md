# Git

<img src="https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2019/08/eyecatch_git-960x504.png" style="zoom:50%;" />

git是分布式的版本管理系统

SVN是集中式版本管理系统



## 安装检查版本

```bash
git --version
git version 2.30.1
```





## 工作流程

![](https://img-blog.csdn.net/2018040822103495?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZpZXdzb25pY2s=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1. 克隆远程仓库内容到本地仓库（git clone）

2. 本地仓库覆盖工作区（git checkout）

3. 提交前存入暂存区（git add）
4. 从暂存区提交到本地仓库（git commit）
5. 从本地仓库推送到远程仓库（git push）





## 环境配置（config）

只需要**第一次Git使用前配置**即可

主要是配置用户的姓名邮箱信息

---

### 设置用户信息

```bash
git config --global user.name "XXX"
git config --global user.email "XXX@XXX.com"
```

---

### 查看配置信息

```bash
git config --list

# credential.helper=osxkeychain
# user.name=XXX
# user.email=XXX@XXX.com
```

---

### 配置文件位置

储存为位置在

> **~/.gitconfig 文件** 





## 密钥

```bash

```





## 初始化本地仓库（init）

在指定目录下创建

```bash
cd ~/XXX/XXX
git init
```





## 克隆远程仓库（clone）

克隆远程仓库到指定目录下

```bash
cd ~/XXX/XXX
git clone 远程仓库URL地址
```





## 拉取最新远程仓库（pull）

克隆只需要一次，即第一次从远程仓库拉去到本地，后面使用pull

pull拉取最新是值自己从远程仓库拉去别人更新的，比自己提交新的记录





## 文件状态

随着Git操作，文件状态会变化

- **untracked**：未被跟踪（没被提交到仓库）

- **tracked**：已跟踪（提交到了仓库）
  - **Unmodified**：未修改
  - **Modified**：已修改
  - **Staged**：已暂存



## 查看文件状态（status）

查看指定本地仓库目录下的文件状态

```bash
git status
```





## 查看提交记录（log）

查看已经被提交的历史记录

```bash
git log
```





## 忽略提交（.gitignore）

在本地仓库所在目录下创建一个文件 `.gitignore`

里面写入要被忽略提交的文件 和 文件夹的全名

注释用 `#`

```bash
# MacIOS
.DS_Store

# modules
node_modules
```





## 提交到缓存区（add）

提交指定未跟踪的文件到缓存区

```bash
git add XXX.XXX
```

提交所以未跟踪的文件到缓存区

```bash
git add .
```





## 提交到本地仓库（commit）

提交暂存区到本地仓库

```bash
git commit -m "XXX"
```





## 提交到远程仓库（push）

把本地仓库提交到指定远程仓库

```bash
git push XXXXX master
```

```bash
git push origin master
```





## 定义别名（remote add）

```bash
git remote add origin XXXXX
```

