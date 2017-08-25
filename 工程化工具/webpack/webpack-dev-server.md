### why？

* webpack —watch 可以进行自动打包，但是文件多了就会很慢
* 必须重新刷新页面才能更新资源，不能hot replace

### How？

* 启动express http服务器，作用伺服资源文件（观察是否发生变化），与client使用websocket通讯协议，
* 文件发生变化，webpack-dev-server会进行实时编译，但是实时编译的文件并未输出，存放在内存中

### How To Use？

两种方式

#### cmd line

```
webpack-dev-server --content-base 
```

