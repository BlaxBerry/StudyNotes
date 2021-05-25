# React脚手架

<img src="https://staging-qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F484272%2F52cfad2d-ef8c-b70d-3aa5-ec79b0576737.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1bb6e4bac443e2d9d368f6b114b249a2" style="zoom:50%;" />



## 安装 create-react-app

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

组件名必须大写

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





## JSX语法

React中使用 **JSX语法** 代替常规 JavaScript，

在React中 JSX 会被 Babel 编译为JavaSCript

JSX创建的看起来是HTML标签，但实际是虚拟DOM，而不是HTML

### JSX的特点

- 执行更快
- 可以更方便书写**HTML**模版

- 通过大小写区分HTML标签和自定义组件

  原生HTML标签用小写，自定义组件名用大写

```jsx
// HTML 标签
<div></div>

// React 组件
<Div></Div>
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

### { } JS表达式

可以在**{ }** 中书写JavaScript表达式，或者引用变量

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

---

### 数组 + 遍历生成元素

如下：通过 **arr.map()** 遍历生成 li

```jsx
import React from "react"

let arr =[1,2,3,4]

class App extends React.Component {
    render() {
        return (
            <div>
                <ul>
                    {
                      arr.map((item,index)=>{
                          return (
                            <li key={index}>我是{item}</li>
                          )
                      })
                    }
                </ul>
            </div>
        )
    }
}

// 导出组件
export default App
```





## 书写注意点

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





## 组件属性 this.props

### 组件数据父传子 

父组件通过**自定义属性**传入数据

子组件通过 **this.props.自定义属性名** 获取父组件传入的数据

```jsx
import React, { Component } from 'react'

class Bar extends Component {
    render(){
        return (
            <div style={{
                width:"100%",
                height:"50px",
                backgroundColor:this.props.bgc
                lineHeight:"50px",
                textAlign:"center",
            }}>
                {this.props.title}
            </div>
        )
    }
}

export default class App6 extends Component {
    render() {
        return (
            <div>
                <Bar title="头部标题" bgc="crimson"></Bar>
                主要内容
                <Bar title="底部内容" bgc="teal"></Bar>

            </div>
        )
    }
}
```

```jsx
import React, { Component } from 'react'

class Bar extends Component {
    render(){
        return (
            <div style={{
                width:"100%",
                height:"50px",
                backgroundColor:"#ccc",
                lineHeight:"50px",
                textAlign:"center",
            }}>
                {this.props.title}
            </div>
        )
    }
}

export default class App6 extends Component {
    constructor(props){
        super(props)

        this.state = {
            top: "头部标题",
            footer: "底部标题"
        }
    }

    render() {
        return (
            <div>
                <Bar title={this.state.top}></Bar>
                主要内容
                <Bar title={this.state.footer}></Bar>
            </div>
        )
    }

```



### 默认值

**static defaultProps** 中定义默认值，

若父组件有数据传入子组件，则子组件使用获取的数据

若父组件没有传入数据，则子组件使用自身的默认数据

```jsx
import React, { Component } from 'react'

class Son extends Component {
    static defaultProps = {
        bgc:"#ccc",
        title:"默认标题栏"
    }

    render(){
        return (
            <div style={{
                width:"100%",
                height:"50px",
                lineHeight:"50px",
                textAlign:"center",
                backgroundColor:this.props.bgc
            }}>
                {this.props.title}
            </div>
        )
    }
}

class Father extends Component {
    constructor(props){
        super(props)

        this.state = {
            top: "头部标题",
            footer: "底部标题"
        }
    }

    render() {
        return (
            <div>
                <Bar title={this.state.top} bgc="crimson"></Bar>
                
                <Bar/>

                <Bar title={this.state.footer} bgc="teal"></Bar>

            </div>
        )
    }
}


```



### 获取双标签子元素

当父组件中以双标签形式调用子组件时，

双标签中间的所有子节点（DOM节点/文本节点）

可在子组件中通过  **this.props.children** 获取

```jsx
import React, { Component } from 'react'

class Son extends Component {
    render(){
        return (
            <div className="son"> 
                {this.props.children}
            </div>
        )
    }
}

export default class Father extends Component {
    render() {
        return (
            <div className="father"> 
                <Son>子元素</Son>
            </div>
        )
    }
}
```



### 遍历

**key属性** 用于标记唯一

提高DOM渲染效率，不再是全部重新渲染，而是仅渲染修改变更的部分

```jsx
import React,{Component} from "react"

export default class App extends Component {
  constructor(props){
    super(props)
    
    this.state = {
      list:[
        {id:1,val:"数值1"},
        {id:2,val:"数值2"},
        {id:3,val:"数值3"}]
    }
  }
  
