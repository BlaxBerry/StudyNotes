##  React脚手架

创建项目

```bash
create-react-app 项目名称
```

```bash
create-react-app demo
```

运行项目：

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

  Local:            http://localhost:3000
  On Your Network:  http://192.168.1.2:3000

Note that the development build is not optimized.
To create a production build, use yarn build.
```

<img src="https://staging-qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F484272%2F52cfad2d-ef8c-b70d-3aa5-ec79b0576737.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=1bb6e4bac443e2d9d368f6b114b249a2" style="zoom:33%;" />





清空src目录

新建React项目入口文件`index.js`



```js
// 引入核心模块
import React from "react"
import ReactDOM from "react-dom"

// 把内容渲染到id为root的标签
// ReactDOM.render(参数1，参数2)
// 参数1是渲染的内容
// 参数2是渲染到public/index.html中的哪一个标签
ReactDOM.render( 
    <div>Hello React</div>,
    document.getElementById("root")
)

```



### 组件化开发

入口文件要保持简洁

所以标签要写入组件

```js
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





### JSX语法

```js
import React, { Component } from "react"

import "./assets/css/style.css"

import img from "./assets/images/img.jpeg"

export default class App2 extends Component {

    render() {
        return (
            <div>
                <h3>JSX语法</h3>
                <p style={{color:"teal",fontSize:20}}>css行内样式</p>

                <p className="box">css外链式</p>


                <img src={img} alt="" width="200"/>

                <br/>

                <label htmlFor="inpt">
                    输入：
                    <input type="text" name="" id="inpt"/>
                </label>
            </div>
        )
    }
}
```



