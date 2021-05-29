# React 路由

## 相关简介

### SPA

单页面Web应用（**S**ingle **P**age **W**eb **A**pplication）

整个应用只有一个完整的页面

**点击页面中的链接不会刷新页面，只会做页面的局部更新**

数据通过Ajax请求获取，并在前端异步展现



### 路由含义

一个路由就是一个映射关系

每一个路径对应一个组件

key是路径，value是可能是function或component



### 路由分类

**后端路由：**

​	value是函数，根据请求地址找到匹配的路径，调用路由中的函数俩处理请求，返回响应数据

```js
app.get('/search',function(req,res){
  axios({
    url:'https://api.github.com/search/users',
    params:{req.query}
  }).then(res=>{
    res.json(res.data)
  })
})
```

**前端路由：**

​	value是组件，当浏览器的path变为指定地址时，当前路由组件就变为相匹配的组件

