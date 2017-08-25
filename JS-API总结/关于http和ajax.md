[TOC]



## 概述

XMLHttpRequest()让发送一个http请求变得很容易，你只需要简单的

* 创建一个 请求对象实例
* 打开一个URL
* 发送这个请求
* 传输完毕，结果的http状态及返回的内容也可以从请求对象中获取

## 请求类型

* 异步 true
* 同步 false（基本被弃用）

## 处理响应

* responseXML
* responseText

## 处理二进制数据

XMLHttpRequest一般用来 **发送和接收文本数据**、其实也可以发送和接收二进制内容，解决方法：

* overrideMimeType方法


* ```
  oReq.overrideMimeType("text/plain; charset=x-user-defined");
  ```

* level2 新规范  responseType  使得接收和发送二进制数据变得容易

## 监听进度

* req.upload.progress
* ​

```
req.upload.addEventListener("progress", updateProgress);
req.upload.addEventListener("load", transferComplete);
req.upload.addEventListener("error", transferFailed);
req.upload.addEventListener("abort", transferCanceled);

function updateProgress(evt) {
  if (evt.lengthComputable) {
    var percentComplete = evt.loaded / evt.total;
    ...
  } else {
    // Unable to compute progress information since the total size is unknown
  }
}
```

##XMLHttpRequest.setRequestHeader() 设置http请求头部的方法

###在open和send之间调用
#XMLHttpRequest
* onabort
* onerror
* onload
* onloadend
* onloadstart

* onprogress
* onreadystatechange
* ontimeout
* readyState

* status
* statusText

* response
* responseText 返回的内容
* responseType
* responseURL
* responseXML

* timeout

* upload  （XMLHttpRequestUpload） 观察上传进度需查看此属性
   *  onabort
   *  onerror
   *  onload
   *  onloadend
   *  onloadstart
   *  onprogress
   *  ontimeout

* withCredentials


#http
##请求头
###请求方式
* accept 客户端能接收的资源类型
* accept-language客户端接受的语言类型
* connection维护客户端和服务端的连接关系
* host 连接的目标主机和端口号
* referer 告诉服务器自己来自哪里
* user-agent客户端版本号名字
* accept-encoding     gzip/defalte   客户端能接收到压缩数据的类型
* if-modified-since 缓存时间
* cookie
* Date

##响应头
* http/1.1响应采用的协议和版本号，200（状态码）OK（描述信息）
* location 服务端需要客户端访问的页面路径
* server 服务器端名
* content-encoding gzip（服务器端能够发送的压缩编码类型）
* content-length    服务器端发送的压缩数据的长度
* content-language  服务器端发送的语言类型
* content-type   text/html charset服务器端发送的类型以及采用的编码方式
* last-modified 最后修改资源的时间
* refresh  服务端要求客户端1s后，刷新访问指定路径
* set-cookie
* expires
* pragma   no-cache
* cache-control
* connection
* control-allow-origin 跨域问题
* content-type 用于指示MIME类型
  * 响应头中，告诉客户端返回的内容的类型是什么
  * 请求头中，告诉服务端实际发送的数据类型

>content-type: text/html;charset = utf-8;boundary=something

##MIME类型
通知客户端其接受文件的多样性的机制：文件后缀名在网页上并没有明确的意义。因此是服务器设置正确的传输类型很重要，所以正确的MIME类型与每个文件一同传送给服务器
###通用结构
>type/subtype
>text/plain
>text/html
>image/jpej
>image/png
>audio/mpeg
>audio/ogg
>application/octet-stream 表示某种二进制数据 
###MIME文件的重要性
* application/octet-stream 未知的应用程序文件，不会自动执行
* text/plain 
* text/css  
* text/html
* mulipart/form-data  

  可用于表单从浏览器发送到服务器。多部分文档格式，由边界线划出不同部分，每部分有自己的实体，自己的http请求


			POST / HTTP/1.1
			Host: localhost:8000
			User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
			Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
			Accept-Language: en-US,en;q=0.5
			Accept-Encoding: gzip, deflate
			Connection: keep-alive
			Upgrade-Insecure-Requests: 1
			Content-Type: multipart/form-data; boundary=---------------------------8721656041911415653955004498
			Content-Length: 465
			
			-----------------------------8721656041911415653955004498
			Content-Disposition: form-data; name="myTextField"
			
			Test
			-----------------------------8721656041911415653955004498
			Content-Disposition: form-data; name="myCheckBox"
			
			on
			-----------------------------8721656041911415653955004498
			Content-Disposition: form-data; name="myFile"; filename="test.txt"
			Content-Type: text/plain
			
			Simple file.
			-----------------------------8721656041911415653955004498--

许多web服务器使用默认的application/octet-stream来发送未知类型。但出于安全原因。对于一些资源不允许浏览器设置一些自定义的默认操作，导致用户必须存储到本地使用。

## http状态码

* 301永久重定向
* 302临时重定向

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

* 500服务器错误，无法完成请求
* 503由于超载，服务器暂时无法处理客户端请求
* 505不支持请求的http协议的版本，无法处理


## readState

readyStateChange事件

## 值的含义 

* UNAENT 0 未调用open
* OPENED 1 已调用open
* HEADERS_RECEIVED2 接收到头信息
* LOADING 3 接收到响应主体
* DONE 4 响应完成