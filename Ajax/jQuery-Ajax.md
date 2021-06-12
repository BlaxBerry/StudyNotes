# jQuery中的Ajax

浏览器提供的原生Ajax是XMLHttpRequest

但是太复杂

jQUery对其进行了封装提供了Ajax相关的函数



## $.get()

专门用来发送get请求，从浏览器请求获取数据

```js
$.get(URL, [params], [callback])
```

- URL：字符串形式的请求资源的url地址

- parmas：请求时携带的参数

- callback：请求成功时的回调函数

---

### 发起不带参数的请求

```js
$.get(
  'http://www.xxxx', 
  function(res){
    // 获取服务器响应返回的数据
    console.log(res)
  }
)
```

```html
<button id="getNoParams">不带参数的get请求</button>

<script>
  $(function () { 
    
    $('#getNoParams').on('click',function () {
      
      $.get(
        'http://www.liulongbin.top:3006/api/getbooks', 
        function(res){
          console.log(res);
        }
      )
      
    })
	})
</script>
```

---

### 发起带参数的请求

```js
$.get(
  'http://www.xxxx', 
  {属性名: 数据}, 
  function(res){
    // 获取服务器响应返回的数据
    console.log(res)
  }
)
```

```html
<button id="getWithParams">带参数的get请求</button>

<script>
  $(function () { 
    
    $('#getwithParams').on('click',function () {
      
      $.get(
        'http://www.liulongbin.top:3006/api/getbooks', 
        {id: 2}, 
        function(res){
          console.log(res);
        }
      )
      
    })
	})
</script>
```





## $.post()

专门用来发起post请求，向服务器提交数据

```js
$.post(URL, [params], [callback])
```

- URL：字符串形式的请求资源的url地址

- parmas：请求时携带的参数

- callback：请求成功时的回调函数

```js
$.post(
  'http://XXXX',
  {属性: 值},
  function(res){
    console.log(res)
  }
)
```

```html
<button id="add">新增</button>

<script>
	const book = {
    bookname: "金瓶梅",
    author: "兰陵笑笑生",
    publisher: "无",
  };

  $(function () {
    
    $("#add").on("click", function () {
      
      $.post(
        "http://www.liulongbin.top:3006/api/addbook",
        book,
        function (res) {
          console.log(res);
        }
      );
      
    }); 
    
  }); 
</script>       
```





## $.ajax()

比起单一功能的$.get() $.post()

$.ajax()功能更综合，可以对AJax做更详细的配置

```js
$.ajax({
  // 请求方式， GET / POST
  type: '',
  // 请求地址
  url: '',
  // 请求时携带的参数
  data: {属性名: 值},
  // 请求成功时的回调函数
  success: function(res){
  	concole.log(res)
  }
})
```

---

### 不携带参数的GET请求

```js
$.ajax({
  type: 'GET',
  url: 'http://XXXXX',
  success: function(res){
  	concole.log(res)
  }
})
```

```html
<button id="getNoParams">不带参数的get请求</button>

<script>
  $(function () { 
    
    $('#getNoParams').on('click',function () {
      
     $.ajax({
       type: "GET",
       url: "http://www.liulongbin.top:3006/api/getbooks",
       success: function (res) {
         console.log(res);
       }
     });
      
    })
	})
</script>
```

---

### 携带参数的GET请求

```js
$.ajax({
  type: 'GET',
  url: 'http://XXXXX',
  data: {属性: 值}
  success: function(res){
  	concole.log(res)
  }
})
```

```html
<button id="getWithParams">带参数的get请求</button>

<script>
  $(function () { 
    
    $('#getwithParams').on('click',function () {
      
     $.ajax({
       type: "GET",
       url: "http://www.liulongbin.top:3006/api/getbooks",
       data:{id:2},
       success: function (res) {
         console.log(res);
       }
     });
      
    })
	})
</script>
```

---

### POST请求

```js
$.ajax({
  type: 'POST',
  url: 'http://XXXXX',
  data: {属性: 值}
  success: function(res){
  	concole.log(res)
  }
})
```

```html
<button id="add">新增</button>
<script>
  const book = {
    bookname: "金瓶梅",
    author: "兰陵笑笑生",
    publisher: "无",
  };

  $(function () {
    
    $("#add").on("click", function () {
      
      $.ajax({
        type: "POST",
        url: "http://www.liulongbin.top:3006/api/addbook",
        data: book,
        success: function (res) {
          console.log(res);
        }
      });
      
    });
    
	})
</script>
```





## 数据渲染页面

