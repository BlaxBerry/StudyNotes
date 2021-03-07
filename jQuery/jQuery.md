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

```js
window.jQuery === window.$ === jQuery
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





## DOM对象 <=> jQuery对象

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







## 获取元素

[jQuery文档](https://jquery.cuishifeng.cn/)

### 选择器获取

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



### eq()

**推荐**

获取指定元素中的第N个，返回的是jQuery对象

```js
jQuery对象.eq(n)
```

**比选择器获取的方法方便**，尤其是序号不固定，使用变量指定序号的时候

```js
$('li:nth-child(n)')；

$("li").eq(1)；

var num = 1 + 2；
$("li").eq(num)
```



### slibilings()

**重要**

获取指定元素的**兄弟元素们**

```js
jQuery对象.sibilings()
```

主要运用在**排他思想**中：

```js
$(this).addClass('clicked').siblings().removeClass('clicked');
```



### children()

```js
$('').children();

$('').children().eq(0)
```





## 获取索引

### index()

获得元素在其兄弟中的索引号，从0开始计数**。

```js
$("li").on('click',function(){
  var index = $(this).index();
  concole.log(index)
})
// 返回该元素的序号
```





## 操作行内样式

[jQuery文档](https://jquery.cuishifeng.cn/)

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

[jQuery文档](https://jquery.cuishifeng.cn/)

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



### 添加 / 删除 类名

[jQuery文档](https://jquery.cuishifeng.cn/)

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

[jQuery文档](https://jquery.cuishifeng.cn/)

```js
		jQuery对象.on(事件类型,function(){}) //可注册多个

//或
		jQuery对象.事件类型(function(){}) //了解
```

---

### click 开关灯案例

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



给当前元素注册事件



### 利用事件委托

给父元素注册事件







## 动画函数

了解即可，目前C3用的多，jQuery动画不常用

[jQuery文档](https://jquery.cuishifeng.cn/)

---

---

### animate() 自定义动画

```js
jQuery对象.animate({CSS属性:'值'},[速度曲线],[回调函数])
```

如下：

```js
 $('div').animate({
            left: '+100px'，
   					height: '200px',
   					width: '200px'
        },1000,function(){
   					console.log('finished')
 }})
```

---

---

### jquery.easing.js 插件

是一款jQuery动画效果插件，增加了**速度曲线**的样式

可以实现直线匀速运功、变加速运动、缓冲等丰富的动画效果。

[菜鸟教程 jquery.easing.js  ](https://www.runoob.com/jqueryui/api-easings.html)

---

---

### stop()

停止**动画队列**。

```js
$('div').stop().fadeToggle();
$('div').stop().slideToggle();
```

---

---

### 渐变动画 

淡入淡出效果，透明度从1～0或0～1，**元素还存在不会被删除**

 **`fadeIn()`	`fadeOut()`	`fade Toggle()`**

```js
jQuery对象.fadeIn([速度曲线],[回调函数])

jQuery对象.fadeOut([速度曲线],[回调函数])

jQuery对象.fadeToggle([速度曲线],[回调函数])
```



### 渐变轮播图

![](https://github.com/BlaxBerry/jQuery_Demo/blob/master/slider_pics_fade/images/shortcut.png?raw=true)

[github jq淡入淡出轮播图](https://github.com/BlaxBerry/jQuery_Demo/tree/master/slider_pics_fade)





## 获得 / 修改元素属性

[jQuery文档](https://jquery.cuishifeng.cn/)

### attr()

获得 / 修改 自定义属性

**键值对**的形式

```js
jquery对象.attr({
  						属性名:'值'，
  						属性名:'值'
        })

jquery对象.attr()
```

```js
$('li').attr({
            index: 1,
            id: 'good'
        });

cosole.log($('li').arrt('index'))
```

就是原生JS中的 `setAttribute` 和  `getAttribute`



## 获得 / 修改 表单属性

### prop()

获得 / 修改 input开关的属性，比如checked 或 disabled

```js
input jQuery对象.prop(属性名)
input jQuery对象.value(属性,'值')
```

如下，设置 **禁用**和**选中**所有页面上的复选框：

```js
var input = $('input')

$(input).prop("disabled", true);

$(input).prop("checked", true);
```

---

如下，获取 表单的 选中属性

返回值是`true` 或 `false`

```js
var input = $('input')

console.log(input.prop('checked'));
// true  flsae
```





## 获取 / 修改元素内容

[jQuery文档](https://jquery.cuishifeng.cn/)

### text() 

获取 / 修改 元素的文本内容

就是原生JS中的 `innerText`

```js
jQuery对象.text()
jQuery对象.text('内容')
```

---

### val()

获取 / 修改 表单的内容

就是原生JS中的 `value`

```js
input jQuery对象.value()
input jQuery对象.value('内容')
```

```js
var input = $('input')
input.val('close')

console.log(input.val());
```





## 动态生成元素

[jQuery文档](https://jquery.cuishifeng.cn/)

### html()

覆盖元素内容

```js
jQuery对象.html('<tag></tag>')
```

就是原生JS中的 `innerHTML`

---

### $('<></>')

生成一个指定标签

```js
$(标签)

$('<div><span><a href="#">哦吼点我</sapn></div>')
```

就是原生JS中的 `document.createElement(''<tag></tag>'')`

如下，创建一个<li\>

````js
var content = $('<li></li>')
````

生成后需要搭配`append()`追加给父级元素





## 动态增加元素

[jQuery文档](https://jquery.cuishifeng.cn/)

### append()

```js
父级jQuery对象.append(子级元素)
```

就是原生JS中的 `appendChild()`

如下：

```js
var li = $('<li></li>');
$('ul').append(li)

$('div').append($('ul'))
```

如下，变量数组，生成数组长度的元素，并渲染到页面

```html
<ul></ul>

<script>
   let arr = [1, 2, 3, 4, 5, 6];

    arr.forEach((item, index) => {
        let li = $(`<li>${item}</li>`);
         $('ul').append(li);
    }
</script>
```



### prepend()

```js
父级jQuery对象.prepend(子级元素)
```

就是原生JS中的 `insertBefore()`

如下：

```js
var li = $('<li></li>');
$('ul').prepend(li)

$('div').prepend($('ul'))
```



## 删除元素



```js
jQuery对象.remove()
```

如下，

每次删除第一个<li\>

```html
   <button>click</button>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>7</li>
        <li>8</li>
    </ul>

<script>
        $('button').on('click', () => {
            $('ul li:first-child').remove()
        })
    </script>
```

或利用 `children()` 和 `eq()`

```html
    <button>click</button>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>7</li>
        <li>8</li>
    </ul>

<script>
        $('button').on('click', () => {
            $('ul').children().eq(0).remove()
        })
</script>
```