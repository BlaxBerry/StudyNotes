# Ajax技术

![](https://www.aurigait.com/resources/files/2016/11/ajax_logo.png)

[TOC]

## 简介

Ajax（ **A**synchronous **J**avascript **A**nd **X**ML，异步JavaScript和XML）

是指一种创建交互式网页应用的网页开发技术

主要用于获取服务器数据

比如用户名、邮箱的验证，加载更多内容等



## 主要作用效果

在不刷新页面的情况下，

通过一个URL地址获取服务器的数据，

然后再进行页面的**局部刷新**



## 运行环境

**Ajax必须运行在网站环境中才能生效**

即 index.html文件必须在服务器环境下运行，

用 localhost域名打开，

不然双击打开html文件的话Ajax代码无法生效

---

比如

通过Node.js开启一个网站环境，并实现静态资源访问

把html文件都放入static静态资源文件夹中

然后在浏览器通过域名访问html文件

服务器目录如下：

```js
|--node_modules
|——public
	|-- index.html
|--app.js
|--package.json
|--package.lock.json
```

如下，通过express快速搭建服务器

```js
const express = require('express');

const path = require('path');

const app = express();

app.use(express.static(path.join(__dirname,'public')));

app.get('/', (req, res) => {
    res.send('Hello express')
});

app.listen(3000);

console.log('server running at localhost:3000')
```





## 原理

![](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=625365557,280187035&fm=26&gp=0.jpg)



## 实现步骤

1. **创建Ajax对象**

```js
var xhr = new XMLHttpRequest();
```

2. **告诉Ajax对象要向哪里发送请求和请求方式**

   请求方式 和 请求地址(服务器端的路由地址）

```js
xhr.open('get/post','http://www.example.com')
```

3. **发送请求**

```js
xhr.send()
```

4. **获取服务器端给客户端的响应数据**

   因为请求受网络影响会有时间消耗，

   不能直接写在send方法后，要通过函数获得

```js
xhr.onload = function() {
    console.log(xhr.responseText);
}
```

5. **拼接获取的数据和HTML字符串，然后渲染到页面**



## 步骤实例

如下：

开启服务器，

服务器目录如下：

```js
//服务器目录
|--node_modules
|——public
	|-- index.html
|--app.js
|--package.json
|--package.lock.json
```

如下，请求地址设定为`localhost:3000/first`：

```html
<!--index.html-->
<body>
    <script type='text/javascript'>
        
        var xhr = new.XMLHttpREquext();
        
        xhr.open('get','http://localhost:3000/first');
        
        xhr.send();
        
        xhr.onload = function(){
            console.log(xhr.responseText)
        }
	</script>
</body>
```

在服务端设定和请求地址相同的路由 `/first`

```js
// app.js
const express = require('express');
const path = require('path')

const app = express();

app.use(express.static(path.join(__dirname, 'public')))

app.get('/', (req, res) => {

    res.send('Hello Ajax')
    
}).listen(3000, () => {
    console.log('server running at localhost:3000');
})
```

然后在浏览器用域名打开html文件

`http://localhost:3000/index.html`

F12打开控制台会显示 `Hello Ajax`

说明Ajax发送请求和获取数据成功



## 响应的数据格式

**服务器端响应的数据一般是JSON对象格式**

但是http请求和响应的过程中，

对象类型的数据最终都会被转换成字符串进行传播

所以需要通过 **`JSON.parse()`** 

把服务端响应的 JSON字符串 转换为 JSON对象

```html
<!--index.html-->
<body>
    <script>
        var xhr = new.XMLHttpREquext();
        
        xhr.open('get','http://localhost:3000/resposeData');
        
        xhr.send();
        
        xhr.onload = function(){
           console.log(xhr.responseText);
           console.log(typeof xhr.responseText) //string
            
           var responseText = JSON.parse(xhr.responseText);
           console.log(responseText);
           console.log(typeof responseText); //object
        }
	</script>
</body>
```

```js
// app.js
const express = require('express');

const path = require('path');

const app = express();

app.use(express.static(path.join(__dirname,'public')));

app.get('/responseData',(req,res)=>{
    res.send({'name':'Andy'})
})

app.listen(3000);

console.log('server running at localhost:3000')
```



## 拼接 JSON对象 和 HTML字符串

**服务器端响应的数据一般是JSON对象格式**

客户端拿到请求响应的数据后，

客户端把 **JSON数据 和 HTML字符串拼接**

最后利用**DOM**操作渲染到页面

---

如下，

运行服务器后，

在浏览器用域名打开html文件后，

页面会渲染出 h1 标签的 Hello Andy

```html
<!--index.html-->
<body>
    <script>
        var xhr = new.XMLHttpREquext();
        
        xhr.open('get','http://localhost:3000/first');
        
        xhr.send();
        
        xhr.onload = function(){
           var responseText = JSON.parse(xhr.responseText);
           
           var str = '<h1> hello '+ responseText.name +'</h1>';
           document.body.innerHTML = str;
        }
	</script>
</body>
```

```js
// app.js
const express = require('express');

const path = require('path');

const app = express();

app.use(express.static(path.join(__dirname,'public')));

app.get('/responseData',(req,res)=>{
    res.send({'name':'Andy'})
})

app.listen(3000);

console.log('server running at localhost:3000')
```

