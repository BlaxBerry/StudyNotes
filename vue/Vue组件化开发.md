# Vue组件 Components

![](https://img10.360buyimg.com/uba/jfs/t17725/89/1051890999/28032/d2a32f5e/5ab8fe1aN70a87b81.png)

组件主要特点体现在各个组件的组合、重用

尽可能把页面内容拆成一个个的独立可复用的小组件

比如把头部、导航，底部等部分拆分，组合

[TOC]



## 全局组件

### 注册

```js
Vue.component('组件名',{
  data:function(){
    return {
      组件数据名:值
    }
  },
    template:组件模版内容
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

局部组件只能在当前组件的父组件中使用，

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



## 父子组件

在实例对象的components属性中定义父组件

在父组件内再定义components属性并在其中定义子组件





## 组件间的数据交互  

因为组件只能用该组件自己的data属性中定义的数据，

不能直接用其父组件中的数据

---

### 父组件 —> 子组件

#### props属性值 和 模版的自定义属性

1. **把父组件的数据作为自组件的自定义属性，写在子组件标签上**

2. **然后在子组件内通过props属性获得这个自定义属性**

```html
<!--静态数据-->
<子组件 自定义属性名=‘hello’></子组件>

<!--v-bind绑定动态数据-->
<子组件 :自定义属性名='数据名'></子组件>
```

```js
//局部组件
components:{
  子组件:{
    props:['自定义属性名']，
    template:'<div>{{自定义属性名}}</div>'
  }
}

//全局组件
Vue.component('组件名',{
  props:['自定义属性名']，
  template:'<div>{{自定义属性名}}</div>'
})
```

如下：

所以，若子组件想使用父组件内的数据str和info，

需要先把数据作为子组件child的自定义属性，用v-bind写入标签

然后再在子组件内通过props属性，用数组方式获取该自定义属性（父组件数据），然后就可以在子组件内使用父组件的数据了

```html
<!--局部组件中的父传数据给子-->
<body>
    <div id="app">
        <child :str='str' :info='info' ></child>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                msg: 'i am father',
                str: 'hello'，
              	info: 'from father'
              
            },
            components: {
                "child": {
                    props: ['str','info'],
                    data: function() {
                        return {
                            msg: 'i am child'
                        }
                    },
                    template: '<h1>{{msg}},{{str}},{{info}}</h1>'
                }
            }
        })
    </script>
</body>
```

```html
<!--全局组件中的父传数据给子-->
<body>
    <div id="app">
        <child :str='str' :info='info'></child>
    </div>

    <script>
        Vue.component('child', {
            props: ['str','info'],
          	data:function(){
              return {
                msg:'i am child'
              }
            },
            template: '<h1>{{msg}},{{str}},{{info}}</h1>'
        });

        new Vue({
            el: '#app',
            data: {
                msg: 'i am father',
                str: 'hello',
                info:'from father'
            }
        })
    </script>
</body>
```

---

#### 传递多个数据（属性）

传递的数据有两种：静态的写死的数据 和 绑定到Vue的动态数据

传递时数据可以是多个，作为数组的多个元素即可

```js
props:['自定义属性名','自定义属性名',....]
```

```html
<子组件 :自定义属性名='Vue数据名' 自定义属性名='静态数据'></子组件>
```

---

#### 数据和自定义属性的命名

因为JavaScript规则，在props数组中接受的的属性名只能是驼峰命名

因为HTML标签不区分大小写，页面标签中的属性名只能是短横线格式

但是在字符串的模版中，可以使用短横线格式的属性名

```html
<!--局部组件中的父传数据给子-->
<body>
    <div id="app">
        <child :child-str='str' :child-info='info' ></child>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                msg: 'i am father',
                str: 'hello'，
              	info: 'from father'
              
            },
            components: {
                "child": {
                    props: ['childStr','childInfo'],
                    data: function() {
                        return {
                            msg: 'i am child'
                        }
                    },
                    template: '<h1>{{msg}},{{child-str}},{{child-info}}</h1>'
                }
            }
        })
    </script>
