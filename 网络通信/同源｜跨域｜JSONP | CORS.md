# 同源 跨域  JSONP



## 同源

![](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fhiphotos.baidu.com%2Flu920508536%2Fpic%2Fitem%2F2ae897c77d1ed21bd8ac054aad6eddc450da3fbd.jpg&refer=http%3A%2F%2Fhiphotos.baidu.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1625413416&t=f9ea2f09207e15d05df2a3e1db8cbe93)

若两个页面有相同的 **协议、域名、端口**，则为同源

则这两个页面有相同的源，即从同一个服务器上提供的资源

**端口号默认80**

---

### 同源策略

同源策略（Same Origin Policy）

是浏览器提供的一个安全功能

是用于隔离潜在恶意文件的重要安全机制

> “ 同源策略限制了从同一个源加载的 文档或脚本 如何与来自另一个源的资源进行交互”
>
> MDN官方

即，A网站的脚本JavaScript不允许和非同源的网站B之间进行资源的交互

1. 无法读取非同源网站的Cookie、LocalStorage、IndexedDM
2. 无法接触非同源网站的DOM
3. 无法向非同源地址发送Ajax请求





## 跨域

若两个页面的 **协议、域名、端口** 一致，则为跨域；

若两个页面的 **协议、域名、端口** 不同，则为跨域

### 出现跨域的原因

浏览器的**同源策略**不允许非同源的URL之间进行资源交互

---

### 跨域请求报错提示

**Access-Control-Allow-Origin**

---

### 浏览器拦截跨域请求

比如：

网页**http://www.test.com/index.html**

向接口**http://www.api.com/list**发送Ajax数据请求时

浏览器允许发起跨域请求，Ajax可以正常发起

服务器可以跨域响应回数据，

但是因为浏览器的同源策略，

响应回的数据会被浏览器拦截，无法被页面获取Ajax也无法获取数据





## 跨域请求的实现方案

实现跨域数据请求的方案有两种：**JSONP** 和 **CORS**

- **JSONP：**

  出现时间早，兼容性好

  非官方解决跨域Ajax的方案

  但**只支持GET请求**，不支持POST请求