  render(){
    return (
      <div>
        <ul>
          {
            this.state.list.map((item,index)=>{
              return (
                <li key={item.id}>{item.val}</li>
              )
            })
          }
        </ul>
      </div>
    )
  }
}
```





## VSCode插件

![](https://miro.medium.com/max/1838/1*XgMBj0lGzZs7O6okKg5sFA.png)

**rcc + 回车** 快速生成React组件

```jsx
import React, { Component } from 'react'

export default class App3 extends Component {
    render() {
        return (
            <div>
                
            </div>
        )
    }
}
```





## React事件

### this指向

```jsx
import React, { Component } from 'react'

export default class App3 extends Component {

    handleClick(e){
        console.log("hello");
        console.log(this);      // undefined
        console.log(e.target);  // <button>DOM元素
    }  
  
    render() {
        return (
            <div>
                <button onClick={this.handleClick}>按钮</button>
            </div>
        )
    }
}
```

可通过**bind()** 来修改this的指向为组件

```jsx
import React, { Component } from 'react'

export default class App3 extends Component {

    handleClick(e){
        console.log(this);      // App3这个组件自己
        console.log(e.target);  // <button>DOM元素
    }  
  
    render() {
        return (
            <div>
                <button 
                  onClick={this.handleClick.bind(this)}>按钮</button>
            </div>
        )
    }
}
```

或者使用**箭头函数**

但是必须要传入e

```jsx
import React, { Component } from 'react'

export default class App3 extends Component {

    handleClick(e){
        console.log(this);      // App3这个组件自己
        console.log(e.target);  // <button>DOM元素
    }  
  
    render() {
        return (
            <div>
                <button 
                  onClick={(e)=>this.handleClick(e)}>按钮</button>
            </div>
        )
    }
}
```





### 动态数据

在**this.state状态属性**中定义动态属性

修改时，在事件中通过 **this.setState()**修改状态属性中定义的数据

```jsx
import React, { Component } from 'react'

export default class App3 extends Component {
    
    constructor(props){
        super(props)

        this.state = {

            num:20
        }
    }

    handleClick(){
        this.setState({
            num: this.state.num +1
        });
    }

    render() {
        return (
            <div>
                <h3>{this.state.num}</h3>
                <button onClick={this.handleClick.bind(this)}>点击+1</button>
            </div>
        )
    }
}
```



### 双向数据绑定

通过表单的value属性和onChange事件

```jsx
import React, { Component } from 'react'

export default class App4 extends Component {

    constructor(props){
        super(props)

        this.state = {
            val:10
        }

    }

    handleChange(e){
       	// console.log(e.target.value);
        this.setState({
            val:e.target.value
        })

    }


    render() {
        return (
            <div>
                <h3>{this.state.val}</h3>
                <input type="text" 
                    value={this.state.val} 
                    onChange={this.handleChange.bind(this)}
                />
            </div>
        )
    }
}
```



### 动态绑定样式(Tab栏切换)

```jsx
import React, { Component } from 'react'

export default class App5 extends Component {

    constructor(props){
        super(props)

        this.state = {
            num:1
        }
    }

    handleClick(number){
        // console.log(e.target.value);
        this.setState({
            num:number
        })
    }

    render() {
        return (
            <div>
                <div className="tab">
                    <div className="tab_btns">
                        <input type="button" 
                          className={this.state.num===1?"active":""} 
                          onClick={this.handleClick.bind(this,1)} 
                          value="1"/>
                        <input 
                          type="button" 
                          className={this.state.num===2?"active":""} 
                          onClick={this.handleClick.bind(this,2)} 
                          value="2"/>
                        <input type="button" 
                          className={this.state.num===3?"active":""} 
                          onClick={this.handleClick.bind(this,3)} 
                          value="3"/>
                    </div>
                    <div className="tab_cons">
                        <div className={this.state.num===1?"current":""}>内容1</div>
                        <div className={this.state.num===2?"current":""}>内容2</div>
                        <div className={this.state.num===3?"current":""}>内容3</div>
                    </div>
                </div>
            </div>
        )
    }
}
```

或者：

```jsx
import React, { Component } from 'react'

export default class App5 extends Component {

    constructor(props){
        super(props)

        this.state = {
            num:1
        }
    }

    handleClick(e){
        // console.log(e.target.value);
        this.setState({
            num:Number(e.target.value)
        })
    }

