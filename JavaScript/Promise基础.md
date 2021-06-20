# Promise

<img src="https://miro.medium.com/max/688/1*UJHg3SkeZQ3VPzqyJJ1Yow.png" style="zoom:50%;" />

Promise是一个对象，是ES6提供的异步编程的一种解决方案，

把异步的嵌套变为线性，解决异步的回调地狱



## 异步回调地狱

若是直接回调嵌套，会容易出现回调地狱

比如，通过fs模块，依次读取A文件、B文件、C文件、D文件

```js
const fs = require('fs')

fs.readFile('./A.html', (err, res)=>{
  console.log(res)
    
	fs.readFile('./B.html', (err, res)=>{
  	console.log(res)
    
    fs.readFile('./C.html', (err, res)=>{
      console.log(res)
      
      fs.readFile('./D.html', (err, res)=>{
        console.log(res)
      })
    })
  })
})
```







## 基础使用步骤

1. 创建一个Promise实例对象，并包裹异步代码部分

2. 在异步中，通过Promise实例对象的回调函数的resolve参数和reject参数

   分别传出异步执行成功和失败的结果

3. 最后通过调用Promise实例的then方法和catch方法，分别获取传出的处理结果

> - **若异步执行成功**
>
>   通过resolve()传出执行的结果
>
>   通过 promise实例.then() 的参数 获取执行结果
>
> - **若异步执行失败**
>
>   通过reject()传出执行的结果
>
>   通过 promise实例.catch() 的参数 获取执行结果
>
>   允许通过链式编程，写在then()后面	

---

### 定时器模拟实例

如下：用定时器模拟调用异步的耗时，

并通过变量true/false模拟异步处理正常和错误的场合

```js
let promise = new Promise((resolve, reject)=>{
  
  setTimeout(()=>{
    let obj = {name:'Andy',age:28}
    
    if(true){
      resolve(obj)
    }else{
      reject('异步执行失败')
    }
    
  },3000)
})


promise.then((result)=>{
  console.log(result)
})
.catch((result)=>{
  console.log(result)
})
```

---

### Node.js异步API实例

比如，Node.js的fs模块读取文件的API

```js
const fs = require('fs')

let promise = new Promise((reslove, reject) => {
  
    fs.readFile('./A.js', 'utf8', (err, data) => {
        if (err) {
            reject(err.message)
        } else {
            reslove(data)
        }
    })
})

promise
    .then((result) => {
        console.log(result);
    })
    .catch((result) => {
        console.log(result);
    })
```







## 封装Promise实例函数

可以将创建Promise实例的代码封入一个函数

因为获取异步返回值是要调用Promise实例对象的方法 then() 和 catch() ，

所以，封装的函数要通过 **return返回值，返回创建的包裹了异步的Promise实例对象**

然后外部链式编程依次调用函数

- 调用封装的函数获取return的返回值（Promise实例对象）

  调用 then() 和 catch() 获取异步处理结果

- 并return返回后续函数的调用获取return返回值（Promise实例对象）

```js
function 第一个(){
  return new Promise((res,rej)=>{
    // 成功时
    res()
    // 失败时
    rej()
  })
}
function 第二个(){
  return new Promise((res,rej)=>{
    // 成功时
    res()
    // 失败时
    rej()
  })
}

第一个()
	.then(res=>{
  		// console.log(res)
  		retuen 第二个()
		})
  .then(res=>{
  		// console.log(res)
		})
```

如下：

```js
function fn() {
    return new Promise((resolve, reject) => {

        setTimeout(() => {
            let obj = { name: 'Andy', age: 28 }

            if (true) {
                resolve(obj)
            } else {
                reject('异步执行失败')
            }

        }, 3000)
    })
}


fn()
  .then((result) => {
    	console.log(result)
	})
  .catch((result) => {
  		console.log(result)
  })

```





## 使用场景

### Node.js

#### 多个异步API

如下：使用函数封装Promsie实例对象

```js
const fs = require('fs')

function p1() {
    return new Promise((reslove, reject) => {
        fs.readFile('./A.js', 'utf8', (err, data) => {
            if (err) {
                reject(err.message)
            } else {
                reslove(data)
            }
        })
    })

}

function p2() {
    return new Promise((reslove, reject) => {

        fs.readFile('./B.js', 'utf8', (err, data) => {
            if (err) {
                reject(err.message)
            } else {
                reslove(data)
            }
        })
    })
}

function p3() {
    return new Promise((reslove, reject) => {

        fs.readFile('./C.js', 'utf8', (err, data) => {
            if (err) {
                reject(err.message)
            } else {
                reslove(data)
            }
        })
    })
}


p1()
    .then(res1 => {
        console.log(res1);
        return p2()
    })
    .then(res2 => {
        console.log(res2);
        return p3()
    })
    .then(res3=> {
        console.log(res3);
    })
```



### 原生Ajax请求（xhr对象）

#### 一个Ajax请求

如下：封装一个函数，Ajax请求地址url作为参数传入

```js
function queryData(url) {
    return new Promise(function(resolve, reject) {
      
        var xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function() {
            if (xhr.readyState !== 4) return;
            if (xhr.readyState == 4 && xhr.status == 200) {
                resolve(xhr.responseText);
            } else {
                reject('出错了')
            }
        };
        xhr.open('get', url);
        xhr.send(null);
    })
};

queryData('http://localhost:3000/test')
  .then((data)=>{
  		console.log(data);
		}, (data)=>{
    console.log(data);
	})
```



#### 多个Ajax请求

