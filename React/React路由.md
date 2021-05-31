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







## React-Router

React-Router是React的一个库，专门用来实现SPA应用

基本上用到React的项目都会用到React-Route

并且又下分为3种库，分别给3种平台使用：

- web(网页应用)：**react-router-dom**

- native(原生应用)

- any(通用)





## react-router-dom

### 下载

```bash
yarn add react-router-dom
```

### 引入

```react
import {} from 'react-router-dom'
```

react-router-dom是分别暴露，需要谁导入谁





## 注册路由

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







## 路由组件，一般组件

靠路由匹配然后展示的组件，叫路由组件

### 1. 写法不同

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

### 3. 接受到的props不同

**一般组件：**

写组件标签时传递了什么，在组件内通过this.props就接受到什么

**路由组件**

接收到由路由器传入**三个固定的属性**

在路由组件中通过this.props获得

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







## 路径前缀与样式丢失

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



解决方法1.

.

index.html文件中的css样式引入的路径不能以 . 开头

. 是从相对路径的从当前路径出发查找css文件

因为路由地址添加前缀后会将前缀地址页加入了查找路径，会导致找不到

```html
<link rel="stylesheet" href="./css/bootstrap.css">
```

是直接从loclahost:3000的根目录public下查找css文件

```html
<link rel="stylesheet" href="/css/bootstrap.css">
```



2.

%PUBLIC_URL%

绝对路径

```html
<link rel="stylesheet" href="%PUBLIC_URL%/css/bootstrap.css">
```



3.

不用BrowserRouter，使用HashRouter

```java
localhost:3000/#
```

在发送网络请求时，#后的路径都认为是前端资源必备忽略，不会带给localhost:3000





内置组件

<Router\>

<BrowserRouter\> 

<HashRouter\> 

<Link\> 路由链接

<Navlink\>

<Route\>

<Switch\>

<Redirect\>





history对象

martch对象

withRouter函数





## BrowserRouter, HashRoouter

```java
// BrowserRouter
loaclhost:3000/home
loaclhost:3000/about

// HashRouter
loaclhost:3000/#/home
loaclhost:3000/#/about
```



