# Vue前后端交互

## 前后端交互模式

### 接口调用方式

- **原生Ajax**

- **jQuery的Ajax**

- **fetch**

- **axios**



传统Ajax

```js
$.ajax({
  url:'http://localhost:3000/data',
  success:function(data){
    console.log(data)
  }
})
```