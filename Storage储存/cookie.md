# cookie

![](https://www.recolize.com/wp-content/uploads/browser-cookie-restrictions-2020.png)

当 web 服务器向浏览器发送 web 页面时，在连接关闭后，服务端不会记录用户的信息，即再发送一次时服务器不知道是哪一个用户发的

Cookie 的作用就是用于解决 "如何记录客户端的用户信息"

即 ，标记身份

- 当用户访问 web 页面时，他的名字可以记录在 cookie 中。
- 在用户下一次访问该页面时，可以在 cookie 中读取用户访问记录。



1. 服务器 会把 第一次登陆后服务器返回的**cookie储存在浏览器中**，

2. 用户第二次发送请求时，浏览器会自动把上次储存的cookie数据带上给服务器，比如获取该用户的购物车列表

3. 服务器接到数据后，再根据浏览器带上的cookie来判断当前是哪一个用户给服务器发的请求



一般4kb大小



Cookie 以名/值对形式存储







session存储在服务端中