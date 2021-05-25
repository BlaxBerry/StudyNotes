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
      return <input/>
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

即通过 **`setState`**来实现

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

---

### setState() 的调用次数

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

在标签上通过 **ref 属性 **来标识标签（可理解为 id 属性）

```react
<节点 ref="自定义名"></节点>
```

在组件内部通过 **refs.ref属性名**，**获取标记的该标签**（真实DOM）

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

**将参数(该节点)放在了类组件实例上，并起了自定义名**

然后可直接通过获取类中自定义名获取存入的该节点

```react
// 设置
<节点 ref={(currentNode)=>{this.自定义名 = currentNode}}></节点>

// 获取
this.自定义名
```

和字符串类型的不同在于：

不再是从refs上获取，而是直接从this实例对象上获取

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

解决方法：将内联式回调函数写为**类组件的方法**

实际上执行的次数无关紧要，

一般还是内联样式回调函数写的多

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

#### 4. 创建专用容器 React.createRef()

通过React的内置API **React.createRef()** 在类组件中生成一个绑定属性，

作为一个容器，用来存储 被 **ref 属性标识的节点**

可通过 **this.自定义名.current** 访问这个节点

```jsx
// 生成容器
myRef = React.createRef()

// 设置ref
<input ref="myRef"/>
```

专人专用，**里面只能存一个节点**

后放入的替换之前放入的

所以需要多少就创建多少

虽然麻烦，但是目前React官方推荐

```react
class Demo extends React.Component {
    // constructor(props){
    //       super(props)
    //       console.log(this);
    //       console.log(this.myRef);
    //			 console.log(this.myRef.current);  // null
    //   }
    myRef = React.createRef()
    myRef2 = React.createRef()

    render() {
      return (
        <div>
            <input type="text"
                ref={this.myRef}
                onBlur={this.onBlur}
            /> 
            <input type="text"
                ref={this.myRef2}
                onBlur={this.onBlur2}
            /> 
        {/*<h3>{this.myRef.current.value}</h3>*/}      
        </div>
      )
    }
    onBlur = ()=>{
        console.log(this.myRef);
        console.log(this.myRef.current);
        console.log(this.myRef.current.value);
    }
    onBlur2 = ()=>{
        console.log(this.myRef2);
        console.log(this.myRef2.current);
        console.log(this.myRef2.current.value);
    }            

  }

ReactDOM.render(
    <Demo/>,
    document.getElementById("root")
)
```





## 事件处理

### onXxxx={this.事件名}

- 通过 **onXxxx 标签属性** 指定事件处理函数

```react
<div onClick={handle}></div>
<input onBlur={handle}></input>
```

- React使用的是自定义合成事件，不是原生DOM事件

  相当于 React 把原生DOM事件重新封装了一遍

- React中的事件时通过**事件委托**方式

  即，将事件委托给组件的**最外层元素**

  效率更高

### event.target

为了**避免 ref属性的过度使用**

当发生事件的元素就是要操作的元素时，

可通过 **event.target** 获得触发事件的DOM元素

```react
class Demo extends React.Component {
        
    render() {
      return (
        <div>
            <input type="text"
                   onBlur={this.onBlur}
            /> 
        </div>
      )
    }
    onBlur = ()=>{
        console.log(event.target);
        console.log(event.target.value);
    }
  }

ReactDOM.render(
  <Demo/>,
  document.getElementById("root")
)
```





```react
   handleMouse = (flag) => {

        return () => {
            this.setState({
                mouseFlag: flag
            })
        }
    }
```







## 表单数据 的收集

包含表单的组件分为：

- **非受控组件**:

  页面中的**数据现用现取**

- **受控组件**:

  **随着输入维护状态**

---

### 非受控组件

通过 **ref属性** 获取标签节点，

并将获取的节点作为类的属性存入，

然后通过类中该属性的value属性获取表单的输入内容

