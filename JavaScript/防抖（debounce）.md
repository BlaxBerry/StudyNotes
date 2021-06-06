# 防抖（debounce）

防抖策略（debounce），是指

**当事件被触发后不会立即执行，延迟 n秒后再执行事件回调函数，**

**若在延迟期间再次被触发，则重新计时使延迟时间**

（比如：英雄回城的读条时间，回城读条时的中断和在回城）

可以防止频繁触发执行某事件函数，

保证即使频繁循环触发事件，事件最终也只会被执行一次

从而提高浏览器的性能

```mermaid
graph LR
A[触发点击事件] -->B[清除定时器] -->C[设置定时器] 
C -->|规定时间内 再次触发事件| D[重新一轮]    
C -->|规定时间内 没有触发事件| E[执行表达的提交]

```

## 使用场景

- **输入框内容的连续输入**（常用）

  防止多次提交请求，仅在输入完成后提交

- 改变页面大小的统计

- 滚动页面位置的统计





## 输入框防抖

### 步骤

1. **定义防抖Timer**

2. **定义防抖函数**

   不是直接发起Ajax / JSONP请求，

   而是通过定时器延时一定时间后再发起数据请求

3. **输入框触发keyUp事件时，清空Timer**

   有内容输入时，清空上一个请求的延时器，使上一个请求不会被发起

   等输入结束后，最后的输入请求的延时器没人清空，最后请求会被发送出去





## 实例

没有防抖，每次输入都会发送数据请求

```html
<body>
    <input type="text" name="" id="" />
    <hr />
    <ul></ul>
</body>

<script type="text/html" id="art">
    {{each result}}
    <li>{{$value[0]}}</li>
    {{/each}}
</script>

<script>
    $(function () {
      $("input").on("keyup", function () {
        var val = $(this).val().trim();
        getData(val);
      });

      // JSONP 请求
      function getData(keyword) {
        $.ajax({
          url: "https://suggest.taobao.com/sug?q=" + keyword,
          dataType: "jsonp",
          success: function (res) {
            // console.log(res);
            renderUI(res);
          },
        });
      }

      // 渲染UI
      function renderUI(res) {
        if (res.result.length <= 0) {
          return $("ul").empty().hide();
        } else {
          var art = template("art", res);
          $("ul").html(art).show();
        }
      }
    });
</script>
```



```html
<body>
    <input type="text" name="" id="" />
    <hr />
    <ul></ul>
</body>

<script type="text/html" id="art">
    {{each result}}
    <li>{{$value[0]}}</li>
    {{/each}}
</script>

<script>
    $(function () {
      //1. 定义延时器ID
      var timer = null;

      // 2. 定义防抖函数
      function debounceSearch(keyword) {
        timer = setTimeout(function () {
          // 调用数据请求函数
          getData(keyword);
        }, 500);
      }

      //3. 输入框绑定keyup事件
      $("input").on("keyup", function () {
        // 4. 清空上一个请求的延时器
        clearTimeout(timer);
        var val = $(this).val().trim();
        // 5. 调用防抖函数
        debounceSearch(val);
      });

      
      // JSONP 请求
      function getData(keyword) {
        $.ajax({
          url: "https://suggest.taobao.com/sug?q=" + keyword,
          dataType: "jsonp",
          success: function (res) {
            // 渲染数据到页面
            renderUI(res);
          },
        });
      }

      // 渲染UI
      function renderUI(res) {
        if (res.result.length <= 0) {
          return $("ul").empty().hide();
        } else {
          var art = template("art", res);
          $("ul").html(art).show();
        }
      }
    });
  </script>
```







## 流程难点解决

### 1、设置事件

```js
const btn = document.querySelector('input');

function sendMessage(){
  console.log('提交表单')
};

btn.addEeventListener('click',sendMessage())

```



### 2、难点一：自动执行

设置并调用防抖函数后，未触发事件但会自动调用

