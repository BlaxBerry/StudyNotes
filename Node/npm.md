# npm

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/db/Npm-logo.svg/1200px-Npm-logo.svg.png)

模块包的安装管理工具

node.js自带npm



## 全局安装 和 全局移除

### npm install -g XXX

安装到自己的电脑内，可以用在多个项目中

---

全局安装的包的位置在：

`~/.nvm/versions/node/v14.16.0/lib/node_modules`

在指定版本的node的 `lib` 中 的`node_modules` 文件夹中

注意：

在 nvm版本管理方式中，安装的模块不是公用的，

也就是说在切换node版本后需要重新安装全局包    

---

### npm uninstall -g XXX

卸载全局安装的包





## 初始化包

### npm init -y

生成**`package.json文件`**

动态记录该项目所用到所有包的下载卸载信息

```bash
npm init -y
```

```json
{
  "name": "blog",
  "version": "1.0.0",
  "description": "- public 静态资源文件",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```



## 生产依赖 开发依赖

提交项目时，`node_modules文件夹`不会一起提交

别人要用时，

需要先根据**`package.json文件`**中记录的包信息

先下载该项目需要的包，然后执行

所以，在开发项目时安装包时要分开依赖，

并记录到**`package.json文件`**



## 开发者安装生产依赖的包

### npm install  XXX --save

安装并向**`package.json文件`**中添加信息

信息记录在**`dependencies`**属性中

文件安装到了当前项目目录下的`node_modules`文件夹中

如下，在blog项目目录下载`express`和`nodemon`两个包：

```bash
npm install express --save
npm install nodemon --save
```

`package.json文件`中`dependencies`属性记载更新如下：

```json
{
  "name": "blog",
  "version": "1.0.0",
  "description": "- public 静态资源文件",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1",
    "nodemon": "^2.0.7"
  }
}
```



## 开发者安装开发依赖的包

### npm install XXX --save-dev

安装包并向**`package.json文件`**中添加信息

信息记录在**`devDependencies`**属性中

文件安装到了当前项目目录下的`node_modules`文件夹中

如下，在blog项目目录下下载`nodemon`包：

```bash
npm install express --save
npm install nodemon --save-dev
```

`package.json文件`中**`devDependencies`**属性更新如下：

```json
{
  "name": "blog",
  "version": "1.0.0",
  "description": "- public 静态资源文件",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  }
}
```



## 开发者移除包

### npm uninstall XXX

```bash
npm uninstall XXX
```

移除并向**`package.json文件`**中添加信息

并根据该包的依赖是生产依赖还是开发依赖，

更新文件的**`dependencies`**属性或**`devDependencies`**属性



因为提交项目时，node_modules文件夹不会一起提交

需要先根据**`package.json文件`**中记录的包信息

先根据依赖下载该项目需要的包

---

## 使用者安装生产依赖的包

### npm install --production

根据packsge.json中**`dependencies`**中记录的包信息

拿到项目后，只下载该项目在生产环境中的包

如下：在项目目录下执行该指令

```bash
npm install --production
```

会安装项目在生产环境中需要的包到`node_modules`



## 使用者安装所有依赖的包

### npm install 

根据packsge.json中`dependencies`和**`devDependencies`**

拿到项目后，下载该项目中用到的全部依赖的包

包括**生产环境**和**开发环境**

如下：拿到项目后，在项目目录下执行

```bash
npm install
```

会安装该项目全部需要的包到`node_modules`





## 执行包

### node .

有了package.json文件后，

```json
{
  "name": "blog",
  "version": "1.0.0",
  "description": "- public 静态资源文件",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

在当前项目目录下，直接

```bash
node .
```

就可以执行主文件了，如上，执行的是app.js



### node run 

或者修改package.json文件中的*test*为指定的js文件

然后执行npm run test命令

```js
{
  "name": "blog",
  "version": "1.0.0",
  "description": "- public 静态资源文件",
  "main": "app.js",
  "scripts": {
    "test": "node app.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

```bash
npm run test
```





## 安装指定版本

### @version

```bash
npm install XXX@version --save
npm install XXX@lversion --save-dev
```

### @lastest  最新版本

```bash
npm install XXX@lastest --save
npm install XXX@lastest --save-dev
```



## 包更新

npm有时updat属性不起作用，可直接下载最新版本

```bash
npm install XXX@lastest --save
npm install XXX@lastest --save-dev
```



## 下载的包列表

查看当前目录下安装的包的列表

```bash
npm list
```

