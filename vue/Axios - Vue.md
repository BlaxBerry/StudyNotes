# Axios

![](https://res.cloudinary.com/practicaldev/image/fetch/s--BMWDpsef--/c_imagga_scale,f_auto,fl_progressive,h_900,q_auto,w_1600/https://dev-to-uploads.s3.amazonaws.com/i/s9285z7xcffirhu4bp0j.png)

比起Ajax更方便



## 导入包

先导入再使用

```html
//axios 
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

//vue
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

导入后会在页面上声明一个全局axios对象

然后就可以使用`axios.方法名`

如下，使用get和post方法



## get请求  

### then()方法

```js
axios.get(URL?key=value&key=value).then(function(res){},function(err){})
```

第一个回调函数在响应完成时触发，返回服务器响应内容

第二个回调函数在请求失败时触发，返回错误信息



## post请求

### then()方法

```js
axios.post(URL,{key:value,key:value}).then(function(res){},function(err){})
```

第一个回调函数在响应完成时触发，返回服务器响应内容

第二个回调函数在请求失败时触发，返回错误信息



如下：

分别给两个按钮绑定点击事件，

点击后分别获得 get请求获得的响应 和 post请求传参后返回的结果：

```html
<input type="button" value="get" class="get">
<input type="button" value="post" class="post">

<script>
document.querySelector('.get').onclick = function() {

       axios.get('https://XXX/XXX/XX?xx=xx&xx=xx').then(function(response) {
                console.log(response);
         
            }, function(err) {
                console.log(err);
            });
}

document.querySelector('.post').onclick = function() {

            axios.post('https://XXX/XXX/XX',{XXX:"XXX"}).then(function(response) {
                console.log(response);
              
            },function(err) {
                console.log(err);
            })
}
</script>
```





## Axios +  Vue

### get请求 获取数据

如下：

实例Vue对象，定义事件并绑定给给元素，

点击后在事件内通过`axios的get方法`获取数据

通过`the方法`里第一个回调函数的参数res获取响应成功时**返回的一个对象**

要获得的数据存放在该返回对象的**data属性**中

```html
 <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
 <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
</head>

<body>
    <div id="app">
        <button @click="click">get data</button>
    </div>

  <script>
        var app = new Vue({
            el: '#app',
            methods: {
                click: function() {
                 	axios.get('./source/music_list.json').then(function(res) {
                        console.log(res);				//<<<<===========================
                    		console.log(res.data);	//<<<<===========================
                    }, function(err) {
                        console.log(err);
                    })
                }
            }
        })
  </script>
```

然后，



## axios中的this指向

**axios对象的方法中的 this不再指向Vue实例**

```js
 var app = new Vue({
            el: '#app',
            data: {
                message: 111111111111
            },
            methods: {
                click: function() {
                    console.log(this);       // Vue实例对象
                    console.log(this.message);  // 1111111111
                  
                    axios.get('./source/music_list.json').then(function(res) {
                        console.log(res.data);  // 请求的数据
                        console.log(this);      // window对象
                        console.log(this.message);  // undefined

                    }, function(err) {
                        console.log(err);
                    });
                }
            }
        })
```



### 变量赋值this指向Vue

解决axios对象方法中不能通过this.属性 获取和修改Vue的属性值的方法，

时通过在axios对象方法外声明一个变量，

储存当前Vue对象中的this，然后在axios对象中使用该变量

```js
var that = this
```

```js
      var app = new Vue({
            el: '#app',
            data: {
                message: 11
            },
            methods: {
                click: function() {

                    var that = this    //<<<==================

                    axios.get('./source/music_list.json').then(function(res) {

                        console.log(that);        // Vue对象
                        console.log(that.message); // 11
                      	console.log(this);    // axios对象
                      
                    }, function(err) {
                        console.log(err);
                    });
                }
            }
        })
```

如下



### 数据渲染页面

如下，通过 that ，实现了axios获得的数值赋值Vue的属性，并渲染到页面

```html
 <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
 <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
</head>

<div id="app">
        <button @click="click">get data</button>
        <p>{{arr}}</p>

</div>

    <script>
        var app = new Vue({
            el: '#app',
            data: {
                arr: []
            },
            methods: {
                click: function() {

                    var that = this

                    axios.get('./source/music_list.json').then(function(res) {
                      
                        that.arr = res.data; //<<=====================

                    }, function(err) {
                        console.log(err);
                    });
                }
            }
        })
```

再比如，

`v-for`动态生成元素

```html
    <div id="app">
        <button @click="click">get data</button>
        <p>{{arr}}</p>
        <ul>
            <li v-for="(item,index) in arr">{{arr[index]}}</li>
        </ul>
    </div>
    <script>
        var app = new Vue({
            el: '#app',
            data: {
                arr: []
            },
            methods: {
                click: function() {

                    var that = this

                    axios.get('./source/music_list.json').then(function(res) {
                        that.arr = res.data;    //<<=====================

                    }, function(err) {
                        console.log(err);
                    });
                }
            }
        })
			</script>
```





```js
var app = new Vue({
            el: '#app',
            data: {
                message: ""
            },
            methods: {
                fn: function(url) {
                    axios.get(url).then(function(res) {
                      
                    }, function(err) {
                      
                    })
                }
            }
        })
```



如下：点击按钮，获得本地的json文件的数据

