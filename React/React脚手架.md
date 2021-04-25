#  React脚手架

<img src="https://staging-qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F484272%2F52cfad2d-ef8c-b70d-3aa5-ec79b0576737.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1bb6e4bac443e2d9d368f6b114b249a2" style="zoom:50%;" />



## 安装

### 1、全局安装脚手架

```bash
npm install create-react-app -g
```

---

### 2、创建项目

```bash
create-react-app 项目名称
```

```bash
create-react-app myreact
```

创建完毕后会提示：

```bash
Success! Created myreact at /Users/chen/StudyPractice/myreact
Inside that directory, you can run several commands:

#  yarn start
    Starts the development server.

#  yarn build
    Bundles the app into static files for production.

#  yarn test
    Starts the test runner.

#  yarn eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

#  cd 项目目录
#  yarn start

Happy hacking!

```

---

### 3、运行项目

```bash
cd 项目目录
yarn start
```

```bash
cd demo
yarn start
```

运行项目后，终端界面会提示：

浏览器地址输入域名`localhost:3000`即可

```bash
Compiled successfully!

You can now view react-demo in the browser.

#  Local:            http://localhost:3000
#  On Your Network:  http://192.168.1.2:3000

Note that the development build is not optimized.
To create a production build, use yarn build.
```

<img src="https://staging-qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F484272%2F52cfad2d-ef8c-b70d-3aa5-ec79b0576737.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1bb6e4bac443e2d9d368f6b114b249a2" style="zoom:33%;" />





## 目录结构

```js
项目
|--node_modules
|--public
		|--index.html
|--src
		|--index.js
|--package.json
|--.gitignore
|--ReadME.md
```





## 使用

首先要清空默认的src目录

然后在src目录中新建项目入口文件 index.js

然后在index.js文件中

### 1、引入核心模块

用ES6的语法引入 **React** 和 **ReactDOM**

- react：用来创建虚拟DOM
- react-dom：用来将创建的虚拟DOM渲染到页面

```js
import React from "react"
import ReactDOM from "react-dom"
```

---

### 2、创建虚拟DOM

使用React的 **JSX语法**书写HTML

若VSCode报错，只需在右下方把语言模式从Javascript 换为 Javascript React

```jsx
const mydiv = <div>HELLO REACT</div>
```

---

### 3、渲染创建的虚拟DOM

通过 **ReactDOM.render()**

把虚拟DOM渲染到 `public / index.html`中的**root容器**

```jsx
ReactDOM.render(虚拟DOM，要渲染的HTML页面中的DOM元素)
```

```html
<body>
  <div id="root"></div>
</body>
```

```jsx
ReactDOM.render(
    <div>Hello React</div>,
    document.getElementById('root')
)
```

---

```jsx
// 引入核心模块
import React from "react"
import ReactDOM from "react-dom"

// 把内容渲染到id为root的标签
ReactDOM.render( 
    <div>Hello React</div>,
    document.getElementById("root")
)
```









## 组件化开发

入口文件要保持简洁，所以虚拟DOM要写入组件

### 1、创建组件文件

#### ES6 Class创建组件

可以使用ES6的 **class类** 创建组件

return只能返回一个根标签，

但是该跟标签内可以有多个并列关系的标签

```jsx
// 组件
// 1. 引入核心模块
import React from "react"


// 2. 定义组件
class App extends React.Component {
    // 渲染
    render() {
        return (
          <div>
            <p>i am component App</p>
          </div>
        )
    }
}

// 3. 导出组件
export default App
```

---

### 2、导入组件

将组件导入 **index.js** 入口文件

并在 **ReactDOM.renders** 中使用

如果多个组件，需要放入一个根标签

```jsx
import React from "react"
import ReactDOM from "react-dom"

// 导入组件
import App from "./App"
import App2 from "./App2"

ReactDOM.render( 
    <div>
        <App></App>
        <App2></App2>
    </div>,
    document.getElementById("root")
)
```

也可以把跟标签放入一个变量，

然后在 **ReactDOM.renders** 中使用该变量

```jsx
import React from "react"
import ReactDOM from "react-dom"


// 引入组件
import App from "./App.js"
import App2 from "./App2.js"

const comps = (
    <div>
        <App></App>
        <App2></App2>
    </div>
)

ReactDOM.render(
    comps,
    document.getElementById('root')
  )
```

---

---

### 模版样式

#### 1、行内样式

