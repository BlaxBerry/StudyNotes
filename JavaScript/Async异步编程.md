# 异步编程

<img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fdevbbs-discuzx.stor.sinaapp.com%2Fdata%2Fattachment%2Fforum%2F201212%2F24%2F1633169qu7icgwspbqwgz7.png&refer=http%3A%2F%2Fdevbbs-discuzx.stor.sinaapp.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1617117429&t=0f732dca3aba5c57cc5e861972ffa0e9" style="zoom:50%;" />

## 异步场景

- 定时器

- 事件函数

- Ajax



## 调用顺序

异步调用的结果顺序并不是书写顺序

比如，Ajxa调用多个后台接口

结果显示顺序取决于调用端口后的耗时长短

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



## 回调地域

若多个异步调用的结果存在依赖关系，就需要嵌套

但是如果嵌套层数过多，就是**回调地狱**

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





## 异步

如下：

函数内部有一个异步定时器

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

上述，

因为定时器是异步，所以该线程内fn函数里什么操作程序都没有，

变量num也是不存在与该线程的，

所以return返回不了，报错

---

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

上述

因为定时器是异步，所以该线程内fn函数里什么操作程序都没有，

直再加上也没有retrun返回值就结束了，

所以函数执行结果是undefined

所以在外部输出调用函数的结果为undefined









## 异步回调

在函数外部获取一个函数内部的异步中的数据

需要把异步内的数据传出去

通过**在异步内调用一个回调函数**，

把要传出的数据作为回调函数的**参数传入**

等异步任务结束就会执行该回调函数

*（就相当于在异步里执行了外部的调用）*



在函数外调用函数时，在实参处声明一个**回调函数传入**函数

函数接受这个回调函数，

函数内的异步也可以使用这个回调函数

等异步内执行结束时调用这个回调函数，并传参

在函数外定义回调函数时，在里面就可应用异步的数据了



如下，

等函数内的异步结束后执行回调函数

1秒钟后打印出num的值

```js
function fn (callback) {

    setTimeout(function() {
        let num = 100;;
        callback(num);
    }, 1000);
};

fn(function(aa) {
    console.log(aa)
});
```

上述，

`fn函数`外部调用函数时，传入一个`回调函数`；

函数内的定时器结束时调用回调函数，并把`num`传入回调函数；

回调函数中就可以`console.log()`获得函数的异步中定义的`num`了





## Promise

promise是一个对象，是异步编程的一种解决方案，用于解决回调地狱

把异步的嵌套变为线性

### 基础使用步骤

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
  function(ret){},
  //reject的结果
  function(ret){}
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





### Promise处理原生Ajax请求

#### Promise处理一个Ajax请求

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

---

#### Promise处理多次Ajax请求（回调地狱）

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





### then方法参数中函数的返回值

#### 返回一个**Promise实例对象**

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

---

#### 返回一个**普通值**

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





### 常用实例APIs

#### 实例对象.then()

获取异步任务的正确结果（和错误信息）

两个函数作为参数，第一个获得正确结果，第二个获得错误结果

---

#### 实例对象.catch()

获取异常信息

**作用等同于.then() 方法中的第二个参数**

就是Promise实例对象中reject()中的结果

---

#### 实例对象.finally()

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





### 常用对象APIs

#### Promis.all()

并发处理多个异步任务

**所有的任务都完成后才能得到结果**

以数组的形式

```js
Promise.all([实例对象,实例对象,实例对象...]).then((result)=>{
  console.log(result)  //[p1返回的结果, p2返回的结果, p3返回的结果]
})
```

---

#### Promise.race()

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

