

# Ajax基础

![](https://www.aurigait.com/resources/files/2016/11/ajax_logo.png)



## 传统网站的缺点

即SSR，服务器渲染页面的网站

- 网速慢的情况下，页面加载时间过长，甚至加载不出来

- 页面的公共部分的加载渲染回浪费资源和时间

- 表单提交后一旦出错，一般会重定向回表单填写页面

  会**重新刷新页面表单**，导致需要重新从头填写表单





## Ajax简介

Ajax（ **A**synchronous **J**avascript **A**nd **X**ML，**异步**JavaScript和XML）

是浏览器提供的一个方法

用于在不刷新页面的情况下向浏览器发悚请求获取数据，提高了用户体验

在不刷新页面的情况下，通过一个URL地址获取服务器的数据，然后再进行页面的**局部刷新**

常用于：

- 搜索框提示文字
- 表单数据验证（比如填写过程中的邮箱验证）
- 页面上拉加载更多内容
- 列表分页

---

Ajax是异步，因为请求会有耗时，为了不阻塞代码

---

虽然是客户端的 JavaScript代码，但**Ajax必须运行在网站环境中才生效**

即运行Ajax的html文件必须在服务器环境下运行，用 localhost域名打开，

不然双击打开html文件的话Ajax代码无法生效





## 运行原理

![](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=625365557,280187035&fm=26&gp=0.jpg)

1. 浏览器通过xhr发送ajax请求给服务器

2. 服务器响应回 **JSON对象格式**的数据

3. 浏览器解析获取的**字符串形式的JSON数据**

   HTTP请求和响应过程中，数据都是以字符串形式传递

   ```js
   JSON.parse(字符串形式的JSON数据)

4. 与浏览器将获取的数据通过字符串拼接后，通过DOM操作渲染









## HTTP请求参数

传统网站中的请求参数：

- GET请求参数：请求URL地址中，以查询字符串的形式传递

  ```http
  http://localhost:80/user?id=123&name=andy
  ```

- POST请求参数：通过表单提交后，被自动存于请求体中传递

  传统网站SSR的提交表单无法传递JSON格式数据

Ajax请求的参数：

- GET请求参数：在设置请求地址时以查询字符串的形式

  ```js
  xhr.open('get','http://localhost:80/user?id=123&name=andy')
  ```

- POST请求参数：

  - **键值对格式**的查询字符串（**key=value&key=value**）

    设置请求头中数据类型**application/x-www-form-urlencoded**，

    然后通过send发送查询字符串

    ```js
    xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
    xhr.send('name=andy&age=28');
    ```

  - **JSON格式**请求参数（**{ key:value, key:value }**）

    设置请求头中数据类型**application/json**，

    HTTP请求和响应过程中，数据都是以字符串形式传递

    必须先转换为字符串形式后再通过send发送查询字符串

    ```js
    xhr.setRequestHeader('Content-Type','application/json');
    xhr.send(JSON.stringify({name:adny, age:28}));
    ```

  





## HTTP状态码

正常状态下是200

- **404**：如果报错提示 `404（Not Found）`，说明请求地址出错

- **500**：如过报错提示  `500 (Internal Server Error)，代表服务器端错误

---

### xhr.status

通过Ajxa对象的status属性获取http状态码，

```js
xhr.status
```







## Ajax状态码

表示Ajax请求的过程的状态，Ajax请求过程中状态码是在不断变化的

Ajax的请求的每一个步骤：

> 创建Ajax对象——>
>
> 配置Ajax对象——>
>
> 发送Ajax请求——>
>
> 接受服务器端的响应数据——>
>
> Ajax请求完成

该过程中的每一个步骤都对应一个状态码

Ajax状态码共有5个：

| 状态码 |                      含义                       |
| :----: | :---------------------------------------------: |
| **0**  |  创建了**xhr对象**，但还没调用 **open()** 配置  |
| **1**  | **open()** 被调用，但是还没 **send()** 发送请求 |
| **2**  |    **send()** 发送了请求，响应头被服务器接收    |
| **3**  | 浏览器正在接受数据，response属性已包含部分数据  |
| **4**  |  Ajax请求完成（ 数据传输成功 / 数据传输失败 ）  |

---

### xhr.readyState

可通过Ajax对象的readyState属性，获得Ajax状态码

```js
xhr.readyState
```

需要在 `send()`发送前，通过 `onreadystatechange`事件获得状态码

`send()`发送请求后状态码是在不断变化的

```js
xhr.onreadystatechange=function(){
  console.log(xhr.readystate)
};
xht.send();
```

---

### Ajax状态码，http状态码

Ajax状态码：

表示Ajax请求的过程，是Ajax对象返回

http状态码：

表示HTTP请求的处理结果，是服务器端返回









## Ajax获取数据的两种方式

![](https://img-blog.csdnimg.cn/20201028113008111.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI3NTc1OTI1,size_16,color_FFFFFF,t_70#pic_center)

### onload事件（推荐）

```js
var xhr = new XMLHttpRequest();
xhr.open('请求方式', '请求地址');

xht.send();

xhr.onload = function() {
  console.log(xhr.responseText);
};
```

### onreadystatechange事件（旧）

监听Ajax状态码的变化，每次变化就调用，效率相对较差

`send()`发送请求后状态码是在不断变化的

```js
var xhr = new XMLHttpRequest();
xhr.open('请求方式', '请求地址');

xhr.onreadystatechange = function(){
  if (xhr.readyState == 4 && xhr.status == 200) {
    console.log(xhr.responseText);
  }
};

xht.send();
```







## Ajax在低版本IE浏览器的缓存问题

在低版本IE浏览器中，Ajax请求有严重的**缓存问题**。

多次请求地址相同时，只有第一次的请求成功发送到了服务器，

第一次请求后的请求都是从浏览器缓存中获取

所以，即使服务器数据更新了客户端获取的仍是缓存中的旧数据

解决方案：可以通过**给请求地址添加不同的GET请求参数**

```js
xhr.open('请求方式','请求地址?随意不重复的参数名='+ Math.random())
```









# 各种Ajax请求

## XHR

原生的最原始的Ajax

通过window上的JavaScript内置构造函数 **XMLHttpRequest**（XHR），

创建实例对象，创建最原始的Ajax对象

### 1. 发送步骤

onload事件获取服务器响应数据

```js
// 1. 创建Ajax对象
var xhr = new XMLHttpRequest()

// 2. 设定请求方式和请求地址
xhr.open('请求方式','请求地址')

// 3. 发送请求
xhr.send()

// 4. 异步获取数据
xhr.onload = function(){
  console.log(xhr.responseText)
  console.log(JSON.parse(xhr.responseText))
}
```

onreadystatechange事件监听Ajax状态码变化获取服务器响应数据

```js
// 1. 创建Ajax对象
var xhr = new XMLHttpRequest()

// 2. 设定请求方式和请求地址
xhr.open('请求方式','请求地址')

// 3. 监听Ajax状态码的变化
xhr.onreadystatechange = function(){
  if (xhr.readyState == 4 && xhr.status == 200) {
      console.log(xhr.responseText)
  		console.log(JSON.parse(xhr.responseText))
  }
};

// 4. 发送请求
xhr.send()
```



### 2. 请求参数

#### GET请求参数

在设置请求地址时以查询字符串的形式

```html
name: <input type="text" id='usrname'>
age: <input type="text" id='usrage'>
<input type="button" value="send" id='btn'>Send</button>


<script>
  var btn = document.getElementById('btn');
 	var usrname = document.getElementById('usrname').value;
	var usrage = document.getElementById('usrage').valeu;
  
  btn.onclick = function() {
    // 拼接查询字符串
    var params = 'usrname=' + name + '&usrage=' + age;
    
    // Ajax
    var xhr = new XMLHttpRequest();
    xhr.open('post','http://localhost:3000/find?'+ params);
    
    xhr.send();
   
    xhr.onload = function() {
      console.log(xhr.responseText);
    }
  }
</script>
```

---

#### POST请求参数

1. 键值对格式的查询字符串（**key=value&key=value**）

设置请求头中数据类型application/x-www-form-urlencoded，然后通过send发送查询字符串

```html
name: <input type="text" id='usrname'>
age: <input type="text" id='usrage'>
<input type="button" value="send" id='btn'>Send</button>


<script>
  var btn = document.getElementById('btn');
 	var usrname = document.getElementById('usrname').value;
	var usrage = document.getElementById('usrage').valeu;
  
  btn.onclick = function() {
    // 拼接查询字符串
    var params = 'usrname=' + name + '&usrage=' + age;
    
    // Ajax
    var xhr = new XMLHttpRequest();
    xhr.open('post','http://localhost:3000/find');
    
    xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
    
    xhr.send(params);
    
    xhr.onload = function() {
      console.log(xhr.responseText);
    }
  }
</script>
```

2. JSON格式请求参数（**{ key:value, key:value }**）

```html
name: <input type="text" id='usrname'>
age: <input type="text" id='usrage'>
<input type="button" value="send" id='btn'>Send</button>


<script>
  var btn = document.getElementById('btn');
 	var usrname = document.getElementById('usrname').value;
	var usrage = document.getElementById('usrage').valeu;
  
  btn.onclick = function() {
    // 设置JSON数据
    var params = {
      "name": name,
      "age": age
    };
    
    // Ajax
    var xhr = new XMLHttpRequest();
    xhr.open('post','http://localhost:3000/find');
    
    xhr.setRequestHeader('Content-Type','application/json');
    
    xhr.send(JSON.stringify(params));
    
    xhr.onload = function() {
      console.log(xhr.responseText);
    }
  }
</script>
```



### 断网时的Ajax错误处理

可通过**onerror事件**对网络中断的错误进行处理

```js
xhr.onerror = function() {
  console.log('没网络，没法发送');
}
```

网络中断时无法触发 `xhr.onload事件`，会自动触发 `xhr.onerror事件`

（可通过F12的network中的offline模拟网络中断）

```js
var xhr = new XMLHttpRequest();
xhr.open('请求方式', '请求地址');
xhr.send();

xhr.onload = function() {
  console.log(xhr.responseText);
  console.log(xhr.status);
}；

xhr.onerror = function() {
  console.log('没网络，没法发送');
}
```



### 封装Ajax函数

因为在项目中的Ajax请求不会只有一次，

多次单独发送Ajax请求会导致书写麻烦和代码冗余，

所以需要将Ajax请求封装为一个函数，使用时传递一个**对象**：

- **type：**请求类型
- **url：**请求地址
- **data:**  JSON格式的请求参数
- **success：**Ajax请求成功时对获得的数据的处理函数
- **error：**Ajax请求失败时的处理函数

```js
ajax({
  type: '请求方式',
  url: '请求地址',
  data: {
    key1: value1,
    key2: value2
  },
  success: function(data){
    console.log(data) 
  }
})
```

基础封装

```js
// 封装
function ajax(obj, dataType = "json") {
    var xhr = new XMLHttpRequest();

  	// GET 请求
    if (obj.type == "get") {
      var paramsGET = "";
      for (var key in obj.data) {
        paramsGET += key + "=" + obj.data[key] + "&";
      }
      paramsGET = paramsGET.substr(0, paramsGET.length - 1);
      xhr.open(obj.type, obj.url + "?" + paramsGET);
      xhr.send();
    } 
  
    // POST 请求
  	if (obj.type == "post") {
      var paramsPOST = "";
      xhr.open(obj.type, obj.url);
      if (dataType == "json") {
        // JSON格式的参数（该函数默认）
        paramsPOST = JSON.stringify(obj.data);
        xhr.setRequestHeader("Content-Type", "application/json");
      } else {
        // 查询字符串格式的参数
        for (var key in obj.data) {
          paramsPOST += key + "=" + obj.data[key] + "&";
        }
        paramsPOST = paramsPOST.substr(0, paramsPOST.length - 1);
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
      }
      xhr.send(paramsPOST);
    }
		
  	// 判断Ajax请求成功或失败
    xhr.onload = function () {
      if (xhr.status == 200) {
        obj.success(xhr.responseText);
      } else {
        obj.error(xhr.responseText);
      }
    };
  
}


// 调用发送 GET请求
ajax({
  type: "get",
  url: "http://localhost:3000/get",
  data: {
    name: "andy",
    age: 28,
  },
  success: function (data) {
    console.log(JSON.parse(data));
  },
  error: function (data) {
    console.log(JSON.parse(data));
  }
});

// 调用发送 POST请求
ajax({
  type: "post",
  url: "http://localhost:3000/post",
  data: {
    name: "andy",
    age: 28,
  },
  success: function (data) {
    console.log(JSON.parse(data));
  },
  error: function (data) {
    console.log(JSON.parse(data));
  }
}, '查询字符串形式');
```











## jQuery

$.get()  

$.post 

$.ajax()

是对xhr的封装，底层是**XMLHttpRequest**，简化了xhr的写法

但是容易出现**回调地狱**（Callback Hell）





### Axios

只专注于网络请求的库，比起jQuery更加轻量化

也是对xhr的封装

**Promise**风格，解决了回调地狱

请求返回的数据在response的data中

#### GET请求

```js
axios.get('请求地址', {
  params: {
    name: 'Andy'
  }
}).then(
	response=>{console.log('获取数据成功了',response.data)},
  error=>{console.log('获取数据失败了了',error)}
)
```

#### POST请求

```js
axios.post('请求地址', {
  name:'Andy'
}).then(
	response=>{console.log('获取数据成功了',response.data)},
  error=>{console.log('获取数据失败了了',error)}
)
```

#### 直接发起请求

jQuery $.ajax

```js
let obi = {name: 'andy'}

axios({
  method: '请求类型'，
  url: '',
  // POST数据
  data: obj,
  // GET数据
  params: {
  	name: 'Andy'
	},
}).then(res=>{
  console.log(res)
})
```





### Fetch

**不借助xhr**

不是第三方库不需要下载，window内置原生函数

**Promise**风格，解决了回调地狱

但是老版本浏览器不支持

---

fetch的设计思想是 **关注分离**，

第一步是判断**链接服务器**的成功或失败，

第二步才是判断**数据请求**成功或失败

```js
fetch('请求地址').then(
	response=>{console.log('链接服务器成功了',response)},
  error=>{console.log('链接失败了了',error)}
)
```

请求返回的数据存放第一次 then()中的response.json()

通过return返回出一个promise实例

若链接服务器成功，则返回response.json()

若链接服务器成功，则返回一个新的空的Promise实例对象

然后再通过第二个then()获取数据

```js
fetch('请求地址').then(
	response=>{
    console.log('链接服务器成功了')
    return response.json()
  },
  error=>{
    console.log('链接失败了了')
    return new Promise(()=>{})
  }
  
).then(
  response=>{'获取数据成功了',response},
  error=>{'获取数据失败了',error}
)
```

上述代码有冗余，还可以简化

删去重复的error错误处理，在最后统一用catch处理错误

```js
fetch('请求地址').then(
	response=>{
    console.log('链接服务器成功了')
    return response.json()
  }
  
).then(
  response=>{'获取数据成功了',response}
  
).catch(
	error=>{console.log('请求出错',error)}
)
```

还可继续简化

```js
try{
  const response = await fetch('请求地址')
  const data = await response.json()
  conso.log(data)
  
}catch(error){
  console.log('请求出错',error)
}
```

