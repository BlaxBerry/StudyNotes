```js
$(window).height(); //浏览器当前窗口可视区域高度 

$(document).height(); //浏览器当前窗口文档的高度 

$(document.body).height();//浏览器当前窗口文档body的高度 

$(document.body).outerHeight(true);//浏览器当前窗口文档body的总高度 包括border padding margin 

$(window).width(); //浏览器当前窗口可视区域宽度 

$(document).width();//浏览器当前窗口文档对象宽度 

$(document.body).width();//浏览器当前窗口文档body的高度 

$(document.body).outerWidth(true);//浏览器当前窗口文档body的总宽度 包括border padding margin
```



文档被卷上去的内容大小：

$(document).scrollTop()

盒子距离文档顶部距离：

offset().top

```js
var boxTop = $('.content').offset().top;

$(window).scroll(function(){
  if($(document).scrollTop() >= boxTop){
    
    $('.goback').fadeIn()
    
  }else{
    $('.goback').fadeOut()
  }
})
```





直接返回顶部

```js
$('.back_top').on('click',function(){ 
  $(document).scrollTop(0)
})
```

动画返回顶部

利用 animate 的 scrollTop属性

animate是针对元素做动画，不是文档，所以改为$('body,html')

```js
$('.back_top').on('click',function(){ 
  $('body,html').stop().animate({
    scrollTop:0
  })
})
```



