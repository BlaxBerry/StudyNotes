# API接口服务器

- Express.js

- MySQL



- 开启了CORS

- 自动表单验证





## 项目结构

```js
api_server
// 链接数据库配置文件夹
|-db
// 路由API地址文件夹
|- router
// 处理路由API函数文件夹
|- router_handler
// 验证表达数据
|-schema
// 入口文件
|-app.js
|-node_modules
|-package.json
|-package-lock.json
|-.gitignore
|-README.md
```



## 模块化开发

### 路由地址与处理函数

为了方便模块化开发

路由们应当写入单独的路由组件中

且，每个路由地址对应的处理函数也要写入单独的组件

```js
|- router
		|- user.js
|- router_handler
		|- user.js	
```

如下：

```js
// 路由user对呀的处理函数
exports.register = (req, res)=>{
  xxxxxx
}
exports.login = (req, res)=>{
  xxxxxx
}
```

```js
// 路由地址文件 之一 user.js
const express = require('express')
const router = express.Router()

const user_handler = require('../router_handler/user.js')


router.get('/register', user_handler.register)

router.get('/login', user_handler.login)

```

---

### 链接数据库MysQL

```js
|-db
	|-index.js
```

```js
const mysql =  require('mysql')

const db = mysql.createPool({
    host: '127.0.0.1',
    user: 'root',
    password: 'admin123',
    database: 'my_db_01'
})

module.exports = db
```

---

### 数据库

```js
my_db_01 (Database)
|- user (Tables)
```

| id   | username | password                         | nickname | email | avator |
| ---- | -------- | -------------------------------- | -------- | ----- | ------ |
| 1    | andy     | $2a$10$Gk7zXUzWvV3iFx8ESp7yzLc1K |          |       |        |

- id：init AI PK 

- username：varchar(45)

- password：varchar(255)
- nickname：varchar(45)
- email：varchar(45)
- avator：text

---

### 入口文件

```js
// 导入并创建express实例
const express = require('express')
const app = express()


// 导入第三方包，开启CORS
const cors = require('cors')
app.use(cors())

// 解析请求体数据格式 application/x-www-form-urlencoded
app.use(express.urlencoded({
    extended: false
}))


// 导入路由 router
const router = require('./router/user')
app.use('/api', router)



// 监听端口，开启服务器
const port = 3000
app.listen(port, () => {
    console.log('Server Running at http://127.0.0.1:3000');
})
```









## User路由

```js
|- router
		|- user.js
|- router_handler
		|- user.js	
```

### 1. 注册用户

---

#### 判断用户名是否被注册

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

---

#### 用户密码加密

万一数据库泄露会造成损失

所以用户的密码在输入数据库之前应当先加密

数据库中存储的是加密后的密码

使用第三方包 bcryptjs

- 加密后无法被逆向破解

- 同一明文密码多次加密得到的结果各不相同，安全

1. 安装：

```bash
npm i bcryptjs@2.4.3
```

2. 导入：

```js
const bcrypt = require(' bcryptjs')
```

3. 使用：

```js
const 加密的密码 =  bcrypt.hashSync(加密前密码, 加后长度)
```

如下：

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



### 表单验证

前端验证归前端，后端必须也要验证





### 2. 用户登陆

1. 判断用户名是否存在
2. 判断密码是否和数据库一致
3. 生成Token字符串

---

#### 判断数据中是否存有用户名

```js
const db = require('../db/index')

// 登陆
exports.login = (req, res) => {
    // 1. 获取用户体提交的注册数据
    const info = req.body
    if (!info.username || !info.password) {
        return res.send({
            status: 1,
            message: '用户名或用户密码不能为空'
        })
    }
    // 2. 判断用户名是否存在
    const sqlLoginName = 'SELECT  * FROM users WHERE username=?'
    db.query(sqlLoginName, info.username, (err, result) => {
        if (err) {
            return res.send({
                status: 1,
                message: err.message
            })
        }
        if (result.length !== 1) {
            return res.send({
                status: 1,
                message: '登陆失败，用户名不存在'
            })
        }
        // 3. 判断密码是否正确
    })
}
```

---

#### 判断密码是否正确

因为使用了第三方包 bcryptjs给注册时密码加密

数据库中存储的是被加密后的密码，无法直接用等号 === 比较

需要通过 bcryptjs提供的方法compareSync()

返回值是个布尔值（true：一致，false：不一致）

```js
const bcrypt = require(' bcryptjs')

const compareRes = bcrypt.compareSync(提交来的密码, 数据库中的密码)

if(!compareRes){
   return res.send('密码错误')
}
```

如下：

```js
const db = require('../db/index')
// 加密
const bcrypt = require('bcryptjs')

// 登陆
exports.login = (req, res) => {
    // 1. 获取用户体提交的注册数据
    const info = req.body
    if (!info.username || !info.password) {
        return res.send({
            status: 1,
            message: '用户名或用户密码不能为空'
        })
    }
  
    // 2. 判断用户名是否存在
    const sqlLoginName = 'SELECT  * FROM users WHERE username=?'
    db.query(sqlLoginName, info.username, (err, result) => {
        if (err) {
            return res.send({
                status: 1,
                message: err.message
            })
        }
        if (result.length !== 1) {
            return res.send({
                status: 1,
                message: '登陆失败，用户名不存在'
            })
        }
      
        // 3. 判断密码是否正确
        const compareRes = bcrypt.compareSync(info.password, result[0].password)
        if (!compareRes) {
            return res.send('密码错误')
        }
        res.send('密码正确')
      
      	// 4. 生成token字符串
    })
}
```

680