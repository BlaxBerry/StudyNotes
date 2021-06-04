# Redux

![](https://redux.js.org/img/redux-logo-landscape.png)

## 简介

是个专门做 **状态管理** 的JS库，

不是React的插件库，基本上与React配合使用

可用在React、Vue、Angular项目（vue常用**VueX**）

**集中式状态管理**React应用中多个组件共享的状态





## Redux优势

共享：一个组件的状态state可以被其他组件随时获取

通信：一个组件改变另一个组件的状态state

目前的方法如下：

**1.  逐层传递**

​	逐层传递给App主组件，

​	然后主组件在逐层逐个传递给需要的组件

​	太麻烦

**2.  消息订阅发布**

​	C组件消息发布，

​	然后其他需要的组件都进行消息订阅

​	也比较麻烦

**3.  Redux**

​	Redux独立于所有的组件

​	需要共享的数据都存在Redux不再存放在组件自身

​	不需要共享的数据还存放于组件自己的状态中

​	但是Redux能不用就不用，比较复杂

​	如果组件嵌套很深很复杂的话再考虑Redux

​	



## 原理图

![](https://pbs.twimg.com/media/E2ysTjGUYAglrhI?format=jpg&name=medium)