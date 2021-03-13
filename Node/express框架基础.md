# Express

![](https://res.cloudinary.com/practicaldev/image/fetch/s--tzFy8l83--/c_imagga_scale,f_auto,fl_progressive,h_900,q_auto,w_1600/https://dev-to-uploads.s3.amazonaws.com/i/1stt4h63pbbb79pjzuf6.png)



express是基于node.js的框架 **Web 开发框架**

原生node.js开发服务端还是有些麻烦

[express文档](https://www.expressjs.com.cn/)



## 安装 和 准备

1. 准备一个文件夹

2. 准备一个入口文件index.js

3. 初始化一个package.json文件

4. npm install express --save

5. 准备入口文件内容

如下：仅监听`根路径的请求地址`，响应 `Hello express～`

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {

    res.send('Hello express ~')
}).listen(3000, () => {
    console.log('app server running at localhost:3000');
})
```

6. 项目目录下运行入口文件

```bash
node .
```

可看到页面响应出 `Hello express～`

终端显示如下：

```bash
server app running at ocalhost:3:3000
```





## 托管静态文件

### express.static()

原生Node.js需要封装读取静态资源，麻烦

用express.static() 可以轻松托管静态文件，如图片、css文件、js文件等

参数是静态文件所在目录

```js
express.static('public')
```

然后就可以通过URL地址访问**该目录下的文件**了

```bash
域名：端口/文件名
```

如下：

入口js文件

```js
const express = require('express');
const app = express();

const server = app.use(express.static('public'));

server.listen(3000, () => {
    console.log('running at localhost:3000');
})
```

项目目录：

```bash
项目
|——node_modules
|——public
		|——images
				｜——01.jpg
				｜——02.jpg
				...
		|——css
				｜——home.css
				｜——about.css
				｜——list.css
				...
		|——js
				｜——home.js
				｜——about.js
				｜——list.js
        ...
		|——home.html
		|——about.html
		|——list.html
|——package.json
|——package-lock.json
|——index.js
```

终端执行入口js文件后，

浏览器输入的请求地址，就可访问到对应的文件

```bash
#访问public目录下的文件
localhost:3000/home.html
localhost:3000/about.html
localhost:3000/list.html
#访问public目录下的目录
http://127.0.0.1:3000/images/01.jpg
http://127.0.0.1:3000/js/sayHello.js
```



### 指定多个目录

如下：指定了三个要托管的目录：

```js
const express = require('express');
const app = express();

app.use(express.static('dir01'));
app.use(express.static('dir02'));
app.use(express.static('dir03'))

app.listen(3000, () => {
    console.log('running at localhost:3000');
})
```



### 添加虚拟目录

给静态文件添加一个虚拟的上级目录

虚拟的目录也可以被托管

```js
app.use(虚拟目录,express.static(托管目录))
```

访问时需要添加上虚拟目录的路径

如下：

```js
const express = require('express');
const app = express();

app.use('/static',express.static('public'));

app.listen(3000, () => {
    console.log('running at localhost:3000');
})
```

```bash
localhost:3000/home.html   #报错找不到请求地址

localhost:3000/static/home.html  #找到了
```





## 路由 动态路径

通过路由访问

路由是根据请求路径 和 请求方式 进行路径分发处理

http常用请求方式：

| 四种常见请求类型 | 含义 |
| :--------------: | :--: |
|       get        | 查询 |
|       post       | 添加 |
|       put        | 更新 |
|      delete      | 删除 |

如下，分别绑定不同请求：

```js
const express = require('express');
const app = express();

// get
app.get('/', (req, res) => {

    res.send('get data ~')
});
// post
app.get('/', (req, res) => {

    res.send('post data')
});
// put
app.get('/', (req, res) => {

    res.send('put data')
});
// delete
app.get('/', (req, res) => {

    res.send('delete data')
});

app.listen(3000, () => {
    console.log('running at localhost:3000');
})
```

或，使用`use()`直接绑定所有请求

```js
const express = require('express');
const app = express();

app.use((req,res)=>{
  res.send('ok')
})

app.listen(3000, () => {
    console.log('running at localhost:3000');
}) 
```





## 中间件（Middleware）

一个Express应用就是在调用各种中间件

中间件是一个函数，可理解为方法，比如use() API

中间件可以访问 请求对象req 和 响应对象res  还有一个next变量

### 简介

分类：

- **应用级中间件**（比如use()、express.static()）

- **路由中间件**（get()）

- **错误处理**

---

参数：

- **req参数**：请求对象

- **res参数**：响应对象

- **next参数**：把请求传递给下一个中间件



### 挂载方式 - use()

#### next()

中间件内部通过`next()`，把请求传递给下一个中间件

```js
const express = require('express');
const app = express();
let total = 0;

app.use('/user', (req, res, next) => {

    console.log(Date.now());
    next()
})
app.use('/user', (req, res, next) => {

    console.log('有人访问了localhost:3000/user');
    next();
})
app.use('/user', (req, res, next) => {

    total++;
    console.log(total);
    res.send(`访问次数 ${total}`);
})

app.listen(3000, () => {
    console.log('running at localhost:3000');
})
```

```bash
#终端响应如下：
running at localhost:3000
202103101310101010
有人访问了localhost:3000/user
访问次数 1
```

如果上一个中间件中没有`next()`，下一个中间件会阻塞

比如在第二个中间件位置，不写`next()`，

第三个中间件不会执行，只执行res.send()

```bash
#终端响应如下：
202103101310101010
有人访问了localhost:3000/user
```

然后页面会一直等待响应中

---

#### 简洁写法

上面写法啰嗦，不明朗

可以利用**分别封装中间件的执行函数，存放于数组中**

然后use() 调用这个数组

如下：

```js
const express = require('express');
const app = express();

let fn01 = function(req, res, next) {
    console.log(111);
    next();
}
let fn02 = function(req, res, next) {
    console.log(222);
    next();
}
let fn03 = function(req, res, next) {
    console.log(333);
    res.send('end');
}

app.use('/', [fn01, fn02, fn03])

app.listen(3000, () => {
    console.log('running at localhost:3000');
})
```



### 挂载方式 - get() 路由

#### next()

中间件内部通过`next()`，把请求传递给下一个中间件

```js
const express = require('express');
const app = express();

app.get('/abc', (req, res, next) => {

    console.log(111);
    next()

}, (req, res, next) => {

    console.log(222);
    next()
}, (req, res) => {

    console.log(333);
    res.send('end')
})


app.listen(3000, () => {
    console.log('running at localhost:3000');
})
```

```bash
#终端响应如下：
running at localhost:3000
111
222
333
```

如果上一个中间件中没有`next()`，下一个中间件会阻塞

比如在第二个中间件位置，不写`next()`，

第三个中间件不会执行，只执行res.send()

```bash
#终端响应如下：
running at localhost:3000
111
222
```

然后页面会一直等待响应中

---

#### 简洁写法

上面写法啰嗦，不明朗

可以利用**分别封装中间件的执行函数，存放于数组中**

然后get()调用这个数组

如下：

```js
const express = require('express');
const app = express();

let fn01 = function(req, res, next) {
    console.log(111);
    next();
}
let fn02 = function(req, res, next) {
    console.log(222);
    next();
}
let fn03 = function(req, res, next) {
    console.log(333);
    res.send('end');
}

app.get('/', [fn01, fn02, fn03])

app.listen(3000, () => {
    console.log('running at localhost:3000');
})
```



#### next('route')

后面的中间件不在执行，直接**跳转到下一个路由**

```js
app.get(PATH,(req,res,next)=>{
  next('route')
},(req,res,next)=>{
  res.send()
});
app.get(PATH,(req,res,next)=>{
  next()
},(req,res,next)=>{
  res.send()
})
```

如下：

在第一个路由的第一个中间件中`next('route')`

```js
const express = require('express');
const app = express();

app.get('/abc', (req, res, next) => {

    console.log(111);
    next('route')

}, (req, res, next) => {

    console.log(222);
    next()
}, (req, res) => {
    console.log(333);
    res.send('end')
});

app.get('/abc', (req, res) => {

    console.log(1111111111);
    res.send('end')
})


app.listen(3000, () => {
    console.log('running at localhost:3000');
})
```

第一个路由的第一个中间件后的中间件并没有执行

直接跳转到下一个路由

```bash
#终端响应如下：
111
11111111111
```





### 错误响应

错误响应中多了一个`error参数`

```js
app.use(err,req,res,next){
  console.err(err.stack);
  res.status(500).send('error,something brken')
}
```



### 第三方包的中间件

比如，获取POST请求的 **body-parser**

安装导入过程

项目目录下生成package.json文件

下载包

在入口文件中require导入包

导入后就可以挂载包的中间件（使用其方法）



#### body-paser 

express没有内置获取POST请求的API，

只能导入第三方包 **body-parser**

使用其封装的body属性



ajax



## POST请求

### body-parser

获取psot请求

express没有内置获取POST请求的API，

只能导入第三方包 **body-parser**

使用其封装的body属性



先生成包

下载包

在入口文件中导入后，挂载包的中间件



### req.body



```js
const express = require('express');
const app = express();
// 导入
const bodyParser = require('body-parser');
//挂载参数处理中间件（post）
app.use(bodyParser.urlencoded({ extended: false }))

// 挂载内置中间件 托管静态文件
app.use(express.static('public'))

// 路由
app.post('/login', (req, res) => {

    let data = req.body;
    console.log(data);
    console.log(data.usrname);
    console.log(data.usrage);
  
    res.send('OK')
})
app.listen(3000, () => {
    console.log('web server running at localhost:3000');
})
```

```bash
#
web server running at localhost:3000
[Object: null prototype] { usrname: 'andy', usrage: '28' }
andy
28
```





### 验证表单

```js
const express = require('express');
const app = express();
const bodyParser = require('body-parser');

app.use(bodyParser.urlencoded({ extended: false }))

app.use(express.static('public'))

app.post('/login', (req, res) => {

    let data = req.body;
    if (data.usrname == 'andy' && data.password == '123') {
        res.send('succeed')
    } else {
        res.send('no')
    }
})

app.listen(3000, () => {
    console.log('web server running at localhost:3000');
})
```





## GET请求

### req.query

```js
const express = require('express');
const app = express();

// 挂载内置中间件 托管静态文件
app.use(express.static('public'))

// 路由
app.get('/login', (req, res) => {

    let data = req.query;

    console.log(data);
    console.log(data.usrname);
    console.log(data.password);

    res.send('ok')
})


app.listen(3000, () => {
    console.log('web server running at localhost:3000');
})
```

```bash
web server running at localhost:3000
{ usrname: 'andy', password: '1234' }
andy
1234
```