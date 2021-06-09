# Node.js

![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2018/12/node-icon-960x504.jpg)

Node.js是个基于Chrome V8 引擎的JavaSCript运行环境





## JavaScript运行环境

JS可以运行在浏览器（前端开发），

还可以运行在基于JS解析引擎的Node.js上（后端开发）

---

### 浏览器运行环境

浏览器是JS的**前端运行环境**

浏览器都内置JS解析引擎，其中Chrome浏览器的V8引擎效率最高

- JS解析引擎

- 运行环境提供的内置API

  （DOM、BOM、Canvas、XMLHttpRequest、JS内置对象...）

---

### Node.js运行环境

Node.js是JS的**后端运行环境**

**Node.js中无法调用浏览器提供的DOM，BOM，Ajax等API函数**

- V8引擎 

- 运行环境提供的内置API

  （fs、path、http、querystring、JS内置对象...）







## Node.js简介

作为是一个JS的运行环境，是一个底层的东西

虽然Node.js仅提供了基础功能+内置API，

但基于Node.js的各种工具、框架很重要，比如：

- **Express框架**：快速构建Web应用
- **Electron框架**：构建跨平台桌面应用
- **restift框架**：快速构建API接口项目
- **读写操作数据库**
- 创建各种命令行工具

---

**建议流程：**

1. JS基础语法

2. 内置API模块

3. 第三方模块（express，mysql）





## 环境安装

可通过官方网站指定版本下载

也可通过npm安装NVM版本管理器下载

---

### 版本区分

**LTS版本**：长期稳定版，适合企业级项目

**Current版本**：新特性测试尝鲜版，可能存在隐藏Bug漏洞

---

### 版本号查看

```bash
node -v
```







## 终端中执行JS文件

在JavaScript文件目录下打开终端

```bash
cd JS文件存放目录
node JS文件名
```

如下：

```js
// JS 文件
console.log('Hello Node.js');
var a = 10;
var b = 20
var c = a + b;
console.log(c);
```

```bash
# 终端
node test.js

Hello Node.js
30
```







## fs 文件系统模块

fs模块是Node.js提供的内置模块

可用于**文件操作**

- **fs.readFile()**

  读取指定文件内容

- **fs.writeFile()**

  向指定文件写入内容

---

在JS文件中向使用fs模块，要先通过require导入模块

```js
const fs = require('fs')
```



### fs.readFile()

```js
fs.readFile('文件路径', ['编码格式'], 回调函数)
```

- 文件路径（必须）

  字符串形式

- 编码格式（可选）

  表示以什么编码格式读取文件，

  常用 **utf8**

- 回调函数（必须）

  文件读取完后，通过回调函数的参数拿到读取结果

  **err**：读取失败的错误对象，错误原因存放在**err.massage**

  **data**：读取成功的文件内容

  错误优先，err在data前面

---

#### 返回结果

```js
// JS文件
const fs = require('fs')

fs.readFile('./01.txt', 'utf8', function(err,data){
    console.log(err);

    console.log('--------');

    console.log(data);
})
```

```js
// txt 文件
Hello
1.apple
2.banana
```

- 读取成功时：

err存的是**null**，

data存的是目标文件的内容

```bash
# 终端显示
null

--------

Hello
1.apple
2.banana
```

- 读取失败时：

err中的存是错误对象，错误原因存放在**err.massage**

data中存的是**undefined**

```bash
[Error: ENOENT: no such file or directory, open './011111111.txt'] {
  errno: -2,
  code: 'ENOENT',
  syscall: 'open',
  path: './011111111.txt'
}

--------

undefined
```

---

#### 判断文件读取成功或失败

因为，

文件读取成功时，回调函数的参数err存的是**null**，

文件读取失败时，回调函数的参数errerr中的存是错误对象

所以可以通过判断err是否为**true**（不是null，是个包含错误信息的对象），

来判断文件读取的成功或失败

```js
const fs = require('fs')

fs.readFile('./0111111.txt', 'utf8', function (err, data) {
    if (err) {
        console.log('文件读取失败');
        console.log(err.message);
    } else {
        console.log('文件读取成功');
        console.log(data);
    }
})
```

```bash
# 读取失败时
文件读取失败
ENOENT: no such file or directory, open './0111111.txt'
```

---

#### 读取HTML页面文件

