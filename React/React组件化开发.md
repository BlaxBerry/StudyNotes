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

必须要写**render**，切必须要有**return**返回值

如果返回的标签是嵌套关系，可以在return后使用括号包裹

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





## state (自身状态)

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





## props (父组件传入数据)

每个组件都有props属性

组件中所有的属性都保存在props中

用于从组件外部传递数据给组件内部

### 传入数据和接收数据

通过自定义属性从组件标签传入数据给组件

组件内部通过this.props.属性名接收数据

props接受的是个对象的形式

```react
class Person extends React.Component {
    render(){
        return (
            <ul>
                <li>姓名：{this.props.name}</li>
                <li>性别：{this.props.gender}</li>
                <li>年龄：{this.props.age}</li>
            </ul>
        )
    }
}

ReactDOM.render(
  <Person 
      name="Andy" 
      gender="female" 
      age="28"/>, 
  document.getElementById("root")
);
```

解构赋值

```react
class Person extends React.Component {
    render(){
        const {name,gender,age} = this.props
        return (
            <ul>
                <li>姓名：{name}</li>
                <li>性别：{gender}</li>
                <li>年龄：{age}</li>
            </ul>
        )
    }
}

ReactDOM.render(
  <Person 
      name="Andy" 
      gender="female" 
      age="28"/>, 
  document.getElementById("root")
);
```

---

### 只读属性不可修改

react是单向数据流，

props属性是**只读属性**

即在组件内只能接受props数据，

但在组件内部**不允许修改props**（修改接受的数据并赋值回去）

---

### 批量传入数据

以**对象形式**声明数据，

结构结构对象以 key-val 标签属性的形式传入数据，

组件内通过props接受数据（可通过解构赋值方式）

```react
class Person extends React.Component {
    render(){
        const {name,age} = this.props
        return (
            <ul>
                <li>{name}</li>
                <li>{age}</li>
            </ul>
        )
    }
}

const data = {name:'andy',age:19}
ReactDOM.render(
  <Person 
      {...data}/>, 
  document.getElementById("root")
)
```

---

### 规定和限制传入数据

#### 传入数据的类型

因为作为标签的属性传入的

数字型数据需要加上花括号 **{}**，不然会被解析为字符串

```react
class Person extends React.Component {
    render(){
        const {num,str} = this.props
        return (
            <ul>
                <li>{num + 1}</li>
                <li>{str + 1}</li>
            </ul>
        )
    }
}

ReactDOM.render(
  <Person 
      num={18} 
      str="18"
  />, 
  document.getElementById("root")
  );
```

#### 设置数据类型的限制

15版本之前的限定数据类型：

```react
class 组件名 extends React.Component{
  
}

组件名.propTypes = {

  属性:React.PropTypes.string

}
```

15.5以后的限定数据类型：

将Proptype作为一个依赖包引入

```react
// 引入 prop-types.js依赖包

class 组件名 extends React.Component{
  
}

组件名.propTypes = {
	// 限制该属性值的数据类型为 字符串
  属性:PropTypes.string,
  // 限制该属性值的数据类型为 数字
  属性:PropTypes.number,
  // 限制该属性值的数据类型为 函数
  属性:PropTypes.func,

}
```

#### 设置必须数据

```react
// 引入 prop-types.js依赖包

class 组件名 extends React.Component{
  
}

组件名.propTypes = {

  属性:PropTypes.string.isRequired

}
```

#### 设置数据默认值

```react
class 组件名 extends React.Component{
  
}

组件名.defaultProps = {
  属性:默认值,
  属性:默认值
}
```

---

### 正式开发中的简写

上述的限制接受数据的规则

可通过 **static 静态属性** 写在类的里面

```react
class 组件名 extends React.Component {
  // 对标签属性进行类型和必要性的限制
  static propTypes = {
    属性:Proptypes.string.isRequired,
    属性:Proptypes.number
  }
	// 设定标签属性的默认值
  static defaultProps = {
    属性:默认值,
    属性:默认值
  }

  render(){
    const {属性，属性} = this.props
    return <h1>Hello{属性}</h1>
  }
}

ReactDOM.render(
  <组件 
    属性名=“值”
    属性名={值}></组件>,
  document.getElementByID('root')
)
```

---

### 函数式组件的props属性

函数式组件也可以接受外部传入的数据

也是通过标签属性传入，再通过props属性接收

传入的标签属性以**对象形式**被当作函数的**参数**

```react
function Person(props){
    console.log(props)
  //{name: "andy", age: 18}
    return (
        <ul>
            <li>姓名：{props.name}</li>
            <li>年龄：{props.age}</li>
        </ul>
    )
}

ReactDOM.render(
  <Person 
      name="andy" 
      age={18}/>, 
  document.getElementById("root")
)
```

