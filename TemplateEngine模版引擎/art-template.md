# art- template

art- template是一个体积小，速度快的模版引擎

![](https://aui.github.io/art-template/images/chart@2x.png)





## 传统方式渲染UI结构

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





## 模版引擎渲染UI结构

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
    var data = {
      name: "andy",
      age: 28,
    };
    
    $(function () {
      
      // console.log(template("art", data));
      var art = template("art", data);

      $('.user').html(art)

    });
</script>
```









## art-template使用步骤

### 1. script标签引入

官方下载后引入

体积小仅6kb

```html
 <script src="./art-template.js"></script>
```

---

### 2. 定义数据

```html
<script>
  var data = {
      name: "andy",
      age: 28,
      time: new Date(),
      skills: ["Vue", "react", "jQuery"],
  };
</script>
```

---

### 3. 定义模版

模版必须单独定义到**<script\>标签**中

因为模版的结构式需要被解析为HTML结构

所以<script\>标签的**type**属性要从默认的text/javascript修改为text/html

且模版需要添加一个**id**

模版中通过 **{{ }}** 写入数据

```html
<script type="text/html" id="art">
    <h1>{{name}} {{age}}</h1>
    <h2>name: {{name}}</h2>
    <h2>age: {{age + 2}}</h2>
</script>
```

---

### 4. 调用template() 函数

导入art-template后,window全局会多出一个**template函数**

```js
template('模版ID'，数据)
```

返回的是模版解析为的HTML结构

```html
<script type="text/html" id='art'>
	<h1>{{name}}</h1>
</script>

<script>
  var data = {
    name: 'Andy'
  }
	$(function(){
    template('art', data)
    
    // console.log( template('art', data))
    // <h1>Andy</h1>
  })
</script>
```

---

### 5.渲染HTML结构

通过DOM操作，将template() 的返回值渲染到页面

```html
<body>
  <div class="targrtNode"></div>
</body>

<script>
  $(function(){
    $('.targrtNode').html(template('art', data))
  })
</script>
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

```js
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

```js
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