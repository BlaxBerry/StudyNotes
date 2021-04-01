# Ajax技术

![](https://www.aurigait.com/resources/files/2016/11/ajax_logo.png)

[TOC]
#目录
- [Ajax状态码](#Ajax状态码)  
- 

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





## 请求参数

传统网站中请求参数都是通过表单的形式发送

通过form表单域 和 submit按钮提交

```html
<form>
    <input type="text">
    <input type="submit">
</form>
```

根据请求方式的不同，表单内容会变成请求参数自动添加到对应位置

- **get请求的参数** 被添加到请求地址的后面

- **post请求的参数** 被放到请求体中

---

通过Ajax发送请求的话，

可以不通过form表单域 和 submit按钮提交

Ajax中需要自己拼接请求参数，

然后根据请求方式的不同，把请求参数放到对应位置

参数的格式和传统请求参数的格式一样

---

### get请求参数

get请求的参数是在请求地址的后面

Ajax需要手动把请求参数拼接到请求地址的后面

```js
// 请求地址?参数名称=参数值and参数名称=参数值
xhr.open('get','http://www.example.com?name=Andy&age=28')
```

如下：

客户端不通过form表单域 和 submit按钮提交，

给btn按钮绑定点击事件，点击后创建Ajax对象，

手动拼接请求参数，然后发送请求给 `localhost:3000/get`

服务端响应 `req.query` 

控制台打印出 JSON字符串形式的  `req.query` 

验证发送get请求成功

```html
<!--index.html-->
<body>
    <div>name: <br>
        <input type="text" id='usrname'>
    </div>
    <div>age: <br>
        <input type="text" id='usrage'>
    </div>
    <div>
        <input type="button" value="send" id='btn'>
    </div>

    <script>
        var btn = document.getElementById('btn');
        var usrname = document.getElementById('usrname');
        var usrage = document.getElementById('usrage')

        btn.onclick = function() {

            var xhr = new XMLHttpRequest();

            var name = usrname.value;
            var age = usrage.value;

            var params = 'usrname=' + name + '&usrage=' + age;

            xhr.open('get', 'http://localhost:3000/get?' + params);

            xhr.send();

            xhr.onload = function() {
                console.log(xhr.responseText);
            }
        }
    </script>
```

```js
// app.js
const express = require('express');

const path = require('path');

const app = express();

app.use(express.static(path.join(__dirname,'public')));

app.get('/get',(req,res)=>{
    res.send(req.query)
})

app.listen(3000);

console.log('server running at localhost:3000')
```

---

---

### post请求参数

post请求的参数在请求体（报文体）中

通过Ajax发送请求的话，

只需要把请求参数放入`send()`中，

并且需要通过`setRequestHeader()`设置请求报文的数据格式

---

### post请求参数的数据格式

#### application/x-www-form-urlencoded

**多个键值对格式**

（传统form表单提交和get请求提交的也是该数据格式）

```js
xhr.setRequestHeader('Content-type','application/x-www-from-urlencoded');
xhr.send('name=andy&age=28')
```

express中需要借助第三方模块 `body-parser`来获取POST参数

并通过`app.use(bodyParser.urlencoded());` 

设定去解析`application/x-www-from-urlencoded`格式的POST参数

如下：

```js
const express = require('express');
const path = require('path');
const app = express();

const bodyParser = require('body-parser');

app.use(bodyParser.urlencoded());

app.use(express.static(path.join(__dirname, 'public')));

app.post('/post', (req, res) => {
    res.send(req.body)
})

app.listen(3000)
console.log('server running at localhost:3000');
```

```html
<!--index.html-->
<body>
    <div>name: <br>
        <input type="text" id='usrname'>
    </div>
    <div>age: <br>
        <input type="text" id='usrage'>
    </div>
    <div>
        <input type="button" value="send" id='btn'>
    </div>

    <script>
        var btn = document.getElementById('btn');
        var usrname = document.getElementById('usrname');
        var usrage = document.getElementById('usrage')

        btn.onclick = function() {

            var xhr = new XMLHttpRequest();

            var name = usrname.value;
            var age = usrage.value;

            var params = 'usrname=' + name + '&usrage=' + age;

            xhr.open('post', 'http://localhost:3000/post');

            xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')

            xhr.send(params);

            xhr.onload = function() {
                console.log(xhr.responseText);
            }
        }
    </script>
```

---

#### application/json

**JSON数据格式**

```js
var JSONObj = {name:'Tom',age:28,sex:'male'}
var JSONString=JSON.stringify(obj)
xhr.send(JSONString)
```

express中需要借助第三方模块 `body-parser`来获取POST参数

并通过`app.use(bodyParser.json());` 

设定去解析`application/json`格式的POST参数

如下：

```js
const express = require('express');
const path = require('path');
const app = express();

const bodyParser = require('body-parser');

app.use(bodyParser.json());

app.use(express.static(path.join(__dirname, 'public')));

app.post('/post', (req, res) => {
    res.send(req.body)
})

app.listen(3000)
console.log('server running at localhost:3000');
```

```html
<body>
    <div>name: <br>
        <input type="text" id='usrname'>
    </div>
    <div>age: <br>
        <input type="text" id='usrage'>
    </div>
    <div>
        <input type="button" value="send" id='btn'>
    </div>

    <script>
        var btn = document.getElementById('btn');
        var usrname = document.getElementById('usrname');
        var usrage = document.getElementById('usrage')

        btn.onclick = function() {

            var xhr = new XMLHttpRequest();

            xhr.open('post', 'http://localhost:3000/json');

            xhr.setRequestHeader('Content-Type', 'application/json');

            var obj = {
                'usrname': usrname.value,
                'usrage': usrage.value
            }

            xhr.send(JSON.stringify(obj));

            xhr.onload = function() {
                console.log(xhr.responseText);
            }
        }
    </script>
```



## 请求参数的格式

- **application/x-www-form-urlencoded**

get 请求和传统表单域请求只能提交该格式的数据

post也可以提交该格式的数据

- **application/json**

post也可以提交该格式的数据





## Ajax状态码

**表示Ajax请求的过程的状态**，即走到了哪一个步骤

创建Ajax对象——>配置Ajax对象——>发送请求——>接受服务器端的响应数据

该过程中的每一个步骤都对应一个状态码

请求过程中状态码是在不断变化的

---

### 01234

Ajax状态码共有5个：

- **0** ： 请求未初始化

创建了Ajax对象，但还没调用`open()方法`配置Ajax对象

- **1** ：请求成立，但还没发送

创建并配置了Ajax对象，但是还没调用`send()方法`发送请求

- **2**  :  请求已经发送

- **3** ：请求正在处理中

正在接受服务器端数据，部分数据已经可以使用

- **4** ： 响应已经完成

客户端已经可以使用响应的数据

---

### 获得Ajax状态码

```js
xhr.readyState
```

---

### onreadystatechange事件

```js
xhr.onreadystatechange=function(){
  console.log(xhr.readystate)
};
xht.send();
```

当Ajax状态码发生变化时触发事件

因为`send()`发送请求后状态码是在不断变化的，2——> 3——> 4

直接使用`readyState` 是看不到效果的，

此时需要在 `send()`发送前通过 `onreadystatechange`事件获得状态码

```html
    <script>
        var xhr = new XMLHttpRequest();
        console.log(xhr.readyState); // 0

        xhr.open('get', 'http:localhost:3000/readyState')
        console.log(xhr.readyState); // 1

        xhr.onreadystatechange = function() {
            console.log(xhr.readyState); // 2 3 4
        };
        xhr.send();
    </script>
```

---

### 监听状态码获取响应数据

不太常用，了解即可

```js
//判断状态码变化
xhr.onreadystatechange = function(){
  //如果状态码==4
  if(xhr.readyState==4){
    //打印获取的服务器响应的数据
    console.log(xhr.responseText)
  }
}
```

对Ajax的状态码进行判断

如果状态码是 4，说明数据已经接受完成了，就可以获取和使用

如下：

判断状态码，然后输出服务器响应的数据

```html
    <script>
        var xhr = new XMLHttpRequest();
        xhr.open('get', 'http://localhost:3000/readyState');

        xhr.onreadystatechange = function() {
            if (xhr.readyState == 4) {
                console.log(xhr.responseText);
            }
        };
        xhr.send()
    </script>
```



## 获取服务端响应数据的方法

![](https://img-blog.csdnimg.cn/20201028113008111.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI3NTc1OTI1,size_16,color_FFFFFF,t_70#pic_center)

```js
// onload
xhr.onload = function() {
    console.log(xhr.responseText);
}

// onreadystatechange
xhr.onreadystatechange = function(){
  if(xhr.readyState == 4){
    console.log(xhr.responseText);
  }
}
```

监听状态码`onreadystatechange`的方式不太常用，效率也低，仅项目需要再使用

推荐使用`onload`



## Ajax错误处理

即Ajax返回的不是预期的结果

![](https://img-blog.csdnimg.cn/2020102814103526.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI3NTc1OTI1,size_16,color_FFFFFF,t_70#pic_center)



### 判断http状态码

通过Ajxa对象的status属性获取http状态码

正常状态下是200

```js
xhr.status
```

然后根据http状态码的数值，对页面进行渲染提示用户

（具体http状态码设定的数值根据项目决定）

```html
    <script>
        var btn = document.getElementById('btn');
        btn.onclick = function() {
            var xhr = new XMLHttpRequest();
            xhr.open('get', 'http://localhost:3000/error');

            xhr.send();

            xhr.onload = function() {
                console.log(xhr.responseText);
                console.log(xhr.status);
            }
        }
    </script>
```

---

如下：

故意模拟Ajax过程中的错误，

在服务端将状态码设为400错误，

客户端发送AJax请求后，控制台会提示400错误

```js
app.get('/error', (req, res) => {
    res.status(505).send('not OK')
})
```

设定http状态码是400时，给用户提示

```html
    <script>
        var btn = document.getElementById('btn');
        btn.onclick = function() {
            var xhr = new XMLHttpRequest();
            xhr.open('get', 'http://localhost:3000/error');
            xhr.send();

            xhr.onload = function() {
                if(xhr.status==400){
                  alert('error，不能发送')
                }
            }
        }
    </script>
```



### 404

如果报错提示 `404（Not Found）`，说明请求地址出错



### 500

如过报错提示  `500 (Internal Server Error)`

代表服务器端错误



### 断网

网络中断时无法触发 `xhr.onload事件`

网络中断时会自动触发 `xhr.onerror事件`

可通过onerror事件对网络中断的错误进行处理

（可通过F12的network中的offline模拟网络中断）

```html
<body>
    <button id="btn">send</button>
    <script>
        var btn = document.getElementById('btn');
        btn.onclick = function() {
            var xhr = new XMLHttpRequest();
            xhr.open('get', 'http://localhost:3000/error');
            xhr.send();

            xhr.onload = function() {
                console.log(xhr.responseText);
                console.log(xhr.status);

            }
          
            xhr.onerror = function() {
                console.log('没网络，没法发送');
            }
        }
    </script>
</body>
```

---

### 区分Ajax状态码 和 http状态码

Ajax状态码

表示Ajax请求的过程，是Ajax对象返回



请求的处理结果

表示请求的处理结果，是服务器端返回



## IE低版本的缓存问题

[IE低版本的缓存问题](https://www.bilibili.com/video/BV1cv411i7GY?p=11&spm_id_from=pageDriver)



