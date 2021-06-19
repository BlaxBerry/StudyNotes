# HTTP基础

## 通信，通信协议含义

通信就是信息的传递和交换

通信三要素：

- 通信的**主体**
- 通信的**内容**
- 通信的**方式**

例子：

服务器通过响应给客户端浏览器A大学的简介

通信主体：服务器，客户端浏览器

通信内容：A大学的简介

通信方式：响应

---

**通信协议**是指，通信双方必须遵守的通信格式规则

即，服务器和客户端之间要实现**网页内容的传输**，

通信双方必须遵守**网页内容的传输协议**





## HTTP协议

网页的内容又称为超文本，

**服务器与客户端之间网页内容的传输协议**，

被称为 **超文本传输协议**，简称**HTTP协议**（**H**yper**T**ext **T**ransfer **P**rotocol）

---

HTTP协议规定了客户端与服务器之间请求与响应的标准：

- **客户端**要以HTTP协议要求的格式将数据提交到服务器
- **服务器**要以HTTP协议要求的格式将数据响应给客户端



## 报文

HTTP协议属于客户端浏览器和服务器之间的通信协议，

HTTP请求和响应过程中，被传递的数据块，叫做报文

包含传递的数据的相关信息

分为请求报文和响应报文

可通过浏览器的开发者工具的network面板查看报文信息



### 请求报文（HTTP请求消息）

客户端发起的请求，称为**HTTP请求**，

客户端发送到服务器的消息，称为**HTTP请求消息**（**HTTP请求报文**）

---

#### 组成

组成从上到下分为4部分：

- **请求行**（request line）

- **请求头**（request header）

- **空行**（blank line）

- **请求体**（request body）：仅POST请求

![](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=816032092,2711661542&fm=26&gp=0.jpg)

---

#### 请求行

由3部分与空格组成：

- **请求方式**（request method）

- **请求URL**（request URL）

- **HTTP协议**

<img src="https://pbs.twimg.com/media/E3MucYuUcAAdFQw?format=jpg&name=medium" style="zoom:50%;" />

---

#### 请求头

是有多行key:value 健值对组成

用来描述**客户端的基本信息**‹

从而将客户端的相关信息告知服务端

常见请求头字段：

- **User-Agent：**

  说明当前时什么类型的服务器

- **Content-Type：**

  描述发送到服务器的数据格式

- **Accept：**

  描述客户端能够接受什么类型的返回内容

- **Accept-Language：**

  描述客户端能接受那种人类语言的文本内容

<img src="https://pbs.twimg.com/media/E3Myye3UUAA0qaV?format=jpg&name=medium" style="zoom:50%;" />

<img src="https://pbs.twimg.com/media/E3MyxeOVIAMvVSX?format=jpg&name=large" style="zoom:50%;" />

详见MDN文档

---

#### 空行

请求头结束后就是空行

用来**分隔请求头和请求体**，

通知服务器请求头至此结束

<img src="https://pbs.twimg.com/media/E3M0GUYVUAA9i-_?format=jpg&name=medium" style="zoom:50%;" />

---

#### 请求体

请求体中存放通过**POST方式**提交到服务器的数据

**只要POST请求才有请求体，GET请求没有请求体**

- GET请求消息的组成：

  请求行，请求头

- POST请求消息组成

  请求行，请求头，空行，请求体





### 响应报文（HTTP响应消息）

响应消息就是**服务器响应回客户端的消息内容**，也叫响应报文

---

#### 组成

从上到下分为4部分：

- **状态行**
- **响应头**（Response Header）
- **空行**
- **响应体**

![](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fupload-images.jianshu.io%2Fupload_images%2F14211115-3d4d1b7f0afcfbf1.jpg&refer=http%3A%2F%2Fupload-images.jianshu.io&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1625576170&t=89e599799b101e52a98ef50321450682)

---

#### 响应状态行

由三部分和空格组成：

- **HTTP协议版本**

- **响应状态码**

- **响应状态码的描述文本**

<img src="https://pbs.twimg.com/media/E3NXuVtVkAA4SpZ?format=jpg&name=medium" style="zoom:50%;" />

---

#### 响应头

用来**描述服务器的基本信息**

是有多行key:value 健值对组成

<img src="https://pbs.twimg.com/media/E3RL3m_UUAAkFSb?format=jpg&name=medium" style="zoom:50%;" />

<img src="https://pbs.twimg.com/media/E3RL19XVgAYa81G?format=png&name=900x900" style="zoom:50%;" />

详见MDN文档

---

#### 空行

响应头结束后就是空行

用来**分隔响应头和响应体**，

通知服务器请求头至此结束

<img src="https://pbs.twimg.com/media/E3RUHajVUAEr-qx?format=jpg&name=medium" style="zoom:50%;" />

---

#### 响应体

存放着服务器响应给客户端的资源内容

<img src="https://pbs.twimg.com/media/E3RUY2vVIAEalYS?format=jpg&name=medium" style="zoom:50%;" />







## HTTP请求方法

属于HTTP请求协议的一部分

用来表明对服务器上的资源进行何种操作

![](https://pbs.twimg.com/media/E3RVVXYVgAkg-cR?format=jpg&name=medium)

浏览器URL地址请求都是GET请求

Form表单有默认行为（action属性的跳转），

会导致默认产生了一次GET请求





## HTTP响应状态码

HTTP响应状态码（HTTP Status Code），也属于HTTP协议的一部分

用来标识资源响应的状态

会随着服务器的响应消息一起发送到客户端浏览器

<img src="https://pbs.twimg.com/media/E3NXuVtVkAA4SpZ?format=jpg&name=medium" style="zoom:50%;" />

---

### 分类

是有三个十进制的数字组成

第一个数字定义了状态的类型，共有5种

剩余的两个队状态进行细分描述

![](https://pbs.twimg.com/media/E3RWyz0UcAAnKJJ?format=jpg&name=large)

---

### 2**

是**响应成功相关的**状态码

表示服务器成功接受到请求，并进行处理

![](https://pbs.twimg.com/media/E3RZRKqVgAEqSrY?format=jpg&name=large)

---

### 3**

是**重定向相关的**状态码

表示服务端要求客户端进行重定向，

需要客户端进一步操作来完成数据的请求

![](https://pbs.twimg.com/media/E3RZRKrVIAAzmez?format=jpg&name=large)

---

### 4**

是**客户端错误相关的**状态码

表示客户端请求中有非法内容，导致请求失败

![](https://pbs.twimg.com/media/E3RZRKqVEAI1jSC?format=jpg&name=large)

---

### 5**

是服务端错误相关的状态码

表示服务器端为能正常出来客户端的请求，导致出现错误

![](https://pbs.twimg.com/media/E3RZRKpVcAY8oOy?format=jpg&name=large)