```react
class Login extends React.Component {
    render(){
        return(
          <form onSubmit={this.getData}>
              姓名：<input type="text" name="userName"
                          ref={c=>{this.userName = c}}/>
                  <br/>
              密码：<input type="password" name="password"
                          ref={c=>{this.passWord = c}}/>
                  <br/>
              <button>确定</button>
          </form>
        )
    }

    getData = ()=>{
        // 阻止表单的提交并刷新页面的默认行为
        event.preventDefault()
        const {userName,passWord} = this
        console.log(userName.value,passWord.value);
        // console.log(this);
        // console.log(this.userName.value);
        // console.log(this.passWord.value);
    }
      
}

ReactDOM.render(
    <Login/>,
    document.getElementById('root')
)

```

---

### 受控组件

不是通过 ref属性

通过 **onChange事件** 获取表单内容的即时变化，

随着表单内容的变化，即时将变化反馈到状态state中

即，随着输入维护状态就是非受控

可理解为**Vue的双向数据绑定**

```react
class Login extends React.Component {
	// 初始化状态
    state = {
        userName:'',
        passWord:''
    }

    render(){
        return(
          <form>
              姓名：<input type="text" name="userName"
                          onChange={this.getName}/>
                  <br/>
              密码：<input type="password" name="password"
                          onChange={this.getPassword}/>
                  <br/>

              姓名：<h3>{this.state.userName}</h3>
              密码：<h3>{this.state.passWord}</h3>
          </form>
        )
    }
	// 将数据存入状态
    getName = ()=>{
        this.setState({
            userName:event.target.value
        })
    }
  // 将数据存入状态
    getPassword = ()=>{
        this.setState({
            passWord:event.target.value
        })
    }
      
}

ReactDOM.render(
    <Login/>,
    document.getElementById('root')
)
```

通过 **高阶函数 + 函数柯里化** 简化

```react
class Login extends React.Component {
    state = {
      userName: "",
      passWord: "",
    };

    render() {
      return (
        <form>
          姓名：<input type="text" name="userName"
                    onChange={this.saveData("userName")}/>
          <br />
          密码：<input type="password" name="password"
                    onChange={this.saveData("passWord")} />
          <br />
          姓名：<h3>{this.state.userName}</h3>
          密码：<h3>{this.state.passWord}</h3>
        </form>
      );
    }
    saveData = (params) => {
      return () => {
        this.setState({
          [params]: event.target.value,
        });
      };
    };
    //   getName = ()=>{
    //       this.setState({
    //           userName:event.target.value
    //       })
    //   }
    //   getPassword = ()=>{
    //       this.setState({
    //           passWord:event.target.value
    //       })
    //   }
  }

  ReactDOM.render(
      <Login />, 
      document.getElementById("root")
 );
```





## 生命周期函数

组件中有一系列 生命周期回调函数（生命周期钩子函数）

React会在在特定时间点调用特定的生命周期函数

生命周期函数是通过类组件的实例对象调用



挂载（渲染到页面）mount

卸载 （从页面移除）unmount

---

### 旧 生命周期

