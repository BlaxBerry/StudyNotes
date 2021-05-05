# 常用数组方法



## concat()

链接两个或多个数组

返回一个新数组

```js
let a = [1, 2, 3]
let b = [4, 5, 6]

let arr = a.concat(b)
console.log(arr); // [ 1, 2, 3, 4, 5, 6 ]
```

---

解构数组

```js
let a = [1, 2, 3]
let b = [4, 5, 6]

let arr = [...a, ...b]
console.log(arr);  // [ 1, 2, 3, 4, 5, 6 ]
```



## join()

转为字符串（将所有元素是放入一个字符串）

参数为分割符，默认是逗号 **，**

返回一个字符串

```js
let a = [1, 2, 3]

let str = a.join(',')
console.log(str);   // 1,2,3
console.log(typeof str); // String
```



## toString()

转为字符串

```js
let a = [ 1, 2, 3, 4 ]

console.log(a.toString(); // 1,2,3,4

console.log(a.toString() === a.join()); true
```



## push()

在末尾添加（追加）一个或多个元素

后改变数组

```js
let a = [1, 2, 3]

a.push(4)
console.log(a); // [ 1, 2, 3, 4 ]
```

```js
let a = [1, 2, 3]

a.push(4)
console.log(a); // [ 1, 2, 3, 4, 5, 6 ]
```

返回的是修改后的新数组的长度

```js
let a = [1, 2, 3]

console.log(a.push(4, 5, 6)); // 6
```



## pop()

删除最后一个元素

会改变数组

```js
let a = [1, 2, 3, 4]

a.pop()
console.log(a); // [ 1, 2, 3 ]
```

返回的是删除的元素

```js
let a = [1, 2, 3, 4, 5]

console.log(a.pop()); // 5
```



## unshift()

在开头添加一个或多个元素

会改变数组

```js
let a = [1 ,2, 3;

a.unshift(0)
console.log(a); // [ 0, 1, 2, 3 ]
```

```js
let a = [4, 5, 6];

a.unshift(1, 2, 3)
console.log(a); // [ 1, 2, 3, 4, 5, 6 ]
```

返回的是修改后的新数组的长度

```js
let a = [4, 5, 6];

console.log(a.unshift(1, 2, 3)); // 6
```



## shift()

删除第一个元素

会改变数组

```js
let a = [1, 2, 3, 4]

a.shift()
console.log(a); // [ 2, 3, 4 ]
```

返回的是删除的元素

```js
let b = [4, 5, 6];

console.log(b.shift()); // 4
```



## silce()

截取数组

返回一个新数组

```js
arr.slice(start, end)
```

截取 start到end之间 的元素（不包含start和end）

start 和 end都是 index序号

```js
let a = [4, 5, 6, 7]

console.log(a.slice(1, 2)); // [ 5 ]
console.log(a.slice(0, 3)); // [4, 5, 6]
console.log(a.slice(1, a.length - 1)); // [5, 6 ]
```



## splice()

删除 / 替换

会改变数组

```js
arr.splice(index, amount, target)
```

第一个参数是从哪里开始，index序号

第二个参数是要删除几个，元素的个数

第三个参数是要替换为的元素，可省略

- 省略第三个参数则是**仅删除**

- 第二个参数为0则是**不删除，仅替换**

  在第index处加入新元素，原来元素index序号后移

  等同与`unshift()`

```js
let a =  [4, 5, 6, 7]

a.splice(0, 1)
console.log(a) //  [5, 6, 7]
```

```js
let a =  [4, 5, 6, 7]

a.splice(1, 2)
console.log(a) //  [4, 7]
```

---

```js
let a = [4, 5, 6, 7]

a.splice(0, 1, 9)
console.log(a) // [ 9, 5, 6, 7 ]
```

```js
let a = [4, 5, 6, 7]

a.splice(0, 3, 9)
console.log(a) // [ 9, 7 ]
```

```js
let a = [4, 5, 6, 7]

a.splice(0, 5, 9)
console.log(a) // [ 9 ]
```

---

```js
let a = [4, 5, 6, 7]

a.splice(0, 0, 9)
console.log(a) // [ 9, 4, 5, 6, 7 ]
```

---

返回的是含有被删除的元素的数组

```js
let a = [4, 5, 6, 7]

console.log(a.splice(0, 1)); // [4]
```



## sort()

升序冒泡排列

会修改数组

返回的是修改后的数组

```js
a = [4, 1, 3, 2, 5]

console.log(a.sort()); // [ 1, 2, 3, 4, 5 ]
```



## reverse()

反转颠倒数组

会改变数组

返回的是颠倒后的数组

```js
let a = [1, 2, 3, 4, 5]

console.log(a.reverse()); // [ 5, 4, 3, 2, 1 ]
```



## indexOf()

## lastIndexOf()

查找元素的序号

参数是元素，返回的该元素是在数组中的index序号

```js
let a = [1, 2, undefined, 4, NaN, 2, 2];

console.log(a.indexOf(2)); // 1
console.log(a.indexOf(9)); // -1
console.log(a.indexOf(undefined)); // 2

console.log(a.lastIndexOf(2)); // 6
console.log(a.lastIndexOf(9)); // -1
```

但是不能判断是否含有 NaN

```js
let a = [1, 2, 3, 4, NaN, 2, 2];

console.log(a.indexOf(NaN)); // -1
```



## every()

## some()



## forEach()

遍历数组

相当于for，给每一个元素执行相同操作

```js
arr.forEach((item, index, self) => {
  
})
```

```js
let a = [1, 2, 3]
let res = []
a.forEach(item => {
    res.push(item.toString())
})

console.log(a); // [1, 2, 3]
console.log(res); // [ '1', '2', '3' ]
```



## map()

给每个元素执行相同回调函数

将执行的返回值存入一个新数组

返回一个新数组，

新数组每个元素是旧数的元素执行回调函数后的结果，

所以必须要 **return** 返回该结果，不然返回结果是undefined

```js
let a = [ 1, 2, 3 ]

let res = a.map(item => {
    item.toString()
})
console.log(res);  
// [ undefined, undefined, undefined ]



res = a.map(item => {
    return item.toString()
})
console.log(res); // [ '1', '2', '3' ]
```

可理解为省略了 forEach遍历后再追加到新数组



## filter()

判断并筛选 所有符合元素

让数组每一个元素都执行相同的回调函数

并将所有执行结果为 true的元素存入一个新数组

返回一个新数组

必须用return返回执行的结果，不然是空数组

```js
let a = [1, 2, 3, 4, 5, 6]
let res = a.filter(item => {
    return item % 2 == 0
})
console.log(res); // [ 2, 4, 6 ]


let res = a.filter(item => {
    item % 2 == 0
})
console.log(res); // [ ]
```



## reduce()

累加

```js
var arr = [10,20,30,40,50];
var sum = arr.reduce(function(prev,now,index,self){
  return prev + now;
})
console.log(sum);  //  150
```





---

---

## 新增ES6 Array API

---

---



## find()



## fineIndex()



## fill()



## from()



## of()



## includes()

