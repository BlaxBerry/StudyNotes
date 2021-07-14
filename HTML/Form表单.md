# <form\>标签

仅用于收集数据，不用来发送数据



## 属性

### action

指向后端提供的URL地址

用于规定表单数据向哪里提交，提交后页面立即跳转到指定页面

若未指定action属性，则默认跳转到当前页面

```html
<form action="http://xxxxxx">
  
</form>
```

---

### target

用于规定在何处发开action属性指向的页面

 **_self**：当前窗口打开，默认

**_blank**：新窗口打开

```html
<form action="http://xxxxxx" target="_blank">
</form>
```

---

### method

用于规定以什么方式提交数据

**get**：通过URL地址提交

```js
http://xxxxxx?key=value&key=value
```

**post**：通过请求体提交，**最常用**

可通过浏览器开发工具的network查看

---

### enctype

用于规定发送数据之前如何进行编码

- **application/x-www-form-urlencoded** 默认

  发送前编码所有字符

- **multipart/form-data**

  不对自负进行编码

  文件上传时，必须通过此属性将上传数据转为二进制

```html
<form action="http://???" method="POST" enctype="multipart/form-data">
  
</form>
```







## 同步提交

通过表单的 **submit** 按钮提交数据到action属性指向的URL

- 数据提交后整个**页面会发生跳转**，用户体验差

- 同步提交后的**页面的状态和数据会丢失**，用户体验差

所以表单只用来收集数据，

数据的提交发生应该通过 **Ajax** 发送给服务器





## jQuery监听表单提交

监听表单的通过 **submit** 按钮的提交

```js
$('form').submit(function(e){
  console.log('提交了')
})


$('form').on('submit', function(e){
  console.log('提交了')
})
```





## jQuery阻止默认行为

只要按了submit按钮，表单就必然会产生数据提交和页面跳转

因为时通过Ajax发送数据，表单仅用来收集，

所以需要阻止默认的提交和跳转

通过事件对象阻止默认行为

```js
$('form').submit(function(e){
  e.preventDefault()
})


$('form').on('submit', function(e){
  e.preventDefault()
})
```





## serialize() 获取表单数据

常规操作是通过DOM操作分别通过value值获取，但是不方便

jQuery提供了**serialize函数**，可一次性获取表单的所有数据

```js
$('选择器').serialize()
```

通过**serialize函数** 获取表单数据时，每个表单必须要有name属性

返回值是key-value的键值对形式

```js
表单的name=表单的数据
```

```html
<body>
    <form action="">
        <span>name</span><input type="text" name="username"><br>
        <span>age</span><input type="text" name="userage"><br>
        <button type="submit">send</button>
    </form>
</body>
<script>
    $(function () {

        $('form').on('submit', function (e) { 
            e.preventDefault();
            console.log( $(this).serialize());
          //username=andy&userage=28
         })

      })
</script>
```





## reset() 清空表单数据

原生form表单的JS的方法

jQuery使用时需要转换为DOM对象后使用

```html
<body>
    <form action="">
        <span>name</span><input type="text" name="username"><br>
        <span>age</span><input type="text" name="userage"><br>
        <button type="submit">send</button>
    </form>
</body>
<script>
    $(function () {

        $('form').on('submit', function (e) { 
            e.preventDefault();
            
            $(this)[0].reset()
         })

      })
</script>
```





## FormData对象

HTML5新增了一个FormData对象用于操作表单

---

### 创建表单数据

手动创建表单数据，并发送Ajax请求：

```js
var fd = new FormData();

// 添加表单项
fd.append("username", "Andy");
fd.append("userage", "28");

var xhr = new XMLHttpRequest();
xhr.open("POST", "http://www.liulongbin.top:3006/api/formdata");

xhr.send(fd);

xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    var res = xhr.responseText;
    console.log(JSON.parse(res));
  }
};
```

---

### 获取表单数据

获取页面中表单的数据，并发送Ajax请求：

```js
var form = document.querySelector("form");

form.addEventListener("submit", function (e) {
  e.preventDefault();

  
  var fd = new FormData();
  var xhr = new XMLHttpRequest();
  
  xhr.open("POST", "http://www.liulongbin.top:3006/api/formdata");
  xhr.send(fd);
  xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && xhr.status === 200) {
      var res = xhr.responseText;
      console.log(JSON.parse(res));
    }
  };
});
```





## 文件上传

### 判断是否上传

通过原生JS的DOM属性 **files**判断是否添加了上传的文件

**DOM对象.files** 返回的是一个对象

可通过该对象的**length属性**判断是否有上传的文件

**DOM对象.files[0]** 是上传文件，以及相关信息 

```js
console.log(JSNode.files)
// FileList {0: File, length: 1}

JSNode.files.length <= 0  // 没有上传的文件
```

因为是原生DOM的方法，所以需要从jQuery对象转换为DOM对象

```html
<body>
  <input type="file"/>
  <button>上传</button>
</body>

<script>
$(function(){
  $('button').on('click', function(){
   if( $('input')[0].files.length <= 0){
      return alert('请上传文件后再提交')
   }else{
     alert('上传成功')
   }
  })
})
</script>
```

### jQuery发送Ajax请求

```html
<body>
  <input type="file" />
  <button>上传</button>
</body>
 
<script>
    $(function () {
      $("button").on("click", function () {
        // console.log($("input")[0].files[0]);

        var fileData = new FormData();
        fileData.append("pic", $("input")[0].files[0]);
       
        // 调用Ajax发送文件
        fun(fileData)
      });


      function fun(fileData) {
        $.ajax({
          type: "POST",
          url: "http://www.liulongbin.top:3006/api/upload/avatar",
          data: fileData,

          // 不修改ContentType属性，使用FormData默认的ContentType属性
          contentType: false,
          // 不对FormData中的数据进行编码，将数据（文件）直接发送到服务器
          processData: false,
          
          success: function (res) {
            console.log(res);
          },
        });
      }
    });
</script>
```