![](https://pbs.twimg.com/media/E0uAjaJUYAAUPUH?format=jpg&name=medium)

---

#### 初始化 组件第一次挂载

由 `ReactDOM.render()` 触发初次渲染（挂载）

1. **constructor(){ } **

   初始化状态

2. **componentWillMount(){  }** 

   组件将要挂载时执行

3. **render(){  }** 

   组件挂载时执行 

4. **componentDidMount(){ }** 

   组件完成挂载完毕后调用

```react
class Demo extends React.Component {
  //构造器函数
  constructor(props){
    super(props)
    this.state = {key:value}
  }
  // 组件将要挂载时执行
  componentWillMount(){ }
  
  // 组件挂载完毕时执行
  componentDidMount(){ }
  
  // 组件挂载、状态更新时执行
  render(){
    return ()
  }
}
```

---

#### 组件自身更新状态

由组件内部的`this.setState()` 触发

1. 当前组件内部**setState()** 

   修改状态

2. **shouldComponentUpdate(){ }**

   组件是否应该被更新，控制组件更新的阀门

   若返回值是 **true**，继续执行下面的render步骤

   若返回值是 **false**，到此为止不执行render

   若是不写底层自动写上，并设定默认返回 true

3. **componentWillUpdate(){  }**

   组件将要更新时执行

4. **render(){  }**

   修改状态时执行

5. **componentDidUpdate(){  }**

   组件更新完毕时执行

```react
class Demo extends React.Component {
    constructor(props){
      super(props)
      this.state = {key:value}
    }
    componentWillMount(){ }
    componentDidMount(){ }
    
    
    fun01 = () => {
      // 修改状态
      this.setState({key:this.state.key???})
    }
                    
    // 是否修改组件 阀门
    shouldComponentUpdate(){
      return true
      // return false
    }
    
    // 组件将要更新时执行
    componentWillUpdate(){}
    
    // 组件将更新完毕执行
    componentDidUpdate(){ }
    
    // 挂载 修改状态
    render(){
      return ()
    }
      
    fun02 = () => {
    // 卸载组件
    ReactDOM.ummountComponentAtNode(doucment.getElementByID('root'))
  }
  
}
```

---

#### 父组件更新重新渲染

由父组件重新`render()`触发

1. 父组件修改状态 `setState()` 会导致重新渲染 **render()**

2. **componentWillReceuveProps(){  }** 

   子组件将要接受新的Props时执行

   *第一次调用到父组件传入的props时是不执行的*

   等后面父组件更新状态后再次传入新的props时才开始执行

3. **shouldComponentUpdate(){ }**

   子组件是否应该被更新

   控制组件更新的阀门

4. **componentWillUpdate(){  }**

   子组件将要更新时执行

5. 当前子组件**render(){  }**

   挂载子组件、更新子组件状态时执行

6. **componentDidUpdate(){  }**

   子组件更新完毕时执行

```react
// 父组件
class Father extends React.Component {
    state = {
        userName: "Andy",
    }
    changeName = ()=>{
        this.setState({
            userName:"Tom"
        })
    }
    render() {
      return (
        <div>
          <Son userName={this.state.userName}></Son>
          <button onClick={this.changeName}>切换</button>
        </div>
      );
    }
  }

//子组件
 class Son extends React.Component {
		// 子组件将要接受新的Props时执行
    componentWillReceiveProps(){
        console.log("B  componentWillReceiveProps");
    }
   // 子组件是否应该更新
    shouldComponentUpdate(){
        console.log("B  shouldComponentUpdate");
        return true
    }
   // 子组件将要更新时执行
    componentWillUpdate(){
        console.log("B  componentWillUpdate");
    }
   // 子组件完成更新
    componentDidUpdate(){
        console.log("B  componentDidUpdate");
    }
   // 子组件挂载
    render() {
      return(
        <div>
            {this.props.userName}
        </div>
      )
    }
 }

ReactDOM.render(
    <Father />, 
    document.getElementById("root")
);
```

---

#### 强制更新

不更改任何状态数据，只强制更新

比正常更新少了更新的阀门 shouldComponentUpdate

1. **forceUpdate()**

2. **componentWillUpdate(){  }**

   组件将要更新时执行

3. **render(){  }**

   修改状态时执行

4. **componentDidUpdate(){  }**

   组件更新完毕时执行

```react
class Demo extends React.Component {
    constructor(props){
      super(props)
      this.state = {key:value}
    }
    componentWillMount(){ }
    componentDidMount(){ }
    
    
    fun01 = () => {
      // 强制更新
      this.forceUpdate()
    }
                    
    // 是否修改组件 阀门
    shouldComponentUpdate(){
      return true
      // return false
    }
    
    // 组件将要更新时执行
    componentWillUpdate(){}
    
    // 组件将更新完毕执行
    componentDidUpdate(){ }
    
    // 挂载 修改状态
    render(){
      return ()
    }
      
    fun02 = () => {
    // 卸载组件
    ReactDOM.ummountComponentAtNode(doucment.getElementByID('root'))
  }
  
}
```

---

#### 卸载组件

 `ReactDOM.unmountComponentAtNode()` 触发

1. 组件的事件方法

2. **componentWillUnmount(){  }**

   组件将要卸载时执行

```react
class Demo extends React.Component {
  
  // 组件挂载、状态更新时执行
  render(){
    return ()
  }
  
  // 类方法回调函数
	fun = () => {
    // 卸载组件
    ReactDOM.ummountComponentAtNode(doucment.getElementByID('root'))
  }
  
}
```

---

### 3个常用生命周期

- **componentDidMount(){  }**

  页面一加载后就执行的处理

  一般写初始化的操作

  （**定时器、发送请求、订阅消息**）

- **render(){  }**

   组件必有的生命周期

-  **componentWillUnmonunt(){  }**

  一般写收尾的操作

  （**关闭定时器、取消订阅消息**）

```react
class Demo extends React.Component {
    state = {
      date: new Date()
    }

    componentDidMount() {
      this.timer = setInterval(() => {
        this.setState({
          date: new Date()
        });
      }, 1000);
    }

    componentWillUnmount(){
        //清除定时器
        clearInterval(this.timer)
    }
    
    close = ()=>{
        // 卸载组件
        ReactDOM.unmountComponentAtNode(document.getElementById('root'))
    }

    render(){
        return (
            <div>
                <h3>Now Time</h3>
                <span>{this.state.date.toTimeString()}</span>
                <button onClick={this.close}>close</button>
            </div>
        )
    }
  }

  ReactDOM.render(<Demo/>, document.getElementById("root"));
```

---

---

### 新 生命周期

React17之后

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy81Mjg3MjUzLTgyZjZhZjhlMGNjOTAxMmIucG5n?x-oss-process=image/format,png)

