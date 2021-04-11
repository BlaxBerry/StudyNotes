# Vue前后端交互

## 接口调用方式

- **原生Ajax**

- **jQuery的Ajax**

- **fetch**

- **axios**



## jQuery的Ajax

```js
$.ajax({
  url:'http://localhost:3000/data',
  success:function(data){
    console.log(data)
  }
})
```



## fetch

可看为传统Ajax的升级版，更简单

**基于Promise语法**

```js
fetch('URL').then(function(data){
  return data.text();
}).then(function(result){
  console.log(result)
})
```

其中的 `text()`方法是fetch的API，返回的是个Promise实例对象

然后用Promise的 `then()`方法返回结果



### fetch请求参数

method ：HTTP请求方法默认是GET

body ： HTTP请求参数

headers ：HTTP请求头

---

### GET请求（传统方式）

一般的写法是：

```js
fetch("URL?key=value&key=value").then(data=>{
  return data.text()
}).then(result=>{
  console.log(result)
})
```

---

如下：

```js
fetch('http://localhost:3000/get?name=andy&age=28').then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(result);
});
```

```js
// 服务端路由
app.get('/get', (req, res) => {
    res.send(req.query)
})
```



### GET请求（restful）

如下：

模拟get请求获取图书的ID

```js
fetch('http://localhost:3000/books/10000', {
    method: 'get'
}).then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(result);
    console.log(parseInt(result));
})
```

```js
// 服务端路由
app.get('/books/:id', (req, res) => {
    res.send(req.parmas.num)
})
```

请求地址是 

> localhost:3000/books/1000





### DELETE请求

如下：

模拟delete请求获取图书的ID

```js
fetch('http://localhost:3000/books/10000', {
    method: 'delete'
}).then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(result);
})
```

```js
// 服务端路由
app.delete('/books/:id', (req, res) => {
    res.send(req.parmas.num)
})
```

请求地址是 

> localhost:3000/books/1000





### POST请求

除了method属性设置请求方式

还要用body设置请求体和headers设置请求头

headers支持两种数据格式类型：

#### application/json 格式的数据

若POST提交的参数是JSON对象格式

如下：

```js
fetch('http://localhost:3000/books', {
    method: 'post',
    body: JSON.stringify({
        name: 'andy',
        age: 28
    }),
    headers: {
        'Content-Type': 'application/json'
    }
}).then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(JSON.parse(result));
  	console.log(JSON.parse(result).name);
})
```

```js
// 服务端路由
const bodyParser = require('body-parser');

app.use(bodyParser.json());

app.post('/books', (req, res) => {
    res.send(req.body)
})
```

#### application/x-www-form-urlencoded 格式的数据

若POST提交的参数是键值对格式

如下：

```js
fetch('http://localhost:3000/books', {
    method: 'post',
    body:'name=andy&age=28',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    }
}).then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(result);
})
```

```js
// 服务端路由
const bodyParser = require('body-parser');

app.use(bodyParser.urlencoded());

app.post('/books', (req, res) => {
    res.send(req.body)
})
```



### PUT请求

若PUT提交的参数是JSON对象格式

如下：

```js
fetch('http://localhost:3000/books', {
    method: 'put',
    body: JSON.stringify({
        name: 'andy',
        age: 28
    }),
    headers: {
        'Content-Type': 'application/json'
    }
}).then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(JSON.parse(result));
  	console.log(JSON.parse(result).name);
})
```

```js
// 服务端路由
const bodyParser = require('body-parser');

app.use(bodyParser.json());

app.put('/books', (req, res) => {
    res.send(req.body)
})
```



### 响应结果

text() ：将返回体处理成字符串类型

json() ：将返回体处理成JSON类型，和JSON.parse(responseText)一样

```js
fetch('URL').then(function(data){
  return data.json();
}).then(function(result){
  console.log(result)
})
```

```js
// 服务端路由
app.get('/books', (req, res) => {
    res.json({
      name:'andy',
      age:'28'
    })
})
```

---

如下：

```js
fetch('http://localhost:3000/data').then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(result);    //字符串类型
});

fetch('http://localhost:3000/data').then(function(data) {
    return data.json()
}).then(function(result) {
    console.log(result);    //JSON对象类型
});

fetch('http://localhost:3000/data').then(function(data) {
    return data.text()
}).then(function(result) {
    console.log(JSON.parse(result));    
});
```

```js
// 服务端路由
app.get('/data', (req, res) => {
    res.send({ 'name': 'Andy' })
});
```





## axios

基于Promise语法的HTTP客户端

比fetch更强大

- 可支持浏览器和node.js

- 支持Promise语法
- 拦截器，可拦截请求和响应
- 自动转换JSON数据



### 导入

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```



参数函数的参数是个对象，响应的结果是在参数的data属性中

```js
axios.get('URL').then(function(res){
  // 成功时的结果
  console.log(res.data);
},function(err){
  // 错误时的结果
  console.log(res.data);
})
```

如下：

用axios自带的 `get()` 方法发送get请求

```js
axios.get('http://localhost:3000/axios').then(function(res) {
    console.log(res.data);
}, function(err) {
    console.log(err);
})
```

```js
// 服务端路由地址
app.get('/axios', (req, res) => {
    res.send('hello Axios')
})
```