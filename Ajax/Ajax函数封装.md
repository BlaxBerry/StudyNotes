# Ajxa函数封装

因为在项目中的Ajax请求不会只有一次，

多次单独发送Ajax请求会导致书写麻烦和代码冗余，

所以需要**将Ajax请求封装为一个函数**

使用时传递一个对象，属性分别是请求类型、请求地址、请求成功后执行的处理

- **type：**Ajax请求的请求类型

- **url：**Ajax请求的请求地址

- **success：**Ajax请求成功获得响应数据后的处理函数

```js
// 调用封装的Ajax函数
ajax({
  type: 'get/post',
  url: 'http://loaclhost:3000/test',
  success: function(data){
    console.log(data)
    
  }
})
```





## 基础封装与使用

```html
<script>
  //封装ajax函数
  function ajax(options){
    //创建XMLHttpRequest实例
    var xhr = new XMLHttpRequest();
    //配置ajax对象
    xhr.open(options.type, options.url);
    //发送请求
    xhr.send();
    //请求成功后，对获取响应数据的处理
    xhr.onload = function(){
      // 响应的数据传参给success函数
     	 options.success(xhr.responseText)
    }
    
  };
  
  //调用ajax函数
  ajax({
    type: 'get',
    url: 'http://localhost:3000/test',
    success: function(data){
      console.log(data)
    }
  })
</script>
```





## 传递请求参数

调用封装的Ajax函数时，传递进去请求参数

```js
// 调用封装的Ajax函数
ajax({
  type: 'get/post',
  url: 'http://loaclhost:3000/test',
  params: {}
  success: function(data){
    console.log(data)
    
  }
})
```

在Ajax函数内部根据请求类型不同，再将请求参数放置在相应位置

---

---

### 请求参数的位置

#### GET请求

请求参数用键值对拼接在请求地址后

#### POST请求

请求参数放在send方法中

---

---

### 请求参数的格式

虽然参数有两种格式，但是传递参数给Ajax函数时更推荐JSON对象格式

在Ajax函数内部对象类型数据转换为字符串更加方便

```js
// 调用封装的Ajax函数
ajax({
  type: 'get/post',
  url: 'http://loaclhost:3000/test',
  params: {
    name: 'Andy',
    age: 28
  }
  success: function(data){
    console.log(data)
    
  }
})
```

#### application/x-www-form-urlencoded 格式

格式：**参数名称=参数值&参数名称=参数值**

```js
name=andy&age=28
```

如下：向服务端传递application/x-www-form-urlencoded 格式的参数

```js
function ajax(options) {
    var xhr = new XMLHttpRequest();

    var params = '';

    for (var attr in options.data) {
        params += attr + '=' + options.data[attr] + '&'
    };
    // console.log(params);
    // 截取字符串最后一个字符
    params = params.substr(0, params.length - 1);

    if (options.type == 'get') {
        options.url = options.url + '?' + params;
    };
    xhr.open(options.type, options.url);

    if (options.type == 'post') {
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
        xhr.send(params);
    } else {
        xhr.send();
    };

    xhr.onload = function() {
        options.success(xhr.responseText);
    }
};


// GET 请求
ajax({
    type: 'get',
    url: 'http://localhost:3000/get',
    data: {
        name: 'Andy',
        age: 28
    },
    success: function(data) {
        console.log(data);
    }
})
--------------------------------------------
//POST 请求
ajax({
    type: 'post',
    url: 'http://localhost:3000/post',
    data: {
        name: 'Andy',
        age: 28
    },
    success: function(data) {
        console.log(data);
    }
})
```

```js
//Node.js
const express = require('express');
const path = require('path');
const app = express();

const bodyParser = require('body-parser');
// application/x-www-form-urlencoded 格式
app.use(bodyParser.urlencoded());

app.use(express.static(path.join(__dirname, 'public')));


app.get('/get', (req, res) => {
    res.send(req.query)
})
app.post('/post', (req, res) => {
    res.send(req.body)
})
```

---

#### application/json 格式

格式：**{参数名称 : 参数值, 参数名称 : 参数值}**

```js
{name:"andy", age:28}
```

如下：向服务端传递json格式的参数

```js
function ajax(options) {
    var xhr = new XMLHttpRequest();

    var params = '';

    for (var attr in options.data) {
        params += attr + '=' + options.data[attr] + '&'
    };
    // console.log(params);
    // 截取字符串最后一个字符
    params = params.substr(0, params.length - 1);
  
    if (options.type == 'get') {
        options.url = options.url + '?' + params;
    };
    xhr.open(options.type, options.url);

    if (options.type == 'post') {
        xhr.setRequestHeader('Content-Type', 'application/json')
        xhr.send(JSON.stringify(options.data));
    } else {
        xhr.send();
    };
    xhr.onload = function() {
        options.success(xhr.responseText);
    }
};

// GET 请求
ajax({
    type: 'get',
    url: 'http://localhost:3000/get',
    data: {
        name: 'Andy',
        age: 28
    },
    success: function(data) {
        console.log(data);
    }
})
------------------------------------------------
//POST 请求
ajax({
    type: 'post',
    url: 'http://localhost:3000/post',
    data: {
        name: 'Andy',
        age: 28
    },
    success: function(data) {
        console.log(data);
    }
})
```

