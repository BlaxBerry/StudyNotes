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
var app = new Vue({})
```

然后设置好对象的相关属性



### el属性

设置Vue的挂载元素

```js
var app = new Vue({
  el: 选择器
})
```

---

可以挂载任何选择器

但是**建议Vue挂载 `ID选择器`**

```js
var app_1 = new Vue({
  el: '.className'
})

var app_2 = new Vue({
  el: '#idName'
})
```

---

可以挂载**除了 body 和 html 标签外**的任何**双标签**

但是**建议Vue挂载 `<div></div>`**



### data属性

设定数据对象

```js
var app = new Vue({
  el: '选择器'，
  data：{
  		message: 内容
	}
})
```

可以设定任何数据类型，

比如简单类型的文本、数字，或复杂类型的数组、对象

```js
var app = new Vue({
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
var app = new Vue({
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

设置DOM元素的内容，仅识别文本内容

相当于`innerText()`

```js
v-text="message"
```

```html
<div id="app" v-text="message"></div>

<script>
	var app = new Vue({
  el: '选择器'，
  data：{
  		message: 'hello'
	}
})
</script>
```



### {{message}}

v-text 的缩写，仅识别文本内容不能解析标签

可直接写在标签内容

```html
<div id ="app">{{message}}</div>
```

```html
<div id="app">{{message}} {{num}}</div>

<script>
	var app = new Vue({
  el: '选择器'，
  data：{
  		message: 'hello',
    	num: 100
	}
})
</script>
```

可写表达式

```html
<div id="app">
 	 <div>{{message + 'Andy'}}</div>
   <div>{{num +100}}</div>
</div>

<script>
	var app = new Vue({
  el: '选择器'，
  data：{
  		message: 'hello',
    	num: 100
	}
})
</script>
```



### v-html

设置DOM元素的内容，可解析标签

相当于`innerHTML()`

若是纯文本，则效果等同于` v-text=“”`

```js
v-html="message"
```

```html
<div id="app" v-html="link"></div>

<script>
	var app = new Vue({
  el: '选择器'，
  data：{
  		link: '<a href="#">my GitHub</a>'
	}
})
</script>
```





## JS表达式

Vue支持JavaScript的表达式

比如

{{message + "hello"}}

{{num+100}}

{{num>0? message_1 : message_2}}

{{message.split('').reverse().join('')}}





## 事件绑定

### v-on

```js
	v-on:事件类型=“方法名”
//比如
	v-on:click="sayHello"
```

```html
<div id ="app">
  	<div v-on:click="say_1"></div>
    <div v-on:mouseenter="say_2"></div>
    <div v-on:dblclick="say_3"></div>
</div>

<script>
var app = new Vue({
  el: '选择器'，
  methods: {
  	 say_1: function(){
   		alert('111')
		},
  	 say_2: function(){
   		alert('222')
		},
    say_3: function(){
      alert('333')
    }
	}
})</script>
```



### @

v-on：写法的省略 

```js
		@事件类型=“方法名”
//比如
		@click="sayHello"
```

```html
<div id ="app" @click="sayHello"></div>

<script>
var app = new vue({
  el: '选择器'，
  methods: {
    sayHello: function(){
      alert('333')
    }
	}
})</script>
```





### 事件参数

就是调用函数时的传参，和定义函数时的形参

```js
// 标签中
v-on:事件类型=“方法名(参数，参数，参数)”

//Vue实例对象中
var app = new vue({
  el: '选择器'，
  methods: {
  	方法名: function(参数，参数，参数){},
  	方法名: function(参数，参数，参数){},
    方法名: function(参数，参数，参数){}
	}
})
```

如下：传入了字符串作为事件方法的参数

```html
<div id ="app">
  	<div v-on:click="sayHello('andy')"></div>
</div>

<script>
var app = new Vue({
  el: '#app'，
  methods: {
  	  sayHello: function(a){
   				alert('hello'+ a)
		  }
	}
})
</script>
```



### 事件修饰符

对事件进行限制



即，只有在指定的按键才会触发事件并调用方法

```js
v-事件类型.修饰符=“方法名”
//比如
v-on:keyup.enter=“sayHello('Vue')”
@keyup.enter=“sayHello('Vue')”
```

如下：设定给input绑定的事件的限定符是分别是

enter、space、control、a键

```html
<div id="app">
    <input type="text" @keyup.enter="sayHello('Vue')">
  
    <input type="text" @keydown.space="sayHello('JS')">
  
    <input type="text" @keydown.control="sayHello('JS')">
  
    <input type="text" @keydown.a="sayHello('JS')">
</div>

<script>
        var app = new Vue({
            el: "#app",
            methods: {
                sayHello: function(a) {
                    alert(`hello  ${a}`)
                }
            }
        })
</script>
```











## 隐藏 和 显示元素

有仅仅是CSS样式display属性的隐藏，和彻底从DOM树移除

比如遮罩层、广告。。。。

### v-show

通过表达式值的 true / false 控制绑定**元素样式的显示和隐藏**

相当于 **`display：block` **和 **`display：none`**

只是元素的样式的隐藏和显示，元素还存在于html页面中。

```js
v-show='true'
v-show='false'
v-show='数据'
v-show='表达式'
```

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

```js
v-if='true'
v-if='false'
v-if='数据'
v-if='表达式'
```

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
:属性名=表达式
```

```html
 <div id="app">
    <a v-bind:href='adtaHref'></a>
    <a :href='adtaHref'></a>
</div>
```

如下，分别设定了href、src、class属性：

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



## 样式类名绑定

### class属性绑定 — 三元表达式

利用三元表达式判断是否添加class类名属性

**若数据或表达式的值为true，则设定class为指定类名，**

**若数据或表达式的值为false，则设定class为空**

```js
v-bind="数据？类名：‘’"
```

如下：

根据`isActive`的值，判读是否给<div\>添加`active类`

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



### class属性绑定 — 对象形式

**对象键值对**的方式，判断是否添加class类名属性。**推荐**

如果数据或表达式的值为true ，则添加类名

如果数据或表达式的值为false，则不添加

```js
v-bind:class='{类名：表达式}'
```

如下：

元素是否添加`active类`属性，

是取决于`isActive`的值是true / false

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



### 键值对动态绑定多个class类名

用多个键值对的方法，判断数据或表达式的值的true / false

```js
:class='{类名:数据/表达式, 类名:数据/表达式}'
```

如下，因为数据值都是true，给p标签设置两个类名

```html
<style>
        .red {
            color: red
        }
        
        .big {
            font-size: 90px;
        }
</style>

<div id="app">
   <p :class='{red:isRed, big:isBig}'>asd</p>
</div>

<script>
        const app = new Vue({
            el: '#app',
            data: {
                isRed: true,
                isBig: true
            }
        })
</script>
```



### 数组形式静态绑定多个class类名

```js
:class='["类名","类名"]'
```

如下：

```html
<style>
        .red {
            color: red
        }
        
        .big {
            font-size: 90px;
        }
</style>

<div id="app">
   <p :class='["red", "big"]'>asd</p>
</div>

<script>
        const app = new Vue({
            el: '#app',
            data: {
                isRed: true,
                isBig: true
            }
        })
</script>
```



### 数组形式静态绑定多个class类名

通过**三元表达式**判断是否绑定该类名

```js
:class='[数据？"类名":"",数据？"类名":""]'
```

如下：

```html
<style>
        .red {
            color: red
        }
        
        .big {
            font-size: 90px;
        }
</style>

<div id="app">
   <p :class='["red", "big"]'>asd</p>
</div>

<script>
        const app = new Vue({
            el: '#app',
            data: {
                isRed: true,
                isBig: false
            }
        })
</script>
```





## v-for

遍历data中设定的一个数组，  **根据数组元素的个数 ** 

响应式生成动态**生成DOM元素**，多用于生成 **<li\>**标签

### 遍历数组

```js
v-for="(item,index) in 数据"
```

---

- 循环生成数组元素个数的DOM元素

如下，循环生成3个<li\>，包含原来的页面中一共3个

```html
<div id='app'>
  <ul>
    <li v-for="item in arr"></li>
  </ul>
</div>

<script>
	var app = new Vue({
  		el: '#app',
  		data: {
    			arr: [1,2,3]
  		}
	})
</script>
```

---

- 元素的内容或内部子元素也会一起循环生成

如下，<li\>标签中的内容也会随着循环生成

```html
<div id='app'>
  <ul>
		<li v-for="item in arr">
   			<span>hello</span><a href="#"></a>
		</li>
  </ul>
</div>

<script>
	var app = new Vue({
  		el: '#app',
  		data: {
    			arr: [1,2,3]
  		}
	})
</script>
```



### item

可把数组元素写入标签内部

item表示数组的每一个元素

可在该标签内使用数组item元素

```html
<li v-for="item in arr">{{item}}</li>
```

如下，循环生成3个<li\>，

且3个<li\>内容是分别是顺序的数组元素1，2，3

```html
<div id='app'>
  <ul>
    <li v-for="item in arr">{{item}}</li>
  </ul>
</div>

<script>
		var app = new Vue({
  				el: '#app',
  				data: {
   						 arr: [1,2,3]
 					 }
		})
</script>
```



### index

可把数组的序号写入标签内部

index表示数组的每一个序号

在标签中写入如下，就可在该标签内使用数组index序号

```js
v-for="(item,index) in arr"
```

如下，生成3 个 < li \>，

< li \>的子元素的内容分别是数组的序号和元素

```html
<div id='app'>
  <ul>
    <li v-for="(item,index) in arr">
        <span>{{index +1}}</span>
      	<span>{{item}}</span>
    </li>
  </ul>
</div>

<script>
		var app = new Vue({
  				el: '#app',
  				data: {
   						 arr: [1,2,3]
 					 }
		})
</script>
```



### 数组元素是对象

数组元素为复杂数据类型时，比如对象，如下：

```js
var app = new Vue({
            el: "#app",
            data: {
                arr: [{
                    name: 'andy'
                }, {
                    name: 'red'
                }, {
                    name: 'james'
                }]
            }
```

 因为数组的元素是个对象，所以此时`{{item}}`获得的也是对象

```html
   <li v-for="item in arr">{{item}}</li>
```

---

若要获得对象的属性，要. **`{{item.属性}}`**



```html
  <li v-for="item in arr">{{item.name}}</li>
```

如下：循环生成3 个 < li \>， 

并把作为数组元素的对象的属性值也写入

```html
<ul>
	<li v-for="(item,index) in arr">
      <span>{{index}}</span>
   		<span>{{item.name}}</span>
	</li>
</ul>

<script>
			var app = new Vue({
            el: "#app",
            data: {
                arr: [
                  {name: 'andy'}, 
                  {name: 'red'}, 
                  {name: 'james'}
                ]
       }
</script>
```



### 响应式增减DOM元素

v-for是响应式生成DOM元素。

**数组长度变化时，要生成的元素个数也随之变化**

如下，

点击按钮会增删数组元素，生成的 <li\> 标签数量也随之改变

若一直触发remove方法，数组元素会一直减少直到为0，

则此时页面中的 <li\> 也为 0个

```html
<div id="app">
        <ul>
            <li v-for="(item,index) in arr">
              	<span>{{item.name}}</span>
          	</li>
        </ul>
        <button @click="add">add</button>
        <button @click="remove">remove</button>
</div>

    <script>
        var app = new Vue({
            el: '#app',
            data: {
                arr: [{
                    name: 'hambuger'
                }, {
                    name: 'taco'
                }]
            },
            methods: {
                add: function() {
                    this.arr.push({
                        name: 'beer'
                    })
                },
                remove: function() {
                    this.arr.pop()
                }
            }
        })
    </script>
```









## v-model

获取和修改普通盒子的内容是通过 `t-text `或 插入表达式 `{{	}}`

获取和设置**表单元**素的值是通过 `v-model`

相当于原生JS的`value`

```js
v-model=数据
```

```html
<input v-model="message"></input>
```



### 同步修改数据

v-model 是**双向数据绑定**

即，数据被修改了<input\> 的值也被同时修改

<input\> 的值被修改了数据也同时被修改

如下：

<input\>的值和<h2\>的内容同步

```html
<div id="app">
     <input type="text" v-model="message">

      <h2>{{message}}</h2>
</div>

<script>
     var app = new Vue({
         el: '#app',
         data: {
             message: 'hell0'
         }
      })
</script>
```



### 获取表单值

如下，

给表单绑定事件，通过`keyup事件`获取表单的输入值

```html
<div id="app">
        <input type="text" v-model="message" @keyup='getValue'>
</div>

<script>
        var app = new Vue({
            el: '#app',
            data: {
                message: ''
            },
            methods: {
                getValue: function() {
                    console.log(this.message);
                }
            }
        })
</script>
```



### 修改表单值

如下，

给按钮绑定事件，点击按钮后修改数据，闭同时响应到表单

```html
<div id="app">
        <input type="text" v-model="message" @keyup='getValue'>
        <button @click="change">change content</button>
</div>

<script>
        var app = new Vue({
            el: '#app',
            data: {
                message: ''
            },
            methods: {
                getValue: function() {
                    console.log(this.message);
                },
                change: function() {
                    this.message = 'hello'
                }
            }
        })
</script>
```





条件

v-if

v-else