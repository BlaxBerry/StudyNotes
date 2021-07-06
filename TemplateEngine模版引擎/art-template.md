# art- template

腾讯开发，速度最快的模版引擎

![](https://aui.github.io/art-template/images/chart@2x.png)

实现了：

- 直接在HTML结构（模版）中直接引入数据

- 不再需要手动以拼接字符串的形式去拼接HTML结构

模版 = HTML结构



## 安装引入

```bash
npm i art-template
```

```js
const template = require('art-template')
```



## 使用

告诉模版引擎要拼接的数据和模版在哪

```js
const html = template ('模版位置', 数据)
```

- 第一个参数是模版位置art文件，使用绝对路径

- 第二个参数是在模版中显示的数据，对象形式

- 返回值是字符串形式的，拼接了数据的HTML页面结构

```js
const template = require('art-template')
const path = require('path')

// 模版文件的绝对路径
const views = path.join(__dirname, 'views', 'index.art')

// 模版中插入数据
const html = template(views, {
    name: 'Andy',
    age: 28
})

console.log(html);
```









## 使用实例

### 目录结构

1. 创建views目录，存放模版们
2. 创建 .art文件，写模版

```js
xxx
|-node_modules
|- views
		|- index.art
|-index.js
|- package.json
|- package-lock.json
```

### 模版中显示数据

即，直接在HTML结构中直接引入数据

art文件就是 .html 文件，写完后改后缀即可

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
  
    <p> {{name}} </p>
    <p> {{age}} </p>
  
</body>
</html>
```

### 插入数据到模版

```js
const template = require('art-template')
const path = require('path')


const views = path.join(__dirname, 'views', 'index.art')

const html = template(views, {
    name: 'Andy',
    age: 28
})

// console.log(html);
```





## art-template模版语法

通过模版语法实现，数据和模版是如何拼接

有两种模版语法：

- **标准语法： {{ 数据 }}**

  容易阅读 

- **原始语法： <%= 数据 %>**

  处理能力强，可直接写JavaScript语法



### 输出数据

直接在HTML结构（模版）中直接引入数据

- 标准语法：

```html
<h2>{{ value }}</h2>
<h2>{{ a ? b : c }}</h2>
<h2>{{ a + b }}</h2>
```

- 原始语法：

```html
<%= value %>
<%= a ? b : c  %>
<%= a + b %>
```

---

- 标准语法：

```html
<body>
  
    <p> {{ name }} </p>
    <p> {{ age }} </p>
  	<p> {{ age + 1 }} </p>
    <p> {{ 1 + 1 == 2 ? '相对' : '不等'}} </p>
  
</body>
```

- 原始语法：

```html
<body>
  
    <p> <%= name %> </p>
    <p> <%= age %> </p>
    <p> <%= age + 1 %> </p>
    <p> <%= 1 + 1 == 2 ? '相对' : '不等' %> </p>
  
</body>
```

- 模版引擎插入数据：

```js
const template = require('art-template')
const path = require('path')


const views = path.join(__dirname, 'views', 'index.art')

const html = template(views, {
    name: 'Andy',
    age: 28
})

console.log(html)
// 返回值是字符串形式的，拼接了数据的HTML页面结构
```





### 输出标签

模版引擎默认不会解析标签

直接在模版中输出tag标签的话，一律被当作字符串放入模版

如下：

- HTML模版：

```html
<body>
  
    <div> {{ content }} </div>
  
</body>
```

- 模版引擎插入数据：

```js
const template = require('art-template')
const path = require('path')


const views = path.join(__dirname, 'views', 'index.art')

const html = template(views, {
    content:'<h1>我是h1标题</h1>'
})

console.log(html);
```

- 打印内容：

```html

<div> &#60;h1&#62;我是h1标题&#60;/h1&#62;</div>

<div> &#60;h1&#62;我是h1标题&#60;/h1&#62;</div>
```

所以需要需要通过原文输出语法，输出标签





### 原文输出

通过原文输出语法，转义解析标签，然后输出到HTML模版上

- 标准语法：

```html
<body>
  
    <div>{{@ content }}</div>
 
</body>
```

- 原始语法：

```html
<body>

    <div> <%- content %> </div>

</body>
```

- 模版引擎插入数据：

```js
const template = require('art-template')
const path = require('path')


const views = path.join(__dirname, 'views', 'index.art')

template(views, {
    content:'<h1>我是h1标题</h1>'
})
```





### 条件判断

根据条件判断显示那些HTML代码

- 标准语法

```html
<body>
  
    <!-- 若条件成立，则显示 ... 处的HTML-->
    {{ if 条件 }} ... {{ /if }}
 
  
  	<!-- 若条件1成立，则显示 ..1.. 处的HTML-->
  	<!-- 若条件2成立，则显示 ..2.. 处的HTML-->
  	<!-- 若条件都不成立，则显示 ..3.. 处的HTML-->
    {{ if 条件1 }} 
  		..1..  
  	{{ else if 条件2 }} 
  		..2.. 
  	{{ else }}
      ..3..
  	{{ /if }}
  	
</body>
```

- 原始语法

```html
<body>
  
    <!-- 若条件成立，则显示 ... 处的HTML-->
    <% if (条件) { %> 
      ... 
    <% } %>
 
  
  	<!-- 若条件1成立，则显示 ..1.. 处的HTML-->
  	<!-- 若条件2成立，则显示 ..2.. 处的HTML-->
  	<!-- 若条件都不成立，则显示 ..3.. 处的HTML-->
    <% if (条件1) { %> 
      ..1.. 
    <% } else if (条件2) { %> 
      ..2.. 
    <% } else { %>
      ..3..
    <% } %>
  	
</body>
```

---

比如：

- 标准语法：

```html
<body>
  
    {{ if age>18 }}
    		<p>{{ name }}</p>
    {{ /if }}

  
    {{ if age<18 }}
    		<p>未成年</p>
    {{ else if age>=18 && age<30 }}
    		<p>成年</p>
    {{ else }}
    		<p>老年</p>
    {{ /if }}

  </body>
```

- 原始语法：

```html
 <body>

    <% if (age>18) { %> 
        <p>{{ name }}</p>
    <% } %>
    
 
    <% if (age<18) { %> 
        <p>未成年</p>
    <% } else if (age>=18 && age < 30) { %> 
        <p>成年</p>
    <% } else { %>
        <p>老年</p>
    <% }%>
      
  </body>
```

- 模版引擎插入数据：

```js
const template = require('art-template')
const path = require('path')


const views = path.join(__dirname, 'views', 'index.art')

const html = template(views, {
    name: 'Andy',
    age: 28
})
```



### 循环

循环数据，展示数据

- 标准语法

```html
<body>
  
  	{{ each 目标数据 }}
  			<!-- $value $index 是固定写法-->
  			{{ $index }} {{ $value }}
  
  	{{ /each }}
  
</body>
```

- 原始语法

```html
<body>
  
  	<% for(var i = 0; i < 目标数据.length; i++) { %>
  
  		<%= i %> <%= i %>
  		<%= i %> <%= 目标数据[i] %>
  
  	<% } %>
  
</body>
```

---

如下：

- 标准语法

```html
<body>

    <ul>
        {{ each  users}}
           <li>
              序号: {{ $index + 1 }} 
              姓名: {{ $value.name }}
             	年龄: {{ $value.age }}
           </li>
        {{ /each }}
    </ul>

</body>
```

- 原始语法

```html
<body>

    <ul>
        <% for(var i = 0; i < users.length; i++){ %>
            <li>
                序号：<%= i+1 %>
                姓名：<%= users[i].name %>
                年龄：<%= users[i].age %>
            </li>
        <% } %>
    </ul>

</body>
```

- 模版引擎插入数据：

```js
const template = require('art-template')
const path = require('path')

const views = path.join(__dirname, 'views', 'index.art')

const html = template(views, {
    users: [
        {
            name: 'Andy',
            age: 28
        },
        {
            name: 'Tom',
            age: 25
        },
        {
            name: 'Jerry',
            age: 29
        }
    ]
})
```

---

#### 循环嵌套

```html
{{ each students}}
    <tr>
      <th>{{ $index + 1 }}</th>
      <th>{{ $value.id }}</th>
      
      <td>
        {{ each $value.skills}}
            <span> {{ $value }} </span>
        {{ /each }}
      </td>
 
    </tr>
{{ /each }}
```

---

#### 动态设置标签属性

直接在标签的属性中用插入数据即可

```html
{{ each students}}

	<!--img的src属性-->
	<img src="{{ $value.picsrc }}" alt="">

 	<!--a标签的href请求参数-->
  <a href="/update?id={{$value.id}}" class="btn"> Update </a>
  <a href="/delete?id={{$value.id}}" class="btn"> Delete </a>
   
{{ /each }}
```







### 子模版导入

将网站的公共HTML部分（头部底部）抽离到公共文件中，方便统一维护管理

抽离出来的公共部分就是子模版

通过子模版语法，将公共部分导入模版页面

- 标准语法

```html
<body>
  
  {{ include './common/header.art' }}
  
</body>
```

- 原始语法

```html
<body>
  
  <% include('./common/header.art') %>
  
</body>
```

---

比如：

- 标准语法

```html
<body>
  	<!-- 引入公共头部 -->
    {{ include '../common/header.art'}}

    <div class="content"></div>

  	<!-- 引入公共底部 -->
    {{ include '../common/footer.art'}}
</body>
```

- 原始语法

```html
<body>
  	<!-- 引入公共头部 -->
    <% include('../common/header.art')%>

    <div class="content"></div>

    <!-- 引入公共底部 -->
    <% include('../common/footer.art')%>
  
</body>
```

- 公共部分子模版

  - header.art

  ```html
  <div class="header">
    <h1>Header</h1>
  </div>
  ```

  - footer.art

  ```html
  <div class="footer">
    <h1>Footer</h1>
  </div>	
  ```

- 模版引擎插入数据：

```js
const template = require('art-template')
const path = require('path')

const views = path.join(__dirname, 'views', 'index.art')

const html = template(views, {})
```





### 骨架模版继承

将HTML页面的骨架<header\><body\>等部分抽离到一个公共文件，方便统一管理

模版继承即，继承HTML骨架文件

<img src="https://pbs.twimg.com/media/E4Yihu5UcAECWIp?format=jpg&name=medium" style="zoom:50%;" />

---

#### 骨架模版设置预留空位

需要在抽离出骨架模版时，需要空出预留位，供继承骨架模版的模版们充填

比如，引用的CSS和JS文件，页面标题，body内容等等

- 标准语法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  	{{ block '自定义预留位名'}}{{/block}}
</head>
<body>
    
  	{{ block '自定义预留位名'}}{{/block}}
  
</body>
</html>
```

---

#### 导入骨架模版 

```html

{{ extend '骨架模版art文件地址'}}

```

---

#### 填充预留空位

```html

{{block '预留位名'}}
	<!--该模版特有的部分-->
{{/block}}

```

---

比如：

- 设定骨架模版

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
  	<!--充填预留位 导入css文件-->
  	{{ block 'cssLink'}}{{/block}}
  	<!--充填预留位 导入js文件-->
    {{ block 'jsScript'}}{{/block}}
</head>
<body>
  
    <!--充填预留位 导入内容模版-->
  	{{ block 'content'}}{{/block}}
  
</body>
</html>
```

- 继承导入骨架模版 + 充填预留位

```html
<!--导入继承骨架模版-->
{{ extend '../common/bone.art'}}

<!--充填预留位 导入css文件-->
{{block 'csslink'}}
    <link rel="stylesheet" href="./index.css">
{{/block}}

<!--充填预留位 导入js文件-->
{{block 'jsscript'}}
    <script src="./index.js"></script>
{{/block}}

<!--充填预留位 充填内容模版-->
{{ block 'content'}}
    <div class="content">
        <p>Hello</p>
    </div>
{{/block}}
```

- 模版引擎插入数据：

```js
const template = require('art-template')
const path = require('path')

const views = path.join(__dirname, 'views', 'index.art')

const html = template(views, {})
```







## 模版的相关配置

### 导入模版变量

若想在模版中使用方法函数（自定义或第三方模块的API）时

**不能直接将方法导入模版，而是作为模版的变量导入**

**template.defaults.imports.自定义函数名 = 第三方模块**

```js
const template = require('art-template')

// 导入第三方模块
const dateFormat = require('dateformat')
// 作为模版的变量导入
template.defaults.imports.dateFormat = dateFormat
```

---

比如：

处理时间的第三方模块的API

1. 安装

```bash
npm i dateformat
```

2. 导入+配置

```js
const template = require('art-template')
const path = require('path')

const dateFormat = require('dateformat')

template.defaults.imports.dateFormat = dateFormat


const views = path.join(__dirname, 'views', 'index.art')

const html = template(views, {
    time: new Date()
})
```

3. 模版中使用

```html
{{ extend '../common/bone.art'}}

{{block 'csslink'}}
    <link rel="stylesheet" href="./index.css">
{{/block}}

{{block 'jsscript'}}
    <script src="./index.js"></script>
{{/block}}



{{ block 'content'}}
    <div class="content">
        <!--使用作为变量传入的第三方模块的API，处理数据-->
        {{ dateFormat(time, 'yyyy-mm-dd') }}
        
    </div>
{{/block}}
```





### 模版引擎目录

通过模版引擎给多个模版（HTML页面）插入数据时，

若逐个定义要插入数据的模版文件的地址，会导致代码冗余，如下：

```js
const template = require('art-template')
const path = require('path')

const views1 = path.join(__dirname, 'views', '01.art')
const views2 = path.join(__dirname, 'views', '02.art')
const views3 = path.join(__dirname, 'views', '03.art')
const views4 = path.join(__dirname, 'views', '04.art')

template(views1, {})
template(views2, {})
template(views3, {})
template(views4, {})
```

可以通过设置**模版根目录**

给模版插入数据时，不用再写完整的路径，只需写模版文件名

模版引擎会自动从设定的根目录中找取指定模版文件

```js
const template = require('art-template')
const path = require('path')

// 配置模版根目录
template.defaults.root =  path.join(__dirname, 'views')

template('01.art', {})
template('02.art', {})
template('03.art', {})
template('04.art', {})
```





### 配置模版后缀

配置了模版后缀之后，

在将数据插入模版时，就不需要再写文件后缀了

```js
const template = require('art-template')
const path = require('path')

template.defauls.root =  path.join(__dirname, 'views')

// 配置模版默认后缀
template.defaults.extname = '.art'

template('01', {})
template('02', {})
template('03', {})
template('04', {})
```

