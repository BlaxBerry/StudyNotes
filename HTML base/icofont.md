# iconfont 字体图标

## Unicode

比如icomoon

1. 下载到项目

2. **fonts文件**夹放入项目的css目录下

3. css文件内写入style.css文件内容，如下：

   ```css
   @font-face {
     font-family: 'icomoon';
     src:  url('fonts/icomoon.eot?z9byv6');
     src:  url('fonts/icomoon.eot?z9byv6#iefix') format('embedded-opentype'),
       url('fonts/icomoon.ttf?z9byv6') format('truetype'),
       url('fonts/icomoon.woff?z9byv6') format('woff'),
       url('fonts/icomoon.svg?z9byv6#icomoon') format('svg');
     font-weight: normal;
     font-style: normal;
     font-display: block;
   }
   ```

4. 需要使用unicode的标签内作为文本内容，

   写入unicode代码

   ```html
   <span>&#xe900;</span>
   ```

5. css文件内修改该标签的字体 **font-family**

   ``` css
   span {
     font-family: "icommon"
   }
   ```

---

## Font class

unicode 的缺点是语义不明、

作为标签的文本内容写入html结构、

维护修改时要在css文件中查找，麻烦

font class 是直接通过设定类名，

把字体图标作为该标签的伪元素::before

推荐阿里

1. 下载到项目css文件夹内

2. html文件内引入下载的**iconfont.css**文件

```html
<link rel="stylesheet" type="text/css" href="iconfont.css">
```

3. 给要使用iconfont的标签添加类名

```html
<span class="iconfont icon-xxx""></span>
```

---

原理：

把字体图标作标签的伪元素::before

```css
.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-yinlemusic214:before {
  content: "\e730";
}

.icon-music5yinle:before {
  content: "\e69b";
}

.icon-swticonyinle2:before {
  content: "\e614";
}
```



## Syboml

还没学