处理多个结果依有赖顺序的Ajax请求

通过多个`.then` 链式操作处理多个异步任务

如下：按顺序依次打印三个请求地址的响应结果

```js
function queryData(url) {
    return promise = new Promise(function(resolve, reject) {
        var xhr = new XMLHttpRequest();

        xhr.onreadystatechange = function() {
            if (xhr.readyState !== 4) return;
            if (xhr.readyState == 4 && xhr.status == 200) {
                resolve(xhr.responseText);
            } else {
                reject('出错了')
            }
        };
        xhr.open('get', url);
        xhr.send(null);
    })
};

queryData('http://localhost:3000/test001')
  .then(res=>{
    console.log(data);     
    return queryData('http://localhost:3000/test002');
		})
  .then(res=>{
    console.log(data);
    return queryData('http://localhost:3000/test003')
		})
  .then(res=>{
    console.log(data);
})

// 第一个请求地址的结果
// 第二个请求地址的结果
// 第三个请求地址的结果
```







## then() 参数函数的返回值

### 返回一个 Promise实例对象

若then() 参数函数return返回了一个 Promise实例对象

则下一个then方法中调用的result参数，是return的该实例对象

如下：

多个链式并存时，then的调用者是前面then方法中return的Promise对象

```js
function queryData(url) {
    return promise = new Promise(function(resolve, reject) {
        var xhr = new XMLHttpRequest();

        xhr.onreadystatechange = function() {
            if (xhr.readyState !== 4) return;
            if (xhr.readyState == 4 && xhr.status == 200) {
                resolve(xhr.responseText);
            } else {
                reject('出错了')
            }
        };
        xhr.open('get', url);
        xhr.send(null);
    })
};



queryData('http://localhost:3000/test001')
  .then(res => {
        return queryData('http://localhost:3000/test002');
    })
  .then(res => {
  			// 返回一个自定义Promise实例对象
        return new Promise((resolve, reject) =>{
            setTimeout(function() {
                resolve(' new instance Promise object')
            }, 1000)
        })
  
    })
  .then(res => {
        console.log(res);     // new instance Promise object
    })
```





### 返回一个 普通值

若then() 参数函数return返回了一个 Promise实例对象

下一个then的参数中的函数的参数接受该普通值

如下：

```js
function queryData(url) {
    return promise = new Promise(function(resolve, reject) {
        var xhr = new XMLHttpRequest();

        xhr.onreadystatechange = function() {
            if (xhr.readyState !== 4) return;
            if (xhr.readyState == 4 && xhr.status == 200) {
                resolve(xhr.responseText);
            } else {
                reject('出错了')
            }
        };
        xhr.open('get', url);
        xhr.send(null);
    })
};

queryData('http://localhost:3000/test001')
    .then(function(data) {
        return queryData('http://localhost:3000/test002')
    }).then(function(data) {
  			// 传出一个普通值
        return '一个普通值'
  
    }).then(function(data) {
        console.log(data);    //hello just a data
    })
```













## 常用实例APIs

### 实例对象.then()

获取异步任务的正确结果（和错误信息）

两个函数作为参数，第一个获得正确结果，第二个获得错误结果



### 实例对象.catch()

获取异常信息

**作用等同于.then() 方法中的第二个参数**

就是Promise实例对象中reject()中的结果



### 实例对象.finally()

不论结果成功与否都会执行

用于输出提示性信息、或销毁资源

---

<img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg2018.cnblogs.com%2Fblog%2F1462173%2F201812%2F1462173-20181221192718414-1334509868.png&refer=http%3A%2F%2Fimg2018.cnblogs.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1620703986&t=220af39e7d46e4b163309ce68e77ee0a" style="zoom:50%;" />

```js
function queryData(){
  return promise = new Promise(function(resolve,reject){
    //XXXXXXXX
    resolve('right');
    reject('error');
  })
};

queryData()
  .then(function(data){
  console.log(data)       // right
	},
       function(data){
  console.log(data)       // error
	})
  .catch(function(data){
  console.log(data)       // error
	})
  .finally(function(){
  console.log('mission finished')    // mission finished
	})
```





## 常用对象APIs

### Promis.all()

并发处理多个异步任务

**所有的任务都完成后才能得到结果**

以数组的形式

```js
Promise.all([实例对象,实例对象,实例对象...]).then((result)=>{
  console.log(result)  //[p1返回的结果, p2返回的结果, p3返回的结果]
})
```



### Promise.race()

并发处理多个异步任务

**只要有一个任务完成就能得到结果**

```js
Promise.race([实例对象,实例对象,实例对象...]).then((result)=>{
  console.log(result)  //第一个返回结果
})
```

---

如下：

调用Ajax接口

```js
function queryData(url) {
    return promise = new Promise(function(resolve, reject) {
        var xhr = new XMLHttpRequest();

        xhr.onreadyStateChange = function() {
            if (xhr.readyState !== 4) return;
            if (xhr.readyState == 4 && shr.status == 200) {
                resolve(xhr.responeseText)
            } else {
                reject('有错误')
            }
        };
        xhr.open('get', url);
        xhr.send(null)
    })
};

var p1 = queryData('http://localhosot:3000/test001');
var p2 = queryData('http://localhosot:3000/test002');
var p3 = queryData('http://localhosot:3000/test003');

Promise.all([p1, p2, p3]).then(function(result) {
    console.log(result);     // [p1返回的结果, p2返回的结果, p3返回的结果]
})

Promise.race([p1, p2, p3]).then(function(result) {
    console.log(result);     // p1返回的结果
})
```

