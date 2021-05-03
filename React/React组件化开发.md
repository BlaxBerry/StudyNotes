# React组件化开发

```html
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
</script>
```



## 创建组件

### 函数式组件

适用于简单组件

- 组件名首字母要大写，

- 渲染到页面时，参数是标签形式

- 组件标签必须闭合，自闭和或者双标签闭合都可以

```react
// 1.函数定义组件
  function Demo() { 
      return <h2>Hello</h2>
   }

// 2. 渲染组件到页面
   ReactDOM.render(
       <Demo></Demo>,
       document.getElementById('root')
   )
```

```react
// 1.函数定义组件
  function Demo() { 
      return <h2>Hello</h2>
   }

// 2. 渲染组件到页面
   ReactDOM.render(
       <Demo/>,
       document.getElementById('root')
   )
```

函数的调用者是React





### 类式组件

适用于复杂组件