# Ajax案例



## 验证邮箱唯一性

1. 获取文本框，添加onblur事件

2. 正则表达式验证用户输入内容是否符合规则

   若符合，则发送验证请求给服务器判断唯一性

   若不符合，阻止程序向下进行并给出提示

3. 发送Ajax请求，检测地址是否已经被注册

4. 将获取的提示展示给用户

```html
邮箱地址: <input type="text" id="email">
<p id="message"></p>


<script src="./js/ajaxFunction.js"></script>
<script>
  var email = document.getElementById('email')
  var msg = document.getElementById('message')
  
  email.onBlur = function(){
    var emailVal = this.value
    // 正则验证
    var reg = /^[a-zA-Z\d]+([-_.][a-zA-Z\d]+)*@([a-zA-Z\d]+[-_.])+[a-zA-Z\d]{2,4}$/;
    // 不符合正则时
    if(!reg.test(emailVal)){
      msg.innerHTML = '错误，邮箱格式不符合规则';
      msg.className = 'bg-danger';
      return;
    }
    // 符合正则时
    // 调用封装的ajax函数
    ajax({
      type: '请求方式',
      url: '请求地址',
      data: {
        请求参数: emailVal
      },
      success: function(res){
        console.log(res);
      	msg.innerHTML = res.message;
      	msg.className = 'bg-success';
      },
      error: function(err){
        console.log(err);
        msg.innerHTML = err.message;
      	msg.className = 'bg-danger';
      }
    })
  }
  
</script>
```





## 搜索框查询提示

1. 获取搜索框，并添加oninput事件
2. 获取用户输入内容
3. 发送Ajax请求，将输入内容作为请求参数
4. 获取响应数据，展示到页面

```html
<input type="text" id="search">
<ul id="list">
	<li></li>
</ul>


<script src="./js/ajaxFunction.js"></script>
<script src="./js/art-template.js"></script>
<script type="text/html" id="art1">
		{{each list}}
		<li>{{$value}}</li>
		{{/each}}
</script>
<script>
	var search = document.getElementById('search');
  var list = document.getElementById('list');
  
  search.oninput = function(){
    var keyword = this.value;
    
    // 调用封装的ajax函数
    ajax({
      type: '请求方式',
      url: '请求地址',
      data: {
        请求参数: keyword
      },
      success: function(res){
        console.log(res);// 一般是个数组
      	var artHTML = template('art1', {list: res});
      	list.innerHTML = artHTML;
      	list.style.display = 'block';
      },

    })
  }
</script>
```

---

### 注意点 1：隐藏关键字

用户情况输入框后，查询的提示应该隐藏

又因为输入内容为空不需要发送无意义请求，return阻止程序向下

```js
if(search.value.trim().length==0){
  list.style.display = 'none';
  return;
}
```

---

### 注意点 2：减少无意义请求

因为使用了oninput事件，每次输入内容就会发送一次Ajax请求，

应该是等用户全输入完后再统一发送

解决方法：

用户连续输入的间隔很短，可以延迟等输入完后再发送Ajax

即通过一个延时定时器包裹Ajax请求

```js
var timer = null;

xxx.oninput = function(){
  clearTimeout(timer);
  timer = setTimeout(function(){
    ajax()
  }, 500)
}
```

---

### 最终完整版

```html
<input type="text" id="search">
<ul id="list">
	<li></li>
</ul>


<script src="./js/ajaxFunction.js"></script>
<script src="./js/art-template.js"></script>
<script type="text/html" id="art1">
		{{each list}}
		<li>{{$value}}</li>
		{{/each}}
</script>
<script>
	var search = document.getElementById('search');
  var list = document.getElementById('list');
  var timer = null;
  
  search.oninput = function(){
    var keyword = this.value;
    
    // 判断输入框是否为空
    if(keyword.trim().length==0){
       list.style.display = 'none';
       return;
    };
    
    // 延时发送
    timer = setTimeout(function(){
      clearTimeout(timer);
      // 调用封装的ajax函数
    	ajax({
      	type: '请求方式',
      	url: '请求地址',
      	data: {
        	请求参数: keyword
      	},
      	success: function(res){
        	console.log(res);
      		var artHTML = template('art1', {list: res});
      		list.innerHTML = artHTML;
      		list.style.display = 'block';
      	}
    	})
    }, 500)
  }
</script>
```





## 省市区三级联动

输入省，会出现市的列表，输入市，会出现区的列表