</body>
```

---

#### props属性值类型

- 字符串型数据
- 数值型数据
- 数值型数据
- 对象型数据
- 布尔型数据

自定义属性绑定的布尔值和数值数据若不是用v-bind设定给子组件的模版的话，

子组件内props属性接受到的都是字符串

所以除了写死的静态数据外，绑定自定义属性时必须用v-bind绑定

```html
<body>
    <div id="app">
        <child :str='str' string-num='12' :num='12' string-bool=true :bool=true :arr='arr' :obj='obj'></child>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                str: 'hello',
                arr: [1, 2, 3, 'A', 'B'],
                obj: {
                    name: 'andy',
                    age: 28
                }
            },
            components: {
                "child": {
                    props: ['str', 'stringNum', 'num', 'stringBool', 'bool', 'arr', 'obj'],
                    template: `
                        <div>
                            <div>{{str + '----' + typeof str}}</div>
                            <div>{{stringNum + '----' + typeof stringNum}}</div>
                            <div>{{num + '----' + typeof num}}</div>
                            <div>{{stringBool+ '----' + typeof stringBool}}</div>
                            <div>{{bool+ '----' + typeof bool}}</div>

                            <ul>
                                <li :key='index' v-for="item,index in arr">{{index}}--{{item}}</li>    
                            </ul>

                            <div>
                                <span>{{obj.name}}</span>   
                                <span>{{obj.age}}</span>     
                            </div>
                        </div>
                    `
                }
            }
        })
    </script>
</body>
```



![](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=4256217190,2885724795&fm=11&gp=0.jpg)



### 子组件——>父组件

#### 通过props直接修改数据（不推荐）

子组件可以直接操作父组件的数据，虽然不报错，**但是不推荐**

不建议不推荐直接在子组件内修改通过props获取的父组件的数据

```html
<!--先把父组件的数据，一个数组传递给子组件
子组件v-for遍历数组
并子组件内绑定一个事件，改变数组数据，并传递回 父组件-->

<body>
    <div id="app">
        <h3>array in father: {{arr}}</h3>
        <child :arr='arr'></child>
    </div>

    <script>
        new Vue({
            el: "#app",
            data: {
                arr: ['apple', 'pear', 'banana']
            },
            components: {
                'child': {
                    props: ['arr'],
                    template: `
                        <div>
                            <ul>
                                <li v-for='item in arr'>{{item}}</li>    
                            </ul>
                            <button @click='add'>click to add</button>    
                        </div>
                    `,
                    methods: {
                        add() {
                            this.arr.push('lemon')
                        }
                    }
                }
            }
        })
    </script>
</body>
```

因为**props传递数据的原则是单项的**

仅允许父组件的数据传递给子组件，但是子组件不能直接操作修改到父组件，

不然会容易出现逻辑错误

所以不建议不推荐直接在子组件内修改通过props获取的父组件的数据

应该通过 `$emit`在子组件内操作父组件的数据

---

---

#### $emit() 修改数据

若想在子组件内操作父组件的数据

1. **子组件通过自定义事件向父组件传递信息**

```js
Vue.component('子组件名',{
  template:`<子组件 v-on:事件类型='$emit(“自定义事件”)'></子组件>`
})
```

2. **在父组件内给子组件绑定自定义事件**

```html
<父组件>
  <子组件 v-on:自定义事件=‘修改父组件数据的逻辑’></子组件>
</父组件>
```

可理解为：

子组件内触发自定义事件——>传递事件给父组件——>父组件内修改数据

如下：

子组件内操作父组件data属性中的数据`fontSize`

用Vue实例对象模拟父子组件，Vue实例对象也是个组件，所以挂载Vue的容器可视为父组件

注册子组件，并通过 `$emit()` 自定义事件传给父组件

Vue挂载的容器即父组件中，给子组件的模版绑定该自定义事件和父组件的数据

即可通过子组件的自定义事件操作父组件的数据了

```html
<body>
    <div id="app">
        <div :style="{fontSize: fontSize +'px'}">from father---{{msg}}</div>
        <child @enlarge-size='handle'></child>
    </div>

    <script>
        Vue.component('child', {
            template: `
						<button @click="$emit('enlarge-size')">click fontSize +1</button>
						`
        });
      
        new Vue({
            el: '#app',
            data: {
                msg: 'hello',
                fontSize: 14
            },
            methods: {
                handle() {
                    this.fontSize += 1
                }
            }
        })
    </script>
