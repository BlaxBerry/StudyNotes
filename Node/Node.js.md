# Node.js

![](https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2018/12/node-icon-960x504.jpg)

## 简介

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





## 查看Node.js的位置

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

---

node的 `lib` 中 `node_modules` 文件夹可以看到模块 

![](https://segmentfault.com/img/bVbk8AP)

在 nvm下 node 版本管理方式，

安装的模块不是公用的，也就是说在切换版本后需要在切换的版本下重新安装       





## node运行js

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

Node.js系统内置模块、自己写的模块（文件）、第三方模块

---

模块的作用域是私有的，必须通过导入导后出才能在外部使用。

不同于ES6的模块化规范，

Node.js 采用`CommonJS 的 Modules`规范，

用`exports`和`moudule.export`导出，`require`导入接口



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





## node中的this指向

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





## Node.js的内置模块简介

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





## path模块



### 特殊变量  __dirname

`__dirname`可获得当前js文件的绝对路径，但不包含该文件名

```js
console.log(__dirname);
// /Users/chen/StudyPractice/JS
```



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

