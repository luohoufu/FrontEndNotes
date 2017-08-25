![](https://camo.githubusercontent.com/707ab384df1cffd60ba1c19f188fd6edbce34d3d/687474703a2f2f62657277696e2e6769746875622e696f2f707074732f6b6f612f696d672f6b6f612d666c6f772e6a7067)

## 一、启动server之前

存在三个方法文件，

* request  包含一些操作原生node请求对象的一些方法（获取query数据，url数据等）
* response 包含操作响应对象的一些方法（设置状态码、主体数据、header等）
* context 上下文，koa中的操作都基于此，

```
this.body = 'hello world';
```

### 1.context

request和response将自身的方法委托到context中,

context中有两部分，一部分是自身属性，一部分是request和response委托到context中。

```
var delegate = require('delegates');
var proto = module.exports = {}; // 一些自身方法

/**
 * Response delegation.
 */

delegate(proto, 'response')
  .method('attachment')
  .method('redirect')
  .method('remove')
  .method('vary')
  .method('set')
  .method('append')
  .access('status')
  .access('message')
  .access('body')
  .access('length')
  .access('type')
  .access('lastModified')
  .access('etag')
  .getter('headerSent')
  .getter('writable');
```

**属性代理，结合getter和setter方法以及delegate，达到ctx.status=500代表ctx.response.status=500的效果，简化代码**。

```
//delegator
Delegator.prototype.method = function(name){
  var proto = this.proto;
  var target = this.target;
  this.methods.push(name);

  proto[name] = function(){
  //每次调用ctx中的方法，会调用对应response或者request中的方法
    return this[target][name].apply(this[target], arguments);
  };
  return this;
};

Delegator.prototype.getter = function(name){
  var proto = this.proto;
  var target = this.target;
  this.getters.push(name);

//在proto上绑定个getter函数，当函数被触发的时候，会去对应的request或response中去读取对应的属性，这样request或response的getter同样会被触发
  proto.__defineGetter__(name, function(){
    return this[target][name];
  });

  return this;
};

//request.js
module.exports = {
  //...
  get method() {
    return this.req.method;//后面每次请求生成ctx时，会将req设置为ctx.request的属性
  },

  set method(val) {
    this.req.method = val;
  }
  //...
}
```

## 二、启动server

```
//第一步初始化app对象
var koa = require('koa');
var app = koa();

//第二步监听端口
app.lister(1995)
```

#### 1.初始化时并没有createServer

```
module.exports = class Application extends Emitter {
  /**
   * Initialize a new `Application`.
   *
   * @api public
   */

  constructor() {
    super();

    this.proxy = false;
    this.middleware = [];   初始化空的中间件集合
    this.subdomainOffset = 2;
    this.env = process.env.NODE_ENV || 'development';
    this.context = Object.create(context);    //创建context实例
    this.request = Object.create(request);    //创建request实例
    this.response = Object.create(response);  //创建response实例
  }
  }
```

#### 2.listen时创建server

```
app.listen = function(){
  debug('listen');
  var server = http.createServer(this.callback());
  return server.listen.apply(server, arguments);
};
```

createserver(**callback**),传入的参数是一个函数，每次服务器收到请求都要执行此函数

#### 3.callback（有两个部分，下面是第一部分，初始化中间件）

```
callback() {
	//第一部分 初始化中间件
    const fn = compose(this.middleware);

    if (!this.listeners('error').length) this.on('error', this.onerror);
	
	//第二部分 处理请求
    const handleRequest = (req, res) => {
      res.statusCode = 404;
      const ctx = this.createContext(req, res);
      const onerror = err => ctx.onerror(err);
      const handleResponse = () => respond(ctx);
      onFinished(res, onerror);
      return fn(ctx).then(handleResponse).catch(onerror);
    };

    return handleRequest;
  }

```

```
const fn = compose(this.middleware);
```

**compose源码:**

fn()返回给定值解析后（resolve或者reject）的promise对象

**把下一个中间件的despatch包装起来作为next函数，传入上一个中间件的函数中作为参数，这样就可以在上一个中间件中调用**

**执行第i个中间件，传入context和下一个中间件的next函数（执行下一个中间件的dispatch）作为参数，await next（）时，会一直递归下去，直到最终的中间件resolve，或者出错，再一层层向上，返回promise的状态结果，从而最终得到promise的结果**

**递归使得中间件自上而下嵌套调用，使得中间件自上而下被执行，**

```
module.exports = compose

/**
 * Compose `middleware` returning
 * a fully valid middleware comprised
 * of all those which are passed.
 *
 * @param {Array} middleware
 * @return {Function}
 * @api public
 */

function compose (middleware) {
  /**
   * @param {Object} context
   * @return {Promise}
   * @api public
   */

  return function (context, next) {
    // last called middleware 
    let index = -1
    return dispatch(0)
    //返回一个
    function dispatch (i) {
      if (i <= index) return Promise.reject(new Error('next() called multiple times'))
      index = i
      let fn = middleware[i]  //第i个中间件
      if (i === middleware.length) fn = next
      if (!fn) return Promise.resolve()
      //返回的是带有状态的promise
      try {
      //执行第i个中间件，传入context和下一个中间件的next包装函数，await next（）时，一直递归下去，知道最终的中间件resolve，或者出错，再一层层向上，返回promise的状态结果，从而最终得到结果
        return Promise.resolve(fn( context, function next() {return dispatch(i + 1)} ))
      } catch (err) {
        return Promise.reject(err)
      }
    }
  }
}
```

```
// 中间件的用法
const Koa = require('koa');
const app = new Koa();

// x-response-time
app.use(async function (ctx, next) {
  const start = new Date();
  await next();
  const ms = new Date() - start;
  ctx.set('X-Response-Time', `${ms}ms`);
});

// logger
app.use(async function (ctx, next) {
  const start = new Date();
  await next();
  const ms = new Date() - start;
  console.log(`${ctx.method} ${ctx.url} - ${ms}`);
});

// response
app.use(ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

返回的是一个第一个promises的对象（成功或者失败）

## 三、接受请求

### 1.callback第二部分处理请求

```
	//第二部分 处理请求
    const handleRequest = (req, res) => {
      res.statusCode = 404;
      //创建可用的ctx
      const ctx = this.createContext(req, res);
      
      const onerror = err => ctx.onerror(err);
      const handleResponse = () => respond(ctx);
      onFinished(res, onerror);
      //
      return fn(ctx).then(handleResponse).catch(onerror);
    };
```

```
const ctx = this.createContext(req, res);
```

### 2.创建可用的context

为context有5个属性 ：

* request request继承于Request，包括一些操作req的方法
* response 同上
* req node原生request对象
* res 同上
* app  koa的原型对象

koa中this就是这里return的context

```

  /**
   * Initialize a new context.
   *
   * @api private
   */

  createContext(req, res) {
    const context = Object.create(this.context);
    const request = context.request = Object.create(this.request);
    const response = context.response = Object.create(this.response);
    
    context.app = request.app = response.app = this;
    context.req = request.req = response.req = req;
    context.res = request.res = response.res = res;
    request.ctx = response.ctx = context;
    request.response = response;
    response.request = request;
    context.originalUrl = request.originalUrl = req.url;
    context.cookies = new Cookies(req, res, {
      keys: this.keys,
      secure: request.secure
    });
    request.ip = request.ips[0] || req.socket.remoteAddress || '';
    context.accept = request.accept = accepts(req);
    context.state = {};
    return context;
  }
```

### 3.错误处理

监听response，监听response，设置状态码和错误信息

```
const onerror = err => ctx.onerror(err);
```

### 4.中间件的执行流程（包括回逆过程）

```
const onerror = err => ctx.onerror(err);
const handleResponse = () => respond(ctx);
onFinished(res, onerror);
return fn(ctx).then(handleResponse).catch(onerror);
```

### 成功之后的hangdleRespond    （respond源码）

```
/**
 * Response helper.
 */

function respond(ctx) {
  // allow bypassing koa
  if (false === ctx.respond) return;

  const res = ctx.res;
  if (!ctx.writable) return;

  let body = ctx.body;
  const code = ctx.status;

  // ignore body
  if (statuses.empty[code]) {
    // strip headers
    ctx.body = null;
    return res.end();
  }

  if ('HEAD' == ctx.method) {
    if (!res.headersSent && isJSON(body)) {
      ctx.length = Buffer.byteLength(JSON.stringify(body));
    }
    return res.end();
  }

  // status body
  if (null == body) {
    body = ctx.message || String(code);
    if (!res.headersSent) {
      ctx.type = 'text';
      ctx.length = Buffer.byteLength(body);
    }
    return res.end(body);
  }

  // responses
  if (Buffer.isBuffer(body)) return res.end(body);
  if ('string' == typeof body) return res.end(body);
  if (body instanceof Stream) return body.pipe(res);

  // body: json
  body = JSON.stringify(body);
  if (!res.headersSent) {
    ctx.length = Buffer.byteLength(body);
  }
  res.end(body);
}
```



[参考1](https://github.com/berwin/Blog/issues/8)

[参考2](https://zhuanlan.zhihu.com/p/24601833)

