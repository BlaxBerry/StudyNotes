# React 基础

<img src="https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2019/07/react.jpg" style="zoom:50%;" />

[尚硅谷React](https://www.bilibili.com/video/BV1wy4y1D7JT?p=11&spm_id_from=pageDriver)



## 前提技术

React对Javascript基础要求高

- this指向

- class类

- ES6语法

- 原型、原型链

- 数组常用API

- 模块化

- npm包管理



## 优点

### 原生 JS 和 jQuery的缺点

- 命令式编程，逐步执行命令，书写代码量多

- 是通过**DOM-API**直接操作页面真实DOM

  频繁操作DOM会导致浏览器大量**重绘重排**，效率低下

- 数据更新后渲染页面是**全部替换**，

  即使只有一处修改低也是全部替换

- 没有组件化编码方案，代码复用率低

---

---

### React 优点

- 组件化开发

- 声明式编码

  仅仅需要获取请求操作数据，React自动操作DOM

---

- 可在**React Native**中进行移动端开发

  只通过React语法，而不是Javva、swift等

---

- 不直接操作页面DOM，而是使用**虚拟DOM**

- 通过**diffing算法**比较虚拟DOM和真实DOM

  仅渲染有差异的内容，并不是全部重新渲染

  尽量减少与DOM的交互，提高效率

- 只专注于将虚拟DOM数据渲染到HTML视图

---

---

### React高效的原因

- **虚拟DOM**

不总是直接操作页面中的真实的DOM

- **DOM Diffing算法**

比较虚拟DOM和真实DOM的差距，仅渲染差距给页面

最小化页面的重绘





## CDN引入 

学习期间使用引入文件到HTML的方法

需要的依赖包：

- react-development.js
- react-dom-developmeny.js
- babel.js
- prop-types.js

```react
<body>
  <!-- 创建容器 -->  
    <div id="root"></div>
  
   
  <!-- 1. 引入react核心库 -->
    <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
    
  <!-- 2. 引入react-dom，操作DOM -->
    <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    
  <!-- 3. 引入babel 将JSX转为JS-->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    
  <!-- 代码 type使用babel编译解析JSX -->
    <script type="text/babel">
        const VDOM= <h1>Hello React</h1>; 
        ReactDOM.render(VDOM,document.getElementById('root'))
    </script>

</body>
```





## 虚拟DOM

- 本质是个Object类型对象

- 只是在React中使用，不需要和真实DOM上那么多的属性

- 最终会被React转换为真实DOM，并渲染到页面上





## 为什么使用 JSX 语法

### 为什么不建议使用原生JS

在React中需要先创建虚拟DOM，然后再渲染

创建虚拟DOM时若使用原生JS，会导致代码量太多，太复杂

所以实际开发不会用

#### 原生JS创建虚拟DOM

需要通过`React.createElement()`创建虚拟DOM

```react
React.createElement(component, props, ...children)
```

繁琐，代码量太多

```react
<body>
  <!-- 创建容器 -->  
    <div id="root"></div>
  
  <!-- 1. 引入react核心库 -->
    <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <!-- 2. 引入react-dom，操作DOM -->
    <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
   
    <script type="text/javascript">
        const VDOM= React.createElement(
          'h1',
          {id:'title'},
          'Hello React'}
        );
        ReactDOM.render(VDOM,document.getElementById('root'))
    </script>

</body>
```

```js
const VDOM = React.createElement(
    'h1', 
    {
      id: 'title'
    },
    son);

ReactDOM.render(
    VDOM,
    document.getElementById('root')
)
```



#### 原生JS创建嵌套虚拟DOM

需要在子节点再创建虚拟DOM，繁琐

代码量太多

```js
const son = React.createElement(
    'span', 
    {},
    'Hello React 02');

const VDOM = React.createElement(
    'h1', 
    {
      id: 'title'
    },
    son);

ReactDOM.render(
    VDOM,
    document.getElementById('root')
)
```



### 使用JSX创建嵌套虚拟DOM

可以理解为原生JS的语法糖

```react
<body>
    <!-- 创建容器 -->
    <div id="root"></div>

    <!-- 1. 引入react核心库 -->
    <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
    <!-- 2. 引入react-dom，操作DOM -->
    <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    <!-- 3. 引入babel 将JSX转为JS-->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <!-- 代码 type使用babel编译解析JSX -->
    <script type="text/babel">
    
        const VDOM= (
        	<div id="father">
            <div id="son">hello</div>
        	</div>
        ); 
        
        ReactDOM.render(VDOM,document.getElementById('root'))
    </script>

</body>
```



## JSX语法

JSX（JavaScript XML），即 JS + XML

React定义的一种类似XML的JS扩展

本质是`React.createElement()`方法的语法糖

定义虚拟DOM时的有以下注意点：

### 1、在括号内直接定义虚拟DOM

不写引号，用小括号包起

```jsx
const VDOM = (
  <div id="father">
    <div id="son">Hello React</div>
  </div>
)
```

### 2、只能有一个跟标签

创建的 **虚拟DOM只能有一个根标签**，

不能有多个同级

```jsx
const VDOM = (
  <div>
    <div></div>
    <div></div>
	</div>
); 
ReactDOM.render(VDOM, document.getElementById("root"));
```

### 3、{ } 插入 Javascript表达式

标签中混人 **JS表达式** 时，使用花括号 **{}**

```jsx
const attrFather = "fatHER"
const attrSon = "sOn"
const content = 'Hello React'

const VDOM = (
  <div id={attrFather.toLowerCase()}>
    <div id={attrSon.toLowerCase()}>{content }</div>
  </div>
)
```

JSX只能写JS表达式不能写JS语句，

表达式，必须有一个结果返回值

```js
a
a + b
fun(a)
function fun(){}
```

语句代码

```
if(){}
for(){}
switch(){case:XXX}
```

### 4、 外联class类名样式

标签中写入 **CSS的class类名** 时，使用 **className**

因为class是ES6中的类的关键字，为了避免使用关键字

样式可以通过引入 css文件的方式

```react
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React 5</title>
    
    <!-- 1. 引入react核心库 -->
    <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
		<!-- 2. 引入react-dom，操作DOM -->
    <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
    <!-- 3. 引入babel 将JSX转为JS-->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <!-- 引入css样式文件-->
    <link rel="stylesheet" href="./01.css" />
  </head>

  <body>
    <!-- 创建容器 -->
    <div id="root"></div>

    <!-- 代码 type使用babel编译解析JSX -->
    <script type="text/babel">
      
      const VDOM = (
        <div id="father" className="father">
          i am father
          <div id="son" className="son">
            i am son
          </div>
        </div>
      );
      ReactDOM.render(VDOM, document.getElementById("root"));
    </script>
  </body>
</html>

```

### 5、内联style样式

JSX中的内联样式使用 双花括号 **{{}}**  和  **驼峰命名**

```jsx
style = {{key: value, key: value}}
```

```react
const VDOM = (
    <div id="father" className="father">
        i am father
        <div id="son" className="son">
            i am son
            <div style={{
                color: 'orangered',
                fontSize: '20px',
                backgroundColor: 'orange'}}>hello</div>
        </div>
    </div>
    ); 
ReactDOM.render(VDOM, document.getElementById("root"));
```

### 6、标签必须闭合

JSX中的虚拟DOM标签必须是闭合的，

如果是创建单标签，可以选择自闭合

```jsx
const VDOM = (
  <div>
    <div></div>
    <div></div>
    <input/>
	</div>
); 
ReactDOM.render(VDOM, document.getElementById("root"));
```

### 7、标签首字母大小写

JSX并不是创建HTML标签，而是将虚拟DOM转换为HTML标签

- 创建的如果虚拟DOM标签名字的**首字母是小写**，

  则被转换为HTML标签的同名标签，

  若不存在该标签，则报错

- 如果虚拟DOM标签名字的**首字母是大写**，

  则被当作React组件，

  若不存在该组件，则报错

```react
const VDOM = (
  <div>
    <div></div>
    <DIV></DIV>
  </div>
)
```

### 8、动态遍历创建虚拟DOM

JSX创建虚拟DOM时，如果插入的JS表达式是个数组

React会自动把所有元素遍历出

```react
const data = ["Reacr", "Vue", "angular"]; 

const VDOM = (
<div>
    <h1>{data}</h1>
</div>
); 

ReactDOM.render(VDOM, document.getElementById("root"));
```

可以利用这一特性**遍历**生成元素

并且，遍历的元素需要指定**唯一的key属性**，

Diffing算法需要靠这个唯一的属性比较虚拟DOM

```react
const data = ["Reacr", "Vue", "angular"]; 

const VDOM = (
<div>
    <h1>JS前端框架列表</h1>
    <ul>
        {
            data.map((item,index)=>{
                return <li key={index}>{item}</li>
            })
        }
    </ul>
</div>
); 

ReactDOM.render(VDOM, document.getElementById("root"));
```

```react
<ul id="list"></ul>

<script>
let person = [
  {id:'001',name:'Andy',age:18},
  {id:'002',name:'Tom',age:10}
]

let str = ''
person.foeEach(item=>{
  str += `<li>${item.id}--${item.name}</li>`
})
  
document.getElementById('list').innerHTML = str
</script>
```