```jsx
import React from "react"

class App extends React.Component {

    render() {
        return (
            <div>
                <p style={{
                    color:"teal",
                    fontSize:"20px",
                    fontWeight:700
                }}>
                i am component App2
                </p>
            </div>
        )
    }
}

// 导出组件
export default App
```

---

#### 2、外链样式

在src目录下新建 **assets目录**

```js
src
|--assets
		|--css
				|--AppStyle.css
|--index.js
|--App.js
```

里面新建 **.css** 样式文件

```css
.box {
    color: teal;
    font-size: 20px;
    font-weight: 700;
}
```

组件文件中通过 **import** 导入  **.css** 样式文件

```jsx
import React from "react"

// 导入 CSS样式文件
import "./assets/App.css"

class App extends React.Component {
    render() {
        return (
            <div>
                <p 
                  className="box"
                  style={{
                    color:"teal",
                    fontSize:"20px",
                    fontWeight:700
                }}>
                i am component App2
                </p>
            </div>
        )
    }
}

export default App
```

---

---

### 引入图片

```jsx
import React from "react"

// 导入 图片地址
import Img from "./assets/react.jpeg"

class App extends React.Component {
    render() {
        return (
            <div>
                <p>Hello React</p>
                <img src={Img} alt="" width="400px"/>            
            </div>
        )
    }
}

export default App
```

---

---

### 表单

#### label标签的htmlfor属性

在HTML中是通过 **label 标签的for属性**绑定 input，从而实现光标聚焦

但是在 React的 JSX 中，是通过label 标签的 **htmlfor属性**

```jsx
import React from "react"

// 导入 图片地址
import Img from "./assets/react.jpeg"

class App extends React.Component {
    render() {
        return (
            <div>
                <label htmlFor="username">
                    <span>用户名：</span>
                    <input type="text" id="username"/>
                </label>         
            </div>
        )
    }
}

export default App

```



## JSX语法

React中使用 **JSX语法** 代替常规 JavaScript，

在React中 JSX 会被 Babel 编译为JavaSCript

### JSX的特点

- 执行更快
- 可以更方便书写**HTML**模版

- 通过大小写区分HTML标签和自定义组件

```jsx
// HTML 标签
<div></div>

// React 组件
<Div></Div>
```

---

### class/className，for/htmlFor

因为JSX语法还是 JS，

所以标签的 `class` 和 `for` 不能作为 XML 属性名

应该分别使用 `className` 和 `htmlFor` 作为替代

```jsx
let mydiv = (
  <div>
    	<div className="username"></div>
  		<label htmlFor="username">
    		<input type="text" id="username"/>
  		</label>
  </div>
)
```

---

### VSCode中的JSX语法提示

在设置中搜索`include Languages`, 然后进入`setings.json`，

在`emmet.includeLanguages`字段中添加：

```json
"emmet.includeLanguages": {
  "javascript": "javascriptreact"
}
```

---

### 虚拟DOM的HTML嵌套

只能有一个根标签不然会报错

```jsx
ReactDOM.render(
  <div></div>,
  document.getElementById('root')
)
```

但是可以一个跟标签内嵌套多个同级标签

```jsx
ReactDOM.render(
  <div id="father">
    <div id="son">son</div>
    <div id="son">son</div>
    <div id="son">son</div>    
  </div>,
  document.getElementById('root')
)
```

---

### { } JS表达式

可以在**{ }** 中书写JavaScript表达式

如果是数组，数组元素会自动展开（不带逗号）

```jsx
// 组件 App.js

import React from "react"

let arr =[1,2,3,4]

class App extends React.Component {
    render() {
        return (
            <div>
                <div>{arr}</div>   
                <div>{arr.join(",")}</div>
                <div>{arr[1]}</div>
                <div>{(arr[0] + arr[2])/2}</div>
            
            </div>
        )
    }
}

export default App
```

---

### {* *} 注释

```jsx
import React from "react"

class App extends React.Component {
    // 渲染
    render() {
        return (
            <div>
                {/* 我是注释，不显示 */}
            </div>
        )
    }
}

export default App
```

---

### 三元表达式

JSX中不能使用 if else 语法，只能使用**三元表达式**

```jsx
import React from "react"

let age = 18

class App extends React.Component {
    render() {
        return (
            <div>
                {age>=18?"成年":"未成年"}
            </div>
        )
    }
}

export default App
```



