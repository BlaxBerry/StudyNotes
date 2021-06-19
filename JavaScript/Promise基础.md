# Promise

<img src="https://miro.medium.com/max/688/1*UJHg3SkeZQ3VPzqyJJ1Yow.png" style="zoom:50%;" />

promise是一个对象，是异步编程的一种解决方案，用于解决回调地狱

把异步的嵌套变为线性



## 基础使用步骤

1. 实例化Promsie对象

2. 给构造函数的参数传递一个函数，在该函数中处理异步任务

3. 函数体内通过s`resolve()` 和 `reject()` 分别处理异步成功和失败的情况

4. 最后通过 `Promise实例.then()` 获取处理的结果

```js
var promise = new Rromise(function(resolve,reject){
  //成功时调用resolve
  resolve();
  //失败时调用reject
  reject();
  
});

promise.then(
  //resolve的正常结果
  function(res){},
  //reject的结果
  function(rej){}
)
```

如下：

用定时器模拟调用异步的耗时

并通过变量flag模拟异步处理正常和错误的场合

```js
var promise = new Promise(function(resolve, reject) {

    setTimeout(function() {
        var flag = true;
        if (flag) {
            resolve('正常')
        } else {
            reject('错误')
        }
    }, 1000)
})

promise.then(function(a) {
    console.log(a);
}, function(b) {
    console.log(b);
})
```

---

可以封入一个函数

```js
function 函数名(){
  return promise = new Promise(function(resolve,reject){
    // 处理成功时：
    resolve(数据);
    // 处理错误时：
    reject("出错了");
  })
};
函数名().then(function(data){
  console.log(data) // 获得处理成功时的结果
})
```





## Promise处理原生Ajax请求

### Promise处理一个Ajax请求

如下：

封装一个函数，Ajax请求地址url作为参数传入

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

queryData('http://localhost:3000/test').then(function(data) {
    console.log(data);
}, function(data) {
    console.log(data);
})
```



### Promise处理多次Ajax请求（回调地狱）

处理多个结果依有赖顺序的Ajax请求

通过多个`.then` 链式操作处理多个异步任务

then方法参数中的函数的return返回值是上一个Promise对象处理的结果

下一个then的调用者是上一个新的Promise对象

如下:

按顺序依次打印三个请求地址的响应结果

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

queryData('http://localhost:3000/test001').then(function(data) {
    console.log(data);     
    return queryData('http://localhost:3000/test002');
}).then(function(data) {
    console.log(data);
    return queryData('http://localhost:3000/test003')
}).then(function(data) {
    console.log(data);
})

// 第一个请求地址的结果
// 第二个请求地址的结果
// 第三个请求地址的结果
```

用定时器模拟响应的耗时，如下：

结果还是依次打印三个结果并且等待耗时

```js
//服务器端的路由地址

app.use('/test001', (req, res) => {
    res.send('hello Promise test 01')
})

app.use('/test002', (req, res) => {
    setTimeout(function() {
        res.send('hello Promise test 02')
    }, 1000)
})

app.use('/test003', (req, res) => {
    setTimeout(function() {
        res.send('hello Promise test 03')
    }, 1000)
})
```





## then方法参数中函数的返回值

### 返回一个 Promise实例对象

该实例对象会调用下一个then方法，

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
    .then(function(data) {
        return queryData('http://localhost:3000/test002');
    }).then(function(data) {
        return new Promise(function(resolve, reject) {
            setTimeout(function() {
                resolve(' new instance Promise object')
            }, 1000)
        })
    }).then(function(data) {
        console.log(data);     // new instance Promise object
    })
```



### 返回一个 普通值

该普通值会直接传递给下一个then，

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
        return 'hello just a data'
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

