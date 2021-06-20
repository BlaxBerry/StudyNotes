# HTML

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR6XQOWw97nEWHWjOM-7aCCrifH19d5hGnUUg&usqp=CAU)

- HTML文件后缀为htm（旧版）或 html（新版）

- 有浏览器直接由上到下解析执行，无需编译

- 通常由 **开始标签** 和 **结束标签** 两部分构成

- 开始标签和结束标签中间的部分叫 **内容体**

```html
<开始标签>内容体</结束标签>
```

- 单标签自关闭，没有内容体

```html
<单标签/>
```

- 标签书写不分大小写，但是为了阅读方便，建议小写

- 标签可以有属性，属性值包含在引号内，单引号或双引号

```html
<开始标签 属性=“属性值” 属性=“属性值”></结束标签>
```

- HTML标签建议 **包裹嵌套**

```html
<html>
  <head></head>
  <body></body>
</html>
```





### <hr/\>

横线标签

```html
<p></p>
<hr>
<p></p>
```



### <br/\>

换行标签

HTML源码中的换行在解析时会被自动忽略

必须使用换行标签实现页面内容的换行显示

```html
<p>hello <br>Wolrde</p>
```



### <!----\>

注释

```html
<!--我是注释，不会显示-->
```



### \&nbsp;

一个空格

HTML源码中的多个空格最终只会被解析为一个空格

可通过多个`\&nbsp;`是页面展示多个空格

```html
<p>hello &bnsp;&nbsp;</p>
```



### <a href=""\><a/\>

超链接标签

**`href 属性` 设定跳转路径**，可以跳转到：

- 内网的本机路径：相对路径 和 绝对路径

```html
<a href="./01.html">XXX</a>
```

- 互联网路径： http://XXXXXXX

```html
<a href="http://127.0.0.1:3000">XXX</a>
```

- 跳转到本页面

```html
<a href="#"></a>
```

- 跳转到图片

```html
<a href="01.jpg">XXX</a>
```

---

超链接中放图片

```html
<a href="">
  <img src="">
</a>
```





### <table\></table\>

### <tr\></tr\>

### <td\></td\>

```html
<table>
  <tr>
    <td>A</td>
    <td>B</td>
  </tr>
  <tr>
    <td>a1</td>
    <td>b1</td>
  </tr>
  <tr>
    <td>a2</td>
    <td>b2</td>
  </tr>
</table>
```

|   A    |   B    |
| :----: | :----: |
| **a1** | **b1** |
| **a2** | **b2** |



### <th\></th\>

表头单元格

**就是将 <td\> 的文本居中加粗**



### colspan=“?”

跨列合并单元格

```html
<table>
  <tr>
    <td colspan="2">A</td>
  </tr>
  <tr>
    <td>a1</td>
     <td>b1</td>
  </tr>
</table>
```



### rowspan="?"

跨行合并单元格

```html
<table>
  <tr>
    <td rowspan="2">A</td>
    <td>B</td>
  </tr>
  <tr>
    <td>b1</td>
  </tr>
</table>
```