</body>
```

---

#### $emit() 传递数据

**子组件通过自定义事件的参数向父组件传递数据**

```js
Vue.component('子组件名',{
  template:`<子组件 v-on:事件类型='$emit(“自定义事件”,数据)'></子组件>`
})
```

#### $event 接受传递的数据

父组件中给自组件绑定自定义事件，并通过`$event`接受自组件传递来的数据

`$event`可作为参数传递给定义在父组件methods中的方法函数

```html
<父组件>
  <子组件 v-on:自定义事件=‘修改父组件数据的逻辑($event)’></子组件>
</父组件>
```

如下：

```html
<body>
    <div id="app">
        <div :style="{fontSize: fontSize +'px'}">from father---{{msg}}</div>
        <child @enlarge-size='handle($event)'></child>
    </div>

    <script>
        Vue.component('child', {
            template: `
						<button @click="$emit('enlarge-size',10)">click fontSize +1</button>
						<button @click="$emit('enlarge-size',40)">click fontSize +1</button>
						`
        })
        new Vue({
            el: '#app',
            data: {
                msg: 'hello',
                fontSize: 14
            },
            methods: {
                handle(size) {
                    this.fontSize += size
                }
            }
        })
    </script>
</body>
```



### 兄弟组件——>兄弟组件

通过事件中心实现兄弟组件的数据交互

#### 事件中心

![](https://img-blog.csdnimg.cn/20200920234839544.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0tvbmdLb25nX1JhYw==,size_16,color_FFFFFF,t_70)



1. 先单独声明一个Vue实例对象，作为事件中心

```js
var hub = new Vue()
```

2. 用事件中心对自定义事件进行监听和销毁

```js
hub.$on('自定义事件名称'，事件函数($emit传递的数据))；
hub.$off('自定义事件名称'，事件函数($emit传递的数据))；
```

3. 触发指定组件的自定义事件，传递数据

```js
hub.$emit('事件中心监听事件名称'，数据)
```

---

如下：

1. 声明一个全局事件中心

2. 分别在两个兄弟组件的template模版中绑定自定义事件

3. 分别在两个兄弟组件中的mounted属性中，通过事件中心监听自身的自定义事件

   并修改数据，修改的是另一个兄弟组件的数据

4. 分别在两个兄弟组件中的methods属性中，

   通过事件中心$emit() 触发被事件中心监听的另一个兄弟组件的自定义事件

5. 该例子把销毁事件定义在Vue实例中，也就是两个子组件的父组件中

   销毁两个组件的自定义事件

```html
<body>
    <div id="app">
        <Tom></Tom>
        <Jerry></Jerry>

        <hr>
        <button @click='handle'>销毁事件</button>
    </div>

    <script>
        //事件中心
        var hub = new Vue();

        Vue.component('Tom', {
            data: function() {
                return {
                    num: 0
                }
            },
            template: `
            <div>
                <div>Tom {{num}}</div>
                <button @click='addJerryNum'>number of Jerry +10</button>
            </div>
            `,
            mounted: function() {
                //监听事件
                hub.$on('Tom-event', (val) => {
                    this.num += val
                })
            },
            methods: {
                addJerryNum() {
                    //触发事件中心监听的兄弟组件的事件，并传递数据
                    hub.$emit('Jerry-event', 10)
                }
            }
        });

        Vue.component('Jerry', {
            data: function() {
                return {
                    num: 0
                }
            },
            template: `
            <div>
                <div>Jerry {{num}}</div>
                <button @click='addTomNum'>number of Jerry +20</button>
            </div>
            `,
            mounted: function() {
                hub.$on('Jerry-event', (val) => {
                    this.num += val
                })
            },
            methods: {
                addTomNum() {
                    hub.$emit('Tom-event', 20)
                }
            }
        });

        new Vue({
            el: "#app",
            methods: {
                handle() {
                    hub.$off('Tom-event');
                    hub.$off('Jerry-event');
                }
            }
        })
    </script>
</body>
```





## 组件插槽

### 插槽的基本用法

#### <slot\></slot\>标签

1. **用`<slot\></slot\>`标签定义插槽**

   子组件的template中给模版添加`<slot\></slot\>`标签

   可理解为预留等待填充标签的空位

```js
Vue.component('子组件名',{
  template:`
			<div>
					<slot></slot>
			</div>
	`
})
```

2. **父组件中给自组件的标签内添加内容**

   该内容会反映到自组件模版中的`<slot\></slot\>`标签内

```html
<父组件>
  <子组件>添加内容</子组件>