对props数据的限制

```react
function 组件(){
  return (
  	<h1>hello</h1>
  )
}
// 限制传入的数据的类型和必要性
组件名.propTypes = {

  属性:PropTypes.string.required

}
// 设置数据的默认值
组件名.defaultProps = {
  属性:默认值,
  属性:默认值
}
```





## refs (子组件数据)

### ref 属性名的设置与获取

标签可以通过 **ref 属性 **来标识自己（可理解为 id 属性）

```react
<节点 ref="自定义名"></节点>
```

在组件内部通过 **refs.ref属性名**，获取标记的真实DOM

```react
this.refs.ref属性的自定义名

this.refs.ref属性的自定义名.value
```

```react
class 组件 extends React.Component {
  
  render(){
    return (
    	<div>
        <input ref="inp1"></input>
        <button ref="btn" 
          			onClick={this.handle}>
          			点击
        </button>
        <input ref="inp2"></input>
      </div>
    )
  }
  
  handle = ()=>{
    console.log(this)  // 类组件的实例对象
    console.log(this.refs)   
    // {inp1: input, btn: button, inp2: input}
    console.log(this.refs.inp1) // 对应的节点
    console.log(this.refs.inp1.value)
  }
}

ReactDOM.render(
  <组件名></组件名>,
  document.getElementByID('root')
)
```

---

### ref 属性 与 事件操作

```react
class Demo extends React.Component {
    render() {
      return (
        <div>
          <input ref="inp1"></input>
          <button ref="btn" 
            			onClick{this.onClick}>
            			click
          </button>
          <input ref="inp2" 
								 onBlur={this.onBlur}>
          </input>
        </div>
      );
    }
    onClick = ()=>{
        console.log(this.refs.inp1.value);
    }
    onBlur = ()=>{
        console.log(this.refs.inp2.value);
    }
  }

ReactDOM.render(
    <Demo/>,
    document.getElementById("root")
)
```

---

### ref 属性的写法

#### 1. 字符串形式

已经过时，效率低，官方不再推荐使用

16版本之前还可以使用

```react
// 设置
<节点 ref="自定义名"></节点>

// 获取
this.refs.自定义名
```

#### 2. 回调函数形式 (内联函数)

在执行组件中的render方法时会自动触发节点的回调函数

回调函数的参数就是当前所在节点本身

通过 **`this.自定义名 = 回调函数参数`** 

将参数(该节点)放在了类组件实例上并起了自定义名

```react
// 设置
<节点 ref={(currentNode)=>{this.自定义名}}></节点>

// 获取
this.自定义名
```

和字符串类型的不同在于：

不是从refs上获取，而是直接从this实例对象上获取

```react
class Demo extends React.Component {
    constructor(props){
          super(props)
          console.log(this); // 组件实例对象上有ref回调保存的属性
      }
    render() {
      return (
        <div>
          <input ref={c => this.inp1 = c}></input>
          <button ref={c => this.btn = c} 
                  onClick={this.onClick}>
                  click
          </button>
          <input ref={c => this.inp2 = c} 
                 onBlur={this.onBlur}>
          </input>
        </div>
      );
    }
    onClick = ()=>{
        console.log(this.inp1.value);
    }
    onBlur = ()=>{
        console.log(this.inp2.value);
    }
  }

ReactDOM.render(
    <Demo/>,
    document.getElementById("root")
)
```

#### 3. 回调函数形式 (类的绑定方法)

ref 回调函数如果是 **内联样式** 

则回调函数在**render更新时会被执行两次**

第一次会被传入**null**，第二次才是当前的节点

[官方文档 回调函数的执行次数](https://react.docschina.org/docs/refs-and-the-dom.html)

解决方法

将内联式回调函数写为**类组件的方法**

实际上执行的次数无关紧要，一般还是内联样式回调函数写的多

```react
class Demo extends React.Component {
    constructor(props){
          super(props)
          console.log(this);
      }
    render() {
      return (
        <div>
        {/*<input ref={c=>this.inp1=c}></input>*/}
          <input ref={this.a}></input>
          <button ref={c=>this.btn=c} 
                  onClick={this.onClick}>
                  click
          </button>
          <input ref={c=>this.inp2=c} 
                 onBlur={this.onBlur}>
          </input>
        </div>
      );
    }
    onClick = ()=>{

        console.log(this.inp1.value);
    }
    onBlur = ()=>{
        console.log(this.inp2.value);
    }

    //
    a = (c)=>{
        this.inp1 = c
        // console.log(c);
        // console.log(this.inp1);
    }

  }

ReactDOM.render(
  <Demo/>,
  document.getElementById("root")
)
```

#### 4. React.createRef()



## context