```js
//Node.js
const express = require('express');
const path = require('path');
const app = express();

const bodyParser = require('body-parser');
// application/json 格式
app.use(bodyParser.json())

app.use(express.static(path.join(__dirname, 'public')));


app.get('/get', (req, res) => {
    res.send(req.query)
})
app.post('/post', (req, res) => {
    res.send(req.body)
})
```



#### 设置请求头为参数

可以在调用Ajax函数并传递参数时，在参数中设定请求的请求头中的数据类型

```js
ajax({
    type: 'get/post',
    url: 'http://localhost:3000/get',
    data: {
        name: 'Andy',
        age: 28
    },
    header: 'application/x-www-from-urlencoded'/'application/json',
    success: function(data) {
        console.log('get', data);
    }
})
```

如下：

```js
function ajax(options) {
    var xhr = new XMLHttpRequest();

    if (options.type == 'get') {
        var params = '';
        for (var attr in options.data) {
            params += attr + '=' + options.data[attr] + '&';
        };
        params = params.substr(0, params.length - 1);
        options.url = options.url + '?' + params;
    };
  
    xhr.open(options.type, options.url);

    if (options.type == 'post') {
      
        xhr.setRequestHeader('Content-Type', options.header)

        if (options.header == "application/x-www-from-urlencode") {
            xhr.send(params);
        } else if (options.header == "application/json") {
            xhr.send(JSON.stringify(options.data))
        }
    } else {
        xhr.send();
    };

    xhr.onload = function() {
        options.success(xhr.responseText)
    }
};


// GET 请求
ajax({
    type: 'get',
    url: 'http://localhost:3000/get',
    data: {
        name: 'Andy',
        age: 28
    },
    header: 'application/x-www-from-urlencoded',
    success: function(data) {
        console.log('get', data);
    }
})
-----------------------------------------
//POST 请求
ajax({
    type: 'post',
    url: 'http://localhost:3000/post',
    data: {
        name: 'Andy',
        age: 28
    },
    header: 'application/json',
    success: function(data) {
        console.log('post', data);
    }
})
```





## 分别处理请求成功与失败

XMLHtpRequest方法实例的onload方法被触发执行，

只是说明当前的Ajax请求完成了，并不是意味着该次Ajax请求是成功的，

所以需要判断`http状态码`

分别在状态是200和不是200的场合进行函数判断处理，

错误的场合的处理也需要调用Ajax函数时作为参数的属性传入：

```js
ajax({
    type: 'get',
    url: 'http://localhost:3000/post',
    data: {
        name: 'Andy',
        age: 28
    },
    header: 'application/json',
    success: function(data,xhr) {
        console.log(data,xhr);
    },
    error: function(data,xhr) {
        console.log(data,xhr);
    }，
})
```

```js
function ajax(options) {
    var xhr = new XMLHttpRequest();
    if (options.type == 'get') {
        var params = '';
        for (var attr in options.data) {
            params += attr + '=' + options.data[attr] + '&';
        };
        params = params.substr(0, params.length - 1);
        options.url = options.url + '?' + params;
    };
    xhr.open(options.type, options.url);
    if (options.type == 'post') {   
        xhr.setRequestHeader('Content-Type', options.header)
        if (options.header == "application/x-www-from-urlencode") {
            xhr.send(params);
        } else if (options.header == "application/json") {
            xhr.send(JSON.stringify(options.data))
        }
    } else {
        xhr.send();
    };
  
    xhr.onload = function() {
      
      if(xhr.status==200){
        options.success(xhr.responseText,xhr)        
      }else{
        options.error(xhr.responseText,xhr)
      }

    }
};

--------------------
ajax({
    type: 'get',
    url: 'http://localhost:3000/post',
    data: {
        name: 'Andy',
        age: 28
    },
    header: 'application/json',
    success: function(data,xhr) {
        console.log(data,xhr);
    },
    error: function(data,xhr) {
        console.log(data,xhr);
    }
})
```





## 响应数据的类型

一般情况下服务器返回的数据是JSON类型，然后客户端需要通过JSON.parse转化

但是在不确定的情况下还是要进行判断服务器端返回的数据类型

通过`xhr.getResponseHeader()`判断响应头中的数据类型，

然后决定是否进行数据类型转换

```js
xhr.onload = function(){
  xhr.getResponseHeader('Content-Type');
}
```

通过 `str.includes()` 判断返回的响应数据的响应头中是否包含指定的格式类型

```js
var contentType = xhr.getResponseHeader('Content-Type');
if(contentType.includes('application/json')){
  // 包含，响应数据的类型是 application/json格式
  var responseText = JSON.parse(xhr.responseText);
  
}else{
  // 不包含，响应数据的类型是 application/json格式
  var responseText = xhr.responseText;
}
```



如下：

