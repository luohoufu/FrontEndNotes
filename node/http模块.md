```
require('http').createServer(function(req, res) {
  res.writeHead('200',{'Content-Type':'text/html'});
  res.end('<b>hello</b>')
}).listen(3000)
```

writeHead只有设置了content-type才会正确显示若设置为text／plain浏览器不能正确解析。

而且这样设置之后，connection 和transfer-Encoding会相应的添加到上面，

connection指的是tcp的链接

transfer-encoding：chunked 	编码采用chunked why？看下面

### 写入图片文件

可以多次调用write方法来发送数据，在首次调用write是，node就会爸所有响应头信息以及第一块儿数据发送出去，因此发送数据块的方式在设计文件系统的情况下非常高效.

```
require('http').createServer(function(req, res) {
  res.writeHead('200',{'Content-Type':'image/png'});
  var stream = require('fs').createReadStream('image.png')
  stream.on('data',function() {
    res.write(data);
  });
  上面两行代码可以改为下面的
  require('fs').createReadStream('image.png').pipe(res)流的对接
}).listen()
```

* 高效的内存分配
* 实际上是把文件系统的流接（piping）到另一个流（http.ServerResponse）上。

### 关于编码

```
req.headers['content-type']
application/x-www-form-urlencoded
```

这些url片段称为查询字符串

node提供了querystring模块，可以方便地对这类字符串进行解析