react的主要思想是通过构建可复用的组件来构建用户界面

组件其实是**有限状态机**，组件通过状态来渲染对应的界面，组件处于某种状态时，输出这个状态对应的界面。**对组件的管理就是对组件状态的管理**每个组件都有自己的生命周期，规定了组件的状态和方法需要在那个生命周期进行改变和执行

> 有限状态机：记录有限个状态，以及在这写状态之间的转移和动作等行为的模型。通过状态、事件、转换、行为来描述。状态机将复杂的关系简单化。

react利用这一概念，通过管理状态来实现对组件的管理，生命周期-状态-组件

## 生命周期详解

react通过三种状态管理生命周期：

* mouting
* receive_props
* unmounting

这三种状态决定了组件处于那种生命周期，需要执行哪些方法以及是否可以更新state。

三种状态对应三种方法：分别对应两种处理方法，will方法进入状态之前调用，did进入状态之后调用

* mountComponent
* updateComponent
* unmountingComponent

### 状态一 ：mouting

mountComponent负责管理生命周期中的：getInitialState、componentWillMount、render、componentDidMount。

ReactCompositeComponentBase创建一个虚拟节点，需要利用instantiateReactComponent得到一个实例，之后mountComponent拿到结果作为自定义元素的结果。

componentMount过程：组件装载

* 设置状态为mounting，触发getInitialState获取初始化state，初始化更新队列
* 执行componentWillMount，若其中有setState，不会触发 re-render，而是进行state合并
* 创建component实例
* 更新组件状态由mounting  —>  null
* 递归render，子组件
* 执行componentDidMount

![](https://pic3.zhimg.com/ec65c26c1123f588c2a57e40423cf6fa_b.png)

### 状态二：receive_props

updateComponent负责生命周期中的componentWillReceiveProps、shouldComponentUpdate、componentWillUpdate、render、componentDidUpdate

updateMount过程：

* 将状态设置为receive_props
* 执行componentWillUpdate
* 完成receive_props,变更状态为null，
* shouldComponentUpdate、componentWillUpdate
* 递归render
* componentDidUpdate

### 状态三：unmounting

unmountComponent负责生命周期中的componentWillUnmount

过程：

* 设置状态为unmounting
* 执行componentWillUnmount（在其中setstate没用）
* 设置状态为null，完成卸载操作



### setState更新机制

setState会先进行_pendingState进行合并操作，不会立刻re-render，因此是异步操作，再通过判断状态来re-render，

**若在shouldUpdateComponent或者componentWillUpdate中调用setState，则会重新调用update生命周期，从而造成循环。**

**不建议在 *getDefaultProps、getInitialState、shouldComponentUpdate、componentWillUpdate、render 和 componentWillUnmount* 中调用 setState，特别注意：不能在 *shouldComponentUpdate 和 componentWillUpdate*中调用 setState，会导致循环调用。**



## 生命周期的过程

实例化

```
getDefaultProps   获取prop
getInitialState   初始化
componentWillMount 只执行一次
render             渲染
componentWillMount 
```

存在期

```
componentWillReceiveProps 
组件接受到一个新的props时执行此生命周期，如果想改变state，setState，不会触发re-render，

shouldComponentUpdate

componentWillUpdate 
render
componentDidUpdate

```

## 生命周期的方法

### getDefaultProps（）

* 通过consturctor管理
* 在整个生命周期中最先执行，只在创建组件时执行一次，缓存返回的对象，且在组件实例中共享，而不是复制
* 在实例初始化之前调用，因此不能在此方法中调用this.props
* 保证组件在没有父组件传入时也总是有值的
* es6中可以

### componentWillMount（）

* 在render之前调用，所以在其中setstate是不会重新渲染的
* 推荐使用constructor代替

### render

### componentDidMount（）

* 可以对DOM进行操作，执行之后ref变为dom
* 可以加载服务器数据
* 可以使用setstate触发re-render

### componentWillReceiveProps（nextProps）

* 已经挂载的组件收到新的props触发
* 如果需要在props改变的情况下修改state，可以比较this.props nextProps,在通过this.setState（）不会触发re-render，会进行state合并
* react可能会在props传入时没有改变时也进行重新渲染，可以自己比较一下

### shouldComponentUpdate（nextProps，nextState）

props或者state改变会以最上层组件重新创建virtual DOM，导致了未发生变化的子组件re-render（也就是组件中render方法的触发），因此在render之前可以通过此方法进行判断，若返回false则render不会触发。

* 接收到props或state时触发
* 在componentWillReceiveProps（nextProps）之后触发
* 首次渲染和forceUpdate（）不会触发
* 若返回false，会阻止子组件的re-render（componentWillUpdate render compoentDidUpdate不会触发）

### componentWillUpdate（nextProps，nextState）

* shouldComponentUpdate触发之后




## 四种触发render的方式

* 首次渲染initial Render
* 调用this.setState()
* 父组件发生更新(一般就是props发生改变，即使没有改变也会去rerender)
* 调用this.forceUpdate()

![](http://upload-images.jianshu.io/upload_images/1814354-4bf62e54553a32b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


[【参考1】](https://zhuanlan.zhihu.com/p/20312691)

[【参考2】](http://www.infoq.com/cn/articles/react-art-of-simplity?utm_source=articles_about_dive-into-react&utm_medium=link&utm_campaign=dive-into-react)