结合后面的path模块，可读取HTML页面文件中的所有结构内容

```js
const fs = require('fs')
const path = require('path')

fs.readFile(path.join(__dirname, './index.html'), 'utf8', function (err, data) {
    if (err) {
        console.log(err.message);
    } else {
        console.log(data);
    }
})
```



### fs.writeFile()

**只能用来创建文件不能创建目录**，不然会报错路径错误

重复调用fs.writeFile()时，**新内容会覆盖旧内容**

```js
fs.writeFile('文件路径', 写入内容, ['编码格式'], 回调函数)
```

- 文件路径（必须）

  字符串形式

- 写入内容（必须）

- 编码格式（可选）

  表示以什么编码格式写入内容，

  常用 **utf8**

- 回调函数（必须）

  内容写入完后执行

  **err**：写入失败的错误对象，错误原因存放在**err.massage**

---

#### 返回结果

```js
const fs = require('fs')

const content = 'Hello'
fs.writeFile('./02/txt',content,'utf8',function(err){
    console.log(err);
})
```

- 文件写入成功时：

err是**null**

```js
null
```

- 文件写入失败时：

err是错误对象，错误原因存放在**err.massage**

```js
[Error: ENOENT: no such file or directory, open './02/txt'] {
  errno: -2,
  code: 'ENOENT',
  syscall: 'open',
  path: './02/txt'
}
```

---

#### 判断文件写入成功或失败

因为，

文件写入成功时，err是**null**，

文件写入失败时，err是错误对象

所以可以通过判断err是否为**true**（不是null，是个包含错误信息的对象），

来判断文件读取的成功或失败

```js
const fs = require('fs')

const content = 'Hello'
fs.writeFile('./02/txt',content,'utf8',function(err){
    if(err){
        console.log('文件写入失败');
        console.log(err.message);
    }else{
        console.log('文件写入成功');
    }
})
```

```bash
# 读取失败时
文件写入失败
ENOENT: no such file or directory, open './02/txt'
```



### 练习实例

获取文件内容进行整理，然后存入新的文件

```js
// 整理前的内容
andy=99 Tom=80 Jerry=95 Jame=40

// 需要整理成如下，然后存入新文件：
andy : 99 
Tom : 80 
Jerry : 95 
Jame : 40
```

1. 先fs.readFile()读取
2. 然后处理，字符串——>数组，替换字符后——>字符串
3. 最后fs.writrFile()写入新文件

```js
const fs = require('fs')

fs.readFile('./01.txt', 'utf8', function (err, data) {
    if (err) {
        console.log('读取失败', err.message);
    } else {
        console.log('读取成功', data);
        // 1. 转为数组
        const oldData = data.split(' ');
        // 2. 替换 = 为 ：， 并存入新数组
        const newData = oldData.map(item => {
            return item.replace('=', ':')
        })
        // 3. 新数组转为字符串，并换行
        const resData = newData.join('\r\n');
        // 4. 调用函数写入文件
        fun(resData)
    }
})

function fun(content) {
    fs.writeFile('./学生成绩.txt', content, function (err) {
        if (err) {
            console.log('写入失败', err.message)
        } else {
            console.log('写入成功');
        }
    })
}
```



### 相对路径导致动态拼接问题

使用fs模块操作文件时，

