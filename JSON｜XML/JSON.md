# JSON基础

JSON（**J**ava**S**cript **O**bject **N**otation(JavaScript）对象表示法

存储和交换文本信息的语法

轻量级的文本数据交换格式

比XML更小更快更易于前端，目前是主流



## 作用

用于计算机与网络之间的传输、存储数据



## 本质

JSON本质就是字符串

用来表示JavaScript对象和数组的 字符串



## JSON的两种结构

因为JSON是通过字符串形式表示JS的对象和字符串

所以JSON有两种结构：**数组** 和 **对象**

通过两种结构的相互嵌套表示各种复杂的数据结构



### 对象结构

用 { } 包裹的 key：value的健值对形式

```json
{
  "key": value,
  "key": value
}
```

- key 必须用英文双引号包裹

- value 的数据类型可以是：
  - 数字、
  - 字符串、(必须用英文双引号包裹)
  - 布尔值、
  - null、
  - 数组、
  - 对象

```json
{
  "name": "Andy",
  "age": 28,
  "address": null,
  "married": false,
  "skills": ["Vue", "React", "Node.js"]
}
```



### 数组结构

用 [ ] 包裹

```json
[value, value, value]
```

数组中值的数据类型可以是：

- 数字、
- 字符串、(必须用英文双引号包裹)
- 布尔值、
- null、
- 数组、
- 对象

```json
["JavaScrip","Vue","TypeScript"]

[true, false, true]

[[100,200,300], ["1","2","3"]]
```





## JSON语法注意点

- 属性名必须用双引号包裹
- 字符串类型数据必须用双引号包裹
- 不允许使用单引号
- 不允许出现注释
- JSON的最外层必须是对象格式的 { } 或 数组格式的 [ ]
- JSON的数据值只能是以下6种：
  - 数字、
  - 字符串、(必须用英文双引号包裹)
  - 布尔值、
  - null、
  - 数组、
  - 对象





## JSON 和 JavaScript对象的关系

JSON是JavaScript对象的字符串表示法，

JSON本质是一个字符串，

试文本能够表示一个JS对象信息

```js
var jsObj = {name:'Andy',age:28} 

var jsonStr = '{"name":"Andy","age":28}'
```





## JSON 和 JavaScrip对象的转换

### JSON.parse()

JSON——> JS对象

```js
JSON.parse(JSON)
```

```js
var jsonStr = '{"name":"Andy","age":28}';

var jsObj = JSON.parse(jsonStr);

console.log(jsObj);
/*
	{
		age: 28
		name: "Andy"
	}
*/
```

---

### JSON.stringify()

JS对象——> JSON

```js
var jsObj = {
    age: 28,
    name: "Andy"
};

var jsonStr = JSON.stringify(jsObj)

console.log(jsonStr);
/*
	{"age":28,"name":"Andy"}
*/
```

---

### 常用于Ajax

```js
var xhr = new XMLHttpRequest();
xhr.open("GET", "http://www.liulongbin.top:3006/api/getbooks?id=2");
xhr.send();
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    var res = xhr.responseText;
    
    console.log(typeof res)
    
    console.log(JSON.parse(res));
  }
};
```





## JSON的序列化 和 反序列化

- 序列化：JS对象——> JSON， JSON.strinfify()

- 反序列化：JSON——> JS对象   JSON.parse()

 