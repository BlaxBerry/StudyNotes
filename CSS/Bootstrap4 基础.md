



# Bootatrap v4



## 下载引入

官网下载

页面中引入下载的css文件和js文件





## 布局容器

- **.container**	固定自适应

- **.container-fluid**    100%流式布局





## 栅格

5种响应尺寸，12列的布局

- **.row**	  一行
- **.col-屏幕类型-列数**    一行中一列

```html
<div class="row">
  <div class="col-sm-2">第一列</div>
  <div class="col-sm-2">第一列</div>
  <div class="col-sm-3">第三列</div>
  <div class="col-sm-3">第四列</div>
</div>
```



### 栅格等级

即换行的最小屏幕宽

栅格等级分为5个，分别对应5中屏幕

|                  | 超小屏    | 小屏         | 中等屏       | 大屏         | 超大屏       |
| ---------------- | --------- | ------------ | ------------ | ------------ | ------------ |
| 范围             | <576px    | >= 576px     | >=768px      | >=992px      | >=1140px     |
| 屏幕内容器的最大 |           | 540px        | 720px        | 960px        | 1140px       |
| 前缀             | **.col-** | **.col-sm-** | **.col-md-** | **.col-lg-** | **.col-xl-** |

如下：

3列平分1行

当屏幕小于576px时才会换行，不然三列一行内显示

```html
<div class="row">
  <div class="col-sm">第一列</div>
  <div class="col-sm">第二列</div>
  <div class="col-sm">第三列</div>
</div>
```





### 栅格列数

容器中一行分为12列的布局

若不设置所占行数，则默认平分

可以通过设置元素所占行数来分配一行内的空间分配

---

#### 列数=12

行中的列数相加=12，会沾满整行

```html
<div class="row">
  <div class="col-sm-4">第一列</div>
  <div class="col-sm-4">第二列</div>
  <div class="col-sm-4">第三列</div>
</div>
```

#### 列数<12

行中的列数相加<12，在行中会以空位形式展示不足的部分

```html
<div class="row">
  <div class="col-sm-4">第一列</div>
  <div class="col-sm-4">第二列</div>
  <!--空出4列大小的位置-->
</div>
```

#### 列数>12

行中的列数相加>12，

超出的部分盒子会直接换行

```html
<div class="row">
  <div class="col-sm-4">第一列</div>
  <div class="col-sm-4">第二列</div>
  <!--另起一行-->
  <div class="col-sm-5">第三列</div>

</div>
```



### 多个屏幕模式多个分配模式

可以指定多个分配模式，

分别对应不同屏幕尺寸下的一行内列的排列样式

如下：

- 小屏幕： 2 5 5
-  中等屏幕：4 4 4 
- 大屏幕：6 6  6 （第三列另起一行）

```html
<div class="container-fluid">
  <div class="row">
        <div class="d1 col-sm-2 col-md-4 col-lg-6">第一列</div>
        <div class="d2 col-sm-5 col-md-4 col-lg-6">第二列</div>
        <div class="d3 col-sm-5 col-md-4 col-lg-5">第三列</div>
  </div>
</div>
```





### 切割行（另起一行）

**.w-100** 实现一行内分割列

```html
<div class="row">
  <div class="col-sm">第一列</div>
  <div class="col-sm">第二列</div>
  <div class="w-100"></div>
  <div class="col-sm">第三列</div>
  <div class="col-sm">第四列</div>
</div>
```

若元素设置了所占行数，

则切割行后，空余的位置不会被填充

```html
<div class="row">
  <div class="col-sm-3">第一列</div>
  <div class="col-sm-3">第二列</div>
  <div class="w-100"></div>
  <div class="col-sm-3">第三列</div>
  <div class="col-sm-3">第四列</div>
</div>
```



### 排序

**.order-first**		强制该列排到第一

**.order-last**		强制该列排到最后