如果fs模块的方法中的**path属性**的路径，写的是**相对路径**的 **./** 或 **../** 

会以执行Node.js命令的当前所在目录路径后，动态拼接上被操作文件的路径

很容易出现路径动态拼接错误问题，导致找不到文件

所以，

Node.js自动拼接路径错误，是因为path属性的路径写的是相对路径

所以为了避免路径拼接错误，path属性的路径应该写为 **绝对路径**

即从目标文件的盘符开始写



### __dirname

因为绝对路径是从目标文件的盘符开始写，

且不同系统的层级 \ /不同，

太麻烦又不利于维护

所以不应该自己手写文件存放路径，

而是采用Node.js 提供的 **__dirname** (双下划线) 获取文件所在目录，然后拼接文件名

**以后路径path属性都要用__dirname拼接绝对路径**

```js
console.log(__dirname)

__dirname + '/文件名'
```

如下：

```js
const fs = require('fs')

fs.readFile(__dirname + '/01.txt', 'utf8', function (err, data) {
    if (err) {
        console.log('文件读取失败');
        console.log(err.message);
    } else {
        console.log('文件读取成功');
        console.log(data);
    }
})
```

```js
const fs = require('fs')

const content = 'Hello'
fs.writeFile(__dirname + '/02.txt', content, 'utf8', function (err) {
    if (err) {
        console.log('文件写入失败');
        console.log(err.message);
    } else {
        console.log('文件写入成功');
    }
})
```







## path 路径模块

path模块是Node.js提供的内置模块

可用于**路径操作**

- **path.join()**

  将多个路径片段 拼接为一个字符串路径，

  代替不正规的 +号拼接字符串的方法

- **path.basename()**

  获取路径字符串中的文件名

- **path.extname()**

  获取路径字符串中文件的扩展名

---

在JS文件中向使用path模块，要先通过require导入模块

```js
const path = require('path')
```



### path.join()

用于拼接字符串的路径

**Node.js的所有的路径拼接全部都要用path.join()**，而不是用 +拼接字符串

路径片段用逗号分隔，返回值是拼接后的路径字符串

```js
const path = require('path')

console.log(path.join('/路径1','/路径2','/路径3'));
/*
	/路径1/路径2/路径3
*/
```

---

#### ../ 抵消路径

**../**  抵消掉前面的一层路径片段

```js
const path = require('path')

console.log(path.join('/路径1','/路径2','../','/路径3'));
/*
	/路径1/路径3
*/


console.log(path.join('/路径1','/路径2','../../','/路径3'));
/*
	/路径3
*/


console.log(path.join('/路径1','/路径2/路径3','../../','/路径4'));
/*
	/路径1/路径3
*/
```

---

#### 拼接文件绝对路径

结合 **__dirname** 拼接文件绝对路径

且，path.join() 在拼接相对路径时会自动去掉相对路径前面的  **.**

```js
const path = require('path')

console.log(path.join(__dirname, '/文件.txt'));
console.log(path.join(__dirname, './文件.txt'));

/* 
/Users/chen/StudyPractice/JS/path/文件.txt
/Users/chen/StudyPractice/JS/path/文件.txt
*/
```

如下：

```js
const fs = require('fs')
const path = require('path')

fs.readFile(path.join(__dirname,'文件.txt'), 'utf8', function (err, data) {
    if (err) {
        console.log('文件读取失败');
        console.log(err.message);
    } else {
        console.log('文件读取成功');
        console.log(data);
    }
})
```



### path.basename()

可以获取一字符串段路径中的最后一部分

常用来获取绝对路径中的**文件名**

```js
path.basename('路径', ['.文件后缀'])
```

- 文件后缀

  没有该参数时（默认），返回值是文件名+后缀名

  有该参数时，返回值会仅文件名不包含后缀名

```js
const path = require('path')

const fullPath = '/Users/chen/StudyPractice/JS/path/文件.html';

console.log(path.basename(fullPath, '.html'))
// 文件.html

console.log(path.basename(fullPath, '.html'))
// 文件名
```



### path.extname()

可以获取一字符串段路径中的扩展名部分

常用来获取绝对路径中的文件的**后缀名**

```js
const path = require('path');

const fullPath = '/Users/chen/StudyPractice/JS/path/文件.html';

console.log(path.extname(fullPath));
// .html
```

结合**path.basename()**

```js
const fullPath = '/Users/chen/StudyPractice/JS/path/文件.html';

const ext = path.extname(fullPath)

console.log(path.basename(fullPath, ext));
// 文件
```





### 练习实例

拆分一个完整 HTML文件，

拆分为HTML文件、CSS文件、JS文件，并存放到指定目录下

并通过外链方式将拆分出的CSS文件、JS文件引入拆分出的HTML文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            padding: 0;
            margin: 0;
            box-sizing: border-box;
        }
        #title {
            font-size: 20px;
            color: red;
        }
    </style>
</head>
<body>
    <div id="title"></div>

    <script>
        const h = document.getElementById('title');
        h.innerHTML = 'Hello';
    </script>
</body>
</html>
```

1. 通过**正则表达式**

   匹配获得完整HTML页面文件中的 **<style\>标签** 和 **<script\>标签**

2. 通过fs模块读取完整的页面文件
3. html页面文件内容中替换去掉正则匹配的内容 <style\>标签 和 <script\>标签
4. 通过fs模块将正则匹配的内容替换掉首尾标签后分别写入HTML文件、CSS文件

