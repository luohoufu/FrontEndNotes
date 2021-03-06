[参考](https://sunshinevvv.com/2017/02/09/HttpGETv.s.POST/)

## 关于RFC

> RFC（request for comment）征求意见稿，由互联网工程组发布的一系列备忘录，收集互联网信息的一些文件，也就是说RFC是互联网的一系列规范

语法和语义

> 句子符合语法，但不一定符合语义，eg 我上大学 大学上我

例如：get语义是获取资源，post是处理资源

符合语法的情况下是可以违背语义的，例如：比如使用GET方法修改用户信息，POST获取资源列表，这样就只能说这个请求是「合法」的，但不是「符合语义」的

RFC中定义了http方法的几个特性：

* 安全性 
  * 不引起服务端的任何变化：get head trace options ，但是并不意味着实际安全，比如get也可以修改用户信息，
  * 这个概念的目的是为了方便爬虫和缓存，以免调用或者缓存某些不安全的方法是引起其他后果，**（user agent）浏览器应该在不安全请求时提醒用户**
* 幂等性 
  * 调用次数增多，但是结果相同，put delete和安全方法都是幂等的
  * 引入目的：请求响应前失去连接，可以放心地再请求一次；**因此浏览器后退刷新页面，遇到post请求会给用户提示**
* 可缓存性
  * get和head是可缓存的

## get和post区别

|        | get                               | post                                     |
| ------ | --------------------------------- | ---------------------------------------- |
| 安全性    | 发送信息在URL中                         | 参数不会保存在浏览器历史活着web服务器日志中                  |
| 历史     | 在                                 | 参数不在浏览器历史中                               |
| 可见性    | 数据在url中                           |                                          |
| 回退刷新影响 | 无影响                               | 重新发送请求                                   |
| 可缓存性   | 可缓存                               | 不可缓存                                     |
| 数据类型限制 | 只允许ASCII字符                        | 没有限制，也允许二进制数据                            |
| 数据大小限制 | url由于浏览器和操作系统的限制，1024个字符          | 无限制                                      |
| 编码类型   | application/x-www-form-urlencoded | application/x-www-form-urlencoded 或 multipart/form-data。为二进制数据使用多重编码。 |
| 书签     | 可收藏                               | 不可收藏                                     |