```html
<div class="row">
  <div class="d1 col-sm order-last">第1列</div>
  <div class="d1 col-sm">第2列</div>
  <div class="d1 col-sm order-first">第3列</div>
</div>
```







## 显示隐藏

### .b-block / .b-none

即，**根据屏幕尺寸的不同显示隐藏元素（自适应显示隐藏）**

- **.d-none**  在所有屏幕尺寸上都隐藏

- **.d-屏幕模式-none**  在指定屏幕模式时隐藏

- **.d-block**  在所有屏幕尺寸上都显示（默认）

- **.d-屏幕模式-block**  从指定屏幕模式开始显示，小于该屏幕尺寸的隐藏

| Screen size         | Class                             |
| ------------------- | --------------------------------- |
| Hidden on all       | `.d-none`                         |
| Hidden only on xs   | `.d-none .d-sm-block`             |
| Hidden only on sm   | `.d-sm-none .d-md-block`          |
| Hidden only on md   | `.d-md-none .d-lg-block`          |
| Hidden only on lg   | `.d-lg-none .d-xl-block`          |
| Hidden only on xl   | `.d-xl-none .d-xxl-block`         |
| Hidden only on xxl  | `.d-xxl-none`                     |
| Visible on all      | `.d-block`                        |
| Visible only on xs  | `.d-block .d-sm-none`             |
| Visible only on sm  | `.d-none .d-sm-block .d-md-none`  |
| Visible only on md  | `.d-none .d-md-block .d-lg-none`  |
| Visible only on lg  | `.d-none .d-lg-block .d-xl-none`  |
| Visible only on xl  | `.d-none .d-xl-block .d-xxl-none` |
| Visible only on xxl | `.d-none .d-xxl-block`            |

如下：

该元素从lg模式（大屏幕）的尺寸开始才显示，小于该尺寸的时候都隐藏

```html
<div class="d-none d-lg-block">元素</div>
```



### .visible  / .invisible

```html
<div class="visible"></div>

<div class="invisible"></div>
```







## Flex布局

大地方用Flex，其中用栅格布局

结合栅格

```html
<div calss="d-flex"></div>
```



### 主轴

```html
<div calss="d-flex flex-row"></div>
<div calss="d-flex flex-column"></div>
```



### 排序

```html
<div calss="d-flex flex-row"></div>
<div calss="d-flex flex-row-reserve"></div>

<div calss="d-flex flex-column"></div>
<div calss="d-flex flex-column-reserve"></div>
```



### 子元素垂直方向对齐

元素们在一行内垂直位置的上下对齐方式（flex布局的align-item和align-self）

---

给一行内的所有列设置统一对齐方式（flex布局的align-item）

- **.align-items-start**   行内所有列都顶部对齐（默认）

- **.align-items-center**   行内所有列都居中对齐

- **.align-items-end**    行内所有列都底部对齐

```html
<div class="row  align-items-center">
  	<div class="col-sm">第一列</div>
  	<div class="col-sm">第二列</div>
    <div class="col-sm">第三列</div>
</div>
```

---

给单独的列设置对齐方式（flex布局的align-self）

- **.align-items-start**    该列在行内顶部对齐

- **.align-items-center**   该列在行内居中对齐

- **.align-items-end**    该列在行内底部对齐

```html
<div class="row  align-items-center">
    <div class="col-sm  align-self-start">第一列</div>
    <div class="col-sm">第二列</div>
    <div class="col-sm align-self-end">第三列</div>
</div>
```



### 子元素水平方向对齐

元素们在一行内水平位置的左右对齐方式（flex布局的justify-content）

给一行内的所有列设置统一对齐方式

- **.justify-content-start**

- **.justify-content-center**

- **.justify-content-end**

- **.justify-content-around**

- **.justify-content-between**

```html
<div class="row  justify-content-end">
    <div class="d1 col-sm-2">第一列</div>
    <div class="d1 col-sm-2">第一列</div>
    <div class="d1 col-sm-2">第一列</div>
</div>
```







## 排版

### 浮动