```js
const btn = document.querySelector('input');

function sendMessage(){
  console.log('提交表单')
};


function debounce(func){
  func()
}


btn.addEeventListener('click',debounce(sendMessage)
```

---

解决方法：

通过高阶函数，让防抖函数返回一个函数

只有触发了事件才会调用函数

```js
const btn = document.querySelector('input');

function sendMessage(){
  console.log('提交表单')
};


function debounce(func){
  return function(){
    func()
  }
}


btn.addEeventListener('click',debounce(sendMessage)
```



### 3、设置延时 和 清除定时器

设置了定时器，就必须在设置前清除定时器，

但清除定时器clearTimeout**无法清除一个未被定义的变量**

所以在清除前先定义一个定时器变量

```js
const btn = document.querySelector('input');

function sendMessage(){
  console.log('提交表单')
};


function debounce(func,delayTime){
  return function(){
    let timer;
    clearTimeout(timer);
    timer = setTimeout(function(){
      func()
    },delayTime)
  }
}


btn.addEeventListener('click',debounce(sendMessage,1000)

```



### 4、难点二：定时器变量不唯一

若仅仅如上设定，规定时间内快速点击后，点击次数全部执行，

因为定时器变量的创建时在执行的函数内部

所以每次点击事件触发的函数都是独立的，**不是同一个变量**

即每次执行函数都会**重新创建新变量，清除不到定时器变量**

所以并没有出现延时执行和重新计时

---

解决方法：

通过作用域链 + 闭包

把创建定时器变量放在定义函数的外部，

每次执行函数时定义和清除的变量就是同一个，唯一的

```js
const btn = document.querySelector('input');

function sendMessage(){
  console.log('提交表单')
};


function debounce(func,delayTime){
  let timer;
  return function(){
    clearTimeout(timer);
    timer = setTimeout(function(){
      func()
    },delayTime)
  }
}


btn.addEeventListener('click',debounce(sendMessage,1000)
```



### 4、难点三：this指向

按理，提交函数中的this应该是指向函数的调用者，即发送按钮

但是，因为该提交函数是在定时器中被调用的，

而定时器中又是在window对象下被调用，

所以此时提交函数中的this成了window，出错了

```js
const btn = document.querySelector('input');

function sendMessage(){
  console.log('提交表单');
  // console.log(this) /// btn触发事件的按钮元素本身
};


function debounce(func,delayTime){
  let timer;
  return function(){
    clearTimeout(timer);
    timer = setTimeout(function(){
      func() /// 在定时器中被调用，是windows
      // console.log(this) // window
    },delayTime)
  }
}


btn.addEeventListener('click',debounce(sendMessage,1000)
```

---

解决方法：

在setTimeout定时器函数前，提前保留下this的指向，

然后用 **call**，把函数的调用者指向为保留的正确的this指向

```js
const btn = document.querySelector('input');

function sendMessage(){
  console.log('提交表单');
};


function debounce(func,delayTime){
  let timer;
  return function(){
    let context = this；
    clearTimeout(timer);
    timer = setTimeout(function(){
      // console.log(context) // btn 
      // console.log(this) // window      
      func.call(context) 
    },delayTime)
  }
}


btn.addEeventListener('click',debounce(sendMessage,1000)
```



### 5、难点四：参数

考虑到了this的指向，也要考虑参数的问题

JS的函数即使没设定参数也是可以传入参数的

每个执行函数都有可能被传入参数

传入的参数因该是给提交函数执行，需要通过 **apply** 传过去

```js
const btn = document.querySelector('input');

function sendMessage(){
  console.log('提交表单');
};


function debounce(func,delayTime){
  let timer;
  return function(){
    let context = this；
    let args = arguments;
    clearTimeout(timer);
    timer = setTimeout(function(){   
      func.apply(context，args) 
    },delayTime)
  }
}


btn.addEeventListener('click',debounce(sendMessage,1000)
```



综上，**防抖函数完成**





防抖：保证会被频繁触发的事件最终只会执行最后一次

节流：控制会被频繁触发的事件的触发频率