# 异步编程（Async）

![](https://gblobscdn.gitbook.com/assets%2F-LA-UVvJIdbk5Kfk3bDs%2F-M4zU72YWwW2hSyXI5LY%2F-M4zVQZMwwMDTDL3Tzd9%2Fasync-programming.png?alt=media&token=7dfdb955-c7bb-4765-a137-89a099e5f411)

## 同步与异步

### 同步API

只有当前API执行完成后才能执行下一个API

```js
console.log('A')
console.log('B')
console.log('C')
/* 
A
B
C
*/
```

从上到下依次执行，会产生阻塞

```js
for(var i = 0; i < 100000000; i++){
  console.log(i)
}

console.log('完成')
```

---

### 异步API

异步常见于：

- 定时器

- 事件函数

- Ajax请求

当前API的执行不会阻塞后续代码的执行

```js
console.log('A')

setTimeout(function(){
  console.log('B')
}, 3000)

setTimeout(function(){
  console.log('c')
}, 1000)

console.log('D')

/* 
A
D
C
B
*/
```

---

### 共存时的代码执行顺序

1. 先执行同步任务

2. 执行时将遇到的异步任务放入**异步执行区**，不执行

3. 等同步都执行完后，在异步执行区值查找的异步callback回调函数

4. 然后将异步函数依次输出在，**同步执行区**中执行完成的同步函数后面

---

### 多个异步的调用顺序

异步调用的结果顺序并不是书写顺序

比如，Ajxa调用多个后台接口

结果显示顺序取决于各个异步的耗时 (调用端口后的耗时长短)

```js
$.ajax({
  url:"http://loaclhost:3000/data001",
  success:function(data){
    console.log(data)
  }
});

$.ajax({
  url:"http://loaclhost:3000/data002",
  success:function(data){
    console.log(data)
  }
});

$.ajax({
  url:"http://loaclhost:3000/data003",
  success:function(data){
    console.log(data)
  }
})
```







## 异步API的返回值

异步API的返回值不能在线程上获取，会报错

### 报错实例 1

```js
function fn() {

    setTimeout(function() {
        let num = 100;;
    }, 1000);

    return num;
};

console.log(fn());

// 报错
// ReferenceError: num is not defined
```

变量num的定义是异步

调用函数时，该线程内的函数内部num还没有被定义

所以函数执行会报错变量**没被定义**



### 报错实例 2

```js
function fn() {

    setTimeout(function() {
        let num = 100;;
        return num;
    }, 1000);
};

console.log(fn());

// undefined
```

因为定时器是异步，

调用函数时，该线程内fn函数里什么操作逻辑都没有，

再加上也没有 retrun返回值，

所以函数执行结果是 **undefined**





## 获取异步的返回值

综上，异步API的返回值不能在线程上获取，会报错

异步API的执行结果的返回值应该通过**回调函数**获取

### 回调函数

```js
// 定义函数，回调函数做参数
function fn(callback){
  // 执行回调
  callback()
}

// 调用函数，并传入回调函数
fn( ()=>{} )
```



### 实例1 获取异步中定义的变量

```js
function fn (callback) {

    setTimeout(function() {
        let num = 100;
        callback(num);
    }, 1000);
};

fn(function(num) {
    console.log(num)
});
```

上述，

调用函数时，传入并执行回调函数，

异步中调用回调函数，并把定义的变量 num 作为形参传入回调函数，

函数外部调用函数时，就通过回调函的实参获取异步中的传入的变量



### 实例2 Node.js 中的异步API

Node.js中的异步API，执行结果都是通过参数获取

比如，服务器事件监听

```js
const app =. http.createServer()
app.on('request', (req, res)=>{
  
})
```

比如，fs模块

```js
fs.readFile('./index.html', (err, res)=>{
  console.log(res)
})
```









## 回调地域（callback hell）

若多个异步调用的结果存在依赖关系，就需要嵌套

会导致嵌套层数过多，就是**回调地狱**

会导致维护困难

比如，依次读取A文件、B文件、C文件、D文件

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

比如：Ajax请求

```js
$.ajax({
    url: 'http://localhost:3000/first',
    success: function(data) {
        console.log(data)
        $.ajax({
            url: 'http://localhost:3000/test001',
            success: function(data) {
                console.log(data)
                $.ajax({
                    url: 'http://localhost:3000/test002',
                    success: function(data) {
                        console.log(data)
                        $.ajax({
                            url: 'http://localhost:3000/test003',
                            success: function(data) {
                                console.log(data)
                            }
                        });
                    }
                });
            }
        });
    }
});
```







## 解决回调地狱

### 1. Promise

ES6提供的解决回调地狱的方法

创建Promise实例对象，将异步函数写在Promis对象内部

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

- 若异步执行成功

  通过resolve()传出执行的结果

  通过 promise实例.then() 的参数 获取执行结果

- 若异步执行失败

  通过reject()传出执行的结果

  通过 promise实例.catch() 的参数 获取执行结果

  允许通过链式编程，写在then()后面		

#### Node.js中的Promise

如下：

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

1. 原本是需要嵌套调用三次 fs.readFile()

   因为是异步API，嵌套会有回调地狱，

   可使用promise

2. 将异步函数外包裹一个promise实例对象，

   并将promise实例对象放入一个函数

   每个函数return 返回的是创建的promise实例对象

3. 异步执行成功时，通过promise实例对象的resolve参数传出结果

   异步执行失败时，通过promise实例对象的reject参数传出结果

4. 调用函数（第一个Promise实例对象）

   通过then()方法获取异步执行的结果，并return返回下一个函数（Promise实例对象）

   链式编程依次调用函数



### 2. async异步函数

> Promise还是比较繁琐
>
> - 需要手动给每个异步API包裹上Promise对象
>
> - 还要手动调用resolve和reject传递出异步执行结果
>
> - 外部的链式比较臃肿

ES7提供的异步函数调用的终极解决方案

将代码写为同步的形式，清晰明了

- 在普通函数前加上**async**就成为了异步函数

```js
const fn = async ()=>{
}

async function fn(){
}
```

- 默认返回值Promise对象，

  不再是一般函数的undefined

```js
async function fn (){
  
}
console.log(fn())
// Promise{ undefined }
```

​		若定义return返回值,

​		则返回值为包裹了Promise对象的值

```js
async function fn (){
  return 12345
}
console.log(fn())
// Promise{ 12345 }
```

- 异步函数内部使用 return关键字 返回结果

  代替Promise的resolve

- 异步函数内部使用 throw关键字 抛出异常

  代替Promise的resolve

- 使用then() 获取异步执行结果

- 使用catch() 获取执行的错误信息

- await关键字 只能写在异步函数内部

  await关键字 后跟Promise对象

  可暂停当前函数执行，

  若后面Promises没有返回结果就不继续向下执行

```js
async function p1(){
  return 'p1'
}

async function p1(){
  return 'p2'
}

async function p1(){
  return 'p2'
}

async function run(){
  let res1 = await p1()
  let res2 = await p2()
  let res3 = await p3()
  console.log(res1)
  console.log(res2)
  console.log(res3)
}

```

#### util内置模块

```js
const promisify =  require('util').promisify
```

包装Node.js的API，使返回值为Promise对象

然后才能支持异步函数的语法

```js
const promisify =  require('util').promisify
const fs = require('fs')
const readFile = promisify(fs.readFile)

async function run (){
  let res1 = await readFile('./A.html', 'utf8')
  let res1 = await readFile('./B.html', 'utf8')
  let res1 = await readFile('./C.html', 'utf8')
  console.log(res1)
  console.log(res2)
  console.log(res3)
}

run();
```



