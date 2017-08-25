[TOC]



## 一、概述

### 1.基本性质

http是无状态的，有会话的，可以使用cookie来创建有状态的会话

一个链接，多个http请求，keep-alive

### 2.http可以控制什么

* 缓存

  文档怎么缓存，缓存什么文件，缓存多久，

* 开放同源限制

​        由于同源限制，http可以通过头部的修改开放限制

* 认证？

  一些界面需要限制访问者，头部信息或者cookies来设定

* 代理和隧道

​        服务器或者客户端需要隐藏自身IP，则需要代理越过网络屏障

* 会话

  cookies，进行配置

### 3.http报文

请求报文

![](https://mdn.mozillademos.org/files/13687/HTTP_Request.png)

* method
* 获取资源的路径，没有protocol
* http协议的版本号
* 对服务端表达信息的headers
* 对于想post这类的，body包含了发送的资源

响应报文

![](https://mdn.mozillademos.org/files/13691/HTTP_Response.png)

* http协议版本号
* 状态码
* 状态信息，非权威，也就是由服务器端自行决定
* http headers
* 可选的，但是比在请求报文中更加常见地包含获取资源的body。

## 二、http cookie

[参考](mdn)

服务器发送到浏览器并保存在浏览器上的一块数据，会在浏览器下一次发起请求时携带并发送到。

可以用来判断两次请求是否来自同一个浏览器，从而确定和保持用户的登录状态

* 会话状态管理（用户登录状态、购物车）
* 个性化设置（用户自定义设置）
* 浏览器行为跟踪（跟踪分析用户行为）

### 1.创建cookie

服务器收到请求后，在响应头中增加一个set-cookie头部，浏览器受到相应后会取出cookie，并存储在浏览器，之后对该服务器的请求都会携带，cookie 的**过期时间、域、路径、有效期、站点**都可以根据需要来指定

#### 会话期cookie

浏览器关闭cookie消失

#### 持久cookie

和关闭浏览器便失效不同，持久Cookie可以指定一个特定的过期时间（`Expires`）或者有效期（Max-Age）。

```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```

#### 安全和httponly类型cookie

只有在使用SLL和HTTPS协议向服务器发起请求时，才能确保Cookie被安全地发送到服务器。`HttpOnly`标志并没有给你提供额外的加密或者安全性上的能力，当整个机器暴露在不安全的环境时，切记*绝不能*通过HTTP Cookie存储、传输机密或者敏感信息。

HTTP-only类型的Cookie不能使用Javascript通过[`Document.cookie`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie)属性来访问，从而能够在一定程度上阻止跨域脚本攻击（[XSS](https://developer.mozilla.org/en-US/docs/Glossary/XSS)）。当你不需要在JavaScript代码中访问你的Cookie时，可以将该Cookie设置成`HttpOnly类型。`特别的，当你的Cookie仅仅是用于定义会话的情况下，最好给它设置一下`HttpOnly标`志

#### cookie的作用域

domain path

#### 同站cookie

#### 访问cookie

### 2.安全

### 3.追踪和隐私

## 三、http访问控制（CORS跨域资源共享）

[参考](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)

同源策略导致不能跨域

不一定是浏览器限制了请求，也可能是正常发起，返回结果被浏览器拦截了

最好的例子是 CSRF 跨站攻击原理，请求是发送到了后端服务器无论是否跨域！注意：有些浏览器不允许从 HTTPS 的域跨域访问 HTTP，比如  Chrome 和 Firefox，这些浏览器在请求还未发出的时候就会拦截请求，这是一个特例。）

### 1.概述

跨域资源共享标准新增了一组http头部字段，允许服务器访问，

对于可能对数据产生影响的方法，浏览器必须先使用options方法发起预检请求（prefligth request），获取服务器是否允许该跨域请求，服务器确认允许之后，在发起请求，服务器还可以告诉客户端，是否需要携带身份认证信息（比如cookie和http认证相关数据）







