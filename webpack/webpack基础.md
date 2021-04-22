# Webpack基础

<img src="https://hiroshi-yokota.com/wp-content/uploads/2019/10/webpack.png" style="zoom:50%;" />

## 简介

Webpack是一个前端项目构建工具（**打包工具**）

用于解决Web开发时候的困境：

- 文件依赖关系复杂
- 静态资源请求效率低
- 模块化支持不友好
- 浏览器对高级JavaScript的特性兼容效率低 

---

目前前端项目都使用Webpack进行打包构建

Webpack提供了 如下等功能：

- **模块化支持**

- **代码压缩混淆**

- 处理**JS的兼容问题**

- 性能优化

![](https://jsstudygroup.github.io/jsStudyBlog/assets/images/post/what-is-webpack.png)





## 浏览器 JS 的兼容性问题

如下：列表换行变色

1. 创建项目目录

2. 创建 **package.json** 包管理文件

```bash
npm init -y
```

3. 创建 **src目录**

4. scr 目录中创建 **index.html** 文件

```html
<script src="./index.js"></script>

<body>
    <ul>
        <li>只是第1个li</li>
        <li>只是第2个li</li>
        <li>只是第3个li</li>
        <li>只是第4个li</li>
        <li>只是第5个li</li>
        <li>只是第6个li</li>
        <li>只是第7个li</li>
        <li>只是第8个li</li>
        <li>只是第9个li</li>
        <li>只是第10个li</li>
    </ul>
</body>
```

5. 安装**jQuery**

```bash
npm i jquery -S
```

6. **模块化的方式导入 jQuery**，在JS文件中导入jQuery

   scr 目录中创建 **index.js** 文件，并导入 index.html

```js
import $ from "jquery"

//jQuery 入口函数
$(function() {
    $('li:odd').css('backgroundColor', 'teal')
    $('li:even').css('backgroundColor', 'red')
})
```

7. 浏览器中打开index.html文件后，样式不会改变

   因为浏览器无法识别 ES6的导入语法`import $ from "jquery"`，

   从而导致**兼容性问题**提示报错，

8. 所以需要通过webpack处理兼容性问题





## Webpack安装与配置

1. 安装 **webpack** 和 **webpack-cli** 到开发依赖 --save-dev

```bash
npm install webpack webpack-cli -D
```

2. 项目根目录下创建 **webpack.config.js** wenpack配置文件

   通过**mode节点**用来指定构建模式

```js
module.exports = {
  mode:'指定构建模式'
}
```

编译模式有**development**和**production**两种

3. 在package.json 配置文件中的scripts节点下

   新增运行脚本

```js
"scripts":{
  "dev":"webpack"
}
```

4. 运行 npm run 脚本，进行项目的打包

```bash
npm run dev
```

5. webpack打包后后在项目下生成 **dist目录**

   目录下有 **mian.js文件**，即对 index.js文件打包处理后的文件

   

6. 将html文件中导入的JS文件换为webpack打包处理后的文件

   即导入 **`dist / main.js`**

   浏览器页面可识别JS语法，页面有显示了





## 配置

### 默认 入口 与 出口

Webpack 4.x默认，

将 src目录下的 index.js文件打包处理为dist目录下的main.js文件

打包**入口文件** ：**src / index.js**

打包**出口文件** ：**dist / main.js**



### 自定义 入口 与 出口

若想修改打包的出口和入口文件，需要在webpack.config.js中配置：

通过 **entry节点** 和 **ouput节点**

```js
// 导入Node.js的path模块
const path = require('path')

module.exports = {
    entry: path.join(__dirname, './src/index.js'),
    output: {
      //输出文件的路径
        path: path.join(__dirname, "./dist"),
      // 输出文件名
        filename: 'bundle.js'
    },
    // 编译模式
    mode: "development"
}
```

然后运行webpack执行打包

html文件导入新的出口文件





















## 

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