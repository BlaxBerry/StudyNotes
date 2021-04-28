# MongoDB

![](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1290818458,1689762935&fm=26&gp=0.jpg)

**NoSQL**(NoSQL = Not Only SQL )

分布式

类似 JSON 的 BSON 格式





## brew安装 启动

Mac可通过的 brew 来安装 mongodb

```bash
brew tap mongodb/brew

brew install mongodb-community@4.4
```

- 使用 brew 命令来**启动服务**

```bash
brew services start mongodb-community@4.4
```

- 使用 brew 命令来**关闭服务**

```bash
brew services stop mongodb-community@4.4
```

开启的**端口号：27017**

```http
http://localhost:27017/
```

浏览器地址栏输入即可访问：

```html
It looks like you are trying to access MongoDB over HTTP on the native driver port.
```



## 创建日志、数据存放的目录

使用brew安装好的MongoDB，默认会在如下位置创建相关文件

- 配置文件（the configuration file）：

  **/usr/local/etc/mongod.conf**

- 日志文件路径（the log directory path ）：

  **/usr/local/var/log/mongodb**

- 数据存放路径（the data directory path ）：

  **/usr/local/var/mongodb**



查看数据库

show databases