```html
   <table>
        <thead>
            <tr>
                <td>index</td>
                <td>id</td>
                <td>name</td>
                <td>author</td>
                <td>publisher</td>
                <td>detele</td>
            </tr>
        </thead>
        <tbody></tbody>
    </table>

<script>
  $(function () { 
    
    // 定义页面渲染函数
    function renderList(){
       $.ajax({
         type: "GET",
         url: "http://www.liulongbin.top:3006/api/getbooks",
         success: function (res) {
           
           if(res.status!==200){return alert('数据获取失败')};
           let list = [];
           $.each(res.data, function(index, item){
             list.push(`
								<tr>
 										<td>${index + 1}</td>
                    <td>${item.id}</td>
                    <td>${item.bookname}</td>
                   	<td>${item.author}</td>
                    <td>${item.publisher}</td>
                    <td>
											<a href="javascript:;" 
												 data-id="${item.id}">
											删除
  										</a>
  									</td>
  							</tr>
						`)
           });
           $('tbody').empty().append(list)
       }
     })
      
    };
    
    // 页面初始化
    renderList();
    
    // 删除功能
    // 通过代理，给动态生成的子元素绑定事件
    $("tbody").on("click", "a", function () {
      // console.log($(this).attr("data-id"));
      if (res.status !== 200) {alert("删除图书失败")}
      $.ajax({
        type: "GET",
        url: "http://www.liulongbin.top:3006/api/delbook",
        data: { id: $(this).attr("data-id") },
        success: function (res) {
          // 调用页面渲染
          renderList();
        },
      });
    });
    
    // 添加功能
    $("#addBtn").on("click", function () {
      let bookname = $(".name").val().trim();
    	let author = $(".author").val().trim();
    	let publisher = $(".publisher").val().trim();

    	// console.log(bookname, author, publisher);
    	if (bookname <= 0 || author <= 0 || publisher <= 0) {return alert("请填完整")} 
        
      $.ajax({
        type: "POST",
        url: "http://www.liulongbin.top:3006/api/addbook",
        data: {
          bookname: bookname,
          author: author,
          publisher: publisher,
        },
        success: function (res) {
          if (res.status !== 200) {alert("添加图书失败")} 
          // 调用页面渲染  
          renderList();         
        },
        
      });

  	});
    
	})
</script>
```





## 文件上传

### 判断是否上传

通过原生JS的DOM属性 **files**判断是否添加了上传的文件

- **DOM对象.files** : 返回的是一个对象

- **DOM对象.files.length属性** : 判断是否有上传的文件

```js
console.log(JSNode.files)
// FileList {0: File, length: 1}

JSNode.files.length <= 0  // 没有上传的文件
```

因为是原生DOM的方法，所以需要从jQuery对象转换为DOM对象

- **jQuery对象[0].files.length**  

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

### 发送上传文件请求

1. 将获取的上传文件加入FormData对象中
2. 然后通过POST请求发送Ajax请求

```js
var fd = new FormData()
fd.append('自定义名', $("文件域标签")[0].files[0])

// 然后将fd FormData对象发送Ajax
ajax(fd)
```

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





## Loading效果

通过document文档下的 **ajaxStart()** 和 **ajaxStop()**

- **ajaxStart(function () {});**

  自动监听页面中 所有被发起的Ajax请求

```js
$(document).ajaxStart(function () {
  图片.show();
});
```

- **ajaxStop(function () {});**

  自动监听页面中 所有结束的Ajax请求

```js
$(document).ajaxStop(function () {
  图片.hide();
});
```

如下：

监听页面中的Ajax请求的发起和结束，

并分别控制Loading的**Gif图片**的显示和隐藏

```html
<body>
  <input type="file" />
  <div><button>上传</button></div>
  
  <!-- Loading gif -->
  <img src="./loading-gif.gif" alt="" style="display: none" id="loading"/>
</body>


<script>
    $(function () {
      $("button").on("click", function () {
        var fileData = new FormData();
        fileData.append("pic", $("input")[0].files[0]);
        fun(fileData);
      });
      
      function fun(fileData) {
        $.ajax({
          type: "POST",
          url: "http://www.liulongbin.top:3006/api/upload/avatar",
          data: fileData,
          contentType: false,
          processData: false,
          success: function (res) {
            console.log(res);
          },
        });
      }

      // 自动监听页面中 被发起的Ajax请求
      $(document).ajaxStart(function () {
        $("#loading").show();
      });
      
      // 自动监听页面中 结束的Ajax请求
      $(document).ajaxStop(function () {
        $("#loading").hide();
      });

    });
</script>
```