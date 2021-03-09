# Node 服务器

在node中主要使用http模块 并搭配其他模块

实现http服务器

服务器就是个js文件

**在终端使用node命令执行入口文件，**

然后在浏览器中输入指定URL就能访问指定的页面了

```bash
node app.js
```

**注意**

在服务器文件中的 console.log()

输出的位置是在终端，不在浏览器了





## 启动 与 修改

启动服务器后修改了的内容，

必须在终端中断服务器，再重新启动服务器后才能显示

```bash
crtrl + c
node app.js
```

因为这样麻烦，

可npm全局下载用第三方模块**nodemn** 或 supervisor

代替node指令去启动服务器js文件

```bash
nodemon app.js
```





## http模块

### createServer()

创建服务器，有两种写法：

#### 写法1:

```js
const http = require('http');

const server = http.createServer();
server.on('request', (req, res) => {

    res.end('hello')
})

server.listen(3000);
console.log('running at localhost:3000');
```

#### 写法2:

```js
const http = require('http');

http.createServer((req, res) => {

    res.end('hello ~')
  
}).listen(3000,() => {
    console.log('running at localhost:3000');
  })
```

使用 `createServer()`创建服务器

使用 `listen()` 监听端口

其中http.createServer() 中有两个参数，`req` 和 `res`



### request参数

获得浏览器发来的请求信息

---

#### req.url 

获取浏览器发送的请求地址

常与URL模块连用，通过判断请求地址实现**路由功能**

返回的是`域名:端口号`后的内容

如下：

```js
const http = require('http');
http.createServer((req, res) => {

    console.log(req.url);  <<===============

    res.end('hello ~')
}).listen(3000, () => {
    console.log('running at localhost:3000');
})
```

若在浏览器中`不请求地址`，或请求 `/` 时：

```bash
#浏览器地址：
loclahost:3000
localhost:3000/

#终端显示
/
```

若在浏览器地址中请求` /home`，如下，

```bash
#浏览器地址：
loclahost:3000/home

#终端显示
/honme
```







### respond参数

服务器响应给浏览器的响应信息

---

#### res.writeHead()

设置响应头

如下，设定响应是200的状态码 和 utf-8的字符集

```js
 res.writeHead(200, { "Content-type": "text/html;charset='utf-8'" })
```

**Content-Type   响应类型**

根据读取的文件决定要响应的类型

建议下载使用第三方模块mime

**charset=utf-8  设定utf-8字符集**

如下：

```js
const http = require('http');
http.createServer((req, res) => {

    res.writeHead(200, { "Content-type": "text/html;charset='utf-8'" })
    res.write('111');
    res.write('222');
    res.write('333');
    res.write('andy');
    res.write('tom');
    res.write('你好');

    res.end('hello ~')
}).listen(3000, () => {
    console.log('running at localhost:3000');
})
```



#### res.write()

可以在浏览器中输出内容

可以多次输出，

```js
const http = require('http');
http.createServer((req, res) => {

    res.writeHead(200, { "Content-type": "text/html;charset='utf-8'" })
    res.write('111');
    res.write('222');
    res.write('333');
    res.write('andy');
    res.write('tom');
    res.write('你好');

    res.end('hello ~')
}).listen(3000, () => {
    console.log('running at localhost:3000');
})
```

如上，响应内容中有中文日文，**页面的出现乱码**

因为只是设定了服务器响应的内容的类型和字符集

但是获得响应的浏览器页面并没有设定字符集

解决方法：

1. 给要跳转的html文件设定utf-8字符集（VSCode自带）
2. res.write()响应内容中写入utf-8字符集标签，如下：

---

res.write() **可解析 html标签**

```js
const http = require('http');
http.createServer((req, res) => {

    res.writeHead(200, { "Content-type": "text/html;charset='utf-8'" })
  
    res.write('<h1>andy</h1>');
    res.write('<a href="#">andy</a>');
    res.write('<ul><li>andy</li></ul>');
  
    res.end('hello ~')
}).listen(3000, () => {
    console.log('running at localhost:3000');
})
```

所以，可以在响应内容中写入utf-8字符集标签，

`<header\><meta charset="UTF-8"></header\>`

解决响应文本内容中有中文会导致页面出现乱码，

如下：

