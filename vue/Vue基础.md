

# Vue基础

![vue](https://iwatani.tv/wp-content/uploads/2019/12/vuejs-keyvisual-1024x538.png)

[Vue文档](https://cn.vuejs.org/v2/guide/)

## 简介



## 安装下载

### 独立安装JS文件

下载官方 JS文件，html文件中引入该 JS文件即可

[开发版本 vue.js](https://cn.vuejs.org/js/vue.js)

[生成版本 vue.min.js](https://cn.vuejs.org/js/vue.min.js)

```html
<!--引入开发版本-->
<script src="js/vue.js"></script>
<!--引入生产版本-->
<script src="js/vuemin..js"></script>
```

---

### CDM引用

学习时推荐

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

---

### NPM安装

```bash
npm install vue
```

---

### vue-cli 命令行工具

```bash
npm install --global vue-cli
```





## vue项目

### 目录结构





## 基本使用

每个Vue应用都需要通过 **实例化Vue对象**

```html
<script>
  new Vue({
    /////////////////////////
  })
</script>
```

在实例化的Vue构造器中包含：

**el参数**：确定挂载的元素的ID

**data属性**：定义属性

**methods属性**：定义方法

```html
<div id="app">
  <p>{{message]}}</p>
</div>

<script>
  new Vue({
    el: "#app",
    data:{
      message: "hello",
      number: 100
    },
    methods: {
      fun(){
        return XXXX
      }
    }
  })
</script>
```





### 自定义属性 实例属性

data属性 和 methods属性里面写的内容

若想直接获得data属性和methods属性等，需要加前缀 **$**

```js
实例化对象.$data
实例化对象.$methods
```

```html
<div id="app"></div>

<script>
  var data = {
   	 name: "Andy",
     age: 28
  };

  var app = new Vue({
     el: "#app",
     data: {
       	people: data
     },
    methods: {
      fun(){
        return 1000
      }
    }
  })
  
  console.log(app.people);   //obj
  console.log(app.people.name);  //Andy
  console.log(app.people.age);   //28
  
  console.log(app.fun);   // function
  console.log(app.fun());   //1000
  
  console.log(app.data); //undefined
  console.log(app.methods);  //undefined
  
  
  console.log(app.$data); //obj
  console.log(app.$methods); //obj
  
  console.log(app.$data.people === data); // true
</script>
```



## 内置指令 模版语法

详细看文档

---

### 插值表达式 {{ }}

能在里面写入JS表达式

作用和 `v-text` 一样，不能解析HTML标签

```html
<div id="app">
  <p>{{message + "_" + message}}</p>
  <div>{{num + 100}}</div>
</div>

<script>
  new Vue({
    el: "#app",
    data:{
      message: "hello",
      number: 100
    },
    methods: {
      fun(){
        return XXXX
      }
    }
  })
</script>
```





## 事件绑定

### methods属性

---

### 事件修饰符

- `.stop` - 阻止冒泡
- `.prevent` - 阻止默认事件
- `.capture` - 阻止捕获
- `.self` - 只监听触发该元素的事件
- `.once` - 只触发一次
- `.left` - 左键事件
- `.right` - 右键事件
- `.middle` - 中间滚轮事件





## 条件

控制显示和隐藏

### v-if

根据后面值或表达式的结果为true/false，删除和显示元素

如果是false，就从DOM树结构中删除该元素

```html
<div v-if="true"></div>
<div v-if="false"></div>
<div v-if="message"></div>
```

```html
<div v-if="num==index"></div>
<div v-if="num==100"></div>
```

### v-else

必须要和`v-if`连用

```html
<div id="app">
    <div v-if="Math.random() > 0.5">
      Sorry
    </div>
    <div v-else>
      Not sorry
    </div>
</div>
    
<script>
new Vue({
  el: '#app'
})
</script>
```

### v-else-if

必须要和`v-if` 和 `v-else`连用

```html
<div id="app">
    <div v-if="type === 'A'">
      A
    </div>
    <div v-else-if="type === 'B'">
      B
    </div>
    <div v-else-if="type === 'C'">
      C
    </div>
    <div v-else>
      Not A/B/C
    </div>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    type: 'C'
  }
})
</script><div v-if="true"></div>
```

---

### v-show

根据后面值或表达式的结果为true/false，隐藏和显示元素

和 v-if 不同的是，元素仅仅隐藏并不会删除

若是false时的作用等于 **display：none；**

```html
<div v-show="true"></div>
<div v-show="false"></div>
<div v-show="message"></div>
```

```html
<div v-show="num==index"></div>
<div v-show="num==100"></div>
```





## 样式绑定

用  `v-bind` 来设置样式属性

### class 类名

**`v-bind:class`**设定类

```html
<div v-bind:src="str"></div>
<div v-bind:class="className"></div>


<div :src="str"></div>
<div :class="className"></div>
```

对象形式

```html
<div class="className"></div>

<div :class="{className:true}"></div>
<div :class="{className:false}"></div>
<div :class="{className:isclass}"></div>

<div :class="{className1:true, className2:true}"></div>
```

数组形式

```html
<div :class="[isclass?'className':'']"></div>

<div :class="[isclass?'className':'', isclass?'className':'']"></div>
```

```html

```

```html

```

---

### style 内联样式

 **`v-bind:style`**直接设置样式

```html
<div id="app">
    <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">hello</div>
</div>
```

- **绑定对象**

```html
<div id="app">
  <div v-bind:style="styleObj">hello</div>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    styleObj: {
      color: 'green',
      fontSize: '30px'
    }
  }
})
</script>
```

- **绑定数组**

```html
<div id="app">
  <div v-bind:style="[obj1, obj2]">hello</div>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    obj1: {
      color: 'green',
      fontSize: '30px'
    },
	  obj2: {
      'font-weight': 'bold'
    }
  }
})
</script>
```







## 循环

使用 `v-for` 绑定数据到数组来渲染（生成）一个列表

### 迭代整数

```html
<div id="app">
  <ul>
    <li v-for="n in 10">{{ n }}</li>
  </ul>
</div>
```

---

### 迭代数组

```html
<div id="app">
  <ul>
    <li v-for="(item, index) in array">
     {{ index }}. {{ item }}
    </li>
    <li v-for="(item, index) in array">
     {{ index + 1}}. {{ item }}
    </li>
  </ul>
</div>
```

---

### 迭代对象

```html
<div id="app">
  <ul>
    <li v-for="(value, key, index) in object">
     {{ index }}. {{ key }} : {{ value }}
    </li>
  </ul>
</div>
```

---

### :key







## 表单操作

- input 单行文本

- textarea 多行文本

- select option 下拉多选

- radio 单选

- checkbox 多选

---

### input

直接通过 `v-model `和  `data`中的数据进行数据的双向绑定

如下：

input的value和data中的uname的值双向绑定

```html
<div id="app">
  <input type="text" id="Name" v-model="uname">
</div>

<script>
new Vue({
  el: "#app",
  data: {
    uname: "andy"
  }
})
</script>
```

---

### radio

**需要给每个单选框一个不同的value值**

直接**通过 `v-model `绑定 `data` 中的`gender`** ，

再判断单选框的 `value`值是否等于 `data` 中的`gender` 的值，

如下：

第一个单选框的value值是1，第二个value值是2，

Vue中的gender值为1，所以第一个单选框被选中：

```html
<div id="app">
  <input type="radio" name="sex" v-model="gender" value=1>
  <input type="radio" name="sex" v-model="gender" value=2>
</div>

<script>
new Vue({
  el: "#app",
  data: {
    gender: 1
  }
})
</script>
```

---

### checkbox

**需要给每个单选框一个不同的value值**

**直接通过 `v-model `绑定 `data` 中的`hobby`** ，

再判断单选框的 `value`**值是否包含在** `data` 中`gender` **数组中**，

如下：

data中的hobby的值是个数组，里面是1，2

所以复选框中value值是1和2的被选中

```html
<div id="app">
  <input type="checkbox" name="Hobby" value="1" v-model="hobby">
  <input type="checkbox" name="Hobby" value="2" v-model="hobby">
  <input type="checkbox" name="Hobby" value="3" v-model="hobby">
</div>

<script>
new Vue({
  el: "#app",
  data: {
    hobby: [”1“,”2“]
  }
})
</script>
```

---

### select 

给每一个选项 `option` 一个`value`值,

给`select`绑定`v-model`

- **select单选**

单选的话就是判断`data`中的变量值是否等于`option`的`value`

如下：

data中的skill的值是1，所以拉下选项中value值是1的被选中

```html
<select name="Skill" v-model="skill">
  <option value="1">Please Choose</option>
  <option value="2">JavaScript</option>
  <option value="3">Vue</option>
  <option value="4">React</option>
</select>

<script>
new Vue({
  el: "#app",
  data: {
    skill: 1
  }
})
</script>
```

- **select多选**

> 给select标签添加multip=“true”
>
> 选择是按住shifit就可多选

多选的话就是判断**`option`的 `value`是否包含在数组中**

如下：

skill的数组中是1、2、3，所以value值是1、2、3的这三个被选中

```html
<div id="app">
  <select name="Skill" v-model="skill" multipe="true">
  <option value="1">Please Choose</option>
  <option value="2">JavaScript</option>
  <option value="3">Vue</option>
  <option value="4">React</option>
</select>
</div>

<script>
new Vue({
  el: "#app",
  data: {
    skill: ["1","2","3"]
  }
})
</script>
```

---

### textarea

文本框textarea的数据绑定和单行文本的input相同

直接通过 `v-model `和  `data中的数据进行数据的双向绑定

```html
<div id="app">
  <textarea name="Content" v-model="content">
</div>
  
<script>
new Vue({
  el: "#app",
  data: {
    content: "hello"
  }
})
</script>
```

---

### 表单修饰符

- `.lazy`  将input事件转为change事件

> **input事件**是**每次值改变**都会触发，
>
> **change事件**是当**失去焦点**时发现值被改变了才出发
>
> 验证邮箱时会用

- `.number`  将输入值转为 Number 类型
- `.trim`  去除输入值的首尾空格

```html
<input type="text" v-model.lazy="value">

<input type="text" v-model.number="value">
<input type="number" v-model.number="value">

<input type="text" v-model.trim="value">
```





## 自定义指令

### 全局指令

内置指令不满足使用，可以自定义指令

用`Vue.directive()`创建自定义指令，里面写入**钩子函数**

[Vue文档 自定义指令 钩子函数](https://cn.vuejs.org/v2/guide/custom-directive.html#%E9%92%A9%E5%AD%90%E5%87%BD%E6%95%B0)

```html
<div id="app">
  <div v-自定义指令名></div>
</div>

<script>
  Vue.directive("自定义指令名称",{
   inserted:function(目标元素){
     //对元素的操作
   }
  });
  
  new Vue({
    el: "#app"
  })
</script>
```

如下：

自定义了一个v-focus指令

指令的逻辑是绑定该指令的表单元素获得焦点。

```html
<!--获得焦点-->
<div id="app">
   <input type="text"><br>
   <input type="text" v-focus>
</div>

<script>
	Vue.directive("focus", {
    	inserted: function(el) {
       	 el.focus();
    }
	});

	new Vue({
    	el: "#app"
	})
</script>
```

### 自定义指令带参数

参数就是指令后面的部分，比如`v-model=“参数”`

### 钩子函数的binding参数

需要再data中声明一个对象

binding参数是个对象，value值就是data中声明对象的值

如下：

改变背景色的自定义指令

```html
<!--改变背景色-->
<div id="app">
	<div v-color='msg'></div>
</div>

<script>
Vue.directive("color", {
    inserted: function(el, binding) {
        el.style.background = binding.value.color;
      	el.style.color = binding.value.color;
    }
})

new Vue({
    el: "#app",
    data: {
        msg: {
            color: "white",
          	backgroundColor: "red"
        }
    }
})
</script>
```

---

### 局部指令

就是把创建自定义指令的过程写入Vue实例对象中

局部指令仅能使用在当前组件中

```js
new Vue({
    el: "#app",

  	directives: {
      自定义指令名: {
        inserted/bind:function(el,binding){
      			////逻辑
    		}
      },
      自定义指令名: {
        inserted/bind:function(el,binding){
      			////逻辑
    		}
      }
    }
})
```

比如：

```html
<div id="app">
   <input type="text"><br>
   <input type="text" v-focus>
  	
   <div v-color="msg"></div>
</div>

<script>
new Vue({
    el: "#app",
    data: {
        msg: {
            color: "white",
            backgroundColor: "red"
        }
    },
    directives: {
        focus: {
            inserted: function(el) {
                el.focus()
            }
        },
        color: {
            inserted: function(el, binding) {
                el.style.backgroundColor = binding.value.backgroundColor;
                el.style.color = binding.value.color
            }
        }
    }
})
</script>
```





## 计算属性 computed 

**计算属性 处理一些复杂逻辑，将模版变得更加简洁**

比如，直接在插值表达式中进行处理会很麻烦

可以把逻辑运算放入一个函数，函数存入计算属性中

使用时直接调用该函数的函数名

```html
<div id="app">
  {{ message.split('').reverse().join('') }}
</div>
<!--麻烦-->
```

---

所以可以通过计算属性

方法里必须有一个return返回值

使用时直接调用computed中的方法名，不需要加括号

```html
<div id=“app”>
  <div>{{方法名}}</div>
</div>

<script>
new Vue({
  el:"#app",
  computed: {
    方法名(){
      ///逻辑
      return 值
    }
  }
})
</script>
```

如下，反转字符串：

```html
<div id="app">
  <p>原始: {{ message }}</p>
  <p>运算后: {{ reversedMessage }}</p>
</div>
 
<script>
var vm = new Vue({
  el: '#app',
  data: {
    message: 'hello Vue!'
  },
  computed: {
    reversedMessage: function () {
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
```



### computed 和 methods 的区别

效果是类似的，但是，

- **computed 基于依赖的数据进行缓存**

- **methods 方法不存在缓存**

比如同样的运算执行多次，用methods每次都调用都会运算会耗时，

而computed计算属性运算完给出一个返回值，这个运算过程会缓存不会再重复执行

所以复杂重复的运算时使用 computed 性能会更好

```html
<div id="app">
  <p>{{ message }}</p>
  <p>{{ reversedMessage }}</p>
  <p>{{ reversedMessage }}</p>
  
  <p>{{ reversedMessage2() }}</p>
  <p>{{ reversedMessage2() }}</p>
</div>
<script>
var vm = new Vue({
  el: '#app',
  data: {
    message: 'Runoob!'
  },
  computed: {
    reversedMessage: function () {
      return this.message.split('').reverse().join('')；
      console.log('i am computed')； //执行过后会缓存，同样重复调用只执行一次
    }
  },
  methods: {
    reversedMessage2: function () {
      return this.message.split('').reverse().join('');
      console.log('i am methods');   //调用一次就执行一次
    }
  }
})
</script>
```

---

### getter 和 setter

```js
var vm = new Vue({
  el: '#app',
  data: {
    name: 'Google',
    url: 'http://www.google.com'
  },
  computed: {
    site: {
      // getter
      get: function () {
        return this.name + ' ' + this.url
      },
      // setter
      set: function (newValue) {
        var names = newValue.split(' ')
        this.name = names[0]
        this.url = names[names.length - 1]
      }
    }
  }
})
// 调用 setter， vm.name 和 vm.url 也会被对应更新
```





## 侦听器 watch

监听属性watch，是用来**侦听数据的变化**

数据一旦变化就会触发watch中的对应该数据的方法，执行异步或耗时的复杂操作

**watch属性中的方法名 必须和 要侦听的数据名 一致**

方法中的参数是被侦听的该数据

```html
<script>
new Vue({
  el:"#app",
  data:{
    数据
  },
  watch:{
    数据:function(val){
      处理数据
    }
  }
})
</script>
```

比如：

分别侦听两个数据 firstName 和 lastName，

当数据变化时立刻改变另一个数据 fullName

```html
<div id="app">
    firstName: <input type="text" v-model="firstName"> <br> 
   	lastName: <input type="text" v-model="lastName"> <br>
    <p>{{fullName}}</p>
</div>

<script>
        new Vue({
            el: "#app",
            data: {
                firstName: 'Andy',
                lastName: 'Red',
                fullName: 'Andy Red'
            },
            watch: {
                firstName: function(val) {
                    this.fullName = val + " " + this.lastName
                },
                lastName: function(val) {
                    this.fullName = this.firstName + " " + val
                }
            }
        })
</script>
```

---

### 侦听和异步

**用于验证用户名的可用性**

监听变化，一旦变化就用Ajax访问接口

判断后渲染页面内容

如下：

```html
    <div id="app">
        <span>User Name: </span>
        <span><input type="text" v-model.trinm.lazy="uname"></span>
        <span class="message">{{message}}</span>
    </div>

    <script>
        var list = ['Andy', 'Red', 'James', 'Odd'];

        new Vue({
            el: "#app",
            data: {
                uname: "",
                message: "请输入"
            },
            methods: {
                check(val) {
                    let that = this;

                    //用定时器模拟 Ajax访问接口的耗时
                    setTimeout(function() {
                        if (list.indexOf(val) != -1) {
                            that.message = "已存在"
                        } else {
                            that.message = "可使用"
                        }
                    }, 2000)
                }
            },
            watch: {
                uname: function(val) {
                    this.check(val);
                    this.message = "验证中..."
                }
            }
        })
    </script>
```







## 过滤器 filter

在数据渲染时处理数据，将处理后的结果展现

**用于 格式化数据**

比如， **字符串首字母大写、日前格式化**

---

### 全局过滤器

使用`Vue.filter()` 设定过滤器函数，

**必须用`return`返回一个值**

其参数`val	`就是要过滤的数值

```html
<div id="app">
  <div>{{数据 ｜ 过滤器名称}}</div>
</div>

<script>
	Vue.filter("过滤器名称",function(value){
  //逻辑
});
  new Vue({
    el:"#app",
    data: {
     数据
    }
  })
</script>
```

---

**使用场景**

- **过滤插值表达式**

```html
<div id="app">
  <div>{{massage | 过滤器名}}</div>
</div>
```

- **过滤属性绑定值** 

```html
<div id="app">
  <div :属性名=“数据 ｜ 过滤器”></div>
</div>
```

---

**多个过滤器并用**

在前一个过滤后的值的基础上进行过滤

```html
<div id="app">
  <div>{{massage | 过滤器名 ｜ 过滤器名}}</div>
</div>
```

---

比如：

（表单和数据双向绑定）

将要渲染的数据的首字母大写：

```html
<div id="app">
   <input type="text" v-model="message">
	 <h2>{{message | upper}}</h2>
</div>

<script>
        Vue.filter('upper', function(val) {
            return val.charAt(0).toUpperCase() + val.slice(1)
        })
        new Vue({
            el: "#app",
            data: {
                message: ""
            }
        })
</script>
```

---

### 局部过滤器

仅本组件可用的过滤器

```html
<script>
	new Vue({
    el:"#app",
    
    filters:{
      过滤器名称: function(val){
        //逻辑
      },
      过滤器名称: function(val){
        //逻辑
      }
    }
	})
</script>
```

如下：

```html
<script>
	new Vue({
    el:"#app",
    filters:{
      upper: function(val){
       return val.charAt(0).toUpperCase()+val.splice(1)
      },
      lower: function(val){
        return val.charAt(0).toLowerCase()+val.splice(1)
      }
    }
	})
</script>
```

---

### 过滤器参数

使用`Vue.filter()` 设定过滤器函数，

其第一个参数`val	`就是要过滤的数值，

第二个参数开始是调用过滤器时传入的参数

```js
Vue.filter("过滤器名",function(value,参数,参数..){
   return /////////////value的处理
})
```

```html
<div>{{数据名 | 过滤器名(参数)}}</div>
```

如下：

后面的过滤器要过滤的数据是前一个过滤器过滤后的数据，

如下例子，第一个过滤器的传参是逻辑判断依据，第二个过滤器的传参是data中的数据

第二个要过滤的数据是第一个过滤后的数据

```html
<div id="app">
        <h2>{{date}}</h2>
        <h2>{{date | format('yyyy-MM-dd')}}</h2>
        <hr>
        <h2>{{date | format('yyyy-MM-dd') | fullDate(date)}}</h2>
</div>

<script>
new Vue({
   el: "#app",
   data: {
        date: new Date()
   },
   filters: {
        format: function(val, arg) {
           if (arg == 'yyyy-MM-dd') {
                let res = '';
                res += val.getFullYear() + '-' + (val.getMonth() + 1) + '-' + val.getDate();
                return res;
            }
         },
        fullDate: function(val, arg) {
           return val += '-' + arg.getHours() + '-' + arg.getMinutes() + '-' + arg.getSeconds()
         }
   }
})
 </script>
```





## 生命周期

1. 挂载

初始化相关属性

2. 更新

页面元素或组件的更新改变

3. 销毁

销毁相关属性施放资源

![](https://cn.vuejs.org/images/lifecycle.png)



## 数组响应式变化

## Vue.set  app.$set

> Vue.set(要处理的数组, 要处理的数组索引, 要处理成的数组的值)
>
>  
>
> app.$set(要处理的数组, 要处理的数组索引, 要处理成的数组的值)

```html
    <div id="app">
        <ul>
            <li v-for="item in list">{{item}}</li>
        </ul>
    </div>

    <script>
        let app = new Vue({
            el: "#app",
            data: {
                list: ['Andy', "Tom", "Bob", "James"]
            }
        });

        console.log(app.list[0]);
        app.list[0] = "Red"
    </script>

<!--页面渲染的内容并不会被修改-->
```









## Vue 组件