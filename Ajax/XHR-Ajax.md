# XMLHttpRequst基础

<img src="https://lpeg.info/images/xmlhttprequest_img1.png" style="zoom:50%;" />

XMLHTTPRequst（简称 **XHR**）是浏览器提供的**JavaScript对象**

专门用于请求服务器的数据资源



jQuery的Ajax函数是以XHR对象为基础封装出来的

jQuery的Ajax函数底层就是使用XMLHttpRequest



如下，使用XMLHttpRequest对象发起网络请求：





## GET请求

步骤：

1. 创建xhr对象

2. 调用 **xhr.open()**函数

3. 调用 **xhr.send()**函数

4. 监听 **xhr.onreadystatechange**事件

   1. 判断属性 **xhr.readyStatus===4&&xhr.status===200** 

      （Ajax请求完成 且 数据传输成功）

   2. 请求数据存储在 **xhr.responseText**属性中

```html
<script>

    var xhr = new XMLHttpRequest();

    xhr.open("GET", "http://www.liulongbin.top:3006/api/getbooks");

    xhr.send();

    xhr.onreadystatechange = function () {
      
      if (xhr.readyState === 4 && xhr.status === 200) {
        
        var res = xhr.responseText;
        console.log(res);
      }
    };
</script>
```

---

### 带参数的GET请求

在 调用xhr.open() 方法时给URL请求地址携带上请求参数

**查询字符串**的形式  **URL?key=value&key=value**

```js
xhr.open('GET','http://xxxxx?key=value&key=value')
```

如下：

```js
var xhr = new XMLHttpRequest();

xhr.open("GET", "http://www.liulongbin.top:3006/api/getbooks?id=1");

xhr.send();

xhr.onreadystatechange = function () {
  
  if (xhr.readyState === 4 && xhr.status === 200) {
    
    var res = xhr.responseText;
    console.log(res);
  }

};
```







## POST请求

步骤：

1. 创建xhr对象

2. 调用 **xhr.open()**函数

3. **xhr.setRequestHeader()**设置请求头的**Content-Type属性**

4. 调用 **xhr.send()**函数，若有参数则指定POST请求参数

5. 监听 **xhr.onreadystatechange**事件

   1. 判断属性 **xhr.readyStatus===4&&xhr.status===200** 

      （Ajax请求完成 且 数据传输成功）

   2. 请求数据存储在 **xhr.responseText**属性中

```js
var xhr = new XMLHttpRequest();

xhr.open("POST", "http://www.liulongbin.top:3006/api/addbook");


xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");


xhr.send("bookname=奥特曼A&author=奥特之父&publisher=M78星云");

xhr.onreadystatechange = function () {
  
  if (xhr.readyState === 4 && xhr.status === 200) {
    
    var res = xhr.responseText;
    console.log(res);
  }
};
```







## xhr.readyStatus

xhr对象的readyStatus属性，是用来表示当前Ajax请求的状态

每一个Ajax请求必有一个状态，共4个

### Ajax请求状态

| 值   | 状态               | 描述                                               |
| ---- | ------------------ | -------------------------------------------------- |
| 0    | `UNSENT`           | xhr对象被创建，但尚未调用 open() 方法              |
| 1    | `OPENED`           | **open() 方法已被调用**                            |
| 2    | `HEADERS_RECEIVED` | send() 方法已经被调用，响应头已被接收              |
| 3    | `LOADING`          | 数据接收中，**responseText**属性已经包含部分数据。 |
| 4    | `DONE`             | **Ajax请求完成**，表示数据传输成功或失败           |

xhr.readyStatus===4时，仅代表当前Ajax请求发送完成

数据的传输可能成功也可能失败



## xhr.status







## 封装Ajax函数

模拟jQuery封装一个Ajax请求函数