```js
const http = require('http');
http.createServer((req, res) => {

    res.writeHead(200, { "Content-type": "text/html;charset='utf-8'" })
  
    res.write('<header><meta charset="UTF-8"></header>');
    res.write('你好');
    res.write('你好');
    res.write('こんにちは');

    res.end('hello ~')
}).listen(3000, () => {
    console.log('running at localhost:3000');
})
```



#### res.end()

用于结束页面响应，

必须写，不然页面会一直刷新状态等待不会结束响应

还可以用来在页面中写仅一行

```js
const http = require('http');
http.createServer((req, res) => {

    res.end('hello ~')    <<==============
  
}).listen(3000,() => {
    console.log('running at localhost:3000');
  })
```









## url 模块

识别URL地址

---

### url.parse(URL)

解析URL地址，参数传入一个url地址，返回一个URL对象

```js
url.parse('localhost:3000/list')
```

如下：

```js
const http = require('http');
const url = require('url')
http.createServer((req, res) => {

    //console.log(req.url);

    console.log(url.parse(req.url));  <<======

    res.end('hello ~')
}).listen(3000, () => {
    console.log('running at localhost:3000');
})
```

比如：浏览器地址栏发送了

```js
console.log(url.parse('localhost:3000?name=andy&age=20'));
```

```bash
Url {
  protocol: null,
  slashes: null,
  auth: null,
  host: null,
  port: null,
  hostname: null,
  hash: null,
  search: '?name=andy&age=20',
  query: 'name=andy&age=20',   #<<========
  pathname: '/',
  path: '/?name=andy&age=20',
  href: '/?name=andy&age=20'
}
```

get请求参数以字符串放在`query属性`中



### url.parse(URL,true)

**常用**

通过参数 true， 获取get请求的参数

`query属性`中的get请求参数会转为**对象形式**，方便获取

```js
console.log(url.parse('localhost:3000?name=andy&age=20',true));
```

```bash
Url {
  protocol: 'localhost:',
  slashes: null,
  auth: null,
  host: '3000',
  port: null,
  hostname: '3000',
  hash: null,
  search: '?name=andy&age=20',
  query: [Object: null prototype] { name: 'andy', age: '20' },  
  pathname: null,
  path: '?name=andy&age=20',
  href: 'localhost:3000?name=andy&age=20'
}
```

---

比如

```js
const http = require('http');
const url = require('url')
http.createServer((req, res) => {

    let params = url.parse('localhost:3000?name=andy&age=20', true);
    console.log(params.query);
    console.log(params.query.name);
    console.log(params.query.age);

    res.end('hello ~')
}).listen(3000, () => {
    console.log('running at localhost:3000');
})
```

```bash
#终端显示如下：
[Object: null prototype] { name: 'andy', age: '20' }
andy
20
```





### url.format(URLobj)

是url.parse()的逆向操作



### url.resolve(from,to)

添加或替换地址







### 路由



根据请求路响应不同（读取）页面的

```js
const http = require('http');
const path = require('path');
const fs = require('fs')

const server = http.createServer();
server.on('request', (req, res) => {
    fs.readFile(path.join(__dirname, 'www', req.url), 'utf-8', (err, data) => {

        if (err) {
            res.writeHead(404, {
                'Content-Type': 'text/plain; charset=utf-8'
            })
            res.end('404 no such file')
        } else {
            res.end(data)
        }
    })

})
server.listen(3000);
console.log('running at localhost:3000');
```





## 自定义模块的导入

如下，在服务器中导入并使用自定义模块：

```js
// 自定义模块内容
function fn(a, b) {
    return a + b
};
function funn(a, s) {
    a.forEach(item => {
        s += item;
    })
    return s;
};

module.exports = { fn, funn }
```

```js
const http = require('http');

const fn = require('./02').fn;
const funn = require('./02').funn;

http.createServer((req, res) => {

    console.log(fn(10, 10));

    let str = '';
    let arr = [1, 2, 3]
    console.log(funn(arr, str));

    res.end('hello ~')
}).listen(3000, () => {
    console.log('running at localhost:3000');
})
```

或

```js
const http = require('http');

const fn = require('./02')

http.createServer((req, res) => {

    console.log(fn.fn(10, 10));

    let str = '';
    let arr = [1, 2, 3]
    console.log(fn.funn(arr, str));

    res.end('hello ~')
}).listen(3000, () => {
    console.log('running at localhost:3000');
})
```

