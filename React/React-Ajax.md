# React - Ajax

React只关注界面，并不包含Ajax请求的代码

若想通过Ajax获取后端的数据

- 可以自己封装Ajax（不建议）

- 通过第三方的Ajax库
  - jQuery（不推荐）
  - **Axios**



## 安装Axios

```bash
yarn add axios
```

## 引入Axios

```
import axios from 'axios'
```

## 使用Axios

```react
axios.get('').then(
  response=>{
    console.log(response.data)
  },
  error=>{
    console.log('error')
  }
)
```





## 跨域问题解决

Access-Control-Allow-Origin

请求可以发送，但是数据回不来

通过代理解决跨域问题

将请求通过代理转发给服务器

假设：

- 返回数据的服务器地址是localhost:5000,

- 客户端域名是localhost:3000

- 则代理写在客户端的相同域名上，localhost:3000
- 代理中的地址写服务器地址localhost:5000

<img src="https://pbs.twimg.com/media/E2dy86GUcAELDYd?format=jpg&name=medium" style="zoom:33%;" />

### 配置代理（一）

通过 **poxy** 配置代理

在项目的 **`package.json`** 中追加请求地址

```json
{
  
  "poxy":"http://localhost:5000"
}
```

```react
axios.get('http://localhost:3000/api1').then(res=>{
  console.log(res)
})
```

- 添加请求时不需要前缀

- 但是不能配置多个代理

- 会优先配置前端资源：

  若请求的地址在客户端自己域名下有（项目的publi目录中）

  则直接请求了自己的3000，

  若请求的地址自己没有则再去向代理配置的地址请求5000



### 配置代理（二）

1. 第一步：配置代理文件

   在 src 目录下新建 **setupProxy.js**

2. 第二步：编写配置规则

   用 common JS 方式引入

```js
const proxy = require('http-poxy-middleware')

module.exports = function(app){
  app.use(
    //设置区分用的请求地址前缀，所有带有/api1前缀的请求都转发给5000
  	proxy('/api1',{ 
      //配置代理转发的目标地址，即能返回数据的服务器地址
      target:'http://localhost:5000',
      //控制服务器收到的请求头host中的值
      // true: 返回目标地址，即服务器地址
      // false: 返回客户端地址
      changeOrigin: true,
      // 去除请求的地址前缀，保证服务器收到正确的请求地址
      pathRewrite:{'^/api1':''}
    }),
    // 可以设置多个代理
    proxy('/api2',{
      target:'http://localhost:5001',
      changeOrigin: true,
      pathRewrite:{'^/api2':''}
    }),
  )
}
```

```react
axios.get('http://localhost:3000/api1/data').then(res=>{
  console.log(res)
})

axios.get('http://localhost:3000/api1/data').then(res=>{
  console.log(res)
})
```

- 可以配置多个代理，向不同服务器地址请求数据
- 可以控制请求是否走代理
- 前端发送请求时地址需要加上前缀