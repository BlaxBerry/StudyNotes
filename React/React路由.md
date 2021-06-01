# React 路由

![](https://repository-images.githubusercontent.com/19872456/05dca500-f010-11e9-9588-a96554294e4e)



## 路由相关简介

### SPA

单页面Web应用（**S**ingle **P**age **W**eb **A**pplication）

整个应用只有一个完整的页面

**点击页面中的链接不会刷新页面，只会做页面的局部更新**

数据通过Ajax请求获取，并在前端异步展现

---

### 路由含义

一个路由就是一个映射关系

每一个路径对应一个组件

key是路径，value是可能是function或component

---

### 路由分类

**后端路由：**value是函数，根据请求地址找到匹配的路径，调用路由中的函数俩处理请求，返回响应数据

```js
app.get('/search',function(req,res){
  axios({
    url:'https://api.github.com/search/users',
    params:{req.query}
  }).then(res=>{
    res.json(res.data)
  })
})
```

**前端路由：**value是组件，当浏览器的path变为指定地址时，当前路由组件就变为相匹配的组件





## react-router-dom

React-Router是React的一个库，专门用来实现SPA应用

基本上用到React的项目都会用到React-Route

并且又下分为3种库，分别给3种平台使用：

- web(网页应用)：**react-router-dom**

- native(原生应用)

- any(通用)

---

### 下载

```bash
yarn add react-router-dom
```

### 引入

```react
import {} from 'react-router-dom'
```

react-router-dom是分别暴露，需要谁导入谁





## 路由链接 与 注册路由

1. 明确界面中的**导航区** 和 **展示区**

2. 将导航区的将a标签改为**link标签**

   ```react
   	<link to="/XXX">XXX</link>
   ```

   原生JS中通过a链接实现跳转不同页面

   React路由中通过link实现切换组件，最后被转换为a标签

   Link必须被 <BrowserRouter\> 或 <HashRouter\> 包裹

   ```react
   <BrowserRouter>
   	<link to="/home">XXX</link>
     <link to="/about">XXX</link>
   </BrowserRouter>
   ```

3. 展示区的**Route标签**进行匹配路径

   通过**path**属性绑定link的to

   通过**component**属性绑定要显示的组件

   ```react
   <Route path="/XXX" component={XXX}/>
   ```

   Route必须被 <BrowserRouter\> 或 <HashRouter\> 包裹

   ```react
   <BrowserRouter>
   	<Route path="/home" component={Home}/>
     <Route path="/about" component={About}/>
   </BrowserRouter>
   ```

4. **APP组件**外侧包裹 <BrowserRouter\> 或 <HashRouter\> 

   整个应用的路由必须使用仅一个Router路由器管理

   即Link和Route都要包裹在一个Router中，而不是分开

   可以直接在 index.js 中用路由器**包裹整个APP组件**，一劳永逸

   ```react
   import React from 'react'
   import ReactDOM from 'react-dom'
   
   import {BrowserRouter} from 'react-router-dom'
   
   import APP from './App'
   
   ReactDOM.render(
     <BrowserRouter>
       <App/>
     </BrowserRouter>,
     document.getElementById('root')
   )
   ```







## 路由组件 与 一般组件

### 1. 写法不同

通过手写展示的组件，叫一般组件

靠路由匹配然后展示的组件，叫路由组件

```react
// 路由组件：
<Route path="/XXX" component={XXX}/>

//一般组件：
<About/>
```

### 2. 存放位置不同

路由组件要和一般组件的文件夹分开，一般放入**pages 目录**

相当于Vue中的Views目录

```js
src
|-components //一般组件
|-pages // 路由组件
|-index.js
|-App.jsx
```

### 3. 接收到的props不同

**一般组件：**

写组件标签时传递了什么，在组件内通过this.props就接受到什么

若不传递则组件内通过this.props获得的是个空对象 **{ }**

**路由组件**

路由组件中this.props默认获得路由器默认传入**三个固定的属性**

- **history**
  - go
  - goBack
  - goForward
  - push
  - replace

- **location**
  - pathname:""
  - search:""
  - state:""

- **match**
  - parmas
  - path
  - url









## NavLink标签

### 选中样式

可以实现路由链接的高亮样式，是Link标签的升级版

Link标签不能添加选中样式，NavLink标签可以

```react
import React,{Components} from 'react'
import {NavLink,Route} from 'react-router-dom'

import Home from './pages/Home'
import About from './pages/About'

export default class APP extends Components {
  render(){
    return (
    	<div id="app">
        <div className="nav">
          <NavLink className="nav-item" to="/home">Home</NavLink>
          <NavLink className="nav-item" to="/About">About</NavLink>
        </div>
      </div>
    )
  }
}
```

被点击的NavLink标签会被自动添加上一个类名 **active**

```css
.active {
  color: red;
  background:gray;
}
```

---

### 修改样式类名

但是可以通过**activeClassName**属性修改样式类名

```react
import React,{Components} from 'react'
import {NavLink,Route} from 'react-router-dom'

import Home from './pages/Home'
import About from './pages/About'

export default class APP extends Components {
  render(){
    return (
    	<div id="app">
        <div className="nav">
          <NavLink className="nav-item" to="/home"
            	activeClassName="clicked"
           >Home
          </NavLink>
          <NavLink className="nav-item" to="/About"
            	activeClassName="clicked"  
           >About
          </NavLink>
        </div>
      </div>
    )
  }
}
```

为了预防样式权重冲突(比如使用了Bootstrap)，可以添加**!important**提高样式权重

```css
.clicked {
  color: red!important;
  background:gray!important;
}
```

---

### 封装NavLink组件

NavLink标签内容基本上是相同的，多次使用的话会出现代码重复冗余，

```react
<NavLink className="nav-item" to="/home" activeClassName="clicked" >Home</NavLink>
<NavLink className="nav-item" to="/home" activeClassName="clicked" >Home</NavLink>
<NavLink className="nav-item" to="/home" activeClassName="clicked" >Home</NavLink>
<NavLink className="nav-item" to="/home" activeClassName="clicked" >Home</NavLink>
<NavLink className="nav-item" to="/home" activeClassName="clicked" >Home</NavLink>
```

可以考虑封装为一个组件，多次调用

```react
<MyNavLink to="/home">Home</MyNavLink>
<MyNavLink to="/about">About</MyNavLink>
```

标签属性被放入props中传入组件

**标签体**是一个特殊的标签属性，被自动放入**props.children**中传入组件

```react
import React,{Components} from 'react'
import {NavLink} from 'react-router-dom'

export default class MyNavLink extends Components {
  render(){
    return (
      <NavLink activeClassName="clicked"
        		className="nav-item" 
        		{...this.props}/>
    )
  }
}
```





## Switch标签

用于规范**一对一的路径匹配**（单一匹配），提高路由匹配的效率

一般情况下一个路径path应该对应一个组件component，

但假设同一路径对应了多个不同组件，

React-Router在匹配上了一个路径后还会继续匹配，会展示所有匹配上的组件

不但会影响展现的内容，还会影响效率

```react
<Route path="/home" component={Home}></Route>
<Route path="/home" component={Home1}></Route>
<Route path="/home" component={Home2}></Route>
<Route path="/home" component={Home3}></Route>
<Route path="/home" component={Home4}></Route>
<Route path="/home" component={Home5}></Route>
```

为了解决该问题，通过路由的Switch内置组件包裹所有<Route\>

只展示匹配上路径的第一个组件

```react
// 引入：
import {Switch} from 'react-router-dom'
```

```react
<Switch>
	<Route path="/home" component={Home}></Route>
  <Route path="/home" component={Home1}></Route> {/*不会展示*/}
  <Route path="/home" component={Home2}></Route> {/*不会展示*/}
</Switch>
```





## 多级路径页面刷新 与 样式丢失

### 多级路径

```react
<div>
	<Link to="food/meat">Meat</Link>
  <Link to="food/fruits">Fruits</Link>
</div>

<div>
	<Route path="food/meat" component={Meat}></Route>
  <Route path="food/fruits" component={Fruits}></Route>
</div>
```

---

---

### 样式丢失原因

React脚手架是通过webpack的devserve开启了一个本地服务器localhost:3000

脚手架项目下的**public**目录就作为该服务器的根路径

```js
项目
|-public
	|-favicon.ico
	|-index.html
	|-css
		|-bootstrap.css
```

可以通过浏览器地址直接访问public目录下的内容

```react
localhost:3000/favicon.ico
localhost:3000/css/bootstrap.css
```

直接访问 localhost:3000，会返回public目录下的 **index.html**

```js
localhost:3000
```

但是若找不到路径的话，会自动返回public目录下的 **index.html**

```js
localhost:3000/a/b/c/d/123.css
```

前端路由不会产生网络请求

刷新页面会产生网络请求

所以点击link路由链接实现路由跳转后刷新页面

路由地址添加前缀后，实现路由跳转后刷新页面，

会导致浏览器以添加了地址前缀的路径发送请求，

从而导致路径层级错误，会使public目录下的css样式文件等找不到

仅返回一个index.html

---

---

### 解决方法

#### 1. %PUBLIC_URL%（脚手架常用）

public / index.html 中引入样式时，

不写 **./**  写 **%PUBLIC_URL%**

```html
<link rel="stylesheet" href="%PUBLIC_URL%/css/bootstrap.css">
```

---

#### 2.   /（常用）

public / index.html 中引入样式时，

不写 **./**  写 **/**

**./** 是从相对路径的从当前路径出发查找css文件

因为路由地址添加前缀后会将前缀地址页加入了查找路径，会导致找不到

```html
<link rel="stylesheet" href="./css/bootstrap.css">
```

是直接从loclahost:3000的根目录public下查找css文件

```html
<link rel="stylesheet" href="/css/bootstrap.css">
```

---

#### 3. 修改路由模式（不常用）

不用BrowserRouter，使用**HashRouter**

```java
localhost:3000/#
```

在发送网络请求时，#后的路径都认为是前端资源必备忽略，

不会带给localhost:3000





## 路由模糊匹配  与 精准匹配 

### 模糊匹配

路由默认是模糊匹配

Link标签to属性的路径 **只要包含有** Route标签的path属性路径，

且路径的**层级顺序一致**，就可以展现对应组件

```react
// 可以匹配
<div>
	<Link to="home">Home</Link>
</div>

<div>
	<Route path="home" component={Home}/>
</div>
```

给少了不匹配

```react
// 无法匹配
<div>
	<Link to="home">Home</Link>
</div>

<div>
	<Route path="home/a/b" component={Home}/>
</div>
```

层级顺序一致给多了可以匹配上

```react
// 可以匹配
<div>
	<Link to="home/a/b">Home</Link>
</div>

<div>
	<Route path="home" component={Home}/>
</div>
```

层级顺序一致给多了无法匹配

```react
// 无法匹配
<div>
	<Link to="a/home/b">Home</Link>
</div>

<div>
	<Route path="home" component={Home}/>
</div>
```

---

### 精准匹配

Link的路由地址和路由Route的地址和层级一致才能展现对应组件

开启路由时，通过**exact**属性开启精准匹配

```react
<Route exact={true} path="/xxx" component={XXX}/>

//简写
<Route exact path="/xxx" component={XXX}/>
```

```react
<div>
	<Link to="home/a/b">Home</Link>
	<Link to="about">About</Link>  
</div>

<div>
	<Route exact={true} path="home/a/b" component={Home}/>
	<Route exact path="about" component={About}/>  
</div>
```

但是**不能随便开启严格匹配**

不到必须的情况不能随便开启，不然导致无法继续匹配二级路由





## Redirect标签

**路由重定向**

常用于刚开启时默认展现的路由地址

```react
import {Link,Route,Redirect} from 'react-route-dom'
```

<Redirect\>标签写在所有路由<Route\>标签的最下方

当所有路由都无法匹配上时跳转到<Redirect\>标签指定的路由地址

```react
<switch>
	<Route path="/home" component={Home}/>
  <Route path="/about" component={About}/>
  <Redirect to="/home"/>
</switch>
```

如上，访问 localhost:3000 时，自动跳转到 localhost:3000/home

（localhost:3000/  等同于 localhost:3000）







## 嵌套路由

<img src="https://pbs.twimg.com/media/E2t8QvQVEAAzrUT?format=png&name=small" style="zoom: 80%;" />

展示区里还有导航区和展示区

```react
export default class App extends Component {
    render() {
        return (
            <div>
               <div className="nav">
                   <Link to="/home">Home</Link>
                   <Link to="/about">About</Link>
               </div>
               <div>
                   <Route path="/home" component={Home}></Route>
                   <Route path="/about" component={About}></Route>
                   <Redirect to="/home"/>
               </div>
            </div>
        )
    }
}
```

```react
export default class About extends Component {
    render() {
        return (
            <div id="about"> 
                <div className="nav">
                    <Link to="/about/news">News</Link>
                    <Link to="/about/messages">Messages</Link>
                </div>
                <div className="show">
                    <Route path="/about/news" component={News}/>
                    <Route path="/about/messages" component={Messages}/>
                  	<Redirect to="/about/news"/>
                </div>
            </div>
        )
    }
}
```

子级路由的路径层级中必须带有父级路由

```react
<Route to="/父级/子级" component={组件}/>
```

---

若是自己路由层级中没有父级路由，

会在匹配路由时直接在父级路由模版出进行匹配，因为父级路由模版处不存在子级路由，

会导致触发**父级路由中的重定向<Redirect\>**

从而直接跳转到重定向地址然后展示相关组件

---

若开启了精准匹配exact，

也会在匹配路由时在父级路由模版出进行匹配，因为父级路由模版处不存在子级路由，

会导致触发**父级路由中的重定向<Redirect\>**

从而直接跳转到重定向地址然后展示相关组件



### 多级路由

loaclhost:3000

```react
export default class App extends Component {
    render() {
        return (
            <div>
               <div className="nav">
                   <Link to="/home">Home</Link>
                   <Link to="/about">About</Link>
               </div>
               <div>
                   <Route path="/home" component={Home}></Route>
                   <Route path="/about" component={About}></Route>
                   <Redirect to="/home"/>
               </div>
            </div>
        )
    }
}
```

一级路由模版：

loaclhost:3000/about

```react
export default class About extends Component {
    render() {
        return (
            <div id="about"> 
                <div className="nav">
                    <Link to="/about/news">News</Link>
                    <Link to="/about/messages">Messages</Link>
                </div>
                <div className="show">
                    <Route path="/about/news" component={News}/>
                    <Route path="/about/messages" component={Messages}/>
                  	<Redirect to="/about/news"/>
                </div>
            </div>
        )
    }
}
```

二级路由模版：

loaclhost:3000/about/news

```react
import React, { Component } from 'react'
import { Link, Route } from 'react-router-dom'
import Detail from '../Detail/Detail'

export default class News extends Component {
    state = {
        newsObj: [
            { id: 1, title: 'news01' },
            { id: 2, title: 'news02' },
            { id: 3, title: 'news03' }
        ]
    }
    render() {
        return (
            <div id="news">
                <ul>
                    {
                        this.state.newsObj.map(item => {
                            return (
                                <li key={item.id}>
                                    <Link to="/about/news/detail">
                                      {item.title}
                                		</Link>
                                </li>
                            )
                        })
                    }
                </ul>
                <Route path="/about/news/detail" component={Detail} />
            </div>
        )
    }
}
```

三级路由模版

loaclhost:3000/about/news/detail

```react
import React, { Component } from 'react'

export default class Detail extends Component {
    render() {
        return (
           <ul>
               <li>ID:???</li>
               <li>TITLE:???</li>
           </ul>
        )
    }
}
```





## 路由组件传参

### 一. params参数

```java
localhost:3000/about/news/detail/2/news02
```

#### 1. 路由链接携带参数

在要传递出参数的路由组件中：

Link标签的to属性的路由路径中带上参数数据

```react
<Link to={`/路由/路由/${数据}/${数据}`}>导航</Link>
```

#### 2. 注册路由声明接收

在要传递出参数的路由组件中：

Route标签的path属性路径中通过 **:自定义属性名** 存放参数数据

不然会被路由模糊匹配给忽略掉后面的参数

```react
<Route path="/路由/路由/:自定义名/:自定义名" component={组件} />
```

#### 3.  接受参数

在接受参数的路由组件中，

通过**this.props.match.params** 接受传参，

收到的是JSON对象形式

```js
console.log(this.props.match.params)
/*
{
  自定义名: 数据，
	自定义名: 数据
}
*/
```

如下：

```react
// 传递参数的组件
import React, { Component } from 'react'
import { Link, Route } from 'react-router-dom'
import Detail from '../Detail/Detail'

export default class News extends Component {
    state = {
        newsObj: [
            { id: 1, title: 'news01' },
            { id: 2, title: 'news02' },
            { id: 3, title: 'news03' }
        ]
    }
    render() {
        return (
            <div id="news">
                <ul>
                    {
                        this.state.newsObj.map(item => {
                            return (
                                <li key={item.id}>
                                    <Link to={`news/detail/${item.id}/${item.title}`}>
                                        {item.title}
                                    </Link>
                                </li>
                            )
                        })
                    }
                </ul>
                <Route path="news/detail/:id/:title" component={Detail} />
            </div>
        )
    }
}
```

```react
// 接受参数的组件
import React, { Component } from 'react'

export default class Detail extends Component {
    state = {
        details: [
            { id: 1, message: 'i am No1' },
            { id: 2, message: 'i am No2' },
            { id: 3, message: 'i am No3' }
        ]
    }

    render() {
        // console.log(this.props.match.params);
        const { params } = this.props.match

        const res = this.state.details.find(item=>{
            return item.id === Number(params.id)
        })

        return (
            <ul>
                <li>ID: {params.id}</li>
                <li>TITLE: {params.title}</li>
                <li>MESSAGE: {res.message}</li>
            </ul>
        )
    }
}
```

---

---

### 二. search参数

```javascript
?id=3&title=news03
```

就是query形式的传参，但是因为最终参数存放在search上，所以叫search参数

#### 1.路由链接携带参数

Link标签通过 **?key=value&key=value**  的query形式传递参数

Route标签无需声明接收，正常注册路由即可

```react
<Link to={`/路由/路由/?自定义属性名=${数据}&自定义属性名=${数据}`}>
```

#### 2. 接受参数

在要接收参数的组件中，

通过 **this.props.location.search** 接受传递的参数

返回的是 **?key=value&key=value** urlencoded编码的字符串

```js
console.log(this.props.location.search)

// ?自定义属性=数据&自定义属性=数据
```

#### 3. querystring库

由于返回的是 **?key=value&key=value** urlencoded编码的字符串

需要借助第三方库解析为JSON对象格式

React脚手架已经下载好了，直接引入即可

```react
import qs from 'querystring'
```

urlencoded格式：多组&链接的key=value形式

JSON格式：花括号包裹的key: value对象形式

- **JSON ——> urlencoded**

```js
import qs from 'querystring'

let obj = {name: 'andy', age: 28}
console.log(qs.stringify(obj))
// name=andy&age=28
```

- **urlencoded ——> JSON**

```js
import qs from 'querystring'

let str = 'name=andy&age=28'
console.log(qs.parse(str))
// {name: 'andy', age: 28}
```

#### 4. 解析参数

先截取字符串，除去 **?key=value&key=value** 的 **?**

然后通过内置querystring库解析

 **key=value&key=value **的urlencoded编码字符串为JSON格式

```js
console.log(this.props.location.search)
// ?自定义属性=数据&自定义属性=数据

this.props.location.search.slice(1)
// 自定义属性=数据&自定义属性=数据

qs.parse(this.props.location.search.slice(1))
/*
{
  自定义名: 数据，
	自定义名: 数据
}
*/
```

如下：

```react
// 传出参数的组件
import React, { Component } from 'react'
import { Link, Route } from 'react-router-dom'
import Detail from '../Detail/Detail'

export default class News extends Component {
    state = {
        newsObj: [
            { id: 1, title: 'news01' },
            { id: 2, title: 'news02' },
            { id: 3, title: 'news03' }
        ]
    }
    render() {
        return (
            <div id="news">
                <ul>
                    {
                        this.state.newsObj.map(item => {
                            return (
                                <li key={item.id}>
                                    <Link to={`/news/detail/?id=${item.id}&title=${item.title}`}>
                                        {item.title}
                                    </Link>
                                </li>
                            )
                        })
                    }
                </ul>
                <Route path="/news/detail" component={Detail} />
            </div>
        )
    }
}

```

```react
// 接受参数的组件
import React, { Component } from 'react'

import qs from 'querystring'

export default class Detail extends Component {
    state = {
        details: [
            { id: 1, message: 'i am No1' },
            { id: 2, message: 'i am No2' },
            { id: 3, message: 'i am No3' }
        ]
    }

    render() {
        // console.log(this.props.location.search);
        // console.log(qs.parse((this.props.location.search).slice(1)));

        const { id, title } = qs.parse((this.props.location.search).slice(1))
        
        const res = this.state.details.find(item => {
            return item.id === Number(id)
        })

        return (
            <ul>
                <li>ID: {id}</li>
                <li>TITLE: {title}</li>
                <li>MESSAGE: {res.message}</li>
            </ul>
        )
    }
}
```

---

---

### 三. state参数

是路由中的state参数，不是组件中的state

params参数和search参数的传递，都可以在浏览器地址栏中看到

而路由**state参数不暴露，在浏览器地址栏中看不到**

刷新页面也会保留参数

#### 1. 路由链接携带参数

Link标签的to属性通过对象形式传递参数

Route标签无需声明接收，正常注册路由即可

```react
<Link to={{ path:'/路由地址', state:{自定义属性名:数据, 自定义属性名:数据 }}}>
```

#### 2. 接收参数

在接受参数的路由组件中，

通过**this.props.loctaion.state** 接受传参，

收到的是JSON对象形式

```js
console.log(this.props.loctaion.state)
/*
{
  自定义名: 数据，
	自定义名: 数据
}
*/
```

如下：

```react
// 传出参数的组件
import React, { Component } from 'react'
import { Link, Route } from 'react-router-dom'
import Detail from '../Detail/Detail'

export default class News extends Component {
    state = {
        newsObj: [
            { id: 1, title: 'news01' },
            { id: 2, title: 'news02' },
            { id: 3, title: 'news03' }
        ]
    }
    render() {
        return (
            <div id="news">
                <ul>
                    {
                        this.state.newsObj.map(item => {
                            return (
                                <li key={item.id}>
                                    <Link to={{ 
                                    	path: '/news/detail', 
                                      state: { 
                                        id: item.id, 
                                        title: item.title 
                                      } 
                                  	}}>
                                     	{item.title}
                                    </Link>
                                </li>
                            )
                        })
                    }
                </ul>
                <Route path="/news/detail" component={Detail} />
            </div>
        )
    }
}
```

```react
// 接收参数的组件
import React, { Component } from 'react'

export default class Detail extends Component {
    state = {
        details: [
            { id: 1, message: 'i am No1' },
            { id: 2, message: 'i am No2' },
            { id: 3, message: 'i am No3' }
        ]
    }

    render() {	
      	//console.log(this.props.location.state);
        const {id, title} = this.props.location.state || {}
        
        const res = this.state.details.find(item => {
            return item.id === Number(id)
        }) || {}

        return (
            <ul>
                <li>ID: {id}</li>
                <li>TITLE: {title}</li>
                <li>MESSAGE: {res.message}</li>
            </ul>
        )
    }
}
```





## 路由跳转模式

### push

路由默认的跳转模式

**压栈**，会留下历史记录，可前进后退

----

### replace

浏览记录的无痕模式

**替换**，没历史记录，无法前进后退

Link标签通过replace属性开启

```react
<Link replace={true} to="/路由地址">导航</Link>

// 简写
<Link replace to="/路由地址">导航</Link>
```





## 编程式路由导航

不借助点击路由链接的Link标签和NavLink标签，

而是借助路由组件的**this.props.history**对象上的API实现路由跳转、前进、后退



### this.props.history.push( )

### this.props.history.replace( )

React的路由有两种跳转模式，有三种携带参数的方式

编程式导航都可以实现：

#### 不带参数

```react
import React, { Component } from 'react'

export default class Home extends Component {
  
    componentDidMount(){
      
        setTimeout(()=>{
            this.props.history.push('/about/messages')
        },2000)
    }

    render() {
        return (
            <div>Home</div>
        )
    }
}
```

---

#### params参数

```react
import React, { Component } from 'react'
import { Route } from 'react-router-dom'
import Detail from '../Detail/Detail'

export default class News extends Component {
    state = {
        newsObj: [
            { id: 1, title: 'news01' },
            { id: 2, title: 'news02' },
            { id: 3, title: 'news03' }
        ]
    }

    pushShow = (id,title) => {
        return () => {
            // console.log(this.props.history);
            this.props.history.push(`/about/news/detail/${id}/${title}`)
        }
    }

    replaceShow =(id,title)=>{
        return ()=>{
          	// console.log(this.props.history.replace);
            this.props.history.replace(`/about/news/detail/${id}/${title}`)
        }

    }

    render() {
        return (
            <div id="news">
                <ul>
                    {
                        this.state.newsObj.map(item => {
                            return (
                                <li key={item.id}>
                                    <button onClick={this.pushShow(item.id,item.title)}>
                                      Push跳转
                                		</button>
                                
                                    <button onClick={this.replaceShow(item.id,item.title)}>
                                      Replace跳转
                                		</button>
                                </li>
                            )
                        })
                    }
                </ul>
                <Route path="/about/news/detail/:id/:title" component={Detail} />
                {/* <Route path="/about/news/detail" component={Detail} /> */}
            </div>
        )
    }
}

```

---

#### search参数

```react
import React, { Component } from 'react'
import { Route } from 'react-router-dom'
import Detail from '../Detail/Detail'

export default class News extends Component {
    state = {
        newsObj: [
            { id: 1, title: 'news01' },
            { id: 2, title: 'news02' },
            { id: 3, title: 'news03' }
        ]
    }

    pushShow = (id,title) => {
        return () => {
            // console.log(this.props.history.push);
            this.props.history.push(`/about/news/detail/?id=${id}&title=${title}`)

        }
    }

    replaceShow =(id,title)=>{
        return ()=>{
            // console.log(this.props.history.replace);
            this.props.history.replace(`/about/news/detail/?id=${id}&title=${title}`)
        }

    }

    render() {
        return (
            <div id="news">
                <ul>
                    {
                        this.state.newsObj.map(item => {
                            return (
                                <li key={item.id}>
                                    <button onClick={this.pushShow(item.id,item.title)}>
                                      Push跳转
                                		</button>
                                
                                    <button onClick={this.replaceShow(item.id,item.title)}>
                                      Replace跳转
                                		</button>
                                </li>
                            )
                        })
                    }
                </ul>
                <Route path="/about/news/detail" component={Detail} />
            </div>
        )
    }
}

```

---

#### state参数

```react
import React, { Component } from 'react'
import { Route } from 'react-router-dom'
import Detail from '../Detail/Detail'

export default class News extends Component {
    state = {
        newsObj: [
            { id: 1, title: 'news01' },
            { id: 2, title: 'news02' },
            { id: 3, title: 'news03' }
        ]
    }

    pushShow = (id,title) => {
        return () => {
            // console.log(this.props.history.push);

            this.props.history.push('/about/news/detail', {id:id,title:title})
        }
    }

    replaceShow =(id,title)=>{
        return ()=>{
            // console.log(this.props.history.replace);
          
            this.props.history.replace('/about/news/detail', {id:id,title:title})
        }

    }

    render() {
        return (
            <div id="news">
                <ul>
                    {
                        this.state.newsObj.map(item => {
                            return (
                                <li key={item.id}>
                                    <button onClick={this.pushShow(item.id,item.title)}>
                                        Push跳转
                                    </button>
                                    <button onClick={this.replaceShow(item.id,item.title)}>
                                        Replace跳转
                                    </button>
                                </li>
                            )
                        })
                    }
                </ul>
                <Route path="/about/news/detail" component={Detail} />
            </div>
        )
    }
}

```

---

### this.props.history.goBack()

返回上一层级路由

### this.props.history.goForward()

前往下一层级

```react
<button onClick={this.goBack}> Back </button>

<button onClick={this.goForward}> Forward </button>
```

```react
goBack=()=>{
  this.props.history.goBack()
}
    
goForward=()=>{
  this.props.history.goForward()
}
```

---

### this.props.history.go()

#### this.props.history.go(1)

返回上一层级路由

#### this.props.history.go(-1)

前往下一层级

#### this.props.history.go(0)

刷新页面





## withRouter 函数（常用）

用于解决一般组件中无法使用路由组件API的问题

编程式导航是通过this.props.history中的API实现路由的跳转、前进、后退

其中的history是路由器默认传递给路由组件的

因此一般组件中的 this.props中不含有histor对象

所以无法直接通过编程式导航实现路由跳转

如下：

浏览器会提示没有history对象

```react
import React, { Component } from 'react'

export default class Header extends Component {

    goBack = () => {
        this.props.history.goBack()
    }

    goForward = () => {
        this.props.history.goForward()
    }

    render() {
        console.log(this.props);
        return (
            <div className="header"}>
                    
                <button onClick={this.goBack}>Back</button>
                <button onClick={this.goForward}>Forward</button>

            </div>
        )
    }
}
```

```js
// Uncaught TypeError: Cannot read property 'goBack' of undefined
```

所以，一般组件上想要使用路由组件API的跳转、前进、后退，

需要借助路由react-router-dom的一个内置函数**withRoute**

**加工一般组件，使该一般组件具有路由组件特有的API**（history、location、match）

使其可以通过this.props.history对象上的API实现路由的跳转、前进、后退

**withRoute(一般组件)** 接收一个一般组件，返回值是个新组件

然后通过export default暴露

```react
import { withRouter } from 'react-router-dom'

