# Canvas

## 简介



坐标

<Canvas\>范围的左上角为X轴和Y轴的中心点（0，0）



## 实例

绘制图形

如下：绘制一个宽150，高75的2d图形

1. 找到元素

```js
let canvas = document.getElementById('myCanvas')
```

2. 创建绘图上下文（对象）用于绘制

```js
let ctx = canvas.getContext('2d')
```

3. 通过 ffillRect函数 在<Canvas\> 上绘制

```js
//从左上角开始绘制宽150，高75的图形
ctx.fillRect(0, 0, 150,75)
```

4. 描述要绘制的图形颜色

```js
ctx.fillStyle = 'Crimson'
```

---

```js
let canvas = document.getElementById('myCanvas')

let ctx = canvas.getContext('2d')

ctx.fillRect(0, 0, 150, 75)

ctx.fillStyle = 'Crimson'
```







绘制直线

如下：绘制一条从左上角（10，10）开始到（100，100）的2d直线

1. 找到元素

```js
let canvas = document.getElementById('myCanvas')
```

2. 创建绘图上下文（对象）用于绘制

```js
let ctx = canvas.getContext('2d')
```

3. 线的起始点和结束点

```js
// 线的起始点
ctx.moveTo(10, 10)
//线的结束点
ctx.lineTo(100, 100)
```

4. 通过 storke函数 在<Canvas\> 上绘制

```js
ctx.stroke()
```

---

```js
let canvas = document.getElementById('myCanvas')

let ctx = canvas.getContext('2d')

ctx.moveTo(10, 10)
ctx.lineTo(100, 100)

// 绘制直线
ctx.stroke()
```





绘制圆形

1. 找到元素

```js
let canvas = document.getElementById('myCanvas')
```

2. 创建绘图上下文（对象）用于绘制

```js
let ctx = canvas.getContext('2d')
```

3. 通过 arc函数 描述绘制图形

```js
ctx.arc(50, 50, 30, 0, 2*Math.PI)
```

4. 通过storke函数 在<Canvas\> 上绘制

```js
ctx.stroke()
```

---

```js
let canvas = document.getElementById('myCanvas')

let ctx = canvas.getContext('2d')

ctx.arc(50, 50, 30, 0, 2*Math.PI)

ctx.stroke()
```





渐变



MDN



APIs