#### 弃用3个旧的生命周期

新的生命周期中依然可以使用旧生命周期

但是除了以下3个生命周期将过时

*ComponentWillMount*

*ComponentWillReceiveProps*

*ComponentWillUpdate*

所以改名，需要在前面加上 **UNSAFE_**

**UNSAFE_ComponentWillMount**

**UNSAFE_ComponentWillReceiveProps**

**UNSAFE_ComponentWillUpdate**

因为**不常用**，新版React中将删除上面的3个生命周期

---

#### 提出2个新的生命周期

提出这两个是static静态方法

几乎不会使用

- **getDerivedStateFromProps**

  组件的state完全取决于props

```react
class Demo extends React.Component {
        
    constructor(props){
        super(props)
        this.state = {}
        console.log("constructor");
    }

    static getDerivedStateFromProps(props){
        console.log("getDerivedStateFromProps");
        return props
    }

    render(){
        console.log("render");
        return (
            <div></div>
        )
    }
  }

ReactDOM.render(<Demo/>, document.getElementById("root"));
```

- **getSnapshotBeforeUpdate**

  获取快照

---

#### 初始化

1. constructor(){ }
2. getDerivedStateFromProps(){ }
3. render(){  }
4. componentDidMount(){  }

---

#### 更新状态

1. getDerivedStateFromProps(){ }
2. shouldComponentUpdate(){  }
3. render(){  }
4. getSnapshotBeforeUpdate(){  }
5. componentDidMount(){  }

---

#### 卸载组件

1. componentWillUnmount(){  }

---







## 组件间通信

### 父组件——>子组件

通过自定义属性传递给子组件数据

```react

```

在子组件中通过this.props接收传入的数据

```

```

---

### 子组件——>父组件

可通过一个函数

父组件传入一个函数，子组件的事件调用该函数，

并将子组件的数据作为函数的参数传入

```

```







## context