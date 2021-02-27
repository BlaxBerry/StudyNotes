# 触屏事件 touch

## touchstart

## touchmove

## touchend

|    名称    |             解释              |
| :--------: | :---------------------------: |
| touchstart |    手指触摸到DOM元素时触发    |
| touchmove  |   手指在DOM元素上滑动时触发   |
|  touchend  | 手指从一个DOM元素上离开时触发 |

类似PC端的mousedown、mousemove、mouseup

```html
    <style>
        div {
            width: 100px;
            height: 100px;
            background-color: crimson;
        }
    </style>
</head>

<body>
    <div></div>
    <script>
        let div = document.querySelector('div');

        div.addEventListener('touchstart', () => {
            console.log(11);
        })
        div.addEventListener('touchmove', () => {
            console.log(222);
        });
        div.addEventListener('touchend', () => {
            console.log(33);
        })
    </script>
</body>
```



# 触摸事件对象 TouchEvent

检测手指在触摸屏上状态的变化，比如

手指的移动，触点的增加和减少等

```js
DOMELement.addEcentListener('touchEvent',(e)=>{})
```

---

| 名称           | 解释                                    |
| -------------- | --------------------------------------- |
| touches        | 在触摸**屏幕**的触摸点（手指数）        |
| targetTouches  | 在触摸**当前DOM元素**的触摸点（手指数） |
| changedTouches | 侦听触摸点的变化（触摸手指数的变化）    |

## e.touches

获得的是触摸的手指列表数组

手指离开时就没有了

## e.targetTouches

**常用**

获得的是触摸该元素的手指列表**数组**

获得一个手指的相关信息比如坐标，如下：

```js
//触摸该元素的第一个手指的信息
div.addEventListener('touchstart', (e) => {
            console.log(e.targetTouches[0]);
        })
//#=>
clientX: 43.40119171142578
clientY: 45.06778335571289
force: 1
identifier: 0
pageX: 43.40119171142578
pageY: 45.06778335571289
radiusX: 15.54054069519043
radiusY: 15.54054069519043
rotationAngle: 0
screenX: 919.1168823242188
screenY: 236.35015869140625
target: div
```

手指离开时就没有了

## e.changedTouches

手指的从有到无、从无到有



# 拖动元素

手指触摸—>手指移动—>手指离开

还需要手指的当前坐标：e.targetTouches[0]里的pageX和pageY



元素原来的坐标 + 手指移动的距离

手指的移动距离 = 手指活动中的坐标 - 手指触摸时的坐标



1. touchstart事件，获取手指触摸的初始坐标，和目标元素的位置
2. touchmove事件，计算手指移动距离，移动元素
3. touchend事件，结束