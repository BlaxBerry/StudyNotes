# Vue-Router 路由

通过地址用来控制组件的切换

用来实现 **SPA**（**S**ingle **P**age **A**pplication）单页面应用



## 安装 与 导入

在vue项目根目录下打开终端，输入：

```bash
npm install vue-router
```

然后，在vue项目根目录下的**`main.js`**中

1. 导入

2. use

3. 创建路由

4. 挂载路由到Vue实例

```js
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

// 导入 router
import VueRouter from "vue-router"
// use
Vue.use(VueRouter)
    // 创建路由
let router = new VueRouter()


new Vue({
    render: h => h(App),
    // 路由挂载到Vue实例
    router
}).$mount('#app')
```




## 配置规则

通过 **`routes`属性**，配置 **地址** 和 路由管理的**组件**的关系（跳转）

然后，在vue项目根目录下的**`main.js`**中

1. 导入要通过路由管理的组件

2. routes属性中配置关系

3. path设置地址

4. component设置组件
5. children设置子路由

```js
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

// 导入 router
import VueRouter from "vue-router"
// use
Vue.use(VueRouter);
// 创建路由
let router = new VueRouter({
    routes: [
        // 配置地址 和组件关系
        {
            path: '/discovery',
            component: discoveryMusic
        }
    ]
})

// 导入要通过路由管理的组件
import discoveryMusic from "@/components/DiscoveryMusic"

new Vue({
    render: h => h(App),
    // 路由挂载到Vue实例
    router
}).$mount('#app')
```



## 设置出口

在希望被路由管理的组件出现的位置设置一个**`<router-view>`标签**

等浏览器地址栏跳转为指定地址时，对应的组件显示

```vue
<template>
    <div class="home">

         <!-- 左侧  导航 -->
        <div class="left-nav">
            <ul>
                <li>发现音乐</li>
                <li>推荐歌单</li>
                <li>最新音乐</li>
                <li>最新MV</li>
            </ul>
        </div>


        <!-- 右侧  路由容器 -->
        <div class="right-main">
            
            <!-- <DiscoveryMusic></DiscoveryMusic> -->

            <!-- 路由出口 -->
            <router-view></router-view>
        </div>


    </div>
</template>

<script>
// 发现音乐 组件
// import DiscoveryMusic from 

export default {
    data(){
        return {}
    },
    components:{
       // DiscoveryMusic
    }
}
</script>

<style>

</style>
```





## 声明式导航

通过 **router-link标签** ，指定 **to 属性** 为跳转到指定路由地址，

实现路由切换，从而实现SPA显示的组件的切换

**`<router-link>`** 标签会被页面解析为一个`<a\>`标签

可通过class类名 router-link-active控制样式

```vue
<template>
    <div class="home">

         <!-- 左侧  导航 -->
        <div class="left-nav">
            <ul>
                <li>
                    <router-link to="/discovery">发现音乐</router-link>
                </li>
                <li>
                    <router-link to="recommendation">推荐歌单</router-link>
                </li>
                <li>
                    <router-link to="new">最新音乐</router-link>
                </li>
                <li>
                    <router-link to="mv">最新MV</router-link>
                </li>
            </ul>
        </div>


        <!-- 右侧  路由容器 -->
        <div class="right-main">
            
            <!-- <DiscoveryMusic></DiscoveryMusic> -->

            <!-- 路由出口 -->
            <router-view></router-view>
        </div>


    </div>
</template>

<style>
/* 点击高亮 */
a.router-link-active {
    color: red;
    background-color: #ccc;
}
</st
</style>
```





## 编程式导航

如果跳转路由地址需要逻辑判断时

通过 **this.$router.push(路由地址)** 实现路由地址切换，从而切换显示组件

```js
this.$router.push(路由地址)
```

```vue
<template>
    <div class="top-bar">
        <div>
            <button>1</button>
            <button>2</button>
            <button>3</button>
        </div>

        <input 
            type="text" 
            @keyup.enter="toSearchMusic"
            v-model="inputVal">
    </div>
</template>

<script>
export default {
    data(){
        return {
            inputVal:''
        }
    },
    methods:{
        toSearchMusic(){
            // console.log(this.inputVal);
            if(this.inputVal){
                this.$router.push('/list')
            }else{
                // 跳转到list页面
                alert('请输入内容')
            }
        }
    }
}
</script>

<style>
</style>
```





## 路由传参

即，跳转路由并传输数据

通过在路由地址 **URL** 后拼接上 **? key=value**，

实现在跳转路由到相应组件时，将value传输给改组件

```js
this.$router.push('URL?key1=value1&key2=value2')
this.$router.push('/list?musicName=baby')
```

在需要获取该数据的组件中，

通过 **this.$roue.query** 获取传输的键值对，反问其 **key** 键即可获得传输的参数数据

```js
this.$route.query
this.$route.query.musicName
```





## 路由守卫（路由拦截）

```js
Vue.use(VueRouter);

const routes = [
  {
    path:'/XXX',
    name:'',
    meta:{},
    component:()=>import('../views/XXX.vue')
  },{},{}()
];

const router = new VueRouter({
  mode:"history",
  base:process.env.BASE_URL,
  routes
});

router.beforeEach((to,from,next)=>{
    console.log(to);  // 跳转到哪个路由
    console.log(from); // 从哪个路由跳转过来
    next(); // 放行
})

exports default router
```



`next()`不调用就无法跳转路由



