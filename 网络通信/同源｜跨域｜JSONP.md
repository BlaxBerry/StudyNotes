# 同源 跨域  JSONP



## 同源含义

![](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fhiphotos.baidu.com%2Flu920508536%2Fpic%2Fitem%2F2ae897c77d1ed21bd8ac054aad6eddc450da3fbd.jpg&refer=http%3A%2F%2Fhiphotos.baidu.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1625413416&t=f9ea2f09207e15d05df2a3e1db8cbe93)

若两个页面有相同的 **协议、域名、端口**，则为同源

则这两个页面有相同的源，即从同一个服务器上提供的资源

**端口号默认80**





## 同源策略

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

### 跨域请求报错提示：

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





## 实现跨域请求

实现跨域数据请求的方案有两种：**JSONP** 和 **CORS**

- **JSONP：**

  出现时间早，兼容性好

  非官方解决跨域Ajax的方案

  但**只支持GET请求**，不支持POST请求

- **CROS：**

  出现时间晚，低版本浏览器兼容差

  是W3C标准，是跨域Ajax的根本解决方案

  **支持GET请求 和 POST请求**





## JSPNP

JSONP（**JSON** with **P**adding）是JSON的一种使用模式

可用于主流浏览器的跨域数据访问问题

**JSONP并不属于Ajax请求**

JSONP是脚本请求，是通过script标签发送请求，被当作一个JS文件的请求

---

### 实现原理

**通过<script\>标签的src属性请求跨域数据接口，**

**并通过函数调用接收跨域接口响应回的数据**

由于浏览器的同源策略，网页无法通过Ajax请求获取非同源的接口数据

但是<script\>标签不受到浏览器同源策略的影响

所以可以通过**src属性**，请求非同源的JS脚本

---

### 实现步骤

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

### 缺点

由于JSONP是通过Script标签的src属性获取跨域请求数据，

**只支持GET请求，不支持POST请求**





## jQuery中JSONP实现原理

也是通过<script\>标签的src属性实现跨域数据请求

只不过jQuery是通过动态创建和移除<script\>标签的方式发情JSONP请求

- 发起JSONP请求时，

  会动态向<header\>标签中append一个<script\>标签

- JSONP请求成功时，

  会动态从<header\>标签中移除刚刚添加的<script\>标签





## jQuery中的JSONP

jQuery的 **$.ajax()**函数，除了可以发起Ajax数据请求。

还可以发情JSONP数据请求

### 发送JSONP请求

通过URL请求地址中的查询字符串传递GET请求参数

通过dataType属性指定请求为jsonp数据请求而不是Ajax数据请求

```js
$.ajax({
  url: 'XXXXX?key=value&key=value',
  dataType: 'jsonp',
  success: function(res){
    console.log(res)
  }
})
```

---

### 请求参数callback

使用jQuery发起JSONP请求时，

会自动携带一个 **callback**参数，

值是jQuery随机生成的回调函数名 **jqueryXXXX**

```http
http://xxxxx?key=value&key=value&callback=jqueryxxxxx
```

---

### 自定义callback回调函数名

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





## 淘宝搜索框相关内容

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









防抖节流