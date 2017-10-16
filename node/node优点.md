I/O密集和cpu密集，

web，静态资源的读取，数据库操作，I/O操作，渲染页面，读取模版文件获取数据。使用node会更有优势。

高并发：单位时间内访问量很大

* 加机器
* 使用多核机器

上述方法与使用语言无关。

两个程序，cpu高速切换，多进程。

## 传统的apache处理的请求

apache处理web请求，每来一个请求，就开启一个进程，等待I/O响应，多个进程处理多个请求。不然就增加机器，增加多核。

* cpu分配的进程有限。不然排队
* cpu处理速度很快，cpu浪费（服务员玩手机）

## nodejs解决这个问题

所有请求到达，吞吐量会比较大，但其实并没有解决性能问题，CPU使用到最大。

nodejs单进程处理请求，具体的操作交给底层操作系统，

多核，利用cluter！



exports指向module.exports,

如果修改exports赋值为一个对象，则会改变其指向，使得其并没有作用。



## 全局对象global

global.testVar = 1;



## process

* argv
* argv0
* execargv
* env