class 组件 extends React.Component {
  render(){}
}

export default withRouter(组件)
```

```react
import React, { Component } from 'react'
import { withRouter } from 'react-router-dom'


class Hello extends Component {

    goBack = () => {
        this.props.history.goBack()
    }

    goForward = () => {
        this.props.history.goForward()
    }

    render() {
        console.log(this.props);
        return (
            <div className={hello.title}>
            
            		<button onClick={this.goBack}>Back</button>
                <button onClick={this.goForward}>Forward</button>

            </div>
        )
    }
}

export default  withRouter(Hello)
```





## 两种路由器的区别

#### BrowserRouter

#### HashRoouter

1. 底层原理不同：

- BrowserRouter使用的是H5的 History API

  （IE9及以下不支持）

- HashRouter使用的是 URL的哈希值

---

2. 表现形式不同：

- BrowserRouter的路由路径中没有#

```js
loaclhost:3000/a/b/c
```

- HashRouter的路由路径中包含#

```	java
loaclhost:3000/#/a/b/c
```

---

3. 刷新页面后对路由参数state的影响不同：

- BrowserRouter没有影响，

  state参数储存在history对象中

- **HashRouter刷新后会导致state参数丢失**，

  因为没有使用history API，没有history对象，

  没有储存state参数

---

4. HashRouter可以用来解决路径错误问题

   比如多层级路径页面刷新时导致的引入样式丢失

---

一般情况下BrowserRouter使用场合更多





## react-router-dom内置组件和API

<BrowserRouter\> 浏览器路由模式

<HashRouter\> Hash路由模式

<Link\> 路由链接

<Navlink\> 带高亮样式的路由链接

<Route\> 路由占位

<Switch\>单一匹配

<Redirect\> 重定向



history对象

location对象

martch对象



withRouter函数









