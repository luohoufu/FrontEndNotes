## 单线程 事件轮询

setTimeOut()  事件轮询存在问题，会被js代码阻塞，一个事件触发，会执行回调函数，回调函数的执行阻塞事件轮询

![](https://user-gold-cdn.xitu.io/2016/12/28/ac398dfd7922059ca259635f8305474a)

三层：

* js编写的标准库，使用过程中可以直接调用API
* node binding 是js与C++交流的关键
* node进行的关键c++实现


* * V8：Google 推出的 Javascript VM，也是 Node.js 为什么使用的是 Javascript 的关键，它为 Javascript 提供了在非浏览器端运行的环境，它的高效是 Node.js 之所以高效的原因之一
  * Libuv：它为 Node.js 提供了跨平台，线程池，事件池，异步 I/O 等能力，是 Node.js 如此强大的关键。
  * C-ares：提供了异步处理 DNS 相关的能力。
  * http_parser、OpenSSL、zlib 等：提供包括 http 解析、SSL、数据压缩等其他的能力。

## 与操作系统交互

```
var fs = require('fs');
fs.open('./test.txt', "w", function(err, fd) {
    //..do something
});
```

![](https://user-gold-cdn.xitu.io/2016/12/28/223a0e054edf35e3c2eea775d9ac09b7)

当我们调用fs.open方法时，会通过process.binding()调用c++层面的文件最终由c++完成于操作系统的交互

**nodes是一个平台不是一门语言**



## 异步、非阻塞I/O

* 发起I/O调用

  1.用户通过js调用node核心模块，传入参数封装为一个**请求对象**

  2.将这个对象推到I／O线程池中

  3.js完成一个I/o请求，继续执行后续代码

* 异步I/O执行结束

​        1.I/O操作完成后将结果放到请求对象的result中，发出操作完成的通知

​	2.事件循环时会检查是否有完成的I／O操作，若有则将其加入到I／O观察者对列中，当作事件处理

​	3.执行事件是，会取出回调函数，将results当作参数传入，完成js回调



**事实上nodejs是单线程是js执行环境是单线程的，最终的实际操作，还是通过 Libuv 以及它的事件循环来执行的。这也就是为什么 Javascript 一个单线程的语言，能在 Node.js 里面实现异步操作的原因，两者并不冲突**





## node中的js

### global对象

浏览器中就是window对象，node中是

* global：和window相同，任何global的对象上的属性都可以被全局访问到
* process：所有全局执行上下文的内容都在process对象中，