    render() {
        return (
            <div>
                <div className="tab">
                    <div className="tab_btns">
                        <input type="button" 
                          className={this.state.num===1?"active":""} 
                          onClick={this.handleClick.bind(this)} 
                          value="1"/>
                        <input type="button" 
                          className={this.state.num===2?"active":""} 
                          onClick={this.handleClick.bind(this)} 
                          value="2"/>
                        <input type="button" 
                          className={this.state.num===3?"active":""} 
                          onClick={this.handleClick.bind(this)} 
                          value="3"/>
                    </div>
                    <div className="tab_cons">
                        <div className={this.state.num===1?"current":""}>内容1</div>
                        <div className={this.state.num===2?"current":""}>内容2</div>
                        <div className={this.state.num===3?"current":""}>内容3</div>
                    </div>
                </div>
            </div>
        )
    }
}
```





## 规定父组件传递的数据的数据类型

实现在子组件内控制传入的props属性的数据类型

如果子组件收到的数据不是指定的数据类型，就会提示报错

```js
Warning: Failed prop type: Invalid prop `title` of type `number` supplied to `Father`, expected `string`.
```

### 1、安装 prop-types

```bash
npm install prop-types

或

yarn add prop-types
```

### 2、导入 prop-types

```jsx
import PropTypes from "prop-types"
```

### 3、指定数据类型

在static中指定父组件传递来的数据（自定义属性）的数据类型

```js
class Son extends Component{
  
    static propTypes = {
      
       父组件传入的属性:PropTypes.数据类型 
    }
}
```

如下：规定传入的 title自定义属性必须是字符串类型，不然会报错提示

```jsx
import React, { Component } from 'react'
import PropTypes from "prop-types"

class Son extends Component{
  
    static propTypes = {
        title:PropTypes.string
    }

    render(){
        return(
            <div>
                {this.props.title}
            </div>
        )
    }
}

export default class Father extends Component {
    render() {
        return (
            <div>
                <Son title={123}></Son>
            </div>
        )
    }
}
```



## props属性值的跨级传递

React的组件之间的数据传递是通过props属性

数据一级一级从上到下传递

1. 父组件通过**自定义属性**传入数据——>

2. 子组件中**this.props.自定义属性**接受数据——>
3. 子组件通过**自定义属性**传入数据给孙子组件——>
4. 孙子组件中**this.props.自定义属性**接受数据

```jsx
import React, { Component } from 'react'

// 孙子
class Son extends Component {
    render(){
        return(
            <div>
                i am Son
                {this.props.title}
            </div>
        )
    }
}

// 子组件
class Father extends Component {
    render(){
        return(
            <div>
                i am Father
                <Son title={this.props.title}></Son>
            </div>
        )
    }
}

// 父组件
export default class GrandFather extends Component {
    render() {
        return (
            <div>
                i am Grandfather
                <Father title="数据"></Father>
            </div>
        )
    }
}
```



### Context

如果传递的层级过多的话，逐级都要写这样写起来太麻烦

React通过**Context配置**解决组件之间的数据跨级传递

可理解为把需要跨级传递的数据放入一个容器，

哪一级的组件需要就取出，不需要每一层级都书写了

1. **父组件将数据存入Context容器**

```jsx
class 父组件 extends Component {
  
  getChildContext(){
    自定义属性：值（数据）
  }
  
  render(){
    return (
      <div>
        <子组件></子组件>
      </div>	
    )
  }
}
```

2. **父组件规定传递数据的类型**

```jsx
import PropTypes from "prop-types"

class 父组件 extends Component {
  static childContextTypes = {
    自定义属性：PropTypes.数据类型
  }
  
  getChildContext(){
    自定义属性：值（数据）
  }
  
  render(){
    return (
      <div>
        <子组件></子组件>
      </div>	
    )
  }
}
```

3. **需要接受数据的子组件通过`this.context.属性`**获取数据

```jsx
class 子孙组件 extends Component {

    render(){
        return(
            <div>
                {this.context.自定义属性}
            </div>
        )
    }
}
```

4. **需要接受数据的子组件规定接受的数据类型**

```jsx
class 子孙组件 extends Component {

    static contextTypes = {
        自定义属性:PropTypes.数据类型
    }

    render(){
        return(
            <div>
                {this.context.自定义属性}
            </div>
        )
    }
}
```

---

```jsx
import React, { Component } from 'react'
import PropTypes from "prop-types"

class Son extends Component {

  // 获取存入Context中的属性类型
    static contextTypes = {
        title:PropTypes.string
    }

