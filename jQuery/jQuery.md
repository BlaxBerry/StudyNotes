# jQuery

![](http://i2.wp.com/tc-networks.com/wp-content/uploads/2016/01/jQueryLogo_%E3%82%A2%E3%82%A4%E3%82%AD%E3%83%A3%E3%83%83%E3%83%81.jpg?resize=690%2C300)

## 简介

是个提前封装好的JavaScripts**库**文件

2006年左右为了兼容浏览器诞生

目前市面上使用了jQuery的基本都是老项目

---

操作元素时比起原生JavaScript更方便，并解决了兼容问题

比如对DOM元素的操作，原生JS如下：

```html
<!--原生JS要先获取元素再操作-->

<div></div>
<script>
     let div = document.querySelector('div');
     div.style.height = '100px';
     div.style.width = '100px';
     div.style.backgroundColor = 'crimson';
</script> 
```

使用jQuery则可以减少代码量，如下：

```html
<div></div>
<script>
	$(function() {

        $('div').css({
            width: '100px',
            height: '100px',
            backgroundColor: 'crimson'
        })
	)
</script> 
```

---

**库 和 框架**

框架大而全，包含设计思想，比如Vue、react、bootstrap等

库小只有封装好的方法和属性





## jQuery版本

**1x**  兼容 IE6、7、8

**2x**  开始不再兼容  IE6、7、8

**3x**  内容增加ES6 相关内容





## jQuery的导入与使用

### 导入下载文件

可以从官网下载文件（复制粘贴）

[jQuery官网下载](https://jquery.com/download/)

> `jquery.js` 是开发版本（development）
>
> `jquery.mini.js `是生产版本（ production）代码被压缩

下载的文件放入项目的`lib文件夹`中，然后引入`html文件`中

先引入jQuery文件，再引用自己的js文件

```html
<script src="lib/jquery.min.js"></script>
<script src="js/index.js"></script>
```



### 导入引用地址的标签

或者直接引用jQuery的地址，放入页面标签内

> jQuery 3.5.1  https://code.jquery.com/jquery-3.5.1.min.js

`html文件`中插入下列标签：

```html
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
```

先引入jQuery文件，再引用自己的js文件

```html
<script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
<script src="js/index.js"></script>
```



### 准备入口函数

对jquery的使用，代码都写在入口函数中（**回调函数**）

```js
$(function() {
    代码
})

//或者
jQuery(function(){
    代码
})
```

---

入口函数的作用类似原生JS文件引入时用的load事件，

就是告诉程序，以下这些代码使用时调用了jquery文件的方法属性



### 写入<script\>标签使用

**直接在html页面文件中写入**jQuery时，如下：

```html
<head>
   <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
</head>
<body>
    <div></div>
    <script>
        $(function() {
            $('div').css({
                width: '100px',
                height: '100px',
                backgroundColor: 'crimson'
            })
        })
    </script>
</body>
```



### 写入外部文件中使用

**写入外部js文件**内使用jQuery时：

```html
<head>
   <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
   <script src="index.js"></script>
</head>
<body>
   <div></div>
</body>
```

```js
//JS文件内
window.addEventListener('load', function() {
  
    $(function() {
        $('div').css({
            width: '100px',
            height: '100px',
            backgroundColor: 'crimson'
        })
    })
})
```





## 入口函数

下面四种写法都可以：

```js
$(function() {})

jQuery(function() {})

$(document).ready(function() {})

jQuery(document).ready(function() {})
```

---

验证` $ === jQuery`，如下：

```html
 <script src="jquery.min.js"></script>
 <script>
        console.log($ == jQuery);
 </script>

//结果
true
```

---

证明jQuery的入口函数是个回调函数，

等DOM页面加载完后才会执行的，如下，

jQuery的入口函数中的2，晚于window的load事件中的3

```js
console.log(1);

$(function() {
  console.log(2);
})
window.onload = function() {
  console.log(3);
}

//1
//3
//2
```



[jQuery中文文档](https://jquery.cuishifeng.cn/)



## 获取元素

```js
		$('选择器')
```

和原生JS获取DOM时的写法一样：

```js
$(function() {

      var item1 = $('div');   //标签名
  
      var item2 = $('.box');  //类名
  
      var item3 = $('#box');  //ID

      var item4 = $('ul li'); //子元素
  
      var item5 = $('div, .box, #div'); //并集选择器
  
  		var item5 = $('li:nth-child(2)'); 

            
})
```





## DOM对象 与 jQuery对象

DOM对象就是用原生JS方法获得元素。

如下：返回的是**NodeList元素节点对象**

```js
let div = document.querySelector('div')
console.log(div);
//   <div></div>


let lis = document.querySelectorAll('li')
console.log(lis);
//   NodeList(2) [li, li]
```

---

用jQuery的`$()`返回的是个**jQuery对象**

```js
$(function() {
    var lis = $('ul li')
   console.log(lis)
})

/*
S.fn.init(4) [li, li, li, li, prevObject: S.fn.init(1)]
*/    // jQuery对象
```



### jQuery对象 —> DOM对象

比如，用序号获取的jQuery对象的子元素，获得的是个DOM对象

该子元素不是jQuery对象，**不能使用jQuery封装的方法**

```js
var lis = $('ul li')

var li_1 = lis[0];    

console.log(li_1); 
//   <li></li>     //DOM节点对象
```

---

**jQuery封装的方法只能用于jQuery对象**

```js
var lis = $('ul li')

var li_1 = lis[0];   

li_1.css('background', 'red')

//报错
// Uncaught TypeError: li_1.css is not a function
```



### DOM对象 —> jQuery对象

应该使用**把DOM对象转换为jQuery对象**。

使DOM对象可是使用jQuery封装的方法。如下：

```js
var lis = $('ul li')

var li_1 = lis[0];

$(li_1).css('background', 'red')
```





## this 和 $(this)

在原生 JS 的`全局作用域`中

`this`指向`window`

---

在jQuery的**入口函数中 **

**`this`指向`document`**

```js
$(function(){
  concole.log(this)
})

//#document
```

---

**在事件中**

`this`指向触发事件的DOM元素

---

**`$(this)`**则是把**该DOM元素转换为jQuery对象**，使其可以使用jQuery的方法

如下：

```js
var div = $('div')

div.on('click', function() {
  
  	console.log(this);
// <div></div>  DOM节点对象
  
  	console.log($(this));
// S.fn.init [div]  jQuery对象
  
})
```





## 操作行内样式

在jQuery中用**`css() `方法访问、改变元素样式**

### 获取属性值

```js
jQuery对象.css('属性名')
```

如下：

```js
var height = $('div').css('height');
var bgColor = $('div').css('backgroundColor');
console.log(height);
console.log(bgColor);

//100px
//rgba(0,0,0,0)
```



### 修改属性值

里面放 **键值对 **的样式属性和属性值

```js
		jQuery对象.css({'属性名':'值'})

//或
		jQuery对象.css('属性名'，'值')
```

如下：

```js
//一个
		$('div').css('backgroundColor':'red')
//多个
		$('div').css({
        'height': '100px',
        'width': '100px',
        'backgroundColor': 'red'
     }))
```

---

修改的属性值也可以为变量，如下，使用定时器自动增加长度：

```js
$(function() {
  
     var num = 100;
     setInterval(function() {
     		$('div').css({
           		'height': num + 'px'
     		});
     num++;
     }, 10)
})
```



### 添加类名  删除类名

```js
jQuery对象.addClass('类名 类名 类名 类名')

jQuery对象.removeClass('类名 类名 类名 类名')

jQuery对象.toggleClass('类名 类名 类名 类名')

//类名不加点
//该方法被`classList`借鉴
```

如下：排他思想+增删类名

```html
<style>
        .current::after {
            background-color: orange;
        }
</style>

<body>
  <ul>
        <li>item</li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</body>

<script>
    $(function() {

        var lis = $('ul li')

        for (let i = 0; i < lis.length; i++) {

            $(lis[i]).on('click', function() {

                for (let i = 0; i < lis.length; i++) {
                    $(lis).removeClass('current')
                }

                $(this).addClass('current');
            })
        }

        $(lis[0]).addClass('current')
    })
</script>
```





## 注册事件

```js
		jQuery对象.on(事件类型,function(){}) //可注册多个

//或
		jQuery对象.事件类型(function(){}) //了解
```

---

#### click开关灯案例

如下：

```html
<button>open / close</button>
    <script>
        $(function() {
            var flag = 0;
            $('button').on('click', function() {
                if (flag == 0) {
                    $('body').css({
                        background: 'black'
                    });
                    flag = 1
                } else {
                    $('body').css({
                        background: ''
                    });
                    flag = 0
                }
            })
        })
    </script>
```

或简单的写法

```js
var flag = false;
$('button').on('click', function() {
    $('body').css('background', !flag ? 'black' : '');
    flag = !flag
})
```