- **CORS：**

  **主流解决方案**

  出现时间晚，低版本浏览器兼容差

  是W3C标准，是跨域Ajax的根本解决方案
  
  **支持GET请求 和 POST请求**
  
  详见 [Express笔记](https://github.com/BlaxBerry/StudyNotes/blob/master/Node.js/Express.md)





## JSONP

### 基础概念

JSONP（**JSON** with **P**adding）是JSON的一种使用模式

可用于主流浏览器的跨域数据访问问题

- **JSONP并不属于Ajax请求**，因为没有使用到XMLHttpRequest对象

- JSONP是脚本请求，通过script标签发送被当作一个JS文件的请求

- **只支持GET请求，不支持POST请求**

---

#### 实现原理

**通过<script\>标签的src属性请求跨域数据接口，**

**服务器返回的是一个函数的调用结果来响应数据**

由于浏览器的同源策略，网页无法通过Ajax请求获取非同源的接口数据

但是<script\>标签不受到浏览器同源策略的影响

所以可以通过**src属性**，请求非同源的JS脚本

详见笔记 [Express写JSONP接口](https://github.com/BlaxBerry/StudyNotes/blob/master/Node.js/Express.md)

---

#### 实现步骤

1. 定义回调函数

2. 页面中定义script标签

3. 通过src属性请求一个接口

4. 通过查询字符串的形式，

   在接口地址后拼接上回调函数

   也可携带上请求参数

```html
<script>
	function success(dtat){
    // 获取服务器响应的数据
    console.log(data)
  }
</script>
<script src="XXXXXX?callback=success&name=andy&age=28"></script>
```

---

#### 缺点

由于JSONP是通过Script标签的src属性获取跨域请求数据，

**只支持GET请求，不支持POST请求**



### jQuery中的JSONP

jQuery的 **$.ajax()**函数，除了可以发起Ajax数据请求。

还可以发情JSONP数据请求

---

#### jQuery中JSONP实现原理

也是通过<script\>标签的src属性实现跨域数据请求

只不过jQuery是通过动态创建和移除<script\>标签的方式发情JSONP请求

1. 发起JSONP请求时，

   动态向<header\>标签中append一个<script\>标签

2. JSONP请求成功时，

   动态从<header\>标签中移除刚刚添加的<script\>标签

---

#### 发送JSONP请求

通过URL请求地址中的查询字符串传递GET请求参数

通过dataType属性指定请求为jsonp数据请求而不是Ajax数据请求

```js
$.ajax({
  type: 'GET',
  url: 'XXXXX?key=value&key=value',
  dataType: 'jsonp',
  success: function(res){
    console.log(res)
  }
})
```

---

#### 请求参数callback

使用jQuery发起JSONP请求时，

会自动携带一个 **callback**参数，

值是jQuery随机生成的回调函数名 **jqueryXXXX**

```http
http://xxxxx?key=value&key=value&callback=jqueryxxxxx
```

---

#### 自定义callback回调函数名

使用jQuery发起JSONP请求时，

若想自定义JSONP请求参数和回调函数名

JSONP回调函数名一般默认callback

```js
$.ajax({
  url: 'xxxxx',
  dataType: 'jsonp',
  
  jsonp: 'callback',
  jsonpCallback: 'callbackFunction',
  
  success: function(res){
    console.log(res)
  }
})
```



### JSONP案例

#### 淘宝搜索框相关内容

art-templayte模版引擎 + JSONP

```html
<body>
    <input type="text" name="" id="" />
    <hr />
    <ul></ul>
</body>

<script type="text/html" id="art">
    {{each result}}
    <li>{{$value[0]}}</li>
    {{/each}}
</script>

<script>
    $(function () {
      $("input").on("keyup", function () {
        var val = $(this).val().trim();
        getData(val);
      });

      // JSONP 请求
      function getData(keyword) {
        $.ajax({
          url: "https://suggest.taobao.com/sug?q=" + keyword,
          dataType: "jsonp",
          success: function (res) {
            // console.log(res);
            renderUI(res);
          },
        });
      }

      // 渲染UI
      function renderUI(res) {
        if (res.result.length <= 0) {
          return $("ul").empty().hide();
        } else {
          var art = template("art", res);
          $("ul").html(art).show();
        }
      }
    });
</script>
```





## CORS 跨域资源共享

### 概念

CORS（Cross-Origin Resource Sharing）跨域资源共享，

是由一系列**HTTP响应头**组成，通过在服务端设置CORS的响应头，

决定浏览器是否阻止前端JS代码跨域获取资源

原本浏览器出于同源安全策略会默认阻止网页跨域获取资源，

但若API接口**服务器**配置了CORS相关的HTTP响应头，

就可以解除的浏览器跨域访问限制

![](https://pbs.twimg.com/media/E3crZQUVoAsrtXo?format=jpg&name=medium)



### 注意点

- **CORS主要配置在服务端**，

  客户端无需做配置

  详见笔记 [Express接口开启CORS](https://github.com/BlaxBerry/StudyNotes/blob/master/Node.js/Express.md)

- CORS在浏览器中有兼容性

  只有支持**XMLHttpRequest Level 2**的浏览器才可正常访问开启了CORS的服务器端口

  （**IE10+、Chrome4+、FireFox3.5+**）



### CORS响应头

CORS由一系列**HTTP响应头**组成，

通过在服务端设置CORS的响应头，

决定浏览器是否阻止前端JS代码跨域获取资源

---

####  Access-Control-Allow-Origin

响应头中可携带一个Access-Control-Allow-Origin字段：

比如，**只允许**来自 http://123.com 的跨域请求

```js
res.setHeader('Access-Control-Allow-Origin', 'http://123.com')
```

比如，允许来自所有域名的跨域请求

```js
res.setHeader('Access-Control-Allow-Origin', '*')
```

---

####  Access-Control-Allow-Headers

默认情况下，CORS仅支持客户端向服务器发送**9个**请求头：

- Accept
- Accept-Language
- Contnet-Type
  - text/plain
  - mulitpart/form-data
  - application/x-www-form-urlencoded
- Content-Language
- DPR
- DownLink
- Save-Data
- Viewport-Width
- Width

若客户端向服务器发送了**额外的请求头信息**，

则需要在服务器端通过 Access-Control-Allow-Headers 对额外请求头进行声明，

否则该次请求会失败

```js
res.setHeader('Access-Control-Allow-Headers', 'Content-Type, X-Custom-Header' )
```

---

####  Access-Control-Allow-Methods

默认情况下，CORS仅支持客户端发起 **GET、POST、HEAD** 请求

若客户端希望通过 **PUT、DELETE** 等请求方式，

则需要在服务器端通过 Access-Control-Allow-Methods声明允许的请求方法

否则该次请求会失败

比如：声明允许 **GET、POST、PUT、DELETE** 方法：

```js
res.setHeader(' Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE')
```

比如：声明允许所有的HTTP请求方式：

```js
res.setHeader(' Access-Control-Allow-Methods', '*')
```





### CORS请求分类

CORS跨域资源请求，根据**请求方式**和**请求头**的不同，分为：

- **简单请求**
- **预检请求**

---

#### 简单请求

客户端与服务器之间只会发生一次请求

简单请求必须同时满足以下所有条件：

- **请求方式**必须是 **GET / POST / HEAD** 之一

- HTTP请求头信息不能包含自定义字段，

  且不能超过默认的**9个**请求头：

  - Accept
  - Accept-Language
  - Contnet-Type
    - text/plain
    - mulitpart/form-data
    - application/x-www-form-urlencoded
  - Content-Language
  - DPR
  - DownLink
  - Save-Data
  - Viewport-Width
  - Width

---

#### 预检请求

客户端与服务器之间会发生两次请求

浏览器于服务器之间正式通信前，先发送一次OPTION请求进行预检，

服务器响应预检请求后客户端才会发送真正的请求

（可通过FireFox浏览器的network查看）

简单请求只要满足以下任意条件：

- **请求方式**是 **GET / POST / HEAD** 以外的HTTP请求方式
- 请求头中包含自定义字段
- 向服务器发送了 **application/json**格式的数据