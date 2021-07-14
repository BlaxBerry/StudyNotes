# MongoDB

![](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1290818458,1689762935&fm=26&gp=0.jpg)

对前端友好：

- 开放的API使用JavaScript语法

- **NoSQL**(NoSQL = Not Only SQL )

  数据采用JSON格式存储





## 安装与初始化

### 安装

- 数据库软件：**MongDB Community Server**

- 可视化软件：**MongoDB Compass**

  可以通过图形界面操作MongoDB

---

官网下载：

```http
https://www.mongodb.com/try/download/community
```

MAC brew下载：

```bash
brew tap mongodb/brew

brew install mongodb-community@4.4
```

---

### 版本

```bash
mongod --version 

db version v4.4.5
Build Info: {
    "version": "4.4.5",
    "gitVersion": "ff5cb77101b052fa02da43b8538093486cf9b3f7",
    "modules": [],
    "allocator": "system",
    "environment": {
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}
```

---

### 开启关闭

brew 启动：

```bash
brew services start mongodb-community
```

brew 停止：

```bash
brew services stop mongodb-community
```

---

### 浏览器访问

```http
http://127.0.0.1:27017/
```

页面显示：

> It looks like you are trying to access MongoDB over HTTP on the native driver port.

---

### 相关文件地址

- 配置文件：**/usr/local/etc/mongod.conf**
- 日志文件路径：**/usr/local/var/log/mongodb**
- 数据存放路径：**/usr/local/var/mongodb**







## 术语对比

| 解释   |     SQL     |    MongoDB     | 解释                       |
| ------ | :---------: | :------------: | -------------------------- |
| 数据库 |  database   |  **database**  | **数据库**                 |
| 数据表 |    table    | **collection** | **集合**（JS数组）         |
| 数据行 |     row     |  **document**  | **文档**（JS对象）         |
| 字段   |   column    |   **field**    | **字段**（JS对象属性）     |
| 主键   | primary key |  primary key   | 主键，自动将id字段设为主键 |





## 可视化工具

![](https://cloud.ibm.com/docs-content/v1/content/eb4a20de9e4437ccdd0678de8678f1e0897dff68/nl/ja/databases-for-mongodb/images/getting-started-compass-page.png)





## 数据库中导入数据文件

```bash
mongoimport -d 数据库名 -c 集合名 --file 要导入的数据文件
```







## 命令行

### 链接MongoDB

```bash
mongo
```

exit / ctrl + c 退出链接



### 查看数据库

链接MongoDB状态下查看数据库

```bash
show dbs

---
> show dbs
admin   0.000GB
blog    0.000GB
config  0.000GB
local   0.000GB
>
```



### 进入/切换数据库

链接MongoDB状态下

```bash
use 数据库名
```

```bash
> use admin
switched to ad admin
>
```



### 创建超级管理员账号

只有先创建了超级管理员账号才能创建其他的账号

进入admin数据库然后创建账号

```bash
use admin
db.createUser({user:'root',pwd:'root',roles:['root']})
```



### 创建普通账号

对哪一个数据库创建账号就要进入哪个数据库

```bash
use 数据库
db.createUser({user:'自定义',pwd:'自定义',roles:['readWrite'])
```

