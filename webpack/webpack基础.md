# webpack基础

![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/94/Webpack.svg/1200px-Webpack.svg.png)

Webpack，模块打包机，侧重于模块打包

开发中的所有图片、JS文件、CSS文件等都可被看作模块

主要通过loader（记载器）和plugins（插件）对资源进行处理



## 安装和使用

### 生成项目依赖文件

通过npm初始化项目的package.json文件

```bash
npm init -y
```



### 安装依赖包

安装 `webpack` 和 `webpack-cli` 到 `开发依赖`

```bash
npm install webpack webpack-cli --save-dev
```

安装 `jquery `到 `生产依赖`

```bash
npm install jquery --save
```



```js
{
  "name": "项目名",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.41.6"，
    "webpack-cli": "^3.3.11"
  },
  "dependencies": {
    "jquery": "^3.4.1"
  }
}
```



比如，安装引入jQuery

并控制html文件的DOM样式

```js
//引入jquery
import $ from jquery

$('DOMElement:even').css("background":"red")
$('DOMElement:odd').css("background":"blue")
```

但是文件HTML无法识别import

只能通过webpack编译打包



### 编译

编译压缩

```bash
webpack 项目主文件 -o/指定文件
```

文件被放入项目目录下的dist文件夹中

```js
项目
|--dist
|		|--bundle.js
|--node_modules
|--package.json
|--index.js
```

如下：

webpack index.js -o/bundle/js

把项目主文件编译压缩并输出到bundle.js文件中

```js
{
  "name": "项目名",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "自定义命令":"webpack index.js -o/bundle/js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.41.6"，
    "webpack-cli": "^3.3.11"
  },
  "dependencies": {
    "jquery": "^3.4.1"
  }
}
```

```bash
npm run 自定义命令名 
```





全局下载webpack后才能使用webpack指令

如果是局部下载只下载到了当前项目中，

则命令行中无法直接使用webpack命令，会报错提示找不到命令

只能在package.json文件的`scripts`属性中配置自定义指令后运行

```bash
npm run 自定义命令名 
```



### 热加载（热重载）

js文件被webpack编译后，html文件读取的文件就变味编译后的指定文件了

所以直接只在原来的js文件中修改内容的话，页面不会直接读取

所以需要重新通过webpack再执行一次编译，麻烦

可使用 `webpack-dev-server`的热加载

只要有修改过内容就自动编译内容



###  webpack-dev-server

下载安装 `webpack-dev-server`

```bash
npm install webpack-dev-server --save-dev
```



```js
{
  "name": "项目名",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "自定义命令":"webpack-dec-server index.js -o/bundle/js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.41.6"，
    "webpack-cli": "^3.3.11"
  },
  "dependencies": {
    "jquery": "^3.4.1"
  }
}
```