# webpack基础

![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/94/Webpack.svg/1200px-Webpack.svg.png)

Webpack，模块打包机，侧重于模块打包

开发中的所有图片、JS文件、CSS文件等都可被看作模块

主要通过loader（记载器）和plugins（插件）对资源进行处理



## 安装和使用

### 1.生成项目依赖文件

通过npm初始化项目的package.json文件

```bash
npm init -y
```

---

### 2.安装依赖包

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



### 3.创建主文件

创建项目 `index.js` 主文件

```js
//引入jquery
import $ from jquery

//$('DOMElement:even').css("background":"red")
//$('DOMElement:odd').css("background":"blue")
```



webpack index.js -o/bundle/js

输出文件到bundle.js文件中



全局下载webpack后才能使用webpack指令

如果是局部下载，只下载到了当前项目中，则命令行无法使用webpack命令，会报错提示找不到改命令

只能在package.json文件的`scripts`属性中配置指令

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