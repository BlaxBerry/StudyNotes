# Node.js

![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2018/12/node-icon-960x504.jpg)

## 简介

Nodejs可以实现

- 静态资源服务器

- 路由处理

- 动态网站

- 模版引擎

- get、post参数传递和处理

---

Node.js是机遇chromeV8引擎的JavaScript运行环境。

就是用来运行JS的编译软件。

以前的JS只能运行在浏览器上，只能通过html文件来实现运行。

有了Node.js后就可通过node运行js文件



### 可用于后端

- 可以做Web服务器
- 命令行工具
- 网络爬虫
- 桌面应用开发（Electron）
- app
- 嵌入式
- 游戏



### 普通下载（不建议）

Mac可直接从官方下载文件然后安装

[Node.js官网](https://nodejs.org/ja/)

![](http://www.w3big.com/nodejs/download-page.jpg)

---

或者通过 homebrew命令，下载最新版本

```bash
brew install node
```

---

不建议 上述的两种下载，推荐**版本管理器下载**的方式





## NVM

**NVM**（Node.js Version Manager）**Node.js的版本管理器**

不同版本的Node.js会有改变，为了防止出错，

开发时用的什么版本，运行时就用什么版本

同一个项目有时也会要多个版本的Node.js

所以需要一个管理Node.js版本的管理器

---

### NVM的下载步骤

**下载NVM前必须先删除已经下载的Node.js**

```bash
brew uninstall node
```

- 首先打开终端，进入当前用户的 `home` 目录中

```bash
cd ~
```

- 显示所有文件（夹）,查看有没有 `.zshrc` 这个文件

```bash
ls -a
```

- 如果没有，则在此新建

```bash
touch ./zshrc
```

- 然后利用brew下载NVM

```bash
brew install nvm
```

- 把下列内容复制粘贴到 `～/.zshr`文件中

```
  export NVM_DIR="$HOME/.nvm"
  [ -s "/usr/local/opt/nvm/nvm.sh" ] && . "/usr/local/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/usr/local/opt/nvm/etc/bash_completion.d/nvm" ] && . "/usr/local/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion
```

```bash
open ~/.ashrc
```

- 然后加载文件（或者重启终端）

```bash
source ~/zshrc
```

- 最后检查NVM是否下载成功可以执行

```bash
nvm
```

会出现 `Node Version Manager` 帮助文档，这表明安装成功

或用`nvm -v`检测nvm的版本

```bash
nvm -v
0.37.2
```



[Github下载即问题解决 ](https://github.com/nvm-sh/nvm)

>- Since macOS 10.15, the default shell is `zsh` and nvm will look for `.zshrc` to update, none is installed by default. Create one with `touch ~/.zshrc` and run the install script again.
>- If you use bash, the previous default shell, run `touch ~/.bash_profile` to create the necessary profile file if it does not exist.
>- You might need to restart your terminal instance or run `. ~/.nvm/nvm.sh`. Restarting your terminal/opening a new tab/window, or running the source command will load the command and the new configuration.



### NVM的下载位置

NVM的位置是：`/Users/用户名/.nvm/`



### 使用NVM下载Node.js

下载指定版本的Node.js

```bash
nvm install 版本号

nvm install 14.16.0
```

验证Node.js的版本

```
node -v
v14.16.0
```



### 使用NVM切换Node.js版本

```bash
nvm use 版本号

nvm use 14.16.0
Now using node v14.16.0 (npm v6.14.11)
```

验证Node.js的版本

```
node -v
v14.16.0
```



### NVM的常用命令

```bash
nvm ls-remote  # 列出所有可安装的版本

nvm ls  # 列出所有已经安装的版本

nvm install <version>  # 安装指定的版本

nvm uninstall <version> # 卸载指定的版本

nvm use <version>  # 切换使用指定的版本

nvm alias default <version> # 设置默认 node 版本

nvm deactivate  # 解除当前版本绑定
```





## 查看安装位置

nvm会将各个版本的node安装在`~/.nvm/versions/node`目录下

---

### 在终端中

使用`which node`查看NVM下载的Node所在位置

```bash
which node

/Users/用户名/.nvm/versions/node/版本/bin/node
```

---

### 使用Mac的Finder

打开Finder

按下`command + shift + g` 

输入 `~/.nvm/versions/node/` 打开文件夹，

按下`command + shift + .`开关显示隐藏文件和文件夹

如下：

![](https://segmentfault.com/img/bVbk8AE)





## node环境运行js

### node交互模式

终端中直接输入`node`，就可在命令行工具中运行JS命令。

仅用于快速验证某一个简单结论

```bash
~ % node
Welcome to Node.js v14.16.0.
Type ".help" for more information.
> var a = 10
undefined
> var b = '10'
undefined
> a == b
true
> a === b
false
> a + 1
11
> b + 1
'101'
> .exit
~ %
```

---

### 电脑的终端

![](https://treehouse.github.io/installation-guides/mac/imgs/node-install1.png)

---

### VSCode的终端

在VScode的`在集成终端中打开`运行js文件

![](https://img2020.cnblogs.com/blog/1423526/202009/1423526-20200923154306800-803285096.png)

---

### VSCode插件Code Runner

![](https://kotlin-code.com/wp-content/uploads/code_runner_visual_studio_code_plugin.png)



但是注意：

Node.js只能解读ECMAScript和核心API(系统模块)，不能解读JS的DOM和BOM

```js
let a = document.querySelector('window');
console.log(a);
//报错
ReferenceError: document is not defined
```





## 全局对象 global

### 简介

全局对象是JavaScript中一个特殊对象，在程序任何地方都可以访问。

例如，在浏览器JavaScript中全局对象是`window`。

在**Node.js中的全局对象是`global`**， 

在node中没有window。

如下，交互模式下验证：

```bash
node
> window
Uncaught ReferenceError: window is not defined
```

如下，终端中运行js文件验证：

```js
console.log(window)
//报错
ReferenceError: window is not defined
```



### 全局变量

全局对象和其属性也称为全局变量。

Node.js中 **所有的全局变量都是global的属性**（成员），

比如setTimeout，console等

如下，交互模式下验证：

```bash
node
> global
<ref *1> Object [global] {
  global: [Circular *1],
  clearInterval: [Function: clearInterval],
  clearTimeout: [Function: clearTimeout],
  setInterval: [Function: setInterval],
  setTimeout: [Function: setTimeout] {
    [Symbol(nodejs.util.promisify.custom)]: [Function (anonymous)]
  },
  queueMicrotask: [Function: queueMicrotask],
  clearImmediate: [Function: clearImmediate],
  setImmediate: [Function: setImmediate] {
    [Symbol(nodejs.util.promisify.custom)]: [Function (anonymous)]
  }
}
```

如下，终端中运行js文件验证：

```js
console.log(global);
//#->
<ref *1> Object [global] {
  global: [Circular *1],
  clearInterval: [Function: clearInterval],
  clearTimeout: [Function: clearTimeout],
  setInterval: [Function: setInterval],
  setTimeout: [Function: setTimeout] {
    [Symbol(nodejs.util.promisify.custom)]: [Function (anonymous)]
  },
  queueMicrotask: [Function: queueMicrotask],
  clearImmediate: [Function: clearImmediate],
  setImmediate: [Function: setImmediate] {
    [Symbol(nodejs.util.promisify.custom)]: [Function (anonymous)]
  }
}
```

---

Node.js中声明的全局变量变量，**不会挂载到globa对象下**，如下：

```js
let a = 10;
console.log(global.a);  //undefined
```

而在浏览器的JavaScript中，

声明的全局变量则会挂载到window对象下，成为window对象的属性。

如下：

```html
<body>
    <script>
        var a = 10;
        console.log(window.a);
        console.log(a === window.a);
    </script>
</body>
//
10
true
```



### 添加成员

可以向global添加成员（属性），

使添加的变量（成员）可在任何地方使用。如下：

```js
global.a = 20;
console.log(a); //20
```





## 模块化开发

为了方便维护和提高复用，还可以避免命名冲突

把函数分类分组分到不同文件中，减少每个文件的代码量。

一个js文件就是一个模块。

### 模块分类

Node.js**系统内置模块**、**自定义模块**（文件）、**第三方模块**

---

模块的作用域是私有的，必须通过导入导后出才能在外部使用。

不同于ES6的模块化规范，

Node.js 采用 **CommonJS** 的 Modules规范，

用`exports`和`moudule.export`导出，

用`require`导入接口



### 导入  require

通过`require()`方法导入模块，导入的是整个文件模块

并用一个常量接收，常量名一般和模块名一致

```js
const 常量 = require('模块的路径.js')
//或省略文件后缀
const 常量 = require('模块的路径')
```



### 导出  exports

通过`exports`把整个文件模块对象化，

要在外部被使用内容作为`exports对象属性`随着该对象导出。

即**导出的是个对象，要别外部使用的是对象的属性**。

如下，变量num作为了整个模块也就是`exports对象`的一个属性

```js
//模块导出文件 01.js

let num = 10;
exports.num = num;

console.log(num);      // 10
console.log(exports);  // {num: 10}
console.log(exports.num); // 10
```

用`require()`导入的是指定模块文件导出的`exports`对象，

即**导入的是个对象，需要的是随着该对一起导出的的属性**。

```js
//模块导入文件 02.js

const m = require('./01.js');

console.log(m);   // {num: 10}
console.log(m.num); // 10
```

---

一个文件要导出多个内容时，

就把这些内容都作为要导出的`exports对象`的属性。

如下：

```js
//模块导出文件 01.js

let num = 10;
let fn = (x, y) => x + y;
class Animal {
    constructor() {}
    static say() {
        console.log('hello');
    }
}

exports.num = num;
exports.fn = fn;
exports.Animal = Animal;

console.log(exports);
// { num: 10, fn: [Function: fn], Animal: [class Animal] }
```

```js
//模块导入文件 02.js

const m = require('./01.js');

console.log(m);
// { num: 10, fn: [Function: fn], Animal: [class Animal] }
console.log(m.num);
// 10
console.log(m.fn);
// [Function: fn]
console.log(m.fn(10, 20)); 
// 30
console.log(m.Animal);
// [class Animal]
m.Animal.say()
// hello
```



### 导出  module.exports

作为`exports`对象属性导出的场合，若外部使用的数据过多，

也就意味导出时要写大量重复的exports对象的属性赋值代码，

可使用`module.exports`对象

```js
module.exports = {数据，数据，数据，......}
```

导出还是一个对象，还是作为该对象的属性。

可理解为是**简写的集中导出**，如下：

```js
//模块导出文件 01.js

let num = 10;
let fn = (x, y) => x + y;
class Animal {
    constructor() {}
    static say() {
        console.log('hello');
    }
}

module.exports = {
    num,
    fn,
    Animal
}

console.log(module.exports);
//// { num: 10, fn: [Function: fn], Animal: [class Animal] }
```

导入文件`require`接收到的还是整个`module.exports`对象

需要的是随着该对一起导出的的属性

```js
//模块导入文件 02.js

const m = require('./01.js');

console.log(m);
// { num: 10, fn: [Function: fn], Animal: [class Animal] }
console.log(m.num);
// 10
console.log(m.fn);
// [Function: fn]
console.log(m.fn(10, 20)); 
// 30
console.log(m.Animal);
// [class Animal]
m.Animal.say()
// hello
```



### exports 和 module.exports

`exports` 是 `module.exports` 的引用

二者在文件中的指向是一致的

```js
console.log(exports);  // {}
console.log(module.exports);  // {}

console.log(exports === module.exports);  //true
```

```js
let a = 10;
exports.a = a

console.log(exports);  // { a: 10 }
console.log(module.exports);  // { a: 10 }

console.log(exports === module.exports);  //true
```

但用 `module.exports` 导出模块对象时，指向不一样：

```js
let a = 10;
module.exports = { a }

console.log(exports); // {}
console.log(module.exports); // { a: 10}

console.log(exports === module.exports); //falses
```

所以在使用 `module.exports` 导出时一定要在后面添加上：

```js
exports = module.exports
```

---

`exports`仅在文件中才有，交互模式下没有

```bash
> node
> exports
Uncaught ReferenceError: exports is not defined
> module.exports
{}
```





## this指向

### 交互模式中的this

在node交互模式下，**this指向的是全局对象global**

```bash
~% node
> this
<ref *1> Object [global] {
  global: [Circular *1],
  clearInterval: [Function: clearInterval],
  clearTimeout: [Function: clearTimeout],
  setInterval: [Function: setInterval],
  setTimeout: [Function: setTimeout] {
    [Symbol(nodejs.util.promisify.custom)]: [Function (anonymous)]
  },
  queueMicrotask: [Function: queueMicrotask],
  clearImmediate: [Function: clearImmediate],
  setImmediate: [Function: setImmediate] {
    [Symbol(nodejs.util.promisify.custom)]: [Function (anonymous)]
  }
}
>
>
> this === global
true
```

---

### 模块中的this

用终端运行js文件时，js文件中的this不指向全局对象global，

**this指向当前模块**的导出对象`exports对象`，也就是这个js文件

若当前文件没有导出的内容时，是个空对象，如下：

```js
console.log(this);
// {}

console.log(this === global);
// false

console.log(this === exports);
// true

console.log(this === module.exports);
// true
```

```js
let a = 10;
exports.a = a

console.log(this);  // { a: 10 }

console.log(this === exports); // true

console.log(this === module.exports); //true
```





## 内置模块简介

就是Node.js提前准备好的系统模块，

直接导入就可以使用其中的方法了

[Node.js v14.16.0 API文档](http://nodejs.cn/api/)

### 常用的内置模块

| 模块名      | 含义                            |
| ----------- | ------------------------------- |
| fs          | 文件操作                        |
| path        | 路径操作                        |
| http        | 网络操作                        |
| url         | url解析                         |
| querystring | 查阅参数解析（GET请求中的参数） |

### 导入

```js
const fs = require('fs');
const path = require('path');
const http = require('http');
const url = require('url');
const querystring = require('querystring');
```





## path 内置模块

### 特殊变量  __dirname

`__dirname`可获得当前js文件的绝对路径，但不包含该文件名

```js
console.log(__dirname);
// /Users/chen/StudyPractice/JS
```

---

### 特殊变量  __filename

`__filename`可获得当前js文件的绝对路径，包括该文件名

```js
console.log(__filename);
// /Users/chen/StudyPractice/JS/01.js
```



### path.parse()

解析路径

以**对象形式**返回路径匹配的文件的盘符，目录，文件名，后缀名

```js
const path = require('path');

let parseobj = path.parse('nodememo.js');
console.log(parseobj)
//#->
{ root: '', 
  dir: '', 
  base: 'nodememo.js', 
  ext: '.js', 
  name: 'nodememo' }
```

---

#### path.extname()

了解即可，获取文件的后缀名（扩展名）

```js
const path = require('path');

let extname = path.extname('o1.js');
console.log(extname);  
// .js
```

---

#### path.basename()

了解即可，获取文件名，包含后缀名

```js
const path = require('path');

let name = path.basename('o1.js');
console.log(name);   
// 01.js
```

---

#### path.dirname()

了解即可，获得文件的当前所在路径

```js
const path = require('path');

let dirname = path.dirname('o1.js');
console.log(dirname);
// .
```



### path.join()

拼接路径

```js
path.join('目录名','目录名','目录名')

path.join('Notes','Node','01.js')
// /Notes/Node/01.js
```

---

#### 和 __dirname 连用

获取当前文件同级目录下的文件的**完整路径**

如下：

```js
const path = require('path');

let fullname = path.join(__dirname, 'o1.js');
console.log(fullname);
//  /Users/chen/StudyPractice/JS/o1.js
console.log(__dirname);
//  /Users/chen/StudyPractice/JS
```

---

获取当前文件同级目录下的文件夹中的文件的**完整路径**

如下：

```js
const path = require('path');

let fullname = path.join(__dirname, 'notes', '00.html');
console.log(fullname);
//  /Users/chen/StudyPractice/JS/notes/00.html
console.log(__dirname);
//  /Users/chen/StudyPractice/JS
```





## fs 内置模块

文件操作

常用path模块搭配，实现文件的操作

---

### 检查是文件还是目录

#### fs.stat()

```js
fs.stat(PATH,(er,data)=>{})
```

回调函数有 err 和 data 参数

若路径是文件，err是个空，data是文件对象；

若路径是个文件夹，err返回错误对象，data是undefined

如下，根据参数err是否是空打印不同结果

```js
const fs = require('fs');

fs.stat('../03', (err, data) => {
    if (err) {
        console.log(err);
        console.log('it`s dir');
        return;
    }

    console.log('it`s file');
})
```

---

---

### 创建目录

#### fs.mkdir()

**常用**

```js
fs.mkdir(PATH/NAME,(err)=>{})
```

如果文件已经存在，会报错并不会进行操作

若创建成功，回调函数中err参数为空

若创建失败，err是错误对象

如下：根据判读err是否为空得知是否创建成功

```js
const fs = require('fs');

fs.mkdir('./dir', (err) => {
    if (err) {
        console.log(err);
        return;
    }
    console.log('succeed');
})
```



### 写入替换文件

#### fs.writeFile()

了解即可，保存主要是保存到数据库

若文件不存在，则会创建文件并写入

如果文件存在，会**覆盖替换**该文件

```js
fs.writeFile(PATHNAME, content, 'utf-8', (err) => {})
```

```js
const fs = require('fs');
const path = require('path');
let fullpath = path.join(__dirname, '01.txt');

let content = 'hello world'
fs.writeFile(fullpath, content, 'utf-8', (err) => {
    if (err) {
        console.log(err);
        return
    }
    console.log('job finished');
})
```

---

验证异步写入不阻塞：

```js
const fs = require('fs');
const path = require('path');
let fullpath = path.join(__dirname, '01.txt');

let content = 'hello world'
fs.writeFile(fullpath, content, 'utf-8', (err) => {
    if (err) {
        console.log(err);
        return
    }
    console.log('job finished');
})
console.log('----end');

/*
----end
finished
*/
```



### 写入追加文件

#### fs.appendFile()

如果文件存在，会在内容后面追加写入内容

若文件不存在，则会创建文件并写入

```js
fs.appendFile(PATH/NAME,content,(err)=>{})
```

如下，运行一次就给文件内容追加一次

```js
const fs = require('fs');

fs.appendFile('./dir/01.css', 'body{color:red}\n', (err) => {
    if (err) {
        console.log(err);
        return;
    }
    console.log('succeed');
})
```



### 读取目录

#### fs.readdir()

返回指定路径下的所有文件和文件夹列表

```js
fs.readdir(PATH,(err.datd)=>{})
```

以**数组的形式**返回读取的列表

```js
const fs = require('fs');

fs.readdir(__dirname, (err, data) => {
    if (err) {
        console.log(err);
    }
    console.log(data);
})
```

```js
['dir01','dir02','file01.js','file02.js']
```



### 异步读取文件

#### fs.readFile()

**重要**

```js
fs.readFile(PATH,["utf-8"],(error,data)=>{  })
```

没有阻塞，不用等文件都读取完毕就执行后面的代码

需要借助回调函数等读取文件信息完毕后执行回调函数

**获得的是个buffer数据**，需要加utf-8字符集如下：

```js
const fs = require('fs');
const path = require('path');
let fullpath = path.join(__dirname, '01.txt');

fs.readFile(fullpath, (err, data) => {
    console.log(err);
    console.log(data);
    console.log(data.toLocaleString());
});

// null
// <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
// hello
```

使用了utf-8：

```js
const fs = require('fs');
const path = require('path');
let fullpath = path.join(__dirname, '01.txt');

fs.readFile(fullpath, 'utf-8', (err, data) => {
    console.log(err);
    console.log(data);
});

// null
// hello world
```

若读取文件时出错：

```bash
[Error: ENOENT: no such file or directory, open 	'/Users/chen/StudyPractice/JS/01.txt'] {
  errno: -2,
  code: 'ENOENT',
  syscall: 'open',
  path: '/Users/chen/StudyPractice/JS/01.txt'
}

undefined
```

---

一般使用异步读取时，都是要判断的，**并用`return`终止**：

```js
const fs = require('fs');
const path = require('path');
let fullpath = path.join(__dirname, '01.txt');

fs.readFile(fullpath, 'utf-8', (err, data) => {
    if (err) {
        console.log('there is error');
        return
    }
    console.log(data);
});

// there is error
```

---

验证异步读取不阻塞：

```js
const fs = require('fs');
const path = require('path');
let fullpath = path.join(__dirname, '01.txt');

fs.readFile(fullpath, 'utf-8', (err, data) => {
    if (err) {
        console.log('there is error');
        return
    }
    console.log(data);
});
console.log('---end');

/*
---end
hello world
*/
```



### 重命名 移动文件

#### fs.rename()

```js
fs.renameSync(oldPATH/NAME，newPATH/NAME,()=>{}
```

- **重命名**

路径不变，文件名改变：

```js
const fs = require('fs');

fs.rename('./dir/01.css', './dir/02.css', (err) => {
    if (err) {
        console.log('failed');
        console.log(err);
    }
    console.log('succeed');
})
```

- **移动**

路径改变，文件名不变：

```js
const fs = require('fs');

fs.rename('./01.css', './dir/01.css', (err) => {
    if (err) {
        console.log('failed');
        console.log(err);
    }
    console.log('succeed');
})
```

- **移动 + 重命名** 

路径不变，文件名改变：

```js
const fs = require('fs');

fs.rename('./01.css', './dir/02.css', (err) => {
    if (err) {
        console.log('failed');
        console.log(err);
    }
    console.log('succeed');
})
```



### 删除目录

#### fs.rmdir()

```js
fs.rmdir(PATH,(err)=>{})
```

**如果目录下有文件，则目录不会被删除**

**应该先删除该目录下的所有文件后，才能删除该目录**

如下，使用方便理解的回调地狱：

```js
const fs = require('fs');

fs.readdir('./dir', (err, data) => {
    if (err) {
        return;
    }；

    data.forEach(item => {
        fs.unlink('./dir/' + item, err => {
            if (err) {
                return;
            }；

            fs.rmdir('./dir', err => {
                if (err) {
                    return;
                }；
                console.log('succedd');
            })
        })
    })
})
```



### 删除文件

#### fs.unlink()

```js
fs.unlink(PATH/NAME,(err)=>{})
```

```js
const fs = require('fs');

fs.readdir('./01.css', (err) => {
   if (err) {
        console.log('failed');
        console.log(err);
    }
    console.log('succeed');
})
```





### 案例练习

1. **判断服务器中过是否有某文件，**

   **如果没有就创建，没如果有就不做操作**

利用 `fs.readdir`  +  `arr.includes()`  +  `fs.mkdir()`

比如，判断是否有upload文件夹：

```js
const fs = require('fs');

fs.readdir(__dirname, (err, data) => {
    if (err) {
        console.log(err);
        return;
    }

    if (data.indexOf('upload') == -1) {
        fs.mkdir('./upload', (err) => {
            if (err) {
                console.log(err);
                return;
            }
            console.log('created a directoy');
        })
    } else {
        console.log('directory is already here');
    }
})
```

2. 整理归类目录和文件

   **异步处理**

比如：把01目录下的所有目录放入一个数组，所有文件放入另一个数组

```js

```



3. **修改当前目录下的所有文件名**

- 添加前缀

```js
const fs = require('fs');
const path = require('path');

changname = 'node-';
let fileList = fs.readdirSync(__dirname);

fileList.forEach(item => {
    if (item.endsWith('.js')) {
        fs.renameSync(item, changname + item);
    }
})
```

- 删除前缀

```js
const fs = require('fs');
const path = require('path');

let fileList = fs.readdirSync(__dirname);
console.log();

changname = 'node-';

fileList.forEach(item => {
    if (item.endsWith('.js')) {
        fs.renameSync(item, item.substring(changname.length))
    }
})
```







### 从文件流中读取数据

#### fs.createReadStream()



异步

## Async，await、promise

```js
const fs = require('fs')

var dirs = [];
var files = [];
fs.readdir(__dirname, (err, data) => {
    if (err) {
        console.log(err);
        return;
    }

    data.forEach(item => {
        fs.stat(item, (err, data) => {
            if (err) {

                dirs.push(item)
            } else {

                files.push(item)
            }

        })
        console.log(dirs);
        console.log(files);
    })
})
```





```js
var p = new Promise(function(resolve, reject) {
    setTimeout(function() {
        let num = 100;
        resolve(num)
    }, 1000)
})
p.then(function(a) {
    console.log(a);
})
```

```js
function getTimer(resolve, reject) {

    setTimeout(function() {
        let num = 100;;
        resolve(num);
    }, 1000);
};

var p = new Promise(getTimer);

p.then(function(a) {
    console.log(a);
})
```







async

让方法变成异步



await

等待一个异步方法执行的完成

















## process

### process.argv

**命令行交互**

收集文件执行时，命令行的所有参数，放入一个数组

```js
// js文件 03.js

console.log(process.argv);
```

在终端命令行中运行该文件：

```js
% node 03.js 
[
 '/Users/chen/.nvm/versions/node/v14.16.0/bin/node',
  '/Users/chen/StudyPractice/JS/03.js'
]
```

---

运行时又在命令行中输入其他参数：

```js
% node 03.js 10 20 30
[
  '/Users/chen/.nvm/versions/node/v14.16.0/bin/node',
  '/Users/chen/StudyPractice/JS/03.js',
  '10',
  '20',
  '30'
]
```

---

获取和操作命令行输入的参数：

```js
//js文件 03.js

console.log(process.argv);
console.log('--------');
console.log(`我是在命令行中输入的${process.argv[2]}`);
console.log(`我是在命令行中输入的${process.argv[3]}`);
console.log(process.argv[2] + process.argv[3]);
```

```js
% node 03.js 10 20
[
  '/Users/chen/.nvm/versions/node/v14.16.0/bin/node',
  '/Users/chen/StudyPractice/JS/03.js',
  '10',
  '20'
]
--------
我是在命令行中输入的10
我是在命令行中输入的20
1020
```

---

---

### process.arch

获取系统的位数

```bash
～% node
> process.arch
'x64'
```





## Buffer数据类型

[菜鸟教程 Node.js Buffer](https://www.runoob.com/nodejs/nodejs-buffer.html)

JavaScript 语言自身只有字符串数据类型，没有二进制数据类型。

但在处理像TCP流或文件流时，必须使用到二进制数据。

因此在 Node.js中定义了一个 Buffer 类，

用来创建专门存放二进制数据的缓存区。

![ASCII码表](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpic2.zhimg.com%2Fv2-7150b4417cd849b2a00292c4f4d26ac9_b.jpg&refer=http%3A%2F%2Fpic2.zhimg.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1617116217&t=bfb9b4741fd226eceb6bf30188c69088)

---

创建buffer对象，转为字符串数据

```js
let buf = Buffer.from([97]);
console.log(buf);
// <Buffer 61>
console.log(buf.toString());
// a
```

```js
let buf = Buffer.from('node');
console.log(buf);
// <Buffer 6e 6f 64 65>
console.log(buf.toString());
// node
```



## 包

[npm](./npm.md)

项目需要一个**入口文件**，比如app.js/index.js

还需要有一个**package.json**文件

---

### package.json文件

存放了当前项目所需的各种模块信息

package.json文件必须存放在项目的顶层目录下

```json
{
  "name": "blog",
  "version": "1.0.0",
  "description": "- public 静态资源文件",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  }
}
```

name： 项目文件目录名

version： 项目版本

main：入口文件

keywords： 关键字，搜索用

dependencies：生产环境的依赖包

devDependencies：开发环境的依赖包

^ : 第一位版本号不变，后面两位取最新

~ : 前面两位不变，后面一个取最新

*：全部取最新

 

