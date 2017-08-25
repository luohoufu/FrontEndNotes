### setState的流程

setState通过队列机制实现更新，执行setState时，先将更新的state合并到当前的state中，插入状态队列、不会立即更新this.state。

使用batchedUpdate的好处就是，多次改变，但只进行一次渲染，提高性能。

调用栈

* newstate插入pending队列
* 调用enqueueUpdate
* 是否正处于批量更新阶段
* 是则保存组件到dirtyComponents
* 否则执行batchUpdates（遍历dirtyComponents、调用updateComponent、更新pending state or props）

```
function enqueueUpdate(component) {
  //判断batchingStrategy.isBathcingUpdates为false，则batchUpdate
  if (!batchingStrategy.isBatchingUpdates) {
    batchingStrategy.batchedUpdates(enqueueUpdate, component);
    return;
  }
  //若为true，则加入dirtyComponent
  dirtyComponents.push(component);
}

var batchingStrategy = {
  isBatchingUpdates: false,

  batchedUpdates: function(callback, a, b, c, d, e) {
    batchingStrategy.isBatchingUpdates = true;
    transaction.perform(callback, null, a, b, c, d, e);
  }
};
```

> transaction
>
> 将所有执行的方法用wrapper包装起来.  ?

```
class Example extends Component {
       constructor(){
           super();
           //在组件初始化可以直接操作this.state
           this.state = {
               val: 0
           }
       }
       componentDidMount(){
           this.setState({
              val: this.state.val + 1
           });
           //第一次输出  0
           console.log(this.state.val);
           this.setState({
              val: this.state.val + 1
           });
           //第二次输出  0
           console.log(this.state.val);
           setTimeout(()=>{
              this.setState({val: this.state.val + 1});
               //第三次输出   2
               console.log(this.state.val);
               this.setState({
                  val: this.state.val + 1
               });
               //第四次输出   3
               console.log(this.state.val);
           }, 0);  
       }
       render(){
           return null;
       }
   }
```

![](https://pic3.zhimg.com/961aed5b866146dff1707fb0c43914d2_b.jpg)

前两次调用setState时，已经处于batchedUpdates中，**因为整个将组件渲染到Dom中的过程会调用batchedUpdates，会开启一个transaction**，而这个transaction包括componentDidMount过程，因此在这其中调用setState时，无法进行batchUpdates，而是把其**加入到dirtyComponents**，因此没有及时调用batchedUpdates更新state。而在之后的batchedUpdates中，两个setState会在一个transaction 中执行，在transaction执行之后，**才通过 ReactUpdates.flushBatchedUpdates 方法将所有的临时 state merge 并计算出最新的 props 及 state**。所以在transaction中执行的两次setState其实都是访问的原来的state，并未进行更新。



![](https://pic1.zhimg.com/444c2862d8bb4be66ab2ceb370dcbe9c_r.jpg)

没有处于batchedUpdates中，因此setState立即生效



可以修改上述setState的方式：

```
this.setState((prevState,props)=>({val: prevState.val + 1}))
this.setState((prevState,props)=>({val: prevState.val + 1}))
```

此时的结果会变为0 0 3 4

与传入对象更新state的不同，**传入函数setState时会将更新的函数加入到一个队列中，按照顺序依次进行，为每个函数传入前一个函数的最终状态。**

[【参考1】](https://juejin.im/post/58f4cfbd61ff4b005802c872)

[【参考2】](https://zhuanlan.zhihu.com/p/20328570)