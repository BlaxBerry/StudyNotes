```html
 <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```





```html
<input type="button" value="get" class="get">
<input type="button" value="post" class="post">

<script>
document.querySelector('.get').onclick = function() {

       axios.get('./01.js?').then(function(response) {
                console.log(response);
            }, function(err) {
                console.log(err);
            });
}

document.querySelector('.post').onclick = function() {

            axios.post('./02.jss', {
                name: 'andy'
            }).then(function(res) {
                console.log(res);
            }, function(err) {
                console.log(err);
            })
}
</script>
```