```html
<div class="float-right"></div>
<div class="float-left"></div>
```

---

### 溢出隐藏/滚动条

```html
<!-- 溢出隐藏 -->
<div class="overflow-hidden">
</div>

<!-- 溢出滚动条 -->
<div class="overflow-auto">
</div>
```







## 块元素/行元素

- **.d-block**

- **.d-inline**

- **.d-inline-block**





## 宽高

**.w-？**    百分比宽度

```html
<div class="bg-success w-10">10%</div>
<div class="bg-success w-20">20%</div>
<div class="bg-success w-50">50%</div>
<div class="bg-success w-100">100%</div>
```

**.h-？**    百分比高度

```html
<div style="height: 100px">
  <div class="bg-success h-10">10%</div>
  <div class="bg-success h-20">20%</div>
  <div class="bg-success h-50">50%</div>
  <div class="bg-success h-100">100%</div>
</div>
```

**.vh-？**    屏幕高

**.vw-？**    屏幕宽

```html
  <div class="vh-100">asd</div>
```







## 距离 *

### 内外边距

- **.p-边距倍数**   	内边距

- **.m-边距倍数**	  外边距

边距倍数1～5

```html
<div class="m-1">qwe</div>
<div class="m-2">qwe</div>
<div class="m-3">qwe</div>
<div class="m-4">qwe</div>
<div class="m-5">qwe</div>
```

```html
<div class="p-1">qwe</div>
<div class="p-2">qwe</div>
<div class="p-3">qwe</div>
<div class="p-4">qwe</div>
<div class="p-5">qwe</div>
```

---

- **.px-?   左右内边距**
- **.py-?   上下内边距** 
- **.pt-?**    上内边距
- **.pb-?**   下内边距
- **.pl-?**    左内边距
- **.pr-?**    右内边距

---

- **.mx-?   左右外边距**
- **.my-?   上下外边距** 
- **.mt-?**    上外边距
- **.mb-?**   下外边距
- **.ml-?**    左外边距
- **.mr-?**    右外边距









## 文字

### 标题

h1~h6可以作为类名添加给其他元素，去改变其字体大小

```html
<h1>Hello</h1>

<p>123123</p>
<p class="h1">Hello</p>
<p class="h6">123123</p>
```



### 文本样式

#### 粗细

| **.font-weight-bold**   | 加粗文本 |
| ----------------------- | -------- |
| **.font-weight-normal** | 普通文本 |

```html
<p>hello</p>

<p class="font-weight-bold">hello</p>
<p class="font-weight-light">hello</p>
```

---

#### 倾斜

| **.font-italic** | 斜体文本 |
| ---------------- | -------- |

```html
    <p>hello</p>

    <p class="font-italic">hello</p>
```

---

#### 大小写

| **.text-lowercase** | 设定文本小写 |
| ------------------- | ------------ |
| **.text-uppercase** | 设定文本大写 |

---

#### 显示代码

直接书写代码片段或者通过HTML的<code\>标签，展现的都是压缩后的一行显示的代码

通过**<pre\> + <code\>**，可在页面展现代码原本的样式

```html
<pre>
  <code>
  	let a = 10;
  	let b = 20;
  
  	function getSum(a,b){
    	return a + b
  	}
  </code>
</pre>

<pre>
  <code>
    let <var>a</var> = 10;
    let <var>b</var> = 20;
  
    <var>var c = a + b</var>
    function getSum(a,b){
      return a + b
    }
  </code>
</pre>
```

可添加滚动条

```html
<pre class="pre-scrollbar" style="height:100px; width:300px">
  <code>
    let a = 10;
    let b = 20;
    
    function getSum(a,b){
      return a + b
    }
  </code>
  <code>
    let a = 10;
    let b = 20;
    
    function getSum(a,b){
      return a + b
    }
  </code>
</pre>
```





### 文本对齐

#### 水平方向（text-align）

