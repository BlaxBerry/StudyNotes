# PostgreSQL

![](https://s3-ap-northeast-1.amazonaws.com/images.programming-beginner-zeroichi.jp/uploads/posdb.jpg)

[菜鸟教程 PosrgreSQL](https://www.runoob.com/postgresql/postgresql-privileges.html)

[PostgreSQL 10.1 手册](https://www.runoob.com/manual/PostgreSQL/index.html)

## 下载

在Mac上可通过**Homebrew**下载

1. 更新Homebrew到最新

```bash
brew update
```

2. 下载

```bash
brew install postgresql
```

3. J检查PostgreSQL版本

```bash
psql --version
```



## 开启数据库服务

```bash
brew services start postgresql
```

连接数据库之前需要先开启数据库，

不然直接连接会报错，如下：

```bash
psql: could not connect to server: No such file or directory
Is the server running locally and accepting
connections on Unix domain socket “/tmp/.s.PGSQL.5432”?
```



## 连接数据库

开启数据库服务后，输入：

```bash
psql postgres
```

然后终端表示会变为如下：

```bash
postgres=#
```



