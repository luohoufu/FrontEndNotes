多数网站不需要复杂的计算，主要时间花费在网路I/O和磁盘I/O上

一个数据包来回的延迟320ms。这个普通的CPU可以执行几千万个周期

因此异步IO要发挥作用，例如多线程，java去读一个文件，只是一个阻塞操作，因此去开一个线程去处理文件读取，读取完之后再来通知主线程

但对于node不可能



## 关于js的单线程

js单线程，同一时间只能做一件事情，js的主要用途是与用户互动，以及操作DOM，如果使用多线程会导致同步问题，虽然h5提出了web worker，允许js创建多个线程，但是子线程完全受主线程控制，且不能修改DOM，因此并没有改变多线程的本质。

## js的执行栈

调用函数时，会把函数参数局部变量压入到一个栈中，函数执行完毕后本地变量从栈中弹出，只有在基本类型时才会发生，对象数组的值是存在于heap中，stack只存放了引用。

当函数之行结束从 stack 中弹出来时，只有对象的指针被弹出，而真正的值依然存在 heap 中，然后由垃圾回收器自动的清理回收

## 任务队列

执行时，可以将不管IO，将任务挂起，执行后面的任务，等返回了结果，在执行。

因此存在两种任务：同步任务和异步任务

> 所有同步任务在主线程执行，形成执行栈execution context stac
>
> 主线程之外有一个任务队列，异步任务有了结果，就在任务队列中放置一个事件，表示异步的操作可以进入执行栈了
>
> 一旦执行栈中的任务执行完毕就会读取任务队列，
>

## event Loop事件循环

![](http://image.beekka.com/blog/2014/bg2014100802.png)

js运行时产生堆和栈，栈中的代码调用各种外部的API，在对列中添加各种事件，只要**主线程的执行栈**为空，就会执行任务队列中的时间的回调函数。

```
process.nextTick(function A() {
  console.log(1);
  process.nextTick(function B(){console.log(2);});
});

setTimeout(function timeout() {
  console.log('TIMEOUT FIRED');
}, 0)
// 1
// 2
// TIMEOUT FIRED
```

## microtask 和macrotask

```
setImmediate(function(){
    console.log(1);
},0);
setTimeout(function(){
    console.log(2);
},0);
new Promise(function(resolve){
    console.log(3);
    resolve();
    console.log(4);
}).then(function(){
    console.log(5);
});
console.log(6);
process.nextTick(function(){
    console.log(7);
});
console.log(8);
//输出 3 4 6 8 7 5 2 1
```

优先级关系：process.nextTick > promise.then > setTimeout > setImmediate

```
macrotasks: script(整体代码),setTimeout, setInterval, setImmediate, I/O, UI rendering
microtasks: process.nextTick, Promises, Object.observe, MutationObserver
```

执行过程

* js引擎从macrotask queue中取出第一个macrotask
* 执行完毕后，将micotaskqueue中的任务取出，全部执行
* 从macrotask取出下一个task执行
* 依次循环