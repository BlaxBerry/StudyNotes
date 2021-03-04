# Vue

![](https://iwatani.tv/wp-content/uploads/2019/12/vuejs-keyvisual-1024x538.png)

## 简介

Vue可以**不通过获取就能操作DOM元素**实现页面效果



## 导入

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

```html
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```



## 使用

1. 首先需要**实例化一个Vue对象**，

2. 然后通过**el属性**，给指定元素**挂载上Vue**

3. 然后通过**data属性**和**methods属性**给实例化的Vue对象**设置各种属性和方法**

4. 然后通过Vue指令，把设定的属性和方法设定绑定给**el挂载的元素和其内部子元素**

```html
<body>
  <div id="app">
  <h1>{{message}}</h1>
  <h2 @add='add'>num: {{num}}</h2>
  <h3 @click='sayHello'>name: {{info.name}}</h3>
  <h3>age: {{info.age}}</h3>
  <ul>
    <li><span>{{lesson[0]}}</span> <span>{{score[0]}}</span></li>
    <li><span>{{lesson[1]}}</span> <span>{{score[1]}}</span></li>
    <li><span>{{lesson[2]}}</span> <span>{{score[2]}}</span></li>
  </ul>
</div>
</body>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<script>
  var app = new Vue({
  el: '#app',
  data: {
    message: 'Student Message'
    num: 2060,
    info: {
      name: 'Andy',
     	age: 28
    }
    lesson: ['English', 'Math', 'Spanish']
    score: [80, 70, 90]
  }，
  methods: {
    add: function(){
      this.id ++
    }
    sayHello: function(){
      console.log(`hello ${this.name}`)
    }
  }
  
})
</script>
```



### 实例化Vue对象

```js
var app = new vue({})
```

然后设置好对象的相关属性



### el属性

设置Vue的挂载元素

```js
var app = new vue({
  el: 选择器
})
```

---

可以挂载任何选择器

但是**建议Vue挂载 `ID选择器`**

```js
var app_1 = new vue({
  el: '.className'
})

var app_2 = new vue({
  el: '#idName'
})
```

---

可以挂载**除了 body 和 html 标签外**的任何**双标签**

但是**建议Vue挂载 `<div></div>`**



### data属性

设定数据对象

```js
var app = new vue({
  el: '选择器'，
  data：{
  		message: 内容
	}
})
```

可以设定任何数据类型，

比如简单类型的文本、数字，或复杂类型的数组、对象

```js
var app = new vue({
  el: '选择器'，
  data：{
  		num: 100,
  		message: 'text content',
  		tags: '<a href="https://github.com/BlaxBerry">my GitHub</a>',
  		arr: [1, 2, 3],
      obj: {
          name: 'andy',
          age: 28
      } 
	}
})
```

然后通过以下三种方法，把定义的data中的数据设置给元素内容

​		 `v-text = data对象中的属性名` 

​		 `v-html = data对象中的属性名`

​		`{{data对象中的属性}}`



### methods属性

定义方法

```js
var app = new vue({
  el: '选择器'，
  methods: {
  	方法名: function(){},
  	方法名: function(){},
    方法名: function(){}
	}
})
```

如下：

```js
var app = new vue({
  el: '选择器'，
  methods: {
  	click: function(){
   		alert('单击了')
		},
  	mouse: function(){
   		alert('鼠标经过了')
		},
    dblclick: function(){
      alert('双击了')
    }
	}
})
```

然后通过给元素绑定事件，给元素绑定定义的方法

​		`v-on:事件类型=“methods中定义的方法名”`

​		`@事件类型 = ”methods中定义的方法名“`







## 设置元素内容

### v-text

```js
v-text="message"
```

```html
<div id="app" v-text="message"></div>
```



### v-html

```js
v-html="message"
```

```html
<div id="app" v-html="message"></div>
```



### {{message}}

```html
<div id ="app">{{message}}</div>
```



## 绑定事件

### v-on

```js
v-on:click="sayHello"
```

```html
<div id ="app" v-on:click="sayHello"></div>
```



### @

```js
@click="sayHello"
```

```html
<div id ="app" @click="sayHello"></div>
```





## 隐藏 和 显示元素

比如遮罩层、广告。。。。



### v-show

通过 true / false 控制绑定**元素样式的显示和隐藏**

相当于 **`display：block` **和 **`display：none`**

只是元素的样式的隐藏和显示，元素还存在于html页面中。

```html
<div id ="app">
  	<img src='./01.jpg' v-show="false">
    <img src='./02.jpg' v-show="flag">
    <img src='./03.jpg' v-show="flag>= 18">
</div>
```

---

- 通过给固定的 **true / false** 控制元素隐藏显示

```html
<div id ="app">
  <img src='./01.jpg' v-show="true">
</div>
```

---

- 通过赋值了**true / false** 的变量来控制元素

```html
<div id ="app">
  <img src='./01.jpg' v-show="flag">
</div>
```

```js
var app = new vue({
  el: '选择器'，
 	data: {
  	//falg: true,
  	falg: false
	}
})
```

---

- 通过**指定条件**，俩控制元素显示隐藏

如下，**age >=18** 才会显现

```html
<div id ="app">
  <img src='./01.jpg' v-show="age>=18">
</div>
```

```js
var app = new vue({
  el: '选择器'，
 	data: {
    //age: 20, 满足条件显示
		age: 15 //不显示
	}
})
```

---

- 通过事件的**布尔值取反**，实现**开关效果**

```html
<div id ="app">
  <img src='./01.jpg' v-show="flag">
  <button @click=“change”>切换开关</button>
</div>
```

```js
var app = new vue({
  el: '选择器',
 	data: {
		flag = true
	},
  methods: {
     change: function(){
       this.flag = !this.flag;
     }
  }               
})
```

---



### v-if

通过 true / false 控制绑定**DOM元素在html结构中的显示和隐藏**

**操作的是DOM结构**，相当于jQuery中的`remove()`

是控制元素在html结构中的存在和移除，

```html
<div id ="app">
  	<img src='./01.jpg' v-if="false">
    <img src='./02.jpg' v-if="flag">
    <img src='./03.jpg' v-if="flag>= 18">
</div>
```

频繁切换的话建议使用 v-show，切换的消耗更少







## 操作元素属性

比如 **src 、 title、 class类名**等的属性



### v-bind

```js
v-bind: 属性名=表达式

//或省略写法
属性名=表达式
```

```html
 <div id="app">
        <a v-bind:href='href'></a>
        <img v-bind:src="src">
 
        <div v-bind:class="isActive?'active':''"></div>
   		  <div v-bind:class="{active: isActive}"></div>
    </div>

    <script>
        var app = new Vue({
            el: '#app',
            data: {
                href: "https://github.com/BlaxBerry",
                src: 'https://github.com/BlaxBerry/Demo_CSS/blob/master/slider_pics/images/02.jpeg?raw=true',
                isActive: false,
                active: 'active'
            }
        })
```



### 三元表达式判断class属性

**三元表达式**，判断是否添加class类名属性

如下：根据`isActive`的值，判读是否给<div\>添加`active类`属性

```html
 <div v-bind:class='isActive?"active":""'></div>
```

---

利用三元表达式判断 + 点击事件

实现**class类名的切换**

```html
<style>
		.active {
            border: 10px solid black;
        }
</style>
</head>

<body>
    <div id="app">
        <img src="./images/02.jpeg" v-bind:class="isActive?'active':''"  @click='toggle'>
    </div>

<script>
        var app = new Vue({
            el: '#app',
            data: {
                isActive: true,
                active: 'active'
            },
            methods: {
                toggle: function() {
                    this.isActive = !this.isActive
                }
            }
        })
</script>
```



### 对象形式判断class属性

**对象**的方式，判断是否添加class类名属性。**推荐**

如下：<div\>是否添加`active类`属性，是取决于`isActive`的值是true / falses

```html
 <div v-bind:class='{active: isActive}'></div>
```

---

利用对象的方式判断 + 点击事件

实现**class类名的切换**

```html
<style>
		.active {
            border: 10px solid black;
        }
</style>
</head>

<body>
    <div id="app">
        <img src="./images/02.jpeg" v-bind:class="{active: isActive}"  @click='toggle'>
    </div>

<script>
        var app = new Vue({
            el: '#app',
            data: {
                isActive: true,
                active: 'active'
            },
            methods: {
                toggle: function() {
                    this.isActive = !this.isActive
                }
            }
        })
</script>
```

---























## this

vue组件或实例中，不管是生命周期函数还是自定义的方法中，this均指向当前vue实例