| **.text-left**    | 左对齐                                      |
| ----------------- | ------------------------------------------- |
| **.text-center**  | 居中                                        |
| **.text-right**   | 右对齐                                      |
| **.text-justify** | 设定文本对齐,段落中超出屏幕部分文字自动换行 |
| **.text-nowrap**  | 段落中超出屏幕部分不换行                    |

---

### 垂直方向（表格中）

| **.align-baseline** |      |
| ------------------- | ---- |
| **.align-middle**   |      |
| **.align-top**      |      |
| **.align-bottom**   |      |
| **.align-left**     |      |

```html
<table style="height: 100px;">
  <tbody>
    <tr>
      <td class="align-baseline">baseline</td>
      <td class="align-top">top</td>
      <td class="align-middle">middle</td>
      <td class="align-bottom">bottom</td>
      <td class="align-text-top">text-top</td>
      <td class="align-text-bottom">text-bottom</td>
    </tr>
  </tbody>
</table>
```







### 溢出

#### 省略溢出

超出的文本换成 **...**

| **.text-truncate** | 省略溢出 |
| ------------------ | -------- |

```html
<p class="text-nowrap text-truncate">sascoidnvowenbvoiwehobiwebn</p>
```

---

#### 溢出换行

```html
<p class="text-nowrap text-break">sascoidnvowenbvoiwehobiwebn</p>
```





## 列表

**.list-unstyled** 	去除<li\>的默认圆点

```html
<ul class="list-unstyled">
  <li>111</li>
  <li>111</li>
  <li>111</li>
</ul>
```

<li\>**一行显示**

```html
    <ul class="list-unstyled list-inline">
      <li class="list-inline-item">111</li>
      <li class="list-inline-item">111</li>
      <li class="list-inline-item">111</li>
    </ul>
```





## 表格

给<table\>添加  **.table**类名 即可使用Bootstrap的表格样式（响应式）

```html
<table class="table">
  <tr>
    <th>#</th>
    <th>name</th>
    <th>age</th>
    <th>gender</th>
  </tr>
  <tr>
    <td>01</td>
    <td>andy</td>
    <td>28</td>
    <td>male</td>
  </tr>
</table>
```



### 表格边框

默认是不带边框的

```html
<table class="table table-bordered">
  <tr>
    <th>#</th>
    <th>name</th>
    <th>age</th>
    <th>gender</th>
  </tr>
  <tr>
    <td>01</td>
    <td>andy</td>
    <td>28</td>
    <td>male</td>
  </tr>
</table>
```



### 紧缩表格

**.table-sm**

去除增加的表单项的内边距

```html
<table class="table table-sm">
  <tr>
    <th>#</th>
    <th>name</th>
    <th>age</th>
    <th>gender</th>
  </tr>
  <tr>
    <td>01</td>
    <td>andy</td>
    <td>28</td>
    <td>male</td>
  </tr>
</table>
```



### 色彩样式

设定色彩

可以给整个表格，可以给单独一行设定颜色

```html
<table class="table table-sm table-danger">
  <tr>
    <th>#</th>
    <th>name</th>
    <th>age</th>
    <th>gender</th>
  </tr>
  <tr>
    <td>01</td>
    <td>andy</td>
    <td>28</td>
    <td>male</td>
  </tr >
  <tr class="table-success">
    <td>01</td>
    <td>andy</td>
    <td>28</td>
    <td>male</td>
  </tr>
</table>
```





#### 颜色反转

**.table-dark**

表单色变黑，字体颜色反选择

```html
<table class="table table-dark">
  <tr>
    <th>#</th>
    <th>name</th>
    <th>age</th>
    <th>gender</th>
  </tr>
  <tr>
    <td>01</td>
    <td>andy</td>
    <td>28</td>
    <td>male</td>
  </tr>
</table>
```

---

#### 标头背景色

- **.thead-light**    浅灰标头

- **.thead-dark**    深色标头 + 颜色反选

