# 各种发送Ajax请求的方式



## XHR

window上带的，最原始的Ajax

```js
const xhr = new XMLHttpRequest()
```

---

### JQuery

是对xhr的封装，简化了xhr的写法

但是容易出现**回调地狱**

---

### Axios

前端的Axios也是对xhr的封装

**Promise**风格，解决了回调地狱

请求返回的数据在response的data中

```js
axios('请求地址').then(
	response=>{console.log('获取数据成功了',response.data)},
  error=>{console.log('获取数据失败了了',error)}
)
```





## Fetch

**不借助xhr**

不是第三方库不需要下载，window内置原生函数

**Promise**风格，解决了回调地狱

但是老版本浏览器不支持

---

fetch的设计思想是 **关注分离**，

第一步是判断**链接服务器**的成功或失败，

第二步才是判断**数据请求**成功或失败

```js
fetch('请求地址').then(
	response=>{console.log('链接服务器成功了',response)},
  error=>{console.log('链接失败了了',error)}
)
```

请求返回的数据存放第一次 then()中的response.json()

通过return返回出一个promise实例

若链接服务器成功，则返回response.json()

若链接服务器成功，则返回一个新的空的Promise实例对象

然后再通过第二个then()获取数据

```js
fetch('请求地址').then(
	response=>{
    console.log('链接服务器成功了')
    return response.json()
  },
  error=>{
    console.log('链接失败了了')
    return new Promise(()=>{})
  }
  
).then(
  response=>{'获取数据成功了',response},
  error=>{'获取数据失败了',error}
)
```

上述代码有冗余，还可以简化

删去重复的error错误处理，在最后统一用catch处理错误

```js
fetch('请求地址').then(
	response=>{
    console.log('链接服务器成功了')
    return response.json()
  }
  
).then(
  response=>{'获取数据成功了',response}
  
).catch(
	error=>{console.log('请求出错',error)}
)
```

还可继续简化

```js
try{
  const response = await fetch('请求地址')
  const data = await response.json()
  conso.log(data)
  
}catch(error){
  console.log('请求出错',error)
}
```