```js
function ajax(options) {
    var xhr = new XMLHttpRequest();

    if (options.type == 'get') {
        var params = '';
        for (var attr in options.data) {
            params += attr + '=' + options.data[attr] + '&';
        };
        params = params.substr(0, params.length - 1);
        options.url = options.url + '?' + params;
    };
    xhr.open(options.type, options.url);

    if (options.type == 'post') {

        xhr.setRequestHeader('Content-Type', options.header)

        if (options.header == "application/x-www-from-urlencode") {

            xhr.send(params);
        } else if (options.header == "application/json") {

            xhr.send(JSON.stringify(options.data))
        }
    } else {
        xhr.send();
    };


    xhr.onload = function() {

        var contentType = xhr.getResponseHeader('Content-Type');

        if (contentType.includes('application/json')) {

            var responseText = JSON.parse(xhr.responseText);

        } else {

            var responseText = xhr.responseText;
        }

        if (xhr.status == 200) {
            options.success(responseText, xhr)
        } else {
            options.error(responseText, xhr)
        }
    }
}


ajax({
    type: 'get',
    url: 'http://localhost:3000/first',
    data: {
        name: 'Andy',
        age: 28
    },
    header: 'application/x-www-from-urlencoded',
    success: function(data, xhr) {
        console.log(data, xhr);
    },
    error: function(data, xhr) {
        console.log(data, xhr);
    }
})
-------------------
ajax({
    type: 'get',
    url: 'http://localhost:3000/json',
    data: {
        name: 'Andy',
        age: 28
    },
    header: 'application/json',
    success: function(data, xhr) {
        console.log(data, xhr);
    },
    error: function(data, xhr) {
        console.log(data, xhr);
    }
})
```

```js
//Node.js
const express = require('express');
const path = require('path');
const app = express();

const bodyParser = require('body-parser');
// application/json 格式
app.use(bodyParser.json())

app.use(express.static(path.join(__dirname, 'public')));


app.get('/firse', (req, res) => {
    res.send('hello i am string text')
})
app.post('/json', (req, res) => {
    res.send{{'name':'Andy','age':28})
})
```





## 调用Ajxa函数时传入参数默认值

上述调用Ajax函数时传入的参数太多了，每次调用Ajax函数时写起来也麻烦

所以可以考虑设定默认值

```js
ajax({
    type: 'get', 
    url: 'http://localhost:3000/json',
    data: {
        name: 'Andy',
        age: 28
    },
    header: 'application/json',
    success: function(data, xhr) {
        console.log(data, xhr);
    },
    error: function(data, xhr) {
        console.log(data, xhr);
    }
})
```

在封装的Ajax函数中定义对象，设定默认值

没有传入的就是用默认值，

传入的久覆盖默认值，

使用`Object.assign(被覆盖，覆盖)`,使传入的参数覆盖默认值

```js
function ajax(options){
  
  var defaults = {
    type: 'get',
    url: '',
    data: {},
    header: 'application/x-www-from-urlencoded',
    success: function(data, xhr) {},
    error: function(data, xhr) {}
  };
  
  Object.assign(defaults, options)
}

```

如下：

```js
function ajax(options) {
    var xhr = new XMLHttpRequest();

    var defaults = {
        type: 'get',
        url: '',
        data: {},
        header: 'application/x-www-from-urlencoded',
        success: function(data, xhr) {},
        error: function(data, xhr) {}
    };
    Object.assign(defaults, options)

    if (defaults.type == 'get') {
        var params = '';
        for (var attr in defaults.data) {
            params += attr + '=' + defaults.data[attr] + '&';
        };
        params = params.substr(0, params.length - 1);
        defaults.url = defaults.url + '?' + params;
    };
    xhr.open(defaults.type, defaults.url);

    if (defaults.type == 'post') {

        xhr.setRequestHeader('Content-Type', defaults.header)

        if (defaults.header == "application/x-www-from-urlencode") {

            xhr.send(params);
        } else if (defaults.header == "application/json") {

            xhr.send(JSON.stringify(defaults.data))
        }
    } else {
        xhr.send();
    };


    xhr.onload = function() {

        // console.log(xhr.getResponseHeader('Content-Type'));
        var contentType = xhr.getResponseHeader('Content-Type');

        if (contentType.includes('application/json')) {

            // 包含，响应数据的类型是 application/json格式
            var responseText = JSON.parse(xhr.responseText);

        } else {
            // 不包含，响应数据的类型是 application/json格式
            var responseText = xhr.responseText;
        }


        if (xhr.status == 200) {
            defaults.success(responseText, xhr)
        } else {
            defaults.error(responseText, xhr)
        }
    }
}


ajax({
    url: 'http://localhost:3000/first',
    success: function(data, xhr) {
        console.log(data, xhr);
    },
    error: function(data, xhr) {
        console.log(data, xhr);
    }
})
--------------------------------------------
ajax({
    type: 'post',
    url: 'http://localhost:3000/post',
    data: {
        name: 'Andy',
        age: 28
    },
    header: 'application/json',
    success: function(data, xhr) {
        console.log(data, xhr);
    },
    error: function(data, xhr) {
        console.log(data, xhr);
    }
})
```