# MySQL基础

<img src="https://d1.awsstatic.com/asset-repository/products/amazon-rds/1024px-MySQL.ff87215b43fd7292af172e2a5d9b844217262571.png" style="zoom:50%;" />





传统型数据库、关系型数据库、SQL数据库



## 安装

需要安装

- **MySQL Server**

  提供数据的存储和服务

- **MySQL Workbench**

  可视化MySQL管理工具

  操作存储在MySQL中的数据



### Mac环境安装

1. 先安装**MySQL Server**

2. 再安装**MySQL Workbench**工具

---

#### MySQL Server的安装

1. 运行 **mysql-8.0.19-macos10.15-x86_64.dmg**

2. 一路继续

3. 在安装过程的Configuration界面

   选下面的Use Legacy Pasword Encryption，

   然后点next

4. 设置**MySQL数据库登陆密码**

   **admin123**

5.安装完成

---

#### MySQL Workbench工具的安装

1. 运行**mysql-workbench-community-8.0.19-macos-x86_64.dmg**

2. 拖拽到Application，
3. 安装完成



### Windows环境安装

运行 **mysql-installer-communicty-8.0.19.0.msis**安装包

便可一次性安装 **MySQL Server** 和 **MySQL Workbench**

若是 win7、win8系统 还需先安装运行环境







## 可视化工具管理数据库

通过MySQL Workbench，

以可视化工具管理MySQL数据库

---

### 链接数据库

通过**MySQL Workbench**链接到MySQL数据库

![](https://pbs.twimg.com/media/E3c-z70UYAEErJt?format=jpg&name=small)

---

### 工具主界面

![](https://pbs.twimg.com/media/E3dDFBKVIAAawvE?format=jpg&name=small)

---

### 创建数据库

1. 工具栏点击创建图标

2. 弹出的 new schema界面中定义数据库名 schem name

   不能使用中文空格

3. 点击apply添加

4. 点击弹出界面下方的apply

5. 完成创建数据库

   显示到了左侧schemas列表中

![](https://pbs.twimg.com/media/E3dDD9FVcAcfppw?format=jpg&name=small)

---

### 创建数据表

![](https://pbs.twimg.com/media/E3dDCzqVgAUTl-A?format=jpg&name=small)



字段特殊标识

- **PK**（Primary Key）：主键，唯一标识
- **NN**（Not Null）：值不允许为空
- **UQ**（Unique）：值唯一
- **AI**（Auto Increment）：值自增长，常用于id

- **Default**：默认值



数据类型（DataType）

- int：整数

- varchar(45)：最大45字符的字符串

- tinyint(1)：布尔值

  0：正常

  1：禁用

---

### 写入数据行

1. 左侧列表中找到目标数据库的数据表

2. 右键点击数据表

   选择弹出的 SelectRows- Limit1000

3. 在表中插入数据

4. 点击apply提交

![](https://pbs.twimg.com/media/E3dP6OyVEAEgXcT?format=jpg&name=medium)







## SQL语言管理数据库

通过SQL语言，以编程的形式操作数据库的数据

详见笔记 [SQL基础](https://github.com/BlaxBerry/StudyNotes/blob/master/DataBase%E6%95%B0%E6%8D%AE%E5%BA%93/SQL%E5%9F%BA%E7%A1%80.md)







## 项目中操作MySQL

### Node.js Express 

1. 安装第**三方模块mysql**

2. 通过mysql模块**链接到MySQL数据库**
3. 通过mysql模块**执行SQL语句**

详见笔记 [Node.js模块](https://github.com/BlaxBerry/StudyNotes/blob/master/Node.js/Node.js%E5%9F%BA%E7%A1%80.md)

```js
// 配置mysql

const mysql = require('mysql')

const db = mysql.createPool({
    host: '127.0.0.1',
    user: 'root',
    password: 'admin123',
    database: 'my_db_01'
})


module.exports = db
```







## Node.js

### 判断用户名是否被注册

```js
// 导入MySQL的配置
const db = require('../db/index')

// 注册用户
exports.register = (req, res) => {
    // 1. 获取用户体提交的注册数据
    const info = req.body
    if (!info.username || !info.password) {
        return res.send({
            status: 1,
            message: '用户名或用户密码不能为空'
        })
    }

    // 2. 判断用户名是否已被注册
    const sql = 'SELECT  * FROM users WHERE username=?'
    db.query(sql, info.username, (err, result) => {
      // 查找失败
        if (err) {
            return res.send({
                status: 1,
                message: err.message
            })
        }
      // 有匹配到的数据
        if(result.length>0){
            return res.send({
                status: 1,
                message: '用户名已存在'
            })
        }
      // 没有匹配到数据
        res.send('用户名未被注册，ok')

    })
}
```



### 用户密码加密

万一数据库泄露会造成损失

所以用户的密码在输入数据库之前应当先加密

数据库中存储的是加密后的密码

使用第三方包 bcryptjs

- 加密后无法被逆向破解

- 同一明文密码多次加密得到的结果各不相同，安全

安装：

```bash
npm i bcryptjs@2.4.3
```

导入：

```js
const bcrypt = require(' bcryptjs')
```

使用：

```js
const 加密的密码 =  bcrypt.hashSync(加密前密码, 加后长度)
```





```js
// User handle Function

// database
const db = require('../db/index')
// 加密
const bcrypt = require('bcryptjs')

// 注册用户
exports.register = (req, res) => {
    // res.send('注册 okKKK')
    // 1. 获取用户体提交的注册数据
    const info = req.body
    if (!info.username || !info.password) {
        return res.send({
            status: 1,
            message: '用户名或用户密码不能为空'
        })
    }

    // 2. 判断用户名是否已被注册
    const sqlCheck = 'SELECT  * FROM users WHERE username=?'
    db.query(sqlCheck, info.username, (err, result) => {
        if (err) {
            return res.send({
                status: 1,
                message: err.message
            })
        }
        if (result.length > 0) {
            return res.send({
                status: 1,
                message: '用户名已存在，请更换其他用户名'
            })
        }

        // 随机加密用户密码
        info.password = bcrypt.hashSync(info.password, 10)

        const sqlInsert = 'INSERT INTO users SET?'
        const objInsert = {
            username: info.username,
            password: info.password
        }
        db.query(sqlInsert, objInsert, (err, result) => {
            if (err) {
                return res.send({
                    status: 1,
                    message: err.message
                })
            }
            if (result.affectedRows !== 1) {
                return res.send({
                    status: 1,
                    message: '注册用户失败。请稍后再尝试'
                })
            }
            res.send({
                status: 0,
                message: '注册成功'
            })
        })

    })

}
```
