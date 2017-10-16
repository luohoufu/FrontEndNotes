* 301永久重定向

- 302临时重定向

```
当服务器将302响应发给浏览器时，浏览器并不是直接进行ajax回调处理，而是先执行302重定向——从Response Headers中读取Location信息，然后向Location中的Url发出请求，在收到这个请求的响应后才会进行ajax回调处理。大致流程如下：
ajax -> browser -> server -> 302 -> browser(redirect) -> server -> browser -> ajax callback
而在我们的测试程序中，由于302返回的重定向URL在服务器上没有相应的处理程序，所以在ajax回调函数中得到的是404状态码；如果存在对应的URL，得到的状态码就是200。
所以，如果你想在ajax请求中根据302响应通过location.href进行重定向是不可行的。
解决方法如下：
1、修改ajax代码：
$.ajax({
    url: '/oauth/respond',
    type: 'post',
    data: data,
    dataType: 'json',
    success: function (data) {
        if (data.status == 302) {
            location.href = data.location;
        }
    }
});
2、改用form：
<form method="post" action="/oauth/respond">
</form>
```

- 500服务器错误，无法完成请求
- 503由于超载，服务器暂时无法处理客户端请求
- 505不支持请求的http协议的版本，无法处理