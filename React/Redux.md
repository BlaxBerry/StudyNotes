# Redux

![](https://redux.js.org/img/redux-logo-landscape.png)

## 简介

是个专门做 **状态管理** 的JS库，

不是React的插件库，基本上与React配合使用

可用在React、Vue、Angular项目（vue常用**VueX**）

**集中式状态管理**React应用中多个组件共享的状态

---

### Redux的优势

共享：一个组件的状态state可以被其他组件随时获取

通信：一个组件改变另一个组件的状态state

目前组件间的通信方法如下：

**1.  逐层传递**

​	逐层传递递给需要的组件

​	太麻烦

**2.  消息订阅发布**

​	组件消息发布，

​	然后其他需要的组件都进行消息订阅

​	也比较麻烦

**3.  Redux**

​	Redux独立于所有的组件

​	需要共享的数据都存到Redux不再存放在组件自身

​	不需要共享的数据还存放于组件自己的状态中

​	如果组件嵌套很深很复杂，或**大量组件共享状态**，考虑使用Redux

​	一般简单的组件间数据通信不需要Redux







## Redux安装

```bash
yarn add redux
```



​	





## Redux原理图（重要）

![](https://pbs.twimg.com/media/E2ysTjGUYAglrhI?format=jpg&name=medium)

组件将需要被Redux保存管理的状态，

1. 通过ActionCreators函数生成action（包含type和action属性的对象）
2. 通过store的API dispath传递action给store

3. store调用各个reducer函数，并将previousState和action传入
4. reducer根据action的type分别处理data，并将处理结果返回store
5. 组件最后通过store的API getState 获取状态
6. 若想同时渲染到页面，需要通过store的API subscribe监控Reudx中状态的改变

- 要在各个使用了Redux的组件的componentDidMount生命周期中通过store的API subscribe调用setState

- 或直接在index.js中导入store，通过store的API subscribe调用reactDOM.render







## 核心概念

### action 

动作对象，实际上是JS对象，包含两个属性：

- type：表示属性，字符串类型，必须
- data：数据属性，任意类型，可选

```js
{
	type: 'ADD_STUDENT',
  data: {
    name: 'Andy',
    age: 28
  }
}
```

### reducer

实际上是个函数，接收perviousState和action两个参数，用于初始化状态和加工状态

根据传入的action的type的不同分别处理data（swich语句）

旧的state初始值是undefined，在匹配不上时需要进行初始化state

加工时，根据旧的state和action加工产生新的state的纯函数

### store

将state、action、reducer联系到一起

接收action并传递给reducer函数们

接收reducer们处理的结果，并在组件需要时返回组件











## 循序渐进的Redux实例

假设一个求相加值组件

### 不实用Redux时

原始React的做法是将在组件内部操作state状态：

```jsx
import React, { Component } from 'react'


export default class App extends Component {
    state = { count: 0 }

    increment = () => {
        const { value } = this.selectVal;
        const { count } = this.state;
        this.setState({ count: count + value * 1 })
    }
    decrement = () => {
        const { value } = this.selectVal;
        const { count } = this.state;
        this.setState({ count: count - value * 1 })
    }

    incremenIfOdd = () => {
        const { value } = this.selectVal;
        const { count } = this.state;
        if (count % 2 !== 0) {
          this.setState({ count: count + value * 1 })
        }
    }

    incremenAsync = () => {
        const { value } = this.selectVal;
        const { count } = this.state;
        setTimeout(() => {
          this.setState({ count: count + value * 1 })
        }, 1000)
    }

    render() {
        return (
            <div>
                <h1 className="result">sum is :{store.getState()} </h1>
                <div>
                    <select ref={c => this.selectVal = c}>
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4">4</option>
                    </select>
                    <button onClick={this.increment}>+</button>
                    <button onClick={this.decrement}>-</button>
                    <button onClick={this.incremenIfOdd}>increment if sum is odd</button>
                    <button onClick={this.incremenAsync}>increment async</button>
                </div>
            </div>
        )
    }
}
```

使用Redux保存和操作状态:

### 实例1 - 略过Action Creator

不通过Action Creator创建action对象，手动生成action对象

#### 目录结构：

```js
src
  |-components
	|-redux		
		|- store.js
		|- reducer.js
	|-App.jsx
	|-index.js
```

#### store.js

```js
// 引入createStore，创建Redux的store对象
import { createStore } from 'redux'

// 引入reducer
import reducer from './app_reducer'

// 暴露store对象
export default createStore(reducer)
```

#### reducer.js

```js
// 组件的初始化状态
const initState = 0;

// 创建一个reducer函数
export default function caculateReducer(preState = initState, action) {

    // 从action对象中获取type和data
    const { type, data } = action;

    // 根据type决定如何加工数据
    switch (type) {
        case 'increment':
            return preState + data;
        case 'decrement':
            return preState - data;
        default:
            return preState;
    }
}
```

#### component组件

```jsx
import React, { Component } from 'react'

// 引入 store
import store from './redux/store.js'

export default class App extends Component {
    // state = { count: 0 }

    /*componentDidMount() {
        // 只要redux中保存的状态发生改变就调用该回调渲染页面数据
        store.subscribe(() => {
            // console.log('state in Redux is changed');
            this.setState({})
        })
    }*/

    increment = () => {
        const { value } = this.selectVal;
        //     const { count } = this.state;
        //     this.setState({ count: count + value * 1 })
        store.dispatch({
            type: 'increment',
            data: Number(value)
        })
    }
    decrement = () => {
        const { value } = this.selectVal;
        //     const { count } = this.state;
        //     this.setState({ count: count - value * 1 })
        store.dispatch({
            type: 'decrement',
            data: Number(value)
        })
    }

    incremenIfOdd = () => {
        //     const { value } = this.selectVal;
        //     const { count } = this.state;
        //     if (count % 2 !== 0) {
        //         this.setState({ count: count + value * 1 })
        //     }
        const count = store.getState();
        if (count % 2 !== 0) {
            this.increment()
        }
    }

    incremenAsync = () => {
        const { value } = this.selectVal;
        //     const { count } = this.state;
        //     setTimeout(() => {
        //         this.setState({ count: count + value * 1 })
        //     }, 1000)
        setTimeout(() => {
            store.dispatch({
                type: 'increment',
                data: Number(value)
            })
        }, 1000)
    }

    render() {
        return (
            <div>
                {/* <h1 className="result">sum is : {this.state.count}</h1> */}
                <h1 className="result">sum is :{store.getState()} </h1>
                <div>
                    <select ref={c => this.selectVal = c}>
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4">4</option>
                    </select>
                    <button onClick={this.increment}>+</button>
                    <button onClick={this.decrement}>-</button>
                    <button onClick={this.incremenIfOdd}>increment if sum is odd</button>
                    <button onClick={this.incremenAsync}>increment async</button>
                </div>
            </div>
        )
    }
}
```

#### index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

// 引入store.js
import store from './redux/store.js'

ReactDOM.render(
    <App />,
    document.getElementById('root')
);

// 只要redux中的状态改变就渲染
store.subscribe(() => {
    ReactDOM.render(
        <App />,
        document.getElementById('root')
    );
})
```

---

### 实例2 - 使用Action Creator

#### 目录结构：

```js
src
  |-components
	|-redux	
		|- actionCreator.js
		|- store.js
		|- reducer.js
	|-App.jsx
	|-index.js
```

#### actionCreator.js

```js
// 专门生成action对象,并分别暴露
export function createActionIncrement(data) {
    return {
        type: 'increment',
        data: data
    }
}
export function createActionDecrement(data) {
    return {
        type: 'decrement',
        data: data
    }
}
```

#### component组件

```jsx
import React, { Component } from 'react'

// 引入 store
import store from './redux/store.js'
// 引入 action creator
import { createActionIncrement, createActionDecrement } from './redux/app_actionCreator.js'

export default class App extends Component {

    increment = () => {
        const { value } = this.selectVal;
        store.dispatch(createActionIncrement(Number(value)))
    }
    decrement = () => {
        const { value } = this.selectVal;
        store.dispatch(createActionDecrement(Number(value)))
    }

    incremenIfOdd = () => {
        const count = store.getState();
        if (count % 2 !== 0) {
            this.increment()
        }
    }

    incremenAsync = () => {
        const { value } = this.selectVal;
        setTimeout(() => {
            store.dispatch(createActionIncrement(Number(value)))
        }, 1000)
    }

    render() {
        return (
            <div>
                <h1 className="result">sum is :{store.getState()} </h1>
                <div>
                    <select ref={c => this.selectVal = c}>
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4">4</option>
                    </select>
                    <button onClick={this.increment}>+</button>
                    <button onClick={this.decrement}>-</button>
                    <button onClick={this.incremenIfOdd}>increment if sum is odd</button>
                    <button onClick={this.incremenAsync}>increment async</button>
                </div>
            </div>
        )
    }
}
```

---

### 实例3 - 使用常量模块保存type

创建一个装门存放type的JS文件，便于方便管理和防止拼写错误

#### 目录结构：

```js
src
  |-components
	|-redux	
		|- constant.js
		|- actionCreator.js
		|- store.js
		|- reducer.js
	|-App.jsx
	|-index.js
```

#### constant.js

```js
// 专门用来保存type
export const INCREMENT = 'increment';
export const DECREMENT = 'decrement'
```

#### actionCreator.js

```js
// 引入保存type的文件
import { INCREMENT, DECREMENT } from './constant';


export function createActionIncrement(data) {
    return {
        type: INCREMENT,
        data: data
    }
}
export function createActionDecrement(data) {
    return {
        type: DECREMENT,
        data: data
    }
}
```

#### reducer.js

```js
// 引入保存type的文件
import { INCREMENT, DECREMENT } from './constant';


const initState = 0;
export default function caculateeducer(preState = initState, action) {

    const { type, data } = action;

    switch (type) {
        case INCREMENT:
            return preState + data;
        case DECREMENT:
            return preState - data;
        default:
            return preState;
    }
}
```

---





## 同步action、异步action

传递的action除了对象形式还可是个函数

**对象**形式的action被称为：**同步action**

**函数**形式的action被称为：**异步action**



但是因为store认可的action必须是个对象，所以异步action需要通过一个中间件 

> Error: Actions must be plain objects. Instead, the actual type was: 'function'. You may need to add middleware to your store setup to handle dispatching other values, such as 'redux-thunk' to handle dispatching functions.

### redux-thunk

```bash
yarn add redux-thunk
```

- actionCreator.js

```js
import { INCREMENT, DECREMENT } from './constant';

import store from './store';


// 同步action，对象形式
export function createActionIncrement(data) {
    return {
        type: INCREMENT,
        data: data
    }
}

// 异步action，函数形式
export function createActionIncrementAsync(data, time) {
    return () => {
        setTimeout(() => {
            store.dispatch(createActionIncrement(data))
        }, time)
    }
}
```

- store.js

```js
// 引入createStore，applyMiddleware创建Redux的store对象
import { createStore, applyMiddleware } from 'redux'

// 引入reducer
import reducer from './app_reducer'

// 引入redux-thunk
import thunk from 'redux-thunk'

// 暴露store对象,并调用thunk
export default createStore(reducer,applyMiddleware(thunk))
```



异步action并不是必须的，

可以在组件中通过定时器实现异步创建action并发送store

```js
incremenAsync = () => {
  const { value } = this.selectVal;
  setTimeout(() => {
    store.dispatch(createActionIncrement(Number(value)))
  }, 1000)
}
```







## 多个组件共享数据

```js
src
|-redux
	|-store.js
	|-constant.js
	|-actions
		|-componentA.js
		|-componentB.js
	|-reducers
		|-componentA.js
		|-componentB.js
```







# react-redux

react是facebook开发的，redux是其他团队开发的

但看到redux的需求很高，react就自己开发了一个插件react-redux，可以更方便在react中使用rudux

​	<img src="https://pbs.twimg.com/media/E5luiTyVEAAFJ3T?format=jpg&name=medium" style="zoom:50%;" />



未完 103



## 