```html
<table class="table">
  <thead class="thead-light">
    <tr>
      <th>#</th>
      <th>name</th>
      <th>age</th>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>01</td>
      <td>andy</td>
      <td>28</td>
      <td>male</td>
    </tr>
  </tbody>
</table>
```

---

#### 隔行换色

**.table-striped**

```html
<table class="table table-striped">
  <tr>
    <th>#</th>
    <th>name</th>
    <th>age</th>
    <th>gender</th>
  </tr>
  <tr>
    <td>01</td>
    <td>andy</td>
    <td>28</td>
    <td>male</td>
  </tr>
</table>
```

---

#### 鼠标悬停高亮

**.table-hover**

```html
<table class="table table-hover">
  <tr>
    <th>#</th>
    <th>name</th>
    <th>age</th>
    <th>gender</th>
  </tr>
  <tr>
    <td>01</td>
    <td>andy</td>
    <td>28</td>
    <td>male</td>
  </tr>
</table>
```



### 溢出滚动条

**.table-responsive-sm**	屏幕小于768px时出现滚动条

```html
<table class="table table-responsive-sm">
  <tr>
    <th>#</th>
    <th>name</th>
    <th>age</th>
    <th>gender</th>
  </tr>
  <tr>
    <td>01</td>
    <td>andy</td>
    <td>28</td>
    <td>male</td>
  </tr>
</table>
```







## 颜色 + 背景+ 阴影 + 边框

### 文本颜色

| 类名                |
| ------------------- |
| **.text-primary**   |
| **.text-secondary** |
| **.text-success**   |
| **.text-danger**    |
| **.text-warning**   |
| **.text-info**      |
| **.text-light**     |
| **.text-dark**      |
| **.text-hide**      |

---

### 背景颜色

| 类名                |
| ------------------- |
| **.bg-primary**     |
| **.bg-secondary**   |
| **.bg-success**     |
| **.bg-danger**      |
| **.bg-warning**     |
| **.bg-info**        |
| **.bg-light**       |
| **.bg-dark**        |
| **.bg-transparent** |

---

### 边框

通过 **.border-？**类名添加边框

默认是加上四个边框

```html
<div class="border"></div>
```

也可单独指定某一边

```html
<div class="border-top"></div>
<div class="border-left"></div>
<div class="border-bottom"></div>
<div class="border-right"></div>
```

清除边框

```html
<div class="border-0"></div>

<div class="border-top-0"></div>
<div class="border-left-0"></div>
<div class="border-bottom-0"></div>
<div class="border-right-0"></div>
```

---

### 边框色

默认颜色为淡灰色

通过 **.border-？**类名添加边框色

```html
<div class="border border-success"></div>

<div class="border-top border-success"></div>
<div class="border-left border-success"></div>
<div class="border-bottom border-success"></div>
<div class="border-right border-success"></div>
```

---

### 圆角

```html
<img src="..." class="rounded-top" alt="">
<img src="..." class="rounded-right" alt="">
<img src="..." class="rounded-bottom" alt="">
<img src="..." class="rounded-left" alt="">
<img src="..." class="rounded-0" alt="">
```

```html
<!-- 圆形 -->
<div class="rounded-circle"></div>
<!-- 胶囊 -->
<div style="width:100px" class="rounded-pill"></div>
```



### 阴影











## 图片

### 响应式大小

| **.img-fluid** | 图片大小响应屏幕大小 |
| -------------- | -------------------- |

即，根据屏幕的大小自动适应

即设置了 **max-width: 100%;** 、 **height: auto;** 

```html
<img src="01.jpg" alt="" class="img-fluid" />
```



### 图片居中

```html
<img src="01.jpg" alt="" class="d-block mx-auto" />
```



### picture + source

屏幕尺寸在600px以上时，显示big.jpg

不到600px时，显示samll.jpg

```html
<picture>
  <source srcset="small.jpg" type="image/svg+xml" media="(max-width:600px)">
  <img src="big.jpg" class="img-fluid" alt="...">
</picture>
```
