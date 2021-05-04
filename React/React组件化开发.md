#  React组件化开发

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

- 组件名首字母要大写
- 函数必须要有返回值，返回组件标签

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

**React的执行步骤**：

1. `ReactDOM.render()`解析组件标签，找到创建的函数组件

2. 调用该函数定义的标签
3. 将返回的虚拟DOM转换真实DOM

4. 渲染呈现在页面





### 类式组件

适用于复杂组件

类组件首字母大写

类组件创建时必须继承 React.Component 这个React自带组件

必须要写**render**，切必须要有返回值

```react
// 1. 定义类组件
class Demo extends React.Component {
  render{
    return <h2>Hello</h2>
  }
}

// 2. 渲染组件
ReactDOM.render(
  <Demo></Demo>,
  document.getElementByID('root')
)
```

render方法放在创建的类的原型对象上，

所以render中的this指向创建的组件的实例对象

**React的执行步骤**：

1. `ReactDOM.render()`解析组件标签，找到创建的类组件

2. React自动new出该类的实例，并通过该实例调用到原型上的render方法
3. 将返回的虚拟DOM转换真实DOM

4. 渲染呈现在页面





## 简单组件 复杂组件

简单组件：函数组件

复杂组件：类组件，带有state、props、refs属性





## 类组件中的this指向

- **构造器函数**中的 **this**：

  React执行该类组件时new创建出的该类组件的**实例对象**

-  **render** 中的 **this**：

  React执行该类组件时new创建出的该类组件的**实例对象**

- **一般的方法**中的 **this**：

  **一般作为事件的回调函数**，是直接调用不是类组件的实例调用

  所以this不指向该类组件的实例对象

  又因为类中的方法默认开启了严格模式（直接调用的函数中的this从指向window变为指向undefined）

  所以最终this指向 **undefined**

```react
class Demo extends React.Component {
    constructor(props){
        super(props)
        console.log(this); // 类组件的实例对象
    }
  render() {
      console.log(this);  // 类组件的实例对象
      return <h1 onClick={this.say}>hello</h1>
  }
  say(){
      console.log(this); // undefined
  }
}

ReactDOM.render(
    <Demo/>, 
    document.getElementById("root")
);
```

### bind() 修改一般方法中的this指向

在构造器函数中修改

```react
class Demo extends React.Component {
    constructor(props){
        super(props)
      // 修改方法的this指向
        this.fun = this.say.bind(this)
    }
  render() {
      return <h1 onClick={this.fun}>hello</h1>
  }
  say(){
      console.log(this);
  }
}

ReactDOM.render(
    <Demo/>, 
    document.getElementById("root")
);
```

```react
class Demo extends React.Component {
    constructor(props){
        super(props)
    }
  render() {
     // 修改方法的this指向
      return <h1 onClick={this.say.bind(this)}>hello</h1>
  }
  say(){
      console.log(this);
  }
}

ReactDOM.render(
    <Demo/>, 
    document.getElementById("root")
);
```

### 箭头函数修改一般方法中的this指向

```react
class Demo extends React.Component {
    constructor(props){
        super(props)
        this.state = {
            flag:true
        }
    }
  render() {
      return (
        <h1 onClick={this.say}>
          {this.state.flag?'true':'false'}
        </h1>
      )
  }
  say = () => {
      this.setState({
          flag:!this.state.flag
      })
  }
}

ReactDOM.render(
    <Demo/>, 
    document.getElementById("root")
);
```





## 组件实例的三大属性

- **state**
- **props**
- **refs**

在类组件的实例对象上

可在构造函数中通过打印 this 展现

```react
class Demo extends React.Component {
    constructor(props){
        super(props)
        console.log(this) // 实例对象的所有属性
    }
  render() {
      return <h1>Hello</h1>
  }
}

ReactDOM.render(
    <Demo/>, 
    document.getElementById("root")
);
```





## state 状态

state属性被放在类组件的实例对象上

通过更新组件的state来更新相对应的页面显示（渲染）

---

### 初始化状态数据

数据以**key-value 对象形式**放在构造函数中的 **this.state** 上

```react
class Demo extends React.Component {
    constructor(props){
        super(props)
        this.state = {
            name:"andy"
        }
    }
  render() {
      return <h1>Hello{this.state.name}</h1>
  }
}

ReactDOM.render(
    <Demo/>, 
    document.getElementById("root")
);
```

---

### 不能直接修改状态数据

**状态state中的数据不可以直接去更改**

如下，直接通过事件触发方法修改类组件中的数据的话，

React不认可不会有任何交互变化

```react
class Demo extends React.Component {
    constructor(props){
        super(props)
        this.state = {
            isHot:true
        }
    }
  render() {
      return (
          <h1 onClick={this.change.bind(this)}>
              今天天气{this.state.isHot?"炎热":"凉爽"}
          </h1>
      )
  }
  change(){
      this.state.isHot = !this.state.isHot
      console.log(this.state.isHot );
  }
}

ReactDOM.render(
    <Demo/>, 
    document.getElementById("root")
);
```

若想修改state状态中的数据，

必须通过类组件继承的React.Component组件的原型上的一个内置API

`setState`来实现

```react
class Demo extends React.Component {
    constructor(props){
        super(props)
        this.state = {
            isHot:true
        }
    }
  render() {
      return (
          <h1 onClick={this.change.bind(this)}>
              今天天气{this.state.isHot?"炎热":"凉爽"}
          </h1>
      )
  }
  change(){
      console.log(this); 
    //沿着原型链找到继承的React.Component组件的 setState方法
  }
}

ReactDOM.render(
    <Demo/>, 
    document.getElementById("root")
);
```

---

### 修改状态数据 setState()

只更新被修改的状态内容（合并），并不是替换原来state中的所有状态

```react
class Demo extends React.Component {
    constructor(props){
        super(props)
        this.state = {
            isHot:true
        }
    }
  render() {
      return (
        <h1 onClick={this.change.bind(this)}>
          今天天气{this.state.isHot?"炎热":"凉爽"}
        </h1>
      )
  }
  change(){
      this.setState({
          isHot:!this.state.isHot
      })
  }
}

ReactDOM.render(
    <Demo/>, 
    document.getElementById("root")
);
```

constructor构造器函数值调用了一次，即初始化时

render函数调用了 1+n 次，即第一次初始化 + setState修改更新的次数

触发setState的方法函数调用了n次，即 setState修改更新几次调用几次

---

### 正式开发时的简写

虽然上面的写法时标准写法，但是太复杂

正式开发中一般采用如下写法：

```react
class 类组件名 extends React.Component {
  state = {
    key: value,
    key: value
  }
  render(){
    return <h1>Hello</h1>
  }
  fun = () => {
    this.setState({
      key: value,
      key:value
    })
  }
}
```

如下：点击切换内容：

```react
class Demo extends React.Component {
    // 初始化
    state = {
      wind: "强风",
      isHot: true,
    };
    render() {
      return (
        <h1 onClick={this.change}>
          今天天气{this.state.wind}, 天气
          {this.state.isHot ? "炎热" : "凉爽"}
        </h1>
      );
    }
    // 赋值语句+箭头函数自定义方法
    change = () => {
        this.setState({
            isHot:!this.state.isHot
        })
    };
  }

  ReactDOM.render(
      <Demo/>, 
      document.getElementById("root")
  );

```





## props



## refs



## context