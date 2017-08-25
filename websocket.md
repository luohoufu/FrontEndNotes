## websocket是什么样的协议

websocket是一个持久化的协议，相对于http这种非持久的协议来说，

* http的生命周期通过request来界定，一次request一次response，请求结束
* 加入keep-alive 可以有多个request

## ajax轮询

每隔一段时间就问问

需要服务器有很快的处理速度和资源

## long poll

客户端发起连接之后，没消息就等到有消息再返回

需要很高的并发，也就是同时接待客户的能力

## websocket

```
首先，被动性，当服务器完成协议升级后（HTTP->Websocket），服务端就可以主动推送信息给客户端啦。所以上面的情景可以做如下修改。
客户端：啦啦啦，我要建立Websocket协议，需要的服务：chat，Websocket协议版本：17（HTTP Request）
服务端：ok，确认，已升级为Websocket协议（HTTP Protocols Switched）
客户端：麻烦你有信息的时候推送给我噢。。
服务端：ok，有的时候会告诉你的。
服务端：balabalabalabala
```



