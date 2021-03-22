#  本地存储 sessionStorage

sessionStorage 是**临时会话储存**

浏览器页面关闭就清除sessionStorage本地存储的内容 ，

刷新页面并不会清除sessionStorage存储的内容

**基础操作和 localStorage 类似**

一般5M大小

---

## setItem()

**储存**

以键值对形式存入本地存储

**key和value必须是字符串**

> sessionStorage.**setItem(key,vale)**

```js
sessionStorage.setItem('str','hello');
```

---

数字会被隐式转换为字符串

key值若不是字符串，比如是个对象

需要 **`JSON.stringify()`** 把对象转换为字符串形式后才能存入

不然存入的是这个：`[object Object]`

```js
let obj = {
  name: "andy",
  age: 28
};

sessionStorage.setItem('obj',JSON.srtringify(obj));
```





## getItem()

**取出**

>sessionStorage.**getItem(key)**

```js
sessionStorage.getItem('str');
```

---

若是通过 **`JSON.stringify()`** 把对象转换为字符串形式后存入的话，

数据类型已经被转为了字符串，取出的也是个字符串，

不能再通过 ` . 属性` 的方法获取对象的值

必须通过 `JSON.parse()` 把字符串形式的对象转为对象

``` js
let obj = {
  name: "andy",
  age: 28
};
sessionStorage.setItem('obj',JSON.srtringify(obj));
console.log(sessionStorage.getItem('obj'));
// '{"name":"andy","age":28}'

console.log(JSON.parse(sessionStorage.getItem('obj')));
// {name: "andy",age: 28}
```





## removeItem()

**删除指定**

> sessionStorage.**removeItem(key)**

```js
sessionStorage.removeItem('str');
```



## clear()

**删除全部**

>sessionStorage.**clear()**

```js
sessionStorage.clear();
```

