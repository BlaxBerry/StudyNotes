

![](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fdevbbs-discuzx.stor.sinaapp.com%2Fdata%2Fattachment%2Fforum%2F201212%2F24%2F1633169qu7icgwspbqwgz7.png&refer=http%3A%2F%2Fdevbbs-discuzx.stor.sinaapp.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1617117429&t=0f732dca3aba5c57cc5e861972ffa0e9)

# 异步

事件

定时器 setTimeout

动画函数

ajax

...

可理解为有回调函数callback的





## 回调函数

callback









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
ReferenceError: num is not defined
```

上述，

因为定时器是异步，所以该线程内fn函数里什么操作程序都没有，

所以return返回的num也是不存在与该线程的，

没有找不到所以undefined

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

直再加上也没有retrun返回值就结束了，所以函数fn返回值是undefined

所以在外部输出调用为undefined









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