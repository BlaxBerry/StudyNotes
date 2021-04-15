# Array APIs 数组方法

## 变异方法

会修改原始数组

---

### splice()

**删除指定个数的元素**

> arr.splice(**开始序号**，**删除个数**)
>
> **返回值**是被删除的元素组成的数组

```js
arr.splice(start,num)
```

```js
// arr.splice(2,1);  从序号2开始删除1个

let arr = [1, 2, 3, 4, 5]
let newArr = arr.splice(2, arr.length - 2);

console.log(arr); // [1, 2]
console.log(newArr); // [3, 4, 5]
```

注意和`slice()`区分



### reduce()

**计算总和**

遍历数组的每个元素 并相加计算总和，遍历次数为元素个数

用return 返回计算的总数

> arr.reduce( functino(总和，每个元素){ 
>
> ​	**return** 总和 += 每个元素;
>
> }, 计算的起始值)

```js
arr.reduce(function(total,item){
  return total += item;
},0)
```

```js
var arr = [1, 2, 3, 4];
var res = arr.reduce(function(total, item) {
  return total += item
}, 0);
console.log(res); // 10

---------------------------------
  
var arr = [1, 2, 3, 4];
var res = arr.reduce(function(total, item) {
  return total
}, 0);
console.log(res); // 0
```

---

若数组的元素是**对象**，

`参数 item` 是每个元素，即是每个对象，可通过 `.属性` 获得对象的属性值

```js
var arr = [
   {num: 1}, 
   {num: 2}, 
   {num: 3}, 
   {num: 4}, 
];
var res = arr.reduce(function(total, item) {
  return total += item.num
}, 0);
console.log(res);
```



push()



pop()



shift()



unshift()









## 非变异方法

不会改变原数组，返回一个新数组
若想修改原数组，需要再赋值回原数组

---

### slice() 

**截取数组元素**

>arr.slice(**开始序号**)
>
> 从开始序号一直截取到到数组结尾的所有元素。
>
>arr.slice(**开始序号，结束位置元素的个数**)
>
>从开始序号一直截取到数组的第？个元素
>
>
>
>**返回值**是截取的元素组成的数组

```js
arr.slice(start)
arr.slice(start,end)
```

```js
// arr.slice(1)
//从序号是1的元素开始截取到数组结尾
//arr.slice(2,5)
//从序号2的元素开始截取到数组的第5个元素

let arr = [1, 2, 3, 4, 5]

let newArr = arr.slice(2, arr.length)
console.log(newArr); // [3, 4, 5]
console.log(arr); // [1, 2, 3, 4, 5]
```

注意和`splice()`区分

若想修改原是数组，可如下：

```js
let arr = [1, 2, 3, 4, 5];

arr = arr.slice(2, arr.length);
console.log(arr); // [3, 4, 5]
```



filter()



### some()

**筛选元素**

> 

```

```

```

```

concat()



