# ES6

![](https://blog-imgs-102.fc2.com/t/r/i/tridentwebdesign/es2015-001.jpg)



## var的弊端

var关键字声明变量的弊端有以下：

- var声明的变量有预解析，可以先使用后定义，不严谨，**逻辑混乱**

```js
var a = 10;
console.log(a);  //10

//变量提升
console.log(b);  //undefined
var b = 10;
```

---

- var可以重复声明一个变量，第二次开始就是重新赋值修改变量，而不是定义，**逻辑错误**

```js
var a = 10;

var a = 100;

console.log(a);  // 100
```

---

- var在for循环条件中，会造成**临时变量污染**

```js
for (var i = 0; i < 3; i++) {
    console.log(i);
}
console.log('结果是：' + i);  
//=》
0
1
2
结果是：3

//for中的变量在循环结束后没有消除回收，并作为全局变量保留了下来，
```

---

- var声明的变量没有块级作用域，只有全局作用域和局部作用域（函数作用域）,块级作用域内的变量在外边也能被使用，会造成**变量污染**

```js
var a = 10; 
{
    var a = 200;
}

console.log(a);  // 200
```





## let关键字的优点

**常用**

- let声明的变量必须先定义，后才能使用，不然报错

没有变量提升

---

- let声明过的变量不能被再次定义，不然会报错

变量不能被重复定义，但是可以被赋值修改

---

- for循环中的临时变量在循环结束后就被回收，不能在循环外被使用，不然会报错。不会出现泄露

---

- 块级作用域内定义的变量只能在块级作用域内被使用，不然会报错





## const定义常量

常量中存放的是不能被修改的数据

比如服务端的server，和文件的路径....

const定义的常量的引用类型不能被修改，不然会报错

```js
const num = 10;
num = 20;

//报错
TypeError: Assignment to constant variable.
```

---

一般const定义的常量名用大写。

---

一般**引入某个某块的某个对象**时，使用const。

比如Node.js中引入模块：

```js
const http = require('http')

const MathFunctionPackage = require(./js/MathFunctionPackage.js)
```

---

**对象型的常量**的**属性**可以修改

```js
const OBJ = {
    name: 'Andy',
    age: 18
}
console.log(OBJ);   // { name: 'Andy', age: 18 }

OBJ.name = 'James';
OBJ.age = 28;
console.log(OBJ);   //{  name: 'James', age: 28 }
```

---

**数组型的常量**的**元素**可以修改

```js
const arr = [1, 2, 3];
console.log(arr);    // [1, 2, 3]

arr[0] = 6;
arr[1] = 7;
arr[2] = 8;
console.log(arr);   // [ 6, 7, 8 ]
```





## 解构（解构赋值）——对象

解构可以用来获取对象的元素

是用来简化代码书写的

### ES5获取对象属性的方法

比如，想要获得对象的属性，

以前是分别单独声明变量，然后分别调用属性，再分别存入，

```js
var obj = {
    name: 'Andy',
    age: 18,
    sex: 'male'
}
var name = obj.name;
var age = obj.age;
var sex = obj.sex;
var score = obj.score;  //undefined

console.log(name, age, sex);
```

或使用键值对

```js
var obj = {
    name: 'Andy',
    age: 18,
    sex: 'male'
}
var name = obj['name'] 
var age = obj['age'];
var sex = obj['sex'];
var score = obj['score'];  //undefined

console.log(name, age, sex);
```

以上两种都是ES5的做法，都比较麻烦



### 对象的解构赋值

使用解构赋值，解构对象的属性，更方便

```js
let {属性名，属性名} = 对象
```

可理解为，把对象中的属性拿出，给了一个同名的变量

---

如下：**完全解构**一个对象

```js
let obj = {
    name: 'Andy',
    age: 18,
    sex: 'male'
}
let { name, age, sex } = obj;

console.log(name, age, sex);
```

---

也可**部分解构**，用谁就解构获取谁。如下：

```js
let obj = {
    name: 'Andy',
    age: 18,
    sex: 'male'
}

let { name } = obj;
console.log(name);
```

---

顺序颠倒不影响

但是不能解构出对象中没有的属性

**若对象里没有同名属性**的话，会返回undefined，还和ES5一样，

```js
let obj = {
    name: 'Andy',
    age: 18,
    sex: 'male'
}

let { score, country } = obj;
console.log(score, country);
// undefined undefined
```

---

解构获得的属性**修改后，不会影响对象**原本属性

```js
let obj = {
    name: 'Andy',
    age: 18,
    sex: 'male'
}
let { name, age, sex } = obj;
name = 'james';
console.log(name);
console.log(obj);
//->
james
{ name: 'Andy', age: 18, sex: 'male' }

//并没有影响到对象原本的属性
```



### 自定义变量名结构对象属性

解构对象是结构对象的属性，可理解为把对象中的属性拿出，赋值给了一个同名的变量，

但如果在对象外，对象的属性名被使用过了，会因为**变量名冲突**报错。如下：

```js
let name = 'James';
let obj = {
    name: 'Andy',
    age: 18
}
let {name}=obj;

//报错
SyntaxError: Identifier 'name' has already been declared
//因为name变量已经被let定义过了，不能重复定义

```

为了防止变量名冲突，可使用**自定义变量**接受对象的属性

```js
let {属性名:自定义变量名} = 对象
```

如下：

```js
let name = 'James';
let obj = {
    name: 'Andy',
    age: 18
}
let { name: myName } = obj;

console.log(myName);  // Andy
```

相当于ES5的:

```js
var myName = obj.name;
```





## 解构（解构赋值） ——数组

解构数组不是解构的主要目的，是顺带的

### ES5获取数组元素的方法

以前是分别单独声明变量，然后分别获取数组元素，再分别存入，如下：

```js
var arr = [
    [1, 2, 3],
    ['A', 'B', 'C'],
    { name: 'andy', age: 18 }
];

var a = arr[0];
var b = arr[1];
var c = arr[2];
console.log(a,b,c)
```

也是写起来麻烦



### 数组的解构赋值

使用解构赋值，解构数组的元素，更方便

```js
let [变量，变量] = 数组
```

可理解为，把数组的元素取出，**按顺序**赋值给变量。

---

因为数组没有对象的键值对，

所以是**按元素的顺序一一**对应

如下，全部取出：

```js
let arr = [
    [1, 2, 3],
    ['A', 'B', 'C'],
    { name: 'andy', age: 18 }
];

let [a, b, c] = arr;

console.log(a);
console.log(b);
console.log(c);
```

如下，只取出一个，此时是第一个元素：

```js
let arr = [
    [1, 2, 3],
    ['A', 'B', 'C'],
    { name: 'andy', age: 18 }
];

let [a] = arr;
console.log(a); //[1,2,3]
```

---

修改解构获取的数组元素，**不会影响到数组本身**

```js
let arr = [
    [1, 2, 3],
    ['A', 'B', 'C'],
    { name: 'andy', age: 18 }
];
let [, , a] = arr;
console.log(a);

a = [6, 6, 6]
console.log(a);
console.log(arr);

//-》
{ name: 'andy', age: 18 }
[ 6, 6, 6 ]
[ [ 1, 2, 3 ], [ 'A', 'B', 'C' ], { name: 'andy', age: 18 } ]
//获取的变量时被修改了，但是
//数组本身没有被修改
```



### 指定解构获取的元素

因为解构数组是**按顺序**解构元素。所以只写一个变量时永远是获取数组的第一个元素。

但可以用逗号空开，来指定要获取的元素的序号

（虽然写起来不如直接写序号简单）

```js
//解构赋值数组的第1个元素
let [变量] = 数组；

//解构赋值数组的第2个元素
let [,变量] = 数组；

//解构赋值数组的第3个元素
let [,,变量] = 数组；

...
```

如下，只获得数组的第2个元素：

```js
let arr = [
    [1, 2, 3],
    ['A', 'B', 'C'],
    { name: 'andy', age: 18 }
];

let [, , a] = arr;

console.log(a);
//{ name: 'andy', age: 18 }
```

目前仅有这种方式，若一个数组中有100个元素，还不如直接用索引号



### 解构数组中的对象

若数组中有对象作为元素,

还是按顺序结构数组元素

```js
let [{属性，属性}]
```

如下：

```js
let arr = [
    { name: 'andy', age: 18 },
    { name: 'james', age: 28 }
];

let [
  { name: andyName, age: andyAge }, 
  { name: jamesName, age: jamesAge }
] = arr;

console.log(andyName, andyAge);  // andy 18
console.log(jamesName, jamesAge); // james 28
```



### ES5获取多维数组的元素

ES5获取多维数组元素的方法如下：

```js
var 数组[i][i][i]
```

```js
var arr = [
    [1, 2, 3],
    [6, 7, 8]
];
console.log(arr[2][0]);  // 6
```

若多维数组中有对象作为元素

```js
var 数组[i][i][i].属性
或
var 数组[i][i][i]['属性']
```

```js
var arr = [
    { name: 'andy', age: 18 },
    { name: 'james', age: 28 }
];
console.log(arr[0].name);    //andy
console.log(arr[1]['name']); //james
```



### ES6解构多维数组

只要是数组，解构时元素就要**按顺序**一一对应元素

```js
let arr = [
    [1, 2, 3],
    [6, 7, 8]
];
let [a, b] = arr;
console.log(a);  //[1,2,3]
```

```js
let arr = [
    [1, 2, 3],
    [6, 7, 8]
];
let [
    [a1, a2, a3],
    [b1, b2, b3]
] = arr;
console.log(a1, a2, a3); // 1 2 3
console.log(b1, b2, b3); // 6 7 8
```

```js
let arr = [1, [2, 3], 4, 5, [6, 7, 8]];

let [a,[ b, c], d, e, [f, g, h]] = arr;

console.log(a, b, c, d, e, f, g, h);
// 1 2 3 4 5 6 7 8
```





## 解构（解构赋值）——字符串

解构也能用于获取字符串的字符，了解即可

### ES5获取字符串的字符

和获取数组的元素一样，使用序号下标

```js
var str = 'hello Node'
console.log(str);
console.log(str[0]);  // h
console.log(str[5]);  // 空格
```



### 字符串也能被解构

和解构数组一样，字符和顺序一一对应。如下：

```js
let str = 'hello';

let [a, b, c, d, e] = str;
console.log(a, b, c, d, e);
//h e l l o
```





## 字符串——模版字符串

### ES5拼接字符串

```js
		'' + 变量 + ''
```

比如用在DOM中：

```js
var div = document.querySelector('div');
var inner = 'Hello~'

div.innerHTML = '<sapn>' + inner + '</sapn>';
```

拼接字符串比较占用内存



### 模版字符串插入变量

作用和字符串的单双引号一样

```js
let str = ``
console.log(typeof str); // string
```

简化了拼接字符串

拼接时直接在模版字符串中放入包裹在`${}`中的变量

```js
		`${变量}`
```

如下：

```js
let name = 'andy';
let age = 18;

console.log(`my name is ${name}, my age is ${age}`);
//my name is andy, my age is 18
```

再比如用在DOM中：

```js
var div = document.querySelector('div');
var inner = 'Hello~'

div.innerHTML = `<sapn>${inner}</span>`;
```





## 对象简化写法

就是简写定义对象时的属性的定义。

在创建对象时，属性的值可以直接写成实现定义好的变量

```js
let name = 'Andy';
let age = 18;
let sex = 'male';

let obj = {
    myname: name,
    myage: age,
    mysex: sex
}
```

如果 对象的属性名和作为值的变量名相同时，如下场合：

```js
let name = 'Andy';
let age = 18;
let sex = 'male';

let obj = {
    name: name,
    age: age,
    sex: sex
}
```

---

可以使用ES6 新增的对象属性的简写方法

直接写变量，省略写属性名，如下：

```js
let name = 'Andy';
let age = 18;
let sex = 'male';

let obj = {
    name,
    age,
    sex
}
console.log(obj);
//{ name: 'Andy', age: 18, sex: 'male' }
```

也就是下面的写法：**常用**

```js
let name = 'Andy';
let age = 18;
let sex = 'male';
let obj = { name,age,sex }
```





## babel翻译器

ES6的简化写起来很方便，但是低版本的浏览器不支持。

可以使用babel翻译器，把ES6语法翻译成ES5

babel可以实现

ES6语法——> ES5语法

![](https://capitalp.jp/wp-content/uploads/2018/09/d5ec986cfb9c29030c488a4b70414518.png)

[babeljs.io](https://babeljs.io/)

