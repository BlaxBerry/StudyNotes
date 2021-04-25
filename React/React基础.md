# React 基础

<img src="https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2019/07/react.jpg" style="zoom:50%;" />

## 简介

- **声明式开发**

  面向数据编程，不需要直接去操控DOM，只需要操纵数据

- **虚拟DOM**

  不会重新渲染DOM树，仅将变化的部分同步

- **使用 JSX语法**

  可以在 JS中书写HTML

- **组件化开发**

- **单项数据流**

  父组件——>子组件，子组件接受数据使用

  但子组件不能通过 this.props 修改父组件的数据

- **灵活**

  React仅操控主页面中的一个DOM容器（脚手架中是“**root**”）

  其他容器仍可以使用其他jQuery等框架





## 基本使用步骤

先安装

webpack、

webpack-cli、

webpack-dev-derver

编译处理高级JS的语法import

[Webpack 4.X 基本用法](https://github.com/BlaxBerry/StudyNotes/blob/master/webpack/webpack%204.X%E5%9F%BA%E7%A1%80.md)

---

### 1、安装包

在项目目录下安装 **react**、**react-dom**

```bash
npm install react react-dom --save
```

- react：专门创建组件和虚拟DOM，

  创建组件、虚拟DOM、生命周期

- react-dom：专门操作DOM，

  把创建好的虚拟DOM放到页面上渲染

  比如	`React.render()`

---

### 2、创建容器

在**index.html**页面中，创建容器

使用React创建的虚拟DOM元素都会被渲染到这个指定的容器中

```html
<div id="app"></div>
```

---

### 3、导入

项目入口文件中导入 **React** 、 **ReactDOM**

```js
import React from "react"
import ReactDOM from "react-dom"
```

---

### 4、创建虚拟DOM元素

第一个参数是要创建的标签

第二个参数是null，或者是对象形式的属性和属性值

第三个及以后的参数是子节点，可以写n个同级的子节点（标签或文本）

```js
const 虚拟DOM名 = React.createElement(
  "标签类型名",
  {
    // 对象形式的元素属性 或 null
  },
  "子节点",// 其他虚拟DOM 或 文本内容
  "子节点",
  "子节点"
)
```

```js
// <h1 title="hello" id="myh1">Hello World</h1>
const myh1 = React.createElement(
  'h1',
  {
    title:'hello',
    id:'myh1'
  },
  'Hello World'
)
```

---

### 5、渲染虚拟DOM

就是把虚拟DOM渲染到页面中的一个容器中显示

第一个参数是要渲染到页面的虚拟DOM

第二个参数必须是个DOM对象

```js
ReactDOM.render(
  要渲染到页面的虚拟DOM,
  指定的页面上的一个容器（一个DOM元素）
)
```

```js
ReactDOM.render(
  myh1,
  document.getElementById('app')
)
```





## HelloWorld

```js
import React from "react"
import ReactDOM from "react-dom"


const myh1 = React.createElement(
    'h1', null, 'Hello World'
);

let target = document.getElementById('app')
ReactDOM.render(myh1, target)
```





## 虚拟DOM

```js
// JS原生DOM
let docDOM = document.createElement("div")

// React虚拟DOM
let reactDOM = React.createElement("div")
```

React虚拟DOM更加简介，内容除去了不需要的DOM对象属性，渲染时也会更加高效

---

### 虚拟DOM的属性

```js
import React from "react"

const mydiv = React.createElement(
  "div",
  {
    id:"box",
    index:1
  }
)
```

一般是使用 JSX语法：

```jsx
import React from "react"
import ReactDOM from "react-dom"

const mydiv = (
  <div id="father" index=1></div>
)

ReactDOM.render(father, document.getElementById('app'))
```

---

### 虚拟DOM的子节点嵌套

**React.createElement( )** 创建虚拟DOM时候，可创建n个子节点

若要实现页面中如下的嵌套关系DOM树：

```html
<div id="root">
  <div id="father">
    i am father
    <div id="son">
      i am son
    </div>
  </div>
</div>
```

```jsx
import React from "react"
import ReactDOM from "react-dom"

const mydiv = React.createElement(
    "div",
    {id:"father"},
    "i am father",
    React.createElement(
        "div",
        {id:"son"},
        "i am son"
    )
)

ReactDOM.render(father, document.getElementById('app'))
```

或者写入一个变量中：

```js
import React from "react"
import ReactDOM from "react-dom"

const son = React.createElement(
    'div', null, 'i am son'
);

const father = React.createElement(
    'div', null, 'i am father', son
)

ReactDOM.render(father, document.getElementById('app'))
```

但是每次创建一个DOM节点就需要调用一次React.createELement()

书写麻烦，所以一般是使用 **JXS语法**：

```jsx
import React from "react"
import ReactDOM from "react-dom"

const mydiv = (
  <div id="father">
    i am father
    <div id="son">
      i am son
    </div>
  </div>
)


ReactDOM.render(father, document.getElementById('app'))
```







## JSX语法

JavaScript中不能直接写入HTML，会报错

React中的 JSX语法（符合XML的JS）可以在JS中混合写入HTML









### babel编译

使用 **babel** 转换 JS 中的 HTML 标签

#### 1、安装babel插件

- **babel-core**
-  **babel-loader** 
- **babel-plugin-transform-runtime** 

```bash
npm install babel-core babel-loader babel-plugin-transform-runtime -D
```

- **babel-preset-env**
- **babel-preset-stage-0**

```bash
npm install babel-preset-env babel-preset-stage-0 -D
```

---

#### 2、安装 babel-present-react

安装能识别转换JSX语法的包 

```bash
npm install babel-present-react -D
```

---

#### 3、添加第三方load规则

Webpack只能打包处理 .js 后缀名的文件

webpack.config.js文件中添加第三方配置规则

```js
module.exports = {
    mode: 'development',
    devtool: 'inline-source-map',
    module: {
        // 第三方匹配规则
        rules: [{
            test: /\.js|jsx$/,
            use: 'babel-loader',
            exclude: /node_modules/
        }]
    }
}
```

#### 4、添加.babelrc 配置文件

```js
{
    "presets": ["env","stage-0","react"],
    "plugins": ["transform-runtime"]
}
```

JXS语法 不是直接把HTML标签哪去显示，

而是在 **运行时被转换为 React.createElement()** 来执行

























## 

