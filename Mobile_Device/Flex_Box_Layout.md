# Flex布局

Flex就是**Flexible Box Layout 弹性布局**/伸缩布局

操作方便，布局简单，

**移动端应用广泛，**PC端兼容较差，IE11以下不支持



# 原理

**给父盒子添加flex属性，来控制 子盒子的位置 和 排列方式**

## dispaly: flex;

```html
    <style>
        div {
            display: flex;
        }
    </style>

    <div>
        <span>1</span>
        <span>2</span>
        <span>3</span>
    </div>
```

flex属性给父级元素指定，

添加了flex属性的元素叫容器，**flex container**

添加了flex属性的元素的子元素叫项目， **flex item**



任何一个容器块级行级元素都可以指定为flex布局

行级元素<sapn>的高宽时根据自身内容决定，直接修改没用的

如果给<span>的父级添加flex属性，则<span>直接转为块级元素





## 主轴和侧轴

flex布局中，主要分为主轴和侧轴两个方向，也称为行和列

**主轴** ： 默认是x轴，也就是水平行

**侧轴** ： 默认是y轴，也就是垂直列

![](https://segmentfault.com/img/bVbG2zL)

侧轴跟着主轴，

**主轴由flex-direction决定**





![](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=652982918,2180743849&fm=26&gp=0.jpg)





# flex布局 父级属性

flex属性给父级元素指定

## flex-direction

**设置主轴的方向** 

```css
.container {
    flex-direction: row | row-reverse | column | column-reverse;
}
```

![](https://segmentfault.com/img/bVbG2zM)

![](https://www.ameamelog.com/wp/wp-content/uploads/2019/09/flexbox-flex-direction_main.png)



### flex-direction: row;

默认，可以省略不写

把主轴设置为 水平方向的x轴，

子元素沿着父元素盒子左侧开始排列，从左往右

![](https://pic2.zhimg.com/80/v2-ae8828b8b022dc6f1b28d5b4f7082e91_1440w.jpg)

```html
    <style>
        div {
            display: flex;
            flex-direction:row;
        }
    </style>
</head>

<body>
    <div>
        <span>1</span>
        <span>2</span>
        <span>3</span>
    </div>
</body>
```



### flex-direction: row-reverse;

了解即可

把主轴设置为水平方向的x轴，

子元素沿着父元素盒子右侧开始排列，从右往左

![](https://pic3.zhimg.com/80/v2-215c8626ac95e97834eddb552cfa148a_1440w.jpg)

```html
   <style>
        div {
            display: flex;
            flex-direction:row-reverse;
        }
    </style>
</head>

<body>
    <div>
        <span>1</span>
        <span>2</span>
        <span>3</span>
    </div>
</body>
```



### flex-direction: column;

把主轴设为垂直方向的y轴

子元素沿着父元素盒子上侧开始排列，从上往下

![](https://pic1.zhimg.com/80/v2-33efe75d166a47588e0174d0830eb020_1440w.jpg)

```html
   <style>
        div {
            display: flex;
            flex-direction:column;
        }
    </style>
</head>

<body>
    <div>
        <span>1</span>
        <span>2</span>
        <span>3</span>
    </div>
</body>
```



### flex-direction: column-reverse；

了解即可

把主轴设为垂直方向的y轴

子元素沿着父元素盒子下侧开始排列，从下往上

![](https://pic2.zhimg.com/80/v2-344757e0fb7eee11e75b127b8485e679_1440w.jpg)

```html
   <style>
        div {
            display: flex;
            flex-direction:column-reverse;
        }
    </style>
</head>

<body>
    <div>
        <span>1</span>
        <span>2</span>
        <span>3</span>
    </div>
</body>
```

---

---



## justify-content

设置**主轴上**的项目（子元素）的排列方式

```css
.container {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

![](https://segmentfault.com/img/bVbG2zP)

要先设定好主轴, 

如下是X轴为主轴：

![](https://user.oc-static.com/upload/2018/06/14/15289918266602_2.png)

如下是Y轴为主轴：

![](https://ichi.pro/assets/images/max/724/1*6NUIFlnX9SAanhWeOwt3Bg.gif)



### justify-content: flex-start;

默认，可省略不写

设定父盒子的主轴上的项目**沿主轴开始方向排列**，从头到尾

如下，以主轴为x轴为例：

![](https://pic1.zhimg.com/80/v2-1bafab80044a7ab2a6198d5937172eb0_1440w.jpg)

```css
        div {
            display: flex;
            flex-direction: row;
            justify-content: flex-start;
        }
```



### justify-content:flex-end;

设定父盒子的主轴上的项目**沿主轴结束方向排列**，从尾到头

和row-reverse、column-reverse不同的是，不颠倒项目顺序，

如下，以主轴为x轴为例：

![](https://pic3.zhimg.com/80/v2-8b163809a4c944486a127a7c22eee7b2_1440w.jpg)

```css
        div {
            display: flex;
            flex-direction: row;
            justify-content: flex-end;
        }
```



### justify-content:center

设定父盒子的主轴上的项目**局中紧挨排列**，

如下，以主轴为x轴为例：下图项目是有外边距的

![](https://pic4.zhimg.com/80/v2-dea82c75d35f532d35a52d1f9c1c762b_1440w.jpg)

```css
        div {
            display: flex;
            flex-direction: row;
            justify-content: center;
        }
```



### justify-content:space-around;

把主轴上的空间平均分配给每一个项目

如下，以主轴为x轴为例：

![](https://pic1.zhimg.com/80/v2-42a358111a221ff52768bdd55238eb0c_1440w.jpg)

```css
        div {
            display: flex;
            flex-direction: row;
            justify-content: space-around;
        }
```



### justify-content:space-between;

**重要**

主轴上的项目**两端贴边，中间的项目平均分配剩余空间**

类似圣杯布局的感觉

如下，以主轴为x轴为例：

![](https://pic1.zhimg.com/80/v2-ea4061e0f64dd8d7a1fcb5b0ad6f96a8_1440w.jpg)

```css
        div {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
        }
```

和justify-content: space-around的区分：

![](https://media.geeksforgeeks.org/wp-content/uploads/20191028151900/1081.png)

---

---



## flex-wrap

**重要**

设定容器内项目(子元素)是否换行

```css
.container {
    flex-wrap: nowrap | wrap;
}
```

![](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F67884%2F8b29926b-b9c8-3861-6dbf-f96600c339c7.png?ixlib=rb-1.2.2&auto=format&gif-q=60&q=75&s=1647d5f9d48bcaf8c1640e89d8fc5c61)



### flex-wrap:nowrap;

以前的传统布局中，一行内如果放不下元素就会自动换行显示

flex布局**默认不换行**，

会自动调整项目大小（子元素）使所有项目排在主轴线上，即**会压缩子元素使其一行显示**

如下，以主轴为x轴为例：

![](https://pic4.zhimg.com/80/v2-a590927ad6d83de8840d52a0cf2f0df3_1440w.jpg)

```css
        div {
            display: flex;
            flex-direction: row;
            flex-wrap:nowrap;
        }
```



### flex-wrap:wrap;

**重要**

如下，以主轴为x轴为例：

![](https://pic2.zhimg.com/80/v2-426949b061e8179aab00cacda8168651_1440w.jpg)

```css
        div {
            display: flex;
            flex-direction: row;
            flex-wrap:wrap;
        }
```

---

---





## align-items （单行）

设置**侧轴上**子元素的排列方式

```css
.container {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

先确定好主轴

| 属性值      | 解释                                                         |
| ----------- | ------------------------------------------------------------ |
| flex-start  | 从侧轴起点开始排列                                           |
| flex-end    | 从侧轴终点开始排列                                           |
| flex-center | 在侧轴的居中                                                 |
| stretch     | 若项目height/width没有设定，将拉伸展满整个父盒子容器（默认） |
| baseline    | 项目的第一行文字的基线对其                                   |

如下：主轴是x轴时：

![](http://w3.unpocodetodo.info/css3/images/flex-align-items.gif)

如下：主轴时y轴时；

![](https://coliss.com/wp-content/uploads-201904/flexbox/17-align-items-column.png)



justify-content设定的是项目在主轴上的排列

align-items设定的是项目在侧轴上的排列

可理解为主轴在侧轴上的位置

![](https://segmentfault.com/img/bVbG2zL)



### align-items：flex-start

如下，主轴为x轴时：

![](https://5b0988e595225.cdn.sohucs.com/q_70,c_zoom,w_640/images/20180315/de536e930bd640ae9a4fd0c9b958506b.webp)

```css
        div {
            display: flex;
            flex-direction: row;
          	justify-content: space-between;
          	align-items: flex-start;
        }
```



### align-items: flex-end;

如下，主轴为x轴时：

![](https://5b0988e595225.cdn.sohucs.com/q_70,c_zoom,w_640/images/20180315/b2aab71f34374efb916ad531767151e5.webp)

```css
        div {
            display: flex;
            flex-direction: row;
          	justify-content: space-between;
          	align-items: flex-start;
        }
```



### align-items: center;

如下，主轴为x轴时：

![](https://5b0988e595225.cdn.sohucs.com/q_70,c_zoom,w_640/images/20180315/caea1b7fec5b4a3180c2179e4a2afc62.webp)

```css
        div {
            display: flex;
            flex-direction: row;
          	justify-content: space-between;
          	align-items: center;
        }
```



### align-items: stretch；

了解即可

如下，主轴为x轴时：

![](https://5b0988e595225.cdn.sohucs.com/q_70,c_zoom,w_640/images/20180315/d373d94b90fd48738d9aeb45fdd914aa.webp)

```css
        div {
            display: flex;
            flex-direction: row;
          	justify-content: space-between;
          	align-items: stretch;
        }
        /*
        子盒子没有设定height时，会拉伸铺满父盒子
        */
```



### align-items: baseline;

了解即可

如下，主轴为x轴时：

![](https://5b0988e595225.cdn.sohucs.com/q_70,c_zoom,w_640/images/20180315/2198754544454cb59d00da36f3db6602.webp)

```css
        div {
            display: flex;
            flex-direction: row;
          	justify-content: space-between;
          	align-items: baseline;
        }
        /*
        子盒子没有设定height时，会拉伸铺满父盒子
        */
```



### 项目水平居中垂直居中

就是项目在主轴上居中，并且在侧轴上也居中

`justify-content: center`;

`align-items:center;`

如下，主轴是x轴：

```css
        div {
            display: flex;
            flex-direction: row;
          	justify-content: center;
          	align-items: center;
        }
```

如下，主轴是y轴：

```css
        div {
            display: flex;
            flex-direction: column;
          	justify-content: center;
          	align-items: center;
        }
```

---

---





## align-content（多行）

**侧轴上**子元素的排列方式







## flex-flow

同时设置flex-direction和flex-wrap







# flex布局 子级属性



父级元素使用flex布局后，其自己元素的**float、clear、vertical-align**属性会失效