</父组件>
```

如下：

```html
<body>
    <div id="app">
        <child>i am content</child>
    </div>

    <script>
        Vue.component('child', {
            template: `
            <div>
                <h3>Hello</h3>
                <h3>i am child</h3>   
                <slot></slot> 
                <slot></slot> 
            </div>`
        })

        new Vue({
            el: "#app"
        })
    </script>
</body>
```

- **<slot\></slot\>中可写入默认内容**

没在父组件若有给子组件的标签内添加内容，则`<slot\></slot\>`使用默认内容

若父组件给子组件的标签内添加了内容，则`<slot\></slot\>`使用添加的内容

```html
<body>
    <div id="app">
        <child>i am content</child>

        <hr>
        <child></child>
    </div>

    <script>
        Vue.component('child', {
            template: `
            <div>
                <h3>Hello</h3>
                <h3>i am child</h3>
                <slot>i m default</slot> 
            </div>`
        })

        new Vue({
            el: "#app"
        })
    </script>
</body>
```



### 具名插槽

就是具有名字的插槽，

1. 在子组件模版中可以定义多个插槽，

   用`name`属性给`<slot></slot>`用加名称

```js
Vue.component('子组件名',{
  template:`
			<div>
					<slot name="插槽名"></slot>
					<slot name="插槽名"></slot>
			</div>
	`
})
```

2. 父组件中给标签添加slot属性和插槽名

   用name名称匹配子组件模版中的插槽内容

   把这些匹配的标签填充到插槽中

```html
<父组件>
  <子组件>
  	<任意标签 slot=”插槽名“></任意标签>
    <任意标签 slot=”插槽名“></任意标签> 
  </子组件>
</父组件>
```

如下：

```html
<body>
    <div id="app">
        <demo-component>
            <h1 slot="top">顶部标题</h1>

            <p>内容</p>
            <p>内容</p>

            <h1 slot="bottom">底部结尾</h1>
        </demo-component>
    </div>

    <script>
        Vue.component('demo-component', {
            template: `
            <div class="container">

                <header>
                    <slot name="top"></slot>
                </header>
                   
                <main>
                    <slot></slot>    
                </main>

                <footer>
                    <slot name="bottom"></slot>
                </footer>
            </div>`
        })

        new Vue({
            el: "#app"
        })
    </script>
</body>
```

最终页面DOM结构如下：

```html
<div id="app">
  <div class="container">
    <header>
      <h1>顶部标题</h1>
    </header>
    <main>
      <p>内容</p>
      <p>内容</p>
    </main>
    <footer>
      <h1>底部结尾</h1>
    </footer>
  </div>
</div>
```

但是有个不足

把插槽名添加到标签中的话是一个标签只能对一个插槽，

如果想在插槽中填充多个标签

需要在父组件中通过`<template\></template\>` 

---

#### <template\></template\>标签

用于包裹多条内容

`<template\></template\>` 标签不会在页面中显示

给其添加slot属性和插槽名，去匹配子组件模版中的插槽内容

```html
<父组件>
  <子组件>
    <template slot="插槽名">
    	<任意标签></任意标签> 
      <任意标签></任意标签> 
      <任意标签></任意标签> 
    </template>  
  </子组件>
</父组件>
```

如下：

```html
<body>
    <div id="app">
        <demo-component>
            <template slot="top">
                <h1>顶部标题</h1>
                <h3>副标题</h3>
                <h3>副标题</h3>
            </template>

            <p>内容</p>
            <p>内容</p>

            <template slot="bottom">
                <h1>底部结尾</h1>
                <h3>副标题</h3>
                <h3>副标题</h3>
            </template>

        </demo-component>
    </div>

    <script>
        Vue.component('demo-component', {
            template: `
            <div class="container">

                <header>
                    <slot name="top"></slot>
                </header>
                   
                <main>
                    <slot></slot>    
                </main>

                <footer>
                    <slot name="bottom"></slot>
                </footer>
            </div>`
        })

        new Vue({
            el: "#app"
        })
    </script>
</body>
```

最终页面DOM结构如下：

```html
<div id="app">
  <div class="container">
    <header>
      <h1>顶部标题</h1>
      <h3>副标题</h3>
      <h3>副标题</h3>
    </header>
    <main>
      <p>内容</p>
      <p>内容</p>
    </main>
    <footer>
      <h1>底部结尾</h1>
      <h3>副标题</h3>      
      <h3>副标题</h3>
    </footer>
  </div>
</div>
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