```js
// ajax({
//     method: 'GET',
//     url: 'http://www.liulongbin.top:3006/api/getbooks',
//     data: {
//         id: 2
//     },
//     success(res) {
//         console.log(res);
//     }
// })


function resolveData(data) {
    var arr = [];
    for (let key in data) {
        arr.push(key + "=data[key]");
    }
    return arr.join("&");
}


function ajax(option) {

    var xhr = new XMLHttpRequest();

    var query = resolveData(option.data);

    var method = option.method.toUpperCase();


    if (method === 'GET') {
        var url = option.url + '?' + query;

        xhr.open(method, url)
        xhr.send()

    } else if (method === 'POST') {

        xhr.open(method, option.url);
        xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
        xhr.send(query)

    }

    xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {

            var res = JSON.parse(xhr.responseText);

            option.success(res);
        }
    };
}
```







## XMLHttpRequest Level 2

### 新特性

旧版的XMLHttpRequest：

- 仅支持文本数据传输

  无法读取和上传文件

- 没有进度信息

  数据传送和接收只提示是否完成

---

新版XMLHttpRequest Level 2：

- 可以设置HTTP请求时限

- 提供的FormData对象可实现表单数据管理

- 可以上传文件
- 可以获取数据传输的进度信息



### 设置HTTP请求时限

Ajax是会有耗时，如果网速过慢，用户等待时间就会无限延期

新版XMLHttpRequest Level 2新增了：

-  **timeout属性**
-  **timeout事件函数**

用于设置HTTP请求的时限

若在规定时间内内没有响应会数据，则自动停止请求

如下：等待时间为3s

```js
xhr.timeout = 3000
```

如下：通过**timeout事件函数**指定请求超时后的回调函数

```js
xhr.ontimeout = function(e){
  alert('请求超时')
}
```

---

```js
var xhr = new XMLHttpRequest();

xhr.timeout = 3000;

xhr.ontimeout = function (e) {
  alert("请求超时");
};

xhr.open("GET", "http://www.liulongbin.top:3006/api/getbooks");
xhr.send();
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
     var res = xhr.responseText;
     console.log(res);
  }
};
```



### FormData对象获取表单数据

详见笔记Form表单



### 上传文件

#### 判断文件是否上传

**DOM对象.files** 返回的是一个对象

可通过该对象的**length属性**判断是否有上传的文件

**DOM对象.files[0]** 是上传文件，以及相关信息 

```js
console.log(JSNode.files)
// FileList {0: File, length: 1}

JSNode.files.length <= 0  // 没有上传的文件
```

#### 上传头像并显示头像

```html
<body>
  <input type="file"/>
  <button>上传</button>
  <img scr="" alt="" id="avatar">
</body>

<script>
  var btn = document.querySelector('button')
  var files = document.querySelector('input').files
  
	btn.addEventListener('click', function(){
   if( files.length <= 0){
      return alert('请上传文件后再提交')
   }else{
     // alert('上传成功')
     var fd = FormData()
     fd.appemd('pic', files[0])
     
     var xhr = new XMLHttpRequest()
     xhr.open("GET", "http://www.liulongbin.top:3006/api/getbooks");
     xhr.send();
     xhr.onreadystatechange = function () {
       if (xhr.readyState === 4 && xhr.status === 200) {
         var res = xhr.responseText;
         // console.log(res);
         document.getElementByID('avatar').src = JSON.parse(res).imgsrc
       }else{
         console.log(res.message)
       }
     };
   }
</script>
```





### 文件上传进度

#### 上传进度的百分比

监听xhr.upload.onprogress事件

```js
var xhr = new XMLHttpRequest()

// 监听上传的进度
xhr.upload.onprogress = function(e){
  // 判断上传资源是否可计算长度
  if(e.lengthComputable){
    // e.loaded 已经传输的字节
    // e.total 上传总字节
    var percentComplete = Math.cell((e.loaded/e.total)*100)
    
    console.log(percentComplete)
  }  
}
```



#### 进度条

可用Bootstrap实现进度条样式



#### 上传结束

监听xhr.upload.onload事件

```js
xhr.upload.onload = function(){
  $('某标签')
    .removeClass()
    .addClass('完成后的样式类名')  
}
```

