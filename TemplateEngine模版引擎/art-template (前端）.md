# art-template在客户端的使用



## 渲染方式比较

### 传统方式渲染UI结构

传统的UI结构渲染需要循**环拼接字符串**

并且需要单独获取各个DOM节点

会导致**结构混乱**，不利于后期检查和维护

```html
  <body>
    <div class="user">
      <div>Name: <span id="name"></span></div>
      <div>Age: <span id="age"></span></div>
      <div>Time: <span id="time"></span></div>
      <div>Skills:
        <ul id="skills"></ul>
      </div>
    </div>
  </body>

  <script>
    var data = {
      name: "andy",
      age: 28,
      time: new Date(),
      skills: ["Vue", "react", "jQuery"],
    };

    $(function () {
     
      $("#name").html(data.name);
      $("#age").html(data.age);
      $("#time").html(data.time.toTimeString());

      var list = [];
      $.each(data.skills, function (index, item) {
        list.push(
          "<li><span>" + (index + 1) + "</span>-<span>" + item + "</span></li>"
        );
      });
      $("#skills").html(list.join(''));
    });
  </script>
```

### 模版引擎渲染UI结构

指定 **模版结构** 和 **数据**，模版引擎会自动生成HTML页面

**不需要进行字符串拼接**，代码结构清晰

```html
<body>
    <div class="user"></div>
</body>

<!-- art-template -->
  <script type="text/html" id="art">
    <h1>{{name}} {{age}}</h1>
    <h2>name: {{name}}</h2>
    <h2>age: {{age + 2}}</h2>
  </script>

<!-- jQuery -->
<script>  
  $(function () {
    
    var artTemp = template("art", {
      name: "andy",
      age: 28,
    });
    $('.user').html(artTemp)
    
  });
</script>
```









## 使用步骤

**1.  script标签引入**

官方下载后引入HTML文件

```html
 <script src="./js/art-template.js"></script>
```

**2.  准备art-template模版**

模版必须单独定义到<script\>标签中，且需要添加一个**id**

```html
<script type="text/html" id="art">
	
</script>
```

因为模版的结构式需要被解析为HTML结构

所以<script\>标签的type类型要从默认的text/javascript修改为text/html

---

**3.  设定模版和数据**

导入art-template后，window全局多出一个**template函数**

调用该函数设定哪一个模版与哪一个数据进行拼接

```js
template('模版ID'，{数据})
```

```html
<!--模版-->
<script type="text/html" id='art'>

</script>

<!--绑定模版与数据-->
<script>
 	template('art', {
      name: 'Andy',
    	age: 28
  })
</script>
```

---

**4.  在模版中拼接数据**

通过 **{{ }}** 将数据写入模版

```html
<script type="text/html" id="art">

		<div> {{name}} {{age}} </div>
    <div> name: {{name}} </div>
    <div class="active"> age: {{age + 2}} </div>
    
</script>
```

---

**5.  渲染到HTML页面**

写入数据的模版会被解析为HTML结构

最后需要通过DOM操作，将写入数据的模版渲染到页面

template函数返回的是解析后的HTML结构，所以需要将其放入页面

```html

<script>
	// 获取template函数返回的HTML
  var atrTemp = template('art', {
      name: 'Andy',
    	age: 28
  });
  
  // 渲染到页面
  document.getElementById('targetBOX').innerHTML = atrTemp；
  
</script>
```







## 使用实例

```html
<head>
  <script src="./js/art-template.js"></script>
</head>
<body>
  
  <div id="targetBOX"></div>
  
  
  <script type="text/html" id="art">
  		<div>  </div>
  		<div></div>
  		<div class="active"></div>
  </script>
  <script>
  	var artTemp = template('art', {
        name: 'andy',
        age: 28
      });
    document.getElementById('targetBOX').innerHTML = artTemp;
  </script>
</body>
```









## 标准语法 - 输出

通过 **{{ }}** 输出的方式，在art-template中成为标准语法

- **变量**的输出
- **对象属性**的输出
- **三元表达式**的输出
- **逻辑或**的输出
- **加减乘除表达式**的输出

```js
{{value}}
{{data.key}}
{{data['key']}}
{{a ? b : c}}
{{a || b}}
{{a + b}}
```



### 原文输出

若要输出的数据中包含了字符串的**HTML结构**，

直接将数据写入**{{}}**  并不会被解析，而是输出字符串形式的HTML结构

```js
{{value}}
```

所以需要通过原文输出，将字符串形式的HTML结构解析为HTML结构

```js
{{@value}}
```

如下：

```html
<body>
  <div class="user"></div>
</body>

<script type="text/html" id='art'>
	{{ui}}
	{{@ui}}
</script>

<script>
  var data = {
    ui: '<h1>HELLO</h1>'
  }
	$(function(){
    var art = template('art', data)
    $('.user').html(art)
    
  })
</script>
```



### 条件输出

**{{ }}** 中可以使用 **if else if** 语法，实现按需输出

```html
{{if value}}内容{{/if}}

{{if value1}}内容1{{else if value2}}内容2{{/if}}
```

```html
  <script type="text/html" id="art">
    <div>{{if flag}} flag是true {{/if}}</div>
    <div>{{if num===1}}num是1{{/if]}}</div>
    <div>{{if num!==0}}num不是0{{/if]}}</div>
    <div>
    	{{if value1}}
    		{{value1}}
    	{{else value2}}
    		{{value2}}
    	{{/if}}
    </div>
  </script>
```



### 循环输出

通过each遍历数组

**$index** 获取被循环的当前数组的每一项的序号

**$value** 获取被循环的当前数组的每一项

```html
{{each 数组}}
	{{$index}}
	{{$value}}
{{/each}}
```

如下：遍历数组

```html
<body>
  <div class="user">
  </div>
</body>

<script type="text/html" id="art">
    <ul>
      {{each skills}}
      <li>{{$index}}---{{$value}}</li>
      {{/each}}
    </ul>
  </script>

<script>
  var data = {
      skills: ["Vue", "react", "jQuery"],
  };
  
	$(function(){
    var art = template('art', data)
    $('.user').html(art)
    
  })
</script>
```





## 标准语法 - 过滤器

将值作为**参数**提交给过**滤器函**数，函数返回值就是值被处理后的结果

需要被渲染到页面的就是这个返回的结果

### 使用

```js
{{数据值 | 过滤器函数名}}
```

### 定义

引入了art- template后全局会多出一个template函数

函数的参数是**管道符 ｜** 前的数据

```js
template.defaults.imports.过滤器函数名 = function(数据){
  return XXXXX
}
```



如下：

```html
<body>
  <div class="user">
  </div>
</body>

<script type="text/html" id="art">
  <div>
    <span>注册时间：</span>
    <div>{{time}}</div>
    <div>{{time | handleTime}}</div>
  </div>
</script>

<script>
  var data = {
    time: new Date(),
  };
  
  template.defaults.imports.handleTime = function (value) {
      // console.log(value);
      var y = value.getFullYear()
      var m = value.getMonth()+ 1
      var d = value.getDate()

      return y + '/' + m + '/' + d
    };

  $(function () {

    var art = template("art", data);
    $(".user").html(art);
    
  }
</script>   
```