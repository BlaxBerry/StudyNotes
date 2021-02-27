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

- 然后加载文件(也可以重启终端)

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





## Node.js运行js

### node交互模式

终端中直接输入node

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
```

---

### 电脑的终端

![](https://treehouse.github.io/installation-guides/mac/imgs/node-install1.png)

---

### VSCode的终端

在VScode的在集成终端中打开js文件

![](https://img2020.cnblogs.com/blog/1423526/202009/1423526-20200923154306800-803285096.png)

---

### VSCode插件Code Runner

![](https://kotlin-code.com/wp-content/uploads/code_runner_visual_studio_code_plugin.png)





但是注意：

Node.js只能解读ECMAScript和核心API(系统模块)，不能解读JS的DOM和BOM

```js
let a = document.querySelector('window');
console.log(a);
//#=>
/Users/chen/StudyPractice/JS/01.js:1
let a = document.querySelector('window');
        ^

ReferenceError: document is not defined
    at Object.<anonymous> (/Users/chen/StudyPractice/JS/01.js:1:9)
    at Module._compile (internal/modules/cjs/loader.js:1063:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)
    at Module.load (internal/modules/cjs/loader.js:928:32)
    at Function.Module._load (internal/modules/cjs/loader.js:769:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:72:12)
    at internal/main/run_main_module.js:17:47
```