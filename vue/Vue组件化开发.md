# Vue组件 Components

![](https://img10.360buyimg.com/uba/jfs/t17725/89/1051890999/28032/d2a32f5e/5ab8fe1aN70a87b81.png)

组件主要特点体现在各个组件的组合、重用

尽可能把页面内容拆成一个个的独立可复用的小组件

比如把头部、导航，底部等部分拆分，组合





## 全局组件

### 注册

```js
Vue.component(组件名，{
		data: function(){
  			组件数据名：值
		}，
    template：组件模版内容
})
```

如下：

注册一个demo组件，并在页面中使用

```html
<div id="app">
  
		<demo></demo>

    <div>
       <demo></demo>
    </div>
</div>

<script>
        Vue.component("demo", {
            template: "<h2>i am H2 tag</h2>"
        })
        let app = new Vue({
            el: "#app",

        })
</script>
```

再如下：

注册一个按钮组件，并添加点击事件

```html
<div id="app">
  <btn-counter></btn-counter>
</div>

<script>
 Vue.component('btn-counter'，{
		 data:function(){
  			 return {
           num：0
         }
		 }，
     template：'<button @click="num++">点击了 {{num}}）次</button>'
 });
  
  new Vue({
    el:'#app'
  })
</script>
```



### 组件内部属性

#### data属性

**data是个函数**，用**return**返回对象形式的数据，数据是键值对形式

虽然Vue实例也是个组件，但Vue实例对象不同的是，

Vue实例的data属性是个对象，

而组件的data是个函数，返回值是个对象

```js
Vue.component('helloComponent',{
	data:function(){
    return {
      message:'hello',
      arr:[1,2,3]
    }
  }
})
```

---

#### template属性

**template 是字符串形式的模版**

建议使用ES6的模版字符串，使模版结构清晰

- **模版必须是单一的根元素**，不能是同级标签

（不能是并列的兄弟元素，只能是单个元素，但可以嵌套多个同级的子元素）

```js
Vue.component('hello', {
  template: `
            <div>
                <h3>hello the 2sd</h3>
                <demo></demo>
                <demo></demo>
            </div>`
});
```

- template模版可以直接使用组件的data属性中的数据

```js
Vue.component('hello', {
  data:function(){
    return {
      str:'hello',
      arr:['tom','jerry']
    }
  },
  template: '<h1>{{str}} {{arr[0]}} and {{arr[1]}}</h1>'
});
```

---

#### 其他属性

组件内部也可以写其他的属性，比如methods等属性

```js
Vue.component("demo", {
  data: function() {
    return {
      count: 0
    }
  },
  template: `
            <div>
                <button @click="handle">clicked {{count}} times</button>
            </div>
            `,
  methods: {
    handle: function() {
      this.count++
    }
  }
});
```



### 组件命名

#### 短横线

推荐

```js
Vue.component('my-component',{ })
```

#### 驼峰命名

不推荐

只能用于其他组件的字符串模版拼接

不能直接作为根组件标签使用到页面，

不然会报错

驼峰命名法命名的组件在页面中使用时可写成短横线格式

```js
Vue.component('myComponent',{ })
```





### 组件使用

全局组件要写入一个容器，不能直接在页面中使用，不然不会被解析，

且该容器必须被Vue实例对象挂载

组件使用时直接在页面中以标签的形式使用组件，

标签名就是组件名，如果注册时时驼峰命名，可在使用时写为短横线

```html
<body>
  <div id="app">
    <组件名1></组件名1>
    <组件名2></组件名2>
  </div>
  
  <script>
  	new Vue({
      el:"#app"
    });
    
    Vue.component('组件名1'{ })；
    Vue.component('组件名2'{ })
  </script>
</body>
```

---

#### 组件复用

组件可以重复使用 ，

每个组件的数据都是独立互不影响的

- **复用在同一容器内**

```html
<div id="app">
        <h3>hello the 1st</h3>
        <demo></demo>
        <demo></demo>
        <hello></hello>
</div>
<script>
  Vue.component("demo", {
    data: function() {
      return {
        count: 0
      }
    },
    template: `
					<div>
						<button @click="handle">clicked {{count}} times</button>
  				</div>
            `,
    methods: {
      handle: function() {
        this.count++
      }
    }
  });
  
	new Vue({
    el: "#app",
  })
</script>
```

- **复用在不同容器**

全局组件想被多个容器使用时，多个容器都要挂载Vue

即给所有需要使用的容器都绑定Vue实例

如下：在容器`container11` 和`container22`中都是用全局组件`hello`

需要先给两个容器都挂载上Vue，分定绑定Vue实例对象

```html
<body>
    <div id="container11">
        <hello></hello>
    </div>

    <div id="container22">
        <hello></hello>
    </div>

    <script>
        Vue.component('hello', {
            data: function() {
                return {
                    str: 'hello',
                    arr: ['tom', 'jerry']
                }
            },
            template: '<h1>{{str}} {{arr[0]}} and {{arr[1]}}</h1>'
        });

        new Vue({
            el: "#container11"
        })

        new Vue({
            el: "#container22"
        })
    </script>
</body>
```

---

#### 组件嵌套

一个组件中可以包含其他组件

如下：hello组件中嵌套了demo组件

```html
<div id="app">
  
        <hello></hello>
  
</div>

<script>
  Vue.component("demo", {
    template: `
					<div>
						<button @click="handle">click</button>
  				</div>
            `
  });

  Vue.component('hello', {
    template: `
            <div>
                <h3>hello the 2sd</h3>
                <demo></demo>
                <demo></demo>
            </div>`
     });

	new Vue({
    el: "#app",
  })
</script>
```









## 局部组件

### 注册  components属性

在Vue实例对象的 **`components属性 `**中注册局部组件对象

和全局组件相同

data属性是个函数，用retutn返回对象形式的数据，

template属性是字符串形式的模版

命名规则也是短横线或驼峰，页面中也是只能用短横线格式

```js
new Vue({
  el:'#app',
  components:{
    组件名:{
      data:function(){
    		组件数据名:值
  		},
  		template:组件模版
    }
  }
})
```

也可以在外部定义组件，实例对象内直接调用，是代码整洁

```js
  var componentA = {
  	data:function(){ return {} },
  	template:''
 	}
	var componentB = {
  	data:function(){ return {} },
  	template:''
	}

	new Vue({
  	el:'#app',
  	components:{
    	'component-A':componentA,
    	'component-B':componentB
  	}
	})
```

如下

```html
<div id="app">
  <component-A></component-A>
  <component-B></component-B>
</div>
<script>
  var componentA = {
  	data:function(){ 
    	return {
        msg:'hello Tom'
      }},
  	template:'<h1>{{msg}}</h1>'
 	};
  
	var componentB = {
  	data:function(){ 
    	return {
        msg:'hello Jerry'
      }},
  	template:'<h1>{{msg}}</h1>'
	};

	new Vue({
  	el:'#app',
  	components:{
    	'component-A':componentA,
    	'component-B':componentB
  	}
	})
</script>
```

---

### 和全局组件的区别

全局组件可在任何挂载了Vue的容器内使用

```html
<body>
    <div id="container11">
        <hello></hello>
    </div>

    <div id="container22">
        <hello></hello>
    </div>

    <script>
        Vue.component('hello', {
            data: function() {
                return {
                    str: 'hello',
                    arr: ['tom', 'jerry']
                }
            },
            template: '<h1>{{str}} {{arr[0]}} and {{arr[1]}}</h1>'
        });

        new Vue({
            el: "#container11"
        })

        new Vue({
            el: "#container22"
        })
    </script>
</body>
```

局部组件只能在当前Vue实例对象挂载的容器中使用，

否则会报错找不到组件，不会被页面渲染

```html
<body>
    <div id="container11">
        <My-component></My-component>
      	<My-component></My-component>
    </div>

    <div id="container22">
        <My-component></My-component>   <!--没渲染-->
        <My-component></My-component>
    </div>

    <script>
        new Vue({
            el: '#container11',
            data: {
                str: 'Vue instance'
            },
            components: {
                'MyComponent': {
                    data: function() {
                        return {
                            str: 'component'
                        }
                    },
                    template: '<h1>{{str}}</h1>'
                }
            }
        })
    </script>
</body>
```

---

### data的数据

局部组件不能直接使用定义在Vue实例对象的data中的数据，

局部组件只能使用来自组件内部的data中的数据

验证如下：

`Vue实例对象中的data`内定义一个字符串数据 str，值是 `Vue instance`，

`局部组件内的data`也定义一个字符串数据 str，值是 `component`

组件模版用插值语法插入str，

最终页面渲染的是组件自己的data属性中的数据 str `component`

不是Vue实例对象的data的数据

```html
<body>
    <div id="app">
        <My-component></My-component>
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                str: 'Vue instance'
            },
            components: {
                'MyComponent': {
                    data: function() {
                        return {
                            str: 'component'
                        }
                    },
                    template: '<h1>{{str}}</h1>'
                }
            }
        })
    </script>
</body>
```





## 组件调试工具 devtools

识别页面中的Vue组件

可以更清楚的看到Vue组件的层级关系

并可以直接对Vue组件的数据进行调试

![](https://raw.githubusercontent.com/vuejs/vue-devtools/dev/media/screenshot-shadow.png)

[devtools github](https://github.com/vuejs/vue-devtools)

1. 按照说明提示克隆仓库、安装、构建

2. 打开chrome扩展页面
3. 选中开发者模式
4. 加载已解压的扩展
5. 选中`vue-devtools/packages/shell-chrome/`完成扩展的加载