[拆分完整HTML页面为.html , .css , .js文件](https://github.com/BlaxBerry/Demo_Node.js/tree/master/splitPage)







## http模块

http模块是Node.js提供的内置模块

用来创建web服务器

Node.js中不需要通过IIS，Apache等web服务器软件，

通过内置http模块的代码手写一个服务器软件，提供web服务

- **http.createServer()**

  可将一个普通电脑变为可以向外提供Web资源的Web服务器

---

在JS文件中向使用http模块，要先通过require导入模块

```js
const http = require('http')
```



### 创建web服务器

1. 导入http模块

2. 创建web服务器实例

3. 给服务器实例绑定request事件监听客户端请求

4. 监听端口启动服务器

   终端中用node打开该JS文件

```js
const http = require('http')

const server = http.createServer()

server.on('request', (req, res) => {
    console.log('有客户端来访问服务器了');
})

server.listen(3000, () => {
    console.log('服务器运行在http://127.0.0.7:3000');
})
```



### server.on()

**server.on()** 中给服务器绑定的**request事件处理函数**

只要服务器收到了客户端请求，

就可以通过request事件获得客户端相关的数据

---

#### req请求对象

包含客户端相关的数据和属性

- **req.url** 

  是客户端请求的的**url地址**

- **req.method**

  客户端请求的方式

```js
server.on('request', (req)=>{
  console.log('有客户端来访问服务器了');
  
  console.log(req.url);
  console.log(req.method);
})
```

---

#### res响应对象

包含服务端相关的数据和属性

- **res.end**

  向客户端响应指定内容，并结束该次请求的处理

```js
server.on('request', (req, res)=>{
  console.log('有客户端来访问服务器了');
  
  res.end('<h1>Hello</h1>')
})
```

- **res.setHeader**

  设置响应头

```js
// 可用于的设置编码格式来解决res.end() 乱码
res.setHeader('Content-Type','text/html; charset=utf-8')
```

```js
server.on('request', (req, res)=>{
  console.log('有客户端来访问服务器了');
  
  res.setHeader('Content-Type','text/html; charset=utf-8')
  
  res.end('<h1>你好</h1>')
})
```



### 简单路由

根据客户端请求URL地址的不同，响应不同的页面内容

```js
const http = require('http')

const server = http.createServer()

server.on('request', (req, res) => {

    console.log('有客户端来访问服务器了');


    // 获取 客户端发送的URL请求地址
    const url = req.url
    // 设置默认响应内容 404
    let responseContnet = '<h1>404 Not Found</h1>'

    // 判断 客户端发送的URL请求地址
    if (url === '/' || url === '/index.html') {
        responseContnet = '<h1>index 首页</h1>'
    } else if (url === '/about.html') {
        responseContnet = '<h1>About 页面</h1>'
    }

    // 设置 响应头的编码格式
    res.setHeader('Content-Type', 'text/html; charset=utf-8');
    // 响应页面内容
    res.end(responseContnet)
})

server.listen(3000, () => {
    console.log('服务器运行在http://127.0.0.7:3000');
})
```





### 项目存入服务器

浏览器访问地址即为文件存放路径

```js
|-项目文件根目录
	|- index.html
	|- inex.css
	|- index.js
|- server.js
```

- 文件在服务器中存放路径：

```http
/项目文件根目录/index.html
/项目文件根目录/index.css
/项目文件根目录/index.js
```

- 浏览器中资源访问的地址：

```http
/项目文件根目录/index.html
/项目文件根目录/index.css
/项目文件根目录/index.js

# 当访问了index.html时，若发现里面有外链引入的CSS文件JS文件，浏览器会默认去请求
```

---

#### 根据请求URL响应指定存放路径的文件

- 文件在服务器中存放路径：

```http
/项目文件根目录/index.html
/项目文件根目录/index.css
/项目文件根目录/index.js
```

- 浏览器中资源访问的地址：

```http
/项目文件根目录/index.html
/项目文件根目录/index.css
/项目文件根目录/index.js

# 当访问了index.html时，若发现里面有外链引入的CSS文件JS文件，浏览器会默认去请求
```

将浏览器访问地址映射为文件存放路径

直接将浏览器访问地址映射为文件存放路径，

在浏览器的文件资源的**请求URL地址前面拼接上文件资源绝对路径**

```js
path.join(__dirname, '/文件资源根目录', 浏览器请求URL地址)
```

如下：

```js
const http = require('http')
const path = require('path')
const fs = require('fs')

const server = http.createServer()

server.on('request', (req, res) => {
    // 获取 请求URL
    const url = req.url
    // 请求URL 映射为文件存放路径
    const filePath = path.join(__dirname,'/项目文件存放目录',url)

		// 读取相应文件并响应给客户端
    fs.readFile(filePath, 'utf8', (err,data)=>{
        if(err){
            return res.end('404 NOT FOUND')
        }else{
            res.end(data)
        }
    })
   
})

server.listen(3000, () => {
    console.log('server running at http://127.0.0.1:3000');
})
```

---

#### 优化完善资源请求路径

直接将用户访问的URL地址映射为文件地址做法

但，该做法仅适合用户在浏览器地址栏中输入的是文件资源绝对路径

即需要用户手动写上 **/文件存放根目录 **这个层级

```http
http://localhost:3000/文件存放根目录/index.html
```

若用户访问的路径是不完整的

即访问的是根目录，或访问URL里不带有**/文件存放根目录** 层级的**index.html**

会导致查不到该路径的资源，无法访问到文件

```http
http://localhost:3000
```

```http
http://localhost:3000/index.html
```

如下：

```js
|-项目文件根目录
	|- index.html
	|- inex.css
	|- index.js
|- server.js
```

```js
const http = require('http')
const path = require('path')
const fs = require('fs')

const server = http.createServer()

server.on('request', (req, res) => {
    // 获取 请求URL
    const url = req.url
    // 请求URL 映射为文件存放路径
    // const filePath = path.join(__dirname,'/test',url)
    let filePath = ''
    if (url === '/') {
        filePath = path.join(__dirname, './test/index.html')
    } else {
        filePath = path.join(__dirname, '/test', url)
    }

    console.log(filePath);

    fs.readFile(filePath, 'utf8', (err, data) => {
        if (err) {
            return res.end('404 NOT FOUND')
        } else {
            res.end(data)
        }
    })

})

server.listen(3000, () => {
    console.log('server running at http://127.0.0.1:3000');
})
```





## querystring模块

Node.js的内置模块，

专门用来处理查询字符串

通过**parse()** 将字符串转为对象格式

```js
const qs = require('querystring')

app.get('/', (req, res) => {
    res.send(qs.parse('name=andy&age=100'))
})
```

```json
{
    "name": "andy",
    "age": "100"
}
```





## 模块化

### 模块化概念

将一个大文件拆分为多个独立且相互有依赖的小文件（模块）

- 提高了代码的维护性、复用性
- 可实现模块的按需加载



### Node.js中模块的分类

- 内置模块（内置的fs、path、http等）

- 自定义模块（用户自己写的每个JS文件）
- 第三方模块（也叫**包**，需要下载引入）



### CommonJS规范

Node.js 遵循CommonJS的模块化规范：

- 每个模块内部的module变量代表当前模块
- module变量是个对象，其exports属性（module.exports）是对我接口
- require() 用于加载模块，加载的是该模块的module.exports属性的内容



### CommonJS - 加载模块

通过 **require() **加载执行其他模块

内置模块和第三方模块直接写名字，

自定义模块要写文件路径

```js
// 内置模块
const fs = require('fs') 

// 自定义模块
const calculate = require('./calcaulat.js')

// 第三方模块
const xxx = require('xxx')
```

---

**require() ** 是被导入模块的**module对象中的exports属性**的内容，

因为module.exports默认是个空对象 **{ }** ，

所以require()默认导入一个空对象 **{ }** 

```js
// 导出成员的模块
let a = 10 + 20

let b = 0
setTimeout(() => {
    b = 10 + 10
}, 1000)

let c = function () {
    console.log("hello");
}

module.exports.a = a
module.exports.b = b
module.exports.c = c
```

```js
// 导入自定义模块的JS文件
const m1 = require('./01')
console.log(m1);
// { a: 30, b: 0, c: [Function: c] }
m1.c()
// Hello
```



### 模块作用域

在自定义模块中定义的变量、方法

只能在当前模块内被访问、使用

即使模块被导入也无法直接访问使用其中的变量方法

模块作用域的限制有利于防止全局变量污染



### CommonJS - 暴露模块成员

因为有模块作用域的限制，

模块只被导入使用其中的变量、方法无法被访问使用，还需要在模块中将其对外暴露（共享）

---

#### module对象

每个 JS文件中都有一个**module对象**，存储了和当前模块相关的信息

其中的 **exports 属性** 可以向外暴露共享模块内成员，默认是个空对象 **{ }**

```js
console.log(module);


Module {
  id: '.',
  path: '/Users/chen/StudyPractice/JS/req',
  exports: {},
  parent: null,
  filename: '/Users/chen/StudyPractice/JS/req/02.js',
  loaded: false,
  children: [],
  paths: [
    '/Users/chen/StudyPractice/JS/req/node_modules',
    '/Users/chen/StudyPractice/JS/node_modules',
    '/Users/chen/StudyPractice/node_modules',
    '/Users/chen/node_modules',
    '/Users/node_modules',
    '/node_modules'
  ]
```

---

#### module.exports对象

在自定义模块中，

可以将模块内的变量、方法挂载到**module.exports对象** ，

将模块内部的私有成员向外暴露分享出去供其他模块使用

module.exports默认是个空对象 **{ }**

```js
console.log(module.exports);
// {}
```

```js
let a = 10 + 20

let b = 0
setTimeout(() => {
    b = 10 + 10
}, 1000)

let c = function () {
    console.log("hello");
}


module.exports.a = a
module.exports.b = b
module.exports.c = c
```

---

#### exports对象

但因为module.exports写起了麻烦，Node提供了exports对象

exports对象的内容默认指向以module.exports对象的内容

最终模块对外暴露的内容永远以module.exports指向的对象为准

```js
console.log(module.exports === exports);
// true
```

```js
let a = 10 + 20

let b = 0
setTimeout(() => {
    b = 10 + 10
}, 1000)

let c = function () {
    console.log("hello");
}


exports.a = a
exports.b = b
exports.c = c
```

![](https://pbs.twimg.com/media/E3V1z7gVoBYDogY?format=jpg&name=medium)

module.exports默认是个空对象，exports默认也是个空对象

**通过exprots.属性的方式挂在模块成员，可以反映到module.exports指向的对象的内容**

**直接让 exports等于一个新对象，并不会修改module.exports指向的对象的内容**

所以，ES6对象的健值同名时的简写只能用于module.exports

为了防止混乱，导出模块是不要混用





### 模块的加载机制

#### 优先缓存加载

模块都是优先缓存中加载

模块在第一次被 require()加载后会被缓存，

即使多次调用require() 导入相同模块也不会导致模块内代码重复被多次执行，

```js
const xxx = require('xxx')
const xxx = require('xxx')
const xxx = require('xxx')
```

---

#### 加载的是目录路径

把目录作为模块的导入地址时：

1. 优先加载**package.jsonz的main属性**指定的入口文件

2. 若没有就根据指定路径，加载路径下的 index.js文件
3. 若都没有，终端报错

---

#### 内置模块加载机制

Node.js的**内置模块的加载优先级最高**

假若同时加载重名的自定义模块、第三方模块、内置模块，

导入的永远是内置模块

---

#### 自定义模块加载机制

- 需要通**过路径标识符** **./** 或 **../** 指定模块路径

  若不指定路径只写某块名，会被当作内置模块或第三方模块

- 会自动补后缀名

  若导入时没有指定自定义模块的的后缀名

  Node.js会自动按顺序补上后缀名，然后加载：

  1. 优先补上.js的后缀名，然后加载

  2. 若没找到带有.js后缀名的文件，就补上 .json 然后加载

  3. 若还没找到.json后缀名的文件，就补上 .node 然后加载

  4. 若都没找到，就在终端报错

---

#### 第三方模块加载地址

require()导入第三方模块时：

1. 优先从的当前文件的父目录开始，

   查找node_modules文件夹中第三方模块

2. 若查找不到要加载的第三方模块，

   就会逐层向上一级目录中查找加载，

   直到文件系统的根目录

3. 若都查不到，终端报错

如下：

导入名为moment的第三方模块时：

```
C:\User\test\project\node_modules\moment
C:\User\test\node_modules\moment
C:\User\project\node_modules\moment
C:\node_modules\moment
报错
```

