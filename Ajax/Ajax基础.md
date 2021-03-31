# Ajax技术

![](https://www.aurigait.com/resources/files/2016/11/ajax_logo.png)



## 主要作用

Ajax（ **A**synchronous **J**avascript **A**nd **X**ML，异步JavaScript和XML）

是指一种创建交互式网页应用的网页开发技术

主要用于获取服务器数据

比如用户名、邮箱的验证，

加载更多



## 效果

在不刷新页面的情况下，

通过一个URL地址获取服务器的数据，

然后再进行页面的**局部刷新**





**Ajax必须运行在网站环境中才能生效**

即 index.html文件必须在服务器环境下运行，用 localhost域名打开，

不然Ajax代码无法生效

 

比如

使用Node.js开启一个网站环境，把html文件放入static静态资源文件夹中

然后通过域名访问html文件

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

app.listen(3000);

console.log('server running at localhost:3000')
```





## 