    render(){
        return(
            <div>
                i am Son
<!--获取存入Context中的属性值-->         
                {this.context.title}
            </div>
        )
    }
}

class Father extends Component {

    render(){
        return(
            <div>
                i am Father

                <Son></Son>
            </div>
        )
    }
}

export default class GrandFather extends Component {

    // 规定数据类型
    static childContextTypes = {
        title:PropTypes.string
    }

    // 传递数据放入Context容器
    getChildContext(){
        return {
            title:"数据"
        }
    }

    render() {
        return (
            <div>
                i am Grandfather
                <Father></Father>
            </div>
        )
    }
}
```





## 子组件修改父组件数据

因为React中没有直接在子组件修改父组件的API

所以，在子组件中修改父组件的数据 时，需要

1. 父组件中声明一个**改变自身数据的方法**
2. 父组件通过**自定义属性传出将该方法**
3. 子组件中通过 **this.props.自定义属性** 接受该方法
4. 子组件**调用该方法**，即可在子组件中修改父组件的数据

---

如下：点击子组件内的按钮，修改父组件中声明的数据

```jsx
import React, { Component } from 'react'

class Son extends Component {

    handleClick(){
        this.props.fatherChange()
    }

    render(){
        return (
            <div className="son"> 
            <button onClick={this.handleClick.bind(this)}>click</button>
                {this.props.children}
            </div>
        )
    }
}

export default class Father extends Component {

    constructor(props){
        super(props)

        this.state = {
            num:20
        }
    }

    // 父组件中传递一个方法，给子组件调用
    fatherChange(){
        this.setState({
            num: this.state.num +1
        });
    }


    render() {
        return (
            <div className="father"> 
                <Son fatherChange={this.fatherChange.bind(this)}>{this.state.num}</Son>
            </div>
        )
    }
}
```





## 生命周期函数

<img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Faliyunzixunbucket.oss-cn-beijing.aliyuncs.com%2Fjpg%2F11ab95cda4dfef0e7b23c457c2a92255.jpg%3Fx-oss-process%3Dimage%2Fresize%2Cp_100%2Fauto-orient%2C1%2Fquality%2Cq_90%2Fformat%2Cjpg%2Fwatermark%2Cimage_eXVuY2VzaGk%3D%2Ct_100&refer=http%3A%2F%2Faliyunzixunbucket.oss-cn-beijing.aliyuncs.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1622010592&t=a63ff737cef785512e947609c1471509"  />

共有3个阶段：

- **实例化（挂载期**）：对象的创建到完全渲染
  - constructor：初始化组件
  - componentWillMount
  - render：页面标签的渲染填充
  - **componentDidMount**
- **更新期**：组件状态的改变
  - props改变
    - componentWillReceiveProps 
  - state改变
    - shouldComponentUpdate
    - **componentWillUpdate**
    - render
    - componentDidUpdate
- **销毁期**：组件使用完毕后，或 不存在与页面中
  - componentWillUnmount

```jsx
import React, {Component} from "react"

export default class App extends Component {
  
  constructor(props){
    //调用会父组件的constructor方法，传递props属性名，让props起作用
    super(props)
    
    //定义初始化状态数据
    this.state = {
      num: 100
    }
    
    console.log('1.1 初始化时执行')
  }
  
-----------------

  UNSAFE_componentWillMount(){
    console.log('1.2 挂载数据前执行')
  }

-----------------

  componentDidMount(){
    console.log('1.4 挂载数据之后执行') 
    // 异步请求
  }
  
-----------------
-----------------
  UNSAFE_componentWillReceiveProps(){
      console.log('2.1 组件接受props属性前执行') 
}
-----------------
  shouldComponentUpdate(nextprops,nextState){
      console.log('2.2 this.state改变时执行')  
  
  		console.log("旧的值" + this.state.num)
  		console.log("修改后的新值" + nextState.num)

  		// return true // this.state改变时
  		// return false // this.state没改变时
  		return (this.state.num !== nextState.num?)

}
-----------------
	UNSAFE_componentWillUpdate(){
     console.log('2.5 this.state改变后执行')   
     // loading加载
  }
-----------------
-----------------   
  handleClick(){
    console.log("hello 自定义方法函数")
  	this.setState({
      num:this.state.num+1
    }) 
  }
  
  render(){
    console.log('1.3/2.4 渲染填充数据到页面时执行')
    return (
    	<div>
      	<button onClick={this.handleClick.bind(this)}></button>
      </div>
    )
  }
}
```