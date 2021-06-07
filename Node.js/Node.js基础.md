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







## 执行JS文件

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

3. 通过fs模块分别写入HTML文件、CSS文件、JS文件

   