![](https://bbsmax.ikafan.com/static/L3Byb3h5L2h0dHAvaW1hZ2VzMjAxNS5jbmJsb2dzLmNvbS9ibG9nLzEwMDM4NDIvMjAxNjEyLzEwMDM4NDItMjAxNjEyMjEwMTQ4MzA2MjAtMTY1NDEwOTM0MS5wbmc=.jpg)

1. 发送Ajax请求，先获取**省**的列表，并渲染

2. 获取下拉选择框元素，添加onchange事件
3. 用户选择**省**时，发送Ajax请求携带对应id请求参数，获取市列表，并渲染
4. 用户选择**市**时，发送Ajax请求携带对应id请求参数，获取区列表，并渲染

```html
<select id="province">
  <option>--选择省--</option>
</select>
<select id="city">
  <option>--选择市--</option>
</select>
<select id="area">
  <option>--选择区--</option>
</select>


<script src="./js/ajaxFunction.js"></script>
<script src="./js/art-template.js"></script>
<!--省的模版-->
<sctip type="text/html" id="provinceTemp">
  <option>--选择省--</option>
	{{each provinceList}}
  <option value="{{$value.id}}">{{$value.name}}</option>
  {{/each}}
</sctip>
<!--市的模版-->
<sctip type="text/html" id="cityTemp">
  <option>--选择市--</option>
	{{each cityList}}
  <option value="{{$value.id}}">{{$value.name}}</option>
  {{/each}}
</sctip>
<!--区的模版-->
<sctip type="text/html" id="areaTemp">
  <option>--选择区--</option>
	{{each areaList}}
  <option value="{{$value.id}}">{{$value.name}}</option>
  {{/each}}
</sctip>
<script>
  var province = document.getElementById('province');
  var city = document.getElementById('city');
  var area = document.getElementById('area');
  
  // 调用封装的ajax函数, 先渲染省
  ajax({
    type: 'get',
    url: '请求地址',
    success: function(res){
      console.log(res)// 一般是个数组
      var provinceHTML = template('provinceTemp',{ provinceList: res});
      province.innerHTML = provinceHTML;
    }
  })
  
  // 选择省时
  province.onchange = function(){
    // 获取省ID
    var provinceID = this.value;
    // 调用封装的ajax函数
    ajax({
      type: '请求方式',
    	url: '请求地址',
      data: {
        请求参数: provinceID
      }
    	success: function(res){
      	console.log(res)// 一般是个数组
      	var cityHTML = template('provinceTemp',{ cityList: res});
      	city.innerHTML = cityHTML;
    	}
    })
  };
  
  // 选择市时
  city.onchange = function(){
    // 获取市ID
    var cityID = this.value;
    // 调用封装的ajax函数
    ajax({
      type: '请求方式',
    	url: '请求地址',
      data: {
        请求参数: cityID
      }
    	success: function(res){
      	console.log(res)// 一般是个数组
      	var areaHTML = template('provinceTemp',{ areaList: res});
      	area.innerHTML = areaHTML;
    	}
    })
  }
</script>
```

---

### 注意点：清空区列表

当用户切换省份时，要清空区的列表内容，

不能直接清空容器，而是要给模版绑定一个空字符串数据

```js
province.onchange= function(){
  // 清空区列表
  var areaHTML = template('areaTemp','');
  area.innerHTML = areaHTML;
  
  // 调用Ajax渲染省列表
  ajax()
}
```

---

### 最终完整版

```html
<select id="province">
  <option>--选择省--</option>
</select>
<select id="city">
  <option>--选择市--</option>
</select>
<select id="area">
  <option>--选择区--</option>
</select>


<script src="./js/ajaxFunction.js"></script>
<script src="./js/art-template.js"></script>
<!--省的模版-->
<sctip type="text/html" id="provinceTemp">
  <option>--选择省--</option>
	{{each provinceList}}
  <option value="{{$value.id}}">{{$value.name}}</option>
  {{/each}}
</sctip>
<!--市的模版-->
<sctip type="text/html" id="cityTemp">
  <option>--选择市--</option>
	{{each cityList}}
  <option value="{{$value.id}}">{{$value.name}}</option>
  {{/each}}
</sctip>
<!--区的模版-->
<sctip type="text/html" id="areaTemp">
  <option>--选择区--</option>
	{{each areaList}}
  <option value="{{$value.id}}">{{$value.name}}</option>
  {{/each}}
</sctip>
<script>
  var province = document.getElementById('province');
  var city = document.getElementById('city');
  var area = document.getElementById('area');
  
  // 调用封装的ajax函数, 先渲染省
  ajax({
    type: 'get',
    url: '请求地址',
    success: function(res){
      console.log(res)// 一般是个数组
      var provinceHTML = template('provinceTemp',{ provinceList: res});
      province.innerHTML = provinceHTML;
    }
  })
  
  // 选择省时
  province.onchange = function(){
    // 清空区列表
  	var areaHTML = template('areaTemp','');
  	area.innerHTML = areaHTML;
    
    // 获取省ID
    var provinceID = this.value;
    // 调用封装的ajax函数
    ajax({
      type: '请求方式',
    	url: '请求地址',
      data: {
        请求参数: provinceID
      }
    	success: function(res){
      	console.log(res)// 一般是个数组
      	var cityHTML = template('provinceTemp',{ cityList: res});
      	city.innerHTML = cityHTML;
    	}
    })
  };
  
  // 选择市时
  city.onchange = function(){
    // 获取市ID
    var cityID = this.value;
    // 调用封装的ajax函数
    ajax({
      type: '请求方式',
    	url: '请求地址',
      data: {
        请求参数: cityID
      }
    	success: function(res){
      	console.log(res)// 一般是个数组
      	var areaHTML = template('provinceTemp',{ areaList: res});
      	area.innerHTML = areaHTML;
    	}
    })
  }
</script>
```

