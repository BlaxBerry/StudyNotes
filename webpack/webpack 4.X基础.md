#  Webpack 4.X 基础

<img src="https://hiroshi-yokota.com/wp-content/uploads/2019/10/webpack.png" style="zoom:50%;" />

## 简介

Webpack是一个前端项目构建工具（**打包工具**）

用于解决Web开发时候的困境：

- 文件依赖关系复杂
- 静态资源请求效率低
- **模块化支持不友好**
- **浏览器对高级JavaScript的特性兼容效率低** 

---

目前前端项目都使用Webpack进行打包构建

Webpack提供了 如下等功能：

- **模块化支持**

- **代码压缩混淆**

- 处理**JS的兼容问题**

- 性能优化

![](https://jsstudygroup.github.io/jsStudyBlog/assets/images/post/what-is-webpack.png)





## 浏览器 JS 的兼容性问题

浏览器HTML无法识别兼容高级JS语法，

比如模块化开发时的引入 **import** XXX **from** XXX，

对模块化开发及其不友好，

只能考虑通过webpack俩编译打包JS文件，是浏览器能够识别

如下：

以模块化安装引入jQuery，去实现页面DOM元素换行变色

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

8. 所以只能通过webpack编译打包处理兼容性问题





## Webpack安装与配置

1. 安装 **webpack** 和 **webpack-cli** 到开发依赖 --save-dev

```bash
npm install webpack webpack-cli -D
```

2. 项目根目录下创建 **webpack.config.js** 配置文件

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

   文件被放入项目目录下的dist文件夹中

   ```js
   项目
   |--dist
   		|--main.js
   |--node_modules
   |--src
   		|--index.html
   		|--index.js
   |--package.json
   |--webpack.config.js
   ```

6. 将html文件中导入的JS文件换为webpack打包处理后的文件

   即导入 **`dist / main.js`**

   浏览器页面可识别JS语法，页面有显示了





## 配置出入口 JS 文件

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





## 自动打包 

html页面中引入被webpack打包处理后的JS出口文件后，

浏览器页面才会显示效果

若只直接修改原来index.JS文件，并没有再次运行webpack打包处理

则页面并不会有变化

即，**每次修改完代码后，都要重新打包处理**

这样会很麻烦，所以需要自动打包功能



### 自动打包工具

1. 安装 **webpack-dev-server**打包工具

```bash
npm install webpack-dev-server -D
```

2. 修改package.json 文件中scripts的运行webpack的脚本

```js
"scripts": {
  "dev": "webpack-dev-server"
},
```

3. 运行webpack打包工具打包处理

```bash
npm run dev
```

4. 然后在浏览器中访问 **localhost:8080**，即可看到实时的结果



###  --open 设置自动开启浏览器

给脚本添加 **--open**

```js
"scripts": {
  "dev": "webpack-dev-server --open"
},
```

然后运行打包处理：

```bash
npm run dev
```

默认是开启chorme浏览器，

也可设置为开启其他浏览器，如下，打开火狐浏览器：

```js
"scripts": {
  "dev": "webpack-dev-server --open firefox"
},
```



###  --port 设置端口号

给脚本添加 **--port 自定义端口号**

```js
"scripts": {
  "dev": "webpack-dev-server --open --port 3000"
},
```



### --host 设置域名

给脚本添加 **--host 自定义域名**

```js
"scripts": {
  "dev": "webpack-dev-server --open --host127.0.0.1 --port 3000"
},
```



### --hot





### 运行报错 与 版本号

若运行 webpack-dev-server 打包处理时报错

```bash
Error: Cannot find module 'webpack-cli/bin/config-yargs'
```

可以下载指定版本

```js
 "dependencies": {
        "jquery": "^3.6.0",
        "webpack": "^4.1.1",
        "webpack-cli": "^2.0.12",
        "webpack-dev-server": "^3.1.1"
    }
```





## webpack-plugin

在内存中自动生成 index页面的插件

```bash
npm install webpack-plugin -D
```

在 webpack.config.js 中配置：

```js
const path = require('path');

//引入插件
const HtmlWebPackPlugin = require('html-webpack-plugin');

// 创建插件实例对象
const = htmlPlugin = new  HtmlWebPackPlugin({
  // 源文件
  template: 		
  path.join(__dirname,'./src/index.html'),
  // 生成的内存中的首页
  filename：'index.html'
})

module.exports = {
  mode:"develpoment",
  plugin:{
    htmlPlugin
  }
}
```

src / index.html 中不用再通过script标签引入打包后的文件了

webpack-plugin插件会自动把打包好的JS文件引入给页面文件





