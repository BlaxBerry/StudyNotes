# React 基础

<img src="https://cdn-ssl-devio-img.classmethod.jp/wp-content/uploads/2019/07/react.jpg" style="zoom:50%;" />



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



JSX 语法







使用

学习期间使用引入文件到HTML的方法

需要的依赖包：

- babel.min.js
- prop-types.js
- react.development.js
- react-dom-developmeny.js



编译高级JS语法编（ES6语法、import引入语法）

编译React的 JSX

























```html
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

