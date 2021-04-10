# Vue-cli

脚手架

![](https://reffect.co.jp/wp-content/uploads/2019/06/vue_tue_cli.png)

[Vue-cli中文文档](https://cli.vuejs.org/zh/)

## 安装脚手架

```bash
npm install -g @vue-cli
```

```bash
vue --version
@vue/cli 4.5.12
```



创建脚手架项目

```bash
vue create XXX
```



配置

```bash
Vue CLI v4.5.12
? Please pick a preset: (Use arrow keys)
❯ Default ([Vue 2] babel, eslint) 
  Default (Vue 3 Preview) ([Vue 3] babel, eslint) 
  Manually select features 
```



按着流程配置完后，

```bash
cd XXX
npm run serve
```

会提示创建项目成功和提示端口号

如下：提示端口号是 localhost：8080

```bash
 DONE  Compiled successfully in 2739ms                                3:56:10 AM


  App running at:
  - Local:   http://localhost:8080/ 
  - Network: http://192.168.1.2:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.

```

访问后会显示界面:

![](https://junpeko.com/wp-content/uploads/2018/06/vue_eye.png)



## 目录结构

```js
XXX
|--node_modules
|--public
|		|--index.html
|--src
|		|--assets
|		|--components
|--App.vue
|--main.js
|--package.json
|--package-lock.json
```



**src目录**：写代码的地方

**components目录**：公用组件

**main.js文件**：整个项目的入口文件

```js
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App), //把App组件替换渲染到id=“app”的节点上
}).$mount('#app')
```



**App.vue**：**一个单文件组件**

里面包含了`模版 <template\>`，`JS <script>`，`样式 <style\>`

```vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <hello-world msg="Welcome to Your Vue.js App"></hello-world>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

App.vue

```vue
<template>
	<div id="app">
    <自定义组件名 :自定义属性=“数据”></自定义组件名>
  </div>
</template>

<script>
  //引入
  import 自定义组件名 from "./components/组件文件.vue"
  
  //注册
  export default {
    name:"",
    data(){
      数据:值
    },
    components:{
      引入的自定义组件名
    }
  }
</script>
```

组件文件.vue

```vue
<template>
	<div id="app">
    <自定义组件名></自定义组件名>
  </div>
</template>

<script>
  
  export default {
    name:自定义组件名,
    props:[自定义属性名]
  }
</script>
```





## 更改项目配置 vue.config.js

在vue-cli 脚手架项目目录下（和 `package.json文件`同级）

创建一个**`vue.config.js` 文件**

在里面写 webpack、项目等相关的配置

更改后会覆盖原来的配置

使用CommonJS的写法



### port 启动端口

比如，更改vue脚手架项目的启动端口：

```js
module.exports = {
  devServer:{
    port: XXXXX
  }
}
```

更改完后，重新启动

```bash
cd 项目目录
npm run serve
```



### publicPath 公共路径

默认情况下，vue脚手架会默认应用部署在一个域名的根目录上，

比如：`http://www.my-app.com/`

所以如果应用是备部署在一个字路径上时，会导致路径出错

需要手动指定该子路径

比如：`http://www.my-app.com/子路径名/`

所以需要在 **`vue.config.js` 文件** 中修改部署应用包时的基本URL

```js
module.exports= {
  publicPath:'/子目录名/'
}
```



但是会导致开发时访问项目，必须要在路径添加上子目录

会写起来麻烦点

可通过**环境变量**判断

node环境变量 process.env,NODE_ENV用来区分开发环境和生存环境

npm run build 的话环境变量是 production

npm run serve 的话环境变量是development

```js
process.env.NODE_ENV === 'production'?'/子目录名/':'';
```

```js
module.exports= {
  publicPath: process.env.NODE_ENV === 'production'?'/子目录名/':'';
}
```

比如用 `npm run build` 打包后的html文件内的导入的CSS、JS文件的路径

需要手动修改文件路径

修改最终的生成的各个文件的依赖地址

```js
module.exports= {
  publicPath:'./'
}
```

































































![](https://reffect.co.jp/wp-content/uploads/2020/04/vue_webpack-1024x585.png)