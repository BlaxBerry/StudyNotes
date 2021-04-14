# async/await

![](https://d23cpcfk0ihnyh.cloudfront.net/blog/posts/1-20-2018-async-await/async-await.png)

ES7引入的新语法，用于更方便处理异步任务



- async关键字用于声明函数时

- await关键字用于async函数中，用来得到异步任务的结果

  

await关键字后可跟一个Promise实例对象

async函数的返回值是一个Promise实例对象



## 基础使用

```js
async function queryData(){
  var result = await new Promise(function(resolve,reject){
    rsolve('正确');
  });
  return result;
};

queryData().then(function(result){
  console.log(result)
})
```



```js
async function queryData(){
  var result = await axios.get('URL');
  return result.data;
};

queryData().then(function(result){
  console.log(result)
})
```





## 处理多个异步请求

```js
async function queryData(){
  var info = await axios.get('URL');
  var result = await axios.get(`URL?info=${info.data}`);
  return result
};

queryData().then(function(result){
  console.log(result)
})
```

若多个异步请求任务的结果又前后顺序，

以前是通过Promise语法把回调地狱的多层嵌套转为线形任务，

`Promise实例对象.then()` 链式使用，

后一个then的调用者是，前一个then方法中return返回的Promise实验对象

```js
//以前的Promise
function queryData(url){
  return promise = new Promise(function(resolve,reject){
    var xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function(){
      if(xhr.readyState !==4} return;
      if(xhr.readyState ==4 && xhr.status == 200}{
        resolve(xhr.responseText);
      } else {
        reject(xhr.responseText);
      };
    };
    xhr.open('get',url);
    xhr.send(null);
  });
};

queryData('URL01').then(function(result){
  return queryData('URL02');
}).then(function(result){
  return queryData('URL03');
}).then(function(result){
  console.log(result)
})
```

而async/await关键字是封装的函数中实现线形任务，

然后return返回最后的请求的Promise实例对象

最后通过调用函数，用返回的Promise实例对象调用then，

即可得到又依赖顺序的多个异步请求任务的最终结果

```js
axios.default.baseURL = 'http://localhost:3000'
async function queryData() {
    var info1 = await axios.get('test001');
    var info2 = await axios.get(`test002?info1=${info1.data}`);
    var result = await axios.get(`test003?info2=${info2.data}`);
    return result.data
}
queryData().then(function(result) {
    console.log(result);
})
```